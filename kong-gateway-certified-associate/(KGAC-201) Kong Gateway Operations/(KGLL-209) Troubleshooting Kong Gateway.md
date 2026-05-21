## 🛠️ **Troubleshooting Methodology**

![[troubleshooting-methodology.png]]
## 🎯 **Goal**

Systematically isolate where a failure occurs (client ↔ **Kong** ↔ upstream), gather the right evidence fast, and resolve with minimal impact.

---

## 🧾 **Problem Report (Capture the Basics)**

Ask and record:

- **Current behavior:** What exactly happens?
- **Expected behavior:** What should happen?
- **Regression check:** Did this work before?
- **Change log:** What changed (config, code, infra, certs, traffic, dependencies)?

---

## 🔍 **Points of Failure (High-Level Map)**

A request can fail at three layers:

1. **Downstream** – Client → Kong (network, TLS, headers, auth, payload)
2. **Kong** – Core, routing, **plugins**, configuration
3. **Upstream** – Kong → Backend (DNS, network, TLS, timeouts, service errors)

> Most downstream/upstream issues are **network** or **certificate** related.

---
## ✅ **REST API Troubleshooting Checklist**

- URL/host/path correct?
- HTTP **method** correct?
- **Authorization** header valid?
- **API key / token** valid & unexpired?
- Consumer/app has **feature access / scope**?
- Request **parameters** valid (shape, types, encoding)?
- **SSL**: valid cert chain and trusted CA?
- Client handles **error codes** correctly?
- Response format follows **contract**?
- Result **correct** and **timely** (latency/SLO)?
- Do **4xx/5xx** reveal insight (auth, rate limit, upstream)?

---
## 🧰 **Diagnosis & Information Gathering**

### **Tools**

- **API clients:** Insomnia, HTTPie, cURL, Postman
- **Debug proxies:** Fiddler, Charles, HTTP Toolkit, Proxyman
- **Packet capture:** `tcpdump`, Wireshark, Sysdig, CloudShark
### **Approach**

- **Test → Treat → Cure**
    - Possible causes? Any **recent changes**?
    - **Reproduce** end-to-end; find the first failing step.
    - **Connectivity**: DNS, routes, ports, TLS, proxies.
    - **Performance**: CPU/RAM/IO/DB, concurrency, load test.

> Keep a log of **tests & results** to quickly narrow suspects.

### **Docs for RCA/Solutioning**

