## Windows Əməliyyat Sistemi ilə Qarşılıqlı Əlaqə

### Qrafik İstifadəçi İnterfeysi (GUI)

Qrafik İstifadəçi İnterfeysi (GUI) konsepsiyası 1970-ci illərin sonlarında **Xerox Palo Alto Tədqiqat Laboratoriyası** tərəfindən təqdim edildi. O, əmr sətrində naviqasiya etməkdə çətinlik çəkə biləcək gündəlik istifadəçilər üçün istifadə imkanlarını nəzərə almaq məqsədilə Apple və Microsoft əməliyyat sistemlərinə əlavə edildi. Əksər adi Windows kompüter istifadəçilərinin əmr sətri vasitəsilə əməliyyat sistemi ilə qarşılıqlı əlaqəyə girməsinə heç vaxt ehtiyac yoxdur. Adından da göründüyü kimi, **GUI** istifadəçilərə əməliyyat sistemi və quraşdırılmış tətbiqlər və xidmətlərlə qarşılıqlı əlaqə qurmaq üçün interaktiv **göstər və kliklə** interfeysi təmin edir.

GUI-nin tətbiqi, istifadəçilərin əmrləri əzbərləmək və ya hər hansı bir proqramlaşdırma dilini bilmədən kompüterləri ilə qarşılıqlı əlaqə qura biləcəyi üçün bir çox demoqrafiya arasında kompüterlərə geniş yayılmış maraq və girişi açdı. Sistem administratorları adətən **Active Directory-ni idarə etmək**, **IIS-i konfiqurasiya etmək** və ya **verilənlər bazaları ilə qarşılıqlı əlaqə qurmaq** üçün GUI əsaslı sistemlərdən istifadə edirlər.

### Uzaq Masaüstü Protokolu (RDP)

**RDP** Microsoft-a məxsus bir protokoldur, hansı ki, istifadəçiyə şəbəkə bağlantısı üzərindən uzaq bir sistemə qoşulmağa və qrafik istifadəçi interfeysi əldə etməyə imkan verir. İstifadəçi **RDP müştəri proqram təminatından** istifadə edərək **RDP server proqram təminatı** işləyən hədəf sistemə qoşulur. RDP məlumatları irəli və geri göndərmək üçün xüsusi bir şəbəkə kanalı açmaq üçün **3389 portundan** istifadə edir. RDP vasitəsilə qoşulduqda, istifadəçi kompüterin qarşısında oturub yerli olaraq daxil olduğu kimi GUI-yə daxil ola bilər. RDP tez-tez sistem administratorları tərəfindən uzaq sistemləri tez idarə etmək üçün istifadə olunur. Həmçinin, istifadəçilərə bir **Virtual Şəxsi Şəbəkəyə (VPN)** qoşulduqdan sonra səyahət edərkən və ya evdən işləyərkən öz iş kompüterlərinə daxil olmaq imkanı verir.

-----

## Windows Əmr Sətri

Əmr sətri interfeysləri istifadəçilərə öz sistemləri üzərində daha çox nəzarət verir və geniş çeşiddə gündəlik, inzibati və problemlərin həlli tapşırıqlarını yerinə yetirmək üçün istifadə edilə bilər. O, müəyyən tapşırıqları tez yerinə yetirmək üçün avtomatlaşdırmanı tətbiq etmək üçün istifadə edilə bilər (məsələn, bir anda çox sayda istifadəçini bir domenə əlavə etmək kimi). Windows əməliyyat sistemlərində əmr sətrindən sistemlə qarşılıqlı əlaqə qurmağın əsas iki yolu **Command Prompt (CMD)** və **PowerShell** vasitəsilədir.

Microsoft-dan **Windows Əmr İstinadı** əksər Windows əmrləri üçün ümumi bir baxış, istifadə nümunələri və əmr sintaksisi daxil olmaqla, A-dan Z-yə əhatəli bir əmr istinadıdır və onunla tanış olmaq tövsiyə olunur.

### CMD

**Command Prompt** (`cmd.exe`) əmrləri daxil etmək və icra etmək üçün istifadə olunur. İstifadəçi IP ünvanı məlumatlarını görmək üçün `ipconfig` kimi təkrar olunan əmrlər daxil edə bilər və ya **planlanmış tapşırıqları qurmaq** və ya **skriptlər və toplu fayllar** (batch files) yaratmaq kimi daha qabaqcıl tapşırıqları yerinə yetirə bilər. Command prompt **Start Menyusundan**, **run dialoq qutusuna** `cmd` yazmaqla və ya ikilik faylı `C:\Windows\system32\cmd.exe` yolundan birbaşa işə salmaqla açıla bilər.

