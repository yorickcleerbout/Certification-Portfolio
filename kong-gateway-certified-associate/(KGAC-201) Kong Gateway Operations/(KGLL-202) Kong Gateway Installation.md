## ⚙️ **Installation Overview**

Kong Gateway Enterprise is a **high-performance**, **low-resource** API Gateway that can run anywhere — on-premises, in the cloud, or in Kubernetes and serverless environments.  
It’s flexible, lightweight, and supports multiple deployment patterns.
### 💡 **Key Facts**

- Works across any environment or platform
- Can be installed **with or without a database**
- Supports **hybrid** and **cloud-hosted** modes

---
## **🧠 Refresh Your Memory** 

Before diving into installation, let’s quickly revisit **Kong Gateway basics**.

>❓ **Check all the correct statements about API Gateway**

- [ ] Reverse proxy that accepts client API calls and forwards them to the appropriate backend service
- [ ] Provides flexibility to implement security and authentication, rate limiting, monetization, logging, etc, all in one place
- [ ] Can perform API Request and Response transformation and validation
- [ ] Can support different API formats like Rest API, gRPC, SOAP, GraphQL

<details>
	<summary>💡 Reveal Answer</summary>
	 - Reverse proxy that accepts client API calls and forwards them to the appropriate backend service <br />
	 - Can support different API formats like Rest API, gRPC, SOAP, GraphQL
</details>


>❓ **Match each Kong product to the correct API lifecycle phase:**

Kong Ingress Controller |  Kong Mesh | Kong Enterprise | Kong Gateway | Insomnia |  Kong Konnect

| **RUN** | **Build & Test** | **Govern & Secure** |
| :-----: | :--------------: | :-----------------: |
|         |                  |                     |
|         |                  |                     |
|         |                  |                     |
<details>
	<summary>💡 Reveal Answer</summary>
	 <br />
	 <table>
		 <tr><th><b>RUN</b></th><th><b>Build & Test</b></th><th><b>Govern & Secure</b></th></tr>
		 <tr><td>Kong Ingress Controller</td><td>Insomnia</td><td>Kong Enterprise</td></tr>
		 <tr><td>Kong Mesh</td><td></td><td>Kong Konnect</td></tr>
		 <tr><td>Kong Gateway</td><td></td><td></td></tr>
	 </table>
</details>

>❓ **Match the following definitions with Kong Gateway application modules**

![[definitions-kong-gateway-modules.png]]

---
## 🧩 **Component Parts - What Needs to Be Installed?**

Kong Gateway Enterprise typically consists of **three key components**:

| Component        | Description                                                      |
| ---------------- | ---------------------------------------------------------------- |
| **Kong Gateway** | Core engine that brokers API requests across upstream services   |
| **Kong Manager** | Web UI for monitoring and managing the gateway                   |
| **PostgreSQL**   | Database for storing routes, services, and plugin configurations |
> ⚠️ _Kong Manager and Kong Gateway are part of the same binary._

---
## 🌍 **Where Can I Install Kong Gateway?**

Kong Gateway is **lightweight and high-performing**, and supports multiple environments.
### 🐳 **Containerized Environments**

- Supports **Docker**, **Kubernetes**, and **OpenShift**
- Offers two container image types:
    - **UBI-based** → Red Hat Enterprise Linux
    - **Slim-based** → Debian Slim
