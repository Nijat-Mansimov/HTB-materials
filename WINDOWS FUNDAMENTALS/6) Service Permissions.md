## Xidmət İcazələri

Xatırladaq ki, xidmətlər uzun müddət davam edən proseslərin idarə edilməsinə imkan verir və Windows əməliyyat sistemlərinin **kritik** bir hissəsidir. Sistem administratorları çox vaxt onlara zərərli **DLL-ləri yükləmək**, administrator hesabına ehtiyac olmadan tətbiqləri icra etmək, **imtiyazları artırmaq** və hətta **davamlılığı (persistence)** qorumaq üçün istifadə oluna bilən potensial təhlükə vektorları kimi məhəl qoymurlar. Windows xidmətlərindəki bu təhlükə vektorları çox vaxt **üçüncü tərəf proqram təminatı** tərəfindən tətbiq edilən **xidmət icazələrinin səhv konfiqurasiyaları** və quraşdırma prosesləri zamanı adminlər tərəfindən edilə bilən asan səhvlər nəticəsində yaranır.

Xidmət icazələrinin vacibliyini dərk etmək üçün ilk addım sadəcə onların mövcud olduğunu **başa düşmək** və onları **nəzərə almaqdır**. Server əməliyyat sistemlərində, **DHCP** və **Active Directory Domain Services** kimi kritik şəbəkə xidmətləri adətən quraşdırmanı həyata keçirən **adminə təyin olunmuş hesab** istifadə edilərək quraşdırılır. Quraşdırma prosesinin bir hissəsi, xidmətin təyin olunmuş bir istifadəçinin etimadnamələri və imtiyazları ilə işləməsini təyin etməyi əhatə edir, bu da defolt olaraq cari daxil olmuş istifadəçi kontekstində təyin edilir.

Məsələn, əgər biz DHCP quraşdırılması zamanı bir serverdə **Bob** kimi daxil olmuşuqsa, o zaman bu xidmət başqa cür göstərilmədiyi təqdirdə **Bob kimi işləmək** üçün konfiqurasiya ediləcək. Bundan hansı pis nəticələr çıxa bilər? Yaxşı, əgər Bob təşkilatı tərk etsə və ya işdən çıxarılsa nə olar? Tipik biznes təcrübəsi, Bobun çıxış prosesinin bir hissəsi olaraq onun hesabını **ləğv etmək** olardı. Bu halda, Bobun hesabından istifadə edən DHCP və digər xidmətlərlə nə baş verərdi? Həmin xidmətlər **başlamaqda uğursuzluğa düçar olardı**. **DHCP** və ya Dinamik Host Konfiqurasiya Protokolu şəbəkədəki kompüterlərə **IP ünvanları icarəyə verməkdən** məsuliyyət daşıyır. Əgər bu xidmət bir Windows DHCP serverində dayansa, IP ünvanı tələb edən müştərilər heç bir ünvan almayacaq. Bu o deməkdir ki, bir xidmət səhv konfiqurasiyası **dayanmaya** və **məhsuldarlığın itirilməsinə** səbəb ola bilər. Kritik şəbəkə xidmətlərini işlətmək üçün **fərdi istifadəçi hesabı** yaratmaq çox tövsiyə olunur. Bunlara **xidmət hesabları (service accounts)** deyilir.

Biz həmçinin xidmət icazələrinə və onların icra olunduğu qovluqların icazələrinə diqqət yetirməliyik, çünki **icra edilə bilən faylın yolunu** zərərli bir DLL və ya icra edilə bilən fayl ilə əvəz etmək mümkündür. Bunu daha yaxşı başa düşmək üçün Windows 10-da işləyən xidmətlərin icazələrini araşdıraq.

## `services.msc` İstifadə Edərək Xidmətləri Araşdırma

Proseslər və xidmətlər bölməsində müzakirə edildiyi kimi, bütün xidmətlərlə bağlı demək olar ki, hər bir detalı görmək və idarə etmək üçün `services.msc` istifadə edə bilərik. Gəlin **Windows Update** (`wuauserv`) ilə əlaqəli xidmətə daha yaxından nəzər salaq.

<img width="2040" height="1240" alt="image" src="https://github.com/user-attachments/assets/d564e2ce-de55-4e75-beb5-500f7ea58246" />

