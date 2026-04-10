# How to Map Drives and Printers using GPP

## Metadata

| Field | Value |
|---|---|
| Category | Active Directory / How-To |
| Last Updated | 2026-04-11 |
| Sources | Pluralsight: Group Policy Fundamentals |

## What is this and why use it?

For decades, IT administrators deployed `logon.bat` or `.vbs` scripts to map network drives and printers based on what department a user was in. 

**Group Policy Preferences (GPP)** completely replaces legacy logon scripts. It is a graphical, native method to map UNC paths and printers that is dramatically faster, easier to troubleshoot, and features item-level logic.

---

## 💾 How to Map a Network Drive

Drive mapping is a **Per-User** preference.

### Step 1: Create the Drive Map Preference
1. Open **Group Policy Management Console (GPMC)** and edit your GPO.
2. Navigate to: `User Configuration \ Preferences \ Windows Settings \ Drive Maps`.
3. Right-click and choose **New -> Mapped Drive**.

### Step 2: Configure the Drive Settings
1. **Action:** Select `Update` (this creates the drive if it doesn't exist, and updates it if the path changes).
2. **Location:** Enter the UNC path (e.g., `\\Server01\FinanceShare$`).
3. **Drive Letter:** 
   * Choose **Use first available** if you don't care what letter it gets.
   * Choose **Use:** and select a specific letter (e.g., `S:`) to hardcode it.
4. **Visibility:** You can optionally check `Hide this drive` if it's a backend system drive you don't want users clicking on. 

> [!WARNING]
> **Do not use the 'Connect As' feature.** It stores the credentials in the GPO XML file out in the open (which can be read by anyone on the domain). Microsoft has formally deprecated this feature due to extreme security risks.

### Step 3: Apply Item-Level Targeting (Department Logic)
If you link this GPO to the whole domain, everyone gets the Finance drive. To restrict it to just the Finance department:
1. Click the **Common** tab.
2. Check **Item-level targeting** and click the **Targeting...** button.
3. Click **New Item -> Security Group**.
4. Select your AD group (e.g., `SG_FinanceUsers`). 
*Result: When a user logs in, Windows transparently checks if they are in the group. If yes, the `S:` drive is mapped.*

---

## 🖨️ How to Map a Printer

Printer mapping can be done **Per-User** or **Per-Computer**. The location is slightly different from drives:
Navigate to: `Preferences \ Control Panel Settings \ Printers`.

### Step 1: Choose the Printer Type
When you right-click to create a new printer, you have three distinct options depending on your environment architecture (see [Windows Print Architecture](../../Print-Management/Concepts/how-windows-print-architecture-works.md)):

1. **Shared Printer (UNC)**: The most common enterprise setup. Points to a Windows Print Server queue (e.g., `\\PrintServer01\FollowMe-Queue`).
2. **TCP/IP Printer**: Bypasses the print server entirely and maps the PC directly over the network to the printer's IP address.
3. **Local Printer**: Connects a physical USB printer mapped to a local port.

### Step 2: Configure the Printer
1. Provide the **Share Path** or **IP Address**.
2. Check **Set this printer as the default printer** if applicable.
3. *(Optional)* Provide a **Printer Path to drivers**. If the endpoint doesn't have the printer driver installed, you can drop a UNC path here pointing to the raw `.inf` driver files so the PC can fetch them automatically during the map.

### Step 3: Deployment
Just like drive maps, you can use **Item-Level Targeting** on the *Common* tab. For printers, a fantastic targeting option is **IP Address Range**:
* Instead of mapping the printer based on *who* the user is, you map it based on *where* they are sitting (e.g., "If endpoint IP is `192.168.10.x`, map the Dallas Branch Office printer").
