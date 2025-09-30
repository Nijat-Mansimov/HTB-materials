## NTFS və Paylaşma (Share) İcazələri

Microsoft Windows ilə qlobal masaüstü əməliyyat sistemləri bazarının **70%-dən çoxuna** sahibdir. Bu, niyə əksər zərərli proqram (malware) müəlliflərinin Windows üçün zərərli proqram yazmağı seçdiyini və niyə bir çox insanın Windows-u digər əməliyyat sistemlərindən daha az təhlükəsiz hesab etdiyini izah edir. Bir iş nöqteyi-nəzərindən, zərərli proqram müəllifləri üçün Windows üçün zərərli proqram yazmağa resurs sərf etmək məntiqlidir. Bu, **yüksək dəyərli bir hədəfdir**. Hər hansı bir ƏS-in zərərli proqramlara qarşı toxunulmaz olması fikri **texniki bir yanlışlıqdır**. Əgər bir əməliyyat sistemi üçün proqram təminatı yazıla bilirsə, o əməliyyat sistemi üçün virus da yazıla bilər. Unutmayın ki, tərifinə görə, virus **zərərli niyyətlə yazılmış proqram təminatıdır** və istənilən ƏS üçün yazıla bilər. Windows üçün yazılmış zərərli proqramların bir çox variantı, tətbiq olunmuş **yumşaq icazələrə** malik şəbəkə paylaşmaları (network shares) vasitəsilə şəbəkə üzərində yayıla bilər. Həmçinin qeyd etmək lazımdır ki, bu günə qədər, məşhur **EternalBlue** zəifliyi hələ də yamalanmamış (unpatched) **SMBv1** işləyən Windows sistemlərini narahat edir və tez-tez fidyə proqramlarının (ransomware) təşkilatları bağlamasına yol açır.

**Server Message Block (SMB) protokolu** Windows-da fayllar və printerlər kimi paylaşılan resursları birləşdirmək üçün istifadə olunur. Böyük, orta və kiçik müəssisə mühitlərində istifadə olunur. Bu konsepsiyanı vizuallaşdırmaq üçün aşağıdakı şəklə baxın:

<img width="1020" height="612" alt="image" src="https://github.com/user-attachments/assets/1d8e6c61-b2e9-4304-b11a-2d9df325baf8" />

**Qeyd:** Bir konsepsiyanın vizuallaşdırılmasını/diaqramını gördüyünüz zaman, onu hərtərəfli başa düşməyə vaxt ayırın. Bir şəkil min sözə dəyər ola bilər, lakin oxuyarkən onu ötürmək çox cazibədar olur.

NTFS icazələri və paylaşma icazələri tez-tez eyni kimi qəbul edilir. Lütfən, bilin ki, onlar eyni deyillər, lakin çox vaxt eyni paylaşılan resursa tətbiq olunurlar. Gəlin, NTFS fayl sistemində işləyən bir Windows ƏS-də yerləşdirilən şəbəkə paylaşmasına obyektlərə giriş vermək/təmin etmək üçün təyin edilə bilən fərdi icazələrə nəzər salaq.

## Paylaşma İcazələri

| İcazə | Təsviri |
| :--- | :--- |
| **Full Control (Tam Nəzarət)** | İstifadəçilərə **Change (Dəyişdirmə)** və **Read (Oxuma)** icazələri ilə verilən bütün hərəkətləri yerinə yetirməyə, həmçinin NTFS faylları və altqovluqları üçün icazələri dəyişdirməyə icazə verilir. |
| **Change (Dəyişdirmə)** | İstifadəçilərə faylları və altqovluqları oxumağa, redaktə etməyə, silməyə və əlavə etməyə icazə verilir. |
| **Read (Oxuma)** | İstifadəçilərə fayl və altqovluq məzmunlarını görməyə və siyahılamağa icazə verilir. |

---

## NTFS Əsas İcazələri

