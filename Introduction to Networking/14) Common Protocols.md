## Ən Çox Yayılmış Protokollar

İnternet protokolları, şəbəkədəki cihazların bir-biri ilə necə əlaqə qurmalı olduğunu müəyyən edən, **RFC**-lərdə təyin olunmuş standartlaşdırılmış qaydalar və təlimatlardır. Onlar istifadə olunan avadanlıq və proqram təminatından asılı olmayaraq, şəbəkədəki cihazların məlumatları ardıcıl və etibarlı şəkildə mübadilə edə bilməsini təmin edir. Cihazların şəbəkədə əlaqə qurması üçün onlar **rabitə kanalı** vasitəsilə (məsələn, simli və ya simsiz əlaqə) birləşdirilməlidir. Cihazlar daha sonra ötürülən məlumatların formatını və strukturunu müəyyən edən standartlaşdırılmış protokollar toplusundan istifadə edərək məlumat mübadiləsi aparırlar. Şəbəkələrdə istifadə olunan əlaqənin iki əsas növü **Transmission Control Protocol (TCP)** və **User Datagram Protocol (UDP)**-dir.

Biz fərqli və ən çox istifadə olunan protokollarla işləməli və onları bilməliyik. Əvvəldən öyrəndiyimiz kimi, bu protokollar şəbəkələrdəki cihazlarımız və kompüterlərimiz arasındakı bütün əlaqənin əsasını təşkil edir. Aşağıda modullar boyu işləyəcəyimiz bu protokollardan bir çoxunu tərtib etmişik. Onları nə qədər yaxşı başa düşsək, onlarla bir o qədər səmərəli işləyə bilərik.

---

## Transmission Control Protocol (TCP)

**TCP** bir **əlaqə yönümlü** (*connection-oriented*) protokol olub, məlumat ötürməzdən əvvəl **Üç-Yollu Əl Sıxma** (*Three-Way-Handshake*) istifadə edərək iki cihaz arasında **virtual əlaqə** qurur. Bu əlaqə məlumat ötürülməsi tamamlanana qədər saxlanılır və əlaqə aktiv olduğu müddətcə cihazlar məlumatları irəli-geri göndərməyə davam edə bilər.

Məsələn, veb-brauzerimizə bir URL daxil etdiyimiz zaman, brauzer **TCP**-dən istifadə edərək veb saytı yerləşdirən serverə bir **HTTP sorğusu** göndərir. Server, **TCP**-dən istifadə edərək veb saytın HTML kodunu brauzerə geri göndərir. Daha sonra brauzer bu koddan istifadə edərək veb saytı ekranımızda göstərir. Bu proses brauzerlə veb-server arasında **TCP** əlaqəsinin qurulmasına və məlumat ötürülməsi tamamlanana qədər saxlanmasına əsaslanır. Nəticədə, **TCP etibarlıdır**, lakin əlaqənin qurulması və saxlanması üçün **əlavə xərc** (*overhead*) tələb etdiyi üçün **UDP**-dən **daha yavaşdır**.

