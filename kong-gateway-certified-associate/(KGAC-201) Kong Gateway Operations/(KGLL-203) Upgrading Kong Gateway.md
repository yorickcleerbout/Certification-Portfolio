## 🚀 **Upgrading Kong Gateway**

Upgrading Kong Gateway means moving from an **older version** to a **newer version** — including migrating stored data, configurations, and database entities.

When upgrading, Kong performs a **migration** process to move configuration and entity data (for example, from PostgreSQL 3.4.x → 3.10.x).

The upgrade process involves two main phases:

1. 🧱 **Preparation** — plan and validate everything.
2. ⚙️ **Upgrade Execution** — apply the changes safely.

---
## 🧭 **Preparation Phase**
### Steps Overview

1. Check **version compatibility**
2. Determine your **upgrade path**
3. Backup **configuration and database data**
4. Choose an **upgrade strategy**
5. Review **breaking changes**

---

## 🧩 **Preparation Phase: Determining Upgrade Path**
### 🔢 **Kong Gateway Versioning**

 Kong versions follow the pattern:  `MAJOR.MINOR.PATCH.ENTERPRISE_PATCH`

| **Type**                         | **Purpose**                                       | **Example** |
| -------------------------------- | ------------------------------------------------- | ----------- |
| **Major**                        | Significant changes or architecture shifts        | 2.x → 3.x   |
| **Minor**                        | New features and bug fixes (backwards compatible) | 3.4 → 3.10  |
| **Patch**                        | Backwards-compatible fixes                        | 3.4.1       |
| **Sub-patch (Enterprise patch)** | Enterprise-specific fixes                         | 3.4.1.2     |

> 💡 **All patches are cumulative.** For example, patch **1.5** includes all fixes from **1.1–1.4**.


>❓ **Drag and drop the definition to the correct version:**

Features and bug fixes, backwards compatible within a major version sequence | Sub-patch for Kong Gateway Enterprise based on OSS | Making backwards compatible bug fixes | Major functionality and breaking changes

|                **Major**                 |                                  **Minor**                                   |               **Patch**               | **Enterprise-Patch**                               |
| :--------------------------------------: | :--------------------------------------------------------------------------: | :-----------------------------------: | -------------------------------------------------- |
| Major functionality and breaking changes | Features and bug fixes, backwards compatible within a major version sequence | Making backwards compatible bug fixes | Sub-patch for Kong Gateway Enterprise based on OSS |

<details>
	<summary>💡 Reveal Answer</summary>
	 <br />
	 <table>
		 <tr><th><b>Major</b></th><th><b>Minor</b></th><th><b>Patch </b></th><th><b>Enterprise-Patch </b></th></tr>
		 <tr><td>Major functionality and breaking changes</td><td>Features and bug fixes, backwards compatible within a major version sequence</td><td>Making backwards compatible bug fixes</td><td>Sub-patch for Kong Gateway Enterprise based on OSS</td></tr>
		 
		 
	 </table>
</details>

---

### 🧭 **Upgrade Paths**

| **Upgrade Type** | **Example**    | **Description**                   |
| ---------------- | -------------- | --------------------------------- |
| **Major**        | 2.x → 3.x      | Introduces breaking changes       |
| **Minor**        | 3.4.x → 3.10.x | New features, backward-compatible |
| **Patch**        | 3.4.1 → 3.4.2  | Bug fixes, safe upgrade           |
#### Rules
- ✅ You can upgrade between **adjacent versions** (e.g., 3.1 → 3.2).
- ❌ You cannot skip major versions (e.g., 1.x → 3.x).
- ⚙️ Always upgrade sequentially to catch intermediate changes.


#### ✅ Guaranteed Upgrade Paths

Tested and supported by Kong:
- Between **patch releases** of the same minor version
- Between **adjacent minor versions**
- Between **LTS (Long-Term Support)** releases

#### 🔎 Release-Specific Migration

Always review the **release notes** before upgrading.  
Check for:
- Database migration steps
- Breaking changes
- Deprecations

