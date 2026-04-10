# How Do You Deploy PaperCut and Find-Me Printing From Scratch?

## Metadata

| Field | Value |
|---|---|
| Category | Print Management / How-To |
| Difficulty | Advanced |
| Time to Complete | 2–4 hours |
| Last Verified | 2026-04-11 |
| Prerequisites | [PaperCut Architecture & Components](../Concepts/papercut-architecture-and-components.md) |

## The Question

> Your enterprise is rolling out PaperCut MF for the first time. You need to set up the PaperCut Application Server, configure "Find-Me" (Secure Release) printing via virtual queues, deploy the configuration to the physical devices, and roll it out to users end-to-end.

## Quick Answer (TL;DR)

1. **Install** PaperCut Application Server and configure AD/LDAP user sync.
2. **Build Physical Queues** on the Print Server that point directly to the devices.
3. **Build a Virtual Queue** (The "Find-Me" queue) on the Print Server using a generic global driver.
4. **Configure PaperCut** to route jobs from the Virtual Queue into the Physical Queues.
5. **Deploy Embedded Apps** to the physical MFPs so users can authenticate and release jobs.
6. **Deploy** the Virtual Queue and PaperCut Client to users via GPO/Intune.

---

## 🧠 Core Concept: Why Use a Virtual Queue?

Before starting, it is critical to understand the architecture of **Find-Me Printing** (also called Secure Release or Pull Printing). 

In a traditional setup, users have 5 different printers mapped on their PC (`Level1-Printer`, `Level2-Printer`, etc.). If a printer breaks, the user asks the helpdesk to install a different one.

In a **Find-Me** setup:
1. Users only have **ONE** printer installed: `Follow-Me-Queue` (The Virtual Queue).
2. Users print to this queue. The job is **HELD** on the server.
3. The user walks up to **ANY** physical printer in the building and swipes their ID badge.
4. PaperCut magically redirects the held job from the Virtual Queue to the Physical Queue of the exact printer they are standing in front of.