`cmd.exe`-ni işə saldıqdan sonra, mövcud əmrlərin siyahısını görmək üçün `help` yaza bilərik.

```bash
C:\htb> help
For more information on a specific command, type HELP command-name
ASSOC          Displays or modifies file extension associations.
ATTRIB         Displays or changes file attributes.
BREAK          Sets or clears extended CTRL+C checking.
BCDEDIT        Sets properties in boot database to control boot loading.
CACLS          Displays or modifies access control lists (ACLs) of files.
CALL           Calls one batch program from another.
CD             Displays the name of or changes the current directory.
CHCP           Displays or sets the active code page number.
CHDIR          Displays the name of or changes the current directory.
CHKDSK         Checks a disk and displays a status report.
CHKNTFS        Displays or modifies the checking of disk at boot time.
CLS            Clears the screen.
CMD            Starts a new instance of the Windows command interpreter.
COLOR          Sets the default console foreground and background colors.
COMP           Compares the contents of two files or sets of files.
COMPACT        Displays or alters the compression of files on NTFS partitions.
CONVERT        Converts FAT volumes to NTFS.  You cannot convert the
               current drive.
COPY           Copies one or more files to another location.

<SNIP>
```

Xüsusi bir əmr haqqında daha çox məlumat üçün `help <command-name>` yaza bilərik.

```bash
C:\htb> help schtasks

SCHTASKS /parameter [arguments]

Description:
    Enables an administrator to create, delete, query, change, run and
    end scheduled tasks on a local or remote system.

Parameter List:
    /Create         Creates a new scheduled task.

    /Delete         Deletes the scheduled task(s).

    /Query          Displays all scheduled tasks.

    /Change         Changes the properties of scheduled task.

    /Run            Runs the scheduled task on demand.

    /End            Stops the currently running scheduled task.

    /ShowSid        Shows the security identifier corresponding to a scheduled task name.

    /?              Displays this help message.

Examples:
    SCHTASKS
    SCHTASKS /?
    SCHTASKS /Run /?
    SCHTASKS /End /?
    SCHTASKS /Create /?
    SCHTASKS /Delete /?
    SCHTASKS /Query  /?
    SCHTASKS /Change /?
    SCHTASKS /ShowSid /?
```

Qeyd edin ki, müəyyən əmrlərin öz kömək menyuları var, hansı ki, `<command> /?` yazmaqla əldə edilə bilər. Məsələn, `ipconfig` əmri haqqında məlumat aşağıda görünə bilər.

```bash
C:\htb> ipconfig /?

USAGE:
    ipconfig [/allcompartments] [/? | /all |
                                 /renew [adapter] | /release [adapter] |
                                 /renew6 [adapter] | /release6 [adapter] |
                                 /flushdns | /displaydns | /registerdns |
                                 /showclassid adapter |
                                 /setclassid adapter [classid] |
                                 /showclassid6 adapter |
                                 /setclassid6 adapter [classid] ]

where
    adapter             Connection name
                       (wildcard characters * and ? allowed, see examples)

    Options:
       /?               Display this help message
       /all             Display full configuration information.
       /release         Release the IPv4 address for the specified adapter.
       /release6        Release the IPv6 address for the specified adapter.
       /renew           Renew the IPv4 address for the specified adapter.
       /renew6          Renew the IPv6 address for the specified adapter.
       /flushdns        Purges the DNS Resolver cache.
       /registerdns     Refreshes all DHCP leases and re-registers DNS names
       /displaydns      Display the contents of the DNS Resolver Cache.
       /showclassid     Displays all the dhcp class IDs allowed for adapter.
       /setclassid      Modifies the dhcp class id.
       /showclassid6    Displays all the IPv6 DHCP class IDs allowed for adapter.
       /setclassid6     Modifies the IPv6 DHCP class id.

<SNIP>
```

-----

### PowerShell

**Windows PowerShell** Microsoft tərəfindən sistem administratorlarına daha çox yönəldilmiş bir əmr qabığıdır (*command shell*). PowerShell, Windows əmr sətri kimi, həm interaktiv əmr sorğusuna, həm də güclü bir **skriptləşdirmə mühitinə** malikdir. PowerShell Windows-da tətbiqlərin qurulması və işlədilməsi üçün istifadə olunan **.NET Framework** üzərində qurulub. Bu, onu birbaşa əməliyyat sistemi ilə əlaqə qurmaq üçün çox güclü bir vasitə edir.

