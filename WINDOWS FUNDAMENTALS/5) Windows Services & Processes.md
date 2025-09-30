## Windows Xidmətləri və Prosesləri

### Windows Xidmətləri (Services)

Xidmətlər Windows əməliyyat sisteminin əsas tərkib hissəsidir. Onlar uzun müddət davam edən proseslərin yaradılmasına və idarə edilməsinə imkan verir. Windows xidmətləri istifadəçinin müdaxiləsi olmadan sistem yükləndikdə avtomatik olaraq başlaya bilər. Bu xidmətlər istifadəçi sistemdəki hesabından çıxdıqdan sonra da arxa planda işləməyə davam edə bilər.

Tətbiqlər də bir xidmət kimi quraşdırılmaq üçün yaradıla bilər, məsələn, bir serverdə quraşdırılmış şəbəkə monitorinqi tətbiqi. Windows-dakı xidmətlər Windows əməliyyat sistemi daxilində şəbəkə funksiyaları, sistem diaqnostikasının yerinə yetirilməsi, istifadəçi etimadnamələrinin idarə edilməsi, Windows yeniləmələrinə nəzarət və daha çox funksiyalar üçün məsuliyyət daşıyır.

Windows xidmətləri **Service Control Manager (SCM)** sistemi vasitəsilə idarə olunur, hansı ki, `services.msc` MMC əlavəsi vasitəsilə əlçatandır.

Bu əlavə xidmətlərlə qarşılıqlı əlaqə qurmaq və idarə etmək üçün **GUI** (Qrafik İstifadəçi İnterfeysi) təqdim edir və hər bir quraşdırılmış xidmət haqqında məlumatları göstərir. Bu məlumatlara Xidmətin **Adı**, **Təsviri**, **Statusu**, **Başlama Tipi** və xidmətin hansı istifadəçi altında işlədiyi daxildir.

Həmçinin `sc.exe` istifadə edərək əmr sətri vasitəsilə və ya `Get-Service` kimi **PowerShell** cmdlet-ləri vasitəsilə xidmətləri sorğulamaq və idarə etmək mümkündür.

```powershell
PS C:\htb> Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl
Name                : AdobeARMservice
DisplayName         : Adobe Acrobat Update Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : Appinfo
DisplayName         : Application Information
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {RpcSs, ProfSvc}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess, Win32ShareProcess
```

Xidmət statusları **Running (İşləyən)**, **Stopped (Dayandırılmış)** və ya **Paused (Fasilə Verilmiş)** kimi görünə bilər və onların sistem yüklənməsi zamanı **əl ilə**, **avtomatik** və ya **gecikmə ilə** başlanması təyin edilə bilər. Xidmətlər həmçinin hər hansı bir hərəkət onları başlamağa və ya dayandırmağa təkan verdikdə **Starting (Başlayır)** və ya **Stopping (Dayanır)** vəziyyətində də göstərilə bilər. Windows-un üç kateqoriyalı xidməti var: **Yerli Xidmətlər**, **Şəbəkə Xidmətləri** və **Sistem Xidmətləri**. Xidmətlər adətən yalnız inzibati imtiyazlara malik istifadəçilər tərəfindən yaradıla, dəyişdirilə və silinə bilər. Xidmət icazələri ətrafındakı səhv konfiqurasiyalar Windows sistemlərində geniş yayılmış **imtiyaz artırma (privilege escalation) vektorudur**.

Windows-da, sistemin yenidən başladılması olmadan dayandırıla və yenidən başladılmayan **kritik sistem xidmətləri** mövcuddur. Əgər bu xidmətlərdən biri tərəfindən istifadə edilən hər hansı bir faylı və ya resursu yeniləsək, sistemi yenidən başlatmalıyıq.

| Xidmət | Təsviri |
| :--- | :--- |
| **smss.exe** | Session Manager SubSystem (Sessiya İdarəetmə Alt Sistemi). Sistemdəki sessiyaların idarə edilməsinə cavabdehdir. |
| **csrss.exe** | Client Server Runtime Process (Müştəri Server İşləmə Prosesi). Windows alt sisteminin istifadəçi rejimi hissəsidir. |
| **wininit.exe** | Kompüter proqram quraşdırıldıqdan sonra yenidən başladıldıqda Windows-da ediləcək bütün dəyişiklikləri sadalayan Wininit faylını (.ini) başladır. |
| **logonui.exe** | İstifadəçinin fərdi kompüterə (PC) daxil olmasını asanlaşdırmaq üçün istifadə olunur. |
| **lsass.exe** | Local Security Authentication Server (Yerli Təhlükəsizlik Autentifikasiya Serveri) istifadəçi girişlərinin PC və ya serverə etibarlılığını yoxlayır. O, Winlogon xidməti üçün istifadəçiləri autentifikasiya etməkdən məsul olan prosesi yaradır. |
| **services.exe** | Xidmətlərin başlanması və dayandırılması əməliyyatını idarə edir. |
| **winlogon.exe** | Təhlükəsiz diqqət ardıcıllığının idarə edilməsinə, daxil olarkən istifadəçi profilinin yüklənməsinə və ekran qoruyucusu işləyərkən kompüterin kilidlənməsinə cavabdehdir. |
| **System** | Windows nüvəsini işlədən arxa plan sistem prosesi. |
| **svchost.exe with RPCSS** | "Automatic Updates", "Windows Firewall" və "Plug and Play" kimi **dinamik əlaqə kitabxanalarından** (.dll uzantılı fayllar) işləyən sistem xidmətlərini idarə edir. Remote Procedure Call (RPC) Xidmətindən (RPCSS) istifadə edir. |
| **svchost.exe with Dcom/PnP** | "Automatic Updates", "Windows Firewall" və "Plug and Play" kimi dinamik əlaqə kitabxanalarından (.dll uzantılı fayllar) işləyən sistem xidmətlərini idarə edir. Distributed Component Object Model (DCOM) və Plug and Play (PnP) xidmətlərindən istifadə edir. |

