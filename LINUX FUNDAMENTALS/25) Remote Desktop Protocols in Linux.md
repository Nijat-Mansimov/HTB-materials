# Linux-da Uzaq Masaüstü Protokolları

Uzaq masaüstü protokolları Windows, Linux və macOS sistemlərində sistemə **qrafik uzaqdan giriş** təmin etmək üçün istifadə olunur. Bu protokollar administratorlara sistemləri uzaqdan idarə etməyə, nasazlıqları aradan qaldırmağa və yeniləməyə imkan verir, bu da onları belə ssenarilər üçün vacib alətlərə çevirir. Bunu etmək üçün administrator idarə etdiyi əməliyyat sistemindən asılı olaraq müvafiq protokol istifadə edərək uzaq sistemə qoşulur.

Məsələn, administratorlar proqram təminatı quraşdırmaq və ya uzaq sistemi idarə etmək lazım olduqda, qrafik sessiya qurmaq üçün müvafiq protokoldan istifadə edirlər. Bu növ giriş üçün ən çox yayılmış iki protokol bunlardır:

  * **Uzaq Masaüstü Protokolu (RDP)**: Əsasən Windows mühitlərində istifadə olunur. RDP administratorlara uzaqdan qoşulmağa və Windows maşınının masaüstü ilə sanki onun qarşısında oturmuş kimi qarşılıqlı əlaqə qurmağa imkan verir.
  * **Virtual Şəbəkə Hesablama (VNC)**: Linux mühitlərində populyar bir protokoldur, lakin eyni zamanda kross-platformadır. VNC uzaq masaüstlərə qrafik giriş təmin edir, administratorlara Linux sistemlərində Windows-dakı RDP-yə bənzər şəkildə tapşırıqları yerinə yetirməyə imkan verir.

Uzaq masaüstü protokollarını müxtəlif növ binalar üçün fərqli açar dəstləri kimi düşünün. RDP, Windows binaları üçün xüsusi olaraq hazırlanmış açar kimidir, bu da otaqlara (masaüstlərə) uzaqdan, sanki içəridəymişsiniz kimi daxil olmağa və idarə etməyə imkan verir. VNC isə, bir çox binada işləyə bilən universal açara daha çox bənzəyir, lakin tez-tez Linux strukturları üçün istifadə olunur.

-----

## XServer

**XServer** (X Server) **X Window System şəbəkə protokolunun (X11 / X)** istifadəçi tərəfi hissəsidir. **X11** bizə qrafik istifadəçi interfeysində tətbiq pəncərələrini displeylərdə çağırmağa imkan verən protokollar və tətbiqlər toplusundan ibarət sabit sistemdir. X11 Unix sistemlərində üstünlük təşkil edir, lakin X serverləri digər əməliyyat sistemləri üçün də mövcuddur. Hal-hazırda, XServer Ubuntu və onun törəmələrinin demək olar ki, hər masaüstü quraşdırılmasının bir hissəsidir və ayrıca quraşdırılmasına ehtiyac yoxdur.

Linux kompüterində bir masaüstü başladıldığı zaman, qrafik istifadəçi interfeysinin əməliyyat sistemi ilə əlaqəsi bir X server vasitəsilə baş verir. Kompüter şəbəkədə olmasa belə, kompüterin daxili şəbəkəsi istifadə olunur. X protokolunun praktik tərəfi **şəbəkə şəffaflığıdır**. Bu protokol əsasən TCP/IP-ni nəqliyyat bazası kimi istifadə edir, lakin sırf Unix soketləri üzərində də istifadə edilə bilər. X server üçün istifadə edilən portlar adətən **TCP/6001-6009** diapazonunda yerləşir və müştəri ilə server arasında əlaqəyə imkan verir. X server vasitəsilə yeni bir masaüstü sessiyası başladıldığı zaman, ilk X displey **`:0`** üçün **TCP portu 6000** açılacaq.

Buna görə də, X server yerli kompüterdən asılı deyil, o, digər kompüterlərə daxil olmaq üçün istifadə edilə bilər və digər kompüterlər yerli X serverdən istifadə edə bilər. Həm yerli, həm də uzaq kompüterlərdə Unix/Linux sistemlərinin olması şərtilə, VNC və RDP kimi əlavə protokollar **lazımsızdır (superfluous)**. VNC və RDP qrafik çıxışı uzaq kompüterdə yaradır və şəbəkə üzərindən ötürür. Halbuki, **X11 ilə qrafik çıxış yerli kompüterdə render edilir.** Bu, trafikə və uzaq kompüter üzərindəki yükə qənaət edir.

Bununla belə, X11-in əhəmiyyətli çatışmazlığı **şifrələnməmiş məlumat ötürülməsidir**. Lakin, bu, **SSH protokolunu tunelə salmaqla** aradan qaldırıla bilər.

Bunun üçün biz tətbiqi təmin edən serverdəki SSH konfiqurasiya faylında (`/etc/ssh/sshd_config`) bu parametri **`yes`** olaraq dəyişməklə **X11 yönləndirməsinə (forwarding)** icazə verməliyik.