- [https://docs.konghq.com](https://docs.konghq.com)
- [https://support.konghq.com/support/s/knowledge](https://support.konghq.com/support/s/knowledge)

---
## 🩺 **Triage → Examine → Diagnose**

- **Business impact**
    - Urgent / High / Normal / Low
- **Blast radius**
    - How many users/flows/environments?
- **Where**
    - Frontend (downstream) / Gateway / Backend (upstream)
- **Reproducibility**
    - Exact steps and inputs
- **Evidence**
    - Errors/warnings in logs:
        - `/usr/local/kong/logs/access.log`
        - `/usr/local/kong/logs/admin_access.log`
        - `/usr/local/kong/logs/error.log`
        - `/usr/local/kong/logs/admin_gui_access.log`

**Capture snapshots**

- Save logs for the **incident window**
- Save config:  
    `http GET <kong_host>:8001`
- Status:  
    `http GET <kong_host>:8001/status`
- Metrics:  
    `http GET <kong_host>:8001/metrics`

---
## 📈 **Business Impact & Support SLAs**

![[support-sla.png]]

---

## 📦 **Kong Information Collection (for Support)**

### Gather:

- `:3001/`
- `:3001/status`
- `error.log`

### Also helpful:

- **Config dumps** (`kong config db_export`)
- **decK** state (to reproduce config)
- **Repro steps** (most valuable for RCA)

---
## 📚 **Kong Error Log Files**

**Adjust granularity via `log_level`. Default paths:**

- **`proxy_error_log`** → `logs/error.log` (HTTP/S proxy errors)
- **`proxy_stream_error_log`** → `logs/error.log` (TCP stream errors)
- **`admin_error_log`** → `logs/error.log` (Admin API errors)
- **`status_error_log`** → `logs/status_error.log` (Status API errors)
- **`admin_gui_error_log`** → `logs/admin_gui_error_log` (Kong Manager UI)
- **`portal_gui_error_log`** → `logs/portal_gui_error_log` (Dev Portal UI)
- **`portal_api_error_log`** → `logs/portal_api_error_log` (Dev Portal API)

### Log Levels (when to use)

- **debug** – deep runloop/plugin details (use sparingly)
- **info/notice** – normal behavior (mostly ignorable)
- **warn** – abnormal but not dropped; investigate
- **error** – dropped request (e.g., 500); monitor rate
- **crit** – severe degradation impacting many clients

---
## 🖥️ **Kong System Logs**

- Default to **stdout** (can redirect to a file)
- Add `--vv` for verbose startup/runtime
- Startup logs reveal **effective configuration** loaded in memory

---
## 🧾 **Kong Debug Header & Granular Tracing**

Send `Kong-Debug: 1` to receive headers:

- `Kong-Route-Id`, `Kong-Route-Name`
- `Kong-Service-Id`, `Kong-Service-Name`

Granular Tracing provides detailed lifecycle metrics/debug output:  
https://developer.konghq.com/gateway/configuration/#granular-tracing-section

---
## 🌐 **Network Troubleshooting**

### **Downstream vs Upstream?**

If an **upstream** error occurs, Kong usually returns a clear error message.  
If a **client** reports a generic failure, suspect **downstream** first.

> Use targeted packet capture to minimize PCAP size (filter by IP/port).

![[upstream-downstream-error.png]]

### **Common Network Symptoms**

- Slow proxy performance
- TLS handshake failures (upstream)
- Unexpected upstream **500s**
- Slow page loads/downloads

> Network issues are tricky—**collect the right evidence** early to converge quickly.

---
## 🧨 **Other Failure Sources**

- Misconfigurations (**most common**)
- Bugs in **custom plugins**
- Kong bugs (check release notes/patches)
- Resource starvation (CPU, RAM, sockets, file descriptors)
- Performance regressions

> Use **method of exclusion**: disable non-essentials, confirm the minimal failing path, then re-enable stepwise.

---
## 🧭 **Quick Triage Cheat-Sheet**

1. **Scope**: Which routes/services/environments? Since when?
2. **Health**: `GET /status/ready` (LB), `GET /status` (liveness)
3. **Metrics**: spikes in **latency**, **5xx**, **timeouts**?
4. **Logs**: any **plugin** errors? **JWT/OIDC** failures? **Rate limit** hits?
5. **Network**: DNS resolve, TLS chain, SNI, firewall, MTU, proxy hops
6. **Upstream**: `curl -vk` directly to upstream (bypass Kong)
7. **Kong path**: Disable nonessential plugins → retest
8. **Config drift**: decK diff vs desired state
9. **Roll back**: last safe version/config if needed

---

## 📌 **Best Practices**

- Prefer **readiness** probes for traffic gating; **liveness** for restart only
- Enable `status_listen`; scrape **/metrics** (Prometheus)
- Alert on **5xx**, **p99 latency**, **DB reachability**, **DP last_seen**
- Keep cert chains current (server & client/mTLS)
- Version and review gateway **config as code** (decK)
- Avoid relying on `kong health` for overall status (process ≠ readiness)

---

## 📝 **Quiz - Troubleshooting Kong Gateway


>❓ **What are some methods to get more details on the problem from the client side?**

- [ ] Generating HAR file
- [ ] Using cURL
- [ ] Getting a packet capture
- [ ] Restarting Kong Gateway

<details>
	<summary>💡 Reveal Answer</summary>
	 - Generating HAR file<br />
	 - Using cURL<br />
	 - Getting a packet capture
</details>

>❓ **What tool can be used to apply configuration changes to Kong from a YAML file?**

- <input type="radio" name="q1"> jq
- <input type="radio" name="q1"> deck
- <input type="radio" name="q1"> kong reload
- <input type="radio" name="q1"> curl

<details>
	<summary>💡 Reveal Answer</summary>
	 - deck
</details>

>❓ **What is the primary purpose of querying `localhost:8001` in Kong Gateway troubleshooting?**

- <input type="radio" name="q2"> To view service logs
- <input type="radio" name="q2"> To verify upstream connectivity
- <input type="radio" name="q2"> To retrieve Kong node configuration and status
- <input type="radio" name="q2"> To display routes and consumers

<details>
	<summary>💡 Reveal Answer</summary>
	 - To retrieve Kong node configuration and status
</details>

>❓ **What type of information is available at the /status endpoint?**

- <input type="radio" name="q4"> Current plugins and routes
- <input type="radio" name="q4"> Admin user activity
- <input type="radio" name="q4"> Memory usage, database connectivity, and Lua VM metrics
- <input type="radio" name="q4"> API key status and permissions

<details>
	<summary>💡 Reveal Answer</summary>
	 - Memory usage, database connectivity, and Lua VM metrics
</details>

---

>  ⬅️ [Previous: (KGLL-220) Kong Gateway Observability](./%28KGLL-220%29%20Kong%20Gateway%20Observability.md) 

---