Əmr sətri kimi, PowerShell də bizə fayl sisteminə birbaşa giriş imkanı verir və biz CMD qabığı daxilində işlədə biləcəyimiz əmrlərin əksəriyyətini işlədə bilərik.

#### Cmdletlər

PowerShell qabığa daxil edilmiş kiçik, tək funksiyalı alətlər olan **cmdletlərdən** istifadə edir. 100-dən çox əsas cmdlet mövcuddur və bir çox əlavə cmdlet yazılmışdır, yaxud daha mürəkkəb tapşırıqları yerinə yetirmək üçün öz cmdletlərimizi yarada bilərik. PowerShell həmçinin sistem idarəetmə tapşırıqları, avtomatlaşdırma və daha çox şey üçün istifadə olunan həm sadə, həm də mürəkkəb skriptləri də dəstəkləyir.

Cmdletlər **Feyl-İsim (Verb-Noun)** formasındadır. Məsələn, `Get-ChildItem` əmri cari qovluğumuzu siyahılamaq üçün istifadə edilə bilər. Cmdletlər həmçinin arqumentlər və ya bayraqlar qəbul edir. Biz `Get-ChildItem -` yazıb tab düyməsini basaraq arqumentlər arasında keçid edə bilərik. `Get-ChildItem -Recurse` kimi bir əmr bizə cari iş qovluğumuzun və bütün altqovluqların məzmununu göstərəcək. Başqa bir nümunə, başqa bir qovluğun məzmununu əldə etmək üçün `Get-ChildItem -Path C:\Users\Administrator\Documents` olardı. Nəhayət, başqa bir qovluq daxilindəki bütün altqovluqların məzmununu rekursiv olaraq siyahılamaq üçün arqumentləri birləşdirə bilərik: `Get-ChildItem -Path C:\Users\Administrator\Downloads -Recurse`.

#### Təxəllüslər (Aliases)

PowerShell-də bir çox cmdletin təxəllüsü (alias) də var. Məsələn, qovluqları dəyişdirmək üçün istifadə olunan `Set-Location` cmdletinin təxəllüsləri ya `cd`, ya da `sl`-dir. Bu vaxt, `Get-ChildItem`-in təxəllüsləri `ls` və `gci`-dir. Bütün mövcud təxəllüsləri `Get-Alias` yazaraq görə bilərik.

```bash
PS C:\htb> get-alias

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           % -> ForEach-Object
Alias           ? -> Where-Object
Alias           ac -> Add-Content
Alias           asnp -> Add-PSSnapin
Alias           cat -> Get-Content
Alias           cd -> Set-Location
Alias           CFS -> ConvertFrom-String                          3.1.0.0    Microsoft.PowerShell.Utility
Alias           chdir -> Set-Location
Alias           clc -> Clear-Content
Alias           clear -> Clear-Host
Alias           clhy -> Clear-History
Alias           cli -> Clear-Item
Alias           clp -> Clear-ItemProperty
```

Biz həmçinin `New-Alias` ilə öz təxəllüslərimizi qura bilərik və `Get-Alias -Name` ilə hər hansı bir cmdlet üçün təxəllüsü əldə edə bilərik.

```powershell
PS C:\htb> New-Alias -Name "Show-Files" Get-ChildItem
PS C:\> Get-Alias -Name "Show-Files"

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Show-Files
```

PowerShell-in cmdletlər, funksiyalar, skriptlər və konsepsiyalar üçün bir kömək sistemi var. Bu, defolt olaraq quraşdırılmayıb, lakin biz ya veb-brauzerimizdə bir cmdlet və ya funksiya üçün onlayn köməyi açmaq üçün `Get-Help <cmdlet-name> -Online` əmrini işlədə bilərik, ya da yerli olaraq kömək fayllarını yükləmək və quraşdırmaq üçün `Update-Help` yaza bilərik.

```powershell
PS C:\htb> help

TOPIC
    Windows PowerShell Help System

SHORT DESCRIPTION
    Displays help about Windows PowerShell cmdlets and concepts.

LONG DESCRIPTION
    Windows PowerShell Help describes Windows PowerShell cmdlets,
    functions, scripts, and modules, and explains concepts, including
    the elements of the Windows PowerShell language.

    Windows PowerShell does not include help files, but you can read the
    help topics online, or use the Update-Help cmdlet to download help files
    to your computer and then use the Get-Help cmdlet to display the help
    topics at the command line.

    You can also use the Update-Help cmdlet to download updated help files
    as they are released so that your local help content is never obsolete.

    Without help files, Get-Help displays auto-generated help for cmdlets,
    functions, and scripts.


  ONLINE HELP
    You can find help for Windows PowerShell online in the TechNet Library
    beginning at http://go.microsoft.com/fwlink/?LinkID=108518.

    To open online help for any cmdlet or function, type:

        Get-Help <cmdlet-name> -Online
		
<SNIP>
```

