## **🪪 OpenID Connect (OIDC)**

**OpenID Connect (OIDC)** is an **identity layer** built on top of **OAuth 2.0**.  
It allows clients to **verify a user’s identity** and **retrieve basic profile information** from an **authorization server (IdP)**.

>🔐 **OAuth 2.0** = Authorization 
>🪪 **OIDC** = Authentication **+** Authorization

---

## 🧩 **OAuth 2.0 Overview**

The **OAuth 2.0 IETF standard** lets an application securely access protected resources on behalf of a user — _without sharing passwords_.

### 🔑 **Key Terminology**

| Term                     | Definition                                                                          |
| ------------------------ | ----------------------------------------------------------------------------------- |
| **Client**               | The app requesting access to a protected resource (e.g., web or mobile app).        |
| **Resource Server**      | The API that holds protected data. Enforces access using tokens.                    |
| **Access Token**         | A credential proving the client is authorized to access a resource.                 |
| **Authorization Server** | Authenticates users and issues tokens (usually part of an IdP).                     |
| **Resource Owner**       | The user granting permission to the client.                                         |
| **Authorization Code**   | A temporary code exchanged for an access token.                                     |
| **Grant Type**           | The method used to obtain the token (e.g., Authorization Code, Client Credentials). |

---

## 🪪 **OpenID Connect Terminology**

| Term                     | Description                                                    | Example                                             |
| ------------------------ | -------------------------------------------------------------- | --------------------------------------------------- |
| **End-User**             | The human being who authenticates.                             | You logging into a website with Google.             |
| **Relying Party (RP)**   | The application that relies on the IdP for authentication.     | `myapp.com` using “Log in with Google”.             |
| **OpenID Provider (OP)** | The identity provider that authenticates and issues ID tokens. | Google, Okta, Auth0, Azure AD, Keycloak.            |
| **ID Token**             | A signed JWT containing identity info.                         | `{ "sub": "jane@example.com", "name": "Jane Doe" }` |

---

## 🧠 **How OIDC Works with OAuth 2.0**

OIDC **extends OAuth 2.0** by adding an identity layer to the authorization framework.

- **OAuth 2.0** → grants _access_ to resources
- **OIDC** → verifies _who_ the user is, in addition to granting access

> OAuth 2.0:  "Can this app access data?"
> OIDC:       "Who is this user, and can the app access data for them?"

---

## 🏢 **What Is an Identity Provider (IdP)?**

An **Identity Provider** manages and verifies digital identities.  
It authenticates users and issues tokens to applications (Relying Parties).

Common IdPs:

- Google
- Azure AD
- Okta
- Auth0
- Keycloak

They may use standards such as **SAML**, **OAuth 2.0**, or **OIDC**.

---

## 🔄 **Simplified OIDC Flow Example**

**Scenario:**  
A user logs in to **MyShop.com** using Google as the identity provider.

![[oidc-flow.png]]

### Step-by-step

1. **User initiates login** → clicks “Log in with Google.”
2. **Redirect to Google’s Auth Server** with:
    - Redirect URI (`myshop.com/callback`)
    - Response type (`code`)
    - Scope (`openid profile`)
3. **User authenticates** → enters credentials at Google.
4. **Consent** → user grants access (“Allow MyShop to access your profile”).
5. **Auth Code returned** → sent to `myshop.com/callback`.
6. **Token exchange** → MyShop requests an **Access Token** and **ID Token** from Google.
7. **Access resource** → MyShop calls `/userinfo` to display “Hello, Joe!”

---
## 🧩 **Kong OpenID Connect (OIDC) Plugin**

Kong’s **OIDC Plugin** (Enterprise-only) integrates Kong with **OpenID Connect-compliant IdPs** like Okta, Auth0, Keycloak, and Azure AD.

### What It Does

- Authenticate API clients via an IdP
- Exchange codes for tokens (access/ID)
- Validate and introspect tokens
- Retrieve user info and propagate identity
- Manage user sessions
- Enforce claims/scopes for fine-grained access control

> 🧭 Kong can act as:
> - **OAuth 2.0 Resource Server (RS)**
> - **OIDC Relying Party (RP)**

