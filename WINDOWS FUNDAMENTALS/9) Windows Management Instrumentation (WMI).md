## Windows Management Instrumentation (WMI)

**WMI** sistem administratorlarına sistem monitorinqi üçün güclü alətlər təqdim edən **PowerShell-in bir alt sistemidir**. WMI-nin məqsədi korporativ şəbəkələrdə cihaz və tətbiq idarəetməsini konsolidasiya etməkdir. WMI Windows əməliyyat sisteminin əsas hissəsidir və Windows 2000-dən bəri əvvəlcədən quraşdırılmışdır. O, aşağıdakı komponentlərdən ibarətdir:

| Komponent Adı | Təsviri |
| :--- | :--- |
| **WMI Xidməti** | Windows Management Instrumentation prosesi, hansı ki, yüklənmə zamanı avtomatik işləyir və WMI təminatçıları, WMI anbarı və idarəedici tətbiqlər arasında vasitəçi kimi çıxış edir. |
| **İdarə Olunan Obyektlər** | WMI tərəfindən idarə oluna bilən hər hansı məntiqi və ya fiziki komponentlər. |
| **WMI Təminatçıları** | Xüsusi bir obyektlə əlaqəli hadisələri/məlumatları monitorinq edən obyektlər. |
| **Siniflər** | WMI təminatçıları tərəfindən WMI xidmətinə məlumat ötürmək üçün istifadə olunur. |
| **Metodlar** | Siniflərə qoşulur və hərəkətlərin yerinə yetirilməsinə imkan verir. Məsələn, metodlar uzaq maşınlarda prosesləri başlamaq/dayandırmaq üçün istifadə edilə bilər. |
| **WMI Anbarı** | WMI ilə əlaqəli bütün statik məlumatları saxlayan verilənlər bazası. |
| **CIM Obyekt İdarəedicisi** | WMI təminatçılarından məlumat tələb edən və tələb edən tətbiqə qaytaran sistem. |
| **WMI API** | Tətbiqlərə WMI infrastrukturuna daxil olmağa imkan verir. |
| **WMI İstehlakçısı** | CIM Obyekt İdarəedicisi vasitəsilə obyektlərə sorğular göndərir. |

WMI-nin bəzi istifadə sahələri bunlardır:

  * Yerli/uzaq sistemlər üçün **status məlumatları**
  * Uzaq maşınlarda/tətbiqlərdə **təhlükəsizlik tənzimləmələrinin konfiqurasiyası**
  * İstifadəçi və qrup **icazələrinin təyin edilməsi və dəyişdirilməsi**
  * Sistem xüsusiyyətlərinin **təyin edilməsi/dəyişdirilməsi**
  * **Kod icrası**
  * **Proseslərin planlaşdırılması**
  * **Jurnallaşdırmanın qurulması**

Bu tapşırıqların hamısı **PowerShell** və **WMI Əmr Sətri İnterfeysi (WMIC)**-nin birləşməsindən istifadə edilərək yerinə yetirilə bilər. WMI, interaktiv bir qabıq açmaq üçün **WMIC** yazmaqla və ya hostname-i əldə etmək üçün `wmic computersystem get name` kimi bir əmri birbaşa işlətməklə Windows əmr sətri vasitəsilə işlədilə bilər. WMIC əmrlərinin və təxəllüslərinin siyahısını `WMIC /?` yazaraq görə bilərik.

```bash
C:\htb> wmic /?

WMIC is deprecated.

[global switches] <command>

The following global switches are available:
/NAMESPACE           Path for the namespace the alias operate against.
/ROLE                Path for the role containing the alias definitions.
/NODE                Servers the alias will operate against.
/IMPLEVEL            Client impersonation level.
/AUTHLEVEL           Client authentication level.
/LOCALE              Language id the client should use.
/PRIVILEGES          Enable or disable all privileges.
/TRACE               Outputs debugging information to stderr.
/RECORD              Logs all input commands and output.
/INTERACTIVE         Sets or resets the interactive mode.
/FAILFAST            Sets or resets the FailFast mode.
/USER                User to be used during the session.
/PASSWORD            Password to be used for session login.
/OUTPUT              Specifies the mode for output redirection.
/APPEND              Specifies the mode for output redirection.
/AGGREGATE           Sets or resets aggregate mode.
/AUTHORITY           Specifies the <authority type> for the connection.
/?[:<BRIEF|FULL>]    Usage information.

For more information on a specific global switch, type: switch-name /?

Press any key to continue, or press the ESCAPE key to stop
```

Aşağıdakı əmr nümunəsi əməliyyat sistemi haqqında məlumatları siyahılayır.

```bash
C:\htb> wmic os list brief

BuildNumber  Organization  RegisteredUser  SerialNumber             SystemDirectory      Version
19041                      Owner           00123-00123-00123-AAOEM  C:\Windows\system32  10.0.19041
```

**WMIC** təxəllüslərdən və əlaqəli fellərdən (*verbs*), zərflərdən (*adverbs*) və açarlardan (*switches*) istifadə edir. Yuxarıdakı əmr nümunəsi məlumatları göstərmək üçün **LIST** istifadə edir və yalnız əsas xüsusiyyətlər dəstini təmin etmək üçün **BRIEF** zərfini istifadə edir. Fellər, açarlar və zərflərin ətraflı siyahısı **burada** mövcuddur. WMI, **Get-WmiObject** modulu istifadə edilərək PowerShell ilə istifadə edilə bilər. Bu modul WMI siniflərinin nümunələrini və ya mövcud siniflər haqqında məlumatları əldə etmək üçün istifadə olunur. Bu modul yerli və ya uzaq maşınlara qarşı istifadə edilə bilər.

Burada əməliyyat sistemi haqqında məlumatları əldə edə bilərik.

```powershell
PS C:\htb> Get-WmiObject -Class Win32_OperatingSystem | select SystemDirectory,BuildNumber,SerialNumber,Version | ft

SystemDirectory     BuildNumber SerialNumber            Version
---------------     ----------- ------------            -------
C:\Windows\system32 19041       00123-00123-00123-AAOEM 10.0.19041
```

Biz həmçinin WMI obyektlərinin metodlarını çağırmaq üçün istifadə olunan **Invoke-WmiMethod** modulunu da istifadə edə bilərik. Sadə bir nümunə, bir faylın adını dəyişməkdir. `ReturnValue` 0 olduğu üçün əmrin düzgün tamamlandığını görə bilərik.

```powershell
PS C:\htb> Invoke-WmiMethod -Path "CIM_DataFile.Name='C:\users\public\spns.csv'" -Name Rename -ArgumentList "C:\Users\Public\kerberoasted_users.csv"


__GENUS          : 2
__CLASS          : __PARAMETERS
__SUPERCLASS     :
__DYNASTY        : __PARAMETERS
__RELPATH        :
__PROPERTY_COUNT : 1
__DERIVATION     : {}
__SERVER         :
__NAMESPACE      :
__PATH           :
ReturnValue      : 0
PSComputerName   :
```

Bu bölmə **WMI**, **WMIC** və **WMIC** ilə **PowerShell**-in birləşdirilməsinə qısa bir ümumi baxış təqdim edir. **WMI** həm **mavi komanda** (*blue team*), həm də **qırmızı komanda** (*red team*) operatorları üçün geniş istifadə sahələrinə malikdir. Bu kursun sonrakı bölmələri WMI-nin həm **nömrələmə (enumeration)**, həm də **yan hərəkət (lateral movement)** üçün hücum məqsədi ilə necə istifadə edilə biləcəyini göstərəcək.