`Get-Help Get-AppPackage` kimi bir əmr yazmaq, Kömək faylları quraşdırılmadığı təqdirdə yalnız qismən köməyi qaytaracaq.

```powershell
PS C:\htb>  Get-Help Get-AppPackage

NAME
    Get-AppxPackage

SYNTAX
    Get-AppxPackage [[-Name] <string>] [[-Publisher] <string>] [-AllUsers] [-PackageTypeFilter {None | Main |
    Framework | Resource | Bundle | Xap | Optional | All}] [-User <string>] [-Volume <AppxVolume>]
    [<CommonParameters>]


ALIASES
    Get-AppPackage


REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.
```

#### Skriptlərin İşlədilməsi

**PowerShell ISE (Integrated Scripting Environment)** istifadəçilərə yerində PowerShell skriptləri yazmağa imkan verir. O, həmçinin PowerShell əmrləri üçün avtomatik tamamlama/axtarış funksiyasına malikdir. PowerShell ISE, sürətli sazlamaya imkan verən eyni konsolda skriptləri yazmağa və işlətməyə imkan verir.

PowerShell skriptlərini müxtəlif yollarla işlədə bilərik. Əgər funksiyaları biliriksə, skripti ya yerli olaraq, ya da aşağıdakı nümunədəki kimi bir yükləmə yuvası (*download cradle*) ilə yaddaşa yüklədikdən sonra işlədə bilərik.

```powershell
PS C:\htb> .\PowerView.ps1;Get-LocalGroup |fl

Description     : Users of Docker Desktop
Name            : docker-users
SID             : S-1-5-21-674899381-4069889467-2080702030-1004
PrincipalSource : Local
ObjectClass     : Group

Description     : VMware User Group
Name            : __vmware__
SID             : S-1-5-21-674899381-4069889467-2080702030-1003
PrincipalSource : Local
ObjectClass     : Group

Description     : Members of this group can remotely query authorization attributes and permissions for resources on
                  this computer.
Name            : Access Control Assistance Operators
SID             : S-1-5-32-579
PrincipalSource : Local
ObjectClass     : Group

Description     : Administrators have complete and unrestricted access to the computer/domain
Name            : Administrators
SID             : S-1-5-32-544
PrincipalSource : Local

<SNIP>
```

PowerShell-də bir skriptlə işləməyin ümumi yollarından biri, bütün funksiyaların cari PowerShell konsol sessiyamızda mövcud olması üçün onu **idxal etməkdir**: `Import-Module .\PowerView.ps1`. Daha sonra ya bir əmr başlada və seçimlər arasında dövr edə, ya da bütün yüklənmiş modulları və onlarla əlaqəli əmrləri siyahılamaq üçün `Get-Module` yaza bilərik.

```powershell
PS C:\htb> Get-Module | select Name,ExportedCommands | fl


Name             : Appx
ExportedCommands : {[Add-AppxPackage, Add-AppxPackage], [Add-AppxVolume, Add-AppxVolume], [Dismount-AppxVolume,
                   Dismount-AppxVolume], [Get-AppxDefaultVolume, Get-AppxDefaultVolume]...}

Name             : Microsoft.PowerShell.LocalAccounts
ExportedCommands : {[Add-LocalGroupMember, Add-LocalGroupMember], [Disable-LocalUser, Disable-LocalUser],
                   [Enable-LocalUser, Enable-LocalUser], [Get-LocalGroup, Get-LocalGroup]...}

Name             : Microsoft.PowerShell.Management
ExportedCommands : {[Add-Computer, Add-Computer], [Add-Content, Add-Content], [Checkpoint-Computer,
                   Checkpoint-Computer], [Clear-Content, Clear-Content]...}

Name             : Microsoft.PowerShell.Utility
ExportedCommands : {[Add-Member, Add-Member], [Add-Type, Add-Type], [Clear-Variable, Clear-Variable], [Compare-Object,
                   Compare-Object]...}

Name             : PSReadline
ExportedCommands : {[Get-PSReadLineKeyHandler, Get-PSReadLineKeyHandler], [Get-PSReadLineOption,
                   Get-PSReadLineOption], [Remove-PSReadLineKeyHandler, Remove-PSReadLineKeyHandler],
                   [Set-PSReadLineKeyHandler, Set-PSReadLineKeyHandler]...}
```

