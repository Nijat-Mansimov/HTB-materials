# Xidmət və Proses İdarəetməsi (Service and Process Management)

**Xidmətlər**, həmçinin **daemonlar** (daemons) kimi tanınır, Linux sisteminin **birbaşa istifadəçi qarşılıqlı təsiri olmadan** arxa planda səssizcə işləyən əsas komponentləridir. Onlar sistemi işlək vəziyyətdə saxlamaq və əlavə funksiyalar təmin etmək üçün kritik tapşırıqları yerinə yetirirlər. Ümumiyyətlə, xidmətlər iki növə bölünə bilər:

1.  **Sistem Xidmətləri (System Services):** Bunlar sistemin işə salınması zamanı tələb olunan daxili xidmətlərdir. Onlar əməliyyat sisteminin düzgün işləməsi üçün zəruri olan əsas avadanlıqla əlaqəli tapşırıqları yerinə yetirir və sistem komponentlərini başladırlar. Bunlar bir avtomobilin mühərrik və transmissiya sistemləri kimidir. Siz açarı çevirdiyiniz zaman işə düşürlər və avtomobilin hərəkət etməsi üçün vacibdirlər. Onlarsız avtomobil hərəkət etməzdi.

2.  **İstifadəçi Tərəfindən Quraşdırılmış Xidmətlər (User-Installed Services):** Bu xidmətlər istifadəçilər tərəfindən əlavə edilir və adətən server tətbiqlərini və xüsusi xüsusiyyətlər və ya imkanlar təmin edən digər arxa plan proseslərini əhatə edir. Bu növ xidmətlər avtomobilin kondisioneri və ya GPS naviqasiya sistemi kimidir. Avtomobilin işləməsi üçün kritik olmasalar da, funksionallığı artırır və sürücünün seçimlərinə əsasən əlavə xüsusiyyətlər təmin edirlər.

Daemonlar tez-tez proqram adlarının sonunda **`d`** hərfi ilə müəyyən edilir, məsələn, **`sshd`** (SSH daimonu) və ya **`systemd`**. Bir avtomobilin tam təcrübə təmin etmək üçün həm əsas komponentlərinə, həm də isteğe bağlı xüsusiyyətlərinə güvəndiyi kimi, bir Linux sistemi də səmərəli işləmək və istifadəçi ehtiyaclarını ödəmək üçün həm sistem, həm də istifadəçi tərəfindən quraşdırılmış xidmətlərdən istifadə edir.

Ümumiyyətlə, bir xidmət və ya proseslə məşğul olduğumuz zaman yalnız bir neçə məqsədimiz var:

  * Bir xidməti/prosesi **başlatmaq/yenidən başlatmaq**
  * Bir xidməti/prosesi **dayandırmaq**
  * Bir xidmətlə/proseslə nəyin baş verdiyini **görmək**
  * Sistemin işə düşməsi zamanı bir xidməti/prosesi **aktivləşdirmək/ləğv etmək**
  * Bir xidməti/prosesi **tapmaq**

