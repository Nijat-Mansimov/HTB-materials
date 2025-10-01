## Windows Təhlükəsizliyi

Təhlükəsizlik Windows əməliyyat sistemlərində **kritik** bir mövzudur. Windows sistemlərində geniş bir hücum səthi yaradan bir çox hərəkətli hissə mövcuddur. Bir çox daxili tətbiqlər, xüsusiyyətlər və tənzimləmə qatları səbəbindən, Windows sistemləri asanlıqla **səhv konfiqurasiya edilə** bilər, bununla da tam yamalı olsalar belə, hücuma açıq qalırlar.

Onun bir çox **sui-istifadə edilə bilən daxili xüsusiyyətləri** var və geniş yayılmış və çox təsirli **uzaqdan və yerli istismarlara** səbəb olan müxtəlif **kritik zəifliklərdən** əziyyət çəkib.

Microsoft illər ərzində Windows təhlükəsizliyini təkmilləşdirib. Dünyamızın qarşılıqlı əlaqəsi genişlənməyə davam etdikcə və hücumçular daha da mürəkkəbləşdikcə, Microsoft sistem administratorlarının sistemləri **möhkəmləndirmək** və müdaxilə və sui-istifadə cəhdlərini aktiv şəkildə **bloklamaq və aşkarlamaq** üçün istifadə edə biləcəyi yeni xüsusiyyətlər əlavə etməyə davam edib.

Windows sistem daxilində **giriş və autentifikasiyanı** nəzarət etmək üçün müəyyən təhlükəsizlik prinsiplərinə əməl edir. Bu prinsiplər müəyyən hərəkətlər üçün səlahiyyət verilə bilən **istifadəçilər**, **şəbəkəyə qoşulmuş kompüterlər**, **yivlər (threads)** və **proseslər** kimi müxtəlif subyektlərə şamil olunur. Təhlükəsizlik modeli icazəsiz giriş riskini minimuma endirmək, hücumçular və ya zərərli proqramlar üçün sistemi istismar etməyi daha çətinləşdirmək üçün nəzərdə tutulub.

-----

## Təhlükəsizlik İdentifikatoru (SID)

Sistemdəki təhlükəsizlik əsaslarının (*security principals*) hər birinin unikal **təhlükəsizlik identifikatoru (SID)** var. Sistem SID-ləri avtomatik olaraq yaradır. Bu o deməkdir ki, məsələn, sistemdə eyni iki istifadəçimiz olsa belə, Windows onları və onların hüquqlarını SID-lərinə əsasən ayırd edə bilər. SID-lər təhlükəsizlik verilənlər bazasında saxlanılan, müxtəlif uzunluqlarda **sətir dəyərlərdir** (*string values*). Bu SID-lər istifadəçinin səlahiyyətli olduğu bütün hərəkətləri müəyyən etmək üçün istifadəçinin **giriş nişanına (access token)** əlavə olunur.

Bir SID **İdentifikator Səlahiyyətindən (Identifier Authority)** və **Nisbi ID-dən (Relative ID - RID)** ibarətdir. **Active Directory (AD)** domen mühitində SID həmçinin **domen SID-ni** də ehtiva edir.

```powershell
PS C:\htb> whoami /user

USER INFORMATION
----------------

User Name           SID
=================== =============================================
ws01\bob            S-1-5-21-674899381-4069889467-2080702030-1002
```

SID bu nümunəyə uyğun olaraq parçalanır:

$$(SID)-(revision\ level)-(identifier-authority)-(subauthority1)-(subauthority2)-(etc)$$

Gəlin SID hissə-hissə təhlil edək.

| Nömrə | Mənası | Təsviri |
| :--- | :--- | :--- |
| **S** | **SID-i müəyyən edir** | Sətri bir SID kimi müəyyən edir. |
| **1** | **Revision Level (Reviziya Səviyyəsi)** | Bu günə qədər heç vaxt dəyişməyib və həmişə **1** olub. |
| **5** | **Identifier-authority (İdentifikator Səlahiyyəti)** | SID-i yaradan səlahiyyəti (kompüteri və ya şəbəkəni) müəyyən edən 48-bitlik bir sətir. |
| **21** | **Subauthority 1 (Alt Səlahiyyət 1)** | Bu, SID tərəfindən təsvir edilən istifadəçinin əlaqəsini və ya qrupunu onu yaradan səlahiyyətlə müəyyən edən dəyişən bir nömrədir. Bu, səlahiyyətin istifadəçi hesabını hansı ardıcıllıqla yaratdığını bildirir. |
| **674899381-4069889467-2080702030** | **Subauthority 2 (Alt Səlahiyyət 2)** | Hesabı hansı kompüterin (və ya domeni) yaratdığını bildirir. |
| **1002** | **Subauthority 3 (Alt Səlahiyyət 3)** | Bir hesabı digərindən fərqləndirən **RID** (Nisbi ID). Bu istifadəçinin normal bir istifadəçi, qonaq, administrator və ya başqa bir qrupun hissəsi olub olmadığını bildirir. |