#### İcra Siyasəti (Execution Policy)

Bəzən bir sistemdə skriptləri işlədə bilmədiyimizi görəcəyik. Bu, zərərli skriptlərin icrasının qarşısını almağa çalışan **icra siyasəti** (*execution policy*) adlı bir təhlükəsizlik xüsusiyyətinə görədir. Mümkün siyasətlər bunlardır:

| Siyasət | Təsviri |
| :--- | :--- |
| **AllSigned** | Bütün skriptlər işləyə bilər, lakin etibarlı bir nəşriyyatçı tərəfindən imzalanmalıdır. Buna həm uzaq, həm də yerli skriptlər daxildir. Hələ etibarlı və ya etibarsız olaraq siyahıya almadığımız nəşriyyatçılar tərəfindən imzalanmış skriptləri işlətməzdən əvvəl bir sorğu alırıq. |
| **Bypass** | Heç bir skript və ya konfiqurasiya faylı bloklanmır və istifadəçi heç bir xəbərdarlıq və ya sorğu almır. |
| **Default** | Bu, Windows masaüstü maşınları üçün defolt icra siyasətini (**Restricted**) və Windows serverləri üçün isə (**RemoteSigned**) təyin edir. |
| **RemoteSigned** | Skriptlər işləyə bilər, lakin internetdən yüklənmiş skriptlərdə rəqəmsal imza tələb olunur. Yerli olaraq yazılmış skriptlər üçün rəqəmsal imzalar tələb olunmur. |
| **Restricted** | Bu, fərdi əmrlərə icazə verir, lakin skriptlərin işlədilməsinə icazə vermir. Konfiqurasiya faylları (.ps1xml), modul skript faylları (.psm1) və PowerShell profilləri (.ps1) daxil olmaqla, bütün skript fayl növləri bloklanır. |
| **Undefined** | Cari əhatə dairəsi üçün heç bir icra siyasəti təyin edilməyib. Əgər BÜTÜN əhatə dairələri üçün icra siyasəti təyin edilməyibsə, o zaman defolt icra siyasəti (**Restricted**) istifadə olunacaq. |
| **Unrestricted** | Bu, Windows olmayan kompüterlər üçün defolt icra siyasətidir və dəyişdirilə bilməz. Bu siyasət imzasız skriptlərin işlədilməsinə icazə verir, lakin yerli intranet zonasından olmayan skriptləri işlətməzdən əvvəl istifadəçini xəbərdar edir. |

Aşağıda bütün əhatə dairələri üçün cari icra siyasətinə bir nümunə verilmişdir.

```powershell
PS C:\htb> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       Undefined
 LocalMachine    RemoteSigned
```

İcra siyasəti istifadəçi hərəkətlərini məhdudlaşdıran bir təhlükəsizlik nəzarəti olmaq üçün nəzərdə tutulmayıb. İstifadəçi skript məzmununu birbaşa PowerShell pəncərəsinə yazaraq, skripti yükləyib çağıraraq və ya skripti kodlanmış bir əmr kimi göstərərək siyasəti asanlıqla **keçə bilər** (*bypass*). O, həmçinin icra siyasətini tənzimləməklə (istifadəçinin müvafiq hüquqları varsa) və ya cari proses əhatə dairəsi üçün icra siyasətini təyin etməklə (hansı ki, demək olar ki, hər hansı bir istifadəçi tərəfindən edilə bilər, çünki konfiqurasiya dəyişikliyi tələb etmir və yalnız istifadəçinin sessiyası müddətində təyin ediləcək) keçilə bilər.

Aşağıda cari proses (sessiya) üçün icra siyasətinin dəyişdirilməsinə bir nümunə verilmişdir.

```powershell
PS C:\htb> Set-ExecutionPolicy Bypass -Scope Process

Execution Policy Change
The execution policy helps protect you from scripts that you do not trust. Changing the execution policy might expose
you to the security risks described in the about_Execution_Policies help topic at
https:/go.microsoft.com/fwlink/?LinkID=135170. Do you want to change the execution policy?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): Y
```

İndi icra siyasətinin dəyişdirildiyini görə bilərik.

```powershell
PS C:\htb>  Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process          Bypass
  CurrentUser       Undefined
 LocalMachine    RemoteSigned
```
