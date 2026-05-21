## 🔐 **Securing Kong Gateway Runtime**

Securing the **runtime** breaks into two areas:

- **Securing the Data Plane** → protect the channel between **client ↔ API** (confidentiality, integrity, replay protection).
- **Securing API Services** → control **who** can call **what**, **how often**, and **under which policies**.

---
## 🔑 **Core Controls (at a glance)**

- **Authentication** – prove client identity.
- **Bot Detection** – detect/mitigate automated abuse.
- **Data Encryption** – TLS/mTLS for in-transit protection.
- **IP Restriction** – allow/deny by source IP/CIDR.
- **Threat Protection** – detect malicious or abnormal requests.
- **DDoS Prevention** – preserve availability under floods.
- **Interception Prevention** – prevent sniffing/tampering (TLS, HSTS, mTLS).
- **Rate Limiting** – cap request volume over time windows.

Kong provides these primarily via **plugins**:

![[kong-runtime-sec.png]]

---

## 🚦 **Rate Limiting**
### **What it is**

Control how many requests a client can send per time window (per second/minute/hour/day) to **prevent abuse**, **protect backends**, and **ensure fairness**.

### **Why it matters**

- Mitigates DDoS and brute-force attempts
- Protects sensitive/data-heavy endpoints
- Prevents accidental overuse and cascading failures
- Controls cost on compute-intensive routes

> **DDoS:** a coordinated flood of requests that degrades or knocks out service.

### **How the plugin works**

1. **Request counting** – track calls by `limit_by` (e.g., **consumer**, **credential**, **ip**, **service**, **header**, **path**).
2. **Response headers** – return remaining/limit/reset info.  
    ![[ratelimit-headers.png]]
3. **Threshold enforcement** – exceed limit → **429 Too Many Requests** (sometimes **503**).
4. **Client backoff** – respect `Retry-After`; use **fixed delay** or **exponential backoff**.
#### Counter storage strategies

| Mode        | Strategy           | Pros                     | Cons                                                    |
| ----------- | ------------------ | ------------------------ | ------------------------------------------------------- |
| **Local**   | In-memory per node | Fastest, simplest        | Not globally accurate without consistent hashing        |
| **Cluster** | Kong DB            | Accurate; no extra infra | Highest DB load/latency                                 |
| **Redis**   | External Redis     | Accurate; offloads DB    | Extra component; slower than local, faster than cluster |

#### Implementation notes

- **Choose strategy**: general protection → **local**; strict accuracy → start **cluster**, move **Redis** if DB load is high.
- **DB-less**: **cluster** strategy **not supported**; use **local/Redis**.
- **Hybrid**: data planes support **local/Redis** (no **cluster**).
- **Aggregation keys**: default **consumer**; fallback to **IP** if the chosen key isn’t present.

---

## 🔐 **Authentication (overview)**

Authentication plugins verify callers before your upstream sees traffic. Common options:

- Vault Auth • OIDC • Basic Auth • HMAC • Key Auth • mTLS • JWT • LDAP • OAuth 2.0
### **Key Auth (example)**

- Client presents API key in **header**, **query**, or **body**.
- Upstream can read injected identity headers for finer authorization.
- **Encrypted variant (EE)** stores keys encrypted at rest.

#### **Anonymous mode**

- Configure an **anonymous consumer**: failed auth falls back to that consumer (e.g., lower rate limit).
- If not configured → failed auth returns **403**.

---

## 🔷 **JWT Authentication**

**JWT** = signed JSON claims (usually not encrypted) that a gateway can verify **without** calling an auth server.

**Plugin role**

- Validates signature & claims; allows/denies access.

#### **Benefits**

- Stateless validation; scalable
- Works across multiple apps sharing verification keys

#### **Flow**  
![[jwt-flow.png]]
#### Steps (use):

1. Create **Consumer**
2. Attach **JWT credential(s)** (public/private or shared secret)
3. Issue token (e.g., via `jwt.io`)
4. Send JWT via header/cookie/query
5. Manage keys via Admin API:

```shell
http GET http://{HOST}:8001/consumers/consumerName/jwt
```