Əksər müasir Linux distribütorları öz ilkinləşdirmə sistemi (init system) kimi **`systemd`**-ni qəbul etmişdir. Bu, işə salınma prosesi zamanı başlayan ilk prosesdir və **Proses ID-si (PID)** təyin edilir. Bir Linux sistemindəki bütün proseslərə **PID** təyin edilir və hər bir proses haqqında məlumatı ehtiva edən **/proc/** qovluğunda görünə bilər. Proseslərin həmçinin, onların başqa bir proses (valideyn) tərəfindən başladıldığını göstərən **Valideyn Proses ID-si (PPID)** ola bilər, bu da onları uşaq prosesləri edir.

-----

## Systemctl

VM-imizdə **OpenSSH** quraşdırdıqdan sonra, xidməti aşağıdakı əmrlə başlada bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ systemctl start ssh
```

Xidməti başlattıqdan sonra, onun səhvsiz işlədiyini yoxlaya bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-05-14 15:08:23 CEST; 24h ago
   Main PID: 846 (sshd)
   Tasks: 1 (limit: 4681)
   CGroup: /system.slice/ssh.service
           └─846 /usr/sbin/sshd -D

Mai 14 15:08:22 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 14 15:08:23 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:23 inlane sshd[846]: Server listening on :: port 22.
Mai 14 15:08:23 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 14 15:08:30 inlane systemd[1]: Reloading OpenBSD Secure Shell server.
Mai 14 15:08:31 inlane sshd[846]: Received SIGHUP; restarting.
Mai 14 15:08:31 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:31 inlane sshd[846]: Server listening on :: port 22.
```

Sistemin işə salınmasından sonra bu xidməti işə salmasını söyləmək üçün OpenSSH-i **SysV skriptinə əlavə etmək** üçün onu aşağıdakı əmrlə bağlaya bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ systemctl enable ssh
Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
```

Sistemi yenidən başlatdıqdan sonra, OpenSSH serveri avtomatik olaraq işləyəcək. Bunu **`ps`** adlı bir alətlə yoxlaya bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ps -aux | grep ssh
root       846  0.0  0.1  72300  5660 ?        Ss   Mai14   0:00 /usr/sbin/sshd -D
```

Həmçinin bütün xidmətləri siyahılaşdırmaq üçün **`systemctl`** istifadə edə bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ systemctl list-units --type=service
UNIT                                                       LOAD   ACTIVE SUB     DESCRIPTION              
accounts-daemon.service                                    loaded active running Accounts Service         
acpid.service                                              loaded active running ACPI event daemon        
apache2.service                                            loaded active running The Apache HTTP Server   
apparmor.service                                           loaded active exited  AppArmor initialization  
apport.service                                             loaded active exited  LSB: automatic crash repor
avahi-daemon.service                                       loaded active running Avahi mDNS/DNS-SD Stack  
bolt.service                                               loaded active running Thunderbolt system service
```

Xidmətlərin bir səhv səbəbindən başlamaması tamamilə mümkündür. Problemi görmək üçün, jurnallara baxmaq üçün **`journalctl`** alətini istifadə edə bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ journalctl -u ssh.service --no-pager
-- Logs begin at Wed 2020-05-13 17:30:52 CEST, end at Fri 2020-05-15 16:00:14 CEST. --
Mai 13 20:38:44 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 13 20:38:44 inlane sshd[2722]: Server listening on 0.0.0.0 port 22.
Mai 13 20:38:44 inlane sshd[2722]: Server listening on :: port 22.
Mai 13 20:38:44 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 13 20:39:06 inlane sshd[3939]: Connection closed by 10.22.2.1 port 36444 [preauth]
Mai 13 20:39:27 inlane sshd[3942]: Accepted password for master from 10.22.2.1 port 36452 ssh2
Mai 13 20:39:27 inlane sshd[3942]: pam_unix(sshd:session): session opened for user master by (uid=0)
Mai 13 20:39:28 inlane sshd[3942]: pam_unix(sshd:session): session closed for user master
Mai 14 02:04:49 inlane sshd[2722]: Received signal 15; terminating.
Mai 14 02:04:49 inlane systemd[1]: Stopping OpenBSD Secure Shell server...
Mai 14 02:04:49 inlane systemd[1]: Stopped OpenBSD Secure Shell server.
-- Reboot --
```

-----

## Prosesi Sonlandırmaq (Kill a Process)

Bir proses aşağıdakı vəziyyətlərdə ola bilər:

  * **İşləyən (Running)**
  * **Gözləyən (Waiting)** (bir hadisə və ya sistem resursu gözləyir)
  * **Dayandırılmış (Stopped)**
  * **Zombi (Zombie)** (dayandırılıb, lakin hələ də proses cədvəlində bir qeydi var).

Proseslər **`kill`**, **`pkill`**, **`pgrep`** və **`killall`** istifadə edilərək idarə oluna bilər. Bir proseslə qarşılıqlı əlaqə qurmaq üçün ona bir **siqnal** göndərməliyik. Bütün siqnallara aşağıdakı əmrlə baxa bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ kill -l
 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 6) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
11) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
16) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
21) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
26) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
31) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
38) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
43) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
48) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
53) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
58) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
63) SIGRTMAX-1  64) SIGRTMAX
```

Ən çox istifadə olunan siqnallar bunlardır:

| Siqnal | Təsvir |
| :---: | :--- |
| **1** | **`SIGHUP`** - Ona nəzarət edən terminal bağlandıqda bir prosesə göndərilir. |
| **2** | **`SIGINT`** - Bir istifadəçi prosesi dayandırmaq üçün nəzarət edən terminalda **`[Ctrl] + C`** basdıqda göndərilir. |
| **3** | **`SIGQUIT`** - Bir istifadəçi çıxmaq üçün **`[Ctrl] + D`** basdıqda göndərilir. |
| **9** | **`SIGKILL`** - Heç bir təmizləmə əməliyyatı olmadan bir prosesi dərhal sonlandırır. |
| **15** | **`SIGTERM`** - Proqramın dayandırılması. |
| **19** | **`SIGSTOP`** - Proqramı dayandırır. Daha sonra idarə oluna bilməz. |
| **20** | **`SIGTSTP`** - Bir istifadəçi bir xidmətin dayandırılmasını tələb etmək üçün **`[Ctrl] + Z`** basdıqda göndərilir. İstifadəçi daha sonra onu idarə edə bilər. |

Məsələn, bir proqram donarsa, onu aşağıdakı əmrlə məcburi şəkildə sonlandıra bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ kill 9 <PID> 
```

-----

## Prosesi Arxa Plana Atmaq (Background a Process)

Bəzən, cari sessiyadan sistemlə qarşılıqlı əlaqə qurmağa və ya digər prosesləri başlamağa davam etmək üçün yeni başladığımız skan və ya prosesi **arxa plana (background)** atmaq lazım olacaq. Daha əvvəl gördüyümüz kimi, bunu **`[Ctrl + Z]`** qısa yolu ilə edə bilərik. Yuxarıda qeyd edildiyi kimi, bu, prosesi dayandıran **`SIGTSTP`** siqnalını nüvəyə (kernel) göndərir.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ping -c 10 www.hackthebox.eu
nijatmansimov@htb[/htb]$ vim tmpfile
[Ctrl + Z]
[2]+  Stopped                 vim tmpfile
```

İndi bütün arxa plan prosesləri aşağıdakı əmrlə göstərilə bilər.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ jobs
[1]+  Stopped                 ping -c 10 www.hackthebox.eu
[2]+  Stopped                 vim tmpfile
```

**`[Ctrl] + Z`** qısa yolu prosesləri **dayandırır** və onlar daha da icra edilməyəcəklər. Onu arxa planda işlək saxlamaq üçün, prosesi arxa plana atmaq üçün **`bg`** əmrini daxil etməliyik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ bg
nijatmansimov@htb[/htb]$ --- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 113482ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
```

Başqa bir seçim, əmrin sonunda **AND simvolu (`&`)** ilə prosesi avtomatik olaraq arxa plana təyin etməkdir.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ping -c 10 www.hackthebox.eu &
[1] 10825
PING www.hackthebox.eu (172.67.1.1) 56(84) bytes of data.
```

Proses bitdikdən sonra nəticələri görəcəyik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ --- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9210ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
```

-----

## Prosesi Ön Plana Çəkmək (Foreground a Process)

Bundan sonra, bütün arxa plan proseslərini siyahılaşdırmaq üçün **`jobs`** əmrini istifadə edə bilərik. Arxa plandakı proseslər istifadəçi qarşılıqlı əlaqəsi tələb etmir və prosesin əvvəlcə bitməsini gözləmədən eyni qabıq sessiyasını istifadə edə bilərik. Skan və ya proses işini bitirdikdən sonra, terminal tərəfindən prosesin bitməsi barədə xəbərdarlıq alacağıq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ jobs
[1]+  Running                 ping -c 10 www.hackthebox.eu &
```

Əgər arxa plan prosesini **ön plana (foreground)** çəkmək və onunla yenidən qarşılıqlı əlaqə qurmaq istəyiriksə, **`fg <ID>`** əmrini istifadə edə bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ fg 1
ping -c 10 www.hackthebox.eu

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9206ms
```

-----

## Birdən Çox Əmri İcra Etmək (Execute Multiple Commands)

Bir neçə əmri ardıcıl olaraq işə salmaq üçün üç imkan var. Bunlar aşağıdakılarla ayrılır:

  * **Nöqtəli vergül** (`;`)
  * **İkiqat amperessant simvolu** (`&&`)
  * **Borular** (`|`, **Pipes**)

Onlar arasındakı fərq əvvəlki proseslərin necə işlənməsindədir və əvvəlki prosesin uğurla, yoxsa səhvlə tamamlanmasından asılıdır. **Nöqtəli vergül (`;`)** bir əmr ayırıcısıdır və əvvəlki əmrlərin nəticələrini və səhvlərini nəzərə almadan əmrləri icra edir.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ echo '1'; echo '2'; echo '3'
1
2
3
```

Məsələn, eyni əmri icra etsək, lakin ikinci yerdə onu mövcud olmayan bir faylla **`ls`** əmri ilə əvəz etsək, bir səhv alırıq və üçüncü əmr yenə də icra olunacaq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ echo '1'; ls MISSING_FILE; echo '3'
1
ls: cannot access 'MISSING_FILE': No such file or directory
3
```

Ancaq əmrləri ardıcıl olaraq işə salmaq üçün **ikiqat AND simvollarını (`&&`)** istifadə etsək, vəziyyət fərqli görünür. Əmrlərdən birində səhv olarsa, sonrakılar artıq icra edilməyəcək və bütün proses dayandırılacaq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ echo '1' && ls MISSING_FILE && echo '3'
1
ls: cannot access 'MISSING_FILE': No such file or directory
```

**Borular (`|`)** isə yalnız əvvəlki proseslərin düzgün və səhvsiz işləməsindən deyil, həm də əvvəlki proseslərin nəticələrindən asılıdır.
