# Şəbəkə Konfiqurasiyası

Penetrasiya testçisi olaraq, **Linux sistemlərində şəbəkə parametrlərini konfiqurasiya etmək və idarə etmək** əsas bacarıqlardan biridir. Bu bacarığa yiyələnmək bizə test mühitlərini səmərəli qurmağa, şəbəkə trafikini manipulyasiya etməyə və zəiflikləri müəyyən etməyə və ya istismar etməyə imkan verir. Linux şəbəkə konfiqurasiyası haqqında möhkəm bilik, test yanaşmamızı xüsusi ehtiyaclara uyğunlaşdırmağa, həm test prosedurlarımızı, həm də nəticələrimizi optimallaşdırmağa kömək edir.

Şəbəkə konfiqurasiyasında əsas vəzifələrdən biri **şəbəkə interfeyslərini idarə etməkdir**. Bu, **IP ünvanlarını** təyin etməyi, routerlər və sviçlər kimi şəbəkə cihazlarını konfiqurasiya etməyi və müxtəlif **şəbəkə protokollarını** qurmağı əhatə edir. **TCP/IP** (İnternet rabitəsinin əsas protokol dəsti), **DNS** (domen adlarının həlli), **DHCP** (dinamik IP ünvanı bölgüsü) və **FTP** (fayl ötürülməsi) daxil olmaqla, şəbəkə protokolları haqqında dərin bilik kritik əhəmiyyətə malikdir. Biz həmçinin fərqli şəbəkə interfeys növləri (naqilli və ya simsiz) ilə tanış olmalı və qoşulma problemlərini aradan qaldıra bilməliyik.

-----

## Şəbəkə Girişə Nəzarət (NAC)

Şəbəkə konfiqurasiyasının başqa bir vacib komponenti **Şəbəkə Girişə Nəzarətdir (NAC)**. Penetrasiya testçiləri olaraq, biz NAC-ın şəbəkə təhlükəsizliyini necə gücləndirə biləcəyini və mövcud olan müxtəlif texnologiyaları yaxşı bilməliyik. Əsas NAC modellərinə daxildir:

| Növ | Təsvir |
| :--- | :--- |
| **İxtiyari Girişə Nəzarət (DAC)** | Bu model, resursun sahibinə ona kimin daxil ola biləcəyinə dair icazələri təyin etməyə imkan verir. |
| **Məcburi Girişə Nəzarət (MAC)** | İcazələr resursun sahibi tərəfindən deyil, əməliyyat sistemi tərəfindən tətbiq edilir, bu da onu daha təhlükəsiz, lakin daha az çevik edir. |
| **Rol Əsaslı Girişə Nəzarət (RBAC)** | İcazələr təşkilat daxilindəki **rollara əsasən** təyin edilir, bu da istifadəçi imtiyazlarını idarə etməyi asanlaşdırır. |

Linux şəbəkə cihazlarının NAC üçün konfiqurasiyası, **SELinux** (Təhlükəsizliyi Gücləndirilmiş Linux), tətbiq təhlükəsizliyi üçün **AppArmor** profilləri və IP ünvanlarına əsasən xidmətlərə girişi idarə etmək üçün **TCP wrappers** kimi təhlükəsizlik siyasətlərinin qurulmasını əhatə edir.

**`syslog`**, **`rsyslog`**, **`ss`** (soket statistikası üçün), **`lsof`** (açıq faylları siyahılamaq üçün) və **ELK stack** (Elasticsearch, Logstash və Kibana) kimi alətlər şəbəkə trafikini izləmək və təhlil etmək üçün istifadə edilə bilər. Bu alətlər anormallıqları, potensial məlumat sızmalarını, təhlükəsizlik pozuntularını və digər kritik şəbəkə problemlərini müəyyən etməyə kömək edir.

-----

## Şəbəkə İnterfeyslərinin Konfiqurasiyası

Ubuntu ilə işləyərkən, yerli şəbəkə interfeyslərini **`ifconfig`** və ya **`ip`** əmrlərini istifadə edərək konfiqurasiya edə bilərsiniz.

### Şəbəkə Parametrlərinə Baxmaq

Şəbəkə interfeysləri haqqında IP ünvanları, netmasklar və status kimi məlumatları əldə etmək üçün **`ifconfig`** və ya daha müasir olan **`ip addr`** əmrini istifadə edə bilərik.

```bash
# ifconfig istifadə edərək şəbəkə parametrləri (Köhnəlmişdir, lakin hələ də geniş istifadə olunur)
cry0l1t3@htb:~$ ifconfig
# ... Çıxış ...
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 178.62.32.126  netmask 255.255.192.0  broadcast 178.62.63.255
# ... Çıxış ...

# ip addr istifadə edərək şəbəkə parametrləri (Daha müasir yanaşma)
cry0l1t3@htb:~$ ip addr
# ... Çıxış ...
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 178.62.32.126/18 brd 178.62.63.255 scope global dynamic eth0
# ... Çıxış ...
```

### İnterfeysi Aktivləşdirmək və Konfiqurasiya Etmək

