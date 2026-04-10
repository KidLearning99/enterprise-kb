# Troubleshoot Template

## Metadata

| Field             | Value                                      |
| ----------------- | ------------------------------------------ |
| Category          | \[Technology Area] / Troubleshoot          |
| Severity          | P1-Critical / P2-High / P3-Medium / P4-Low |
| Last Verified     | YYYY-MM-DD                                 |
| Sources           | SRC-XXX, SRC-YYY                           |
| Related Incidents | INC-XXXXX (if applicable)                  |

## The Symptom

> Describe exactly what the user reports and what you observe. Be specific: error codes, error messages, screenshots if possible.
>
> Example: "User reports 'Document failed to print' popup. In PaperCut Admin, the printer shows 'Online' with a green status. Print queue on the server shows jobs accumulating with 'Error - Printing' status."

## Quick Fix (If One Exists)

```powershell
# Quick fix command (if applicable)
```

## Impact Assessment

| Question                 | Answer                                         |
| ------------------------ | ---------------------------------------------- |
| How many users affected? | One user / One floor / All users               |
| Which systems affected?  | Specific printer / All printers / Specific app |
| Is there a workaround?   | Yes: \[describe] / No                          |
| Business impact?         | Low / Medium / High / Critical                 |

## Root Cause Analysis Tree

```
Symptom: [The symptom]
│
├── Is the printer pingable?
│   ├── NO → Network issue
│   │   ├── Check cable/port
│   │   ├── Check VLAN assignment
│   │   └── Check firewall rules
│   └── YES → Continue
│       │
│       ├── Is the Print Spooler running?
│       │   ├── NO → Start Spooler service
│       │   └── YES → Continue
│       │       │
│       │       ├── Is the port correct?
│       │       │   ├── NO → Fix port config
│       │       │   └── YES → Driver issue or device fault
│       │       ...
```

## Diagnostic Steps

| Step | What to Check        | Command / Action                                      | What It Tells You                          |
| ---- | -------------------- | ----------------------------------------------------- | ------------------------------------------ |
| 1    | Network connectivity | `Test-NetConnection -ComputerName PRINTER -Port 9100` | Is the device reachable on the print port? |
| 2    | Service status       | `Get-Service Spooler`                                 | Is the core print service running?         |
| 3    | Queue status         | `Get-PrintJob -PrinterName "QUEUE-NAME"`              | Are jobs stuck? What's the error status?   |
| 4    | Application logs     | Check `server.log` in PaperCut logs folder            | Any errors from PaperCut?                  |
| 5    | Event logs           | Event Viewer → PrintService → Operational             | OS-level print errors?                     |

## Resolution Options

### Fix A: \[Most Common Fix]

**When to use**: \[Condition that indicates this is the right fix]

| Step | Action |
| ---- | ------ |
| 1    | ...    |
| 2    | ...    |

### Fix B: \[If Fix A Didn't Work]

**When to use**: \[Condition that indicates this is the right fix]

| Step | Action |
| ---- | ------ |
| 1    | ...    |
| 2    | ...    |

### Fix C: \[Edge Case / Escalation]

**When to use**: \[Condition — this is the "call the vendor" scenario]

| Step | Action |
| ---- | ------ |
| 1    | ...    |
| 2    | ...    |

## Prevention

* Monitoring: Set up alert for \[specific condition]
* Configuration: Change \[setting] to prevent recurrence
* Process: Add \[check] to the maintenance checklist

## Related Questions

* [Related How-To](../How-To/related-entry.md)
* [Related Troubleshoot](../_Templates/related-entry.md)
* [Related Concept](../Concepts/related-entry.md)

## Incident History

| Date       | Affected | Root Cause | Resolution | Time to Fix |
| ---------- | -------- | ---------- | ---------- | ----------- |
| YYYY-MM-DD | ...      | ...        | ...        | X min       |
