## Fayl Sistemi

Windows fayl sistemlərinin 5 növü var: **FAT12**, **FAT16**, **FAT32**, **NTFS** və **exFAT**. FAT12 və FAT16 müasir Windows əməliyyat sistemlərində artıq istifadə edilmir. Biz bu təlim üçün FAT32 və exFAT fayl sistemlərinə toxunacağıq, lakin əsas diqqətimiz **NTFS** fayl sistemi olacaq.

**FAT32 (File Allocation Table)** USB yaddaş stikləri və SD kartlar kimi bir çox növ yaddaş qurğularında geniş şəkildə istifadə olunur, lakin sərt diskləri formatlamaq üçün də istifadə edilə bilər. Adındakı "32" FAT32-nin yaddaş qurğusunda məlumat klasterlərini müəyyən etmək üçün **32 bitlik məlumatdan** istifadə etməsinə aiddir.

**FAT32-nin Üstünlükləri:**

  * **Qurğu uyğunluğu:** Kompüterlərdə, rəqəmsal kameralarda, oyun konsollarında, smartfonlarda, planşetlərdə və s. istifadə edilə bilər.
  * **Əməliyyat sistemi çarpaz uyğunluğu:** Windows 95-dən başlayaraq bütün Windows əməliyyat sistemlərində işləyir və həmçinin MacOS və Linux tərəfindən də dəstəklənir.

**FAT32-nin Mənfi Cəhətləri:**

  * Yalnız 4 GB-dan az olan fayllarla istifadə edilə bilər.
  * Daxili məlumat mühafizəsi və ya fayl sıxılma xüsusiyyətləri yoxdur.
  * Fayl şifrələməsi üçün üçüncü tərəf vasitələrdən istifadə etmək lazımdır.

**NTFS (New Technology File System)** Windows NT 3.1-dən bəri defolt Windows fayl sistemidir. FAT32-nin çatışmazlıqlarını aradan qaldırmaqla yanaşı, NTFS həm də yaxşılaşdırılmış məlumat strukturlaşdırılması sayəsində **metadata** üçün daha yaxşı dəstəyə və daha yaxşı performansa malikdir.

**NTFS-in Üstünlükləri:**

  * NTFS **etibarlıdır** və sistemin sıradan çıxması və ya elektrik enerjisinin kəsilməsi halında fayl sisteminin ardıcıllığını bərpa edə bilər.
  * Bizə həm fayllar, həm də qovluqlar üzərində **dənəvər icazələr** təyin etməyə imkan verməklə təhlükəsizliyi təmin edir.
  * Çox böyük ölçülü hissə (partition) ölçülərini dəstəkləyir.
  * Daxili **jurnallaşdırmaya** (journaling) malikdir, yəni fayl dəyişiklikləri (əlavə etmə, dəyişdirmə, silmə) qeyd olunur.

**NTFS-in Mənfi Cəhətləri:**

  * Əksər mobil cihazlar NTFS-i yerli olaraq dəstəkləmir.
  * TV-lər və rəqəmsal kameralar kimi köhnə media qurğuları NTFS yaddaş qurğuları üçün dəstək təklif etmir.

-----

## İcazələr (Permissions)

NTFS fayl sisteminin bir çox əsas və qabaqcıl icazələri var. Əsas icazə növlərindən bəziləri bunlardır:

| İcazə Növü | Təsviri |
| :--- | :--- |
| **Full Control (Tam Nəzarət)** | Faylları/qovluqları oxumağa, yazmağa, dəyişməyə, silməyə icazə verir. |
| **Modify (Dəyişdirmə)** | Faylları/qovluqları oxumağa, yazmağa və silməyə icazə verir. |
| **List Folder Contents (Qovluq Mündəricatını Siyahılamaq)** | Qovluqları və altqovluqları görməyə və siyahılamağa, həmçinin faylları icra etməyə icazə verir. Yalnız qovluqlar bu icazəni miras alır. |
| **Read and Execute (Oxumaq və İcra Etmək)** | Faylları və altqovluqları görməyə və siyahılamağa, həmçinin faylları icra etməyə icazə verir. Fayllar və qovluqlar bu icazəni miras alır. |
| **Write (Yazmaq)** | Qovluqlara və altqovluqlara fayl əlavə etməyə və bir fayla yazmağa icazə verir. |
| **Read (Oxumaq)** | Qovluqları və altqovluqları görməyə və siyahılamağa, həmçinin faylın məzmununu görməyə icazə verir. |
| **Traverse Folder (Qovluqda Hərəkət)** | Digər fayllara və ya qovluqlara çatmaq üçün qovluqlar arasında hərəkət etmək imkanını icazə verir və ya qadağan edir. Məsələn, bir istifadəçinin bu nümunədəki (`c:\users\bsmith\documents\webapps\backups\backup_02042020.zip`) sənədlər və ya veb-tətbiqlər qovluğundakı məzmunu siyahılamağa və ya faylları görməyə icazəsi olmaya bilər, lakin **Traverse Folder** icazələri tətbiq edildikdə, o, ehtiyat nüsxə arxivinə daxil ola bilər. |

