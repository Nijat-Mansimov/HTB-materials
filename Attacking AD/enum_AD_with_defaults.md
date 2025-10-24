Basic Enumeration Commands
```powershell
hostname
[System.Environment]::OSVersion.Version
wmic qfe get Caption,Description,HotFixID,InstalledOn
ipconfig /all
set
echo %USERDOMAIN%
echo %logonserver%
systeminfo
```

Quick WMI checks
```powershell
wmic qfe get Caption,Description,HotFixID,InstalledOn
wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List
wmic process list /format:list
wmic ntdomain list /format:list
wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress
wmic useraccount list /format:list
wmic group list /format:list
wmic sysaccount list /format:list
```

Searching for Domain Controllers
```powershell
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```

Getting disabled users and their descriptions
```powershell
Get-ADUser -Filter 'Enabled -eq $False' -Properties Description | Select-Object Name, Description
```