| İcazə | Təsviri |
| :--- | :--- |
| **Full Control (Tam Nəzarət)** | İstifadəçilərə faylları və qovluqları əlavə etməyə, redaktə etməyə, köçürməyə, silməyə, həmçinin bütün icazə verilmiş qovluqlara tətbiq olunan NTFS icazələrini dəyişdirməyə icazə verilir. |
| **Modify (Dəyişdirmə)** | İstifadəçilərə faylları və qovluqları görməyə və dəyişdirməyə icazə verilir və ya qadağan edilir. Buna faylların əlavə edilməsi və ya silinməsi daxildir. |
| **Read & Execute (Oxumaq və İcra Etmək)** | İstifadəçilərə faylların məzmununu oxumağa və proqramları icra etməyə icazə verilir və ya qadağan edilir. |
| **List folder contents (Qovluq Mündəricatını Siyahılamaq)** | İstifadəçilərə faylların və altqovluqların siyahısını görməyə icazə verilir və ya qadağan edilir. |
| **Read (Oxuma)** | İstifadəçilərə faylların məzmununu oxumağa icazə verilir və ya qadağan edilir. |
| **Write (Yazmaq)** | İstifadəçilərə faylda dəyişiklik yazmağa və qovluğa yeni fayllar əlavə etməyə icazə verilir və ya qadağan edilir. |
| **Special Permissions (Xüsusi İcazələr)** | Müxtəlif qabaqcıl icazə seçimləri. |

---

## NTFS Xüsusi İcazələri

| İcazə | Təsviri |
| :--- | :--- |
| **Full control (Tam Nəzarət)** | İstifadəçilərə faylları və qovluqları əlavə etməyə, redaktə etməyə, köçürməyə, silməyə, həmçinin bütün icazə verilmiş qovluqlara tətbiq olunan NTFS icazələrini dəyişdirməyə icazə verilir və ya qadağan edilir. |
| **Traverse folder / execute file (Qovluqda Hərəkət / Faylı İcra Etmək)** | İstifadəçilərə ana qovluq səviyyəsindəki məzmuna giriş qadağan edilsə belə, bir qovluq strukturu daxilində altqovluğa daxil olmaq imkanı verilir və ya qadağan edilir. İstifadəçilərə proqramları icra etməyə də icazə verilə bilər və ya qadağan edilə bilər. |
| **List folder/read data (Qovluğu Siyahılamaq/Məlumatı Oxumaq)** | İstifadəçilərə ana qovluqda olan faylları və qovluqları görməyə icazə verilir və ya qadağan edilir. İstifadəçilərə faylları açmağa və görməyə də icazə verilə bilər. |
| **Read attributes (Atributları Oxumaq)** | İstifadəçilərə fayl və ya qovluğun əsas atributlarını görməyə icazə verilir və ya qadağan edilir. Əsas atributlara misallar: sistem, arxiv, yalnız oxuma və gizli. |
| **Read extended attributes (Genişləndirilmiş Atributları Oxumaq)** | İstifadəçilərə fayl və ya qovluğun genişləndirilmiş atributlarını görməyə icazə verilir və ya qadağan edilir. Atributlar proqramdan asılı olaraq dəyişir. |
| **Create files/write data (Fayl Yaratmaq/Məlumat Yazmaq)** | İstifadəçilərə qovluq daxilində fayllar yaratmağa və faylda dəyişiklik etməyə icazə verilir və ya qadağan edilir. |
| **Create folders/append data (Qovluq Yaratmaq/Məlumat Əlavə Etmək)** | İstifadəçilərə qovluq daxilində altqovluqlar yaratmağa icazə verilir və ya qadağan edilir. Fayllara məlumat əlavə edilə bilər, lakin əvvəlcədən mövcud olan məzmunun üzərinə yazıla bilməz. |
| **Write attributes (Atributları Yazmaq)** | İstifadəçilərə fayl atributlarını dəyişdirməyə icazə verilir və ya qadağan edilir. Bu icazə faylların və ya qovluqların yaradılmasına icazə vermir. |
| **Write extended attributes (Genişləndirilmiş Atributları Yazmaq)** | İstifadəçilərə fayl və ya qovluq üzərində genişləndirilmiş atributları dəyişdirməyə icazə verilir və ya qadağan edilir. Atributlar proqramdan asılı olaraq dəyişir. |
| **Delete subfolders and files (Altqovluqları və Faylları Silmək)** | İstifadəçilərə altqovluqları və faylları silməyə icazə verilir və ya qadağan edilir. Ana qovluqlar silinməyəcək. |
| **Delete (Silmək)** | İstifadəçilərə ana qovluqları, altqovluqları və faylları silməyə icazə verilir və ya qadağan edilir. |
| **Read permissions (İcazələri Oxumaq)** | İstifadəçilərə bir qovluğun icazələrini oxumağa icazə verilir və ya qadağan edilir. |
| **Change permissions (İcazələri Dəyişdirmək)** | İstifadəçilərə bir faylın və ya qovluğun icazələrini dəyişdirməyə icazə verilir və ya qadağan edilir. |
| **Take ownership (Sahibliyi Almaq)** | İstifadəçilərə bir faylın və ya qovluğun sahibliyini almağa icazə verilir və ya qadağan edilir. Bir faylın sahibi istənilən icazəni dəyişdirmək üçün tam icazələrə malikdir. |