**Benefits:** No wasted paper (jobs left on the tray are never printed), extreme security (confidential docs aren't sitting on printers), and zero helpdesk calls for mapping new printers.

---

## Step-by-Step Procedure

### Phase 1: Server Setup & Identity Sync

| Step | Action | Verification |
|---|---|---|
| 1.1 | Spin up a Windows Server to act as both the Print Server and PaperCut Server (Small/Mid-sized org). | Server joined to domain. |
| 1.2 | Install the **Print and Document Services** role via Server Manager. | `printmanagement.msc` opens. |
| 1.3 | Run the **PaperCut MF Installer**. Select "Standard Installation" (Application Server + Print Provider). | Admin console accessible at `http://localhost:9191/admin` |
| 1.4 | Log into the Admin console. Navigate to **Options → User/Group Sync**. | |
| 1.5 | Connect to your Active Directory. Select the groups that contain your users and click **Synchronize Now**. | Users appear in the **Users** tab. |

> [!WARNING] 
> Do not sync your entire AD forest. Only sync groups containing actual human users. Syncing service accounts wastes licenses and clutters the database.

### Phase 2: Create the Physical Queues

These are the queues that actually talk to the hardware. **Users will NEVER see these queues.**

| Step | Action | Verification |
|---|---|---|
| 2.1 | Open `printmanagement.msc`. | |
| 2.2 | Add a **Standard TCP/IP Port** for Printer A (e.g., `10.1.1.50`). | |
| 2.3 | Install the exact vendor-specific driver for Printer A (e.g., *Canon Generic Plus PCL6*). | |
| 2.4 | Create the queue. Name it clearly as a physical device: e.g., `PHYSICAL-L1-Canon-C5560i`. | |
| 2.5 | **DO NOT share this printer.** (Leaving it unshared ensures users cannot bypass PaperCut by connecting to it directly). | Queue exists only locally on the server. |
| 2.6 | Repeat for all other physical printers in the fleet. | All physical devices have a local, unshared queue. |

### Phase 3: Create the Virtual "Find-Me" Queue

This is the ONLY queue that users will see and connect to.

| Step | Action | Verification |
|---|---|---|
| 3.1 | In `printmanagement.msc`, create a new port: **Local Port** named `NUL` (or use an IP that points nowhere). | Port exists. |
| 3.2 | Select a **Global/Universal Driver** (e.g., *PaperCut Global PostScript* or *HP Universal Print Driver*). This driver must be highly compatible since jobs rendered by it will ultimately be sent to various physical printer brands. | |
| 3.3 | Name the queue to be user-friendly: e.g., `Find-Me-Print` or `Secure-Print`. | |
| 3.4 | **SHARE** this printer (e.g., Share name: `Find-Me-Print`). | `\\printserver\Find-Me-Print` is accessible. |
| 3.5 | Right-click the queue → Advanced → Uncheck "Enable advanced printing features" (this forces RAW spooling instead of EMF, which is required for PaperCut watermarking and tracking). | |

### Phase 4: Configure the Find-Me Routing in PaperCut

Now we tell PaperCut how to connect the Virtual Queue to the Physical Queues.

| Step | Action | Verification |
|---|---|---|
| 4.1 | Log into **PaperCut Admin** → **Printers** | All physical queues and the virtual queue should be listed. |
| 4.2 | Click your virtual queue (`Find-Me-Print`). | |
| 4.3 | Under the **Summary** tab, look for **Queue Type** and select **This is a virtual queue (jobs will be forwarded to a different queue)**. | Virtual routing settings appear. |
| 4.4 | Scroll down to **Job Routing**. Select the physical queues (`PHYSICAL-L1-Canon`, etc.) that this virtual queue is allowed to forward to. | Checkboxes ticked for target hardware. |
| 4.5 | Under **Hold/Release Queue Settings**, check **Enable hold/release queue**. | |
| 4.6 | Change **Release mode** to **User release (users must authenticate at the device)**. | |

### Phase 5: Deploy Embedded Apps to Physical Printers

The physical printer touchscreen needs the PaperCut app so the user can log in and command the server to route their held job.

| Step | Action | Verification |
|---|---|---|
| 5.1 | PaperCut Admin → **Devices** tab → **Create Device**. | |
| 5.2 | Enter the IP Address of the physical printer and select its brand (e.g., Canon MEAP, Ricoh SmartSDK, HP OXP). | PaperCut connects to the device. |
| 5.3 | Under **Print Release**, check **Enable print release** and select the Virtual Queue (`Find-Me-Print`). This tells the physical printer to look inside that exact virtual queue for held jobs. | |
| 5.4 | Under **Authentication methods**, enable **Identity card (Swipe badge)** and **PIN** (as a fallback). | |
| 5.5 | Click **Apply**. PaperCut will push the embedded app over the network to the physical printer. | Printer screen reboots and shows the PaperCut "Please swipe your card" screen. |

### Phase 6: Rollout to the User

| Step | Action | Verification |
|---|---|---|
| 6.1 | **Deploy the Queue:** Use Group Policy (User Config → Preferences → Printers) to map `\\printserver\Find-Me-Print` to all staff. | User sees `Find-Me-Print` in their devices. |
| 6.2 | **Deploy the PaperCut Client:** (Optional but recommended). Use SCCM/Intune/GPO to deploy `C:\Program Files\PaperCut MF\client\win\pc-client-admin-deploy.msi` to user workstations. | The green "P" icon appears in the user's system tray, displaying their balance. |

---

## 🎯 End-to-End Verification (The Ultimate Test)

Do not hand this over to production until you have completed this exact physical walkthrough:

1. **Print**: Tell a test user to print a Word document to the `Find-Me-Print` queue.
2. **Observe Server**: Go to PaperCut Admin → **Printers** → **Jobs Pending Release**. You should see the job sitting there, HELD.
3. **Walk**: Walk to ANY physical printer that has the embedded app installed.
4. **Authenticate**: Swipe the test user's ID badge on the reader.
5. **Card Registration**: If it's a new card, the screen will ask the user to enter their AD Username and Password to associate the badge with their account (this only happens once).
6. **Release**: The screen shows "1 job pending". Press **Print All**. 
7. **Observe Hardware**: The printer receives the physical data stream and prints the page.
8. **Audit**: Log back into PaperCut Admin → **Logs** → **Job Log**. Verify the job was recorded, charged the correct amount, and explicitly states it was printed on the *Physical* queue but originated from the *Virtual* queue.

---

## What Can Go Wrong (Troubleshooting)

| Symptom | Cause | Resolution |
|---|---|---|
| Job prints immediately when user hits print (no hold). | Hold/Release is not enabled on the Virtual Queue. | Go to Phase 4.5 and check "Enable hold/release". |
| User swipes badge at MFP, but screen says "No jobs pending". | The MFP's embedded app isn't looking at the right virtual queue. | Go to Devices → click device → Print Release settings → ensure it is pointing to `Find-Me-Print`. |
| Printer spits out hundreds of pages of garbage text (wingdings/symbols). | Driver mismatch. The virtual queue rendered the job in a language (like PostScript) that the physical printer hardware doesn't understand (e.g., it only speaks PCL). | Change the Virtual Queue driver to match what the physical hardware expects, or use a true Universal Driver. |
| Jobs get stuck in the virtual queue with "Error - Printing". | The Virtual Queue is trying to send data to a hardware port, but it's set to a Local Port (NUL). | Ensure the "Enable advanced printing features" box is unchecked on the Virtual Queue (Phase 3.5). |
| Users complain they can't print in colour. | The global driver used on the Virtual Queue defaults to Black & White only, or PaperCut filters are overriding it. | Check Printing Defaults on the Virtual Queue and PaperCut Filters tab. |

## Related Questions
- [Setup a Brand New Printer on PaperCut](./setup-brand-new-printer-on-papercut.md)
- [How Windows Print Architecture Works](../Concepts/how-windows-print-architecture-works.md)
- [PaperCut Architecture and Components](../Concepts/papercut-architecture-and-components.md)