| Protokol | Abreviatura | Port | Təsvir |
| :--- | :--- | :--- | :--- |
| **Telnet** | Telnet | $23$ | Uzaqdan giriş xidməti. |
| **Secure Shell** | **SSH** | $22$ | Təhlükəsiz uzaqdan giriş xidməti. |
| **Simple Network Management Protocol** | **SNMP** | $161-162$ | Şəbəkə cihazlarını idarə edir. |
| **Hyper Text Transfer Protocol** | **HTTP** | $80$ | Veb səhifələri ötürmək üçün istifadə olunur. |
| **Hyper Text Transfer Protocol Secure** | **HTTPS** | $443$ | Təhlükəsiz veb səhifələri ötürmək üçün istifadə olunur. |
| **Domain Name System** | **DNS** | $53$ | Domen adlarını axtarır. |
| **File Transfer Protocol** | **FTP** | $20-21$ | Faylları ötürmək üçün istifadə olunur. |
| **Trivial File Transfer Protocol** | **TFTP** | $69$ | Faylları ötürmək üçün istifadə olunur. |
| **Network Time Protocol** | **NTP** | $123$ | Kompüter saatlarını sinxronlaşdırır. |
| **Simple Mail Transfer Protocol** | **SMTP** | $25$ | E-poçt ötürülməsi üçün istifadə olunur. |
| **Post Office Protocol** | **POP3** | $110$ | E-poçtları əldə etmək üçün istifadə olunur. |
| **Internet Message Access Protocol** | **IMAP** | $143$ | E-poçtlara daxil olmaq üçün istifadə olunur. |
| **Server Message Block** | **SMB** | $445$ | Faylları ötürmək üçün istifadə olunur. |
| **Network File System** | **NFS** | $111, 2049$ | Uzaq sistemləri bağlamaq (*mount*) üçün istifadə olunur. |
| **Bootstrap Protocol** | **BOOTP** | $67, 68$ | Kompüterləri *bootstraplamaq* üçün istifadə olunur. |
| **Kerberos** | Kerberos | $88$ | Autentifikasiya və avtorizasiya üçün istifadə olunur. |
| **Lightweight Directory Access Protocol** | **LDAP** | $389$ | Kataloq xidmətləri üçün istifadə olunur. |
| **Remote Authentication Dial-In User Service** | **RADIUS** | $1812, 1813$ | Autentifikasiya və avtorizasiya üçün istifadə olunur. |
| **Dynamic Host Configuration Protocol** | **DHCP** | $67, 68$ | IP ünvanlarını konfiqurasiya etmək üçün istifadə olunur. |
| **Remote Desktop Protocol** | **RDP** | $3389$ | Uzaq masaüstü girişi üçün istifadə olunur. |
| **Network News Transfer Protocol** | **NNTP** | $119$ | Xəbər qruplarına daxil olmaq üçün istifadə olunur. |
| **Remote Procedure Call** | **RPC** | $135, 137-139$ | Uzaq prosedurları çağırmaq üçün istifadə olunur. |
| **Identification Protocol** | Ident | $113$ | İstifadəçi proseslərini müəyyən etmək üçün istifadə olunur. |
| **Internet Control Message Protocol** | **ICMP** | $0-255$ | Şəbəkə problemlərini aradan qaldırmaq üçün istifadə olunur. |
| **Internet Group Management Protocol** | **IGMP** | $0-255$ | Multicast üçün istifadə olunur. |
| **Oracle DB (Default/Alternative) Listener** | oracle-tns | $1521/1526$ | Oracle verilənlər bazası standart/alternativ dinləyicisi, verilənlər bazası hostunda işləyən və Oracle klientlərindən gələn sorğuları qəbul edən bir xidmətdir. |
| **Ingres Locking** | ingreslock | $1524$ | Ingres verilənlər bazası adətən böyük kommersiya tətbiqləri üçün və **RPC** vasitəsilə əmrləri uzaqdan yerinə yetirə bilən bir *backdoor* kimi istifadə olunur. |
| **Squid Web Proxy** | http-proxy | $3128$ | Squid veb proksisi, təkrarlanan sorğuları keşləməklə veb-serveri sürətləndirmək üçün istifadə olunan bir keşləmə və yönləndirmə **HTTP** veb proksisidir. |
| **Secure Copy Protocol** | **SCP** | $22$ | Sistemlər arasında faylları təhlükəsiz şəkildə kopyalamaq. |
| **Session Initiation Protocol** | **SIP** | $5060$ | **VoIP** sessiyaları üçün istifadə olunur. |
| **Simple Object Access Protocol** | **SOAP** | $80, 443$ | Veb xidmətləri üçün istifadə olunur. |
| **Secure Socket Layer** | **SSL** | $443$ | Faylları təhlükəsiz şəkildə ötürmək. |
| **TCP Wrappers** | **TCPW** | $113$ | Girişə nəzarət üçün istifadə olunur. |
| **Internet Security Association and Key Management Protocol** | **ISAKMP** | $500$ | **VPN** əlaqələri üçün istifadə olunur. |
| **Microsoft SQL Server** | ms-sql-s | $1433$ | Microsoft SQL Server-ə klient qoşulmaları üçün istifadə olunur. |
| **Kerberized Internet Negotiation of Keys** | **KINK** | $892$ | Autentifikasiya və avtorizasiya üçün istifadə olunur. |
| **Open Shortest Path First** | **OSPF** | $89$ | Marşrutlaşdırma üçün istifadə olunur. |
| **Point-to-Point Tunneling Protocol** | **PPTP** | $1723$ | **VPN**-lər yaratmaq üçün istifadə olunur. |
| **Remote Execution** | **REXEC** | $512$ | Bu protokol uzaq kompüterlərdə əmrləri yerinə yetirmək və əmrlərin çıxışını yerli kompüterə geri göndərmək üçün istifadə olunur. |
| **Remote Login** | **RLOGIN** | $513$ | Bu protokol uzaq kompüterdə interaktiv *shell* sessiyası başladır. |
| **X Window System** | **X11** | $6000$ | Şəbəkəyə qoşulmuş kompüterlər üçün *qrafik istifadəçi interfeysi* (**GUI**) təmin edən kompüter proqram təminatı sistemi və şəbəkə protokoludur. |
| **Relational Database Management System** | **DB2** | $50000$ | **RDBMS** maliyyə sistemləri, müştəri əlaqələrinin idarə edilməsi (**CRM**) sistemləri kimi müəssisə tətbiqləri üçün məlumatları strukturlaşdırılmış formatda saxlamaq, əldə etmək və idarə etmək üçün nəzərdə tutulmuşdur. |

