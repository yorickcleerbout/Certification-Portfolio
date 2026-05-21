# 🔌Introduction to Kong Plugins

A **Kong plugin** is a modular extension that adds extra functionality to the **Kong Gateway**.  
Plugins can **intercept, inspect, and modify** requests and responses as they pass through the Gateway.

They’re built using languages like **Lua**, **Go**, **JavaScript**, or **Python**, leveraging Kong’s **Plugin Development Kit (PDK)**.  
Plugins make it possible to implement advanced features such as:

- 🔐 Authentication & Authorization
- 📊 Logging & Analytics
- ⚙️ Traffic Control
- 🧩 Data Transformation
- 🌩️ Integration with External Services

---
## 🧱 **Plugin Categories**

Kong offers plugins across multiple functional areas:
### 🔐 **Authentication**

Control access to APIs and gateways.  
Examples: **OpenID Connect**, **OAuth2.0**, **Key Authentication**, and more.
### 🛡️ **Security**

Protect APIs against common threats and vulnerabilities.  
Examples: **Bot Detection**, **IP Restriction**, **CORS**.
### 🚦 **Traffic Control**

Manage and regulate API flow.  
Examples: **Rate Limiting**, **Request Size Limiting**, **Access Control Lists (ACLs)**.
### 🧠 **Serverless Integration**

Invoke serverless functions from platforms such as **AWS Lambda**, **Azure Functions**, or **Apache OpenWhisk** in response to gateway events.
### 📈 **Analytics & Monitoring**

Track, analyze, and visualize API traffic and performance.  
Integrates with **Prometheus**, **Datadog**, **Grafana**, etc.
### 🔄 **Transformation**

Modify requests and responses dynamically.  
Examples: change headers, rewrite body data, adjust URL parameters — useful for compatibility, security, or data shaping.
### 🗂 **Logging**

Capture detailed records of API activity.  
Send logs to systems like **Syslog**, **HTTP logging**, **File logging**, or **Kafka**.

![[plugin-categories.png]]

> ✅ Each plugin type can be combined to create a robust and secure API management layer.

---
## ⚙️ **Plugin Tiers**

Kong categorizes plugins into **tiers**, depending on the product edition and deployment type.

| **Tier**           | **Description**                                                    | **Available In**              |
| ------------------ | ------------------------------------------------------------------ | ----------------------------- |
| **Open Source**    | Community-built, free to use                                       | Kong OSS, Enterprise, Konnect |
| **Enterprise**     | Proprietary, advanced plugins (e.g., enhanced security, analytics) | Kong Enterprise               |
| **Paid (Konnect)** | Proprietary plugins offered for a monthly fee                      | Konnect                       |
| **Premium**        | Highest-level enterprise plugins with extended capabilities        | Konnect                       |
![[plugin-tiers.png]]

> 💡 Enterprise and Premium plugins unlock advanced monitoring, security, and analytics features for large-scale deployments.

---
## 🧭 **Plugin Hub**

The **Plugin Hub** is Kong’s central **catalog** of available plugins.  
You can browse by:

- Category (e.g., Traffic Control, Security)
- Type (Community, Enterprise, Konnect)
- Compatibility

![[plugin-hub.png]]
#### 📘 Access it via:

- [Kong Plugin Hub](https://developer.konghq.com/plugins/)
- **Kong Manager UI**
- **Konnect UI**

> 🪄 Tip: The Plugin Hub is your go-to reference when selecting the right plugin for your use case.

---
## ⚙️ **Plugin Execution Order**

In Kong, multiple plugins can run on:
- A **service**, **route**, **consumer**, **consumer group**, or **globally**.

Because multiple plugins might apply to a single request, Kong follows a **static priority system**:
- Plugins execute **from highest priority (largest number)** → **lowest priority**.
- Execution happens **sequentially**.

![[plugin-exec-order.png]]

> 📑 This ensures that critical operations (like authentication) happen before optional tasks (like logging).

---
## 🔁 **API Request Lifecycle**

Every request in Kong Gateway passes through **two main phases**:
### 📨 **1. Request Phase**

1. Kong inspects the **endpoint and headers**.
2. Determines which **route** and **service** should handle the request.
3. Executes **request-phase plugins** in order of priority.

### 📬 **2. Response Phase**

1. The upstream service responds.
2. Kong executes **response-phase plugins** (e.g., transformations, logging).
3. The processed response is sent back to the client.

![[api-request-lifecycle.png]]

> 🔍 Understanding plugin order and request phases is essential for debugging and designing reliable API workflows.

---
## 📝 **Quiz – Introduction to Kong Plugins**

>❓**Plugins in Kong Gateway can only be executed in a static order.**

- <input type="radio" name="q1"> True
- <input type="radio" name="q1"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 - False
</details>

>❓**Are plugins in Kong Gateway only developed using Lua?**

- <input type="radio" name="q2"> True
- <input type="radio" name="q2"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 - False
</details>

>❓**What is the Kong Plugin Development Kit (PDK)?**

- <input type="radio" name="q3"> A set of programming languages used to develop Kong plugins
- <input type="radio" name="q3"> Modular extensions crucial for customizing API requests and response behaviour
- <input type="radio" name="q3"> A set of Lua functions and variables that can be used by plugins to implement their own logic.
- <input type="radio" name="q3"> Kong's proprietary plugin development platform

<details>
	<summary>💡 Reveal Answer</summary>
	 - A set of Lua functions and variables that can be used by plugins to implement their own logic.
</details>

>❓**What is the purpose of WebAssembly in Kong Gateway?**

- <input type="radio" name="q4"> To enhance API security
- <input type="radio" name="q4"> To manage traffic with rate limiting
- <input type="radio" name="q4"> To execute high-performance plugins in various languages that compile to Wasm bytecode
- <input type="radio" name="q4"> To capture API traffic information

<details>
	<summary>💡 Reveal Answer</summary>
	 - To execute high-performance plugins in various languages that compile to Wasm bytecode
</details>

>❓**What are the factors that determine plugin compatibility in Kong Gateway?**

- <input type="radio" name="q5"> Deployment topology, network protocols, and entity scopes
- <input type="radio" name="q5"> Programming languages, databases, and server architecture
- <input type="radio" name="q5"> Entity scope, authentication methods, and encryption algorithms
- <input type="radio" name="q5"> Payment tiers, network protocols, and entity scopes

<details>
	<summary>💡 Reveal Answer</summary>
	 - Deployment topology, network protocols, and entity scopes
</details>

>❓**Kong Gateway offers a versatile range of plugins for API management only in its Enterprise version.**

- <input type="radio" name="q6"> True
- <input type="radio" name="q6"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 - False
</details>

>❓**What type of environment does Wasm offer for executing code in Kong Gateway?**

- <input type="radio" name="q7"> Insecure and untrustworthy
- <input type="radio" name="q7"> A secure, sandboxed environment
- <input type="radio" name="q7"> A distributed networking environment
- <input type="radio" name="q7"> A virtual environment for testing plugins

<details>
	<summary>💡 Reveal Answer</summary>
	 - A secure, sandboxed environment
</details>

>❓**Which category of plugins include Bot Detection and CORS?**

- <input type="radio" name="q8"> Serverless
- <input type="radio" name="q8"> Analytics and Monitoring
- <input type="radio" name="q8"> Traffic Control
- <input type="radio" name="q8"> Security

<details>
	<summary>💡 Reveal Answer</summary>
	 - Security
</details>

---

>  ⬅️ [Previous: (KGLL-115) Getting Started with Kong Konnect Gateway](./%28KGLL-115%29%20Getting%20Started%20with%20Kong%20Konnect%20Gateway.md) 

---
