# Setup a Brand New Printer on PaperCut

## Metadata

| Field            | Value                                                                                                                                                                        |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Category         | Print Management / How-To                                                                                                                                                    |
| Difficulty       | Intermediate                                                                                                                                                                 |
| Time to Complete | 30–45 minutes                                                                                                                                                                |
| Last Verified    | 2026-04-10                                                                                                                                                                   |
| Sources          | _(Add your source IDs after processing your company KB docs)_                                                                                                                |
| Prerequisites    | [PaperCut Architecture & Components](../concepts/papercut-architecture-and-components.md), [Windows Print Architecture](../concepts/how-windows-print-architecture-works.md) |

## The Question

> A brand new Canon imageRUNNER ADVANCE C5560i has been delivered to Level 3. Set it up end-to-end: network, print server, PaperCut, and deploy to Level 3 users.

## Quick Answer (TL;DR)

1. Physically connect & assign a static IP + DNS record
2. Install the printer on the Print Server (add port → driver → queue)
3. PaperCut auto-discovers the new queue → configure it in PaperCut Admin
4. Deploy the embedded app to the MFP (if using Secure Print Release)
5. Deploy the printer to users via GPO or Intune
6. Test end-to-end: print → hold → release → verify PaperCut logs

## Prerequisites Checklist

* [ ] Physical: MFP is plugged in, powered on, connected to the correct network port/VLAN
* [ ] Network: A static IP has been reserved in DHCP or assigned manually; DNS A-record created
* [ ] Access: You have admin access to the Print Server, PaperCut Admin Console, and the MFP web interface
* [ ] Driver: The correct print driver is downloaded (PCL6 or PS — check PaperCut compatibility matrix)
* [ ] PaperCut: Application server is running and healthy → Admin at `https://printserver:9191/admin`

***

## Step-by-Step Procedure

### Phase 1: Physical & Network Setup

| Step | Action                                                                            | Verification                                           |
| ---- | --------------------------------------------------------------------------------- | ------------------------------------------------------ |
| 1.1  | Unbox MFP, connect power and Ethernet to the designated network port              | Power LED on, link LED on                              |
| 1.2  | Access MFP web interface (default IP) → assign **static IP** (e.g., `10.1.3.50`)  | `ping 10.1.3.50` responds                              |
| 1.3  | Set the MFP hostname (e.g., `PRN-L3-C5560i`) on the device panel or web UI        | Hostname appears in MFP status page                    |
| 1.4  | Create a **DNS A record**: `PRN-L3-C5560i.corp.local → 10.1.3.50`                 | `nslookup PRN-L3-C5560i.corp.local` resolves correctly |
| 1.5  | Create a **DNS PTR record** (reverse): `10.1.3.50 → PRN-L3-C5560i.corp.local`     | `nslookup 10.1.3.50` returns hostname                  |
| 1.6  | Configure SNMP community string on MFP (PaperCut uses this for status monitoring) | Note the string for Phase 3                            |
| 1.7  | Set admin password on MFP web interface (do NOT leave default)                    | Can log in with new credentials                        |

> \[!WARNING] **Do not skip the DNS record.** PaperCut and the Print Server both reference printers by hostname. IP-only setups break during IP changes and complicate troubleshooting.

### Phase 2: Print Server Configuration

| Step | Action                                                                                            | Verification                                            |
| ---- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------- |
| 2.1  | On the Print Server, open **Print Management** (`printmanagement.msc`)                            | Console opens showing all existing printers             |
| 2.2  | Right-click **Ports** → Add **Standard TCP/IP Port** → enter `PRN-L3-C5560i.corp.local`           | Port created, status: "OK"                              |
| 2.3  | Right-click **Drivers** → **Add Driver** (if not already installed): select the Canon PCL6 driver | Driver listed in Drivers node                           |
| 2.4  | Right-click **Printers** → **Add Printer** → use the port from 2.2 and driver from 2.3            | Printer appears in Printers node                        |
| 2.5  | **Name the queue**: Follow your naming convention (e.g., `L3-Canon-C5560i` or `Level3-Colour`)    | Queue name is clear and consistent                      |
| 2.6  | **Set printing defaults**: Duplex ON, Colour OFF (B\&W default), A4 paper                         | Printing Preferences reflect defaults                   |
| 2.7  | **Print a test page** from the server                                                             | Test page prints successfully on the MFP                |
| 2.8  | **Share the printer** → set share name (e.g., `L3-Colour`)                                        | `\\printserver\L3-Colour` is accessible from another PC |

