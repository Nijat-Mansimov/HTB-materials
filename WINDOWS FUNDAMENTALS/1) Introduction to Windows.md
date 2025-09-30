## Windows-a Giriş

Penetrasiya testçisi olaraq, geniş çeşiddə texnologiyalar haqqında bilik sahibi olmaq vacibdir. Windows və Linux əməliyyat sistemləri haqqında dərin anlayış geniş çeşiddə qiymətləndirmə növlərində faydalıdır. Qiymətləndirmələr zamanı rast gəldiyimiz sistemlərin əksəriyyəti, istər yerli (on-premise) olsun, istərsə də buludda (in the cloud), bu iki əməliyyat sisteminə əsaslanacaq. Bu əməliyyat sistemlərinə necə hücum etməyi və onları necə müdafiə etməyi, həmçinin hər birinin əlavə penetrasiya testi fəaliyyətlərini həyata keçirmək üçün necə platforma kimi istifadə oluna biləcəyini başa düşmək vacibdir.

### Windows Əməliyyat Sistemi

Microsoft ilk dəfə **Windows** əməliyyat sistemini **20 noyabr 1985-ci ildə** təqdim etdi. Windows-un ilk versiyası MS-DOS üçün qrafik əməliyyat sistemi qabığı (shell) idi. Windows Desktop-un sonrakı versiyaları Windows File Manager, Program Manager və Print Manager proqramlarını təqdim etdi.

**Windows 95**, Windows və DOS-un ilk tam inteqrasiyası idi və ilk dəfə daxili İnternet dəstəyi təklif etdi. Bu versiya həmçinin **Internet Explorer** veb-brauzerini debüt etdi. İlk versiyadan bəri, Windows XP, Vista və 8 kimi, indiki versiya olan **Windows 11**-ə qədər ondan çox Windows versiyası buraxılmışdır. Zaman keçdikcə, Microsoft hər bir Windows Desktop buraxılışının müxtəlif nəşrlərini təsadüfi istifadəçilərdən tutmuş korporativ müştərilərə qədər hər kəsə uyğunlaşdırmaq üçün təklif etmişdir.

**Windows Server** ilk dəfə 1993-cü ildə **Windows NT 3.1 Advanced Server**-in buraxılması ilə təqdim edildi. Windows NT illər ərzində İnternet Məlumat Xidmətləri (IIS), müxtəlif şəbəkə protokolları, admin tapşırıqlarını asanlaşdırmaq üçün İnzibati Ustalar (Administrative Wizards) və daha çox texnologiyalar əlavə edərək bir neçə yeniləmə gördü. **Windows 2000**-in buraxılması ilə Microsoft, əslində sistem administratorlarına fayl mübadiləsini, məlumat şifrələməsini, VPN-ləri və s. qurmağa kömək etmək üçün nəzərdə tutulmuş **Active Directory**-ni debüt etdi. Windows Server 2000 həmçinin Microsoft Management Console (MMC) daxil idi və dinamik disk həcmlərini dəstəkləyirdi.

Növbəti **Windows Server 2003** Server rolları, daxili firewall, Volume Shadow Copy Service və daha çoxunu təklif etdi. **Windows Server 2008** failover klasterləşdirmə, Hyper-V virtuallaşdırma proqramı, Server Core, Event Viewer və Active Directory-də əsas təkmilləşdirmələri əhatə edirdi. İllər keçdikcə, Microsoft daha sonrakı Server versiyalarını, o cümlədən Server 2012, Server 2016 və ən son **Server 2019**-u buraxdı. Bu ən son versiya Kubernetes, Linux konteynerləri və daha təkmil təhlükəsizlik xüsusiyyətləri üçün dəstək əlavə etdi.

Windows-un yeni versiyaları təqdim edildikcə, köhnə versiyalar dəstəkdən çıxarılır və artıq Microsoft yeniləmələrini almır (bəzi hallarda uzunmüddətli dəstək müqaviləsi alınmadığı təqdirdə). **Windows Server 2008** və **2012** təhlükəsizlik yeniləmələri üçün ömürlərinin sonuna 14 yanvar 2020-ci ildə çatdı. Hal-hazırda yalnız Server 2012 R2 və daha sonrakı versiyalar dəstəklənir. Lakin, Microsoft kritik SMBv1 zəifliyinin (EternalBlue) aşkar edilməsi səbəbindən son bir neçə ildə Windows-un əvvəlki versiyaları üçün vaxtından kənar yamalar (out-of-band patches) buraxmışdır.

