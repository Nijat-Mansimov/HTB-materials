Windows CMD - Net Use
```shell
net use n: \\192.168.220.129\Finance
```
```shell
net use n: \\192.168.220.129\Finance /user:plaintext Password123
```

```shell
dir n:\*cred* /s /b
```

```shell
findstr /s /i cred n:\*.*
```

Windows PowerShell
```powershell
Get-ChildItem \\192.168.220.129\Finance\
```

```powershell
New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem"
```

```powershell
PS C:\htb> $username = 'plaintext'
PS C:\htb> $password = 'Password123'
PS C:\htb> $secpassword = ConvertTo-SecureString $password -AsPlainText -Force
PS C:\htb> $cred = New-Object System.Management.Automation.PSCredential $username, $secpassword
PS C:\htb> New-PSDrive -Name "N" -Root "\\192.168.220.129\Finance" -PSProvider "FileSystem" -Credential $cred
```

```powershell
PS C:\htb> N:
PS N:\> (Get-ChildItem -File -Recurse | Measure-Object).Count
```

```powershell
Get-ChildItem -Recurse -Path N:\ -Include *cred* -File
```

```powershell
Get-ChildItem -Recurse -Path N:\ | Select-String "cred" -List
```

Linux - Mount
```shell
nijatmansimov@htb[/htb]$ sudo mkdir /mnt/Finance
nijatmansimov@htb[/htb]$ sudo mount -t cifs -o username=plaintext,password=Password123,domain=. //192.168.220.129/Finance /mnt/Finance
```

```shell
find /mnt/Finance/ -name *cred*
```

```shell
grep -rn /mnt/Finance/ -ie cred
```

Email
```shell
sudo apt-get install evolution
```

<img width="1902" height="1064" alt="image" src="https://github.com/user-attachments/assets/d75a57f4-d532-4389-b241-a4360875bb58" />

<img width="1027" height="493" alt="image" src="https://github.com/user-attachments/assets/da32d08b-88eb-443a-b410-001669dfe0e7" />





