**PowerShell alternative for steps 2.2–2.5:**

```powershell
# Add the printer port
Add-PrinterPort -Name "TCP_PRN-L3-C5560i" -PrinterHostAddress "PRN-L3-C5560i.corp.local"

# Add the driver (if not already installed)
Add-PrinterDriver -Name "Canon Generic Plus PCL6"

# Add the printer and share it
Add-Printer -Name "L3-Canon-C5560i" `
    -DriverName "Canon Generic Plus PCL6" `
    -PortName "TCP_PRN-L3-C5560i" `
    -ShareName "L3-Colour" `
    -Shared

# Set defaults (duplex on, B&W)
Set-PrintConfiguration -PrinterName "L3-Canon-C5560i" -DuplexingMode TwoSidedLongEdge
```

### Phase 3: PaperCut Configuration

| Step | Action                                                                                                | Verification                                    |
| ---- | ----------------------------------------------------------------------------------------------------- | ----------------------------------------------- |
| 3.1  | Open PaperCut Admin: `https://printserver:9191/admin`                                                 | Dashboard loads                                 |
| 3.2  | Navigate to **Printers** tab → the new queue should auto-appear (PaperCut monitors the Print Spooler) | `L3-Canon-C5560i` listed with "Enabled" status  |
| 3.3  | If NOT visible → click **Enable Printing** for the new queue                                          | Queue appears with "Enabled" status             |
| 3.4  | Click the printer → **Summary tab** → configure:                                                      |                                                 |
|      | • **Display name**: `Level 3 - Colour Printer`                                                        |                                                 |
|      | • **Location**: `Building A, Level 3, Bay 4`                                                          |                                                 |
|      | • **Cost model**: Select or create (e.g., B\&W: $0.05, Colour: $0.15, Duplex: 50% discount)           |                                                 |
| 3.5  | **Filters & Restrictions** tab → configure:                                                           |                                                 |
|      | • Max pages per job: `100`                                                                            |                                                 |
|      | • Require duplex for jobs > 5 pages                                                                   |                                                 |
|      | • Block colour for jobs > 20 pages (optional)                                                         |                                                 |
| 3.6  | **Advanced Config** → enable SNMP monitoring                                                          | Toner % and paper tray levels show on dashboard |
| 3.7  | **Printer Groups** → add to relevant group (e.g., "Level 3 Printers")                                 | Appears in group                                |

> \[!IMPORTANT] **Cost Model**: If your org uses departmental charge-back, verify the correct cost model is assigned. Wrong cost model = wrong billing = finance escalation. Double-check with your finance/admin team.

### Phase 4: Embedded App — Secure Print Release (If Applicable)

_Skip this phase if your org uses direct printing without hold/release._

