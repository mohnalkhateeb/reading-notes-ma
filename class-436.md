# Cognito
# AUTHENTICATION
- The Amplify Auth category provides an interface for authenticating a user. Behind the scenes, it provides the necessary authorization to the other Amplify categories. It comes with default, built-in support for Amazon Cognito User Pool and Identity Pool. The Amplify CLI helps you to create and configure the auth category with an authentication provider.
- Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Apple, Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0 and OpenID Connect.  

### Goal
- To setup and configure your application with Amplify Auth and go through a simple api to check the current auth session

### Prerequisites
- An Android application targeting at least Android SDK API level 16 with Amplify libraries integrated

### Configure Auth Category
- To start provisioning auth resources in the backend, go to your project directory and execute the command:

        amplify add auth

- Enter the following when prompted:

        ? Do you want to use the default authentication and security configuration?
            `Default configuration`
        ? How do you want users to be able to sign in?
            `Username`
        ? Do you want to configure advanced settings?
            `No, I am done.`

- To push your changes to the cloud, execute the command:

        amplify push

### Install Amplify Libraries
- Add the following dependency to your app's build.gradle along with others you added above in Prerequisites and click "Sync Now" when prompted:

        dependencies {
            implementation 'com.amplifyframework:aws-auth-cognito:1.28.3'
        }

### Initialize Amplify Auth
Add the Auth plugin before calling Amplify.configure

        // Add this line, to include the Auth plugin.
        Amplify.addPlugin(new AWSCognitoAuthPlugin());
        Amplify.configure(getApplicationContext());

### Check the current auth session
We can now check the current auth session.

For testing purposes, you can run this from your MainActivity's onCreate method.

        Amplify.Auth.fetchAuthSession(
            result -> Log.i("AmplifyQuickstart", result.toString()),
            error -> Log.e("AmplifyQuickstart", error.toString())
        );

### Register a user
- The default CLI flow as mentioned in the getting started guide requires a username, password and a valid email id as parameters to register a user. Invoke the following api to initiate a sign up flow.

        AuthSignUpOptions options = AuthSignUpOptions.builder()
            .userAttribute(AuthUserAttributeKey.email(), "my@email.com")
            .build();
        Amplify.Auth.signUp("username", "Password123", options,
            result -> Log.i("AuthQuickStart", "Result: " + result.toString()),
            error -> Log.e("AuthQuickStart", "Sign up failed", error)
        );

- The next step in the sign up flow is to confirm the user. A confirmation code will be sent to the email id provided during sign up. Enter the confirmation code received via email in the confirmSignUp call.

        Amplify.Auth.confirmSignUp(
            "username",
            "the code you received via email",
            result -> Log.i("AuthQuickstart", result.isSignUpComplete() ? "Confirm signUp succeeded" : "Confirm sign up not complete"),
            error -> Log.e("AuthQuickstart", error.toString())
        );

- You will know the sign up flow is complete if you see the following in your console window:

        Confirm signUp succeeded

### Sign in a user
Implement a UI to get the username and password from the user. After the user enters the username and password you can start the sign in flow by calling the following method:

    Amplify.Auth.signIn(
        "username",
        "password",
        result -> Log.i("AuthQuickstart", result.isSignInComplete() ? "Sign in succeeded" : "Sign in not complete"),
        error -> Log.e("AuthQuickstart", error.toString())
    );

- You will know the sign in flow is complete if you see the following in your console window:

        Sign in succeeded
