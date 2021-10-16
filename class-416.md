# Spring Authentication
## Spring Security Architecture
### Authentication and Access Control
Application security boils down to two more or less independent problems: authentication (who are you?) and authorization or access control (what are you allowed to do?)
### Authentication
- The main strategy interface for authentication is `AuthenticationManager`, which has only one method:

        public interface AuthenticationManager {
            Authentication authenticate(Authentication authentication)
            throws AuthenticationException;
            }
- The most commonly used implementation of `AuthenticationManager` is `ProviderManager`, which delegates to a chain of `AuthenticationProvider` instances. An `AuthenticationProvider` is a bit like an`AuthenticationManager`, but it has an extra method to allow the caller to query whether it supports a given Authentication type:

    public interface AuthenticationProvider {

        Authentication authenticate(Authentication authentication)
                throws AuthenticationException;

        boolean supports(Class<?> authentication);
    }

![authon](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/authentication.png)

### Customizing Authentication Managers
- Spring Security provides some configuration helpers to quickly get common authentication manager features set up in your application. The most commonly used helper is the `AuthenticationManagerBuilder`, which is great for setting up in-memory, JDBC, or LDAP user details or for adding a custom UserDetailsService.

        @Configuration
        public class ApplicationSecurity extends WebSecurityConfigurerAdapter {

        ... // web stuff here

        @Autowired
        public void initialize(AuthenticationManagerBuilder builder, DataSource dataSource) {
            builder.jdbcAuthentication().dataSource(dataSource).withUser("dave")
            .password("secret").roles("USER");
        }

        }
- the usage of `AuthenticationManager`Builder is more widely applicable (see Web Security for more detail on how web application security is implemented). Note that the `AuthenticationManagerBuilder` is `@Autowired` into a method in a `@Bean` — that is what makes it build the global (parent) AuthenticationManager.
- Spring Boot provides a default global `AuthenticationManager` (with only one user) unless you pre-empt it by providing your own bean of type `AuthenticationManager`. The default is secure enough on its own for you not to have to worry about it much, unless you actively need a custom global `AuthenticationManager`. If you do any configuration that builds an `AuthenticationManager`, you can often do it locally to the resources that you are protecting and not worry about the global default.

### Authorization or Access Control
- Once authentication is successful, we can move on to authorization, and the core strategy here is `AccessDecisionManager`. There are three implementations provided by the framework and all three delegate to a chain of `AccessDecisionVoter` instances, a bit like the `ProviderManager` delegates to `AuthenticationProviders`.

-An `AccessDecisionVoter` considers an Authentication (representing a principal) and a secure Object, which has been decorated with ConfigAttributes:

        boolean supports(ConfigAttribute attribute);
        boolean supports(Class<?> clazz);
        int vote(Authentication authentication, S object,
                Collection<ConfigAttribute> attributes);
### Web Security
- Spring Security in the web tier (for UIs and HTTP back ends) is based on Servlet Filters, so it is helpful to first look at the role of Filters generally.
![websec](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/filters.png)

- The client sends a request to the application, and the container decides which filters and which servlet apply to it based on the path of the request URI. At most, one servlet can handle a single request, but filters form a chain, so they are ordered. In fact, a filter can veto the rest of the chain if it wants to handle the request itself. A filter can also modify the request or the response used in the downstream filters and servlet. The order of the filter chain is very important, and Spring Boot manages it through two mechanisms: `@Beans` of type Filter can have an `@Order` or implement Ordered, and they can be part of a `FilterRegistrationBean` that itself has an order as part of its API. 
- Some off-the-shelf filters define their own constants to help signal what order they like to be in relative to each other

