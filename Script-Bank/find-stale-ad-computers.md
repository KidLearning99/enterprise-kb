# Find and Remove Stale AD Computers

## Metadata

| Field | Value |
|---|---|
| Category | Script Bank |
| Language | PowerShell |
| Requirements | ActiveDirectory Module, Domain Admin Privileges |

## Purpose
Active Directory gets messy quickly when computers are decommissioned, re-imaged, or broken without being cleanly unjoined. This script identifies any computer account that hasn't authenticated to the domain in over 90 days and moves them to a specified "Disabled" OU to safely take them out of production without deleting them outright.

## The Script

```powershell
<#
.SYNOPSIS
    Finds stale computer accounts and disables them.
.DESCRIPTION
    Queries the domain for computers whose PasswordLastSet date is older than X days.
    It automatically disables the account and moves it to a quarantine OU.
#>

Import-Module ActiveDirectory

$DaysInactive = 90
$CutoffDate = (Get-Date).AddDays(-$DaysInactive)
$QuarantineOU = "OU=Stale_Computers,DC=corp,DC=local" # UPDATE THIS TO YOUR DOMAIN'S OU

Write-Host "Finding computers that haven't checked in since $CutoffDate..." -ForegroundColor Cyan

# Find stale computers
$StaleComputers = Get-ADComputer -Filter {PasswordLastSet -lt $CutoffDate} -Properties PasswordLastSet

if ($StaleComputers.Count -eq 0) {
    Write-Host "✅ Active Directory is clean. No stale computers found." -ForegroundColor Green
    Exit
}

Write-Host "Found $($StaleComputers.Count) stale computers. Processing..." -ForegroundColor Yellow

foreach ($PC in $StaleComputers) {
    try {
        # 1. Disable the Computer Account
        Disable-ADAccount -Identity $PC.DistinguishedName
        
        # 2. Move to Quarantine OU
        Move-ADObject -Identity $PC.DistinguishedName -TargetPath $QuarantineOU
        
        Write-Host "Quarantined: $($PC.Name) (Last Logon: $($PC.PasswordLastSet))" -ForegroundColor Green
    }
    catch {
        Write-Host "FAILED to process $($PC.Name): $_" -ForegroundColor Red
    }
}

Write-Host "Hygiene cleanup complete!" -ForegroundColor Cyan
```

## How to Use
1. **Critical:** Update the `$QuarantineOU` variable to match the exact Distinguished Name (DN) of an Organizational Unit in your Domain where you throw away old computer objects.
2. Run the script as a Domain Administrator.
3. If a computer magically comes back online (e.g. from a closet), it will initially be blocked. You can easily drag it back to the original OU and right-click -> Enable Account.
