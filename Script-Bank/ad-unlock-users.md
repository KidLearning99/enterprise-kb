# Find and Unlock AD Accounts

## Metadata

| Field | Value |
|---|---|
| Category | Script Bank |
| Language | PowerShell |
| Requirements | ActiveDirectory Module, Domain Admin Privileges |

## Purpose
This script quickly scans your entire Active Directory domain for any user accounts that are currently locked out (usually due to bad password attempts) and gives you an interactive grid to select which ones to unlock.

## The Script

```powershell
<#
.SYNOPSIS
    Finds and unlocks locked-out Active Directory accounts.
.DESCRIPTION
    This script queries AD for all locked-out accounts, displays them in a grid view,
    and unlocks whichever accounts the administrator selects.
#>

Import-Module ActiveDirectory

Write-Host "Scanning Active Directory for locked-out accounts..." -ForegroundColor Cyan

# Find all locked out accounts
$LockedAccounts = Search-ADAccount -LockedOut | Select-Object Name, SamAccountName, ObjectClass

if ($LockedAccounts.Count -eq 0) {
    Write-Host "✅ No locked out accounts found on the domain!" -ForegroundColor Green
    Exit
}

# Pipe to an interactive GUI grid where you can select the users
$AccountsToUnlock = $LockedAccounts | Out-GridView -Title "Select Accounts to Unlock (Hold CTRL to select multiple)" -PassThru

if ($AccountsToUnlock) {
    foreach ($Account in $AccountsToUnlock) {
        Unlock-ADAccount -Identity $Account.SamAccountName
        Write-Host "Unlocked account: $($Account.SamAccountName)" -ForegroundColor Green
    }
} else {
    Write-Host "Operation cancelled. No accounts were unlocked." -ForegroundColor Yellow
}
```

## How to Use
1. Open PowerShell as Administrator on a machine with RSAT (Remote Server Administration Tools) installed.
2. Run the script.
3. A graphical window (`Out-GridView`) will appear showing only locked out users.
4. Click on the user(s) you want to unlock and hit `OK` in the bottom right corner.
