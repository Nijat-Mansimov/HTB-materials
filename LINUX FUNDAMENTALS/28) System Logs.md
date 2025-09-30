## Linux Sistem Logları

Linux sistem logları, sistemin və üzərində baş verən fəaliyyətlər haqqında məlumatları ehtiva edən fayllar toplusudur. Bu loglar sistemin monitorinqi, nasazlıqların aradan qaldırılması (troubleshooting) və potensial təhlükəsizlik zəifliklərinin aşkar edilməsi üçün kritik əhəmiyyətə malikdir. Penetrasiya testçiləri olaraq, logları təhlil edərək sistem davranışına, şəbəkə aktivliyinə və istifadəçi fəaliyyətinə dair dəyərli məlumatlar əldə edə bilərik.

### Əsas Log Növləri və Yerləri

Linux sistemlərində bir neçə fərqli log növü var:

| Log Növü | Məzmun | Tipik Yerləşmə |
| :--- | :--- | :--- |
| **Kernel Logları** | Kernel haqqında məlumatlar, **aparat drayverləri**, sistem çağırışları və kernel hadisələri. | `/var/log/kern.log` |
| **Sistem Logları** | Sistem səviyyəsində hadisələr: **xidmət start/stop**, **giriş cəhdləri**, sistem yenidən başlamaları. | `/var/log/syslog` |
| **İdentifikasiya (Authentication) Logları** | İstifadəçi identifikasiya cəhdləri (uğurlu və uğursuz girişlər), `sudo` istifadəsi. | `/var/log/auth.log` |
| **Tətbiq Logları** | Xüsusi tətbiqlərin fəaliyyətləri (məsələn, veb serverlər, verilənlər bazaları). | Tətbiqə görə fərqlənir (`/var/log/apache2/error.log`, `/var/log/mysql/error.log` və s.) |
| **Təhlükəsizlik Logları** | Təhlükəsizlik tətbiqləri tərəfindən qeydə alınan hadisələr (məsələn, Fail2ban, UFW). | Tətbiqə görə fərqlənir (`/var/log/fail2ban.log`, `/var/log/ufw.log`) |

-----

## Detallı Log Təhlili

### Kernel Logları (`/var/log/kern.log`)

Bu loglar drayverlərdəki **zəiflikləri** və ya **köhnəlmiş drayverləri** aşkar etməyə kömək edə bilər. Həmçinin, sistem çöküşləri və ya **DoS** (xidmətdən imtina) ilə nəticələnə biləcək resurs məhdudiyyətləri haqqında məlumat verə bilər. Şübhəli sistem çağırışları da burada qeydə alınır.

### Sistem Logları (`/var/log/syslog`)

Bu loglar sistem səviyyəli fəaliyyətləri izləməklə sistemdəki icazəsiz girişi və ya qeyri-adi fəaliyyətləri aşkar etməyə imkan verir.

**Syslog Nümunəsi Təhlili:**

```log
Feb 28 2023 15:04:22 server sshd[3010]: Failed password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:07:19 server sshd[3010]: Accepted password for htb-student from 10.14.15.2 port 50223 ssh2
```

Bu sətirlər **uğursuz şifrə cəhdindən** (brute-force cəhdinin bir hissəsi ola bilər) sonra, eyni hostdan gələn `htb-student` istifadəçisi üçün **uğurlu SSH girişini** göstərir.

-----

### İdentifikasiya Logları (`/var/log/auth.log`)

Bu log xüsusilə **istifadəçi identifikasiyasına** (login) diqqət yetirdiyindən, təhlükəsizlik təhdidlərini müəyyən etmək üçün **`syslog`**-dan daha dəyərlidir.

**Auth.log Nümunəsi Təhlili:**

```log
Feb 28 2023 18:15:01 sshd[5678]: Accepted publickey for admin from 10.14.15.2 port 43210 ssh2: RSA SHA256:+KjEzN2cVhIW/5uJpVX9n5OB5zVJ92FtCZxVzzcKjw
Feb 28 2023 18:15:03 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/bin/bash
Feb 28 2023 18:15:05 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/usr/bin/apt-get install netcat-traditional
```

1.  **`admin`** istifadəçisi **açıq açarla** (public key) uğurla daxil olub.
2.  `admin` istifadəçisi daxil olduqdan dərhal sonra **`sudo`** istifadə edərək **`root`** səviyyəsində `/bin/bash` və `netcat` quraşdırma əmrini icra edir. Bu, onun **`sudoers`** qrupunda olduğunu və yüksək imtiyazlara sahib olduğunu göstərir.

-----

### Tətbiq və Giriş Logları (Application and Access Logs)

Bu loglar veb serverlər və verilənlər bazaları kimi **xüsusi tətbiqlərin** necə işlədiyinə dair məlumatları qeydə alır, bu da **zəif konfiqurasiyaları** və ya **hədəflənmiş istismar cəhdlərini** aşkar etmək üçün kritikdir.

| Xidmət | Access Log-un Standart Yeri |
| :--- | :--- |
| **Apache** | `/var/log/apache2/access.log` (və ya oxşarı) |
| **Nginx** | `/var/log/nginx/access.log` (və ya oxşarı) |
| **OpenSSH** | `/var/log/auth.log` (Ubuntu) və ya `/var/log/secure` (CentOS/RHEL) |
| **MySQL** | `/var/log/mysql/mysql.log` |

**Access Log Girişi Nümunəsi Təhlili:**

```log
2023-03-07T10:15:23+00:00 servername privileged.sh: htb-student accessed /root/hidden/api-keys.txt
```

Bu log sətiri **`htb-student`** istifadəçisinin yüksək imtiyazlı **`privileged.sh`** skriptindən istifadə edərək `/root/hidden/` kataloqundakı **`api-keys.txt`** faylına daxil olduğunu göstərir. Bu, imtiyazların **səhv idarə olunduğunu** və ya **zəifliyə yol açdığını** göstərən bir nümunədir.

### Təhlükəsizlik Logları

Bu loglar xüsusi təhlükəsizlik alətləri tərəfindən yaradılan hadisələri qeyd edir:

  * **Fail2ban:** Uğursuz giriş cəhdlərini `/var/log/fail2ban.log` faylında qeyd edir.
  * **UFW:** Firewall fəaliyyətini `/var/log/ufw.log` faylında qeyd edir.

-----

## Logların İdarə Edilməsi və Təhlili

Logların düzgün konfiqurasiyası (məsələn, uyğun **log səviyyələrinin** təyin edilməsi və **log rotasiyasının** qurulması) təhlükəsizliyin təmin edilməsi üçün vacibdir.

Logları təhlil etmək üçün Linux-un əmr sətri alətlərindən istifadə edilə bilər:

  * **`tail`**: Log fayllarının son hissəsinə baxmaq və ya logları real vaxtda izləmək (`tail -f`).
  * **`grep`**: Loglarda müəyyən sətirləri (məsələn, "Failed password" və ya "Accepted") axtarmaq.
  * **`sed`**: Logları süzgəcdən keçirmək və ya dəyişdirmək.

Logların müntəzəm təhlili təhlükəsizlik pozuntularını vaxtında aşkar etməyə və sistem problemlərini aradan qaldırmağa kömək edir.
