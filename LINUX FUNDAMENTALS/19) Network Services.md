# Şəbəkə Xidmətləri (Network Services)

Linux ilə işləyərkən, müxtəlif **şəbəkə xidmətlərini** idarə etmək çox vacibdir. Bu xidmətləri idarə etməkdə bacarıqlı olmaq bir neçə səbəbə görə həlledicidir. Şəbəkə xidmətləri, bir çoxu **uzaqdan əməliyyatlara** imkan verən xüsusi tapşırıqları yerinə yetirmək üçün nəzərdə tutulub. Şəbəkə üzərindən digər kompüterlərlə əlaqə qurmaq, əlaqələr yaratmaq, faylları köçürmək, şəbəkə trafikini təhlil etmək və bu xidmətləri effektiv şəkildə konfiqurasiya etmək üçün bilik və bacarıqlara sahib olmaq vacibdir. Bu təcrübə, **penetrasiya testi** zamanı potensial zəiflikləri müəyyənləşdirməyə imkan verir. Əlavə olaraq, hər bir xidmətin konfiqurasiya seçimlərini başa düşmək, şəbəkə təhlükəsizliyinə dair ümumi anlayışımızı artırır.

Bir **penetrasiya testi** apardığımız və zəifliklər üçün qiymətləndirilən bir Linux hostu ilə qarşılaşdığımız ssenarini nəzərdən keçirin. Şəbəkə trafikini izləyərək, bu Linux hostundakı bir istifadəçinin **şifrələnməmiş FTP serveri** vasitəsilə başqa bir serverə qoşulduğunu müşahidə edirik. Nəticədə, istifadəçinin **etimadnamələrini (credentials) açıq mətndə** ələ keçirə bilirik. Əgər istifadəçi FTP-nin əlaqələri və ya ötürülən məlumatları şifrələmədiyini bilsəydi, bu vəziyyət çox az ehtimal edilərdi. Bir Linux administratoru üçün bu, **kritik bir nəzarətsizlikdir**, çünki o, yalnız şəbəkədəki təhlükəsizlik zəifliklərini üzə çıxarmır, həm də şəbəkənin təhlükəsizliyinin qorunmasına cavabdeh olan administratorlara pis təsir göstərir.

Hər bir şəbəkə xidmətini əhatə etmək mümkün olmasa da, **ən vacib olanlara** diqqət yetirəcəyik. Bu yanaşma, yalnız administratorlar və istifadəçilər üçün deyil, həm də müxtəlif hostlar və öz sistemləri arasındakı qarşılıqlı əlaqəni başa düşməli olan penetrasiya testçiləri üçün faydalıdır.

-----

## SSH (Secure Shell)

**Secure Shell (SSH)** şəbəkə üzərindən məlumatların və əmrlərin **təhlükəsiz ötürülməsinə** imkan verən bir şəbəkə protokoludur. O, uzaq sistemləri təhlükəsiz idarə etmək və əmrləri icra etmək və ya faylları köçürmək üçün uzaq sistemlərə təhlükəsiz daxil olmaq üçün geniş istifadə olunur. SSH vasitəsilə öz və ya uzaq bir Linux hostuna qoşulmaq üçün müvafiq SSH serveri mövcud olmalı və işləməlidir.

Ən çox istifadə olunan SSH serveri **OpenSSH serveridir**. OpenSSH, şəbəkə üzərindən məlumatların və əmrlərin təhlükəsiz ötürülməsinə imkan verən Secure Shell (SSH) protokolunun pulsuz və açıq mənbəli tətbiqidir.

Administratorlar, uzaq bir hosta **şifrələnmiş əlaqə** quraraq uzaq sistemləri təhlükəsiz idarə etmək üçün OpenSSH-dən istifadə edirlər. OpenSSH ilə administratorlar uzaq sistemlərdə əmrləri icra edə, faylları təhlükəsiz köçürə və məlumatların və əmrlərin ötürülməsinin üçüncü tərəflər tərəfindən ələ keçirilməsi ehtimalı olmadan təhlükəsiz uzaq əlaqə qura bilərlər.

### OpenSSH Quraşdırmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install openssh-server -y
```

### Serverin Vəziyyəti

Serverin işlədiyini yoxlamaq üçün aşağıdakı əmri istifadə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/system/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2023-02-12 21:15:27 GMT; 1min 22s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 7740 (sshd)
      Tasks: 1 (limit: 9458)
     Memory: 2.5M
        CPU: 236ms
     CGroup: /system.slice/ssh.service
             └─7740 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```

