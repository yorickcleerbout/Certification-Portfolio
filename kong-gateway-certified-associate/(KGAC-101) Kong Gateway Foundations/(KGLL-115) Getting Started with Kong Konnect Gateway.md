# 🚀 **Getting Started with Kong Konnect**

## 🧭 **What is Kong Konnect?**
Kong Konnect is an **end-to-end SaaS API lifecycle platform** for the cloud-native era.  
Kong hosts the **global control plane** in the cloud, while **Kong Gateway runtimes (data planes)** run in your network (on-prem or cloud).

![[kong-konnect.png]]

### **Platform services (at a glance)**

- **Gateway Manager** → create/control **control planes** and monitor **data planes**
- **Security** → authN/authZ, audit logging, access control
- **Regional operations** → deploy gateways/APIs in multiple regions
- **Mesh Manager** → global managed control plane for service meshes (out of scope here)
- **Kong Ingress Controller** → manage Kubernetes-based Gateway runtimes
- **API Products + Dev Portal** → productize, version, document, and publish APIs
- **Analytics** → prebuilt/custom reports, export to external observability tools
- **Service Hub** _(coming soon)_ → catalog services across gateways, KIC, Mesh, AWS Lambda

> ✅ **Konnect = Cloud control plane + your runtimes** (hybrid by design)

---

## 🧱 **Deployment Types (Konnect → Hybrid Mode)**

Konnect deployments use **Hybrid**: **control plane** manages config; **data planes** proxy traffic.

![[types-deployment-kong-konnect.png]]
### **Control plane options**

- **Hybrid control plane** → Kong-managed **control plane** + **self-managed data planes** (your infra)
- **Dedicated Cloud Gateway control plane** → **control + data planes fully managed** by Kong

---
## 🔌 **Control Plane ↔ Data Plane Communication**

![[c-plane-d-plane-coms.png]]

- Each **data plane** opens **two WebSocket connections** to Konnect:
    1. **Config channel** ← receives configuration
    2. **Analytics channel** → sends aggregated metrics
- **Heartbeat**: DPs ping CP about every **30s** for reachability
- **Resilience**:
    - If Konnect is unavailable, **DPs continue serving** using **locally cached config**
    - On reconnection, **configs + analytics sync** resume automatically
- **Config sources**: Admin API, Konnect UI, or **decK** push → **CP distributes** to all DPs
- **Analytics** include request/response data, latency, upstream info
- 🔐 Ensure **network egress** from DPs to CP endpoints is allowed

> ℹ️ Low-level network details are out of scope for this course.

---
## 🏢 **Multi-Tenancy & Compliance**

Kong Konnect is **multi-tenant** (shared management plane on AWS) with tenant isolation following SaaS best practices.

![[multi-tenant-konnect.png]]

- **Compliance**: SOC 2 Type II, CSA STAR Level 1
- **SLA**: 99.9% uptime for Konnect control plane
- **Traffic continuity**: DPs keep proxying with cached config during control plane downtime

---
## 🌍 **Multi-Geography Support**

Operate control planes and publish APIs in **US, EU, or Australia**.

- **Region-scoped data**: services, routes, consumers, Dev Portal, API Products, teams/roles
- **Global data**: account registration, billing, support (stored in **US**)
- **Latency & compliance**: deploy **closer to users** and **meet data locality** requirements

---
## 🌟 **Benefits of Konnect**

![[benefits-konnect.png]]

- **Global management plane** across clouds, hybrid, and on-prem
- **CI/CD-ready**: automate gateway/APIs with fewer components to own  
    → Kong handles control-plane **provisioning, security, scaling**
- **Rich plugin library** (traffic control, security, serverless, analytics, logging, transforms)
    - **Custom plugins** for enterprise-specific needs
- **Runtime**: lightweight, fast, reliable **Kong Gateway** for proxy
- **Observability**: real-time health/performance across products, routes, apps
- **Security & isolation**:
    - Fine-grained **RBAC** across environments
    - **Runtimes are self-managed** → API traffic **never traverses** Konnect
    - Out-of-the-box **security policies**

> ✅ **Konnect = “less to run, more to ship”**: SaaS control plane + your high-performance runtimes.

---
## 🗂️ **What is Kong Gateway Manager?**

Central place to **catalog, connect, and monitor** all control planes and data planes.

![[kong-gateway-manager.png]]

- **Control planes**: independent config/behavior domains (each org/region gets a **default** CP)
- **Data planes (runtimes)**: your Kong Gateway instances (Docker, Linux, macOS, Windows)
    - No local DB required → **config pushed from CP** and stored **in memory**
- **Install scripts** provided for quick DP bootstrap across platforms

> 🔁 Add more control planes to separate by **environment, region, team**, etc

---
## 📦 **What is an API Product?**

An **API Product** bundles **one or more services** and **at least one version**.  
Each version links to a **Gateway Service** and may include **docs + OpenAPI spec**.

![[api-product.png]]
### **Typical flow**

- **Version 1** → linked to **Production** control plane
- **Version 2** → linked to **Dev/Test** control planes
- After testing, **promote** Version 2 to **Production**

> 🔌 Plugins/config attached to the Gateway Service apply to the **API Product version**.

---
## 📣 **Publish API**

Publishing = **make it discoverable + understandable** via **API Products** and the **Developer Portal**.

- **Product-level docs (Markdown)**: release notes, SLAs, support, business context, workflows
- **Version-level OpenAPI** (formerly Swagger): language-agnostic contract for clients and tools
- **Developer Portal** enables:
    - Browsing available APIs
    - Reading docs + trying endpoints inline
    - Registering apps and requesting access

---

