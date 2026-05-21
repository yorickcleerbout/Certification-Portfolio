# 🖥 **Introduction to APIs**
### 📌 **What is an API?**

> **A**pplication **P**rogramming **I**nterface
> A set of rules that allows two applications to communicate.
#### 🔗 Connections:
* **A2A** – App ↔ App
* **A2S** – App ↔ Server
* Service ↔ Consumer

---
### ⚙️ **Fundamental components of an API**
| Component    | Purpose                           | Examples                        |
| ------------ | --------------------------------- | ------------------------------- |
| **Protocol** | Defines _how_ apps communicate    | REST, SOAP, GraphQL, WebSockets |
| **Endpoint** | Defines _where_ requests are sent | `/users`, `/products`           |
| **Response** | Defines _what_ is returned        | JSON, XML, HTML                 |
#### 💡 Remember
- **Protocol → How?**
- **Endpoint → Where?**
- **Response → What?**

---
### 🌐 **What is a REST API?**

> **RE**presentational **S**tate **T**ransfer – works like the web.

- Stateless: no memory of past requests
- Each resource has a unique **URL**
- Uses HTTP methods:
	- **GET** → Read data
	- **POST** → Create data
	- **PUT** → Update data
	- **DELETE** → Delete data
- Common formats: JSON, XML

---
### 🏢 **Monolithic Architecture Challenges**

![[monolithic-architecture.png]]
#### ✅ Pros:

- Simple to start
- Easy to develop, test, deploy
#### ⚠️ Cons:

- Becomes complex over time
- Slows down feature development
- Hard to maintain

---
### 🧩**Microservices as a Solution**

#### 📊 Monolith vs Microservices
| **Microservices**             | **Monolith**            |
| ----------------------------- | ----------------------- |
| Small, independent codebases  | One big codebase        |
| Deploy services independently | Whole app must redeploy |
| Different tech per service    | One tech stack          |
| Scale specific services       | Scale everything        |

---
#### 🚀 **Advantages of Microservices**
|**Benefit**|**Why It Matters**|
|---|---|
|**Simpler management**|Divide into smaller, maintainable parts|
|**Isolated failures**|One service down ≠ whole system down|
|**Better team structure**|Clear service ownership|
|**Selective scaling**|Save resources, scale only what’s needed|

---
#### 🔄 **Transition to Microservices**

1. **Extract business logic** → standalone app + own database
2. **Add NFRs**: Auth, Logging, Security
3. **Expose an API** for interaction
4. **Scale** with multiple instances
5. **Add Load Balancer** for traffic distribution
6. Repeat for other features

![[monolithic-to-microservices.png]]

⚠️ **Challenge**: Duplicated Auth/Logging
💡 **Solution**: Centralize via **API Gateway**

---
### 🛡**API Gateway**
#### 📌 What is it?

> An API gateway is a central "door" that grands access to all backend services from one place. The API Gateway can process and modify the requests and responses as needed. The API Gateway acts as a gatekeeper between the client application and the backend services, it serves as a single entry-point for all requests. It not only knows where to route the request to, but can also enhance security, manage large volume of traffic, and ensure efficient communication.

---
#### **🛠 How it works**

![[apigateway.png]]

1. Client → API Gateway
2. Gateway checks endpoint & headers
3. Verifies credentials (if enabled)
4. Enforces quotas & rate limits
5. Transforms requests if needed
6. Routes to backend service(s)
7. Backend processes request
8. Response → Gateway → Client

---
#### 🌟**Benefits**

> 💡 **Think**: _Performance + Security + Simplicity_

- **Reduced complexity** – Centralized logic
- **Performance boost** – Caching, routing, load balancing
- **Security** – Auth, attack prevention
- **Resilience** – Graceful failover
- **Lower cost** – Consolidated infrastructure
- **Better Developer Experience** – Easier versioning, deployment, doc

---
# 🖥 **Introduction to API Management**

### 🌟**Business Benefits of APIs**

APIs provide businesses with powerful tools to grow, innovate, and operate more efficiently.
#### 🚀 Key Benefits

* **Enhanced Connectivity**
	APIs connect different systems and software, synchronizing data across platforms,  streamlining workflows, and boosting efficiency.
	