Windows-un bir çox versiyası indi "köhnə" (legacy) hesab olunur və artıq dəstəklənmir. Təşkilatlar tez-tez kritik tətbiqləri dəstəkləmək və ya əməliyyat və ya büdcə problemləri səbəbindən müxtəlif köhnə əməliyyat sistemlərini işlətdiklərini görürlər. Bir qiymətləndiricinin versiyalar arasındakı fərqləri və hər birinə xas olan müxtəlif səhv konfiqurasiyaları və zəiflikləri başa düşməsi lazımdır.

### Windows Versiyaları

Aşağıda əsas Windows əməliyyat sistemlərinin və əlaqəli versiya nömrələrinin siyahısı verilmişdir:

| Əməliyyat Sistemi Adları | Versiya Nömrəsi |
| :--- | :--- |
| Windows NT 4 | 4.0 |
| Windows 2000 | 5.0 |
| Windows XP | 5.1 |
| Windows Server 2003, 2003 R2 | 5.2 |
| Windows Vista, Server 2008 | 6.0 |
| Windows 7, Server 2008 R2 | 6.1 |
| Windows 8, Server 2012 | 6.2 |
| Windows 8.1, Server 2012 R2 | 6.3 |
| Windows 10, Server 2016, Server 2019 | 10.0 |

Əməliyyat sistemi haqqında məlumat tapmaq üçün `Get-WmiObject` cmdlet-dən istifadə edə bilərik. Bu cmdlet WMI siniflərinin nümunələrini və ya mövcud WMI sinifləri haqqında məlumat almaq üçün istifadə edilə bilər. Sistemimizin versiya və yığma nömrəsini (build number) tapmaq üçün müxtəlif yollar var. Bu məlumatı **Windows 10** hostunda olduğumuzu, yığma nömrəsinin **19041** olduğunu göstərən `win32_OperatingSystem` sinifindən istifadə edərək asanlıqla əldə edə bilərik.

```powershell
PS C:\htb> Get-WmiObject -Class win32_OperatingSystem | select Version,BuildNumber

Version    BuildNumber
-------    -----------
10.0.19041 19041
```

`Get-WmiObject` ilə istifadə edilə bilən digər faydalı siniflər arasında proses siyahısını almaq üçün `Win32_Process`, xidmətlərin siyahısını almaq üçün `Win32_Service` və Əsas Giriş/Çıxış Sistemi (BIOS) məlumatını almaq üçün `Win32_Bios` var. **BIOS** kompüterin ana plitəsinə quraşdırılmış, kompüterin enerji idarəetməsi, giriş/çıxış interfeysləri və sistem konfiqurasiyası kimi əsas funksiyalarına nəzarət edən proqram təminatıdır (firmware). Uzaq kompüterlər haqqında məlumat əldə etmək üçün `ComputerName` parametrindən istifadə edə bilərik. `Get-WmiObject` yerli və uzaq kompüterlərdə xidmətləri başlamaq və dayandırmaq və daha çox şey üçün istifadə edilə bilər. Cmdlet haqqında əlavə məlumatı **burada** və **burada** tapa bilərsiniz.

-----

### Windows-a Giriş

#### Yerli Giriş Konsepsiyaları

Əgər siz bu sözləri indi oxuyursunuzsa, deməli, hansısa bir kompüterə yerli girişiniz var. İstər smartfon, planşet, noutbuk, Raspberry Pi, istərsə də Masaüstü (Desktop) olsun. Yerli giriş, Windows ilə işləyən kompüterlər də daxil olmaqla, istənilən kompüterə daxil olmaq üçün ən ümumi yoldur. **Giriş (Input)** çox güman ki, klaviatura, trackpad və/və ya siçan vasitəsilə baş verir. **Çıxış (Output)** ekran(lar)dan gəlir. İşçilərin gündəlik işlədiyi ofis sahəsi olan təşkilatlar, işçilərinin təşkilata məxsus kompüterlərdə xüsusi iş yerlərində işləməsi fikri ətrafında təhlükəsizlik siyasətləri və təhlükəsizlik nəzarətləri qururlar. Texniki olmayan və texniki işçi qüvvələri ilə uzaqdan işləməyə (remote work) dəstəklərini artırdıqlarını görmək daha adi hala gəlir. Bu, İT, Proqram Təminatının İnkişafı və İnformasiya Təhlükəsizliyi sahəsində çalışan texniki mütəxəssislər üçün yeni bir reallıq deyil. Hər hansı bir gündə, bir texniki mütəxəssis bir neçə maşına yerli və uzaqdan daxil ola bilər. Bununla da, gəlin uzaqdan giriş (remote access) konsepsiyasını müzakirə edək.

