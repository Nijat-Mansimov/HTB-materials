## Əməliyyat Sistemi Strukturu

Windows əməliyyat sistemlərində kök qovluğu `<sürücü_hərfi>:\` (adətən **C sürücüsü**) olur. Kök qovluğu (həmçinin **yükləmə hissəsi** - *boot partition* kimi tanınır) əməliyyat sisteminin quraşdırıldığı yerdir. Digər fiziki və virtual sürücülərə başqa hərflər təyin edilir, məsələn, Data (**E:**). Yükləmə hissəsinin qovluq strukturu aşağıdakı kimidir:

| Qovluq Adı | Funksiyası |
| :--- | :--- |
| **PerfLogs** | Windows performans jurnallarını saxlaya bilər, lakin defolt olaraq boşdur. |
| **Program Files** | 32-bit sistemlərdə bütün 16-bit və 32-bit proqramlar burada quraşdırılır. 64-bit sistemlərdə isə yalnız 64-bit proqramlar burada quraşdırılır. |
| **Program Files (x86)** | Windows-un 64-bit nəşrlərində 32-bit və 16-bit proqramlar burada quraşdırılır. |
| **ProgramData** | Bu, quraşdırılmış müəyyən proqramların işləməsi üçün vacib olan məlumatları ehtiva edən gizli bir qovluqdur. Bu məlumat hansı istifadəçinin proqramı işlətməsindən asılı olmayaraq proqram tərəfindən əldə edilə bilər. |
| **Users** | Bu qovluq sistemə daxil olan hər bir istifadəçi üçün istifadəçi profillərini ehtiva edir və **Public** və **Default** olmaqla iki qovluqdan ibarətdir. |
| **Default** | Bu, bütün yaradılmış istifadəçilər üçün defolt istifadəçi profili şablonudur. Sistemə yeni bir istifadəçi əlavə edildikdə, onun profili **Default** profili əsasında yaradılır. |
| **Public** | Bu qovluq kompüter istifadəçilərinin faylları paylaşması üçün nəzərdə tutulub və defolt olaraq bütün istifadəçilər üçün əlçatandır. Bu qovluq defolt olaraq şəbəkə üzərindən paylaşılır, lakin daxil olmaq üçün etibarlı bir şəbəkə hesabına ehtiyac duyur. |
| **AppData** | Hər bir istifadəçiyə aid tətbiq məlumatları və tənzimləmələri gizli bir istifadəçi altqovluğunda saxlanılır (yəni, `cliff.moore\AppData`). Bu qovluqların hər biri üç altqovluqdan ibarətdir: **Roaming** qovluğu fərdi lüğətlər kimi, istifadəçinin profilinə uyğun gəlməli olan maşından asılı olmayan məlumatları ehtiva edir. **Local** qovluğu kompüterin özünə xasdır və heç vaxt şəbəkə üzərində sinxronlaşdırılmır. **LocalLow** Local qovluğuna bənzəyir, lakin daha aşağı məlumat bütövlüyü səviyyəsinə malikdir. Buna görə də, məsələn, qorunan və ya təhlükəsiz rejimə quraşdırılmış bir veb-brauzer tərəfindən istifadə edilə bilər. |
| **Windows** | Windows əməliyyat sistemi üçün tələb olunan faylların əksəriyyəti burada yerləşir. |
| **System, System32, SysWOW64** | Windows-un əsas xüsusiyyətləri və Windows API üçün tələb olunan bütün **DLL-ləri** ehtiva edir. Əməliyyat sistemi, bir proqram mütləq bir yol göstərmədən bir DLL yükləməsini istədiyi zaman bu qovluqlarda axtarış aparır. |
| **WinSxS** | **Windows Komponent Mağazası** (Windows Component Store) bütün Windows komponentlərinin, yeniləmələrinin və xidmət paketlərinin bir nüsxəsini ehtiva edir. |

-----

## Əmr Sətrindən İstifadə Edərək Qovluqları Tədqiq Etmə

Fayl sistemini `dir` əmrindən istifadə edərək tədqiq edə bilərik.

```bash
C:\htb> dir c:\ /a
 Volume in drive C has no label.
 Volume Serial Number is F416-77BE

 Directory of c:\

08/16/2020  10:33 AM    <DIR>          $Recycle.Bin
06/25/2020  06:25 PM    <DIR>          $WinREAgent
07/02/2020  12:55 PM             1,024 AMTAG.BIN
06/25/2020  03:38 PM    <JUNCTION>     Documents and Settings [C:\Users]
08/13/2020  06:03 PM             8,192 DumpStack.log
08/17/2020  12:11 PM             8,192 DumpStack.log.tmp
08/27/2020  10:42 AM    37,752,373,248 hiberfil.sys
08/17/2020  12:11 PM    13,421,772,800 pagefile.sys
12/07/2019  05:14 AM    <DIR>          PerfLogs
08/24/2020  10:38 AM    <DIR>          Program Files
07/09/2020  06:08 PM    <DIR>          Program Files (x86)
08/24/2020  10:41 AM    <DIR>          ProgramData
06/25/2020  03:38 PM    <DIR>          Recovery
06/25/2020  03:57 PM             2,918 RHDSetup.log
08/17/2020  12:11 PM        16,777,216 swapfile.sys
08/26/2020  02:51 PM    <DIR>          System Volume Information
08/16/2020  10:33 AM    <DIR>          Users
08/17/2020  11:38 PM    <DIR>          Windows
               7 File(s) 51,190,943,590 bytes
              13 Dir(s)  261,310,697,472 bytes free
```

`tree` yardımçı proqramı bir yolun və ya diskin qovluq strukturunu qrafik şəkildə göstərmək üçün faydalıdır.

```bash
C:\htb> tree "c:\Program Files (x86)\VMware"
Folder PATH listing
Volume serial number is F416-77BE
C:\PROGRAM FILES (X86)\VMWARE
├───VMware VIX
│   ├───doc
│   │   ├───errors
│   │   ├───features
│   │   ├───lang
│   │   │   └───c
│   │   │       └───functions
│   │   └───types
│   ├───samples
│   └───Workstation-15.0.0
│       ├───32bit
│       └───64bit
└───VMware Workstation
    ├───env
    ├───hostd
    │   ├───coreLocale
    │   │   └───en
    │   ├───docroot
    │   │   ├───client
    │   │   └───sdk
    │   ├───extensions
    │   │   └───hostdiag
    │   │       └───locale
    │   │           └───en
    │   └───vimLocale
    │       └───en
    ├───ico
    ├───messages
    │   ├───ja
    │   └───zh_CN
    ├───OVFTool
    │   ├───env
    │   │   └───en
    │   └───schemas
    │       ├───DMTF
    │       └───vmware
    ├───Resources
    ├───tools-upgraders
    └───x64
```

`tree` əmri bizə çoxlu məlumat verə bilər. Aşağıdakı əmr, C sürücüsündəki bütün faylları bir ekran bir ekran gəzmək üçün istifadə edilə bilər. Bu əmr istənilən qovluğa qarşı işlədilmək üçün dəyişdirilə bilər.

```bash
tree c:\ /f | more
```