### SSH - Daxil Olmaq

Penetrasiya testçiləri olaraq, bir şəbəkə auditi apararkən uzaq sistemlərə təhlükəsiz daxil olmaq üçün OpenSSH-dən istifadə edirik. Bunu etmək üçün aşağıdakı əmri istifadə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ssh cry0l1t3@10.129.17.122
The authenticity of host '10.129.17.122 (10.129.17.122)' can't be established.
ECDSA key fingerprint is SHA256:bKzhv+n2pYqr2r...Egf8LfqaHNxk.

Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '10.129.17.122' (ECDSA) to the list of known hosts.

cry0l1t3@10.129.17.122's password: ***********
```

OpenSSH, **/etc/ssh/sshd\_config** faylını bir mətn redaktoru ilə redaktə etməklə konfiqurasiya edilə və fərdiləşdirilə bilər. Burada eyni vaxtda əlaqələrin maksimum sayı, daxil olmaq üçün parolların və ya açarların istifadəsi, host açarının yoxlanılması və s. kimi ayarları tənzimləyə bilərik. Lakin, OpenSSH konfiqurasiya faylında dəyişikliklərin diqqətlə edilməsi bizim üçün vacibdir.

Məsələn, biz SSH-dən uzaq bir sistemə təhlükəsiz daxil olmaq və əmrləri icra etmək və ya üçüncü tərəflərin məlumatların və əmrlərin ötürülməsini ələ keçirməsi ehtimalı olmadan şəbəkə ayarlarını və digər sistem ayarlarını yoxlamaq üçün şifrələnmiş bir əlaqə üzərindən məlumatları tunelləmək üçün **tunelləmə və port yönləndirmə** (tunneling and port forwarding) istifadə edə bilərik.

-----

## NFS (Network File System)

**Network File System (NFS)** bizə faylları yerli sistemdə saxlanıldığı kimi **uzaq sistemlərdə saxlamağa və idarə etməyə** imkan verən bir şəbəkə protokoludur. Bu, şəbəkələr arasında faylların asan və səmərəli idarə edilməsini təmin edir. Məsələn, administratorlar məlumatların asan əməkdaşlığı və idarə edilməsini təmin etmək üçün faylları mərkəzi şəkildə saxlamaq və idarə etmək üçün (Linux və Windows sistemləri üçün) NFS-dən istifadə edirlər. Linux üçün NFS serverləri bir neçədir, o cümlədən **NFS-UTILS** (Ubuntu), **NFS-Ganesha** (Solaris) və **OpenNFS** (Redhat Linux).

O, həmçinin resursları səmərəli şəkildə paylaşmaq və idarə etmək üçün istifadə edilə bilər, məsələn, serverlər arasında fayl sistemlərini təkrarlamaq. O, həmçinin **giriş nəzarətləri**, **real vaxt rejimində fayl köçürmə** və **birdən çox istifadəçinin məlumatlara eyni vaxtda daxil olmasına dəstək** kimi xüsusiyyətlər təklif edir. Hədəf sistemdə FTP müştərisi quraşdırılmadığı və ya FTP əvəzinə NFS işlədiyi halda, bu xidmətdən FTP kimi istifadə edə bilərik.

### NFS Quraşdırmaq

Linux-da NFS-i aşağıdakı əmrlə quraşdıra bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install nfs-kernel-server -y
```

### Serverin Vəziyyəti

Serverin işlədiyini yoxlamaq üçün aşağıdakı əmri istifadə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ systemctl status nfs-kernel-server
● nfs-server.service - NFS server and services
     Loaded: loaded (/lib/system/system/nfs-server.service; enabled; vendor preset: enabled)
     Active: active (exited) since Sun 2023-02-12 21:35:17 GMT; 13s ago
    Process: 9234 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
    Process: 9235 ExecStart=/usr/sbin/rpc.nfsd $RPCNFSDARGS (code=exited, status=0/SUCCESS)
   Main PID: 9235 (code=exited, status=0/SUCCESS)
        CPU: 10ms