* **Increased Revenue**
	Companies can monetize APIs directly (charging for access) or indirectly (building paid apps around them).
	
* **Innovation**
	APIs foster new product and service development by combining existing capabilities.
	
* **Cost Efficiency**
	Reusing existing APIs reduces development time and costs, avoiding “reinventing the wheel.”
	
* **Analytics & Insights**
	APIs enable effective data collection and analysis, helping businesses understand customer behavior, market trends, and performance.
	
* **Scalability & Flexibility**
	APIs let businesses scale operations and add features without overhauling existing systems.

>✅ **In summary:** APIs enhance connectivity, open new revenue streams, drive innovation, cut costs, provide insights, and support scalability.

---
### 📌 What is API Management?

API Management is the **end-to-end process** of designing, deploying, monitoring, and maintaining APIs so they are:

- **Secure**
- **Efficient**
- **Aligned with business goals**

At its core, API management revolves around the **API Lifecycle**:  
Design → Build → Test → Deploy → Publish → Maintain → Retire

---
### 🔑 Focus Areas of API Management

![[api-management-life-cycle.png]]

|**Focus Area**|**Description**|
|---|---|
|**Standard Practice**|Consistency in design, development, and documentation using best practices.|
|**Governance**|Ensures APIs comply with security, privacy, and regulatory requirements.|
|**Control**|Access control (authentication & authorization) and versioning strategies.|
|**Observability**|Monitoring usage, performance, and error rates for insights & troubleshooting.

---
### 📌 Importance of API Management

API Management enables organizations to maximize the value of APIs by ensuring **security, scalability, and governance**.
#### ✅ Capabilities

- **Scale** — Dynamically adjust infrastructure to meet demand
- **Protect** — Secure with authentication & authorization
- **Govern** — Enforce policies and compliance
- **Redirect** — Send clients to the latest API versions
- **Track** — Manage multiple versions simultaneously
- **Rollback** — Allow reverting to previous versions if needed
- **Resilience** — Ensure high availability and fault tolerance
- **Throttle** — Regulate traffic based on usage patterns
- **Monitor & Analyze** — Track usage and performance for optimization

---
### 🛠 Core Features of API Management

![[core-features-api-management.png]]

- **API Gateway** → Routes requests between providers and consumers
- **API Portal** → Central hub for access, testing, and documentation
- **Analytics** → Provides metrics for usage and performance insights
- **Security** → Role-Based Access Control (RBAC), authentication, and authorization
- **Lifecycle Management** → Tracks API journey from design through retirement
- **Policies** → Define rules and behaviors for APIs (e.g., throttling, access control)

---
### 🔄 API Management Lifecycle

The lifecycle ensures APIs remain **useful, reliable, and secure** throughout their existence.

![[api-lifecycle.png]]

1. **Design** — Define structure, endpoints, and data models.
2. **Mock** — Create prototypes for early feedback/testing.
3. **Test** — Verify functionality, performance, and security.
4. **Manage** — Maintain and update APIs continuously.
5. **Onboard** — Provide docs, tools, and support for users.
6. **Portal** — Developer portal as a central API resource hub.
7. **Analytics** — Track usage and performance trends.
8. **Monitor** — Continuous performance and security monitoring.
9. **API Programs** — Strategic initiatives to maximize adoption and developer engagement.

---

### 📝 Quiz – Introduction to APIs and API Management


>❓**What is a microservices architecture?**

-  <input type="radio" name="q1"> A software development approach where an application is built as a collection of small, loosely coupled services.
* <input type="radio" name="q1"> A single-tiered software application that combines the user interface and data access code
* <input type="radio" name="q1"> A method for scaling a monolithic application by replicating it on multiple servers
* <input type="radio" name="q1"> A programming language used to develop APIs

<details>
	<summary>💡 Reveal Answer</summary>
	- A software development approach where an application is built as a collection of small, loosely coupled services.
</details>

>❓**What is the purpose of a response code in an API interaction?**

- <input type="radio" name="q2"> To provide the client with a summary of the requested data
- <input type="radio" name="q2"> Tell the client whether the request could be handled successfully or not
- <input type="radio" name="q2"> To indicate the specific endpoint where the data was retrieved from
- <input type="radio" name="q2"> To transmit messages between applications using diverse protocols such as REST and SOAP

