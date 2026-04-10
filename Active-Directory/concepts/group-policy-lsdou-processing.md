# Group Policy & LSDOU Processing Explained

## Metadata

| Field | Value |
|---|---|
| Category | Active Directory / Concept |
| Last Updated | 2026-04-11 |
| Sources | Pluralsight: Group Policy Fundamentals |

## One-Line Summary

> Active Directory Group Policy Objects (GPOs) apply configuration settings to computers and users based on a strict hierarchy known as **LSDOU** (Local, Site, Domain, Organizational Unit), where the policy applied closest to the object always wins conflicts.

---

## The Big Picture

Group Policy is the primary mechanism for standardizing security, mapping drives, deploying software, and restricting user environments in an Active Directory (AD) enterprise.

Instead of touching 1,000 computers individually, you attach a **Group Policy Object (GPO)** to an Active Directory container. When a computer boots up, or a user logs in, they process their assigned GPOs and apply the settings automatically.

## Understanding LSDOU (Order of Precedence)

Because AD is a highly nested hierarchy, a user might inherit policies from five different levels. To ensure predictability, Windows applies GPOs in a strict order: **LSDOU**. 

When conflicts occur (e.g. Domain policy says "Enable Firewall", but OU policy says "Disable Firewall"), **the last policy applied wins**.

1. **(L) Local GPO:** Applied first. This represents settings physically configured on the individual machine via `gpedit.msc`. It has the lowest priority and is overwritten by anything from the domain.
2. **(S) Site GPO:** Applied second. These are linked to AD Sites (physical IP subnet groupings, like "Dallas Branch Office"). Rarely used, but good for linking branch-specific printers.
3. **(D) Domain GPO:** Applied third. These are linked at the very root of the AD domain (e.g., `contoso.local`) and affect every user and computer inside it.
4. **(OU) Organizational Unit GPO:** Applied last. These are linked to specific OUs (e.g., `Sales-Laptops`). Because they are processed last, an OU policy will overwrite a conflicting Domain policy.

## Modifying the Flow: Filtering

Sometimes you apply a GPO to an OU, but you don't want *everyone* in that OU to receive the settings (e.g., locking down USB drives, but excluding the local IT admins). You use **Filtering** to exclude them:

| Filter Type | Where it Works | Description |
|---|---|---|
| **Security Group Filtering** | Whole GPO Level | By default, GPOs apply to the `Authenticated Users` group. You can remove this and specify a specific AD Security Group (e.g., `HR_Computers`). |
| **WMI Filtering** | Whole GPO Level | Evaluates a hardware/software query. (e.g., `Select * FROM Win32_OperatingSystem WHERE Caption LIKE "%Server%"`). The GPO only applies if the machine passes the query. |
| **Item-Level Targeting** | Setting Level (GP Preferences) | Instead of filtering the whole GPO, you filter individual settings inside it. (e.g., Map the S:\ drive, but only if the user is in the Sales group or using Windows 10). |

## Modifying the Flow: Force & Blocks

Enterprise environments require exceptions. OU Administrators and Domain Administrators often battle for control over endpoint settings. Microsoft provides two switches to override standard LSDOU inheritance:

### 1. Block Inheritance (The OU Shield) 🛡️
* **Where it's applied:** At the **Container (OU)** level.
* **What it does:** It blocks all upstream GPOs (Domain, Site, parent OUs) from flowing into this OU.
* **Why use it:** When setting up a "Kiosk" or "VIP" OU where standard corporate restrictive policies would break specialized systems.

### 2. Enforced Links (The Domain Hammer) 🔨
* **Where it's applied:** At the **GPO Link** level (usually by Domain Admins).
* **What it does:** It ensures that a specific GPO is applied *no matter what* downstream OUs try to do. 
* **The Golden Rule:** `Enforced` always trumps `Block Inheritance`. If an OU attempts to block inheritance, an Enforced Domain policy will still smash through the shield and apply.

## How This Connects to Other Systems

### Upstream Dependencies
* **Active Directory Structure:** Badly designed OU structures make Group Policy a nightmare. GPOs map directly to AD layout.
* **DNS:** If a client cannot resolve the Domain Controller via DNS, it cannot download the GPO.

### Downstream Impact
* **Endpoint Management / Intune:** Modern MDM tools (like Intune) often conflict with legacy GPOs if not managed correctly.
* **Security & Auditing:** GPOs are the primary delivery vehicle for CIS hardened baselines and EDR agent deployments.

## Troubleshooting Quick Tips
* If a policy isn't applying, use `gpresult /r` on the client PC to generate a report of exactly which GPOs processed, which ones were denied, and why (e.g., "Filtered out by WMI").