Upgrade paths can vary depending on:
- Deployment mode (Traditional / Hybrid / DB-less)
- Custom plugins
- Hardware or performance limits


>❓ **Match the version to the upgrade:**

![[match-versions.png]]

---

### 🕓 **Long-Term Support (LTS)**

Kong designates specific minor versions as **LTS** (Long-Term Support).

| **Feature**       | **Details**                                         |
| ----------------- | --------------------------------------------------- |
| Support Duration  | 3 years or until distribution lifecycle ends        |
| Compatibility     | Backwards-compatible within same major version      |
| Fixes             | Includes security and selected non-security patches |
| Upgrade Guarantee | Between adjacent LTS versions                       |
> 📘 Reference: [Kong Version Support Policy](https://developer.konghq.com/gateway/version-support-policy/)

---
### 🌇 **Sunset Support**

After the LTS period ends, Kong offers a **12-month sunset period** to assist with upgrades.

- ❌ No new patches released.
- ✅ Support only for upgrading to a supported version.

> [Sunset Policy Details](https://developer.konghq.com/gateway/version-support-policy/#older-versions)

---
### ⚠️ **Feature Deprecation**

Kong may deprecate older features as the platform evolves.  
Recent examples:
- **Vitals**
- **Dev Portal** (deprecated from **3.5+**)

> Kong provides at least **6 months’ notice** before removal, unless urgent for **security** or **legal** reasons.  
> Once deprecated, the feature will **not receive new functionality**.

---

## 💾 **Preparation Phase : Backup Strategies**

Before any upgrade, **always back up** your configuration and database.

> 📘 Reference: [Backup Strategy Guide](https://developer.konghq.com/gateway/upgrade/#preparation-choose-a-backup-strategy)

#### 🗃 Database Backup:

- Use **PostgreSQL native tools** for export/import.
- Ensures consistent backups and quick restores.
- Required for **Traditional** and **Hybrid** modes.

#### 🧾 Declarative Backup

- Use **decK** or **Kong CLI** for configuration export/import.
- **decK** = YAML-based “configuration as code” (great for version control).
- **Kong CLI** = direct command-line management of configs.
- **DB-less** mode: maintain YAML declarative files manually.

---

## ⚙️ **Preparation Phase : Gateway Upgrade Strategies**

Upgrades can be **simple or complex**, depending on your environment. Kong recommends proven strategies to minimize downtime and risk.

| **Strategy**     | **Description**                                                           | **Downtime** | **Used For**               |
| ---------------- | ------------------------------------------------------------------------- | ------------ | -------------------------- |
| **Dual Cluster** | Run old (X) and new (Y) clusters side by side, gradually shifting traffic | ❌ None       | Production-safe            |
| **In-Place**     | Stop old version, upgrade to new                                          | ⚠️ Possible  | Controlled, single cluster |
| **Blue-Green**   | Both versions share DB; traffic switched logically                        | ⚠️ Low       | Not recommended            |
| **Rolling**      | Replace nodes gradually with new version                                  | ❌ None       | DB-less / Konnect          |

#### Dual Cluster Upgrade
-  A new cluster (say version Y) is deployed alongside the current version (say version X) , so that two clusters serve requests concurrently during the upgrade process.
- Traffic ratio is adjusted between the two clusters to switch traffic over from the old cluster to the new one based on the business metrics.

![[dual-cluster-upgrade.png]]

#### In Place Upgrade
In this method, the current cluster X is shutdown and new version cluster Y is configured. This is may result in business downtime, hence this strategy must be carefully evaluated.

![[inplace-upgrade.png]]
#### Blue-Green Upgrade
- An advanced strategy derived from in-place upgrade, where both the current version and the new version can use the same database without having to shut down the current cluster.
- This strategy consumes fewer resources without causing business downtime. However, this approach is not recommended.

![[blue-green-upgrade.png]]

#### Rolling Upgrade
- Used specifically in DB-less mode, in which new nodes are continuously added while shutting down current nodes.
- All the nodes will point to the same yaml config file.

![[rolling-upgrade.png]]

---

## 🧭 **Strategy guidelines by deployment type**


![[upgrade-guidance-deploy-type.png]]

|**Deployment Mode**|**Recommended Strategies**|
|---|---|
|Traditional / Hybrid (Control Plane)|Dual Cluster or In-Place|
|DB-less / Konnect (Data Plane)|Rolling Upgrade|

> [Kong Upgrade Strategy Guide](https://developer.konghq.com/gateway/upgrade/#preparation-choose-an-upgrade-strategy-based-on-deployment-mode)

---

## 🧩 **Preparation Phase : Upgrade Strategies - Hybrid mode**

Hybrid deployments involve both **Control Plane (CP)** and **Data Plane (DP)** nodes, so the strategy may vary for each.

### 🧱 **Dual Cluster (Control Plane)**

The new **CP Y** is deployed alongside the current **CP X**, while the current **DP** nodes **X** are still serving **API** requests.

![[hybrid-dual-cluster.png]]

### 🔄 **In-Place (Control Plane)**

The current **CP X** is directly replaced by a new **CP Y**. The database is reused by the new **CP Y**, and the current **CP X** is shut down once all nodes are migrated.

> ⚠️ Requires **downtime** during CP switchover.

![[hybrid-in-place.png]]

### **Dual Cluster CP + DP Rolling Upgrade**

The new **CP Y** is deployed alongside the current **CP X**, while the current **DP** nodes **X** are still serving **API** requests.

> ✅ Zero downtime approach for hybrid environments.

![[hybrid-rolling.png]]

---

## 📝 **Test your knowledge**

>❓ **Which upgrade strategy in Hybrid Mode involves deploying a new Control Plane alongside the current one while the current Data Plane nodes continue serving API requests?**

- <input type="radio" name="q1"> In-Place Upgrade Strategy
- <input type="radio" name="q1"> Dual Cluster Strategy
- <input type="radio" name="q1"> Database Migration Strategy
- <input type="radio" name="q1"> Rolling Upgrade Strategy

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Rolling Upgrade Strategy<br />
	 <br />
	 <i>This is the correct strategy where a new Control Plane is deployed alongside the current one, allowing the current Data Plane nodes to continue serving API requests.</i>
</details>

---

## ⚙️ **Execution Phase : Performing an Upgrade**

Once the **preparation phase** is complete, you can begin the **execution phase**, which involves applying the upgrade and performing necessary data migrations.

### 🧩 **Performing a Kong Upgrade**

Upgrading Kong Gateway typically involves **migrating datastore data** to a newer schema version.  
Kong provides built-in migration commands via **`kong migrations`**.

#### 🔧 Example

```bash
kong migrations bootstrap
```

> Bootstraps a new version of the database compatible with the installed Kong version.

#### 📋 Migration Commands Overview

| Command                                  | Purpose                                                                                      |
| ---------------------------------------- | -------------------------------------------------------------------------------------------- |
| `bootstrap`                              | Bootstrap the database and run all migrations                                                |
| `up`                                     | Run any new migrations                                                                       |
| `finish`                                 | Finish running any pending migrations after 'up'                                             |
| `list`                                   | List executed migrations                                                                     |
| `reset`                                  | Reset the database                                                                           |
| `migrate-community-to-enterprise`        | Migrates CE entities to EE on the default workspace                                          |
| `upgrade-workspace-table`                | Outputs a script to be run on the DB to upgrade the entity for 2.x workspaces implementation |
| `reinitialize-workspace-entity-counters` | Resets the entity counters from the database entities                                        |

### **🗃️ Migrating Database Data**

Kong’s migrations are designed to be **non-destructive** and **incremental** —  
you don’t need to copy the entire database manually.
#### 🔄 Two-Stage Migration Process
1. **`kong migrations up`**
    - Performs _non-destructive_ operations (safe schema updates).
    - Applies changes that won’t break the current configuration.
2. **`kong migrations finish`**
    - Puts the database into its final upgraded state.
    - Completes any pending changes introduced by `up`.

### 🚀 **Performing the Upgrade**

Follow these steps during the upgrade phase to ensure a smooth transition.

#### Step 1 — Stop Configuration Updates

❌ Do **not** modify configurations during migration.  
Pause all Admin API, Kong Manager, `decK`, or direct database writes.

> This guarantees **data consistency** between the old and new clusters.

#### Step 2 — Create Clusters

- Stop the **old cluster**.
- Install **Kong Gateway (new version)** on the new cluster.
- Verify connectivity and configuration compatibility.

#### Step 3 — Database Migration
|**Strategy**|**Action**|
|---|---|
|**Dual Cluster Upgrade**|Deploy a **new database instance**, restore the backup created during preparation.|
|**In-Place Upgrade**|Run: `kong migrations up` `kong migrations finish`|

#### Step 4 — Configure the New Cluster
| **Strategy**     | **Configuration**                                         |
| ---------------- | --------------------------------------------------------- |
| **Dual Cluster** | Point the new cluster to the **newly restored database**. |
| **In-Place**     | Point the new cluster to the **existing database**.       |
#### Step 5 — Decommission Old Clusters

After verifying that the **new cluster is functional**, you can safely **decommission** the old cluster(s).

> 🧩 Tip: Always validate plugin compatibility and test key API routes before retiring old nodes.

### 🧠 **Summary**

|**Phase**|**Key Tasks**|**Goal**|
|---|---|---|
|Migrations|Run `up` and `finish`|Upgrade database schema safely|
|Configuration Freeze|Stop config updates|Maintain consistency|
|Cluster Setup|Deploy and connect new nodes|Validate new environment|
|Cleanup|Decommission old systems|Finalize transition|
> ✅ Successful upgrades rely on **planning**, **backup validation**, and **controlled execution**.


---
## 📝 **Quiz - Upgrading Kong Gateway

>❓ **Which of the following are valid Kong migration subcommands? (Select all that apply)**

- [ ] bootstrap
- [ ] up
- [ ] finish
- [ ] apply
- [ ] list

<details>
	<summary>💡 Reveal Answer</summary>
	 -  bootstrap<br />
	 - up<br />
	 - finish<br />
	 - list
</details>

>❓ **What happens during the sunset support period of a Kong Gateway version?**

- <input type="radio" name="q2"> No patches are provided, but limited support helps customers upgrade
- <input type="radio" name="q2"> All patches continue to be provided
- <input type="radio" name="q2"> Only security patches are delivered
- <input type="radio" name="q2"> Version is fully supported with regular feature updates

<details>
	<summary>💡 Reveal Answer</summary>
	 -  No patches are provided, but limited support helps customers upgrade
</details>

>❓ **Kong adheres to semantic versioning, making a distinction between the following versions:**

- [ ] Major
- [ ] Minor
- [ ] Major Patch
- [ ] Minor Patch
- [ ] Patch
- [ ] Bug Fix

<details>
	<summary>💡 Reveal Answer</summary>
	 -  Major<br />
	 - Minor<br />
	 - Patch
</details>

>❓ **Stages of full migration for a Kong Gateway installation are:**

- [ ] kong migrations bootstrap
- [ ] kong migrations list
- [ ] kong migrations up
- [ ] kong migrations finish
- [ ] kong migrations reset

<details>
	<summary>💡 Reveal Answer</summary>
	 - kong migrations up<br />
	 - kong migrations finish
</details>

---

>  ⬅️ [Previous: (KGAC-202) Kong Gateway Installation](./%28KGAC-202%29%20Kong%20Gateway%20Installation.md) | ➡️ [Next: (KGLL-204) Securing Kong Gateway Control Plane](./%28KGLL-204%29%20Securing%20Kong%20Gateway%20Control%20Plane.md)

---

