## Windows Autentifikasiya Prosesi

Windows müştəri autentifikasiya prosesi loqin, etimadnamənin əldə edilməsi və yoxlanılması üçün məsuliyyət daşıyan bir çox modulu əhatə edir. Windows-da müxtəlif autentifikasiya mexanizmləri arasında **Kerberos** ən geniş istifadə edilən və mürəkkəb protokollardan biridir. **Yerli Təhlükəsizlik Orqanı (LSA)** istifadəçiləri autentifikasiya edən, yerli loqinləri idarə edən, yerli təhlükəsizliyin bütün aspektlərinə nəzarət edən və istifadəçi adları ilə təhlükəsizlik identifikatorları (SID-lər) arasında tərcümə xidmətləri göstərən qorunan bir alt sistemdir.

Təhlükəsizlik alt sistemi bir kompüter sistemində təhlükəsizlik siyasətlərini və istifadəçi hesablarını saxlayır. Domen Kontrollerində (Domain Controller), bu siyasətlər və hesablar bütün domenə aiddir və **Active Directory**-də saxlanılır. Əlavə olaraq, LSA alt sistemi giriş nəzarəti, icazə yoxlamaları və təhlükəsizlik audit mesajlarının yaradılması üçün xidmətlər təmin edir.

### Windows Autentifikasiya Prosesi Diaqramı

<img width="2480" height="1424" alt="image" src="https://github.com/user-attachments/assets/56471f43-ea46-4b39-9174-e7a78ff2f30c" />

Yerli interaktiv loqin bir neçə komponentin koordinasiyası ilə idarə olunur: loqin prosesi (**WinLogon**), loqin istifadəçi interfeysi prosesi (**LogonUI**), **etimadnamə təminatçıları (credential providers)**, **Yerli Təhlükəsizlik Orqanının Alt Sistem Xidməti (LSASS)**, bir və ya daha çox **autentifikasiya paketi** və ya **Təhlükəsizlik Hesabları İdarəçisi (SAM)**, ya da **Active Directory**. Bu kontekstdə autentifikasiya paketləri autentifikasiya yoxlamalarını yerinə yetirmək üçün məsuliyyət daşıyan **Dinamik-Əlaqə Kitabxanaları (DLL-lər)**-dır. Məsələn, domenə qoşulmamış (non-domain-joined) və interaktiv loqinlər üçün adətən **Msv1_0.dll** autentifikasiya paketi istifadə olunur.

**WinLogon** təhlükəsizliklə əlaqəli istifadəçi qarşılıqlı əlaqələrini idarə edən etibarlı sistem prosesidir, məsələn:
* Loqin zamanı etimadnamələr üçün tələb irəli sürmək məqsədilə **LogonUI**-ni işə salmaq
* Şifrə dəyişikliklərini idarə etmək
* İş stansiyasını kilidləmək və kilidini açmaq

İstifadəçinin hesab adını və şifrəsini əldə etmək üçün WinLogon sistemdə quraşdırılmış etimadnamə təminatçılarına (credential providers) güvənir. Bu təminatçılar DLL-lər kimi tətbiq edilən COM obyektləridir.

WinLogon, **Win32k.sys**-dən RPC mesajları vasitəsilə göndərilən klaviaturadan loqin tələblərini tutan yeganə prosesdir. Loqin zamanı dərhal qrafik istifadəçi interfeysini təqdim etmək üçün **LogonUI** tətbiqini işə salır. İstifadəçinin etimadnamələri etimadnamə təminatçısı tərəfindən toplandıqdan sonra, WinLogon istifadəçini autentifikasiya etmək üçün onları **Yerli Təhlükəsizlik Orqanının Alt Sistem Xidmətinə (LSASS)** ötürür.

### LSASS

**Yerli Təhlükəsizlik Orqanının Alt Sistem Xidməti (LSASS)** bir çox moduldan ibarətdir və bütün autentifikasiya proseslərini idarə edir. Fayl sistemində **%SystemRoot%\System32\Lsass.exe** ünvanında yerləşən bu xidmət yerli təhlükəsizlik siyasətini tətbiq etmək, istifadəçiləri autentifikasiya etmək və təhlükəsizlik audit jurnallarını **Event Log**-a ötürmək üçün məsuliyyət daşıyır. Əslində, LSASS Windows əsaslı əməliyyat sistemlərində qapıçı rolunu oynayır.

| Autentifikasiya Paketləri | Təsvir |
| :--- | :--- |
| **Lsasrv.dll** | LSA Server xidməti həm təhlükəsizlik siyasətlərini tətbiq edir, həm də LSA üçün təhlükəsizlik paketi idarəçisi kimi çıxış edir. LSA, hansı protokolun uğurlu olacağını müəyyən etdikdən sonra **NTLM** və ya **Kerberos** protokolunu seçən **Negotiate** funksiyasını ehtiva edir. |
| **Msv1_0.dll** | Xüsusi autentifikasiya tələb etməyən yerli maşın loqinləri üçün autentifikasiya paketi. |
| **Samsrv.dll** | **Təhlükəsizlik Hesabları İdarəçisi (SAM)** yerli təhlükəsizlik hesablarını saxlayır, yerli olaraq saxlanılan siyasətləri tətbiq edir və API-ləri dəstəkləyir. |
| **Kerberos.dll** | Maşında Kerberos əsaslı autentifikasiya üçün LSA tərəfindən yüklənən təhlükəsizlik paketi. |
| **Netlogon.dll** | Şəbəkə əsaslı loqin xidməti. |
| **Ntdsa.dll** | Bu kitabxana Windows registriyyasında yeni qeydlər və qovluqlar yaratmaq üçün istifadə olunur. |
| Mənbə: Microsoft Docs. | |

