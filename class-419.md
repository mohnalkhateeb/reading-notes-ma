# Using WebSocket to build an interactive web application
WebSocket is a thin, lightweight layer above TCP. This makes it suitable for using “subprotocols” to embed messages. In this guide, we use STOMP messaging with Spring to create an interactive web application. STOMP is a subprotocol operating on top of the lower-level WebSocket.

### Adding Dependencies
    implementation 'org.webjars:webjars-locator-core'
    implementation 'org.webjars:sockjs-client:1.0.2'
    implementation 'org.webjars:stomp-websocket:2.3.3'
    implementation 'org.webjars:bootstrap:3.3.7'
    implementation 'org.webjars:jquery:3.1.1-1'

### Create a Resource Representation Class
- The service will accept messages that contain a name in a STOMP message whose body is a JSON object
- To model the message that carries the name, you can create a plain old Java object with a name property and a corresponding `getName()` method
- Upon receiving the message and extracting the name, the service will process it by creating a greeting and publishing that greeting on a separate queue to which the client is subscribed. The greeting will also be a JSON object, which as the following listing shows:

        {
            "content": "Hello, Fred!"
        }
- To model the greeting representation, add another plain old Java object with a content property and a corresponding `getContent()` method
- Spring will use the Jackson JSON library to automatically marshal instances of type Greeting into JSON.

- Next, you will create a controller to receive the hello message and send a greeting message.

### Create a Message-handling Controller
- In Spring’s approach to working with STOMP messaging, STOMP messages can be routed to` @Controller` classes. For example, the `GreetingController` 

        package com.example.messagingstompwebsocket;

        import org.springframework.messaging.handler.annotation.MessageMapping;
        import org.springframework.messaging.handler.annotation.SendTo;
        import org.springframework.stereotype.Controller;
        import org.springframework.web.util.HtmlUtils;

        @Controller
        public class GreetingController {


        @MessageMapping("/hello")
        @SendTo("/topic/greetings")
        public Greeting greeting(HelloMessage message) throws Exception {
            Thread.sleep(1000); // simulated delay
            return new Greeting("Hello, " + HtmlUtils.htmlEscape(message.getName()) + "!");
        }

        }
- This controller is concise and simple, but plenty is going on. We break it down step by step.

The `@MessageMapping` annotation ensures that, if a message is sent to the `/hello` destination, the `greeting()` method is called.
- The payload of the message is bound to a HelloMessage object, which is passed into `greeting()`.

- Internally, the implementation of the method simulates a processing delay by causing the thread to sleep for one second. This is to demonstrate that, after the client sends a message, the server can take as long as it needs to asynchronously process the message. The client can continue with whatever work it needs to do without waiting for the response.

- After the one-second delay, the `greeting()` method creates a `Greeting` object and returns it. The return value is broadcast to all subscribers of `/topic/greetings`, as specified in the `@SendTo` annotation. Note that the name from the input message is sanitized, since, in this case, it will be echoed back and re-rendered in the browser DOM on the client side.

### Configure Spring for STOMP messaging
- Now that the essential components of the service are created, you can configure Spring to enable WebSocket and STOMP messaging.

Create a Java class named `WebSocketConfig` that resembles the following listing

        package com.example.messagingstompwebsocket;

        import org.springframework.context.annotation.Configuration;
        import org.springframework.messaging.simp.config.MessageBrokerRegistry;
        import org.springframework.web.socket.config.annotation.EnableWebSocketMessageBroker;
        import org.springframework.web.socket.config.annotation.StompEndpointRegistry;
        import org.springframework.web.socket.config.annotation.WebSocketMessageBrokerConfigurer;

        @Configuration
        @EnableWebSocketMessageBroker
        public class WebSocketConfig implements WebSocketMessageBrokerConfigurer {

        @Override
        public void configureMessageBroker(MessageBrokerRegistry config) {
            config.enableSimpleBroker("/topic");
            config.setApplicationDestinationPrefixes("/app");
        }

        @Override
        public void registerStompEndpoints(StompEndpointRegistry registry) {
            registry.addEndpoint("/gs-guide-websocket").withSockJS();
        }

        }