---

## User Datagram Protocol (UDP)

Digər tərəfdən, **UDP** **əlaqəsiz** (*connectionless*) bir protokoldur, yəni məlumat ötürməzdən əvvəl virtual əlaqə qurmur. Bunun əvəzinə, məlumat paketlərinin qəbul edilib-edilmədiyini yoxlamadan məlumat paketlərini təyinat yerinə göndərir.

Məsələn, YouTube kimi bir platformada bir video yayımladığımız və ya izlədiyimiz zaman, video məlumatları cihazımıza **UDP** istifadə edilərək ötürülür. Bunun səbəbi, videonun bəzi məlumat itkilərinə dözə bilməsi və ötürmə sürətinin etibarlılıqdan daha vacib olmasıdır. Əgər yolda bir neçə video məlumat paketi itirsə, videonun ümumi keyfiyyətinə əhəmiyyətli dərəcədə təsir etməyəcəkdir. Bu, **UDP**-ni **TCP**-dən **daha sürətli**, lakin paketlərin təyinat yerinə çatacağına dair **zəmanət olmadığı üçün daha az etibarlı** edir.

| Protokol | Abreviatura | Port | Təsvir |
| :--- | :--- | :--- | :--- |
| **Domain Name System** | **DNS** | $53$ | Domen adlarını IP ünvanlarına çevirən protokoldur. |
| **Trivial File Transfer Protocol** | **TFTP** | $69$ | Sistemlər arasında faylları ötürmək üçün istifadə olunur. |
| **Network Time Protocol** | **NTP** | $123$ | Şəbəkədəki kompüter saatlarını sinxronlaşdırır. |
| **Simple Network Management Protocol** | **SNMP** | $161$ | Şəbəkə cihazlarını uzaqdan monitorinq edir və idarə edir. |
| **Routing Information Protocol** | **RIP** | $520$ | Routerlər arasında marşrutlaşdırma məlumatlarını mübadilə etmək üçün istifadə olunur. |
| **Internet Key Exchange** | **IKE** | $500$ | İnternet Açar Mübadiləsi. |
| **Bootstrap Protocol** | **BOOTP** | $68$ | Şəbəkədə hostları *bootstraplamaq* üçün istifadə olunur. |
| **Dynamic Host Configuration Protocol** | **DHCP** | $67$ | Şəbəkədəki cihazlara IP ünvanlarını dinamik olaraq təyin etmək üçün istifadə olunur. |
| **Telnet** | TELNET | $23$ | Mətn əsaslı uzaqdan giriş rabitə protokolu. |
| **MySQL** | MySQL | $3306$ | Açıq mənbəli verilənlər bazası idarəetmə sistemi. |
| **Terminal Server** | **TS** | $3389$ | Defolt olaraq Microsoft Windows Terminal Xidmətləri üçün istifadə edilən uzaqdan giriş protokolu. |
| **NetBIOS Name** | netbios-ns | $137$ | Windows əməliyyat sistemlərində **LAN**-da NetBIOS adlarını IP ünvanlarına çevirmək üçün istifadə olunur. |
| **Microsoft SQL Server** | ms-sql-m | $1434$ | Microsoft SQL Server Browser xidməti üçün istifadə olunur. |
| **Universal Plug and Play** | **UPnP** | $1900$ | Cihazların şəbəkədə bir-birini kəşf etməsi və əlaqə qurması üçün bir protokoldur. |
| **PostgreSQL** | **PGSQL** | $5432$ | Obyekt-əlaqəli verilənlər bazası idarəetmə sistemi. |
| **Virtual Network Computing** | **VNC** | $5900$ | Qrafik masaüstü paylaşım sistemi. |
| **X Window System** | **X11** | $6000-6063$ | Unix bənzəri sistemlərdə **GUI** təmin edən kompüter proqram təminatı sistemi və şəbəkə protokoludur. |
| **Syslog** | SYSLOG | $514$ | Kompüter sistemində loq mesajlarını toplamaq və saxlamaq üçün standart protokoldur. |
| **Internet Relay Chat** | **IRC** | $194$ | Real-time İnternet mətn mesajlaşması (*chat*) və ya sinxron rabitə protokoludur. |
| **OpenPGP** | OpenPGP | $11371$ | Məlumatların və rabitənin şifrələnməsi və imzalanması üçün bir protokoldur. |
| **Internet Protocol Security** | **IPsec** | $500$ | Təhlükəsiz, şifrələnmiş rabitəni təmin edən bir protokoldur. VPN-lərdə təhlükəsiz tunel yaratmaq üçün istifadə olunur. |
| **Internet Key Exchange** | **IKE** | $11371$ | Məlumatların və rabitənin şifrələnməsi və imzalanması üçün bir protokoldur. |
| **X Display Manager Control Protocol** | **XDMCP** | $177$ | İstifadəçinin **X11** işləyən bir kompüterə uzaqdan daxil olmasına imkan verən şəbəkə protokoludur. |