Hər bir interaktiv loqin sessiyası **WinLogon** xidmətinin ayrıca bir nümunəsini yaradır. **Qrafik İdentifikasiya və Autentifikasiya (GINA)** arxitekturası WinLogon tərəfindən istifadə olunan proses sahəsinə yüklənir, etimadnamələri qəbul edir və işləyir, və **LSALogonUser** funksiyası vasitəsilə autentifikasiya interfeyslərini çağırır.

### SAM verilənlər bazası

**Təhlükəsizlik Hesabları İdarəçisi (SAM)** Windows əməliyyat sistemlərində istifadəçi hesabı etimadnamələrini saxlayan verilənlər bazası faylıdır. Həm yerli, həm də uzaq istifadəçiləri autentifikasiya etmək üçün istifadə olunur və icazəsiz girişi önləmək üçün kriptoqrafik qorumalardan istifadə edir. İstifadəçi şifrələri registriyyada adətən **LM** və ya **NTLM heşləri** şəklində saxlanılır. SAM faylı **%SystemRoot%\system32\config\SAM** ünvanında yerləşir və **HKLM\SAM** altında montaj edilir. Bu fayla baxmaq və ya daxil olmaq **SYSTEM** səviyyəsində imtiyazlar tələb edir.

Windows sistemləri quraşdırma zamanı ya iş qrupuna (*workgroup*), ya da domenə təyin edilə bilər. Əgər sistem iş qrupuna təyin edilibsə, o, SAM verilənlər bazasını yerli olaraq idarə edir və bütün mövcud istifadəçiləri bu verilənlər bazasında yerli olaraq saxlayır. Lakin, əgər sistem domenə qoşulubsa, **Domen Kontrolleri (DC)** etimadnamələri **Active Directory** verilənlər bazasından (**ntds.dit**) təsdiqləməlidir ki, bu da **%SystemRoot%\ntds.dit** ünvanında saxlanılır.

SAM verilənlər bazasının oflayn sındırılmasına qarşı qorumaları yaxşılaşdırmaq üçün Microsoft Windows NT 4.0-da **SYSKEY (syskey.exe)** adlı bir xüsusiyyət təqdim etdi. Aktivləşdirildikdə, SYSKEY SAM faylını diskdə qismən şifrələyir və bütün yerli hesablar üçün şifrə heşlərinin sistem tərəfindən yaradılan açarla şifrələnməsini təmin edir.

### Credential Manager

<img width="749" height="716" alt="image" src="https://github.com/user-attachments/assets/8be677d9-727f-4cbf-bd1c-c091fe72b402" />

**Credential Manager** bütün Windows əməliyyat sistemlərinin daxili xüsusiyyətidir ki, istifadəçilərə şəbəkə resurslarına, veb-saytlara və tətbiqlərə daxil olmaq üçün istifadə olunan etimadnamələri saxlamağa və idarə etməyə imkan verir. Bu saxlanılan etimadnamələr istifadəçinin **Etimadnamə Şkafında (Credential Locker)**, istifadəçi profili üzrə şifrələnmiş şəkildə aşağıdakı yerdə saxlanılır:

$\text{PS C:\Users\[Username]\AppData\Local\Microsoft\[Vault/Credentials]\}$

Credential Manager vasitəsilə saxlanılan etimadnamələri deşifrə etmək üçün müxtəlif üsullar mövcuddur. Biz bu modulda bu üsulların bəziləri ilə praktiki məşğul olacağıq.

### NTDS

Windows sistemlərinin Windows domenlərinə qoşulduğu şəbəkə mühitləri ilə qarşılaşmaq çox yaygındır. Bu quraşdırma mərkəzləşdirilmiş idarəetməni sadələşdirir, administratorlara təşkilat daxilindəki bütün sistemlərə səmərəli nəzarət etməyə imkan verir. Belə mühitlərdə, loqin tələbləri eyni Active Directory meşəsi daxilindəki Domen Kontrollerlərinə göndərilir. Hər bir Domen Kontrolleri **Read-Only Domain Controller (RODC)** istisna olmaqla, bütün Domen Kontrollerləri arasında sinxronlaşdırılan **NTDS.dit** adlı bir faylı saxlayır.

**NTDS.dit** aşağıdakılar daxil olmaqla, lakin bunlarla məhdudlaşmayaraq, Active Directory məlumatlarını saxlayan bir verilənlər bazası faylıdır:
* İstifadəçi hesabları (istifadəçi adı və şifrə heşi)
* Qrup hesabları
* Kompüter hesabları
* Qrup siyasəti obyektləri

Bu modulun davamında biz NTDS.dit faylından etimadnamələrin çıxarılması üsullarını araşdıracağıq.

İndi etimadnamə saxlama konsepsiyaları haqqında ilkin məlumatı nəzərdən keçirdiyimizə görə, gəlin etimadnamələri çıxarmaq və qiymətləndirmələr zamanı girişimizi daha da inkişaf etdirmək üçün edə biləcəyimiz müxtəlif hücumları öyrənək.