-----

## Təhlükəsizlik Hesabları İdarəedicisi (SAM) və Giriş Nəzarət Qeydləri (ACE)

**SAM (Security Accounts Manager)** müəyyən prosesləri icra etmək üçün bir şəbəkəyə hüquqlar verir.

Giriş hüquqları özləri **Giriş Nəzarət Siyahılarında (ACL)** olan **Giriş Nəzarət Qeydləri (ACE)** tərəfindən idarə olunur. ACL-lər hansı istifadəçilərin, qrupların və ya proseslərin bir fayla daxil olmaq və ya bir prosesi icra etmək səlahiyyətinə malik olduğunu müəyyən edən **ACE-ləri** ehtiva edir.

Təhlükəsizləşdirilə bilən bir obyektə giriş icazələri, iki növ ACL-ə təsnif edilən **təhlükəsizlik təsvirçisi** (*security descriptor*) tərəfindən verilir: **İxtiyari Giriş Nəzarət Siyahısı (DACL)** və ya **Sistem Giriş Nəzarət Siyahısı (SACL)**. Bir istifadəçi tərəfindən başladılan və ya təşəbbüs göstərilən hər bir yiv və proses **avtorizasiya prosesindən** keçir. Bu prosesin ayrılmaz hissəsi, **Yerli Təhlükəsizlik Səlahiyyəti (LSA)** tərəfindən təsdiqlənən **giriş nişanlarıdır** (*access tokens*). SID-dən əlavə, bu giriş nişanları digər təhlükəsizliklə əlaqəli məlumatları da ehtiva edir. Bu funksionallıqları başa düşmək, imtiyazların artırılması mərhələsində bu təhlükəsizlik mexanizmlərindən necə istifadə edəcəyinizi və onları necə aşacağınızı öyrənməyin əsas hissəsidir.

-----

## İstifadəçi Hesabının Nəzarəti (UAC)

**İstifadəçi Hesabının Nəzarəti (UAC)** zərərli proqram təminatının kompüterə və ya onun məzmununa zərər verə biləcək prosesləri işlətməsini və ya manipulyasiya etməsini əngəlləyən Windows-dakı bir təhlükəsizlik xüsusiyyətidir. UAC-də, administratorun xəbəri olmadan arzuolunmaz proqram təminatının quraşdırılmasının və ya sistem miqyasında dəyişikliklərin edilməsinin qarşısını almaq üçün nəzərdə tutulmuş **Admin Təsdiqləmə Rejimi** (*Admin Approval Mode*) var. Əgər müəyyən bir proqramı quraşdırmısınızsa və sisteminiz onun quraşdırılmasını təsdiqləmək istəyibsə, yəqin ki, **razılıq sorğusunu** (*consent prompt*) görmüsünüz. Quraşdırma administrator hüquqlarını tələb etdiyi üçün bir pəncərə açılır və sizdən quraşdırmanı təsdiqləmək istəyib-istəmədiyinizi soruşur. Quraşdırma üçün hüquqları olmayan standart bir istifadəçi ilə icra rədd ediləcək və ya sizdən administrator şifrəsi tələb olunacaq. Bu razılıq sorğusu, zərərli proqramların və ya hücumçuların icra etməyə çalışdığı skriptlərin və ya ikilik faylların icrasını, istifadəçi şifrəni daxil edənə və ya icranı təsdiqləyənə qədər **dayandırır**. UAC-nin necə işlədiyini başa düşmək üçün onun necə qurulduğunu, necə işlədiyini və razılıq sorğusunu nəyin işə saldığını bilməliyik. Aşağıdakı diaqram, **UAC-nin necə işlədiyini** göstərir .

