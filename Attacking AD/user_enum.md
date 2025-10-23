SMB NULL Session to Pull User List

Using enum4linux
```shell
enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```

Using rpcclient
```shell
rpcclient -U "" -N 172.16.5.5
```

Using CrackMapExec --users Flag
```shell
crackmapexec smb 172.16.5.5 --users
```

Using windapsearch
```shell
./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```

Kerbrute User Enumeration
```shell
kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt 
```

Credentialed Enumeration to Build our User List

Using CrackMapExec with Valid Credentials
```shell
sudo crackmapexec smb 172.16.5.5 -u htb-student -p Academy_student_AD! --users
```