```

NFS-i **/etc/exports** konfiqurasiya faylı vasitəsilə konfiqurasiya edə bilərik. Bu fayl hansı qovluqların paylaşılmalı olduğunu və istifadəçilər və sistemlər üçün **giriş hüquqlarını** göstərir. Həmçinin, köçürmə sürəti və şifrələmənin istifadəsi kimi ayarları konfiqurasiya etmək mümkündür. NFS giriş hüquqları hansı istifadəçilərin və sistemlərin paylaşılan qovluqlara daxil ola biləcəyini və hansı hərəkətləri yerinə yetirə biləcəyini müəyyənləşdirir. NFS-də konfiqurasiya edilə bilən bəzi vacib giriş hüquqları aşağıdakılardır:

| İcazələr | Təsvir |
| :--- | :--- |
| **`rw`** | İstifadəçilərə və sistemlərə paylaşılan qovluğa **oxuma və yazma icazəsi** verir. |
| **`ro`** | İstifadəçilərə və sistemlərə paylaşılan qovluğa **yalnız oxuma icazəsi** verir. |
| **`no_root_squash`** | Müştəridəki **root istifadəçisinin** adi istifadəçinin hüquqları ilə məhdudlaşdırılmasının qarşısını alır. |
| **`root_squash`** | Müştəridəki root istifadəçisinin hüquqlarını **adi istifadəçinin hüquqları** ilə məhdudlaşdırır. |
| **`sync`** | Dəyişikliklərin yalnız fayl sistemində saxlanıldıqdan sonra köçürülməsini təmin etmək üçün məlumatların köçürülməsini sinxronlaşdırır. |
| **`async`** | Məlumatları asinxron şəkildə köçürür, bu da köçürməni daha sürətli edir, lakin dəyişikliklər tamamilə həyata keçirilməyibsə, fayl sistemində **uyğunsuzluqlara səbəb ola bilər**. |

### NFS Paylaşımı Yaratmaq

Məsələn, yeni bir qovluq yarada və onu NFS-də müvəqqəti olaraq paylaşa bilərik. Bunu aşağıdakı kimi edərdik:

Müntəzəm İfadələr

```bash
cry0l1t3@htb:~$ mkdir nfs_sharing
cry0l1t3@htb:~$ echo '/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
cry0l1t3@htb:~$ cat /etc/exports | grep -v "#"
/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)
```

### NFS Paylaşımını Quraşdırmaq (Mount)

Əgər bir NFS paylaşımı yaratmışıqsa və hədəf sistemdə onunla işləmək istəyiriksə, əvvəlcə onu **quraşdırmalıyıq (mount)**. Bunu aşağıdakı əmrlə edə bilərik:

Müntəzəm İfadələr

```bash
cry0l1t3@htb:~$ mkdir ~/target_nfs
cry0l1t3@htb:~$ mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
cry0l1t3@htb:~$ tree ~/target_nfs
target_nfs/
├── css.css
├── html.html
├── javascript.js
├── php.php
└── xml.xml

0 directories, 5 files
```

Beləliklə, hədəfimizdən (**10.129.12.17**) olan NFS paylaşımını (**dev\_scripts**) yerli olaraq sistemimizdəki **`target_nfs`** quraşdırma nöqtəsinə şəbəkə üzərindən quraşdırdıq və sanki hədəf sistemdəymişik kimi məzmununa baxa bilərik. Hətta xüsusi hallarda uzaq sistemdə imtiyazlarımızı artırmaq (escalate our privileges) üçün NFS istifadə edilə bilən bəzi metodlar mövcuddur.

-----

## Veb Server (Web Server)

Veb serverlərin işləməsini başa düşmək, penetrasiya testçiləri üçün vacibdir, çünki bu serverlər veb tətbiqlərinin ayrılmaz hissəsidir və tez-tez təhlükəsizlik qiymətləndirmələri zamanı **əsas hədəflər** kimi xidmət edir. **Veb server** İnternet üzərindən məlumatları, sənədləri, tətbiqləri və müxtəlif funksiyaları təqdim edən proqram təminatıdır. O, veb brauzerləri kimi müştərilərə məlumatları ötürmək və bu müştərilərdən sorğuları qəbul etmək üçün **Hypertext Transfer Protocol (HTTP)** istifadə edir. Qəbul edilmiş məlumat daha sonra müştərinin brauzerində **Hypertext Markup Language (HTML)** olaraq göstərilir, bu da istifadəçi sorğularına interaktiv cavab verən dinamik veb səhifələrin yaradılmasını asanlaşdırır. Nəticə etibarilə, veb server funksionallıqlarının hərtərəfli anlaşılması, təhlükəsiz və səmərəli veb tətbiqləri inkişaf etdirmək və ümumi sistem təhlükəsizliyini qorumaq üçün həyati əhəmiyyət daşıyır. Linux platformalarında ən çox istifadə olunan veb serverlər arasında **Apache**, **Nginx**, **Lighttpd** və **Caddy** var, xüsusilə də Apache Ubuntu, Solaris və Red Hat Linux daxil olmaqla əməliyyat sistemləri ilə geniş uyğunluğuna görə populyardır.

Penetrasiya testçiləri üçün veb serverlər müxtəlif köməkçi proqramlar təklif edir. Onlar **fayl köçürmələrini** asanlaşdırmaq üçün istifadə edilə bilər, testçilərə HTTP və ya HTTPS portları vasitəsilə daxil olmağa və hədəf sistemlərlə qarşılıqlı əlaqə qurmağa imkan verir. Əlavə olaraq, veb serverlər **fişinq hücumlarını** həyata keçirmək üçün hədəf səhifələrin replikalarını yerləşdirməklə, bununla da istifadəçi etimadnamələrini ələ keçirməyə cəhd edərək istifadə edilə bilər. Bu tətbiqlərdən başqa, veb serverlər bir şəbəkədə zəiflikləri test etmək və istismar etmək üçün çoxsaylı digər imkanlar təmin edir.

Apache veb serveri, təhlükəsiz və səmərəli bir veb tətbiqini yerləşdirməyə imkan verən müxtəlif xüsusiyyətlərə malikdir. Bundan əlavə, hücumları təhlil etməyimizə kömək edən serverimizdəki trafik haqqında məlumat almaq üçün **jurnallamayı (logging) konfiqurasiya edə** bilərik. Apache-ni aşağıdakı əmrlə quraşdıra bilərik:

### Apache Veb Serveri Quraşdırmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install apache2 -y
```