<img width="2576" height="3808" alt="image" src="https://github.com/user-attachments/assets/9b9717e2-1eb6-44de-888c-0c32f7cf167d" />

## Reyestr (Registry)

**Reyestr (Registry)** Windows əməliyyat sistemi üçün **kritik** olan iyerarxik bir verilənlər bazasıdır. O, **Windows əməliyyat sistemi** və ondan istifadə etməyi seçən **tətbiqlər üçün aşağı səviyyəli tənzimləmələri** saxlayır. Reyestr kompüterə xas və istifadəçiyə xas məlumatlara bölünür. Reyestr Redaktorunu (`Registry Editor`) əmr sətrindən və ya Windows axtarış çubuğundan **regedit** yazaraq aça bilərik.

<img width="1031" height="308" alt="image" src="https://github.com/user-attachments/assets/ea193d3d-d254-493d-b7bc-98091eb08820" />

## Reyestr: Ağac Strukturu və Dəyər Növləri

Reyestrin **ağac strukturu** əsas qovluqlardan (**kök açarları**, *root keys*) ibarətdir ki, bunların daxilində öz girişləri/faylları (**dəyərləri**, *values*) olan alt qovluqlar (**alt açarlar**, *subkeys*) yerləşir. Bir alt açara daxil edilə bilən **11 fərqli dəyər növü** var:

| Dəyər Növü | Təsviri |
| :--- | :--- |
| **REG\_BINARY** | Hər hansı bir formada **ikilik məlumat**. |
| **REG\_DWORD** | **32-bitlik rəqəm**. |
| **REG\_DWORD\_LITTLE\_ENDIAN** | **Kiçik-endian formatında 32-bitlik rəqəm**. Windows, kiçik-endian kompüter arxitekturaları üzərində işləmək üçün nəzərdə tutulub. Buna görə də, bu dəyər Windows başlığı fayllarında **REG\_DWORD** kimi təyin edilir. |
| **REG\_DWORD\_BIG\_ENDIAN** | **Böyük-endian formatında 32-bitlik rəqəm**. Bəzi UNIX sistemləri böyük-endian arxitekturalarını dəstəkləyir. |
| **REG\_EXPAND\_SZ** | Ətraf mühit dəyişənlərinə açılmamış istinadları ehtiva edən (**məsələn, "%PATH%"**) **sıfırla bitən sətir** (*null-terminated string*). Bu, Unicode və ya ANSI funksiyalarından istifadə etməyinizdən asılı olaraq Unicode və ya ANSI sətri olacaq. Ətraf mühit dəyişəni istinadlarını genişləndirmək üçün **ExpandEnvironmentStrings** funksiyasından istifadə edin. |
| **REG\_LINK** | **RegCreateKeyEx** funksiyasını **REG\_OPTION\_CREATE\_LINK** ilə çağıraraq yaradılmış **simvolik keçidin hədəf yolunu** ehtiva edən sıfırla bitən Unicode sətri. |
| **REG\_MULTI\_SZ** | Boş sətirlə (**\0**) bitən **sıfırla bitən sətirlər ardıcıllığı**. Aşağıdakı bir nümunədir: *String1\0String2\0String3\0LastString\0\0*. Birinci **\0** birinci sətri bitirir, sondan ikinci **\0** sonuncu sətri bitirir və son **\0** isə ardıcıllığı bitirir. Qeyd edin ki, son bitirici sətrin uzunluğunda nəzərə alınmalıdır. |
| **REG\_NONE** | Təyin olunmuş dəyər növü yoxdur. |
| **REG\_QWORD** | **64-bitlik rəqəm**. |
| **REG\_QWORD\_LITTLE\_ENDIAN** | **Kiçik-endian formatında 64-bitlik rəqəm**. Windows, kiçik-endian kompüter arxitekturaları üzərində işləmək üçün nəzərdə tutulub. Buna görə də, bu dəyər Windows başlığı fayllarında **REG\_QWORD** kimi təyin edilir. |
| **REG\_SZ** | **Sıfırla bitən sətir**. Bu, Unicode və ya ANSI funksiyalarından istifadə etməyinizdən asılı olaraq ya Unicode, ya da ANSI sətri olacaq. |

---

## Əsas Reyestr Açarları

