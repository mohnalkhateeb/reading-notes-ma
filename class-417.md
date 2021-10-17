# Spring Boot and OAuth2
 The samples are all single-page apps using Spring Boot and Spring Security on the back end. They also all use plain jQuery on the front end. But, the changes needed to convert to a different JavaScript framework or to use server-side rendering would be minimal.

 - There are several samples building on each other, adding new features at each step:

    - **simple**: a very basic static app with just a home page and unconditional login via Spring Boot’s OAuth 2.0 configuration properties (if you visit the home page, you will be automatically redirected to GitHub).

    - **click**: adds an explicit link that the user has to click to login.

    - **logout**: adds a logout link as well for authenticated users.

    - **two-providers**: adds a second login provider so the user can choose on the home page which one to use.

    - **custom-error**: adds an error message for unauthenticated users, and a custom authentication based on GitHub’s API.

### Single Sign On With GitHub
* ##### 1- Creating a New Project
        $ mkdir ui && cd ui
        $ curl https://start.spring.io/starter.tgz -d style=web -d name=simple | tar -xzvf -
* ##### 2- Add a Home Page
        <!doctype html>
        <html lang="en">
        <head>
            <meta charset="utf-8"/>
            <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
            <title>Demo</title>
            <meta name="description" content=""/>
            <meta name="viewport" content="width=device-width"/>
            <base href="/"/>
            <link rel="stylesheet" type="text/css" href="/webjars/bootstrap/css/bootstrap.min.css"/>
            <script type="text/javascript" src="/webjars/jquery/jquery.min.js"></script>
            <script type="text/javascript" src="/webjars/bootstrap/js/bootstrap.min.js"></script>
        </head>
        <body>
            <h1>Demo</h1>
            <div class="container"></div>
        </body>
        </html>
* ##### 3- Securing the Application with GitHub and Spring Security
    - ###### 1- Add a New GitHub App
        - Select "New OAuth App" and then the "Register a new OAuth application" page is presented. Enter an app name and description. Then, enter your app’s home page, which should be `http://localhost:8080`, in this case. Finally, indicate the Authorization callback URL as `http://localhost:8080/login/oauth2/code/github` and click Register Application.The OAuth redirect URI is the path in the application that the end-user’s user-agent is redirected back to after they have authenticated with GitHub and have granted access to the application on the Authorize application page.
    - ###### 2- Configure 
        - Then, to make the link to GitHub, add the following to your application.yml.
        - Boot Up the Application : With that change, you can run your app again and visit the home page at `http://localhost:8080`. Now, instead of the home page, you should be redirected to login with GitHub. If you do that, and accept any authorizations you are asked to make, you will be redirected back to the local app, and the home page will be visible
### Add a Welcome Page
* #### Conditional Content on the Home Page `index.html`
        <div class="container unauthenticated">
            With GitHub: <a href="/oauth2/authorization/github">click here</a>
        </div>
        <div class="container authenticated" style="display:none">
            Logged in as: <span id="user"></span>
        </div>
        <script type="text/javascript">
            $.get("/user", function(data) {
                $("#user").html(data.name);
                $(".unauthenticated").hide()
                $(".authenticated").show()
            });
        </script>
* #### The `/user` Endpoint `SocialApplication.java`
        @SpringBootApplication
        @RestController
        public class SocialApplication {

            @GetMapping("/user")
            public Map<String, Object> user(@AuthenticationPrincipal OAuth2User principal) {
                return Collections.singletonMap("name", principal.getAttribute("name"));
            }

            public static void main(String[] args) {
                SpringApplication.run(SocialApplication.class, args);
            }

        }

* #### Making the Home Page Public
        @SpringBootApplication
        @RestController
        public class SocialApplication extends WebSecurityConfigurerAdapter {

            // ...

            @Override
            protected void configure(HttpSecurity http) throws Exception {
                // @formatter:off
                http
                    .authorizeRequests(a -> a
                        .antMatchers("/", "/error", "/webjars/**").permitAll()
                        .anyRequest().authenticated()
                    )
                    .exceptionHandling(e -> e
                        .authenticationEntryPoint(new HttpStatusEntryPoint(HttpStatus.UNAUTHORIZED))
                    )
                    .oauth2Login();
                // @formatter:on
            }

        }

### Add a Logout Button
* ##### Client Side Changes `index.html`
        <div class="container authenticated">
        Logged in as: <span id="user"></span>
        <div>
            <button onClick="logout()" class="btn btn-primary">Logout</button>
        </div>
        </div>
        var logout = function() {
            $.post("/logout", function() {
                $("#user").html('');
                $(".unauthenticated").show();
                $(".authenticated").hide();
            })
            return true;
        }
* #### Adding a Logout Endpoint
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            // @formatter:off
            http
                // ... existing code here
                .logout(l -> l
                    .logoutSuccessUrl("/").permitAll()
                )
                // ... existing code here
                .csrf(c -> c
            .csrfTokenRepository(CookieCsrfTokenRepository.withHttpOnlyFalse())
        )
            // @formatter:on
        }
* #### Adding the CSRF Token in the Client
    - To make the code a bit simpler, include the `js-cookie` library.
    - `index.html`
    `<script type="text/javascript" src="/webjars/js-cookie/js.cookie.js"></script>`
    - Finally, you can use Cookies convenience methods in XHR: `Cookies.get('XSRF-TOKEN'));`
### Login with GitHub
- ##### Initial setup
    To use Google’s OAuth 2.0 authentication system for login, you must set up a project in the Google API Console to obtain OAuth 2.0 credentials.

- ##### Setting the redirect URI
    Then, you need to configure the client to point Google. Because Spring Security is built with multiple clients in mind, you can add our Google credentials alongside the ones you created for GitHub.
- ##### Adding the Login Link
        <div class="container unauthenticated">
        <div>
            With GitHub: <a href="/oauth2/authorization/github">click here</a>
        </div>
        <div>
            With Google: <a href="/oauth2/authorization/google">click here</a>
        </div>
        </div>

### How to Add a Local User Database
* Many applications need to hold data about their users locally, even if authentication is delegated to an external provider. We don’t show the code here, but it is easy to do in two steps.

    - 1- Choose a backend for your database, and set up some repositories (using Spring Data, say) for a custom User object that suits your needs and can be populated, fully or partially, from external authentication.

    - 2- Implement and expose OAuth2UserService to call the Authorization Server as well as your database. Your implementation can delegate to the default implementation, which will do the heavy lifting of calling the Authorization Server. Your implementation should return something that extends your custom User object and implements OAuth2User.

### Adding an Error Page for Unauthenticated Users
* ##### Switching to GitHub
    The two-providers sample uses GitHub as an OAuth 2.0 provider.
* ##### Detecting an Authentication Failure in the Client
        <div class="container text-danger error"></div>
        $.get("/error", function(data) {
            if (data) {
                $(".error").html(data);
            } else {
                $(".error").html('');
            }
        });      
* ##### Adding an Error Message
        protected void configure(HttpSecurity http) throws Exception {
            // @formatter:off
            http
                // ... existing configuration
                .oauth2Login(o -> o
                    .failureHandler((request, response, exception) -> {
                        request.getSession().setAttribute("error.message", exception.getMessage());
                        handler.onAuthenticationFailure(request, response, exception);
                    })
                );
        }  
         
            @GetMapping("/error")
            public String error(HttpServletRequest request) {
                String message = (String) request.getSession().getAttribute("error.message");
                request.getSession().removeAttribute("error.message");
                return message;
            }