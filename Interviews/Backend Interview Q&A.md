---
tags:
  - interviews
  - backend
---
### 1. What is the main purpose of a backend in a web application?

**Answer:**  
The backend handles business logic, data management, security, and communication with external systems.  
It acts as the bridge between frontend and database, ensuring requests are processed correctly and securely.

---

### 2. What is the difference between REST and GraphQL APIs?

**Answer:**

|Feature|REST|GraphQL|
|---|---|---|
|Structure|Multiple endpoints|Single endpoint|
|Data Fetching|Fixed response structure|Client specifies data shape|
|Over-fetching|Common|Eliminated|
|Versioning|Needed for changes|Schema evolves without versioning|

GraphQL is more flexible; REST is simpler and widely adopted.

---

### 3. What are HTTP methods, and when do you use each?

**Answer:**

|Method|Purpose|
|---|---|
|GET|Retrieve data|
|POST|Create new resource|
|PUT|Replace entire resource|
|PATCH|Update part of a resource|
|DELETE|Remove a resource|

Example:

- `GET /users` → fetch all users
    
- `POST /users` → add new user
    

---

### 4. What are middleware functions and why are they important?

**Answer:**  
Middleware intercepts requests before they reach route handlers.  
They’re used for:

- Authentication/authorization
    
- Logging
    
- Request validation
    
- Error handling
    

They keep code modular and reusable.

---

### 5. What’s the difference between synchronous and asynchronous processing?

**Answer:**

- **Synchronous:** Tasks execute one after another (blocking).
    
- **Asynchronous:** Tasks run concurrently using event loops or background workers.
    

Example: Fetching from a database asynchronously avoids blocking other user requests.

---

### 6. What is authentication vs authorization?

**Answer:**

- **Authentication:** Verifies who you are (login).
    
- **Authorization:** Verifies what you can access (permissions).
    

Example:

- Login = Authentication
    
- Admin-only dashboard access = Authorization
    

---

### 7. What are JWTs (JSON Web Tokens)?

**Answer:**  
JWT is a compact, signed token used for stateless authentication.  
It has three parts: `header.payload.signature`  
Stored on the client side (e.g., localStorage or cookie).

Advantages:

- No session storage on server
    
- Easy to scale APIs horizontally
    

---

### 8. Explain the difference between monolithic and microservices architectures.

**Answer:**

|Aspect|Monolithic|Microservices|
|---|---|---|
|Structure|Single unified codebase|Multiple independent services|
|Scaling|Scale entire app|Scale individual services|
|Deployment|Single deploy|Each service deploys separately|
|Communication|In-process|Over network (REST/gRPC)|

Microservices are modular but add deployment and communication complexity.

---

### 9. What is caching and why is it used?

**Answer:**  
Caching stores frequently accessed data temporarily to reduce computation and database load.  
Common tools: Redis, Memcached  
Used for:

- Session data
    
- API responses
    
- Query results
    

Improves response time and scalability.

---

### 10. What is load balancing?

**Answer:**  
Distributing incoming traffic across multiple servers to:

- Prevent overload
    
- Improve uptime
    
- Enhance performance
    

Examples: Nginx, HAProxy, AWS ELB

---

### 11. What are database transactions and why are they important?

**Answer:**  
A transaction is a unit of work that must either fully succeed or fully fail.  
It ensures ACID properties:

- Atomicity
    
- Consistency
    
- Isolation
    
- Durability
    

Example: Bank transfer = debit + credit → both succeed or both rollback.

---

### 12. What are indexes in databases and when should you use them?

**Answer:**  
Indexes speed up query lookups by maintaining sorted data structures (like B-Trees).  
Use them on frequently filtered/search columns, but not excessively (they slow down writes).

---

### 13. Explain what rate limiting is and why it’s important.

**Answer:**  
Rate limiting restricts how many requests a user/IP can make within a timeframe.  
Prevents:

- DDoS attacks
    
- API abuse
    
- Resource exhaustion
    

Implemented via Redis counters or API gateways.

---

### 14. What is an API gateway?

**Answer:**  
An API Gateway acts as a single entry point for microservices.  
It handles:

- Routing
    
- Load balancing
    
- Authentication
    
- Logging
    
- Rate limiting
    

Examples: Kong, Nginx, AWS API Gateway

---

### 15. What is the difference between vertical and horizontal scaling?

**Answer:**

|Type|Description|
|---|---|
|Vertical|Adding more power (CPU/RAM) to one server|
|Horizontal|Adding more servers (load balancing)|

Horizontal scaling is better for distributed systems.

---

### 16. What is the difference between SQL and NoSQL databases?

**Answer:**

|Feature|SQL|NoSQL|
|---|---|---|
|Schema|Fixed|Flexible|
|Data Type|Relational|Document / Key-value / Graph|
|Examples|PostgreSQL, MySQL|MongoDB, Redis|
|Use Case|Complex queries|Fast & scalable reads/writes|

---

### 17. What is ORM and why is it used?

**Answer:**  
ORM (Object Relational Mapping) connects database tables with objects in your language.  
Example: Sequelize (JS), SQLAlchemy (Python).  
Reduces SQL boilerplate and increases maintainability.

---

### 18. Explain the concept of web sockets.

**Answer:**  
WebSockets provide real-time, two-way communication between client and server.  
Unlike HTTP, connection stays open.  
Used for:

- Chat apps
    
- Live notifications
    
- Multiplayer games
    

---

### 19. What are background jobs or queues used for?

**Answer:**  
Used to process time-consuming tasks asynchronously (email sending, report generation, etc.)  
Tools:

- Celery (Python)
    
- BullMQ / RabbitMQ (Node.js)
    
- Redis Queue (RQ)
    

---

### 20. What are common security best practices for backend APIs?

**Answer:**

- Validate and sanitize all inputs
    
- Use HTTPS
    
- Hash passwords (e.g., bcrypt, Argon2)
    
- Use JWTs or OAuth securely
    
- Implement rate limiting
    
- Never expose sensitive env variables
    
- Keep dependencies updated
    

---

### 21. What is the difference between synchronous APIs and event-driven systems?

**Answer:**

- Synchronous: Client waits for response (e.g., REST APIs)
    
- Event-driven: System reacts to events asynchronously (e.g., Kafka, RabbitMQ)
    

Event-driven improves decoupling and scalability.

---

### 22. What is gRPC and how does it differ from REST?

**Answer:**  
gRPC uses Protocol Buffers (binary format) instead of JSON for communication.

- Faster and smaller payloads
    
- Strongly typed contracts
    
- Supports bi-directional streaming
    

Ideal for microservices and internal APIs.

---

### 23. How would you handle versioning in APIs?

**Answer:**

- URL versioning: `/api/v1/users`
    
- Header versioning: `Accept: application/vnd.myapp.v2+json`
    
- Maintain backward compatibility during transitions
    

---

### 24. What’s the difference between logging and monitoring?

**Answer:**

- Logging: Captures event details (errors, requests)
    
- Monitoring: Tracks system health and metrics (CPU, memory, uptime)
    

Combined → observability.

---

### 25. How do you test backend APIs effectively?

**Answer:**

- Unit Tests: Test isolated logic (Jest, PyTest)
    
- Integration Tests: Test database + API endpoints
    
- Load Tests: Check performance (Locust, k6)
    
- Mock External APIs: Avoid hitting real services
    

---