**Kompüter (Computer)** altında olan hər bir qovluq bir **açar** (*key*) adlanır. Bütün kök açarları **HKEY** ilə başlayır. **HKEY\_LOCAL\_MACHINE** kimi bir açar **HKLM** olaraq qısaldılır. **HKLM** yerli sistem üçün əlaqəli olan bütün tənzimləmələri ehtiva edir. Bu kök açarı, yüklənmə zamanı yaddaşa yüklənən **SAM**, **SECURITY**, **SYSTEM**, **SOFTWARE**, **HARDWARE** və **BCD** kimi altı alt açarı ehtiva edir (dinamik olaraq yüklənən **HARDWARE** istisna olmaqla).

<img width="1027" height="299" alt="image" src="https://github.com/user-attachments/assets/fa536b35-7066-47ea-b224-c2a7e5fd8b78" />

Bütün sistem reyestri əməliyyat sistemində bir neçə faylda saxlanılır. Bu faylları \**C:\\Windows\\System32\\Config\** qovluğunda tapa bilərsiniz.

```powershell
PS C:\htb> ls

    Directory: C:\Windows\system32\config

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         12/7/2019   4:14 AM                Journal
d-----         12/7/2019   4:14 AM                RegBack
d-----         12/7/2019   4:14 AM                systemprofile
d-----         8/12/2020   1:43 AM                TxR
-a----         8/13/2020   6:02 PM        1048576 BBI
-a----         6/25/2020   4:36 PM          28672 BCD-Template
-a----         8/30/2020  12:17 PM       33816576 COMPONENTS
-a----         8/13/2020   6:02 PM         524288 DEFAULT
-a----         8/26/2020   7:51 PM        4603904 DRIVERS
-a----         6/25/2020   3:37 PM          32768 ELAM
-a----         8/13/2020   6:02 PM          65536 SAM
-a----         8/13/2020   6:02 PM          65536 SECURITY
-a----         8/13/2020   6:02 PM       87818240 SOFTWARE
-a----         8/13/2020   6:02 PM       17039360 SYSTEM
```

İstifadəçiyə xas reyestr yuvası (**HKCU**) istifadəçi qovluğunda (yəni, **C:\\Users\<USERNAME\>\\Ntuser.dat**) saxlanılır.

```powershell
PS C:\htb> gci -Hidden

    Directory: C:\Users\bob

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d--h--         6/25/2020   5:12 PM                AppData
d--hsl         6/25/2020   5:12 PM                Application Data
d--hsl         6/25/2020   5:12 PM                Cookies
d--hsl         6/25/2020   5:12 PM                Local Settings
d--h--         6/25/2020   5:12 PM                MicrosoftEdgeBackups
d--hsl         6/25/2020   5:12 PM                My Documents
d--hsl         6/25/2020   5:12 PM                NetHood
d--hsl         6/25/2020   5:12 PM                PrintHood
d--hsl         6/25/2020   5:12 PM                Recent
d--hsl         6/25/2020   5:12 PM                SendTo
d--hsl         6/25/2020   5:12 PM                Start Menu
d--hsl         6/25/2020   5:12 PM                Templates
-a-h--         8/13/2020   6:01 PM        2883584 NTUSER.DAT
-a-hs-         6/25/2020   5:12 PM         524288 ntuser.dat.LOG1
-a-hs-         6/25/2020   5:12 PM        1011712 ntuser.dat.LOG2
-a-hs-         8/17/2020   5:46 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.0.regtrans-ms
-a-hs-         8/17/2020  12:13 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.1.regtrans-ms
-a-hs-         8/17/2020  12:13 PM        1048576 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.2.regtrans-ms
-a-hs-         8/17/2020   5:46 PM          65536 NTUSER.DAT{53b39e87-18c4-11ea-a811-000d3aa4692b}.TxR.blf
-a-hs-         6/25/2020   5:15 PM          65536 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TM.blf
-a-hs-         6/25/2020   5:12 PM         524288 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TMContainer000000000
                                                  00000000001.regtrans-ms
-a-hs-         6/25/2020   5:12 PM         524288 NTUSER.DAT{53b39e88-18c4-11ea-a811-000d3aa4692b}.TMContainer000000000
                                                  00000000002.regtrans-ms
---hs-         6/25/2020   5:12 PM             20 ntuser.ini
```

-----

## Run və RunOnce Reyestr Açarları

