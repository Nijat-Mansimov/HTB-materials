Discover Modules
```powershell
Get-Module
```

Load ActiveDirectory Module
```powershell
Import-Module ActiveDirectory
```
```powershell
Get-Moduley
```

Get Domain Info
```powershell
Get-ADDomain
```

Get-ADUser
```powershell
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName
```

Checking For Trust Relationships
```powershell
Get-ADTrust -Filter *
```

Group Enumeration
```powershell
Get-ADGroup -Filter * | select name
```

Detailed Group Info
```powershell
Get-ADGroup -Identity "Backup Operators"
```

Group Membership
```powershell
Get-ADGroupMember -Identity "Backup Operators"
```

Domain User Information
```powershell
Get-DomainUser -Identity mmorgan -Domain inlanefreight.local | Select-Object -Property name,samaccountname,description,memberof,whencreated,pwdlastset,lastlogontimestamp,accountexpires,admincount,userprincipalname,serviceprincipalname,useraccountcontrol
```

Recursive Group Membership
```powershell
Get-DomainGroupMember -Identity "Domain Admins" -Recurse
```

Trust Enumeration
```powershell
Get-DomainTrustMapping
```

Testing for Local Admin Access
```powershell
Test-AdminAccess -ComputerName ACADEMY-EA-MS01
```

Finding Users With SPN Set
```powershell
Get-DomainUser -SPN -Properties samaccountname,ServicePrincipalName
```

## SharpView

```powershell
.\SharpView.exe Get-DomainUser -Help
```
```powershell
.\SharpView.exe Get-DomainUser -Identity forend
```

Snaffler in Action

```powershell
.\Snaffler.exe  -d INLANEFREIGHT.LOCAL -s -v data
```

## BloodHound

SharpHound in Action
```powershell
.\SharpHound.exe -c All --zipfilename ILFREIGHT
```