Unutmayın ki, **NTFS icazələri** qovluq və faylların yerləşdiyi sistemə tətbiq olunur. NTFS-də yaradılan qovluqlar defolt olaraq icazələri ana qovluqlardan miras alır. Bu modulda daha sonra edəcəyimiz kimi, ana və altqovluqlarda xüsusi icazələr təyin etmək üçün miras alınmanı ləğv etmək mümkündür. **Paylaşma icazələri** qovluğa SMB vasitəsilə, adətən şəbəkə üzərindən fərqli bir sistemdən daxil olunduğu zaman tətbiq olunur. Bu o deməkdir ki, maşına yerli olaraq və ya RDP vasitəsilə daxil olan biri, sadəcə fayl sistemindəki yerə getməklə paylaşılan qovluğa və fayllara daxil ola bilər və yalnız **NTFS icazələrini** nəzərə almalıdır. NTFS səviyyəsindəki icazələr administratorlara istifadəçilərin bir qovluq və ya fayl daxilində nə edə biləcəyinə dair daha dənəvər nəzarət imkanı verir.

## Şəbəkə Paylaşımı Yaratmaq

**SMB** və onun **NTFS** ilə əlaqəsi haqqında möhkəm bir əsas anlayış əldə etmək üçün **Windows 10 hədəf qutusunda** bir şəbəkə paylaşımı yaradacağıq.

**Qeyd:** Pwnbox-un tam ekranı ayrı bir monitorda açıq olması ideal bir öyrənmə təcrübəsidir, beləliklə yazılı məzmunu göstərmək üçün ən azı bir ekranımız və qarşılıqlı əlaqədə olduğumuz qutular üçün bir ekranımız ola bilər. Alternativ olaraq, yalnız bir ekrana girişimiz varsa, onu qutularla qarşılıqlı əlaqə üçün və yazılı məzmuna istinad etmək üçün smartfon və ya planşet istifadə edə bilərik.

Bu halda, əvvəlcə Windows 10 masaüstündə yeni bir qovluq yaradaraq paylaşılan qovluq yaradacağıq. Unutmayın ki, əksər böyük müəssisə mühitlərində paylaşmalar **Storage Area Network (SAN)**, **Network Attached Storage (NAS)** cihazında və ya Windows Server kimi server əməliyyat sistemi vasitəsilə daxil olunan disklərdəki ayrı bir hissədə (partition) yaradılır. Əgər biz masaüstü əməliyyat sistemində paylaşmalara rast gəlsək, bu ya kiçik bir iş olacaq, ya da penetrasiya testçisi və ya zərərli hücumçu tərəfindən məlumat toplamaq və ixrac etmək üçün istifadə edilən bir çıxış nöqtəsi (*beachhead system*) ola bilər.

Bu prosesi Windows-da **GUI** istifadə edərək həyata keçirəcəyik.

### Qovluğun Yaradılması

<img width="1025" height="600" alt="image" src="https://github.com/user-attachments/assets/ccabaf53-1168-4f1c-9604-fad987772705" />

Paylaşımı konfiqurasiya etmək üçün Qabaqcıl Paylaşım seçimindən istifadə edəcəyik.

### Qovluğun Paylaşılması

<img width="1024" height="600" alt="image" src="https://github.com/user-attachments/assets/975bc0d0-8990-415c-9ccc-7a62a38baeb8" />

Paylaşma adının avtomatik olaraq qovluğun adına necə **defolt** olaraq təyin olunduğunu qeyd edin. Həmçinin, bu paylaşmaya eyni anda qoşula bilən istifadəçilərin sayını məhdudlaşdırmağın mümkün olduğunu görə bilərik. Real dünya mühitində, administratorlar üçün bu sayı paylaşılan resursa mütəmadi olaraq ehtiyacı olan istifadəçilərin sayına uyğun təyin etmək **yaxşı bir praktikadır**.