Həmçinin, əməliyyat sistemi başladıldıqda və ya istifadəçi daxil olduqda yaddaşa yüklənən proqram təminatını və faylları dəstəkləmək üçün məntiqi açar, alt açar və dəyərlər qrupunu ehtiva edən **reyestr yuvaları** (*registry hives*) da var. Bu yuvalar sistemə daxil olmanı qorumaq üçün faydalıdır. Bunlara **Run və RunOnce reyestr açarları** deyilir.

Windows reyestri aşağıdakı dörd açarı ehtiva edir:

  * **HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Run**
  * **HKEY\_CURRENT\_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\Run**
  * **HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\RunOnce**
  * **HKEY\_CURRENT\_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\RunOnce**

Aşağıda bir sistemə daxil olarkən **HKEY\_LOCAL\_MACHINE\\Software\\Microsoft\\Windows\\CurrentVersion\\Run** açarının nümunəsi verilmişdir.

```powershell
PS C:\htb> reg query HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run
    SecurityHealth    REG_EXPAND_SZ    %windir%\system32\SecurityHealthSystray.exe
    RTHDVCPL    REG_SZ    "C:\Program Files\Realtek\Audio\HDA\RtkNGUI64.exe" -s
    Greenshot    REG_SZ    C:\Program Files\Greenshot\Greenshot.exe
```

Aşağıda bir sistemə daxil olarkən cari istifadəçi altında işləyən tətbiqləri göstərən **HKEY\_CURRENT\_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\Run** açarının nümunəsi verilmişdir.

```powershell
PS C:\htb> reg query HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run

HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
    OneDrive    REG_SZ    "C:\Users\bob\AppData\Local\Microsoft\OneDrive\OneDrive.exe" /background
    OPENVPN-GUI    REG_SZ    C:\Program Files\OpenVPN\bin\openvpn-gui.exe
    Docker Desktop    REG_SZ    C:\Program Files\Docker\Docker\Docker Desktop.exe
```

-----

## Tətbiqlərin Ağ Siyahısı (Application Whitelisting)

**Tətbiqlərin ağ siyahısı** bir sistemdə mövcud olmasına və işləməsinə icazə verilən təsdiqlənmiş proqram tətbiqləri və ya icra edilə bilən faylların siyahısıdır. Məqsəd mühiti zərərli proqramlardan və bir təşkilatın xüsusi biznes ehtiyaclarına uyğun olmayan təsdiqlənməmiş proqram təminatından qorumaqdır. Məcburi bir ağ siyahının tətbiqi, xüsusən də böyük bir şəbəkədə çətin ola bilər. Bir təşkilat, bütün lazımi tətbiqlərin ağ siyahıya salındığından və buraxılma səhvi ilə bloklanmadığından əmin olmaq üçün əvvəlcə ağ siyahını **audit rejimində** tətbiq etməlidir, çünki bu, həll etdiyindən daha çox problem yarada bilər.

Əksinə, **qara siyahı** (*blacklisting*) bloklanacaq zərərli və ya icazə verilməyən proqram/tətbiqlərin siyahısını göstərir və digərlərinin işləməsinə/quraşdırılmasına icazə verilir. **Ağ siyahı** isə **"sıfır etibar"** (*zero trust*) prinsipinə əsaslanır ki, burada xüsusi olaraq icazə verilənlər istisna olmaqla, bütün proqram/tətbiqlər **"pis"** hesab edilir. Ağ siyahının saxlanması ümumiyyətlə daha az əlavə iş tələb edir, çünki bir sistem administratoru yalnız icazə verilənləri göstərməli və daima yeni zərərli tətbiqlərlə bir "qara siyahını" yeniləməməlidir.

Ağ siyahı, xüsusilə yüksək təhlükəsizlik mühitlərində **NIST** kimi təşkilatlar tərəfindən tövsiyə olunur.

### AppLocker

**AppLocker** Microsoft-un tətbiqlərin ağ siyahıya salınması həllidir və ilk dəfə **Windows 7**-də tətbiq edilmişdir. AppLocker sistem administratorlarına istifadəçilərin hansı tətbiqləri və faylları işlədə biləcəyinə nəzarət etmək imkanı verir. O, **icra edilə bilən fayllar**, **skriptlər**, **Windows quraşdırıcı faylları**, **DLL-lər**, **paketlənmiş tətbiqlər** və **paketlənmiş tətbiq quraşdırıcıları** üzərində dəqiq nəzarət verir.