- `WebSocketConfig` is annotated with `@Configuration` to indicate that it is a Spring configuration class. It is also annotated with `@EnableWebSocketMessageBroker`. As its name suggests, `@EnableWebSocketMessageBroker` enables WebSocket message handling, backed by a message broker.

- The `configureMessageBroker()` method implements the default method in `WebSocketMessageBrokerConfigurer` to configure the message broker. It starts by calling `enableSimpleBroker()` to enable a simple memory-based message broker to carry the greeting messages back to the client on destinations prefixed with `/topic`. It also designates the `/app` prefix for messages that are bound for methods annotated with `@MessageMapping`. This prefix will be used to define all the message mappings. For example, `/app/hello` is the endpoint that the `GreetingController.greeting()` method is mapped to handle.

- The `registerStompEndpoints()` method registers the `/gs-guide-websocket` endpoint, enabling SockJS fallback options so that alternate transports can be used if WebSocket is not available. The SockJS client will attempt to connect to `/gs-guide-websocket` and use the best available transport (websocket, xhr-streaming, xhr-polling, and so on).

### Create a Browser Client
- Create an `index.html` file similar to the following listing 

        <!DOCTYPE html>
        <html>
        <head>
            <title>Hello WebSocket</title>
            <link href="/webjars/bootstrap/css/bootstrap.min.css" rel="stylesheet">
            <link href="/main.css" rel="stylesheet">
            <script src="/webjars/jquery/jquery.min.js"></script>
            <script src="/webjars/sockjs-client/sockjs.min.js"></script>
            <script src="/webjars/stomp-websocket/stomp.min.js"></script>
            <script src="/app.js"></script>
        </head>
        <body>
        <noscript><h2 style="color: #ff0000">Seems your browser doesn't support Javascript! Websocket relies on Javascript being
            enabled. Please enable
            Javascript and reload this page!</h2></noscript>
        <div id="main-content" class="container">
            <div class="row">
                <div class="col-md-6">
                    <form class="form-inline">
                        <div class="form-group">
                            <label for="connect">WebSocket connection:</label>
                            <button id="connect" class="btn btn-default" type="submit">Connect</button>
                            <button id="disconnect" class="btn btn-default" type="submit" disabled="disabled">Disconnect
                            </button>
                        </div>
                    </form>
                </div>
                <div class="col-md-6">
                    <form class="form-inline">
                        <div class="form-group">
                            <label for="name">What is your name?</label>
                            <input type="text" id="name" class="form-control" placeholder="Your name here...">
                        </div>
                        <button id="send" class="btn btn-default" type="submit">Send</button>
                    </form>
                </div>
            </div>
            <div class="row">
                <div class="col-md-12">
                    <table id="conversation" class="table table-striped">
                        <thead>
                        <tr>
                            <th>Greetings</th>
                        </tr>
                        </thead>
                        <tbody id="greetings">
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
        </body>
        </html>

- This HTML file imports the SockJS and STOMP javascript libraries that will be used to communicate with our server through STOMP over websocket. We also import app.js, which contains the logic of our client application.

        var stompClient = null;

        function setConnected(connected) {
            $("#connect").prop("disabled", connected);
            $("#disconnect").prop("disabled", !connected);
            if (connected) {
                $("#conversation").show();
            }
            else {
                $("#conversation").hide();
            }
            $("#greetings").html("");
        }

        function connect() {
            var socket = new SockJS('/gs-guide-websocket');
            stompClient = Stomp.over(socket);
            stompClient.connect({}, function (frame) {
                setConnected(true);
                console.log('Connected: ' + frame);
                stompClient.subscribe('/topic/greetings', function (greeting) {
                    showGreeting(JSON.parse(greeting.body).content);
                });
            });
        }

        function disconnect() {
            if (stompClient !== null) {
                stompClient.disconnect();
            }
            setConnected(false);
            console.log("Disconnected");
        }

        function sendName() {
            stompClient.send("/app/hello", {}, JSON.stringify({'name': $("#name").val()}));
        }

        function showGreeting(message) {
            $("#greetings").append("<tr><td>" + message + "</td></tr>");
        }

        $(function () {
            $("form").on('submit', function (e) {
                e.preventDefault();
            });
            $( "#connect" ).click(function() { connect(); });
            $( "#disconnect" ).click(function() { disconnect(); });
            $( "#send" ).click(function() { sendName(); });
        });

