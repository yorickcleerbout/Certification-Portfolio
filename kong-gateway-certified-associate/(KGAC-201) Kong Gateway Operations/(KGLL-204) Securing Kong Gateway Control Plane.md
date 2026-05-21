## 🔒 **Securing the Kong Gateway Control Plane**

Once **Kong Gateway** is installed, access to its **Control Plane components** must be properly secured.  
Kong provides tools to manage **who can access** these components and **what level of access** they are granted — following the two core security principles:

> **Authentication** and **Authorization**

---
## 🧭 **Control Plane Access**

Kong offers built-in access control for:

- **Kong Manager (UI)**
- **Kong Admin API**

These two components are the primary entry points into the control plane configuration.

---
## **🧱 Authentication vs. Authorization**

Although they sound similar, these are two **distinct** security mechanisms:

| Concept            | Purpose                            | Example                |
| ------------------ | ---------------------------------- | ---------------------- |
| **Authentication** | Verifies _who_ the user is         | Password, MFA, SSO     |
| **Authorization**  | Defines _what_ the user can access | Role-based permissions |

### 🔐 **Authentication Methods**

Kong supports several authentication techniques:

- Password-based authentication
- SMS / Email / Authenticator app
- **Single Sign-On (SSO)**
- **Multi-Factor Authentication (MFA)**
- **Biometric** authentication

### **🧾 Authorization Techniques**

The most common authorization model in Kong is:

- **Role-Based Access Control (RBAC)**

 > Users are granted permissions based on their roles within an organization.

---

## 🧩 **Authorization in the Kong Control Plane**

### **Kong Control Plane Entities**

The control plane defines configuration “entities” such as:
- Routes
- Services
- Consumers
- Upstreams  
- etc.

Each of these can be managed through **Kong Manager** or the **Admin API**.

> Kong RBAC allows **granular access** at the entity level — e.g.,  
> a user may have full CRUD access to some routes and read-only access to others.

---
### 🏗 **RBAC Authorization Model**

RBAC defines access in three hierarchical layers:

1. **Workspace** → Logical grouping of entities per team
2. **Roles** → Define permissions within a workspace
3. **Permissions** → Define CRUD-level actions on entities

> Example:  
> Tom (Payments team) → CRUD access in _Payments Workspace_,  
> but _read-only_ access in _Bookings Workspace_.

![[rbac.png]]

---

### **👥 Control Plane User Types**
| User Type       | Access Method                  | Description                                                    |
| --------------- | ------------------------------ | -------------------------------------------------------------- |
| **Admins**      | Kong Manager (UI) or Admin API | Authenticated with username/password and RBAC token            |
| **RBAC Users**  | Admin API only                 | Access via token, commonly for CI/CD                           |
| **Super Admin** | Full system access             | Created during installation; manages all users and permissions |

---
### 🧰 **Kong Gateway Roles**

When a new workspace is created, Kong automatically provisions default roles:

| Role                      | Scope     | Description                                            |
| ------------------------- | --------- | ------------------------------------------------------ |
| **Admin (default)**       | Global    | Full access to all endpoints except RBAC Admin API     |
| **super-admin**           | Global    | Full unrestricted access                               |
| **read-only**             | Global    | Read-only across all workspaces                        |
| **workspace-admin**       | Workspace | Full access within a workspace (except RBAC Admin API) |
| **workspace-read-only**   | Workspace | Read-only access within workspace                      |
| **workspace-super-admin** | Workspace | Full access, including RBAC management                 |

---

### 🧩 **Permissions**

Each role consists of one or more **permissions** that define CRUD-level access.

![[permissions.png]]

> Permissions follow the **principle of least privilege** — users get only the access they need.

---
### 🧠 **Permission Flow**

Kong applies permissions in a **hierarchical precedence model**, from most specific to least specific:

1. Rule against the current endpoint in the current workspace
2. Rule against the current endpoint in _any_ workspace `(*)`
3. Rule against _any_ endpoint in the current workspace
4. Rule against _any_ endpoint in _any_ workspace `(*)`