## 🧑‍💻 **Developer Portal**

Customizable portal to **find, learn, test, and consume** your APIs.

![[dev-portal.png]]

- Browse/search docs
- Test endpoints
- Manage credentials
- Supports internal and external audiences

---
## 🔐 **Konnect Authentication (to the Konnect platform)**

- **Basic Auth** (username/password)
- **Social login**: Google, GitHub, Microsoft
- **SSO** via **Okta (OIDC)**

---
## 👥 **Teams & Role-Based Access Control (RBAC)**

![[konnect-rbac.png]]

- **User types**
    - **Platform users** → humans (email-based)
    - **System accounts** → programmatic access
- **Teams/roles**: pick **predefined** roles or **create custom** ones
- **Common roles**
    - **Org Admins** → manage all objects/users/roles
    - **Org Admins (Read-only)** → view only
    - **Portal Admins** → manage Dev Portal content
    - **Control Plane Admins** → manage DPs and runtime config (services, upstreams)
    - **Product Admins** → manage API Products, publishing, app registration
    - **Analytics Admins** → create reports
    - **Analytics Viewers** → view analytics

> 🔒 Use **least privilege**: scope teams/roles to the smallest surface needed.

---
## 📝 **Quiz – Getting Started with Kong Konnect Gateway**

>❓**What are some of the functions of Kong Konnect Teams? (Select all that apply)**

- [ ] Managing access control through predefined or custom roles
- [ ] Deploying a data plane node using an installation script
- [ ] Managing platform users and system accounts
- [ ] Accessing and managing API services

<details>
	<summary>💡 Reveal Answer</summary>
	 - Managing access control through predefined or custom roles<br />
	 - Managing platform users and system accounts<br />
</details>

>❓**Which of the following is not a feature of Kong Konnect's Developer Portal?**

- <input type="radio" name="q2"> API traffic routing
- <input type="radio" name="q2"> API documentation management
- <input type="radio" name="q2"> Testing API endpoints
- <input type="radio" name="q2"> Managing developer credentials

<details>
	<summary>💡 Reveal Answer</summary>
	 - API traffic routing
</details>

>❓**What is the purpose of application registration in Kong Konnect?**

- <input type="radio" name="q3"> To register new users to the Kong Konnect platform
- <input type="radio" name="q3"> To allow developers to register their application for API access
- <input type="radio" name="q3"> To register new plugins to the Kong Gateway
- <input type="radio" name="q3"> To sign up for Kong Konnect services

<details>
	<summary>💡 Reveal Answer</summary>
	 - To allow developers to register their application for API access
</details>

>❓**What is the primary function of Gateway Manager?**

- <input type="radio" name="q4"> Managing organizational security
- <input type="radio" name="q4"> Monitoring API health and performance
- <input type="radio" name="q4"> Managing control and data plane nodes
- <input type="radio" name="q4"> Productizing APIs, including versioning and documentation

<details>
	<summary>💡 Reveal Answer</summary>
	 - Managing control and data plane nodes
</details>

>❓**What is the main difference between Kong-managed and self-managed setups in Konnect?**

- <input type="radio" name="q5"> The type of APIs supported
- <input type="radio" name="q5"> The way data is managed
- <input type="radio" name="q5"> The user interface
- <input type="radio" name="q5"> Who manages the control planes and data planes

<details>
	<summary>💡 Reveal Answer</summary>
	 - Who manages the control planes and data planes
</details>

>❓**Kong Konnect supports which of the following authentication methods for accessing the UI (Select all that apply)**

- [ ] Basic Authentication
- [ ] oAuth2.0
- [ ] OpenID Connect
- [ ] LDAP

<details>
	<summary>💡 Reveal Answer</summary>
	 - Basic Authentication<br />
	 - OpenID Connect
</details>

>❓**What is the uptime SLA commitment in Kong Konnect?**

- <input type="radio" name="q7"> 99.9%
- <input type="radio" name="q7"> 99.5%
- <input type="radio" name="q7"> 98.8%
- <input type="radio" name="q7"> 100%

<details>
	<summary>💡 Reveal Answer</summary>
	 - 99.9%
</details>

>❓**Konnect data planes initiate mTLS connections to Konnect for configuration and analytics**

- <input type="radio" name="q8"> True
- <input type="radio" name="q8"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 - True
</details>

>❓**What is the role of the Gateway Manager component in Konnect?**

- <input type="radio" name="q9"> Manage Control planes and Data plane nodes
- <input type="radio" name="q9"> Manage organizational security
- <input type="radio" name="q9"> Managing Service Meshes
- <input type="radio" name="q9"> Monitor API health and performance
- <input type="radio" name="q9"> Productize APIs including versioning and documentation

<details>
	<summary>💡 Reveal Answer</summary>
	 - Manage Control planes and Data plane nodes
</details>

>❓**What is the default authentication method in Konnect?**

- <input type="radio" name="q10"> Basic Authentication (username and password)
- <input type="radio" name="q10"> OpenID Connect
- <input type="radio" name="q10"> Social Identity sign-in
- <input type="radio" name="q10"> Single sign-on via Okta
- <input type="radio" name="q10"> JWT Authentication

<details>
	<summary>💡 Reveal Answer</summary>
	 - Basic Authentication (username and password)
</details>

---

>  ⬅️ [Previous: (KGLL-119) Getting Started with Kong Enterprise](./%28KGLL-119%29%20Getting%20Started%20with%20Kong%20Gateway%20Enterprise.md) | ➡️ [Next: (KGLL-120) Introduction to Kong Plugins](./%28KGLL-120%29%20Introduction%20to%20Kong%20Plugins.md)

---