### Apache Konfiqurasiyası

Apache2 üçün, hansı qovluqlara daxil oluna biləcəyini göstərmək üçün **`/etc/apache2/apache2.conf`** faylını bir mətn redaktoru ilə redaktə edə bilərik. Bu fayl qlobal ayarları ehtiva edir. Hansı qovluqlara daxil oluna biləcəyini və bu qovluqlarda hansı hərəkətlərin yerinə yetirilə biləcəyini göstərmək üçün ayarları dəyişdirə bilərik.

```txt
<Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</directory>
```

Bu bölmə, defolt **`/var/www/html`** qovluğunun əlçatan olduğunu, istifadəçilərin **`Indexes`** və **`FollowSymLinks`** seçimlərini istifadə edə biləcəyini, bu qovluqdakı fayllarda dəyişikliklərin **`AllowOverride All`** ilə ləğv edilə biləcəyini və **`Require all granted`**-in bütün istifadəçilərə bu qovluğa giriş verdiyini göstərir. Məsələn, bir veb server istifadə edərək faylları hədəf sistemlərimizdən birinə köçürmək istəyiriksə, müvafiq faylları **`/var/www/html`** qovluğuna qoya və bu faylları hədəf sistemdə yükləmək üçün **`wget`** və ya **`curl`** və ya digər tətbiqləri istifadə edə bilərik.

Həmçinin, söhbət gedən qovluqda yarada biləcəyimiz **`.htaccess`** faylını istifadə edərək qovluq səviyyəsində fərdi ayarları fərdiləşdirmək mümkündür. Bu fayl, Apache konfiqurasiya faylını fərdiləşdirmədən **giriş nəzarətləri** kimi müəyyən qovluq səviyyəsindəki ayarları konfiqurasiya etməyə imkan verir. Veb tətbiqimizin təhlükəsizliyini yaxşılaşdırmağımıza kömək edən **`mod_rewrite`**, **`mod_security`** və **`mod_ssl`** kimi xüsusiyyətlər əldə etmək üçün modullar da əlavə edə bilərik.

### Python Veb Serveri

**Python Veb Serveri** Apache-yə sadə, sürətli bir alternativdir və faylları başqa bir sistemə köçürmək üçün tək bir əmrlə tək bir qovluğu yerləşdirmək üçün istifadə edilə bilər. Python Veb Serverini quraşdırmaq üçün sistemimizdə **Python3** quraşdırmalı və sonra aşağıdakı əmri işə salmalıyıq:

### Python və Veb Serveri Quraşdırmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install python3 -y
nijatmansimov@htb[/htb]$ python3 -m http.server
```

Bu əmri işlətdikdə, Python Veb Serverimiz **TCP/8000** portunda başladılacaq və hazırda olduğumuz qovluğa daxil ola bilərik. Həmçinin, aşağıdakı əmrlə başqa bir qovluğu yerləşdirə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ python3 -m http.server --directory /home/cry0l1t3/target_files
```