Mövcud olan fərqli xüsusiyyətləri görmək və konfiqurasiya etmək üçün qeyd edin. **Xidmət adını** bilmək, xidmətləri araşdırmaq və idarə etmək üçün əmr sətri alətlərindən istifadə edərkən xüsusilə faydalıdır. **İcra edilə bilən faylın yolu** (Path to the executable) xidmət başlayanda icra ediləcək proqrama və əmrə gedən tam yoldur. Əgər təyinat qovluğunun **NTFS icazələri zəif konfiqurasiya edilibsə**, bir hücumçu orijinal icra edilə bilən faylı **zərərli məqsədlər üçün yaradılmış** başqa bir faylla əvəz edə bilər. Biz NTFS icazələrini bu modulun **"NTFS vs. Paylaşma İcazələri"** bölməsində daha ətraflı müzakirə edirik.

<img width="1020" height="767" alt="image" src="https://github.com/user-attachments/assets/00177b6a-cf45-4387-a4ff-5d48e69ccd7e" />

Əksər xidmətlər defolt olaraq **LocalSystem** imtiyazları ilə işləyir ki, bu da fərdi Windows ƏS-də icazə verilən **ən yüksək giriş səviyyəsidir**. Bütün tətbiqlərin Local System hesabı səviyyəsində icazələrə ehtiyacı yoxdur, buna görə də bir Windows mühitində yeni tətbiqlər quraşdırmaq barədə düşünərkən **hər bir vəziyyət üzrə ayrıca tədqiqat aparmaq** faydalıdır. **Ən az imtiyaz (Principle of Least Privilege)** prinsipinə uyğun olmaq üçün mümkün olan ən az imtiyazla işləyə bilən tətbiqləri müəyyən etmək yaxşı bir praktikadır.

Ən az imtiyaz prinsipinin bir izahı:

## Windows-da Qeyd Edilən Daxili Xidmət Hesabları:

* **LocalService**
* **NetworkService**
* **LocalSystem**

**Qeyd:** Biz həmçinin yeni hesablar yarada və onlardan yalnız bir xidməti işlətmək məqsədi ilə istifadə edə bilərik.

<img width="1020" height="668" alt="image" src="https://github.com/user-attachments/assets/04c65f9a-28bf-4075-b1da-2caa1a4c33e1" />

Bərpa (Recovery) nişanı bir xidmət uğursuz olduqda hansı addımların konfiqurasiya edilə biləcəyini göstərir. Bu xidmətin **ilk uğursuzluqdan sonra bir proqram işə salmaq üçün** necə tənzimlənə biləcəyinə diqqət yetirin. Bu, bir hücumçunun **qanuni bir xidmətdən istifadə edərək zərərli proqramları işlətmək** üçün istifadə edə biləcəyi daha bir vektordur.

## `sc` İstifadə Edərək Xidmətləri Araşdırma

`sc` həmçinin xidmətləri konfiqurasiya etmək və idarə etmək üçün istifadə edilə bilər. Gəlin bir neçə əmrlə təcrübə edək.

```bash
C:\Users\htb-student>sc qc wuauserv
[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: wuauserv
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\WINDOWS\system32\svchost.exe -k netsvcs -p
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Update
        DEPENDENCIES       : rpcss
        SERVICE_START_NAME : LocalSystem
```

`sc qc` əmri xidməti sorğulamaq üçün istifadə olunur. Məhz burada xidmətlərin adlarını bilmək faydalı ola bilər. Əgər şəbəkə üzərindəki bir cihazda xidməti sorğulamaq istəsək, **hostname** və ya **IP ünvanını** birbaşa `sc`-dən sonra göstərə bilərik.

```bash
C:\Users\htb-student>sc //hostname or ip of box query ServiceName
```

Biz həmçinin xidmətləri başlamaq və dayandırmaq üçün də `sc` istifadə edə bilərik.

```bash
C:\Users\htb-student> sc stop wuauserv

[SC] OpenService FAILED 5:

Access is denied.
```

Bu hərəkəti **inzibati kontekstdə (elevated privileges)** işlətmədən icra etməyə **icazəmizin olmadığını** qeyd edin. Əgər əmr sətrini **artırılmış imtiyazlarla** işlətsək, bu hərəkəti tamamlamağa icazə veriləcək.