<details>
	<summary>💡 Reveal Answer</summary>
	- Tell the client whether the request could be handled successfully or not
</details>

>❓**What is a REST API?**

- <input type="radio" name="q3"> A set of rules and standards that allow different software applications to communicate over the internet using HTTP methods
- <input type="radio" name="q3"> An API that only connects mobile apps to external systems
- <input type="radio" name="q3"> An API that uses SOAP instead of HTTP
- <input type="radio" name="q3"> An API that only allows viewing of data, but not updating or deleting it

<details>
	<summary>💡 Reveal Answer</summary>
	- A set of rules and standards that allow different software applications to communicate over the internet using HTTP methods
</details>

>❓**What is the difference between a monolithic architecture and a microservices architecture in terms of functionality?**

* <input type="radio" name="q4"> A monolithic architecture puts each module of functionality into a separate service, while a microservices architecture combines all functionality into a single service
* <input type="radio" name="q4"> A monolithic architecture scales by distributing services across multiple servers, while a microservices architecture replicates on a single server
* <input type="radio" name="q4"> A monolithic architecture puts all functionality into a single applications, while a microservices architecture puts each module of functionality into a separate service
* <input type="radio" name="q4"> A monolithic architecture relies on a single technology stack, while a microservices architecture supports multiple technology stacks

<details>
	<summary>💡 Reveal Answer</summary>
	- A monolithic architecture puts all functionality into a single applications, while a microservices architecture puts each module of functionality into a separate service
</details>

>❓**What is the protocol of an API?**

- <input type="radio" name="q5"> The specific URL where requests are sent and data is retrieved
- <input type="radio" name="q5"> The response data returned after a successful API call
- <input type="radio" name="q5"> The set of rules and standards that govern the conversation between applications
- <input type="radio" name="q5"> The unique address of each API

<details>
	<summary>💡 Reveal Answer</summary>
	- The set of rules and standards that govern the conversation between applications
</details>

>❓**What are the benefits of a microservices architecture compared to a monolithic architecture?**

- [ ] Smaller code bases for easier maintenance and understanding
- [ ] Independent development, testing, and deployment of services
- [ ] Faster development cycles and enhanced agility
- [ ] Greater scalability and fault tolerance
- [ ] A single technology stack for the entire application

<details>
	<summary>💡 Reveal Answer</summary>
	- Smaller code bases for easier maintenance and understanding<br />
	- Independent development, testing, and deployment of services<br />
	- Faster development cycles and enhanced agility<br />
	- Greater scalability and fault tolerance
</details>

> ❓**In a microservices architecture, each service is developed, deployed, and scaled [ ______ ].**

- <input type="radio" name="q6"> Independently
- <input type="radio" name="q6"> As one

<details>
	<summary>💡 Reveal Answer</summary>
	- Independently
</details>

>❓**What is an API endpoint?**

- <input type="radio" name="q7"> The protocol that governs the conversation between applications
- <input type="radio" name="q7"> The specific URL where requests are sent and data is retrieved
- <input type="radio" name="q7"> The response data returned after a successful API call
- <input type="radio" name="q7"> The unique address of each API

<details>
	<summary>💡 Reveal Answer</summary>
	- The specific URL where requests are sent and data is retrieved
</details>

>❓**What are the challenges of a monolithic architecture when an application expands?**

- [ ] It becomes harder to manage the code base
- [ ] Small changes can affect the entire system, reducing agility
- [ ] Continuous deployment can be challenging as any update requires redeploying the entire application
- [ ] It is easier to adopt new technologies or frameworks that are more appropriate for specific functionality

<details>
	<summary>💡 Reveal Answer</summary>
	 - It becomes harder to manage the code base<br />
	- Small changes can affect the entire system, reducing agility<br />
	- Continuous deployment can be challenging as any update requires redeploying the entire application
</details>

---

>  ⬅️ [Previous: (KGAC-101) Kong Gateway Foundations](Certification-Portfolio/Kong%20Gateway%20Certified%20Associate/(KGAC-101)%20Kong%20Gateway%20Foundations/README.md) | ➡️ [Next: (KGLL-118) Introduction to Kong Gateway](./%28KGLL-118%29%20Introduction%20to%20Kong%20Gateway.md)

---
