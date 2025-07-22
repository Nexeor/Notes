2025-03-11 15:22

Status: #Leaf 

Tags: #WebDev 

# HTTP
HyperText Transfer Protocol (HTTP) is an [[application-layer]] protocol that enables communication between a client and a server in a request-response model. This protocol defines the format of the clients request and the format of the servers response. It is not concerned with how the data is physically sent (that is up to TCP in the [[transport-layer]]), instead only concerned with the formatting of how the requests and responses are sent. This way, any client can communicate with any server as long as they are both using HTTP. 
### Request Response Model
HTTP uses a client-server model. A client, such as a web browser, sends an HTTP request. The sever processes this request and then sends an HTTP response. 

A **HTTP request** specifies the client action. It consists of:
- **Request Line:** HTTP method, requested URL, and HTTP version
- **Headers**: Metadata, like the language or content type
- **Body (Optional)**: Data being sent to the server

A **HTTP response** specifies the result of the action. It consists of: 
- **Status line:** HTTP version and status code
- **Headers:** Response metadata
- **Body:** The response content
	- Response is usually JSON (for data) or HTML (for pages), but can be anything, including XML, plain text, binary, javascript, and much more
	- The HTTP response header contains the type of data being sent back, so the client knows how to interpret it

A GET request might look like this:
- The first line is the **request line**, specifying the HTTP method (GET), the URL (`/index.html`), and the HTTP version (`HTTP/1.1`)
- The second block are the **headers**, specifying different metadata
	- `Host` is the domain the client wants to access
	- `User-Agent` is the clients software 
- This request has no **body** because GET requests data
```http
GET /index.html HTTP/1.1

Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.82 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
```

A similar response might look like:
- The first line is the **status line**, containing the HTTP version and status code
- The second group are the **headers**, containing metadata about the content
- The third group is the **body**, containing the requested content (in this case an HTML webpage)
```http
HTTP/1.1 200 OK

Date: Mon, 11 Mar 2025 10:00:00 GMT
Server: Apache/2.4.29 (Ubuntu)
Content-Type: text/html; charset=UTF-8
Content-Length: 350
Connection: keep-alive

<html>
    <head>
        <title>Welcome to Example</title>
    </head>
    <body>
        <h1>Welcome to Example.com</h1>
        <p>This is the homepage of Example.com!</p>
    </body>
</html>

```
### Methods
HTTP comes packaged with several methods, or "verbs", to perform common CRUD operations. All requests must specify a method. The four most commonly used verbs are:
- **GET**: Retrieve data 
- **POST**: Send data
- **PUT**: Update existing data
- **DELETE**: Remove data

There are three other, less often used verbs:
- **PATCH**: Partially update data
- **HEAD**: Retrieve only the headers with no body
- **OPTIONS**: Check what other methods (GET, POST) are available for this resource
### Status Codes
Similarly to how a request provides a method to determine the type of operation, the response must contain a status code that reports whether the operation succeeded or not. There are many different status codes, with the first digit always determining the general type:

| Code    | Meaning       | Example                                                |
| ------- | ------------- | ------------------------------------------------------ |
| **1xx** | Informational | `100 Continue`                                         |
| **2xx** | Success       | `200 OK`, `201 Created`                                |
| **3xx** | Redirection   | `301 Moved Permanently`, `302 Found`                   |
| **4xx** | Client Error  | `400 Bad Request`, `404 Not Found`                     |
| **5xx** | Server Error  | `500 Internal Server Error`, `503 Service Unavailable` |
The most common status codes are:
- `200 OK`: Request was successful and server returned the data
- `301 Moved Permanently`: The requested resource has been moved to a new URL
	- The `Location` header specifies the new URL
- `400 Bad Request`: The server cannot process the request due to malformed data or invalid syntax
	- Client Error: The client must reform the request 
- `404 Not Found`: The sever cannot find the requested resource. Either the URL doesn't exist or has been deleted
	- Client Error: The client must send to the correct URL
- `500 Internal Server Error`: Something went wrong with the server while it was processing the request, but the server doesn't know what happened
	- Server Error: Something with the server is incorrectly configured and must be fixed
## References