```bash
C:\WINDOWS\system32> sc config wuauserv binPath=C:\Winbows\Perfectlylegitprogram.exe

[SC] ChangeServiceConfig SUCCESS

C:\WINDOWS\system32> sc qc wuauserv

[SC] QueryServiceConfig SUCCESS

SERVICE_NAME: wuauserv
        TYPE               : 20  WIN32_SHARE_PROCESS
        START_TYPE         : 3   DEMAND_START
        ERROR_CONTROL      : 1   NORMAL
        BINARY_PATH_NAME   : C:\Winbows\Perfectlylegitprogram.exe
        LOAD_ORDER_GROUP   :
        TAG                : 0
        DISPLAY_NAME       : Windows Update
        DEPENDENCIES       : rpcss
        SERVICE_START_NAME : LocalSystem
```

Əgər sistemdə **zərərli proqram** olduğundan şübhələndiyimiz bir vəziyyəti araşdırsaq, `sc` bizə ümumi hədəf alınan xidmətləri və yeni yaradılmış xidmətləri tez bir zamanda axtarmaq və təhlil etmək imkanı verəcək. Həmçinin, `services.msc` kimi **GUI** alətlərindən istifadə etməkdən daha çox **skriptlərə uyğundur**.

`sc` istifadə edərək xidmət icazələrini araşdıra biləcəyimiz başqa bir faydalı yol isə `sdshow` əmri vasitəsilədir.

```bash
C:\WINDOWS\system32> sc sdshow wuauserv

D:(A;;CCLCSWRPLORC;;;AU)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)S:(AU;FA;CCDCLCSWRPWPDTLOSDRCWDWO;;;WD)
```

İlk baxışdan, çıxış qəribə görünür. Demək olar ki, əmrimizdə səhv bir şey etdiyimiz görünür, lakin bu qarışıqlığın bir mənası var. Windows-da adlandırılmış hər bir obyekt **təhlükəsizləşdirilə bilən obyektdir** (*securable object*), hətta bəzi adsız obyektlər də təhlükəsizləşdirilə biləndir. Əgər Windows ƏS-də təhlükəsizləşdirilə biləndirsə, onun **təhlükəsizlik təsvirçisi** (*security descriptor*) olacaq. Təhlükəsizlik təsvirçiləri obyektin sahibini və **İxtiyari Giriş Nəzarət Siyahısı (DACL)** və **Sistem Giriş Nəzarət Siyahısı (SACL)** ehtiva edən əsas bir qrupu müəyyən edir.

Ümumiyyətlə, **DACL** bir obyektə girişi nəzarət etmək üçün, **SACL** isə giriş cəhdlərini qeydə almaq və hesablamaq üçün istifadə olunur. Bu bölmədə **DACL** araşdırılacaq, lakin eyni konsepsiyalar **SACL** üçün də tətbiq olunur.

```
D:(A;;CCLCSWRPLORC;;;AU)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;SY)
```

Bir-birinə sıxılmış və açılan və bağlanan mötərizələrlə ayrılmış bu simvol qarışığı **Təhlükəsizlik Təsvirçisi Tərif Dili (Security Descriptor Definition Language - SDDL)** adlanan formatdadır.

İngilis dilinin tipik yazılma tərzi olduğu üçün soldan sağa oxumağa meyl edə bilərik, lakin kompüterlərlə qarşılıqlı əlaqədə olarkən bu, fərqli ola bilər. **Windows Update** (`wuauserv`) xidməti üçün bütün təhlükəsizlik təsvirçisini birinci hərfdən və mötərizələr dəstindən başlayaraq bu qaydada oxuyun:

  * **D:** - ardınca gələn simvollar **DACL** icazələridir
  * **AU:** - **Autentifikasiya Edilmiş İstifadəçilər** (*Authenticated Users*) təhlükəsizlik əsasını müəyyən edir
  * **A;;** - girişi **icazə verilir** (*access is allowed*)
  * **CC** - Tam adı **SERVICE\_QUERY\_CONFIG**-dir və xidmət konfiqurasiyası üçün xidmətə nəzarət menecerinə (SCM) edilən sorğudur
  * **LC** - Tam adı **SERVICE\_QUERY\_STATUS**-dur və xidmətin cari statusu üçün xidmətə nəzarət menecerinə (SCM) edilən sorğudur
  * **SW** - Tam adı **SERVICE\_ENUMERATE\_DEPENDENTS**-dir və asılı xidmətlərin siyahısını sadalayacaq
  * **RP** - Tam adı **SERVICE\_START**-dır və xidməti işə salacaq
  * **LO** - Tam adı **SERVICE\_INTERROGATE**-dir və xidmətin cari statusunu sorğulayacaq
  * **RC** - Tam adı **READ\_CONTROL**-dur və xidmətin təhlükəsizlik təsvirçisini sorğulayacaq

