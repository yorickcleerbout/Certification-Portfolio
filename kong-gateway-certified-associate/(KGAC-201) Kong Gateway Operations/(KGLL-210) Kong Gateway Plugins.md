## 🔌 **What Are Plugins?**

**Kong plugins** are modular components that run inside Kong Gateway to **inspect, enrich, secure, transform, and route** API traffic.

**Why use plugins?**

- **Extend functionality** beyond core gateway features
- **Customize** behavior per service/route/consumer
- **Middleware integration** with external systems
- **Traffic management** (throttling, caching, canary)
- **Logging & monitoring** (observability)
- **Security** (authN/Z, threat protection)

> 📘 Plugin catalog: [https://developer.konghq.com/plugins/](https://developer.konghq.com/plugins/)

---

## 🧭 **Plugin Categories (with common examples)**

### 🔐 **Authentication**

Secure access so only authorized callers reach your APIs.

- Key Authentication
- Basic Authentication
- JWT Authentication
- OAuth 2.0 / OIDC

### 🛡 **Security**

Protect against threats and enforce transport policies.

- CORS
- Bot Detection
- IP Restriction
- TLS Handshake Modifier / TLS Metadata Headers

### 🚦 **Traffic Control**

Shape and optimize traffic for reliability and performance.

- Rate Limiting
- Proxy Caching
- Canary Releases
- Route by Header
- Request Size Limiting

### 📈 **Analytics & Monitoring**

Gain real-time and historical visibility into usage and health.

- Prometheus
- OpenTelemetry
- Datadog

### 🔄 **Transformation**

Adapt requests/responses for clients and backends.

- Request Transformer
- Response Transformer
- `jq`
- Correlation ID

### 🗂 **Logging**

Centralize API logs for troubleshooting and auditing.

- StatsD
- Kafka Log
- TCP/UDP/HTTP/File Log
- Loggly

### ☁️ **Serverless**

Invoke functions as part of the request pipeline.

- AWS Lambda
- Azure Functions
- Apache OpenWhisk

---

## 🧩 **Custom Plugins**

**Custom plugins** let you implement behavior not covered by the catalog.

- Built with the **Plugin Development Kit (PDK)** in **Lua** or **Go**
- Provide SDKs and abstractions to interact with Kong core and datastore
- Typically maintained by the author/team (Community or Third-Party if published on Plugin Hub)

> 📘 Custom plugin docs:  [https://developer.konghq.com/custom-plugins/#custom-plugins](https://developer.konghq.com/custom-plugins/#custom-plugins)

---

## 🏷 **Plugin Categories by Licensing**

| Tier                     | Where it’s available             | Notes                                           |
| ------------------------ | -------------------------------- | ----------------------------------------------- |
| **Free (OSS)**           | Kong OSS / Enterprise / Konnect  | Open-source, community supported                |
| **Premium (Enterprise)** | Kong Enterprise                  | Requires Enterprise license; SLA-backed support |
| **Paid (Add-ons)**       | Konnect tiers or partner add-ons | Separately licensed modules/features            |

---

## ✅ **Compatibility Notes**

Not every plugin runs everywhere (protocols, scopes, or platforms differ).

- Some plugins work at **L7 (HTTP)**, others at **L4 (TCP/UDP)**
- Certain plugins aren’t supported on **Kubernetes CRDs**

> 📘 Compatibility matrix: [https://developer.konghq.com/plugins/compatibility/](https://developer.konghq.com/plugins/compatibility/)


---

## 🧑‍🤝‍🧑 **Who Manages What?**

### Platform-Managed (Global)

Applied by **Platform/DevOps** to enforce org-wide policies:

- Authentication (OIDC/JWT/Key Auth)
- Rate Limiting
- Logging & Monitoring (HTTP Log, Prometheus)

### Developer-Managed (Per Service/Route)

Applied by **app teams** for app-specific behavior:

- Request/Response Transformers
- Caching / Proxy Cache
- A/B Testing / Canary


| Responsibility | Global Plugins            | Per-Service/Route           |
| -------------- | ------------------------- | --------------------------- |
| **Owner**      | Platform/DevOps           | Application Developers      |
| **Examples**   | Auth, Logging, Monitoring | Translation, Caching        |
| **Tooling**    | IaC / CI pipelines        | Admin API, Konnect UI, decK |

---

## 🎯 **Plugin Scope & Precedence**

### Scope

- **Scoped**: attached to a **service**, **route**, **consumer**, or **consumer group**
- **Global**: applies to **all** services/routes/consumers/consumer groups

![[scoped-plugins.png]]

### Precedence (most specific → least specific)

> “Consumer” and “Consumer group” require the request to be authenticated.

1. Consumer **+** Route **+** Service
2. Consumer **group** **+** Route **+** Service
3. Consumer **+** Route
4. Consumer **+** Service
5. Consumer **group** **+** Route
6. Consumer **group** **+** Service
7. Route **+** Service
8. Consumer
9. Consumer **group**
10. Route
11. Service
12. **Global**

> 📘 Reference: [https://developer.konghq.com/gateway/entities/plugin/#precedence](https://developer.konghq.com/gateway/entities/plugin/#precedence)

---

## 🚀 **Deploying Plugins**

### **How to configure**

- **Konnect UI** or **Admin API** (quick, but not easily reproducible/auditable)
- **decK** (recommended): declarative, version-controlled, CI/CD-friendly

### Why decK?

- Configuration as code (YAML)
- Repeatable and auditable changes
- Fits APIOps/GitOps workflows

---

## 🔁 **Dynamic Plugin Ordering**

A single plugin **runs once per request**. The **effective configuration** depends on where it’s attached (per the precedence rules).

**Example**

- Global rate limit for everyone
- Stricter rate limit for a specific **consumer group**  → The **more specific** config wins.

### Limitations & Cautions

- **Consumer-scoped** plugins don’t support dynamic ordering until **after** consumer mapping
- No automatic detection of dependencies on removed plugins
- Sorting at request time adds **latency** (often acceptable if it saves heavier work, e.g., rate limit before expensive auth)
- Validation of dynamic ordering depends on your business logic—**test thoroughly**

> 📘 Docs: [https://developer.konghq.com/gateway/entities/plugin/](https://developer.konghq.com/gateway/entities/plugin/)

---

## 📝 **Quiz - Kong Gateway Plugins


>❓ **Which plugin category includes AWS Lambda and Azure Functions?**

- <input type="radio" name="q1"> Serverless
- <input type="radio" name="q1"> Transformation
- <input type="radio" name="q1"> Monitoring
- <input type="radio" name="q1"> Traffic Control

<details>
	<summary>💡 Reveal Answer</summary>
	 - Serverless
</details>

>❓ **Which of the following Kong Gateway entities can plugins get enabled on directly?**

- <input type="radio" name="q2"> Service, Route, and Consumer
- <input type="radio" name="q2"> Route and Consumer
- <input type="radio" name="q2"> Service and Route
- <input type="radio" name="q2"> Service and Consumer

<details>
	<summary>💡 Reveal Answer</summary>
	 - Service, Route, and Consumer
</details>

>❓ **Which of the following is NOT a category of plugin?**

- <input type="radio" name="q3"> Transformations
- <input type="radio" name="q3"> Logging
- <input type="radio" name="q3"> Troubleshooting
- <input type="radio" name="q3"> Security
- <input type="radio" name="q3"> Traffic Control

<details>
	<summary>💡 Reveal Answer</summary>
	 - Troubleshooting
</details>

>❓ **Are plugins in Kong Gateway only developed using Lua?**

- <input type="radio" name="q4"> False
- <input type="radio" name="q4"> True

<details>
	<summary>💡 Reveal Answer</summary>
	 - False
</details>


---

>  ⬅️ [Previous: (KDLL-202) Administering Kong Gateway with decK](./%28KDLL-202%29%20Administering%20Kong%20Gateway%20with%20decK.md) | ➡️ [Next: (KGLL-205) Securing Services on Kong Gateway](./%28KGLL-205%29%20Securing%20Services%20on%20Kong%20Gateway.md)

---

