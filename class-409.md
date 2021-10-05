#  WRRC and Java
## The HTTP Request Lifecycle
* #### Step 1: Local Processing
    -  browser extracts the "scheme"/protocol (we have established that this will be HTTP), host (www.example.com), and optional port number, resource path, and query strings 

    - Now that the browser has the intended hostname for the request, it needs to resolve an IP address1. The browser will then look through its own cache of recently requested URLs, the operating system’s cache of recent queries, your router’s cache, and your DNS cache.

* #### Step 2: Resolve an IP
    - If the cache lookup fails (we will assume it does), your browser fires off a DNS request using UDP. The DNS request contains the preconfigured IP for your DNS server and your return IP in its header. The hostname for which you are trying to resolve an IP is in the request’s "Question" section. UDP is a lightweight protocol, but the tradeoff is that it offers no guarantees in terms of delivery, and there is no acknowledgement other than a response being sent and received.
    - request will now have to travel many network devices to reach its target DNS server. Whenever the packet hits a piece of networking equipment, the device uses a routing table to determine which other device it is connected to that is most likely situated along the shortest path to the destination.
    - Once request arrives at your configured DNS server, the server looks for the address associated with the requested hostname. If it finds one, it sends a response. If, on the other hand, the DNS server you have targeted cannot locate the given hostname, it passes the request along to another DNS server it is configured to defer to. This happens recursively until the address is found, or an "authoritative" nameserver is hit. If an address for the given domain cannot be resolved, the server responds with a failure and your browser returns an error.
    - We will assume the request is successful though, given that all of this is still a precursor. If the response makes it back (remember, with UDP there’s no guarantee!), the requesting client now has a target IP. It will also have received a piece of information as part of the answer that will let it know how long the returned answer can be cached for. 
* #### Step 3: Establish a TCP Connection
    - Much of this is done very simply, using what’s known as a sequence number for every byte sent. This allows the receiver to re-order received packets back into their original order, and allows the sender to retransmit any packet that does not get acknowledged by the receiver.

    - TCP connections are opened using what’s known as a three-way handshake: 
        - The client initiates the active open by sending a SYN "control" packet to the server. The client sets the sequence number for the first packet to a random value purposely, in service of security. We’ll refer to this number as x for now
        - The server responds with a SYN-ACK message, which contains an acknowledgement number for the original message that is always x+1, and a new sequence number for the response itself, which is another random number y.
        - In the third step, the client sends an ACK message back to the server with a sequence number equal to x+1, which should match the SYN-ACK message’s acknowledgment number and ensure that our data is being delivered reliably. The ACK message’s acknowledgment number (since it is acknowledging the SYN-ACK) is set to one more than the received sequence number, or y+1.
    - In the third step, the client sends an ACK message back to the server with a sequence nWe now have a completed three-way handshake and an established connection where both the client and server have received acknowledgment of the connection from the other party.

* #### Step 4: Send an HTTP Request
    - The request is made up of a "request line", request header, and a body. The "request line" is simply a line that indicates the HTTP method, the resource being requested, and the protocol version. The header of the request is made up of pairs in the form name:value < CR >< LF >. Two consecutive < CR >< LF > pairs indicate the end of the header section. The only mandatory field in an HTTP request is HOST, which contains the domain and port that the request is being sent to (domain.com:8080), although in some cases the port can be omitted. Outside of the host field, common standard HTTP header fields include Origin, Accept, Accept-Encoding, and many more. The request can also include any non-standard header fields, and historically non-standard fields are indicated by prefixing X- to the field’s name. The body content of an HTTP request is completely optional, but often contains something like form data or JSON.
    - Once the HTTP request is sent, it follows a similar routing procedure as the one discussed earlier, with the difference being that using TCPs magic sequence number powers, the server can ensure it receives the whole request, in the correct order.
    - Once the server receives the request, processes it, and finds the resource being requested, it generates an HTTP response. An HTTP response has a similar structure to an HTTP request, containing a "status line", response header fields, and an optional body. The status line contains an HTTP status code indicating the success, failure, or error-state of the request along with a "reason message" that provides detail.
    - Once the response is generated, the server responds to the request. At the TCP layer, the client receives the first data packet, the first byte of which should contain the HTTP response header. More packets start coming in, and at the TCP layer they are re-ordered as needed. For every two packets that the client receives at the TCP layer, it sends an ACK message to the server. This goes on until the response is (hopefully) fully loaded.