Təhlükəsizlik təsvirçisini oxuyarkən, simvolların xaotik görünən sırasına aldanmaq asan ola bilər, lakin xatırlayın ki, biz mahiyyətcə bir giriş nəzarət siyahısında (*access control list*) **giriş nəzarət qeydlərini** (*access control entries*) nəzərdən keçiririk. Nöqtəli vergüllər arasında olan hər bir 2 simvoldan ibarət dəst müəyyən bir istifadəçi və ya qrup tərəfindən yerinə yetirilməsinə icazə verilən hərəkətləri təmsil edir.

```
;;CCLCSWRPLORC;;;
```

Son nöqtəli vergüllər dəstindən sonra gələn simvollar həmin hərəkətləri yerinə yetirməyə icazə verilən **təhlükəsizlik əsasını** (İstifadəçi və/və ya Qrup) göstərir.

```
;;;AU
```

Açılan mötərizədən dərhal sonra və birinci nöqtəli vergüllər dəstindən əvvəl gələn simvol, hərəkətlərin **İcazə Verildiyini (A)** və ya **Qadağan Edildiyini (D)** müəyyən edir.

```
A;;
```

**Windows Update** (`wuauserv`) xidməti ilə əlaqəli bu bütün təhlükəsizlik təsvirçisi **üç fərqli təhlükəsizlik əsası** olduğu üçün üç dəst giriş nəzarət qeydinə malikdir. Hər bir təhlükəsizlik əsasına xüsusi icazələr tətbiq edilir.

## PowerShell İstifadə Edərək Xidmət İcazələrini Araşdırma

**Get-Acl** PowerShell cmdlet-indən istifadə edərək, reyestrdə müəyyən bir xidmətin yolunu hədəf alaraq xidmət icazələrini araşdıra bilərik.

```powershell
PS C:\Users\htb-student> Get-ACL -Path HKLM:\System\CurrentControlSet\Services\wuauserv | Format-List

Path   : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\wuauserv
Owner  : NT AUTHORITY\SYSTEM
Group  : NT AUTHORITY\SYSTEM
Access : BUILTIN\Users Allow  ReadKey
         BUILTIN\Users Allow  -2147483648
         BUILTIN\Administrators Allow  FullControl
         BUILTIN\Administrators Allow  268435456
         NT AUTHORITY\SYSTEM Allow  FullControl
         NT AUTHORITY\SYSTEM Allow  268435456
         CREATOR OWNER Allow  268435456
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  ReadKey
         APPLICATION PACKAGE AUTHORITY\ALL APPLICATION PACKAGES Allow  -2147483648
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         ReadKey
         S-1-15-3-1024-1065365936-1281604716-3511738428-1654721687-432734479-3232135806-4053264122-3456934681 Allow
         -2147483648
Audit  :
Sddl   : O:SYG:SYD:AI(A;ID;KR;;;BU)(A;CIIOID;GR;;;BU)(A;ID;KA;;;BA)(A;CIIOID;GA;;;BA)(A;ID;KA;;;SY)(A;CIIOID;GA;;;SY)(A
         ;CIIOID;GA;;;CO)(A;ID;KR;;;AC)(A;CIIOID;GR;;;AC)(A;ID;KR;;;S-1-15-3-1024-1065365936-1281604716-3511738428-1654
         721687-432734479-3232135806-4053264122-3456934681)(A;CIIOID;GR;;;S-1-15-3-1024-1065365936-1281604716-351173842
         8-1654721687-432734479-3232135806-4053264122-3456934681)
```

Bu əmrin xüsusi hesab icazələrini oxunması asan formatda və **SDDL**-də necə qaytardığına diqqət edin. Həmçinin, hər bir təhlükəsizlik əsasını (İstifadəçi və/və ya Qrup) təmsil edən **SID** (Security Identifier) SDDL-də mövcuddur. Bu, əmr sətrindən `sc` işlətdiyimiz zaman əldə etmədiyimiz bir şeydir.

Əmr sətrindən xidmətlərlə və onlarla əlaqəli icazələrlə necə qarşılıqlı əlaqə qurulacağını bilmək bu tapşırıqları **skriptləşdirməyi** asanlaşdırır. Bu tapşırıqları **GUI**-dən necə yerinə yetirəcəyimizi bilmək yaxşı olsa da, daha böyük şəbəkə mühitlərinə və Domenlərə keçməyə başladıqca o qədər də miqyaslana bilən deyil.






