```bash
# X11 Yönləndirilməsi parametrinə baxılır
nijatmansimov@htb[/htb]$ cat /etc/ssh/sshd_config | grep X11Forwarding
X11Forwarding yes
```

Bununla biz müştərimizdən tətbiqi aşağıdakı əmrlə başlata bilərik:

```bash
# SSH tuneli üzərindən uzaq tətbiqi (Firefox) başlatmaq
nijatmansimov@htb[/htb]$ ssh -X htb-student@10.129.23.11 /usr/bin/firefox
htb-student@10.129.14.130's password: ********
<SKIP>
```
<img width="918" height="450" alt="image" src="https://github.com/user-attachments/assets/1c03c6a3-bb41-40de-b867-c91d7fd5054b" />

## X11 Təhlükəsizliyi

Əvvəllər qeyd edildiyi kimi, X11 **defolt olaraq təhlükəsiz protokol deyil**, çünki onun rabitəsi **şifrələnməmişdir**. Buna görə də, Linux əsaslı hədəflərlə işləyərkən **TCP portları 6000-6010** diapazonuna diqqət yetirməliyik. Müvafiq təhlükəsizlik tədbirləri olmadan, açıq bir **X server** həssas məlumatları şəbəkə üzərindən ifşa edə bilər. Məsələn, eyni şəbəkədəki bir hücumçu ənənəvi şəbəkə sniffinqinə ehtiyac duymadan, istifadəçinin xəbəri olmadan **X serverin pəncərələrinin məzmununu oxuya** bilər. Bu zəiflik ciddi təhlükəsizlik pozuntularına səbəb olur. Hücumçu potensial olaraq `xwd` (X pəncərələrinin ekran görüntülərini çəkən) və `xgrabsc` kimi standart X11 alətlərindən istifadə edərək şifrələr və ya şəxsi məlumatlar kimi həssas məlumatları ələ keçirə bilər.

Bundan əlavə, illər ərzində XServer kitabxanaları və proqram təminatının özü ilə əlaqəli digər təhlükəsizlik zəiflikləri aşkar edilmişdir. Məsələn, 2017-ci ildə X Window System-in açıq mənbə tətbiqi olan **XOrg Serverdə bir sıra zəifliklər** aşkar edilmişdir. Zəif, proqnozlaşdırıla bilən və ya **brute-force edilə bilən sessiya açarlarından** irəli gələn bu zəifliklərin istismarı, hücumçunun başqa bir istifadəçinin Xorg sessiyasında **özbaşına kod icra etməsinə** imkan verə bilərdi. Unix, Red Hat Enterprise Linux, Ubuntu Linux və SUSE Linux kimi geniş çeşidli sistemlər təsirlənmişdi. Bu zəifliklər **CVE-2017-2624, CVE-2017-2625 və CVE-2017-2626** kimi tanınır.

## XDMCP

**X Display Manager Control Protocol (XDMCP)** Unix/Linux altında işləyən X terminalları və kompüterlər arasında **UDP port 177** vasitəsilə rabitə üçün **X Display Manager** tərəfindən istifadə olunur. O, digər maşınlarda uzaq X Window sessiyalarını idarə etmək üçün istifadə olunur və Linux sistem administratorları tərəfindən uzaq masaüstlərə giriş təmin etmək üçün tez-tez istifadə edilir. XDMCP **təhlükəsiz olmayan bir protokoldur** və yüksək səviyyəli təhlükəsizlik tələb edən heç bir mühitdə istifadə edilməməlidir. Bununla bütün qrafik istifadəçi interfeysini (GUI) (məsələn, KDE və ya Gnome) müvafiq müştəriyə yönləndirmək mümkündür. Linux sisteminin XDMCP serveri kimi çıxış etməsi üçün serverdə GUI ilə bir X sistemi quraşdırılmalı və konfiqurasiya edilməlidir.

XDMCP-nin istismar edilməsinin potensial yollarından biri **man-in-the-middle (ortadakı adam) hücumu** vasitəsilədir. Bu hücum növündə, hücumçu uzaq kompüter ilə X Window System serveri arasındakı rabitəni ələ keçirir və serverə icazəsiz giriş əldə etmək üçün tərəflərdən birini təqlid edir.

## VNC

**Virtual Network Computing (VNC)** istifadəçilərə kompüteri uzaqdan idarə etməyə imkan verən **RFB protokolu** əsasında qurulmuş uzaq masaüstü paylaşma sistemidir. O, istifadəçiyə şəbəkə bağlantısı üzərindən uzaqdan bir masaüstü mühitini görməyə və onunla qarşılıqlı əlaqə qurmağa imkan verir. İstifadəçi uzaq kompüteri sanki qarşısında oturmuş kimi idarə edə bilər. Bu, həmçinin Linux hostları üçün uzaq qrafik bağlantılar üçün ən ümumi protokollardan biridir.