Bu, **TCP/8000** portunda bir Python veb serverini işə salacaq və biz, məsələn, brauzerdən **`/home/cry0l1t3/target_files`** qovluğuna daxil ola bilərik. Python veb serverimizə daxil olduğumuz zaman, brauzerimizdə keçidi yazaraq və faylları yükləyərək faylları digər sistemə köçürə bilərik. Python veb serverimizi defolt portdan başqa bir portda da yerləşdirə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ python3 -m http.server 443
```

Bu, Python veb serverimizi defolt **TCP/8000** portu əvəzinə **443** portunda yerləşdirəcək. Bu veb serverə brauzerimizdə keçidi yazaraq daxil ola bilərik.

-----

## VPN (Virtual Private Network)

**Virtual Private Network (VPN)** sanki fiziki olaraq onun içindəymişik kimi, başqa bir şəbəkəyə **təhlükəsiz, görünməz bir tunel** kimi işləyir və maneəsiz və qorunan girişi təmin edir. Buna müştəri və server arasında **şifrələnmiş bir tunel** qurmaqla nail olunur, bu da bu əlaqə vasitəsilə ötürülən bütün məlumatların məxfi qalmasını və icazəsiz girişdən qorunmasını təmin edir.

Təşkilatlar əsasən işçilərinə yerində olmağı tələb etmədən daxili şəbəkəyə **təhlükəsiz giriş** vermək üçün VPN-lərdən istifadə edirlər. Bu çeviklik, işçilərin hər hansı bir yerdən daxili resurslara və tətbiqlərə çatmasına imkan verir, məhsuldarlığı və mobilliyi artırır. Əlavə olaraq, VPN-lər internet trafikini **anonimləşdirməyə** və xarici müdaxilələri bloklamağa xidmət edir, təhlükəsizliyi daha da gücləndirir.

Linux serverləri üçün ən çox istifadə olunan VPN həlləri arasında **OpenVPN**, **L2TP/IPsec**, **PPTP**, **SSTP** və **SoftEther** var. **OpenVPN** Ubuntu, Solaris və Red Hat Linux daxil olmaqla müxtəlif əməliyyat sistemləri ilə uyğun gələn populyar bir açıq mənbə seçimi kimi fərqlənir. Administratorlar OpenVPN-dən korporativ şəbəkələrə təhlükəsiz uzaqdan girişi asanlaşdırmaq, şəbəkə trafikini şifrələmək və istifadəçi anonimliyini qorumaq üçün istifadə edirlər.

Penetrasiya testçiləri üçün OpenVPN **əvəzsiz imkanlar** təklif edir. O, testçilərə, xüsusən coğrafi məhdudiyyətlər səbəbindən birbaşa giriş mümkün olmadıqda, daxili şəbəkələrə **təhlükəsiz qoşulmağa** imkan verir. OpenVPN istifadə edərək, penetrasiya testçiləri daxili sistemlərin hərtərəfli təhlükəsizlik qiymətləndirmələrini apara, potensial zəiflikləri müəyyənləşdirə və aradan qaldıra bilərlər. OpenVPN-in şifrələmə, tunelləmə, trafik formalaşdırma, şəbəkə marşrutlaşdırma və dinamik şəbəkə mühitlərinə uyğunlaşma kimi xüsusiyyətləri ilə **çox yönlülüyü**, onu həm şəbəkə administratorlarının, həm də təhlükəsizlik mütəxəssislərinin arsenalında əvəzolunmaz bir alət edir. Serveri və müştərini aşağıdakı əmrlə quraşdıra bilərik:

### OpenVPN Quraşdırmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install openvpn -y
```

OpenVPN, **/etc/openvpn/server.conf** konfiqurasiya faylını redaktə etməklə fərdiləşdirilə və konfiqurasiya edilə bilər. Bu fayl OpenVPN serveri üçün ayarları ehtiva edir. Şifrələmə, tunelləmə, trafik formalaşdırma və s. kimi müəyyən xüsusiyyətləri konfiqurasiya etmək üçün ayarları dəyişdirə bilərik.

### VPN-ə Qoşulmaq

Əgər bir OpenVPN serverinə qoşulmaq istəyiriksə, serverdən aldığımız və sistemimizdə saxladığımız **`.ovpn`** faylını istifadə edə bilərik. Bunu əmr sətrində aşağıdakı əmrlə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo openvpn --config internal.ovpn
```

Əlaqə qurulduqdan sonra, daxili şəbəkədəki daxili hostlarla əlaqə qura bilərik.