---

## ⚙️ **Key Plugin Parameters**

| Parameter       | Type   | Description                                                    |
| --------------- | ------ | -------------------------------------------------------------- |
| `issuer`        | string | URL of the OIDC provider (e.g., `https://accounts.google.com`) |
| `client_id`     | string | Client ID registered with the IdP                              |
| `client_secret` | string | Secret registered with the IdP                                 |
| `scopes`        | array  | OIDC scopes (default: `openid`)                                |
| `realm`         | string | Optional realm for WWW-Authenticate header                     |
### Example configuration

```shell
curl -i -X POST http://localhost:8001/routes/my-route-id/plugins \
  --data "name=openid-connect" \
  --data "config.issuer=https://keycloak.example.com/realms/myrealm" \
  --data "config.client_id=my-client-id" \
  --data "config.client_secret=my-client-secret" \
  --data "config.scopes=openid,email,profile" \
  --data "config.session_secret=supersecretkey" \
  --data "config.redirect_uri_path=/oidc/callback" \
  --data "config.consumer_by=sub,email"
```

---

## 🔄 **Supported Authentication Flows**

| Flow                   | Description                                            |
| ---------------------- | ------------------------------------------------------ |
| **Authorization Code** | Redirect-based flow (most common for user login).      |
| **Client Credentials** | Machine-to-machine (no end-user).                      |
| **Password Grant**     | Legacy flow using username/password (not recommended). |

---

## 🧾 **Authorization Scenarios**

| Type                          | Description                                                           |
| ----------------------------- | --------------------------------------------------------------------- |
| **Claim-Based Authorization** | Use token claims (e.g., roles, groups, scopes) to permit/deny access. |
| **ACL Plugin Integration**    | Combine with the ACL plugin for group-based authorization.            |
| **Consumer Mapping**          | Map IdP users → Kong Consumers to apply policies and quotas.          |

---

## 📝 **Quiz - The OpenID Connect (OIDC) Kong Plugin

>❓ **Mark some mechanisms to authorize access to services protected with Kong OIDC plugin:**

- [ ] Consumer Authorization
- [ ] Client Authorization
- [ ] Claims based Authorization
- [ ] ACL Plugin Authorization

<details>
	<summary>💡 Reveal Answer</summary>
	 - Consumer Authorization<br />
	 - Claims based Authorization<br />
	 - ACL Plugin Authorization
</details>

>❓ **What role does the OpenID Provider play in an OIDC Flow?**

- <input type="radio" name="q2"> It stores refresh tokens
- <input type="radio" name="q2"> It hosts the upstream service
- <input type="radio" name="q2"> It issues access tokens and ID tokens after authenticating the user
- <input type="radio" name="q2"> It redirects all requests to the relying party

<details>
	<summary>💡 Reveal Answer</summary>
	 - It issues access tokens and ID tokens after authenticating the user
</details>

>❓ **What is the main purpose of OpenID Connect (OIDC)**

- <input type="radio" name="q3"> To provide encrypted communication between servers
- <input type="radio" name="q3"> To issue refresh tokens for resource access
- <input type="radio" name="q3"> To verify the identity of end-users and provide basic profile information
- <input type="radio" name="q3"> To manage user access roles across microservices

<details>
	<summary>💡 Reveal Answer</summary>
	 - To verify the identity of end-users and provide basic profile information
</details>

>❓ **Which of the following is NOT listed as a credential or grant type supported by Kong's OIDC plugin?**

- <input type="radio" name="q4"> Authorization Code
- <input type="radio" name="q4"> Session Cookies
- <input type="radio" name="q4"> Client Certificate
- <input type="radio" name="q4"> Username and Password

<details>
	<summary>💡 Reveal Answer</summary>
	 - Client Certificate
</details>


---

>  ⬅️ [Previous: (KGLL-205) Securing Services on Kong Gateway](./%28KGLL-205%29%20Securing%20Services%20on%20Kong%20Gateway.md) | ➡️ [Next: (KGLL-220) Kong Gateway Observability](./%28KGLL-220%29%20Kong%20Gateway%20Observability.md)

---
