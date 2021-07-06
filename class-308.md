# **APIs**
* ### What does REST stand for?
     REST is an architectural style for building distributed systems based on hypermedia. REST is independent of any underlying protocol and is not necessarily tied to HTTP. However, most common REST API implementations use HTTP as the application protocol

* ### REST APIs are designed around a ____.
    REST APIs are designed around resources, which are any kind of object, data, or service that can be accessed by the client

* ### What is an identifer of a resource? Give an example.
    A resource has an identifier, which is a URI that uniquely identifies that resource. For example, the URI for a particular customer order might be:

    HTTP : https://adventure-works.com/orders/1

* ### What are the most common HTTP verbs?
     For REST APIs built on HTTP, the uniform interface includes using standard HTTP verbs to perform operations on resources. The most common operations are **GET**, **POST**, **PUT**, **PATCH**, and **DELETE**.
* ### What should the URIs be based on?
    Focus on the business entities that the web API exposes. For example, in an e-commerce system, the primary entities might be customers and orders. Creating an order can be achieved by sending an HTTP POST request that contains the order information. The HTTP response indicates whether the order was placed successfully or not. When possible, resource URIs should be based on nouns (the resource) and not verbs (the operations on the resource).

* ### Give an example of a good URI.
    https://adventure-works.com/orders

* ### What does it mean to have a ‘chatty’ web API? Is this a good or a bad thing?
    The more requests, the bigger the load. Therefore, try to avoid "chatty" web APIs that expose a large number of small resources. Such an API may require a client application to send multiple requests to find all of the data that it requires. Instead, you might want to denormalize the data and combine related information into bigger resources that can be retrieved with a single request.

* ### What status code does a successful GET request return?
    A successful **GET** method typically returns HTTP status code 200 (OK).

* ### What status code does an unsuccessful GET request return?
    If the resource cannot be found, **GET**  method should return 404 (Not Found).
* ### What status code does a successful POST request return?
    If a POST method creates a new resource, it returns HTTP status code 201 (Created). The URI of the new resource is included in the Location header of the response. The response body contains a representation of the resource.

* ### What status code does a successful DELETE request return?
    If the delete operation is successful, the web server should respond with HTTP status code 204, indicating that the process has been successfully