* #### Step 5: Tearing Down and Cleaning Up
    - Once the response has been fully delivered, the client sends a FIN packet at the TCP level, to which the server responds with an ACK, and then generally sends a FIN of its own, which the client responds to with its own ACK signal. The client then waits for a brief timeout, during which it cannot accept new connections, to prevent delayed packets from previous connections arriving during subsequent activities on the port. This four way handshake signals the end of the TCP connection.
    - At this point, your browser begins processing what it has received. If it is an image, data, or other media file that is being consumed by some application inside the browser, a variety of things can happen. If the data received is HTML, the browser will start parsing the HTML, and rendering the page you requested. As it parses, the browser may come across links to images or other media that are external to the HTML it has received, and will spin up new requests for that content, restarting this whole process (although usually skipping steps 1 & 2 thanks to caching). But, given that we were only interested in the lifecycle of an individual request: our (application’s) work is done, congratulations!

## Do a Simple HTTP Request in Java
* #### HttpUrlConnection
    - The HttpUrlConnection class allows us to perform basic HTTP requests without the use of any additional libraries. All the classes that we need are part of the java.net package.

    - The disadvantages of using this method are that the code can be more cumbersome than other HTTP libraries and that it does not provide more advanced functionalities such as dedicated methods for adding headers or authentication.
* #### Creating a Request
    - create an HttpUrlConnection instance using the openConnection() method of the URL class.
    - The HttpUrlConnection class is used for all types of requests by setting the requestMethod attribute to one of the values: GET, POST, HEAD, OPTIONS, PUT, DELETE, TRACE.

    ***URL url = new URL("http://example.com");***
    ***HttpURLConnection con = (HttpURLConnection) url.openConnection();***
    ***con.setRequestMethod("GET");***

* #### Adding Request Parameters
    - set the doOutput property to true, then write a String of the form param1=value¶m2=value to the OutputStream of the HttpUrlConnection instance:

    ***Map< String, String > parameters = new HashMap<>();***
    ***parameters.put("param1", "val");***
    ***con.setDoOutput(true);***
    ***DataOutputStream out = new DataOutputStream(con.getOutputStream());***
    ***out.writeBytes(ParameterStringBuilder.getParamsString(parameters));***
    ***out.flush();***
    ***out.close();***

* #### Setting Request Headers
    - to set requset header

    ***con.setRequestProperty("Content-Type", "application/json");***

    - to get requset header 

    ***String contentType = con.getHeaderField("Content-Type");***

* #### Configuring Timeouts
    - HttpUrlConnection class allows setting the connect and read timeouts. These values define the interval of time to wait for the connection to the server to be established or data to be available for reading.

    ***con.setConnectTimeout(5000);***
    ***con.setReadTimeout(5000);***

* #### Handling Cookies
    - First, to read the cookies from a response, we can retrieve the value of the Set-Cookie header and parse it to a list of HttpCookie objects:

    ***String cookiesHeader = con.getHeaderField("Set-Cookie");***
    ***List< HttpCookie > cookies = HttpCookie.parse(cookiesHeader);***

    - add the cookies to the cookie store:

    ***cookies.forEach(cookie -> cookieManager.getCookieStore().add(null, cookie));***

    - check if a cookie called username is present, and if not, 

    ***Optional< HttpCookie > usernameCookie = cookies.stream() .findAny().filter(cookie -> cookie.getName().equals("username"));***
    ***if (usernameCookie == null) {***
    ***cookieManager.getCookieStore().add(null, new HttpCookie("username", "john"));}***
    - add the cookies to the request, we need to set the Cookie header, after closing and reopening the connection:

    ***con.disconnect();***
    ***con = (HttpURLConnection) url.openConnection();***
    ***con.setRequestProperty("Cookie",StringUtils.join(cookieManager.getCookieStore().getCookies(), ";"));***

* #### Handling Redirects
    - enable or disable automatically following redirects for a specific connection by using the setInstanceFollowRedirects() method with true or false parameter:

    **con.setInstanceFollowRedirects(false);**

    - enable or disable automatic redirect for all connections:

    ***HttpUrlConnection.setFollowRedirects(false);***

* #### Reading the Response
    - To execute the request, we can use the getResponseCode(), connect(), getInputStream() or getOutputStream() methods:

    ***int status = con.getResponseCode();***

    - read the response of the request and place it in a content String:

    ***BufferedReader in = new BufferedReader(new InputStreamReader(con.getInputStream()));***
    ***String inputLine;***
    ***StringBuffer content = new StringBuffer();***
    ***while ((inputLine = in.readLine()) != null) {content.append(inputLine);}***
    ***in.close();***

    - To close the connection, we can use the disconnect() method:

     ***con.disconnect();***

* #### Reading the Response on Failed Requests
    - We can decide which InputStream to use by comparing the HTTP status code:

    ***int status = con.getResponseCode();***
    ***Reader streamReader = null;***
    ***if (status > 299) {streamReader = new InputStreamReader(con.getErrorStream());}*** 
    ***else {streamReader = new InputStreamReader(con.getInputStream());}***

* #### Building the Full Response
    - we can build it using some of the methods that the HttpUrlConnection instance offers:

        ***StringBuilder fullResponseBuilder = new StringBuilder();***