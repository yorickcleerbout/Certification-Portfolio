# 🖥 **Introduction to Kong Gateway**

Kong Gateway is an **API management platform** that helps organizations design, secure, deploy, and monitor APIs across any cloud, protocol, or architecture. It supports the **entire API lifecycle** — from design (Insomnia), to deployment (Gateway proxying), to defining organizational standards and best practices.

---
## 🌐 **Kong Ecosystem Overview**

### Kong Konnect
- Multi-cloud API lifecycle management offered as a **SaaS platform**.
- Kong hosts the **management** and **control planes** in the cloud.
- Gateways (data planes) can run as:
    - **Dedicated Konnect cloud gateways**
    - **Self-managed** (on-prem or in the cloud)
### Kong Enterprise
- Extends Kong Gateway with:
	- Advanced plugins
	- Enterprise-grade security
	- 24/7 support
### Kong Gateway
- Lightweight, fast, flexible **reverse proxy**.
- Routes, manages, and secures API requests.
- Built for **decentralized, hybrid, and multi-cloud** architectures.
### Kong Mesh
- Enterprise service mesh for **Kubernetes & VMs**.
- Based on Kuma + Envoy.
- Provides:
    - Service discovery
    - Zero-trust security
    - Traffic reliability
### Kong Ingress Controller
- Runs Kong Gateway as a **Kubernetes ingress**.
- Converts Kubernetes resources (Ingress, HTTPRoute) → Kong Gateway configuration.
### Kong Insomnia
- Open-source REST client.
- Features:
    - Debug APIs
    - Send HTTP requests
    - Set headers
    - Inspect responses

---
## 🏗 **Kong Gateway Application Architecture**

### Nginx
- Core engine powering Kong Gateway.
- Handles **routing** and **load balancing**.
- Kong adds management features (auth, security, analytics).
### OpenResty
- Extends Nginx with Lua scripting.
- Allows real-time execution of Lua code for request/response handling.
### Clustering & Datastore
- **Kong Gateway:** optional DB-less mode.
- **Kong Enterprise:** requires Postgres for config + telemetry.
- Database can be:
    - Local (server or container)
    - Cloud Postgres (AWS RDS, GCP SQL, Azure Database)
### Plugins
- Modular extensions for Kong Gateway.
- Written in **Lua, Go, JS, Python**.
- Enable features such as:
    - Authentication
    - Rate limiting
    - Logging
    - External service integration
### RESTful Admin API
- Used to configure and automate Kong Gateway.
- Supports:
    - Adding services & routes
    - Configuring plugins
    - Managing consumers
- Often paired with **decK CLI** to apply YAML declarative configs.

![[kong-gateway-app-architecture.png]]

---
## ⚙️ **Kong Installation Options**

- **Containers:** Docker, Kubernetes, OpenShift
- **Linux Distros:** Amazon Linux, Debian, RHEL, Ubuntu

---
## 🚀 **Deployment Modes**

### Self-Managed
- Full control over config, scaling, security.
- Requires internal resources for ops/maintenance.
### Managed by Konnect
- Control plane hosted by Kong.
- Data planes:
    - Customer-managed, or
    - Fully managed (dedicated cloud gateways).

---
### Traditional Mode (DB-backed)
- Gateway = both control + data plane.
- Config stored in Postgres.
- Suitable for:
    - Plugins requiring DB (OAuth2.0, clustered rate limiting).
- Downsides:
    - Higher security risk (CP+DP combined).
    - Performance impact from Kong Manager / Dev Portal.

![[traditional-mode.png]]

---
### DB-less Mode
- Config stored **in memory** (YAML or JSON file).
- Great for CI/CD automation.
- Advantages:
    - No database dependency
    - Stable + predictable config state
- Limitations:
    - Admin API is read-only
    - Some DB-dependent plugins don’t work

![[db-less-mode.png]]

---
### Hybrid Mode
- **Control Plane** → config & management
- **Data Plane** → traffic proxying only
- Benefits:
    - Separation improves security & reliability
    - Data planes keep running if control plane goes down
    - Lower DB traffic (only CP talks to DB)
    - Easier scaling across data centers/zones

![[hybrid-mode.png]]

---
## 🔄 **API Request & Response Flow**

![[request-flow.png]]

---
## 🧩 **Kong Gateway Components**

- **Client** → Any user/system sending requests.
- **Consumer** → Entity using APIs (user/app).
- **Route** → Defines how requests map to services.
- **Service** → Abstraction of upstream API/microservice.
- **Load Balancer** → Distributes traffic across upstreams.
- **Upstream API** → The actual backend service.
- **Plugins** → Extend Gateway behavior.
    - _Open source plugins_ → Basic auth, traffic shaping.
    - _Enterprise plugins_ → Advanced security, analytics.
- **Admin API** → Config interface for routes, services, plugins.
- **Kong Manager** → UI for Gateway management (open source vs enterprise editions).
- **Vitals** → Real-time analytics & monitoring.
- **Dev Portal** → Central hub for API documentation + testing.