![[permission-flow.png]]

> 🔸 By default, roles deny all actions (least privilege).  
> 🔸 Higher roles may inherit permissions from lower-level ones.

---
## 🔑 **Authentication in the Control Plane**
### **Enabling Authentication**

To enable authentication, set the following configuration values in `/etc/kong/kong.conf`:

```conf
enforce_rbac = on
admin_gui_auth = basic-auth  # or oidc, ldap
admin_gui_session_conf = { "secret": "set-your-string-here" }
```

> ⚠️ `enforce_rbac` must be `on` before enabling `admin_gui_auth`.

Additional optional parameters:
- `admin_gui_auth_password_complexity`
- `admin_gui_auth_header`
- etc.

📘 [Kong Gateway Configuration Docs](https://developer.konghq.com/gateway/configuration/#admin_gui_auth)

---

### 🍪 **Session Cookies**

Session cookies maintain authenticated user sessions for Kong Manager.

1. User logs in → Session cookie created
2. Cookie used for subsequent authenticated requests
3. Session auto-renews periodically and expires after timeout

> Helps prevent stolen or replayed cookies from being reused after expiry.

📘 [Mozilla Docs on Cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Cookies#Secure_and_HttpOnly_cookies)

---
## 🛡️ **Hardening the Kong Control Plane**

Apply these security measures to strengthen your deployment:

| Category                    | Examples                                                          |
| --------------------------- | ----------------------------------------------------------------- |
| **RBAC & Access Control**   | Principle of least privilege, Workspaces isolation, Enable RBAC   |
| **Network Security**        | Restrict Admin API & Kong Manager by IP/firewall, Use mTLS        |
| **Data Protection**         | Use TLS 1.2+, Encrypt DB connections, Disable HTTP                |
| **Secrets Management**      | Store secrets in Vault/K8s, not in plain text                     |
| **System Hardening**        | Disable untrusted Lua, Disable fingerprinting, Audit logs enabled |
| **Compliance & Monitoring** | Send logs to SIEM, Enable health checks, Use LTS versions         |
📘 [Hardening Reference](https://developer.konghq.com/gateway/secrets-management/#referenceable-plugin-fields)

---
## 📝 **Quiz - Securing Kong Gateway Control Plane

>❓ **RBAC is used to secure access to:**

- [ ] Admin API
- [ ] Services
- [ ] Routes
- [ ] Kong Manager

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Admin API<br />
	 - Kong Manager
</details>

>❓ **What is true about the super-admin role in Kong Gateway?**

- <input type="radio" name="q1"> It has full access to all endpoints across all workspaces
- <input type="radio" name="q1"> It can only read configuration
- <input type="radio" name="q1"> It is limited to UI access only
- <input type="radio" name="q1"> It has access only to the default workspace

<details>
	<summary>💡 Reveal Answer</summary>
	 -  It has full access to all endpoints across all workspaces
</details>

>❓ **Common authentication techniques include:**

- [ ] Certificate-based authentication
- [ ] Token-based authentication
- [ ] Bionomic authentication
- [ ] Two-factor (2FA) authentication

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Certificate-based authentication<br />
	 - Token-based authentication<br />
	 - Two-factor (2FA) authentication
</details>

>❓ **One of the recommended control plane hardening practices is to use TLS for DB connections.**

- <input type="radio" name="q2"> True
- <input type="radio" name="q2"> False

<details>
	<summary>💡 Reveal Answer</summary>
	 -  True
</details>

---

>  ⬅️ [Previous: (KGLL-203) Upgrading Kong Gateway](./%28KGLL-203%29%20Upgrading%20Kong%20Gateway.md) | ➡️ [Next: (KDLL-202) Administering Kong Gateway with decK](./%28KDLL-202%29%20Administering%20Kong%20Gateway%20with%20decK.md)

---