Bu **linkdə** əsas xidmətlər daxil olmaqla, Windows komponentlərinin siyahısı var.

-----

## Proseslər (Processes)

Proseslər Windows sistemlərində arxa planda işləyir. Onlar ya Windows əməliyyat sisteminin bir hissəsi olaraq avtomatik işləyir, ya da digər quraşdırılmış tətbiqlər tərəfindən başladılır.

Quraşdırılmış tətbiqlərlə əlaqəli proseslər çox vaxt əməliyyat sisteminə ciddi təsir göstərmədən sonlandırıla bilər. Bəzi proseslər kritikdir və sonlandırılarsa, əməliyyat sisteminin müəyyən komponentlərinin düzgün işləməsini dayandıracaq. Buna misal olaraq **Windows Logon Application**, **System**, **System Idle Process**, **Windows Start-Up Application**, **Client Server Runtime**, **Windows Session Manager**, **Service Host** və **Local Security Authority Subsystem Service (LSASS)** prosesini göstərmək olar.

### Local Security Authority Subsystem Service (LSASS)

`lsass.exe` Windows sistemlərində **təhlükəsizlik siyasətini tətbiq etməkdən** məsul olan prosesdir. Bir istifadəçi sistemə daxil olmağa cəhd etdikdə, bu proses onların giriş cəhdini yoxlayır və istifadəçinin icazə səviyyələrinə əsaslanaraq **giriş tokenləri** yaradır. LSASS həmçinin istifadəçi hesabının şifrə dəyişikliklərinə də cavabdehdir. Bu proseslə əlaqəli bütün hadisələr (giriş/çıxış cəhdləri və s.) **Windows Təhlükəsizlik Jurnalında** qeyd olunur. **LSASS** son dərəcə yüksək dəyərli bir hədəfdir, çünki bu proses tərəfindən yaddaşda saxlanılan həm **açıq mətn** (cleartext), həm də **heşlənmiş etimadnamələri** çıxarmaq üçün bir neçə alət mövcuddur.

### Sysinternals Alətləri

**SysInternals Alətlər dəsti** Windows sistemlərini idarə etmək üçün istifadə oluna bilən bir sıra portativ Windows tətbiqləridir (əksər hallarda quraşdırma tələb olunmur). Alətlər ya Microsoft veb-saytından yüklənə bilər, ya da Windows Explorer pəncərəsinə `\\live.sysinternals.com\tools` yazaraq birbaşa internetdən əlçatan fayl paylaşımından yüklənə bilər.

Məsələn, `procdump.exe` proqramını birbaşa diskə yükləmədən bu paylaşmadan işlədə bilərik.

```bash
C:\htb> \\live.sysinternals.com\tools\procdump.exe -accepteula

ProcDump v9.0 - Sysinternals process dump utility
Copyright (C) 2009-2017 Mark Russinovich and Andrew Richards
Sysinternals - www.sysinternals.com

Monitors a process and writes a dump file when the process exceeds the
specified criteria or has an exception.

Capture Usage:
   procdump.exe [-mm] [-ma] [-mp] [-mc Mask] [-md Callback_DLL] [-mk]
                [-n Count]
                [-s Seconds]
                [-c|-cl CPU_Usage [-u]]
                [-m|-ml Commit_Usage]
                [-p|-pl Counter_Threshold]
                [-h]
                [-e [1 [-g] [-b]]]
                [-l]
                [-t]
                [-f  Include_Filter, ...]
                [-fx Exclude_Filter, ...]
                [-o]
                [-r [1..5] [-a]]
                [-wer]
                [-64]
                {
                 {{[-w] Process_Name | Service_Name | PID} [Dump_File | Dump_Folder]}
                |
                 {-x Dump_Folder Image_File [Argument, ...]}
                }
				
<SNIP>
```