#### Uzaqdan Giriş Konsepsiyaları

**Uzaqdan Giriş** şəbəkə üzərindən bir kompüterə daxil olmaqdır. Bir kompüterə uzaqdan daxil olmaq üçün əvvəlcə ona yerli giriş lazımdır. Uzaqdan giriş üçün saysız-hesabsız metodlar var. Bu modulda biz əsasən Windows əməliyyat sistemlərinə qoşulmaq və onlarla qarşılıqlı əlaqə qurmaq üçün uzaqdan giriş metodlarından istifadə edəcəyik. Şəbəkə və İnternet texnologiyalarındakı irəliləyişlər, tamamilə kompüter sistemlərinin uzaqdan girişinə və uzaqdan idarə edilməsinə güvənən bütün sənayeləri doğurmuşdur.

**MSP-ləri (Managed Service Providers)** və **MSSP-ləri (Managed Security Service Providers)** nəzərdən keçirin, hər iki sənaye əsasən müştərilərinin kompüter sistemlərini uzaqdan idarə etməkdən asılıdır. Bu funksionallıq onlara idarəetməni mərkəzləşdirməyə, istifadə olunan texnologiyaları standartlaşdırmağa, çoxsaylı tapşırıqları avtomatlaşdırmağa, uzaqdan iş rejimini təmin etməyə və problemlər ortaya çıxdıqda və ya potensial təhlükəsizlik təhdidləri meydana çıxdıqda sürətli reaksiya müddətinə imkan verir. Uzaqdan giriş yalnız MSP və MSSP-lərlə məhdudlaşmır. İT, Proqram Təminatının İnkişafı və/və ya Təhlükəsizlik qrupları olan təşkilatlar tətbiqlər yaratmaq, serverləri idarə etmək və işçilərin iş stansiyalarını idarə etmək üçün gündəlik olaraq uzaqdan giriş metodlarından istifadə edirlər. Ən çox yayılmış uzaqdan giriş texnologiyalarından bəziləri bunlardır, lakin bunlarla məhdudlaşmır:

  * Virtual Şəxsi Şəbəkələr (**VPN**)
  * Secure Shell (**SSH**)
  * File Transfer Protocol (**FTP**)
  * Virtual Network Computing (**VNC**)
  * Windows Remote Management (və ya PowerShell Remoting) (**WinRM**)
  * Remote Desktop Protocol (**RDP**)

Bu modulda biz əsasən **RDP**-dən istifadəyə diqqət yetirəcəyik.

#### Remote Desktop Protocol (RDP)

**RDP** bir şəbəkə üzərindən RDP girişinin aktiv olduğu bir kompüterin hədəf IP ünvanını və ya hostname-ni (ana kompüterin adını) təyin etmək üçün müştəri tərəfi tətbiqinin istifadə edildiyi **müştəri/server arxitekturasından** istifadə edir. RDP uzaqdan girişinin aktiv olduğu hədəf kompüter server hesab olunur. Qeyd etmək vacibdir ki, RDP defolt olaraq **3389** məntiqi portunda dinləyir. Unutmayın ki, IP ünvanı şəbəkədəki bir kompüter üçün məntiqi identifikator, məntiqi port isə bir tətbiqə təyin olunmuş identifikator hesab olunur. Daha sadə dildə desək, bir şəbəkə altşəbəkəsini (network subnet) bir qəsəbədəki bir küçə (korporativ şəbəkə), bu altşəbəkədəki bir hosta təyin olunmuş IP ünvanını o küçədəki bir ev və məntiqi portları isə evə daxil olmaq üçün istifadə edilə bilən pəncərələr/qapılar kimi nəzərdən keçirə bilərik.

