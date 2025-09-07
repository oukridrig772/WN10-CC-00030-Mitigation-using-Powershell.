# WN10-CC-00030-Mitigation-using-Powershell.
##To prevent Internet Control Message Protocol (ICMP) redirects from overriding Open Shortest Path First (OSPF) generated routes on a Windows system, you need to disable a specific setting in the Group Policy Editor or the Windows Registry. This is a common security best practice to prevent malicious actors from using ICMP redirects to manipulate a host's routing table, which could lead to man-in-the-middle attacks.

# Set the registry path
$registryPath = "HKLM:\System\CurrentControlSet\Services\Tcpip\Parameters"

# Set the property name
$propertyName = "EnableICMPRedirect"

# Set the value to 0 to disable ICMP redirects
$propertyValue = 0

# Check if the registry key exists and set the value
if ((Get-ItemProperty -Path $registryPath -Name $propertyName -ErrorAction SilentlyContinue) -ne $null) {
    # The key exists, so we will set the value
    try {
        Set-ItemProperty -Path $registryPath -Name $propertyName -Value $propertyValue -Type DWORD
        Write-Host "Successfully set $($propertyName) to $($propertyValue) in the registry." -ForegroundColor Green
    }
    catch {
        Write-Host "An error occurred while setting the registry value: $_" -ForegroundColor Red
    }
} else {
    # The key does not exist, so we will create it and set the value
    try {
        New-ItemProperty -Path $registryPath -Name $propertyName -Value $propertyValue -PropertyType DWORD
        Write-Host "Successfully created and set $($propertyName) to $($propertyValue) in the registry." -ForegroundColor Green
    }
    catch {
        Write-Host "An error occurred while creating the registry value: $_" -ForegroundColor Red
    }
}
