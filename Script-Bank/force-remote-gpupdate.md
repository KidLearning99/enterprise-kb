# Force Remote Group Policy Update

## Metadata

| Field | Value |
|---|---|
| Category | Script Bank |
| Language | PowerShell |
| Requirements | PowerShell Remoting Enabled (WinRM), Admin Privileges |

## Purpose
When you deploy a critical enterprise policy (like a security fix or a mapped printer), you don't want to wait 90-120 minutes for the fleet to pull down the Group Policy naturally. This script reads a list of computers and explicitly forces them to instantly process new GPOs.

## The Script

```powershell
<#
.SYNOPSIS
    Forces a GPUpdate on a collection of remote computers.
.DESCRIPTION
    Uses Invoke-Command to run gpupdate /force asynchronously across multiple computers.
    It includes a short random delay to prevent saturating the Domain Controller's processing queue.
#>

$ComputerList = @("PC-Finance-01", "PC-Finance-02", "PC-HR-05") # Replace with your list or Get-ADComputer

Write-Host "Initiating remote GPUpdate on $($ComputerList.Count) endpoints..." -ForegroundColor Cyan

Invoke-Command -ComputerName $ComputerList -ScriptBlock {
    # Stagger the execution randomly between 1 and 10 seconds to avoid a SYSVOL storm
    $Delay = Get-Random -Minimum 1 -Maximum 10
    Start-Sleep -Seconds $Delay
    
    Write-Output "[$env:COMPUTERNAME] Executing gpupdate /force"
    # Run gpupdate silently
    gpupdate /force /Target:Both
    
    Write-Output "[$env:COMPUTERNAME] GPUpdate Complete."
} -ErrorAction SilentlyContinue

Write-Host "✅ Network GPUpdate broadcast complete." -ForegroundColor Green
```

## How to Use
1. Replace the `$ComputerList` array with the hostnames you want to target. Alternatively, you can feed from an Active Directory query (e.g. `(Get-ADComputer -Filter * -SearchBase "OU=Workstations,DC=corp,DC=com").Name`).
2. Ensure you run this script as a user with local administrative rights on the target endpoints so WinRM accepts the connection.
3. The script is highly scalable because `Invoke-Command` runs in parallel across up to 32 endpoints simultaneously.