- Both are security-scanned and dependency-minimal
- FIPS-140-2 compliant images available  
    👉 [Learn more](https://developer.konghq.com/gateway/fips-support/)
- Can be installed on **Kubernetes** using **Helm Charts** or **Kong Ingress Controller**

### 💻 **Operating Systems**

- AWS Linux
- Debian
- Red Hat Enterprise Linux (RHEL)
- Ubuntu

---
## 🧠 **Kong Gateway Deployment Topologies**

Kong supports several **deployment models** for different needs.  
This section focuses on **non-Ingress** deployments.

### 🏗️ **Traditional Mode**

![[TraditionalDeployment.png]]

- Backed by a **database** storing all configuration
- Supports **single** or **multi-node clusters**
- Plugins and custom extensions can read/write to the database
    
> 🧩 Best for persistent environments with database-backed plugins.

### **🧾 DB-less Mode**

![[DBLess.png]]

- Runs **without a database**; uses **in-memory** configuration only
- Entities are defined **declaratively** in YAML or JSON config files
#### ⚠️ **Plugin Limitations**

- Plugins that require centralized DB coordination are **not supported**
- Some plugins work **partially** using static credentials in the config file

> ✅ Ideal for **CI/CD automation**, **lightweight environments**, and **stateless deployments**.

### **🔀 Hybrid Mode**

![[HybridDeployment.png]]

Kong Gateway nodes are split into two roles:

| **Role**               | **Responsibility**                                        |
| ---------------------- | --------------------------------------------------------- |
| **Control Plane (CP)** | Hosts the Admin API & Kong Manager; manages configuration |
| **Data Plane (DP)**    | Handles live traffic and proxies API requests             |
#### 🧩 **Key Characteristics**

- DPs **pull configuration** from CPs in real-time
- **Many DPs** can connect to a **single CP**
- DPs scale **horizontally**
- CP provides configuration to all DPs using the **hybrid protocol**

> 💡 Combines the best of both worlds: DB-backed management with stateless runtime performance.

### **☁️ Kong Konnect (Cloud SaaS)**

![[KongKonnectDeployment.png]]

Kong Konnect provides **cloud-hosted control planes** and **data planes**, available as:

- **Self-managed DPs** → run on-prem or in your cloud
- **Dedicated Cloud Gateways** → fully managed by Kong

> 🔐 Simplifies management while maintaining flexibility in deployment location.

### **🧭 Kong Ingress Controller (KIC)**

![[kic.png]]

- Runs Kong Gateway as a **Kubernetes Ingress**
- Accepts external traffic and load balances it to in-cluster services
- Converts Kubernetes CRDs → Kong configuration

> ⚠️ KIC architecture is **not covered** in this course.

---
## 📚 **Summary**

|**Deployment Type**|**Database**|**Use Case**|
|---|---|---|
|Traditional|✅ Required|Persistent config with complex plugin needs|
|DB-less|❌ Not required|Declarative config, automation-friendly|
|Hybrid|⚙️ Optional|Separate control & data planes for scaling|
|Konnect|☁️ SaaS-managed|Simplified API management via Konnect Cloud|
|KIC|☸️ Kubernetes|Gateway as K8s ingress for cluster traffic|

---
## 📝 **Test your knowledge**


>❓ **Kong Gateway can be installed on which of following Operating Systems?**

- [ ] RedHat
- [ ] Debian
- [ ] Windows
- [ ] Ubuntu

<details>
	<summary>💡 Reveal Answer</summary>
	 - RedHat<br />
	 - Debian<br />
	 - Ubuntu<br />
	 <br />
	 <i>Kong Gateway supports direct installation of Kong Gateway on RedHat, Debian and Ubuntu OS. However, Kong Gateway can be installed in Windows using Docker images</i>
</details>

>❓ **Check the statements that is correct about Control Plane and Data Plane**

- [ ] Control Plane is used to manage the configuration for Kong Gateway and passes on configuration details to Data plane at regular intervals
- [ ] Data Plane is used as reverse proxy to manage API traffic
- [ ] Rate limiting, Security and Authentication is configured in Control Plane
- [ ] Admin API is served from control plane

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Control Plane is used to manage the configuration for Kong Gateway and passes on configuration details to Data plane at regular intervals<br />
	 - Data Plane is used as reverse proxy to manage API traffic<br />
	 - Rate limiting, Security and Authentication is configured in Control Plane<br />
	 - Admin API is served from control plane
</details>

>❓ **Match the deployment topologies with appropriate features**

![[match-topologies.png]]

>❓ **Can we configure Kong Gateway to use any other DB, apart from Postgres, to store config related details?**

- <input type="radio" name="q1"> Yes
- <input type="radio" name="q1"> No

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Yes
</details>

---

## ⚙️ **Deployment Considerations**

Before installing **Kong Gateway Enterprise**, there are several **key planning factors** to ensure optimal performance, reliability, and security.

### **🧮 Resource Sizing Guidelines**

Proper sizing ensures that Kong Gateway can handle your expected API traffic efficiently.
#### Core Dimensions to Evaluate
- **Bandwidth** → Estimate total data throughput requirements.
- **Performance Metrics**
	-  **Latency** → Delay between request and response.
	- **Throughput** → Number of requests handled per second.
- **CPU & RAM** → Allocate enough compute for plugin execution and traffic load.
- **Database Resources** → Optimize Postgres (or chosen DB) for concurrent connections.
- **Scaling Dimensions** → Plan horizontal scaling for data plane nodes.
- **Memory Cache Size** → Tune caching for better performance and lower latency.

>📘 Reference: [Kong Resource Sizing Guidelines](https://developer.konghq.com/gateway/resource-sizing-guidelines/)

### **🔌 Default Ports**

Each Kong component listens on specific default ports.  
These may vary between HTTP and HTTPS configurations.

|**Component**|**HTTP Port**|**HTTPS Port**|**Description**|
|---|---|---|---|
|**Proxy**|`8000`|`8443`|Handles client API requests|
|**Admin API**|`8001`|`8444`|Configures and manages Kong|
|**Kong Manager (GUI)**|`8002`|`8445`|Web-based management interface|
|**Dev Portal**|`8003`|`8446`|Developer-facing portal|
> 🔗 [Network Configuration Reference](https://developer.konghq.com/gateway/network/)

### 🌐 **DNS Considerations**

DNS setup ensures that Kong components are **accessible and resolvable** from clients and internal systems.
#### Key Hostnames
- **Kong Manager & Admin API**
- **Portal API & Developer Portal**
#### Additional Concerns
- **CORS (Cross-Origin Resource Sharing)** → Configure for secure browser interactions.
- **Cookie Management** → Define cookie behavior for session-based authentication.

> 📘 Reference: [DNS Configuration Guide](https://developer.konghq.com/gateway/network/dns-config-reference/)

### 🔒 **Network & Firewall Settings**

Network policies directly affect Kong’s reachability and data flow.
#### Key Areas
- **Firewall Configuration** → Allow access to proxy, Admin API, and Manager ports.
- **Transparent Proxying** → Enable traffic inspection and routing without client reconfiguration.
- **TCP/TLS Stream Proxying** → Handle raw TCP or encrypted traffic streams.
- **Client Port Access** → Ensure all necessary ports are open between clients and services.

> 🔗 [Network Setup Reference](https://developer.konghq.com/gateway/network/)

### 🧑‍💻 **Security & Certificates**

Kong Gateway offers extensive features for **secure deployments**.
#### Security Best Practices
- **Secure the Admin API** (restrict access & enable HTTPS)
- **API Loopback Protection** (prevent self-calls)
- **Data Encryption/Decryption** for sensitive configuration fields
- **RSA Key Pair Management** (used for signed tokens or mTLS)
- **Certificate Management** (for SSL/TLS)
- **Secrets Management** → integrate with **HashiCorp Vault** or other secure stores
- **RBAC Enforcement** (`enforce_rbac` flag in configuration)

> 📘 Reference: [Kong Secrets Management Guide](https://developer.konghq.com/gateway/secrets-management/#referenceable-plugin-fields)

### 📝 **Licensing**

A valid license is required to enable **Enterprise** features.
#### Licensing Steps
- **Deploy the license file** → upload or mount it as part of your Kong configuration.
- **Monitor expiration dates** → plan renewals in advance to avoid downtime.

> 🔗 [License Deployment Guide](https://developer.konghq.com/gateway/entities/license/#deploying-the-license-file)

---

## 📝 **Test your knowledge**

>❓ **Match the factor to the correct guideline.**

![[match-correct-guideline-deploy-considerations.png]]

>❓ **Which configuration parameter is essential for securing the Admin API in Kong Gateway?**

- <input type="radio" name="q2"> enforce_rbac
- <input type="radio" name="q2"> db_cache_ttl
- <input type="radio" name="q2"> dns_resolver
- <input type="radio" name="q2"> proxy_listen

<details>
	<summary>💡 Reveal Answer</summary>
	 -  enforce_rbac
</details>

>❓ **Transparent proxying is not an important factor that needs to be considered before installing Kong Gateway.**

- <input type="radio" name="q3"> True
- <input type="radio" name="q3"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 -  False
</details>

---

## **⚙️ Kong Gateway Configuration Parameters**

Kong Gateway offers a large number of **configuration parameters** across both **Control Plane** and **Data Plane** nodes.  
These parameters are grouped into several broad categories, depending on their purpose and deployment type.

### 🧩 **Configuration Categories**
#### ⚙️ General

Used to define the **core behavior** of the gateway.

Examples:
- Log levels (`debug`, `info`, `warn`, `error`)
- List of plugins to load
- System paths and general runtime parameters

#### 🔀 Hybrid Mode (Control Plane / Data Plane)

Defines how Control and Data Plane nodes interact.

Examples:
- Node **roles** (`control_plane` / `data_plane`)
- mTLS **certificates** for secure CP–DP communication
- `cluster_cert` and `cluster_cert_key` configuration
- Synchronization and clustering parameters

#### 🌐 NGINX Settings

Low-level NGINX parameters that influence performance and security.

Examples:
- Supported **SSL/TLS ciphers** and protocols
- **TCP timeouts**
- **HTTP directories** and buffer sizes
- Worker connections and keep-alive tuning

#### 🗃️ Database Settings

Control how Kong interacts with its datastore.

Examples:
- Entity **TTLs** (Time-to-Live)
- **Caching** and connection pooling
- Database connection credentials and replication settings

#### 🌍 DNS Settings

Handle upstream service name resolution.

Examples:
- DNS **cache TTL** values
- DNS **caching behavior** and refresh intervals
- Custom DNS resolver configuration

#### 🧭 Kong Manager

Parameters specific to the web-based management UI.

Examples:
- **URLs** and base API endpoints
- **Ports** (HTTP/HTTPS)
- UI access configuration and session handling

---

###  📁 **Configuration File Locations**

Configuration parameters are set differently depending on the **deployment environment**.

| **Deployment Type** | **Configuration File**  |
| ------------------- | ----------------------- |
| **Bare Metal / VM** | `/etc/kong/kong.conf`   |
| **Docker**          | `docker-compose.yaml`   |
| **Kubernetes**      | Helm `values.yaml` file |
> 💡 Tip: Keep configuration files under version control for reproducibility and auditability.

---

### **🌱 Environment Variables**

You can also set or override **configuration parameters** via **environment variables**.  
This is especially common in **containerized** or **CI/CD** deployments.

#### ✅ Convention
- All environment variable names are **capitalized**
- Prefixed with **`KONG_`**

| **Config Property** | **Environment Variable** |
| ------------------- | ------------------------ |
| `proxy_error_log`   | `KONG_PROXY_ERROR_LOG`   |
| `admin_gui_api_url` | `KONG_ADMIN_GUI_API_URL` |
Example (Docker Compose):
```yaml
environment:
  - KONG_DATABASE=postgres
  - KONG_PG_HOST=db
  - KONG_ADMIN_GUI_API_URL=http://localhost:8001
```

> ⚙️ Using environment variables simplifies automation and aligns with container best practices.

---

## 📝 **Test your knowledge**

>❓ **_Drag and drop the configuration setting parameter into the correct category_**

_(__To attempt this activity__,_ _refer to the [documentation](https://developer.konghq.com/gateway/configuration/)__)_

Vaults | Headers | nginx_user | cluster_cert_key | ssl_ciphers | Caching | cluster_listen | cluster_cert | cluster_mtls | nginx_worker_processes | log_level | error_template_json | TTLs 

| **General Settings** | **NGINX Settings** | **Database Settings** | **Hybrid Mode DP/CP Settings** |
| :------------------: | :----------------: | :-------------------: | ------------------------------ |
|                      |                    |                       |                                |
|                      |                    |                       |                                |
|                      |                    |                       |                                |
|                      |                    |                       |                                |


<details>
	<summary>💡 Reveal Answer</summary>
	 <br />
	 <table>
		 <tr><th>General Settings</th><th>NGINX Settings</th><th>Database Settings</th><th>Hybrid Mode DP/CP Settings</th></tr>
		 <tr><td>Vaults</td><td>Headers</td><td>Caching</td><td>cluster_cert_key</td></tr>
		 <tr><td>log_level</td><td>nginx_user</td><td>TTLs</td><td>cluster_listen</td></tr>
		 <tr><td>error_template_json</td><td>ssl_ciphers</td><td></td><td>cluster_cert</td></tr>
		 <tr><td></td><td>nginx_worker_processes</td><td></td><td>cluster_mtls</td></tr>
		 </table>
</details>

---

## 📝 **Quiz - Kong Gateway Installation**

>❓ **Which DBs are supported by Kong as a datastore?**

- <input type="radio" name="q4"> Oracle
- <input type="radio" name="q4"> MongoDB
- <input type="radio" name="q4"> MySQL
- <input type="radio" name="q4"> Postgress

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Postgress
</details>

>❓ **What are some factors to consider before installing the Kong Gateway? (Select Multiple)**

- [ ] Licensing
- [ ] Network and Firewall
- [ ] Client OS
- [ ] Default Ports

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Licensing<br />
	 - Network and Firewall<br />
	 - Default Ports
</details>

>❓ **Which deployment modes are recommended for production environments? (Select Multiple)**

- [ ] DB-less
- [ ] Embedded
- [ ] Hybrid
- [ ] Konnect

<details>
	<summary>💡 Reveal Answer</summary>
	 -  DB-less<br />
	 - Hybrid<br />
	 - Konnect
</details>

>❓ **Which deployment mode is recommended for Multi-Cloud environments?**

- <input type="radio" name="q5"> DB-less
- <input type="radio" name="q5"> Embedded
- <input type="radio" name="q5"> Hybrid
- <input type="radio" name="q5"> Distributed

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Hybrid
</details>

---

>  ⬅️ [Previous: (KGAC-201) Kong Gateway Operations](Certification-Portfolio/Kong%20Gateway%20Certified%20Associate/(KGAC-201)%20Kong%20Gateway%20Operations/README.md) | ➡️ [Next: (KGLL-203) Upgrading Kong Gateway](./%28KGLL-203%29%20Upgrading%20Kong%20Gateway.md)

---