---
## 🔒 **mTLS (Mutual TLS)**

**TLS** protects transport; **mTLS** adds **mutual** authentication (client and server present valid certs).

### **Handshake (mTLS)**

1. Client → Server
2. Server sends cert; client verifies
3. Client sends cert; server verifies
4. Encrypted, mutually authenticated channel
#### Trust chain

- Kong must trust the **full chain** (root → intermediate → leaf), not only the CA.
#### mTLS plugin

1. Validate client cert against configured **CA list**
2. Map cert subject to a **Consumer** (optional)
3. Enforce policies (optionally combine with **ACL** for allow/deny groups)
4. Scope: **global**, **service**, or **route**
#### Error responses

- Missing cert → **401 No required TLS certificate was sent**
- Failed verification → **401 TLS certificate failed verification**

#### TLS Flow

![[working-tls.png]]

---

## 🔐 **Secrets Management**

Treat credentials/keys as **secrets** (DB creds, private certs, API keys).  
Store them in a **Vault** and reference them from configs:

```shell
{vault://env/my-secret-redis-password}
```

#### Env example

```shell
export KONG_VAULT_ENV_REDIS_USERNAME=foo
# use in config:
{vault://env/redis/username}
```
#### Supported backends

- **Environment variables** (all editions)
- **AWS Secrets Manager, GCP Secret Manager, Azure Key Vault, HashiCorp Vault** (Enterprise)

> [https://developer.konghq.com/gateway/entities/vault/#supported-vault-backends](https://developer.konghq.com/gateway/entities/vault/#supported-vault-backends)

---

## 🛡 **Hardening the Data Plane (pre-prod checklist)**

- Health checks for data plane nodes
- mTLS for data plane → managed by a cert manager
- Store **encrypted Key-Auth keys** in a **Vault**
- Disable Kong debug headers
- Enable **global observability** plugins
- Enable **global rate-limit** guardrails

---

## 📝 **Quiz - Securing Services on Kong Gateway

>❓ **What is the primary difference between TLS and mTLS in the context of Kong Gateway?**

- <input type="radio" name="q1"> TLS uses symmetric encryption; mTLS uses asymmetric
- <input type="radio" name="q1"> TLS authenticates the client only; mTLS authenticates both client and server
- <input type="radio" name="q1"> mTLS uses cookies for session tracking
- <input type="radio" name="q1"> TLS is used only for admin APIs

<details>
	<summary>💡 Reveal Answer</summary>
	 - TLS authenticates the client only; mTLS authenticates both client and server
</details>

>❓ **When a limit set in Rate Limiting plugin is reached, the plugin returns which status code?**

- <input type="radio" name="q2"> 429
- <input type="radio" name="q2"> 404
- <input type="radio" name="q2"> 401
- <input type="radio" name="q2"> 402

<details>
	<summary>💡 Reveal Answer</summary>
	 - 429
</details>

>❓ **In JWT authentication with Kong, what must each consumer have?**

- <input type="radio" name="q3"> Their own Redis instance
- <input type="radio" name="q3"> A basic-auth password
- <input type="radio" name="q3"> JWT credentials (key and secret)
- <input type="radio" name="q3"> A TLS certificate signed by a public CA

<details>
	<summary>💡 Reveal Answer</summary>
	 - JWT credentials (key and secret)
</details>

>❓ **If the JWT supplied by the request to a service protected by JWT plugin has invalid signature, plugin response code is:**

- <input type="radio" name="q4"> 401 Unauthorized
- <input type="radio" name="q4"> 403 Forbidden
- <input type="radio" name="q4"> 404 Not Found
- <input type="radio" name="q4"> 406 Not Acceptable

<details>
	<summary>💡 Reveal Answer</summary>
	 - 401 Unauthorized
</details>


---

>  ⬅️ [Previous: (KGLL-210) Kong Gateway Plugins](./%28KGLL-210%29%20Kong%20Gateway%20Plugins.md) | ➡️ [Next: (KGLL-206) The OpenID Connect (OIDC) Kong Plugin](./%28KGLL-206%29%20The%20OpenID%20Connect%20%28OIDC%29%20Kong%20Plugin.md)

---