Fayllar və qovluqlar idarəetmənin asanlığı üçün valideyn qovluqlarının NTFS icazələrini miras alır, beləliklə administratorlar hər bir fayl və qovluq üçün icazələri açıq şəkildə təyin etməyə ehtiyac duymurlar, çünki bu, həddindən artıq vaxt aparan bir proses olardı. Əgər icazələrin açıq şəkildə təyin edilməsinə ehtiyac olarsa, administrator zəruri fayllar və qovluqlar üçün icazə miras alınmasını (inheritance) ləğv edə və sonra icazələri birbaşa hər birinə təyin edə bilər.

-----

## Bütövlük Nəzarəti Giriş Nəzarət Siyahısı (icacls)

Windows-da fayllar və qovluqlar üzərindəki NTFS icazələri Təhlükəsizlik sekmesi altında **File Explorer GUI** istifadə edilərək idarə oluna bilər. GUI-dən başqa, biz həmçinin `icacls` yardımçı proqramından istifadə edərək əmr sətrindən Windows-da NTFS fayl icazələri üzərində incə dənəvərliyə nail ola bilərik.

İş qovluğunda `icacls` əmrini və ya hal-hazırda olmadığımız bir qovluğa qarşı `icacls C:\Windows` əmrini işlətməklə müəyyən bir qovluqdakı NTFS icazələrini siyahılaya bilərik.

```bash
C:\htb> icacls c:\windows
c:\windows NT SERVICE\TrustedInstaller:(F)
           NT SERVICE\TrustedInstaller:(CI)(IO)(F)
           NT AUTHORITY\SYSTEM:(M)
           NT AUTHORITY\SYSTEM:(OI)(CI)(IO)(F)
           BUILTIN\Administrators:(M)
           BUILTIN\Administrators:(OI)(CI)(IO)(F)
           BUILTIN\Users:(RX)
           BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
           CREATOR OWNER:(OI)(CI)(IO)(F)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(RX)
           APPLICATION PACKAGE AUTHORITY\ALL RESTRICTED APPLICATION PACKAGES:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

Çıxışda hər bir istifadəçidən sonra resurs giriş səviyyəsi qeyd olunur. Mümkün miras alınma (inheritance) parametrləri bunlardır:

  * **(CI):** konteyner miras alınması (*container inherit*)
  * **(OI):** obyekt miras alınması (*object inherit*)
  * **(IO):** yalnız miras al (*inherit only*)
  * **(NP):** miras alınmanı yayma (*do not propagate inherit*)
  * **(I):** valideyn konteynerdən miras alınmış icazə (*permission inherited from parent container*)

Yuxarıdakı nümunədə, **NT AUTHORITY\\SYSTEM** hesabı obyekt miras alınmasına, konteyner miras alınmasına, yalnız miras alınmaya və tam giriş icazələrinə malikdir. Bu o deməkdir ki, bu hesab bu qovluqdakı və altqovluqlardakı bütün fayl sistemi obyektləri üzərində **tam nəzarətə** malikdir.

Əsas giriş icazələri aşağıdakılardır:

  * **F:** tam giriş (*full access*)
  * **D:** silmə girişi (*delete access*)
  * **N:** giriş yoxdur (*no access*)
  * **M:** dəyişdirmə girişi (*modify access*)
  * **RX:** oxumaq və icra etmək girişi (*read and execute access*)
  * **R:** yalnız oxuma girişi (*read-only access*)
  * **W:** yalnız yazma girişi (*write-only access*)

Biz `icacls` istifadə edərək əmr sətri vasitəsilə icazələri əlavə edə və silə bilərik. Burada, **C:\\users** qovluğunun göstərildiyi yerli administrator hesabının kontekstində `icacls` icra edirik, burada **joe** istifadəçisinin heç bir yazma icazəsi yoxdur.

```bash
C:\htb> icacls c:\Users
c:\Users NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         BUILTIN\Users:(RX)
         BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
         Everyone:(RX)
         Everyone:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

`icacls c:\users /grant joe:f` əmrindən istifadə edərək biz **joe** istifadəçisinə qovluq üzərində **tam nəzarət** verə bilərik, lakin əmrdə `(oi)` və `(ci)` daxil edilmədiyi üçün joe istifadəçisinin yalnız **c:\\users** qovluğu üzərində hüquqları olacaq, lakin istifadəçi altqovluqları və onların içərisindəki fayllar üzərində yox.

```bash
C:\htb> icacls c:\users /grant joe:f
processed file: c:\users
Successfully processed 1 files; Failed processing 0 files

C:\htb> >icacls c:\users
c:\users WS01\joe:(F)
         NT AUTHORITY\SYSTEM:(OI)(CI)(F)
         BUILTIN\Administrators:(OI)(CI)(F)
         BUILTIN\Users:(RX)
         BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
         Everyone:(RX)
         Everyone:(OI)(CI)(IO)(GR,GE)

Successfully processed 1 files; Failed processing 0 files
```

Bu icazələr `icacls c:\users /remove joe` əmri istifadə edilərək ləğv edilə bilər.

`icacls` çox güclüdür və domen şəraitində müəyyən istifadəçilərə və ya qruplara fayl və ya qovluq üzərində xüsusi icazələr vermək, girişi açıq şəkildə qadağan etmək, miras alınma icazələrini aktivləşdirmək və ya ləğv etmək və qovluq/fayl sahibliyini dəyişdirmək üçün istifadə edilə bilər.