NTFS icazələrinə bənzər şəkildə, paylaşılan resurslar üçün bir **giriş nəzarət siyahısı (ACL)** mövcuddur. Biz bunu SMB icazələr siyahısı kimi qəbul edə bilərik. Unutmayın ki, paylaşılan resurslarla, Windows-da paylaşılan hər bir resursa həm **SMB**, həm də **NTFS** icazələr siyahısı tətbiq olunur. ACL **giriş nəzarət qeydlərindən (ACEs)** ibarətdir. Tipik olaraq, bu ACE-lər **istifadəçilər** və **qruplardan** (həmçinin **təhlükəsizlik əsasları** adlanır) ibarətdir, çünki onlar paylaşılan resurslara girişi idarə etmək və izləmək üçün uyğun bir mexanizmdir.

Defolt giriş nəzarət qeydini və icazə parametrlərini qeyd edin.

**Paylaşma İcazələri ACL (Paylaşma Nişanı)**

<img width="1024" height="600" alt="image" src="https://github.com/user-attachments/assets/86740487-8454-4b13-9f7c-a33826d47591" />

İndilikdə, biz bu **ACL-in** (Giriş Nəzarət Siyahısı) və tətbiq olunmuş icazələrin təsirini yoxlamaq üçün bu parametrləri olduğu kimi tətbiq edəcəyik. Biz Pwnbox-dan terminalı açaraq və `smbclient` istifadə edərək əlaqəni yoxlayacağıq.

**Qeyd:** Texniki cəhətdən bir server, müştərinin sorğularına xidmət etmək üçün istifadə olunan bir proqram təminatı funksiyasıdır. Bu halda, **Pwnbox** bizim müştərimiz, **Windows 10 hədəf qutusu** isə bizim serverimizdir.

## Mövcud Paylaşmaları Siyahılamaq üçün `smbclient` İstifadəsi

```bash
nijatmansimov@htb[/htb]$ smbclient -L SERVER_IP -U htb-student
Enter WORKGROUP\htb-student's password: 

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	Company Data    Disk      
	IPC$            IPC       Remote IPC
```

## "Company Data" Paylaşmasına Qoşulma

```bash
nijatmansimov@htb[/htb]$ smbclient '\\SERVER_IP\Company Data' -U htb-student
Password for [WORKGROUP\htb-student]:
Try "help" to get a list of possible commands.

smb: \> 
```

-----

## Əlaqəni Potensial Olaraq Nə Bloklaya Bilər?

**Bütün girişlərimiz doğrudursa** və icazələr siyahımızda **Everyone** (Hər Kəs) qrupu ən azı **Read (Oxuma)** icazəsi ilə mövcuddursa, bu paylaşmaya daxil olmağımızı potensial olaraq **Windows Defender Firewall** bloka bilər.

Linux əsaslı bir sistemdən qoşulduğumuz üçün, firewall eyni **işçi qrupuna (workgroup)** qoşulmamış hər hansı bir cihazdan gələn girişi bloklamış ola bilər. Qeyd etmək vacibdir ki, bir Windows sistemi bir işçi qrupunun hissəsi olduqda, bütün `netlogon` sorğuları həmin Windows sisteminin **SAM** (Security Account Manager) verilənlər bazasına qarşı autentifikasiya edilir. Bir Windows sistemi **Windows Domain** mühitinə qoşulduqda isə, bütün `netlogon` sorğuları **Active Directory-yə** qarşı autentifikasiya edilir. Autentifikasiya baxımından bir işçi qrupu ilə Windows Domain arasındakı əsas fərq, işçi qrupunda yerli SAM verilənlər bazasının, Windows Domain-də isə mərkəzləşdirilmiş şəbəkə əsaslı verilənlər bazasının (**Active Directory**) istifadə edilməsidir. Windows sisteminə daxil olmağa və autentifikasiya etməyə çalışarkən bu məlumatı bilməliyik. Hədəfə düzgün qoşulmaq üçün `htb-student` hesabının harada yerləşdiyini nəzərdən keçirin.

Firewall-un qoşulmaları bloklaması məsələsi, Windows-da hər bir firewall profilini tamamilə deaktiv etməklə və ya **Windows Defender Firewall inkişaf etmiş təhlükəsizlik tənzimləmələrində** xüsusi əvvəlcədən təyin edilmiş daxil olan (inbound) firewall qaydalarını aktiv etməklə yoxlana bilər. Əksər firewall-lar kimi, Windows Defender Firewall da **daxil olan** və/və ya **çıxan** trafikə (bu halda giriş və qoşulma sorğularına) icazə verir və ya qadağan edir.

