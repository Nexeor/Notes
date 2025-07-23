2025-03-11 14:44

Status: #Leaf 

Tags: #API #Database #WebDev #REST

# RESTful API's
REST (REpresentational State Transfer) is an **architectural style** for designing API systems. This is opposed to the competing [[SOAP]], which is a **protocol**.
- **Architectural Style**: A set of design principles and constraints to guide API behavior without enforcing strict rules on how the data itself is formatted and transmitted. 
- **Protocol:** A strict set of rules dictating exactly how data is transmitted and formatted. 
	- HTTP can also be considered a protocol

In this way REST acts as a guiding set of principles of how we should design our API's, but doesn't dictate how we actually implement them behind the scenes. REST uses a separate protocol (typically [[HTTP]]) to determine how the data is actually formatted and sent.
## RESTful Constraints 
1) **Client-Server Architecture**
	- *Separation of Concerns*: RESTful assumes that the client (frontend) and the server (backend) operate independently.
	- The client sends a request to the server, and the server can fulfill that request without knowing anything about the client
	- **Advantages:**
		- *Scalability*: As client demand increases, we can scale up the server side dynamically
			- Also allows for a "microservice" architecture where client-server systems can be broken up into small pieces, with each service handing a specific function. This also allows us to scale up individual functions as needed, rather than the entire system
		- *Portability*: Because the client and server are independent, any client should be able to work with any server if the API is set up correctly. 
			- This allows us to split up services across different servers that serve different purposes
			- Any client can access our API, regardless of device or browser
		- *Maintainability:* By separating the client from the server, we can debug each seperately. Buissness logic lives on the server, and UI lives in the client. Both can be updated or changed independently of the other
			- Updating the server has no effect on the client and can be done faster
			- API versioning allows the system to support older clients while still updating and rolling out new features. 
2) **Statelessness**
	- The client knows **nothing** of the server and vice versa. A client request contains all the information needed to complete the quest, and a server response contains all the data the client needs. 
	- No state is saved in between requests, it must be sent by the client each time if needed
	- *Simplicity:* The same request should return the same response each time, acting like a function
3) **Cacheability**
	- Responses indicate whether they are **cacheable** or **non-cacheable**
		- This is indicated in [[HTTP]] headers
	- Must be managed properly to avoid stale data
	- *Performance*: Caching improves performance by reducing duplicate/redundant requests
4) **Layered System**
	- The system is composed of several *layers*, such as security, caching, and load balancing that each interact independently
	- Clients don't know whether they are interacting with the origin system, cache, or an intermediary
	- **Advantages**: 
		- *Security*: We can install a security layer in our API to prevent bad actors, but our client will never see nor directly interact with it 
		- *Scalability/Flexibility*: If we need to scale up our performance, a balancing or caching layer can be implemented on top of our existing system
5) **Uniform Interface**:
	- The system should interact with resources in a consistent manner
	- Each resource should have a **unique URI** 
		- `GET /users` retrieves all users, `GET /users/123` gets the user with ID 123
	- Clients interact with the resources through **representations** (JSON, XML), not the actual resource
	- Requests and responses should contain the necessary **metadata**
		- The server/client can use it to process the request/response appropriately
		- Headers are common way of expressing metadata
			- Specify the type of content: `Content-Type: application/json`
		- Can also include metadata in the body
			- Was the operation a success? `{ "status" : "success" }`
	- *Hypermedia as the Engine of Application State*: The responses should contain links to related actions and data so the user can dynamically interact with the API, learning its features over time
		- Also called HATEOAS
		- The API response not only includes the requested data, but also links to related data or actions
		- For example, if we send a GET request for a user, the response should contain the links for updating and deleting that user. It may also contain links to related users, creating a new user, and any other related action/data.
## References