| Vəzifə | `ifconfig` Əmri | `ip` Əmri |
| :--- | :--- | :--- |
| **Aktivləşdirmək** | `sudo ifconfig eth0 up` | `sudo ip link set eth0 up` |
| **IP təyin etmək** | `sudo ifconfig eth0 192.168.1.2` | `sudo ip addr add 192.168.1.2/24 dev eth0` |
| **Netmask təyin etmək** | `sudo ifconfig eth0 netmask 255.255.255.0` | IP təyinatı ilə birləşdirilir (`/24`) |
| **Defolt şlyuz təyin etmək** | `sudo route add default gw 192.168.1.1 eth0` | `sudo ip route add default via 192.168.1.1` |

### DNS Parametrlərinin Qalıcı Edilməsi

DNS serverlərini təyin etmək üçün **`/etc/resolv.conf`** faylı yenilənir. Lakin, dəyişikliklərin sistem yenidən başladıqdan sonra qalıcı olması üçün **`/etc/network/interfaces`** faylını redaktə etmək lazımdır.

```bash
# /etc/resolv.conf faylını redaktə etmək (Qalıcı deyil!)
nijatmansimov@htb[/htb]$ sudo vim /etc/resolv.conf
# Faylın içində:
nameserver 8.8.8.8
nameserver 8.8.4.4

# /etc/network/interfaces faylını statik IP ilə redaktə etmək (Qalıcı dəyişikliklər üçün)
nijatmansimov@htb[/htb]$ sudo vim /etc/network/interfaces
# Faylın içində:
auto eth0
iface eth0 inet static
  address 192.168.1.2
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8 8.8.4.4
```

Dəyişiklikləri tətbiq etmək üçün şəbəkə xidmətini yenidən başlatın:

```bash
nijatmansimov@htb[/htb]$ sudo systemctl restart networking
```

-----

## Təhlükəsizlik Mexanizmləri və Sərtləşdirmə

Linux sistemlərini təhlükəsizləşdirmək üçün istifadə edilən əsas mexanizmlər **Məcburi Girişə Nəzarət (MAC)** sistemləri və **Host əsaslı Giriş Nəzarəti** alətləridir.

| Mexanizm | Növ | Əsas Xüsusiyyət |
| :--- | :--- | :--- |
| **SELinux** (Security-Enhanced Linux) | MAC | Nüvəyə dərin inteqrasiya olunmuş, resurslara və tətbiqlərə **çox dəqiq (granular) nəzarət** təmin edir. Konfiqurasiyası mürəkkəbdir. |
| **AppArmor** | MAC | Kernel Modulu kimi işləyir, **profil əsaslı təhlükəsizlikdən** istifadə edir. SELinux-dan daha sadə və idarə edilməsi asandır. |
| **TCP Wrappers** | Host-based NAC | Daxil olan qoşulmaların **IP ünvanına əsaslanaraq** şəbəkə xidmətlərinə girişi məhdudlaşdırır. Əsas şəbəkə səviyyəsində qorunma üçün idealdır. |

-----

## Nasazlıqların Aradan Qaldırılması (Troubleshooting) və Monitorinq

Penetrasiya testçisi üçün şəbəkə nasazlıqlarını müəyyənləşdirmək və aradan qaldırmaq, həmçinin trafikə nəzarət etmək vacibdir.

### Əsas Troubleshooting Alətləri 🛠️

| Əmr | Təsvir | Nümunə |
| :--- | :--- | :--- |
| **`ping`** | İki cihaz arasında **qoşulma qabiliyyətini** (connectivity) və gecikməni yoxlayır (ICMP paketləri). | `ping 8.8.8.8` |
| **`traceroute`** | Paketlərin uzaq hosta çatması üçün keçdiyi **marşrutu (yolu)** göstərir. | `traceroute www.inlanefreight.com` |
| **`netstat`** | Aktiv şəbəkə qoşulmalarını, dinləyən portları və onların vəziyyətini (`LISTEN`, `ESTABLISHED`) göstərir. | `netstat -a` |
| **`ss`** | Daha müasir və sürətli **soket statistikası** alətidir. | `ss -tuln` |
| **`Tcpdump`**, **`Wireshark`** | Şəbəkə trafikini **tutmaq və təhlil etmək** üçün istifadə olunur (qeyri-şifrələnmiş əlaqələrdə etibarnamələri tutmaq üçün faydalıdır). | |
| **`Nmap`** | **Şəbəkə aşkarlanması** və **port skaneri** kimi istifadə olunur (vulnerable girişləri müəyyən etmək). | |

### Ümumi Şəbəkə Problemləri

Penetrasiya testləri zamanı rast gəlinən ən ümumi şəbəkə problemləri bunlardır:

1.  **Şəbəkə qoşulma problemləri** (Əsasən firewall, router və ya səhv kabelləşmə səbəbindən).
2.  **DNS həlli problemləri** (Ən yaygın səbəblərdən biri: səhv DNS server parametrləri).
3.  **Məlumat paketlərinin itirilməsi** (Şəbəkə tıxacları və ya avadanlıq nasazlıqları səbəbindən).
4.  **Şəbəkə performans problemləri** (Köhnəlmiş avadanlıq və ya səhv konfiqurasiya).

Bu problemlərin səbəblərini anlamaq, testlər zamanı şəbəkə sistemlərindəki zəiflikləri effektiv şəkildə müəyyən etmək və istismar etmək üçün kritikdir.