Fərqli daxil olan və çıxan qaydalar Defender-dəki fərqli firewall profilləri ilə əlaqələndirilir.

**Windows Defender Firewall Profilləri:**

  * **Public (İctimai)**
  * **Private (Şəxsi)**
  * **Domain (Domen)**

Bütün firewall-u tamamilə deaktiv etmək əvəzinə, əvvəlcədən təyin edilmiş qaydaları aktivləşdirmək və ya xüsusi istisnalar əlavə etmək **ən yaxşı praktikadır**. Təəssüf ki, rahatlıq naminə və ya anlayış çatışmazlığı səbəbindən firewall-ların tamamilə deaktiv edilmiş vəziyyətdə qalması çox yayılmış bir haldır. Masaüstü sistemlərindəki firewall qaydaları, Windows Domain mühitinə qoşulduqda **Group Policy** istifadə edilərək mərkəzləşdirilmiş şəkildə idarə oluna bilər. Group Policy konsepsiyaları və konfiqurasiyaları bu modulun əhatə dairəsindən kənardır.

Müvafiq **daxil olan** firewall qaydaları aktiv edildikdən sonra, biz paylaşmaya uğurla qoşulacağıq. Unutmayın ki, biz yalnız paylaşmaya qoşula bilərik, çünki istifadə etdiyimiz istifadəçi hesabı (`htb-student`) **Everyone** qrupundadır. Xatırlayın ki, biz Everyone qrupu üçün xüsusi paylaşma icazələrini **Read (Oxuma)** olaraq qoymuşduq, bu da tam olaraq o deməkdir ki, bu paylaşmada yalnız faylları **Oxuya** biləcəyik. Paylaşma ilə əlaqə qurulduqdan sonra, Pwnbox-dan Windows 10 hədəf qutusunun fayl sisteminə bir **birləşmə nöqtəsi (mount point)** yarada bilərik. Bu, həm də **NTFS icazələrinin** paylaşma icazələri ilə yanaşı tətbiq olunduğunu nəzərə almalı olduğumuz yerdir. Unutmayın ki, NTFS Windows-da defolt fayl sistemidir. Gəlin Windows 10 hədəf qutumuzla olan `xfreerdp` sessiyamıza qayıdaq və **Company Data** qovluğundakı NTFS icazələrinə nəzər salaq.

**NTFS İcazələri ACL (Təhlükəsizlik Nişanı)**

<img width="1024" height="540" alt="image" src="https://github.com/user-attachments/assets/2be036b9-fe9b-4384-a372-fb4ccf7a9ebf" />

NTFS icazələri ilə istifadəçilərə və qruplara tətbiq oluna bilən daha dənəvər nəzarət mövcuddur. Bir icazənin yanında **boz rəngli işarə** görəndə, bu, onun valideyn qovluğundan miras alındığı deməkdir. Defolt olaraq, bütün NTFS icazələri valideyn qovluğundan miras alınır. Windows dünyasında, sistem administratoru yeni yaradılmış qovluğun qabaqcıl Təhlükəsizlik tənzimləmələrində miras alınmanı ləğv etmədiyi təqdirdə, **C:\\ sürücüsü** bütün qovluqları idarə edən valideyn qovluğudur.

Bir çox hallarda, bir təşkilatın sistem administrator(lar)ı bir istifadəçinin və ya istifadəçi qrupunun şəbəkə resursları üzərində hansı icazələri əldə edəcəyinə qərar verməkdən məsul olacaqlar. Məhz buna görə də bir çox **spear-phishing** (hədəfli fişinq) hücumları sistem administratorlarına və digər İT liderlərinə yönəldilir. Onlar nəzarət etdikləri mühitlərdə icazə verilən şeylərə böyük təsir göstərirlər, bir çox hallarda təşkilatın qeyri-texniki c-səviyyəli liderlərindən daha çox. Məsələn, bir xəstəxanada işləyən həkimlər və ya rəhbərlər şəbəkə üzərində inzibati hüquqlara malik olmayacaqlar, lakin sistem administratorları malik olacaqlar.