VNC ümumiyyətlə təhlükəsiz hesab olunur. O, ötürülmə zamanı məlumatların təhlükəsiz olmasını təmin etmək üçün **şifrələmədən** istifadə edir və istifadəçinin giriş əldə etməzdən əvvəl **identifikasiya (authentication)** tələb edir.

VNC serverləri üçün iki fərqli konsepsiya mövcuddur:

1.  **Adi server:** İstifadəçi dəstəyi üçün host kompüterin **faktiki ekranını** təklif edir. Uzaq kompüterdə klaviatura və siçan istifadə edilə bildiyi üçün razılaşma tövsiyə olunur.
2.  **İkinci qrup server proqramları:** Terminal server konsepsiyasına bənzər şəkildə **virtual sessiyalara** istifadəçi girişinə imkan verir.

Ənənəvi olaraq, VNC serveri **TCP port 5900** üzərində dinləyir. Beləliklə, o, **`display 0`**-ı orada təklif edir. Digər displeylər əlavə portlar, əsasən **`590[x]`** vasitəsilə təklif edilə bilər, burada `x` displey nömrəsidir. Məsələn, əlavə bağlantılar **5901, 5902, 5903** və s. kimi daha yüksək TCP portuna təyin ediləcək.

Bu cür VNC bağlantıları üçün **TigerVNC, TightVNC, RealVNC, UltraVNC** kimi bir çox fərqli alətlər istifadə olunur. UltraVNC və RealVNC şifrələmələri və yüksək təhlükəsizliklərinə görə ən çox istifadə edilən alətlərdir.

### TigerVNC Quraşdırılması və Konfiqurasiyası

Nümunədə **TigerVNC** serveri qurulur, bunun üçün digər şeylərlə yanaşı, GNOME ilə VNC bağlantıları bir qədər qeyri-sabit olduğundan **XFCE4** masaüstü meneceri də tələb olunur.

1.  **Zəruri paketləri quraşdırmaq:**

    ```bash
    htb-student@ubuntu:~$ sudo apt install xfce4 xfce4-goodies tigervnc-standalone-server -y
    ```

2.  **VNC bağlantısı üçün şifrə yaratmaq:**

    ```bash
    htb-student@ubuntu:~$ vncpasswd
    Password: ******
    Verify: ******
    Would you like to enter a view-only password (y/n)? n
    ```

3.  **Konfiqurasiya fayllarını yaratmaq (`xstartup` və `config`):**

    ```bash
    htb-student@ubuntu:~$ touch ~/.vnc/xstartup ~/.vnc/config

    # xstartup faylının məzmunu (VNC sessiyasının necə yaradılacağını müəyyən edir)
    htb-student@ubuntu:~$ cat <<EOT >> ~/.vnc/xstartup
    #!/bin/bash
    unset SESSION_MANAGER
    unset DBUS_SESSION_BUS_ADDRESS
    /usr/bin/startxfce4
    [ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
    [ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
    x-window-manager &
    EOT

    # config faylının məzmunu (sessiya parametrlərini müəyyən edir)
    htb-student@ubuntu:~$ cat <<EOT >> ~/.vnc/config

    geometry=1920x1080
    dpi=96
    EOT

    # xstartup faylına icra hüququ vermək
    htb-student@ubuntu:~$ chmod +x ~/.vnc/xstartup
    ```

4.  **VNC serveri başlatmaq:**

    ```bash
    htb-student@ubuntu:~$ vncserver
    New 'linux:1 (htb-student)' desktop at :1 on machine linux
    # ... Çıxış ...
    ```

5.  **Aktiv sessiyaları siyahılamaq:**

    ```bash
    htb-student@ubuntu:~$ vncserver -list
    TigerVNC server sessions:
    X DISPLAY #     RFB PORT #      PROCESS ID
    :1              5901            79746
    ```

### SSH Tuneli Quraşdırmaq və Qoşulmaq

Bağlantını şifrələmək və daha təhlükəsiz etmək üçün **SSH tuneli** yaradılır.

1.  **SSH tunelini qurmaq** (Yerli port 5901-i uzaq hostun yerli portu 5901-ə yönləndirir):

    ```bash
    nijatmansimov@htb[/htb]$ ssh -L 5901:127.0.0.1:5901 -N -f -l htb-student 10.129.14.130
    htb-student@10.129.14.130's password: *******
    ```

2.  **SSH tuneli vasitəsilə VNC serverə qoşulmaq:**

    ```bash
    nijatmansimov@htb[/htb]$ xtightvncviewer localhost:5901
    # ... Çıxış ...
    Password: ******
    Authentication successful
    # ... Çıxış ...
    ```

Bu üsulla bağlantı şifrələnmiş olur, çünki VNC trafiki yerli maşından uzaq serverə **SSH tuneli** vasitəsilə ötürülür.

<img width="1235" height="649" alt="image" src="https://github.com/user-attachments/assets/a2c18ead-210b-48bc-a17d-6c16df7ea942" />











