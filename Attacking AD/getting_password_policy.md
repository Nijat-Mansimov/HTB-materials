Enumerating the Password Policy - from Linux - Credentialed
```shell
crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```

Enumerating the Password Policy - from Linux - SMB NULL Sessions
```shell
rpcclient -U "" -N 172.16.5.5
```

Obtaining the Password Policy using rpcclient
```shell
rpcclient $> querydominfo

Domain:		INLANEFREIGHT
Server:		
Comment:	
Total Users:	3650
Total Groups:	0
Total Aliases:	37
Sequence No:	1
Force Logoff:	-1
Domain Server State:	0x1
Server Role:	ROLE_DOMAIN_PDC
Unknown 3:	0x1
rpcclient $> getdompwinfo
min_password_length: 8
password_properties: 0x00000001
	DOMAIN_PASSWORD_COMPLEX
```

Tool	Ports
nmblookup	137/UDP
nbtstat	137/UDP
net	139/TCP, 135/TCP, TCP and UDP 135 and 49152-65535
rpcclient	135/TCP
smbclient	445/TCP

Using enum4linux
```shell
enum4linux -P 172.16.5.5
```

Enumerating Null Session - from Windows
Establish a null session from windows

```cmd
net use \\DC01\ipc$ "" /u:""
```

Enumerating the Password Policy - from Linux - LDAP Anonymous Bind
Using ldapsearch
```shell
ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength
```

Enumerating the Password Policy - from Windows
```powershell
net accounts
```

Using PowerView
```powershell
import-module .\PowerView.ps1
```
```powershell
Get-DomainPolicy
```

















