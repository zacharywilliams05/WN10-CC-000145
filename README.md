# WN10-CC-000145
STIG Implementation WN10-CC-000145

<#
.SYNOPSIS
    This PowerShell script ensures that authentication is required on resume from sleep (on battery).

.NOTES
    Author          : Zachary Williams
    LinkedIn        : linkedin.com/in/zacharywilliams05/
    GitHub          : github.com/zacharywilliams05
    Date Created    : 2025-08-19
    Last Modified   : 2024-08-19
    Version         : 1.0
    CVEs            : N/A
    Plugin IDs      : N/A
    STIG-ID         : WN10-CC-000145

.TESTED ON
    Date(s) Tested  : 
    Tested By       : 
    Systems Tested  : 
    PowerShell Ver. : 

.USAGE
    PS C:\> .\__remediation_template(STIG-ID-WN10-CC-000145).ps1 
#>

```powershell
# Define the registry path and value
$registryPath = "HKLM:\SOFTWARE\Policies\Microsoft\Power\PowerSettings\0e796bdb-100d-47d6-a2d5-f7d2daa51f51"
$valueName = "DCSettingIndex"
$valueData = 1  # Value to set (dword:00000001)

# Check if the registry path exists
if (-not (Test-Path $registryPath)) {
    # Create the registry path
    New-Item -Path $registryPath -Force | Out-Null
    Write-Host "The registry path '$registryPath' has been created."
}

# Check if the value exists
$existingValue = Get-ItemProperty -Path $registryPath -Name $valueName -ErrorAction SilentlyContinue

if ($null -eq $existingValue) {
    # Set the value if it does not exist
    New-ItemProperty -Path $registryPath -Name $valueName -Value $valueData -PropertyType DWord -Force
    Write-Host "The registry value '$valueName' has been created with a value of $valueData."
} else {
    Write-Host "The registry value '$valueName' already exists with a value of $($existingValue.$valueName)."
}
```