![[gateway-components.png]]

---
## ☁️ **Kong Konnect Components**
- **Konnect Gateway Manager**
    - Enhanced Kong Manager.
    - Adds control plane & data plane management.
- **Konnect Dev Portal**
    - Customizable developer portal.
    - Deployment: hosted by Kong or self-hosted for compliance needs.
- **API Products**
    - Bundle multiple services into versioned products.
    - Manage + publish to Dev Portal.
    - Track usage analytics.
- **Service Hub**
    - Centralized view of services from tools like PagerDuty, GitHub.
- **Konnect Analytics**
    - Tracks request rates, error rates, response times.
    - Helps optimize API operations.

![[kong-konnect-components.png]]

---

## 📝 **Quiz – Introduction Kong Gateway**


>❓**What is the purpose of hybrid mode in the deployment of Kong Gateway?**

- <input type="radio" name="q1"> To enhance data processing speed.
- <input type="radio" name="q1"> To handle control plane configuration more efficiently.
- <input type="radio" name="q1"> To manage data plane traffic more effectively.
- <input type="radio" name="q1"> To separate control and data planes.

<details>
	<summary>💡 Reveal Answer</summary>
	 - To separate control and data planes.
</details>

>❓**What is an API Gateway and what role does it play in the API system?**

- <input type="radio" name="q2"> It is a proxy that allows only certain requests to be fulfilled.
- <input type="radio" name="q2"> It is responsible for managing only community-developed plugins.
- <input type="radio" name="q2"> It is a reverse proxy that manages, configures, and routes API requests.
- <input type="radio" name="q2"> It is responsible for processing API requests and responses proxying only.

<details>
	<summary>💡 Reveal Answer</summary>
	 - It is a reverse proxy that manages, configures, and routes API requests.
</details>

>❓**What is the purpose of Kong Gateway plugins?**

- <input type="radio" name="q3"> To extend the functionality of Kong Gateway.
- <input type="radio" name="q3"> To create APIs from scratch.
- <input type="radio" name="q3"> To manage teams and gateways.
- <input type="radio" name="q3"> To develop Kong Konnect.

<details>
	<summary>💡 Reveal Answer</summary>
	 - To extend the functionality of Kong Gateway.
</details>

>❓**What does Kong Konnect offer?**

- <input type="radio" name="q4"> Comprehensive API lifecycle management.
- <input type="radio" name="q4"> Open-source API management.
- <input type="radio" name="q4"> Microservices transformation.
- <input type="radio" name="q4"> Technical support only.

<details>
	<summary>💡 Reveal Answer</summary>
	 - Comprehensive API lifecycle management.
</details>

>❓**What is one of the purposes of Kong Insomnia?**

- <input type="radio" name="q5"> To enable microservice transformation.
- <input type="radio" name="q5"> To manage and scale APIs.
- <input type="radio" name="q5"> To convert Kubernetes resources into valid Kong Gateway configuration.
- <input type="radio" name="q5"> API Testing

<details>
	<summary>💡 Reveal Answer</summary>
	 - API Testing
</details>

>❓**What is the role of the API Gateway in software architecture?**

- <input type="radio" name="q6"> Reverse proxy.
- <input type="radio" name="q6"> API consumer.
- <input type="radio" name="q6"> REST principles.
- <input type="radio" name="q6"> Data processing.

<details>
	<summary>💡 Reveal Answer</summary>
	 - Reverse proxy.
</details>

>❓**What does RESTful API use for communication?**

- <input type="radio" name="q7"> UDP
- <input type="radio" name="q7"> TCP
- <input type="radio" name="q7"> HTTP
- <input type="radio" name="q7"> FTP

<details>
	<summary>💡 Reveal Answer</summary>
	 - HTTP
</details>

>❓**What are the deployment topologies offered by Kong Gateway?**

- [ ] DB-Less Mode
- [ ] Traditional Mode
- [ ] Hybrid Mode
- [ ] Cloud Mode
- [ ] Kubernetes Mode

<details>
	<summary>💡 Reveal Answer</summary>
	 - DB-Less Mode<br />
	 - Traditional Mode<br />
	 - Hybrid Mode
</details>

>❓**What are the containerized environments Kong can be deployed in?**

- [ ] Docker
- [ ] Kubernetes
- [ ] Apache
- [ ] IIS

<details>
	<summary>💡 Reveal Answer</summary>
	 - Docker<br />
	 - Kubernetes
</details>

>❓**API stands for Application Programming Interface. True or False?**

- <input type="radio" name="q10"> True
- <input type="radio" name="q10"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 - True
</details>

---

>  ⬅️ [Previous: (KGLL-117) Introduction to APIs and API Management](./%28KGLL-117%29%20Introduction%20to%20APIs%20and%20API%20Management.md) | ➡️ [Next: (KGLL-119) Getting Started with Kong Enterprise](./%28KGLL-119%29%20Getting%20Started%20with%20Kong%20Gateway%20Enterprise.md)

---