---

## Internet Control Message Protocol (ICMP)

**Internet Control Message Protocol (ICMP)** internetdə cihazlar tərəfindən **səhv hesabatı** və **status məlumatları** daxil olmaqla müxtəlif məqsədlər üçün bir-biri ilə əlaqə qurmaq üçün istifadə edilən bir protokoldur. O, səhvləri bildirmək və ya status məlumatlarını təqdim etmək üçün istifadə edilə bilən cihazlar arasında sorğular və mesajlar göndərir.

### ICMP Sorğuları

**Sorğu** bir cihaz tərəfindən məlumat tələb etmək və ya müəyyən bir hərəkəti yerinə yetirmək üçün başqa bir cihaza göndərilən bir mesajdır. ICMP-də bir sorğu nümunəsi, iki cihaz arasında **əlaqəni sınayan** **ping** sorğusudur. Bir cihaz başqasına bir **ping sorğusu** göndərdikdə, ikinci cihaz bir **ping cavabı** mesajı ilə cavab verir.

### ICMP Mesajları

ICMP-də bir **mesaj** ya bir sorğu, ya da bir cavab ola bilər. Ping sorğuları və cavablarına əlavə olaraq, ICMP digər mesaj növlərini, məsələn, **səhv mesajları**, **təyinat yerinə çatıla bilməyən** (*destination unreachable*) və **vaxt aşıldı** (*time exceeded*) mesajlarını dəstəkləyir. Bu mesajlar şəbəkədəki cihazlar arasında müxtəlif növ məlumatları və səhvləri bildirmək üçün istifadə olunur.

Məsələn, bir cihaz başqa bir cihaza paket göndərməyə çalışırsa və paket çatdırıla bilmirsə, cihaz **ICMP**-dən istifadə edərək göndəriciyə bir səhv mesajı geri göndərə bilər. **ICMP**-nin iki fərqli versiyası var:

* **ICMPv4:** Yalnız IPv4 üçün
* **ICMPv6:** Yalnız IPv6 üçün

**ICMPv4** IPv4 ilə istifadə üçün hazırlanmış **ICMP**-nin orijinal versiyasıdır. Hələ də geniş istifadə olunur və ICMP-nin ən yaygın versiyasıdır. Digər tərəfdən, **ICMPv6** IPv6 üçün hazırlanmışdır. O, əlavə funksionallığı ehtiva edir və **ICMPv4**-ün bəzi məhdudiyyətlərini aradan qaldırmaq üçün nəzərdə tutulmuşdur.