- Spring Security is installed as a single Filter in the chain, and its concrete type is `FilterChainProxy`, for reasons that we cover soon. In a Spring Boot application, the security filter is a `@Bean` in the `ApplicationContext`, and it is installed by default so that it is applied to every request. It is installed at a position defined by `SecurityProperties.DEFAULT_FILTER_ORDER`, which in turn is anchored by `FilterRegistrationBean.REQUEST_WRAPPER_FILTER_MAX_ORDER` (the maximum order that a Spring Boot application expects filters to have if they wrap the request, modifying its behavior). There is more to it than that, though: From the point of view of the container, Spring Security is a single filter, but, inside of it, there are additional filters, each playing a special role
![webservices](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/security-filters.png)

- there is even one more layer of indirection in the security filter: It is usually installed in the container as a `DelegatingFilterProxy`, which does not have to be a Spring `@Bean`. The proxy delegates to a `FilterChainProxy`, which is always a `@Bean`, usually with a fixed name of `springSecurityFilterChain`. It is the `FilterChainProxy` that contains all the security logic arranged internally as a chain (or chains) of filters. All the filters have the same API (they all implement the Filter interface from the Servlet specification), and they all have the opportunity to veto the rest of the chain.
![filter](https://github.com/spring-guides/top-spring-security-architecture/raw/main/images/security-filters-dispatch.png)

### Creating and Customizing Filter Chains
- The default fallback filter chain in a Spring Boot application (the one with the /** request matcher) has a predefined order of `SecurityProperties.BASIC_AUTH_ORDER`. You can switch it off completely by setting `security.basic.enabled=false`, or you can use it as a fallback and define other rules with a lower order. To do the latter, add a `@Bean` of type `WebSecurityConfigurerAdapter` (or `WebSecurityConfigurer`) and decorate the class with `@Order`, as follows:

        @Configuration
        @Order(SecurityProperties.BASIC_AUTH_ORDER - 10)
        public class ApplicationConfigurerAdapter extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.antMatcher("/match1/**")
            ...;
        }
        }

### Request Matching for Dispatch and Authorization
- A security filter chain (or, equivalently, a `WebSecurityConfigurerAdapter`) has a request matcher that is used to decide whether to apply it to an HTTP request. Once the decision is made to apply a particular filter chain, no others are applied. However, within a filter chain, you can have more fine-grained control of authorization by setting additional matchers in the HttpSecurity configurer, as follows:

        @Configuration
        @Order(SecurityProperties.BASIC_AUTH_ORDER - 10)
        public class ApplicationConfigurerAdapter extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.antMatcher("/match1/**")
            .authorizeRequests()
                .antMatchers("/match1/user").hasRole("USER")
                .antMatchers("/match1/spam").hasRole("SPAM")
                .anyRequest().isAuthenticated();
        }
        }

### Combining Application Security Rules with Actuator Rules
- If you want your application security rules to apply to the actuator endpoints, you can add a filter chain that is ordered earlier than the actuator one and that has a request matcher that includes all actuator endpoints. If you prefer the default security settings for the actuator endpoints, the easiest thing is to add your own filter later than the actuator one, but earlier than the fallback (for example, `ManagementServerProperties.BASIC_AUTH_ORDER + 1`), as follows:

        @Configuration
        @Order(ManagementServerProperties.BASIC_AUTH_ORDER + 1)
        public class ApplicationConfigurerAdapter extends WebSecurityConfigurerAdapter {
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http.antMatcher("/foo/**")
            ...;
        }
        }
### Method Security
- As well as support for securing web applications, Spring Security offers support for applying access rules to Java method executions. For Spring Security, this is just a different type of “protected resource”. For users, it means the access rules are declared using the same format of `ConfigAttribute` strings (for example, roles or expressions) but in a different place in your code. The first step is to enable method security — for example, in the top level configuration for our application:

        @SpringBootApplication
        @EnableGlobalMethodSecurity(securedEnabled = true)
        public class SampleSecureApplication {
        }
- Then we can decorate the method resources directly:

        @Service
        public class MyService {

        @Secured("ROLE_USER")
        public String secure() {
            return "Hello Security";
        }

        }

- There are other annotations that you can use on methods to enforce security constraints, notably `@PreAuthorize` and `@PostAuthorize`, which let you write expressions containing references to method parameters and return values, respectively

- ###### **Tip** :
    ***It is not uncommon to combine Web security and method security. The filter chain provides the user experience features, such as authentication and redirect to login pages and so on, and the method security provides protection at a more granular level.***

### Working with Threads
Spring Security is fundamentally thread-bound, because it needs to make the current authenticated principal available to a wide variety of downstream consumers. The basic building block is the `SecurityContext`, which may contain an `Authentication` (and when a user is logged in it is an `Authentication` that is explicitly `authenticated`). You can always access and manipulate the `SecurityContext` through static convenience methods in `SecurityContextHolder`, which, in turn, manipulate a `ThreadLocal`. 

        SecurityContext context = SecurityContextHolder.getContext();
        Authentication authentication = context.getAuthentication();
        assert(authentication.isAuthenticated);
- It is not common for user application code to do this, but it can be useful if you, for instance, need to write a custom authentication filter (although, even then, there are base classes in Spring Security that you can use so that you could avoid needing to use the `SecurityContextHolder`).
- If you need access to the currently authenticated user in a web endpoint, you can use a method parameter in a `@RequestMapping`, as follows:

        @RequestMapping("/foo")
        public String foo(@AuthenticationPrincipal User user) {
        ... // do stuff with user
        }

- If Spring Security is in use, the `Principal` from the `HttpServletRequest` is of type `Authentication`, so you can also use that directly:

        @RequestMapping("/foo")
        public String foo(Principal principal) {
        Authentication authentication = (Authentication) principal;
        User = (User) authentication.getPrincipal();
        ... // do stuff with user
        }

### Processing Secure Methods Asynchronously
- Since the `SecurityContext` is thread-bound, if you want to do any background processing that calls secure methods (for example, with `@Async`), you need to ensure that the context is propagated. This boils down to wrapping the `SecurityContext` with the task (`Runnable`, `Callable`, and so on) that is executed in the background. Spring Security provides some helpers to make this easier, such as wrappers for `Runnable` and `Callable`. To propagate the `SecurityContext` to `@Async` methods, you need to supply an `AsyncConfigurer` and ensure the `Executor` is of the correct type:

        @Configuration
        public class ApplicationConfiguration extends AsyncConfigurerSupport {

        @Override
        public Executor getAsyncExecutor() {
            return new DelegatingSecurityContextExecutorService(Executors.newFixedThreadPool(5));
        }

        }

## Spring Auth Cheat Sheet

### Step 1: set up a user model and repo

### Step 2: create a controller for that model

### Step 3: UserDetailsServiceImpl implements UserDetailsService

- gets a User from the database by username (make sure your repository has the method to make this easy!)

### Step 4: ApplicationUser implements UserDetails

- use IntelliJ to implement the methods; make the boolean ones all return true

### Step 5: WebSecurityConfig extends WebSecurityConfigurerAdapter

- has a `UserDetailsService`
- `passwordEncoder` bean
- configure `AuthManagerBuilder`
    - `auth.userDetailsService(userDetailsService).passwordEncoder(passwordEncoder());`
- configure `HttpSecurity`
    - cors? csrf?
    - matchers for URLs that are allowed
        - ensure that login and signup URLs allowed; also consider homepage etc.
    - formLogin with login page set up
    - logout

```
    @Override
    @Bean
    public AuthenticationManager authenticationManagerBean() throws Exception {
        return super.authenticationManagerBean();
    }
```

### Step 6: registration page
- create it w/ form
- ensure it posts to a route your controller is ready for
- check it's saving in the DB
```
    // maybe autologin?
    Authentication authentication = new UsernamePasswordAuthenticationToken(newUser, null, new ArrayList<>());
    SecurityContextHolder.getContext().setAuthentication(authentication);
```

### Step 7: login page
- create it w/ form
- ensure it posts to the route you specified in web config
- try it out!
- add to a template w/ things about the Principal