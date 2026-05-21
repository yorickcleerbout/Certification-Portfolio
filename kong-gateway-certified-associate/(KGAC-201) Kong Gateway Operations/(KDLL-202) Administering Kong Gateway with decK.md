## ⚙️ **Overview of decK**

Kong’s **decK** (_Declarative Configuration for Kong_) is a command-line tool used to **manage Kong Gateway configurations declaratively**, bringing automation, version control, and consistency to API operations.

---
## 🧱 **Imperative Configuration (and Its Drawbacks)**

**Imperative configuration** means configuring Kong Gateway **step-by-step** using direct Admin API calls.  Each command makes an immediate change to the system.

### **🔧 Example: Imperative Setup**

```shell
# Create a Service
curl -i -s -X POST https://<admin-api-url>/services \
  --data name='my-example_service' \
  --data url='http://example.org/request'

# Create a Route
curl -i -s -X POST https://<admin-api-url>/routes \
  --data paths[]='/example'

```

### ⚠️ **Drawbacks**

- Each API call modifies a single component — multiple calls are needed to build a complete configuration.
- A single incorrect command can cause misconfiguration or downtime.
- No **version control** or **audit trail** — changes are hard to track or reproduce.
- Difficult to maintain consistency across environments (e.g., dev → prod).

---
## 🧾 **Declarative Configuration Using decK**

Kong provides **decK** to manage configuration in a **declarative** way —  you define your desired state once (in YAML or JSON), and decK applies it to Kong Gateway.

### ✨ **Benefits of decK**

- Manage Kong configurations as **code** (stored in version control).
- Enables **CI/CD** and **APIOps** pipelines for automation and repeatability.
- Guarantees **consistency**, **speed**, and **traceability**.
- Facilitates **rollback** and **change history** through Git.
- Defines _state files_ that fully describe Kong entities — services, routes, plugins, etc.

> 🧩 **Note:**  decK manages **Kong entities**, not **Kong Gateway system settings** (e.g., those in `kong.conf`).

📘 [decK Documentation →](https://developer.konghq.com/deck/)

---
### 🧬 **Entities Managed Through decK**

decK can manage most runtime configuration entities, including:

- Services & Routes
- Consumers & Consumer Groups
- Plugins
- Certificates & CA Certificates
- SNIs
- Upstreams & Targets
- Vaults
- Workspaces
- RBAC roles and endpoint permissions

---
### 🖥️ **decK CLI Command Structure**

decK commands fall into two main categories:
#### 1️⃣ **Gateway State File Management (`deck file …`)**

Used to **manipulate declarative state files**, e.g.:

- Generate YAML configuration files
- Segment or combine configuration files
- Convert industry API specs to Kong configuration
- Validate or lint declarative files

| Example Command       | Description                               |
| --------------------- | ----------------------------------------- |
| `deck file add-tags`  | Add tags to configuration objects         |
| `deck file convert`   | Convert file format                       |
| `deck file kong2kic`  | Generate CRDs for Kong Ingress Controller |
| `deck file lint`      | Validate YAML syntax                      |
| `deck file list-tags` | List all tags in file                     |
#### 2️⃣ **Gateway State Management (`deck gateway …`)**

Used to **interact with a running Kong Gateway instance**, e.g.:

- Synchronize or validate configurations
- Compare current state vs declarative file
- Reset or export configurations

|Example Command|Description|
|---|---|
|`deck gateway diff`|Show differences between Kong and declarative file|
|`deck gateway dump`|Export current configuration|
|`deck gateway sync`|Apply configuration from declarative file|
|`deck gateway validate`|Validate connection and config integrity|
|`deck gateway reset`|Wipe all entities from Kong Gateway

---
## 🧭 **Kong Enterprise Configuration Automation Maturity Levels**

Kong supports several ways to manage Gateway configuration, arranged in **maturity levels** — from manual to fully automated.

![[config-hierarchy.png]]

|Level|Approach|Description|
|---|---|---|
|**Kong Manager**|UI-based|Manual configuration using browser-based interface|
|**Admin API**|Imperative|JSON/HTTP API for direct configuration calls|
|**decK**|Declarative|Manage configuration as YAML; supports CI/CD|
|**APIOps**|Fully automated|End-to-end automation from OpenAPI Spec → Deployment, with embedded checks and best practices

---

## 📝 **Quiz - Administering Kong Gateway with decK

>❓ **Which command would you run to test decK's connectivity with a local Kong Gateway?**

- <input type="radio" name="q1"> deck ping
- <input type="radio" name="q1"> deck status
- <input type="radio" name="q1"> deck check
- <input type="radio" name="q1"> deck localhost:8001

<details>
	<summary>💡 Reveal Answer</summary>
	 -  deck ping
</details>

>❓ **Which command would you run to download a configuration from Kong Gateway?**

- <input type="radio" name="q2"> deck dump
- <input type="radio" name="q2"> deck download
- <input type="radio" name="q2"> deck get config
- <input type="radio" name="q2"> deck sync

<details>
	<summary>💡 Reveal Answer</summary>
	 -  deck dump
</details>

>❓ **Issuing 'http' or 'curl' requests via Admin API to Kong Gateway is an example of Declarative configuration.**

- <input type="radio" name="q3"> True
- <input type="radio" name="q3"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 -  False
</details>

>❓ **Why is declarative configuration preferred over imperative in some environments**

- <input type="radio" name="q4"> It reduces memory usage
- <input type="radio" name="q4"> It bypasses the Admin API
- <input type="radio" name="q4"> It allows for version-controlled, reproducible configurations
- <input type="radio" name="q4"> It doesn't require YAML

<details>
	<summary>💡 Reveal Answer</summary>
	 -  It allows for version-controlled, reproducible configurations
</details>


---

>  ⬅️ [Previous: (KGLL-204) Securing Kong Gateway Control Plane](./%28KGLL-204%29%20Securing%20Kong%20Gateway%20Control%20Plane.md) | ➡️ [Next: (KGLL-210) Kong Gateway Plugins](./%28KGLL-210%29%20Kong%20Gateway%20Plugins.md)

---

