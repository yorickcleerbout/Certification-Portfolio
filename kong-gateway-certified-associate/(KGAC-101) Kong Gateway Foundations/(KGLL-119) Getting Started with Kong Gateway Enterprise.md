# 🚀 **Getting Started with Kong Gateway Enterprise**
## 🏢 **What is Kong Enterprise?**

Kong Enterprise is the **scalable, secure, and flexible API management solution** that extends Kong Gateway — the fastest and most widely adopted API gateway.

### ✨ **It adds:**

- **Enterprise plugins**
- **Advanced security features**
- **Role-Based Access Control (RBAC)**
- **Graphical UI (Kong Manager)**
- **24/7 enterprise support**

![[kong-enterprise.png]]

---
## 🌟 **Benefits of Kong Enterprise**

Kong Enterprise helps organizations **secure, manage, and scale APIs** effectively.
### 🔐 **Security & Access Control**

- Plugins for authentication & authorization (e.g., OpenID Connect)
- IP restriction → allow/deny based on source IP
- Bot detection → block automated traffic
- Rate limiting & quotas → control request volume
- TLS encryption → secure requests/responses
### ⚙️ **Advanced Plugins & Customization**

- Prebuilt plugins for:
    - Traffic control
    - Serverless integration
    - Analytics & monitoring
    - Logging & data transformation
- Support for **custom plugins** (Lua, Go, JS, Python)
### 📈 **Enterprise Support & Integration**

- 24/7 Customer Reliability Engineering (CRE) team
- Smooth integration into CI/CD pipelines → faster onboarding & deployments

![[benefits-kong-enterprise.png]]

---
## 🖥 **Kong Manager**

**Kong Manager** is a **browser-based UI** for managing Kong Gateway.

- Create routes & services
- Configure plugins
- Manage users & teams with RBAC
### 🔌 **Access**:

- HTTP → `:8002`
- HTTPS → `:8445

![[kong-manager.png]]

---
## 📂 **Key Sections in Kong Manager**

### **Workspaces**
- Isolated environments for organizing APIs & services
- Enable tailored configuration per workspace
### **Teams**
- Manage **user access & roles**
- Assign default roles (Super Admin, Admin, Read-Only) or custom ones
- Ensure proper RBAC across workspaces
### **Info**
- View **gateway configuration details**:
	- Database setup
	- Port configuration
	- License info (seats, expiry)

---
## ⚡ **Creating an API with Kong Manager**

 **1. Create a Service**
- Go to **Gateway Services → New Gateway Service**
- Provide:
    - Name
    - URL (protocol + host + port)
- Save

![[new-service.png]]

**2. Create a Route**
- Go to **Routes** or use **Service → Add Route**
- Provide:
    - Name
    - Matching rules (Path, Hosts, Methods, Headers, SNI)
- Example: match by **path** only (disable “strip path”)
- Save

![[new-route.png]]

---
### 🔒 **Securing an API with a Plugin**

We’ll secure the API using the **Key Auth Plugin**.
- Adds **API key authentication** at the **service level**
- All requests must include a valid API key (via header, query param, or body)
- Invalid/missing key → `401 Unauthorized`

![[securing-api.png]]
### 🔧 **Configuring the Plugin**

- Navigate to the service → **Actions → Install Plugin**
- Search for **Key Auth** → Enable
- Confirm plugin is scoped to the service → Save

![[configure-key-auth.png]]

---
### 👤 **Creating a Consumer**

1. Go to **Consumers → New Consumer**
2. Provide username → Save
3. Open the consumer → **Credentials → New Key Auth Credential**
4. Generate and save the API key

![[new-consumer.png]]

---
## 🔗 **Kong Admin API**

The **Admin API** is a RESTful interface for **automating Kong configuration**.
### 🔌 **Access**:

- HTTP → `:8001`
- HTTPS → `:8444`
### ⚙️ **Capabilities**:

- Manage services, routes, consumers
- Configure & update plugins
- Automate deployments with tools like **decK**

> 📖 Docs: [Kong Admin API Reference](https://developer.konghq.com/api/gateway/admin-ee/latest/)

---
## 🔐 **Kong Manager Authentication**

Kong Manager can be secured with:
- **Basic Auth** → username & password
- **OpenID Connect** → external identity provider
- **LDAP** → integrate with enterprise directory

---
## 👥 **Teams & RBAC**

- Two user types:
    - **Admins** → tied to email addresses
    - **RBAC Accounts** → for programmatic/system access
- **Roles** define permissions per workspace:
    - Default roles → Super-Admin, Admin, Read-Only
    - Custom roles → tailored access

> ⚠️ Only **Super Admins** can assign or modify RBAC permissions.

![[teams.png]]

---
## 📝 **Quiz – Getting Started with Kong Gateway Enterprise**

>❓**What is the function of the Admin API in Kong Gateway?**

- <input type="radio" name="q1"> To connect to LDAP for user authentication.
- <input type="radio" name="q1"> To manage global configurations.
- <input type="radio" name="q1"> To facilitate automation and management of the gateway
- <input type="radio" name="q1"> To configure plugins and manage API consumers

<details>
	<summary>💡 Reveal Answer</summary>
	 - To facilitate automation and management of the gateway
</details>

>❓**What determines the level of access for users in Kong?**

- <input type="radio" name="q2"> Roles
- <input type="radio" name="q2"> Teams
- <input type="radio" name="q2"> RBAC accounts
- <input type="radio" name="q2"> Super Admins

<details>
	<summary>💡 Reveal Answer</summary>
	 - Roles
</details>

>❓**Kong Gateway Enterprise is tailored for enterprise needs and includes features like enterprise plugins, advanced security, role-based access control and dedicated support.**

- <input type="radio" name="q3"> True
- <input type="radio" name="q3"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 - True
</details>

>❓**What components can be used to apply configuration to Kong Gateway?**

- [ ] Admin API
- [ ] Kong Manager
- [ ] RBAC
- [ ] Workspaces
- [ ] decK

<details>
	<summary>💡 Reveal Answer</summary>
	 - Admin API<br/>
	 - Kong Manager<br/>
	 - decK
</details>

>❓**What is the purpose of workspaces in Kong?**

- <input type="radio" name="q5"> To accommodate different business divisions such as departments, teams, or environments
- <input type="radio" name="q5"> To monitor and manage Kong Gateway
- <input type="radio" name="q5"> To add or update APIs/services
- <input type="radio" name="q5"> To configure plugins and manage API consumers

<details>
	<summary>💡 Reveal Answer</summary>
	 - To accommodate different business divisions such as departments, teams, or environments
</details>

>❓**What is the difference between Authentication and Authorization?**

- <input type="radio" name="q6"> Authentication involves verifying the identity of a user or device, while authorization determines the rights and privileges of a user, device, or entity to access specific resources or perform certain actions within a system.
- <input type="radio" name="q6"> Authentication involves determining the rights and privileges of a user, device or entity to access specific resources or perform certain actions within a system, while authorization involves verifying the identity of the user or device.
- <input type="radio" name="q6"> Authentication and authorization are the same thing.
- <input type="radio" name="q6"> Neither authentication nor authorization involve security or controlling access to systems and resources.

<details>
	<summary>💡 Reveal Answer</summary>
	 - Authentication involves verifying the identity of a user or device, while authorization determines the rights and privileges of a user, device, or entity to access specific resources or perform certain actions within a system.
</details>

>❓**What ports are used by Kong Gateway for the proxy, Admin API and Kong Manager?**

- <input type="radio" name="q7"> 8000, 8443, 8001, 8444, 8002, and 8445
- <input type="radio" name="q7"> 9000,  9443, 9001, 9444, 9002, and 9445
- <input type="radio" name="q7"> 7000, 7443, 7001, 7444, 7002, and 7445
- <input type="radio" name="q7"> 6000, 6443, 6001, 6444, 6002, and 6445

<details>
	<summary>💡 Reveal Answer</summary>
	 - 8000, 8443, 8001, 8444, 8002, and 8445
</details>

>❓**What functions does Kong Manager offer?**

- [ ] Creating routes and services
- [ ] Configuring plugins
- [ ] Monitoring the server
- [ ] Managing user groups through RBAC

<details>
	<summary>💡 Reveal Answer</summary>
	 - Creating routes and services<br />
	 - Configuring plugins<br />
	 - Managing user groups through RBAC
</details>

---

>  ⬅️ [Previous: (KGLL-118) Introduction to Kong Gateway](./%28KGLL-118%29%20Introduction%20to%20Kong%20Gateway.md) | ➡️ [Next: (KGLL-115) Getting Started with Kong Konnect Gateway](./%28KGLL-115%29%20Getting%20Started%20with%20Kong%20Konnect%20Gateway.md)

---
