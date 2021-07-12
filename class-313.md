# **CRUD**
* ### In your own words, describe what each group of status code represents:
    - 100’s : These are informational status codes; they usually tell the client that the header part of the request has been received and the server will try to comply with a transmission demand of the client. Like using a different protocol or telling the client that its request will fail before they start sending the body.


    - 200’s  : These are the success codes. They tell the client that its request was accepted. In case of asynchronous processing of a request (202), this doesn’t mean the request was successfully processed only that it met all validation requirements at the time of sending.

    - 300’s : These are redirection codes. They tell the client that the resource they are requesting isn’t available at the expected location anymore. This can have multiple reasons, be temporary or permanent, but the client has to issue a request to the new location.

    - 400’s : These are the client error codes. They are all about invalid requests a client sent to a server. There are several causes to this, timeouts, wrong URI, missing authentication, etc. A client is sending incorrect input and should confirm the input parameters are correct before retrying the request.

    - 500’s : These are the server error codes. Often they indicate problems with overwhelmed servers or unreachable servers behind proxies, but sometimes they can be directly related to client requests that trigger error exceptions on the server. These errors can be temporary or permanent. Usually it’s best for the client to retry the same request.

* ### What is a status code 202?
     Often used for asynchronous processing. This code tells the client that the request was valid, but its processing will finish sometime in the future. The response body should include an URL to the finished resource with some information about when it will be available, or an URL to some monitoring endpoint that tells the client when the resource is available.

* ### What is a status code 308?
    This tells the client to use another URL to access the resource and not use the current URL anymore. It’s helpful when we have multiple endpoints for one resource, but don’t want to implement reading from all of them.

* ### What code would you use if an update didn’t return data to a client
    **204 No Content** : A proper code for updates that don’t return data to the client, for example when just saving a currently edited document

* ### What code would you use if a resource used to exist but no longer does?
    **414 Request-URI Too Long** : This is sometimes the case when the endpoint is right, and the resource exists, but the query makes the URL too long to be interpreted.

* ### What is the ‘Forbidden’ status code
    **403 Forbidden** : The client has authorized or doesn’t need to authorize itself, but still has no permissions to access the resource.


* ### Why do we need to pull our MongoDB database string out of our server and put it into our .env?
     To define an environment variable , simple type in the name of the variable , an equal-to sign and then the value.
    **VARIABLE_NAME = your_password**

* ### What is middleware?
    Middleware is software that provides common services and capabilities to applications outside of what’s offered by the operating system. Data management, application services, messaging, authentication, and API management are all commonly handled by middleware. Middleware helps developers build applications more efficiently. It acts like the connective tissue between applications, data, and users.

* ### What does app.use(express.json()) do?
    The express.json() function is a built-in middleware function in Express. It parses incoming requests with JSON payloads and is based on body-parser.

* ### What does the /:id mean in a route?
    it used to create only one route path where the id parameter is optional. To do this, specify it as : id? You can see an example in the route defined below

* ### What is the difference beween PUT and PATCH?
    The main difference between the PUT and PATCH method is that the PUT method uses the request URI to supply a modified version of the requested resource which replaces the original version of the resource, whereas the PATCH method supplies a set of instructions to modify the resource

* ### How do you make a defalut value in a schema?
    - You can also set the **default** schema option to a function. Mongoose will execute that function and use the return value as the **default**.
    - By default, mongoose only applies defaults when you create a new document. It will not set defaults if you use **update()** and **findOneAndUpdate()**. However, mongoose 4.x lets you opt-in to this behavior using the **setDefaultsOnInsert** option.

* ### What does a 500 error status code mean?
    **500** means Internal Server Error, which can be anything from a missing header field the backend accessed without checking its existence to an unreachable third party service the backend wanted to call.

* ### What is the difference between a status 200 and a status 201?
    - **The 200 status code** is by far the most common returned. It means, simply, that the request was received and understood and is being processed.
    - **A 201 status code** indicates that a request was successful and as a result, a resource has been created (for example a new page).