O, nəşriyyatçının adı (rəqəmsal imzadan əldə edilə bilər), məhsul adı, fayl adı və versiyası kimi **fayl atributlarına** əsaslanan qaydalar yaratmağa imkan verir. Qaydalar həmçinin **fayl yollarına** və **heşlərə** əsasən də qurula bilər. Qaydalar biznes ehtiyacından asılı olaraq ya **təhlükəsizlik qruplarına**, ya da **fərdi istifadəçilərə** tətbiq oluna bilər. AppLocker bütün qaydaları məcbur etməzdən əvvəl təsirini yoxlamaq üçün əvvəlcə **audit rejimində** tətbiq edilə bilər.

-----

## Yerli Qrup Siyasəti (Local Group Policy)

**Qrup Siyasəti (Group Policy)** administratorlara müxtəlif tənzimləmələri təyin etməyə, konfiqurasiya etməyə və tənzimləməyə imkan verir. Bir domen mühitində, qrup siyasətləri **Domen Nəzarətçisindən (Domain Controller)** **Qrup Siyasəti Obyektlərinin (GPOs)** əlaqəli olduğu bütün domenə qoşulmuş maşınlara ötürülür. Bu tənzimləmələr həmçinin **Yerli Qrup Siyasətindən (Local Group Policy)** istifadə edilərək fərdi maşınlarda da təyin edilə bilər.

Qrup Siyasəti həm domen mühitlərində, həm də qeyri-domen mühitlərində **yerli olaraq konfiqurasiya edilə** bilər. Yerli Qrup Siyasəti, **Control Panel** vasitəsilə əlçatan olmayan müəyyən qrafik və şəbəkə tənzimləmələrini dəqiqləşdirmək üçün istifadə edilə bilər. O, həmçinin yalnız müəyyən proqramların quraşdırılmasına/işlədilməsinə icazə vermək və ya ciddi istifadəçi hesabı şifrə tələblərini məcbur etmək kimi sərt təhlükəsizlik tənzimləmələri ilə fərdi kompüter siyasətini **kilidləmək** üçün də istifadə edilə bilər.

Biz **Start menyusunu** açıb **gpedit.msc** yazaraq **Yerli Qrup Siyasəti Redaktorunu (Local Group Policy Editor)** aça bilərik. Redaktor **Local Computer Policy** altında iki kateqoriyaya bölünür: **Kompüter Konfiqurasiyası (Computer Configuration)** və **İstifadəçi Konfiqurasiyası (User Configuration)**.

Məsələn, biz **Turn On Virtualization Based Security** tənzimləməsini aktiv etməklə **Credential Guard**-ı aktiv etmək üçün Local Computer Policy-ni aça bilərik. Credential Guard, əməliyyat sisteminin LSA prosesini təcrid edərək etimadnamə oğurluğu hücumlarından qoruyan Windows 10-dakı bir xüsusiyyətdir.

<img width="1632" height="1272" alt="image" src="https://github.com/user-attachments/assets/df520d99-ffa2-40c7-8715-aa1f3743bdb0" />

Biz həmçinin **dəqiq tənzimlənmiş hesab auditini** aktiv edə və **AppLocker**-i **Yerli Qrup Siyasəti Redaktoru** vasitəsilə konfiqurasiya edə bilərik. Yerli Qrup Siyasətini araşdırmağa və ondan Windows sistemini kilidləmək üçün istifadə edilə bilən geniş üsullar haqqında öyrənməyə dəyər.

---

## Windows Defender Antivirus

**Windows Defender Antivirus (Defender)**, əvvəllər **Windows Defender** kimi tanınan, Windows əməliyyat sistemləri ilə pulsuz gələn daxili antivirus proqramıdır. O, ilk dəfə **Windows XP** və **Server 2003** üçün yüklənə bilən **anti-spyware** aləti kimi buraxılmışdı. Defender **Windows Vista/Server 2008** ilə əməliyyat sisteminin bir hissəsi kimi əvvəlcədən paketlənmiş şəkildə gəlməyə başladı. Proqram **Windows 10 Creators Update** ilə **Windows Defender Antivirus** adlandırıldı.

Defender bir neçə xüsusiyyətlə gəlir:

* **Real-time protection (Real-vaxt qorunması)**: Cihazı real vaxt rejimində məlum təhlükələrdən qoruyur.
* **Cloud-delivered protection (Bulud vasitəsilə çatdırılan qorunma)**: Şübhəli faylları təhlil üçün yükləmək üçün avtomatik nümunə göndərilməsi ilə birlikdə işləyir. Fayllar bulud qorunma xidmətinə göndərildikdə, təhlil başa çatana qədər hər hansı potensial zərərli davranışın qarşısını almaq üçün **"kilidlənir"**.
* **Tamper Protection (Dəyişdirilmədən Qorunma)**: Təhlükəsizlik tənzimləmələrinin **Reyestr**, **PowerShell cmdletləri** və ya **qrup siyasəti** vasitəsilə dəyişdirilməsinin qarşısını alır.

Windows Defender bir sıra əlavə təhlükəsizlik xüsusiyyətləri və tənzimləmələrinin aktivləşdirilə və idarə oluna biləcəyi **Təhlükəsizlik Mərkəzindən (Security Center)** idarə olunur.

<img width="1546" height="975" alt="image" src="https://github.com/user-attachments/assets/24dd548a-a2ee-41d9-ad95-e66a14a0e799" />

Real-time qorunma tənzimləmələri, icazəsiz dəyişikliklərin qarşısını almaq üçün nəzarət edilən qovluq girişinə faylları, qovluqları və yaddaş sahələrini əlavə etmək üçün dəqiqləşdirilə bilər. Həmçinin, skan edilməməsi üçün faylları və ya qovluqları **istisna siyahısına (exclusion list)** əlavə edə bilərik. Buna bir nümunə, penetrasiya sınağı üçün istifadə edilən alətlər qovluğunu skandan istisna etməkdir, çünki onlar zərərli kimi işarələnəcək və karantinə alınacaq və ya sistemdən silinəcəkdir. **Nəzarət Edilən Qovluq Girişi (Controlled Folder Access)** Defender-in daxili **Ransomware** qorumasıdır.

Hansı qorunma tənzimləmələrinin aktiv olduğunu yoxlamaq üçün **Get-MpComputerStatus** PowerShell cmdlet-indən istifadə edə bilərik.

```powershell
PS C:\htb> Get-MpComputerStatus | findstr "True"
AMServiceEnabled                : True
AntispywareEnabled              : True
AntivirusEnabled                : True
BehaviorMonitorEnabled          : True
IoavProtectionEnabled           : True
IsTamperProtected               : True
NISEnabled                      : True
OnAccessProtectionEnabled       : True
RealTimeProtectionEnabled       : True
```

Heç bir antivirus həlli mükəmməl olmasa da, **Windows Defender** digər həllərlə, hətta ödənişli olanlarla müqayisədə aylıq aşkarlama sürəti testlərində çox yaxşı nəticə göstərir. Həmçinin, əməliyyat sisteminin bir hissəsi kimi əvvəlcədən quraşdırıldığı üçün, digər proqramların brauzer uzantıları və izləyicilər əlavə etməsi kimi, sistemə **"şişkinlik"** (*bloat*) gətirmir. Digər məhsulların isə əməliyyat sisteminə qoşulma tərzi səbəbindən sistemi yavaşlatdığı məlumdur.

Windows Defender qüsursuz deyil və sistemlərimizi qorumaq üçün yeganə çıxış yolu kimi qəbul edilməməli, əksinə **konfiqurasiya və yamalar idarəetməsinin əsas prinsipləri** üzərində qurulmuş **dərin müdafiə strategiyasının** bir hissəsi olmalıdır. Təriflər daim yenilənir və Windows Defender-in yeni versiyaları Windows 10, versiya 1909 kimi əsas əməliyyat buraxılışlarına daxil edilir (bu yazı zamanı ən son versiyadır).

Windows Defender **Metasploit** kimi ümumi açıq mənbəli freymvorklardan və ya **Mimikatz** kimi alətlərin dəyişdirilməmiş versiyalarından gələn zərərli yükləri aşkarlayacaq.

<img width="1331" height="853" alt="image" src="https://github.com/user-attachments/assets/1459e210-3a1d-47f4-8b40-7588c630ac58" />

Getdikcə çətinləşsə də, ən müasir təriflər quraşdırılmış ən son versiyanın tətbiq etdiyi Windows Defender qorumalarından tam yan keçmək hələ də mümkündür.





