Bir sorğu (paketin daxilində qapalı) öz **IP ünvanı** vasitəsilə təyinat kompüterə çatdıqdan sonra, sorğu həmin sorğuda göstərilən porta (paketin daxilində başlıq olaraq daxil edilmişdir) əsaslanaraq kompüterdə yerləşən bir tətbiqə yönləndiriləcəkdir. IP ünvanlama və protokol qapalılaşdırma ("Introduction to Networking" modulunda daha ətraflı işıqlandırılır) mövzusuna toxunulmuşdur. Şəbəkə baxımından, bu modulda yalnız hər bir kompüterin şəbəkə üzərindən əlaqə qurmaq üçün təyin edilmiş bir IP ünvanına sahib olduğunu və hədəf kompüterlərdə yerləşən tətbiqlərin müəyyən məntiqi portlarda dinlədiyini başa düşməliyik.

**RDP**-dən Linux və ya Windows işləyən hücum hostundan Windows hədəfinə qoşulmaq üçün istifadə edə bilərik. Əgər Windows hostundan Windows hədəfinə qoşuluruqsa, biz daxili RDP müştəri tətbiqindən istifadə edə bilərik, bu, **Remote Desktop Connection** (`mstsc.exe`) adlanır. Əsas istifadəni görmək üçün aşağıdakı klipə baxın:

<img width="1024" height="600" alt="image" src="https://github.com/user-attachments/assets/23d888f0-6a19-4bed-bfb3-330b8c9c9f1e" />

Bunun işləməsi üçün, hədəf Windows sistemində uzaqdan girişə artıq **icazə verilməlidir**. Defolt olaraq, Windows əməliyyat sistemlərində uzaqdan girişə icazə verilmir. HTB Akademiya komandası Akademiya laboratoriyalarına VPN vasitəsilə qoşulduqdan sonra Windows hədəflərimizin çoxunu RDP girişinə icazə vermək üçün konfiqurasiya etmişdir.

**Remote Desktop Connection** həmçinin bizə qoşulma profillərini yadda saxlamağa imkan verir. Bu, İT adminləri arasında yayılmış bir vərdişdir, çünki uzaq sistemlərə qoşulmağı daha rahat edir.

<img width="1024" height="600" alt="image" src="https://github.com/user-attachments/assets/eec32ebb-7dd0-4426-ae7c-a6d3c1c60deb" />

Penetrasiya testçiləri olaraq, bir tapşırıq zamanı yadda saxlanılmış bu **Uzaq Masaüstü Fayllarını (.rdp)** axtarmaqdan faydalana bilərik.

Bir çox başqa Uzaq Masaüstü müştəri tətbiqləri mövcuddur, bunlardan bəziləri Microsoft-un **"Remote Desktop clients"** adlı məqaləsində sadalanır. Bu modulda hər bir Uzaq Masaüstü müştəri tətbiqini əhatə etməyəcəyik.

### xfreerdp-dən İstifadə

Linux əsaslı bir hücum hostundan Windows hədəflərinə uzaqdan daxil olmaq üçün **xfreerdp** adlı bir vasitədən istifadə edə bilərik. Onun istifadəsinin rahatlığı, xüsusiyyətlər toplusu, əmr sətiri (command line) faydalılığı və səmərəliliyi səbəbindən bir çox modullarda xfreerdp-dən istifadə etdiyimizi görəcəksiniz. Pwnbox-dan əsas istifadəni görmək üçün aşağıdakı klipə baxın:

Unutmayın ki, **xfreerdp** əmrlərini əmr sətrinə kopyalayıb yapışdıra da bilərik, buna görə də seçimləri əl ilə daxil etməyə ehtiyac yoxdur. xfreerdp ilə istifadəmiz üçün bir neçə seçim mövcuddur, məsələn, hədəf hosta faylları köçürmək üçün **sürücünün yönləndirilməsi (drive redirection)**, hansılar ki, təcrübə etməyə dəyər və biz onları HTB Akademiyası daxilindəki digər modullarda əhatə edəcəyik.

**Remmina** və **rdesktop** kimi digər RDP müştəriləri də mövcuddur və biz sizi başqalarını sınaqdan keçirməyə və sizin üçün ən yaxşı nəticə verəni görməyə təşviq edirik. İndi bu konsepsiyaları əhatə etdiyimizə görə, gəlin aşağıdakı hədəfi işə salaraq və təqdim olunan etimadnamələrdən istifadə edərək RDP ilə ona qoşularaq tətbiq edək.

### Windows Hədəfinə Qoşulma

Aşağıdakı əmrdən istifadə edərək **Uzaq Masaüstü (RDP)** vasitəsilə qoşulun:

```bash
nijatmansimov@htb[/htb]$ xfreerdp /v:<hədəfIp> /u:htb-student /p:Şifrə
```
