| Sorğu Növü | Təsvir |
| :--- | :--- |
| **Echo Request** | Bu mesaj bir cihazın şəbəkədə əlçatan olub-olmadığını yoxlayır. Bir cihaz bir *echo* sorğusu göndərdikdə, bir *echo* cavab mesajı almağı gözləyir. Məsələn, **tracert** (Windows) və ya **traceroute** (Linux) alətləri həmişə ICMP *echo* sorğuları göndərir. |
| **Timestamp Request** | Bu mesaj uzaq bir cihazdakı vaxtı müəyyən edir. |
| **Address Mask Request** | Bu mesaj bir cihazın altşəbəkə maskasını tələb etmək üçün istifadə olunur. |

| Mesaj Növü | Təsvir |
| :--- | :--- |
| **Echo reply** | Bu mesaj bir *echo* sorğusu mesajına cavab olaraq göndərilir. |
| **Destination unreachable** | Bu mesaj bir cihaz paketi təyinat yerinə çatdıra bilmədikdə göndərilir. |
| **Redirect** | Bir router bu mesajı bir cihaza paketlərini fərqli bir routerə göndərməli olduğunu bildirmək üçün göndərir. |
| **Time exceeded** | Bu mesaj bir paketin təyinat yerinə çatması üçün çox uzun çəkdiyi zaman göndərilir. |
| **Parameter problem** | Bu mesaj paketin başlığında problem olduğu zaman göndərilir. |
| **Source quench** | Bu mesaj bir cihaz paketləri çox tez qəbul etdikdə və ayaqlaşa bilmədikdə göndərilir. Paketlərin axın sürətini azaltmaq üçün istifadə olunur. |

**ICMP**-nin bizim üçün başqa bir vacib hissəsi, ICMP paketinin başlığındakı **Time-To-Live (TTL)** sahəsidir ki, bu da paketin şəbəkədə hərəkət etdiyi müddəti məhdudlaşdırır. O, marşrutlaşdırma dövrələri (*routing loops*) halında paketlərin şəbəkədə qeyri-müəyyən müddətə dövr etməsinin qarşısını alır. Bir paket bir routerdən keçdiyi hər dəfə, router **TTL** dəyərini $1$ vahid azaldır. **TTL** dəyəri $0$-a çatdıqda, router paketi atır və göndəriciyə bir **ICMP Time Exceeded** mesajı göndərir.

Biz həmçinin **TTL**-dən bir paketin keçdiyi *hop* sayını və təyinat yerinə **təxmini məsafəni** müəyyən etmək üçün istifadə edə bilərik. Məsələn, bir paketin **TTL**-i $10$ olarsa və təyinat yerinə çatmaq üçün $5$ *hop* çəkirsə, təyinat yerinin təxminən $5$ *hop* uzaqda olduğu nəticəsinə gəlmək olar. Məsələn, **TTL** dəyəri $122$ olan bir **ping** görürüksə, bu, $6$ *hop* uzaqda olan bir **Windows** sistemi ($TTL\ 128$ defolt olaraq) ilə qarşılaşdığımız anlamına gələ bilər.

Lakin, cihaz tərəfindən istifadə olunan **defolt TTL dəyərinə** əsaslanaraq **əməliyyat sistemini təxmin etmək** də mümkündür. Hər bir əməliyyat sistemi adətən paketlər göndərərkən bir defolt **TTL** dəyərinə malikdir. Bu dəyər paketin başlığında təyin edilir və paket bir routerdən keçdiyi hər dəfə $1$ vahid azaldılır. Buna görə də, bir cihazın defolt **TTL** dəyərini araşdıraraq, cihazın hansı əməliyyat sistemini istifadə etdiyini təxmin etmək mümkündür. Məsələn: **Windows** sistemləri ($2000/XP/2003/Vista/10$) adətən defolt **TTL** dəyəri $128$-ə malikdir, halbuki **macOS** və **Linux** sistemləri adətən defolt **TTL** dəyəri $64$-ə və **Solaris**-in defolt **TTL** dəyəri $255$-ə malikdir. Lakin, qeyd etmək vacibdir ki, istifadəçi bu dəyərləri dəyişə bilər, buna görə də onlar bir cihazın əməliyyat sistemini müəyyən etmək üçün qəti bir yoldan asılı olmamalıdır.