| Step | Action                                                                           | Verification                                |
| ---- | -------------------------------------------------------------------------------- | ------------------------------------------- |
| 4.1  | PaperCut Admin → **Enable Devices** → **Add Device**                             | Add device wizard opens                     |
| 4.2  | Enter MFP IP/hostname → select device type (Canon) → test connection             | Device detected and connected               |
| 4.3  | **Deploy embedded application** to the MFP (PaperCut pushes it over the network) | MFP touchscreen shows PaperCut login screen |
| 4.4  | Configure **authentication method** on the device:                               |                                             |
|      | • Badge/Card reader (HID iCLASS, SEOS, Wiegand 26-bit — match your badge system) |                                             |
|      | • PIN code (fallback)                                                            |                                             |
|      | • Username + Password (least preferred)                                          |                                             |
| 4.5  | **Test full flow:**                                                              |                                             |
|      | 1. Print a document from a test PC → job goes to HELD state                      | Job visible in PaperCut "Held Jobs"         |
|      | 2. Walk to MFP → swipe badge → PaperCut shows your held jobs                     | User identified, jobs listed                |
|      | 3. Select job → Release → document prints                                        | Prints correctly                            |
|      | 4. Check PaperCut logs                                                           | Release event logged with timestamp         |

### Phase 5: Deploy Printer to Users

#### Option A: Group Policy (GPO) — Domain-joined devices

| Step  | Action                                                                                         |
| ----- | ---------------------------------------------------------------------------------------------- |
| 5A.1  | Open **Group Policy Management** (`gpmc.msc`)                                                  |
| 5A.2  | Create a new GPO (e.g., `Deploy-L3-Colour-Printer`) or edit an existing printer deployment GPO |
| 5A.3  | Navigate to: `User Configuration → Preferences → Control Panel Settings → Printers`            |
| 5A.4  | Right-click → **New → Shared Printer**                                                         |
| 5A.5  | Action: **Create**                                                                             |
| 5A.6  | Share Path: `\\printserver\L3-Colour`                                                          |
| 5A.7  | Check "Set this printer as the default" (if applicable for this floor)                         |
| 5A.8  | Go to **Common** tab → enable **Item-Level Targeting**                                         |
| 5A.9  | Add targeting: **Security Group** = `SG-Level3-Users` (or target by OU)                        |
| 5A.10 | Link the GPO to the relevant OU                                                                |

**Verification:** On a Level 3 user's PC, run:

```powershell
gpupdate /force
gpresult /h C:\temp\gp-report.html   # Check GPO applied
Get-Printer                            # Confirm printer appears
```

#### Option B: Intune — Entra-joined / Hybrid-joined devices

| Step | Action                                                           |
| ---- | ---------------------------------------------------------------- |
| 5B.1 | Create a PowerShell script to add the printer connection         |
| 5B.2 | Package as a **Platform Script** or **Win32 App** in Intune      |
| 5B.3 | Assign to the device group for Level 3 machines                  |
| 5B.4 | Alternatively, use **Universal Print** if you have the licensing |

```powershell
# Intune deployment script example
$printerPath = "\\printserver.corp.local\L3-Colour"
if (-not (Get-Printer | Where-Object { $_.Name -like "*L3-Colour*" })) {
    Add-Printer -ConnectionName $printerPath
    Write-Output "Printer $printerPath added successfully"
} else {
    Write-Output "Printer $printerPath already exists"
}
```

#### Option C: Manual / Self-Service

| Step | Action                                                          |
| ---- | --------------------------------------------------------------- |
| 5C.1 | User opens `\\printserver` in File Explorer                     |
| 5C.2 | Double-clicks `L3-Colour` to connect                            |
| 5C.3 | Or use PaperCut's **Mobility Print** for self-service discovery |

***

### Phase 6: End-to-End Verification

Run through every check before closing the task:

| #  | Check                               | How to Verify                                 | Expected Result                          | ✅ |
| -- | ----------------------------------- | --------------------------------------------- | ---------------------------------------- | - |
| 1  | Printer pingable by hostname        | `ping PRN-L3-C5560i.corp.local`               | Reply received                           | ☐ |
| 2  | Printer visible in Print Management | `printmanagement.msc` → Printers              | Listed, status: Ready                    | ☐ |
| 3  | Printer visible in PaperCut         | Admin → Printers                              | Listed, status: Online                   | ☐ |
| 4  | User can see the printer            | `Get-Printer` on user PC                      | `L3-Colour` listed                       | ☐ |
| 5  | Test print completes                | Print test page from user PC                  | Page prints correctly                    | ☐ |
| 6  | PaperCut tracks the job             | Admin → Logs → Recent Print Logs              | Job logged: user, pages, colour/BW, cost | ☐ |
| 7  | Quota deducted correctly            | Admin → Users → \[user] → Transaction History | Balance reduced by correct cost          | ☐ |
| 8  | Secure release works                | Print → walk to MFP → swipe badge → release   | Job prints only after badge swipe        | ☐ |
| 9  | SNMP status reporting               | Admin → Printers → Status                     | Toner levels, paper status visible       | ☐ |
| 10 | Cost model correct                  | Print 1 B\&W page + 1 colour page             | Charges match expected cost model        | ☐ |

***

## What Can Go Wrong

| # | Problem                                     | Likely Cause                                                        | Diagnostic Command                                          | Fix                                                                  |
| - | ------------------------------------------- | ------------------------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------- |
| 1 | Printer doesn't auto-appear in PaperCut     | Print Provider not monitoring the queue                             | Check PaperCut Admin → Printers list                        | Click "Enable Printing" manually for the queue                       |
| 2 | "Access Denied" when users connect to share | Share permissions or NTFS permissions too restrictive               | `Get-SmbShareAccess -Name "L3-Colour"`                      | Grant "Print" permission to Domain Users or the target group         |
| 3 | Jobs print but PaperCut doesn't log them    | Print Provider not intercepting the spooler                         | Check `[PaperCut]\server\logs\print-provider.log`           | Reinstall Print Provider; verify spooler integration                 |
| 4 | Wrong cost charged to user                  | Incorrect cost model assigned to the printer                        | PaperCut Admin → Printer → Summary → Cost Model             | Change to the correct cost model; adjust past charges if needed      |
| 5 | Driver mismatch / garbled output            | Wrong driver type (PCL vs PS vs vendor-specific)                    | Print test page; check for garbled text or missing graphics | Switch to **Canon Generic Plus PCL6** or the vendor universal driver |
| 6 | GPO doesn't deploy the printer              | GPO not linked to correct OU, or item-level targeting misconfigured | `gpresult /h report.html` on user PC                        | Fix targeting; ensure GPO is linked and not filtered out             |
| 7 | Badge swipe says "Unknown user"             | Card format mismatch (Wiegand 26-bit vs HID iCLASS vs SEOS)         | Check PaperCut → Device → Card reader config                | Match card format setting to your badge system spec                  |
| 8 | Printer offline in PaperCut but pingable    | SNMP community string mismatch                                      | `Test-NetConnection -ComputerName MFP-IP -Port 161` (SNMP)  | Update SNMP string in PaperCut to match MFP config                   |
| 9 | Jobs stuck in queue, never reach MFP        | Port configuration wrong (IP changed, hostname doesn't resolve)     | `nslookup PRN-L3-C5560i.corp.local`                         | Fix DNS record or update port to current IP                          |

***

## Related Questions

* [How do you configure Secure Print Release (Follow-Me Printing)?](../../Print-Management/How-To/configure-secure-print-release.md)
* [How do you deploy printers via Group Policy?](../../Print-Management/How-To/deploy-printer-to-users-via-gpo.md)
* [How do you configure print quotas and cost recovery?](../../Print-Management/How-To/configure-print-quotas-and-restrictions.md)
* [Troubleshoot: PaperCut not tracking print jobs](../../Print-Management/Troubleshoot/papercut-not-tracking-jobs.md)
* [Troubleshoot: Jobs stuck in queue](../../Print-Management/Troubleshoot/jobs-stuck-in-queue.md)
* [Concept: How Windows Print Architecture works](../concepts/how-windows-print-architecture-works.md)
* [Concept: PaperCut Architecture & Components](../concepts/papercut-architecture-and-components.md)

## Notes & Lessons Learned
