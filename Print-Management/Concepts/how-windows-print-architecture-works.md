# How Windows Print Architecture Works

<!-- TODO: Fill in from your company KB and Microsoft docs -->
<!-- Use the concept template: Enterprise-KB\_Templates\concept-template.md -->

## Metadata

| Field | Value |
|---|---|
| Category | Print Management / Concept |
| Last Updated | 2026-04-10 |
| Sources | *(Add source IDs)* |

## One-Line Summary

> Windows Print Architecture consists of the Print Spooler service, print processors, port monitors, and drivers — working together to receive a print job from an application, render it, and send it to a physical device.

## How It Works

<!-- TODO: Document the Windows print pipeline:
  1. Application → GDI/XPS → Print Processor
  2. Print Processor → RAW/EMF spool file
  3. Spooler → Port Monitor → Device
  
  Cover:
  - V3 vs V4 print drivers
  - Direct printing vs spooled printing
  - Type 3 vs Type 4 drivers
  - Point and Print security implications (PrintNightmare)
-->

## Key Facts to Remember

- The Print Spooler service (`Spooler`) is the core — if it crashes, ALL printing stops
- Print jobs are stored as spool files in `C:\Windows\System32\spool\PRINTERS\`
- PrintNightmare (CVE-2021-34527) changed the security model — Point and Print now requires admin by default
- V4 drivers are sandboxed and more secure than V3 drivers

## How This Connects to Other Systems

### Used By (Downstream)
- [PaperCut Architecture & Components](./papercut-architecture-and-components.md)
- [How-To: Set up a brand new printer](../How-To/setup-brand-new-printer-on-papercut.md)
- [Troubleshoot: Jobs stuck in queue](../Troubleshoot/jobs-stuck-in-queue.md)