---

## Voice over Internet Protocol (VoIP)

**Voice over Internet Protocol (VoIP)** səs və multimedia rabitələrini ötürmə metodudur. Məsələn, o, Skype, Whatsapp, Google Hangouts, Slack, Zoom və digərləri kimi ənənəvi telefon xətti əvəzinə **genişzolaqlı internet bağlantısı** istifadə edərək telefon zəngləri etməyə imkan verir.

Ən çox yayılmış **VoIP** portları **Session Initiation Protocol (SIP)** üçün istifadə olunan **TCP/5060** və **TCP/5061**-dir. Lakin, **TCP/1720** portu da paket əsaslı şəbəkələr üzərindən multimedia rabitəsi üçün bir sıra standart olan **H.323 protokolu** üçün bəzi **VoIP** sistemləri tərəfindən istifadə edilə bilər. Yenə də, **SIP VoIP** sistemlərində **H.323**-dən daha geniş istifadə olunur.

Bununla belə, **SIP** internetdə iki və ya daha çox son nöqtə arasında video, səs, mesajlaşma və digər rabitə tətbiqləri və xidmətlərini əhatə edən **real-time sessiyalarını** başlatmaq, saxlamaq, dəyişdirmək və ləğv etmək üçün bir **siqnal protokoludur**. Buna görə də, o, son nöqtələr arasında **sorğular və metodlar** istifadə edir. Ən çox yayılmış SIP sorğuları və metodları aşağıdakılardır:

| Metod | Təsvir |
| :--- | :--- |
| **INVITE** | Bir sessiyanı başladır və ya başqa bir son nöqtəni iştiraka dəvət edir. |
| **ACK** | Bir **INVITE** sorğusunun qəbul edildiyini təsdiqləyir. |
| **BYE** | Bir sessiyanı ləğv edir. |
| **CANCEL** | Gözləyən bir **INVITE** sorğusunu ləğv edir. |
| **REGISTER** | Bir **SIP** istifadəçi agentini (**UA**) bir **SIP** serveri ilə qeydiyyatdan keçirir. |
| **OPTIONS** | Bir **SIP** serverinin və ya istifadəçi agentinin imkanları haqqında məlumat tələb edir (məsələn, dəstəklədiyi media növləri). |

### Məlumatın Aşkarlanması (*Information Disclosure*)

Lakin, **SIP** potensial hücumlar üçün **mövcud istifadəçiləri aşkar etməyə** imkan verir. Bu, istifadəçinin əlçatanlığını müəyyən etmək, istifadəçinin imkanları və ya xidmətləri haqqında məlumat tapmaq və ya daha sonra istifadəçi hesablarına qarşı *kobud qüvvə* (*brute-force*) hücumları həyata keçirmək kimi müxtəlif məqsədlər üçün edilə bilər.

İstifadəçiləri aşkar etməyin mümkün yollarından biri **SIP OPTIONS** sorğusudur. Bu, bir **SIP** serverinin və ya istifadəçi agentlərinin imkanları haqqında məlumat (məsələn, dəstəklədiyi media növləri, deşifrə edə biləcəyi kodeklər və digər təfərrüatlar) tələb etmək üçün istifadə edilən bir metoddur. **OPTIONS** sorğusu bir **SIP** serveri və ya istifadəçi agenti üçün məlumat yoxlaya və ya onun əlaqəsini və mövcudluğunu sınaqdan keçirə bilər.

Analizimiz zamanı, **SEPxxxx.cnf** faylını (burada *xxxx* unikal identifikatoru) aşkar etmək mümkündür ki, bu da əvvəllər **Cisco CallManager** kimi tanınan **Cisco Unified Communications Manager** tərəfindən bir **Cisco Unified IP Telefonu** üçün parametrləri və ayarları müəyyən etmək üçün istifadə olunan bir konfiqurasiya faylıdır. Fayl telefon modelini, *firmware* versiyasını, şəbəkə parametrlərini və digər təfərrüatları göstərir.
