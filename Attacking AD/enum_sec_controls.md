Checking the Status of Defender with Get-MpComputerStatus
```powershell
Get-MpComputerStatus
```

Organizations also often focus on blocking the `PowerShell.exe` executable, but forget about the other PowerShell executable locations such as `%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe` or `PowerShell_ISE.exe`. We can see that this is the case in the AppLocker rules shown below. All Domain Users are disallowed from running the 64-bit PowerShell executable located at:

`%SystemRoot%\system32\WindowsPowerShell\v1.0\powershell.exe`

Using Get-AppLockerPolicy cmdlet
```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

Enumerating Language Mode
```powershell
$ExecutionContext.SessionState.LanguageMode
```

Using Find-LAPSDelegatedGroups
```powershell
Find-LAPSDelegatedGroups
```

Using Find-AdmPwdExtendedRights
```powershell
Find-AdmPwdExtendedRights
```

Using Get-LAPSComputers
```powershell
Get-LAPSComputers
```