Dəstə **Process Explorer** (Task Manager-in təkmilləşdirilmiş versiyası) və sistemdə işləyən hər hansı bir proseslə əlaqəli fayl sistemi, reyestr və şəbəkə fəaliyyətini izləmək üçün istifadə edilə bilən **Process Monitor** kimi alətləri əhatə edir. Bəzi əlavə alətlər arasında internet fəaliyyətini izləmək üçün istifadə olunan **TCPView** və SMB protokolu vasitəsilə sistemləri uzaqdan idarə etmək/qoşulmaq üçün istifadə edilə bilən **PSExec** var.

Bu alətlər penetrasiya testçiləri üçün maraqlı prosesləri və mümkün **imtiyaz artırma yollarını** kəşf etmək, həmçinin **yan hərəkət (lateral movement)** üçün faydalı ola bilər.

### Task Manager (Tapşırıq İdarəedici)

**Windows Task Manager** Windows sistemlərini idarə etmək üçün güclü bir vasitədir. O, işləyən proseslər, sistem performansı, işləyən xidmətlər, başlanğıc proqramları, daxil olmuş istifadəçilər/daxil olmuş istifadəçi prosesləri və xidmətlər haqqında məlumat verir. Task Manager tapşırıq çubuğunda sağ düyməni basıb **Task Manager** seçməklə, **Ctrl + Shift + Esc** basaraq, **Ctrl + Alt + Del** basıb **Task Manager** seçməklə, başlanğıc menyusunu açaraq **Task Manager** yazmaqla və ya CMD və ya PowerShell konsolundan `taskmgr` yazmaqla açıla bilər.

<img width="1630" height="744" alt="image" src="https://github.com/user-attachments/assets/82ea0cfc-b09f-4de2-91dc-fc7f95842f8c" />

| Tab | Təsviri |
| :--- | :--- |
| **Processes (Proseslər)** nişanı | İşləyən tətbiqlərin və arxa plan proseslərinin siyahısını, hər biri üçün **CPU**, **yaddaş (memory)**, **disk**, **şəbəkə** və **enerji istifadəsini** göstərir. |
| **Performance (Performans)** nişanı | **CPU istifadəsi**, **sistem işləmə müddəti (uptime)**, **yaddaş istifadəsi**, **disk**, **şəbəkə** və **GPU istifadəsi** kimi qrafikləri və məlumatları göstərir. Biz həmçinin cari **CPU**, **Yaddaş**, **Disk** və **Şəbəkə** resurs istifadəsinə daha dərin bir baxış təqdim edən **Resource Monitor** (Resurs İzləyicisi) aça bilərik. |

<img width="1572" height="896" alt="image" src="https://github.com/user-attachments/assets/a493148a-4cd3-4bbe-bb90-ab1c39aab62f" />

| Tab | Təsviri |
| :--- | :--- |
| **App history (Tətbiq Tarixçəsi)** nişanı | Müəyyən bir müddət üçün cari istifadəçi hesabı üçün hər bir tətbiqin resurs istifadəsini göstərir. |
| **Startup (Başlanğıc)** nişanı | Hansı tətbiqlərin yüklənmə zamanı başlamaq üçün konfiqurasiya edildiyini, habelə başlanğıc prosesinə təsirini göstərir. |
| **Users (İstifadəçilər)** nişanı | Daxil olmuş istifadəçiləri və onların sessiyası ilə əlaqəli prosesləri/resurs istifadəsini göstərir. |
| **Details (Ətraflı Məlumat)** nişanı | Hər bir işləyən tətbiq üçün **adını**, **proses ID-sini (PID)**, **statusunu**, əlaqəli **istifadəçi adını**, **CPU** və **yaddaş istifadəsini** göstərir. |
| **Services (Xidmətlər)** nişanı | Hər bir quraşdırılmış xidmətin **adını**, **PID-sini**, **təsvirini** və **statusunu** göstərir. **Services** əlavəsinə bu nişandan da daxil olmaq olar. |

---

## Process Explorer

**Process Explorer** **Sysinternals** alətlər dəstinin bir hissəsidir. Bu alət bir proqram işləyərkən hansı **tutacaqların (handles)** və **DLL proseslərinin** yükləndiyini göstərə bilər. Process Explorer hal-hazırda işləyən proseslərin siyahısını göstərir və oradan biz bir görünüşdə prosesin hansı tutacaqları seçdiyini və ya başqa bir görünüşdə yüklənmiş **DLL-ləri** və yaddaşda dəyişdirilmiş faylları görə bilərik. Biz həmçinin xüsusi bir tutacağa və ya DLL-ə hansı proseslərin bağlı olduğunu göstərmək üçün alət daxilində axtarış edə bilərik. Alət həm də hansı uşaq proseslərin bir tətbiq tərəfindən yaradıldığını görmək və bir proses sonlandırıldıqda geridə qala bilən **yetim proseslər (orphaned processed)** kimi hər hansı bir problemi aradan qaldırmaq üçün **valideyn-uşaq proses əlaqələrini** təhlil etmək üçün istifadə edilə bilər.

