İndi gəlin **Everyone** qrupuna paylaşma səviyyəsində **Full control (Tam Nəzarət)** verək və Pwnbox-umuzun Masaüstündən paylaşmaya bir birləşmə nöqtəsi (mount point) yaratmağa çalışaraq dəyişikliyin təsirini yoxlayaq.

## Paylaşmaya Birləşdirmə (Mounting)

```bash
nijatmansimov@htb[/htb]$ sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //ipaddoftarget/"Company Data" /home/user/Desktop/
```

Əgər bu əmr işləmirsə, sintaksisi yoxlayın. Əgər sintaksis doğrudursa, lakin əmr hələ də işləmirsə, **cifs-utils** quraşdırılmasına ehtiyac ola bilər. Bu, aşağıdakı əmrlə edilə bilər:

### CIFS Utilities Quraşdırma

```bash
nijatmansimov@htb[/htb]$ sudo apt-get install cifs-utils
```

Pwnbox-umuzun Masaüstündə birləşmə nöqtəsini uğurla yaratdıqdan sonra, etdiklərimizi izləməyə və nəzarət etməyə imkan verəcək Windows-a daxil edilmiş bir neçə alətə nəzər salmalıyıq.

`net share` əmri sistemdəki bütün paylaşılan qovluqları görməyə imkan verir. Yaratdığımız paylaşmanı və həmçinin C:\\ sürücüsünü qeyd edin.

**Biz C:\\ sürücüsünü əl ilə paylaşdığımızı xatırlayırsınızmı?**

Biz C:-ni əl ilə paylaşmadıq. Windows sistemindəki ən kritik fayllara malik ən vacib sürücü quraşdırma zamanı **SMB** vasitəsilə paylaşılır. Bu o deməkdir ki, müvafiq girişi olan hər kəs şəbəkədəki hər bir Windows sisteminin bütün C:-nə uzaqdan daxil ola bilər.

Yaratdığımız paylaşmanı da görə bilərik.

### `net share` İstifadə Edərək Paylaşmaları Göstərmə

```bash
C:\Users\htb-student> net share

Share name   Resource                        Remark

-------------------------------------------------------------------------------
C$           C:\                             Default share
IPC$                                         Remote IPC
ADMIN$       C:\WINDOWS                      Remote Admin
Company Data C:\Users\htb-student\Desktop\Company Data

The command completed successfully.
```

**Computer Management (Kompüter İdarəetməsi)** Windows sistemində paylaşılan resursları müəyyən etmək və izləmək üçün istifadə edə biləcəyimiz başqa bir vasitədir.

### Computer Management-dən Paylaşmaları İzləmə

<img width="1024" height="713" alt="image" src="https://github.com/user-attachments/assets/5a155533-4a3e-4fe5-9bfa-04be2c3e1861" />

Biz **Paylaşmalar (Shares)**, **Sessiyalar (Sessions)** və **Açıq Fayllar (Open Files)** bölmələrini yoxlayaraq bunun bizə hansı məlumatları verdiyini anlaya bilərik. Əgər SMB ilə əlaqəli bir təhlükəsizlik pozuntusuna (breach) cavab verməkdə bir fərdə və ya təşkilata kömək etsək, bunlar pozuntunun necə baş verdiyini və nəyin geridə qaldığını başa düşməyə başlamaq üçün yoxlanılmalı əla yerlərdir.

## Event Viewer-də Paylaşma Girişi Jurnallarını Görmə

**Event Viewer (Hadisə İzləyicisi)** Windows-da tamamlanmış hərəkətləri araşdırmaq üçün başqa bir yaxşı yerdir. Demək olar ki, hər bir əməliyyat sisteminin bir jurnallaşdırma mexanizmi və tutulmuş jurnalları görmək üçün bir yardımçı proqramı var. Bilin ki, jurnal kompüter üçün bir növ gündəlik qeydi kimidir, burada kompüter yerinə yetirilən bütün hərəkətləri və bu hərəkətlərlə əlaqəli çoxsaylı detalları qeyd edir. Biz Windows 10 hədəf qutusuna daxil olarkən, habelə paylaşılan qovluğu yaradarkən, redaktə edərkən və ona daxil olarkən yerinə yetirdiyimiz hər bir hərəkət üçün yaradılmış jurnalları görə bilərik.

<img width="1024" height="729" alt="image" src="https://github.com/user-attachments/assets/2f029355-363a-43ab-a622-3c47df086e10" />









