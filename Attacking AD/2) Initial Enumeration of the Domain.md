Elbəttə, mətni Azərbaycan dilinə tərcümə edirəm. Mətn **Inlanefreight** şirkətinə qarşı keçirilən **Domen Başlanğıc Təftişi** (Initial Enumeration of the Domain) adlı sızma testi ssenarisini təsvir edir.

## Domenin Başlanğıc Təftişi

Biz **Inlanefreight** şirkətinə qarşı AD (Active Directory) mərkəzli sızma testimizin lap əvvəlindəyik. Bəzi əsas məlumat toplama işləri görmüşük və əhatə sənədləri vasitəsilə müştəridən nə gözlədiyimiz barədə təsəvvür əldə etmişik.

### Quruluşun Müəyyənləşdirilməsi (Setting Up)

Testin bu birinci hissəsi üçün bizə şəbəkə daxilində yerləşdirilmiş bir hücum hostunda başlayırıq. Bu, müştərinin daxili sızma testi aparmaq üçün seçə biləcəyi ümumi yollardan biridir. Müştərinin test üçün seçə biləcəyi quruluş növlərinin siyahısı bunlardır:

  * Bizim nəzarətimizdə olan bir **jump host**-a VPN üzərindən geri zəng edən və bizim SSH vasitəsilə daxil ola bildiyimiz, onların daxili infrastrukturunda bir virtual maşın (adətən Linux) kimi bir sızma testi distrosu.
  * VPN üzərindən bizə geri zəng edən və bizim SSH vasitəsilə daxil ola bildiyimiz, bir ethernet portuna qoşulmuş fiziki cihaz.
  * Bizim noutbukumuzun bir ethernet portuna qoşulduğu ofislərindəki fiziki mövcudiyyət.
  * Daxili şəbəkəyə çıxışı olan, bizim açıq açar autentifikasiyası və whitelisted (ağ siyahıya salınmış) ictimai IP adresimiz vasitəsilə SSH ilə daxil ola bildiyimiz Azure və ya AWS-də bir Linux VM.
  * Daxili şəbəkəyə VPN girişi (biraz məhdudlaşdırıcıdır, çünki LLMNR/NBT-NS Zəhərlənməsi kimi bəzi hücumları həyata keçirə bilməyəcəyik).
  * Müştərinin VPN-nə qoşulmuş korporativ noutbukdan.
  * Fiziki olaraq onların ofisində oturan, məhdud və ya heç internet çıxışı olmayan, yaxud alətləri yükləmək imkanı olmayan idarə olunan iş stansiyasında (adətən Windows). Onlar həmçinin bu variantı seçə bilər, lakin sizə tam internet çıxışı, yerli admin hüququ verə bilər və alətləri istədiyiniz vaxt yükləyə bilməniz üçün son nöqtə mühafizəsini monitorinq rejiminə qoya bilərlər.
  * Citrix və ya bənzəri vasitəsilə daxil olunan VDI (virtual desktop) üzərində, adətən ya uzaqdan VPN, ya da korporativ noutbukdan əldə edilə bilən, idarə olunan iş stansiyası üçün təsvir edilən konfiqurasiyalardan biri ilə.

Bunlar gördüyüm ən ümumi quruluşlardır, baxmayaraq ki, müştəri bunlardan başqa bir variantla da gələ bilər. Müştəri həmçinin bizə yalnız əhatə dairəsində olan IP ünvanları/CIDR şəbəkə diapazonlarının siyahısını verdiyi bir **"grey box"** (boz qutu) yanaşması və ya müxtəlif texnikalardan istifadə edərək kor-koranə bütün kəşfiyyatı etməli olduğumuz **"black box"** (qara qutu) yanaşması seçə bilər. Nəhayət, onlar **qaçınan** (evasive), **qaçınmayan** (non-evasive) və ya **hibrid qaçınan** (hybrid evasive) (səs-küyün nə qədər olduğunu yoxlamaq üçün "sakit" başlayıb, yavaş-yavaş səs-küylü olmağa başlayıb, sonra qaçınmayan testə keçmək) seçə bilərlər. Onlar həmçinin heç bir etimadnaməsiz və ya standart domen istifadəçisi nöqteyi-nəzərindən başlamağımızı da tələb edə bilərlər.

Müştərimiz **Inlanefreight** ən əhatəli qiymətləndirməni axtardığı üçün aşağıdakı yanaşmanı seçib. Hal-hazırda, onların təhlükəsizlik proqramı hər hansı bir qaçınan test və ya "qara qutu" yanaşmasından faydalanmaq üçün kifayət qədər yetkin deyil.

  * Daxili şəbəkələrində bizim jump hosta geri zəng edən, bizim isə SSH ilə daxil olub testi icra edə biləcəyimiz **xüsusi pentest VM**-i.
  * Onlar həmçinin lazım olarsa alətləri yükləyə biləcəyimiz bir **Windows host** da veriblər.
  * Bizdən **autentifikasiya olunmamış** vəziyyətdən başlamağımızı xahiş ediblər, lakin Windows hücum hostuna daxil olmaq üçün istifadə edilə biləcək standart bir **domen istifadəçi hesabı** da (htb-student) veriblər.
  * **"Grey box"** testi. Onlar bizə **172.16.5.0/23** şəbəkə diapazonunu veriblər və şəbəkə haqqında başqa heç bir məlumat verməyiblər.
  * **Qaçınmayan** (non-evasive) test.
  * Bizə etimadnamələr və ya ətraflı daxili şəbəkə xəritəsi verilməyib.

-----

### Tapşırıqlar

Bu hissə üçün yerinə yetirməli olduğumuz tapşırıqlar bunlardır:

1.  Daxili şəbəkəni təftiş etmək, **hostları, kritik xidmətləri** və **möhkəmlənmək üçün potensial yolları** müəyyən etmək.
2.  Bura istifadəçiləri, hostları və daha da irəli getmək üçün istifadə edə biləcəyimiz potensial zəiflikləri müəyyən etmək üçün **aktiv və passiv tədbirlər** daxil ola bilər.
3.  Sonradan istifadə etmək üçün qarşımıza çıxan bütün tapıntıları **sənədləşdirmək**. Bu, olduqca vacibdir\!

Biz Linux hücum hostumuzdan **domen istifadəçisi etimadnamələri olmadan** başlayacağıq. Bir çox təşkilat sınaqlara bu şəkildə başlamağınızı istəyəcək ki, sizə əlavə məlumat verməzdən əvvəl bu kor perspektivdən nə edə biləcəyinizi görsün. Bu, rəqibin domenə sızmaq üçün istifadə etməli olduğu potensial yollara (yəni, fişinq hücumu, binaya fiziki giriş, kənardan simsiz giriş (əgər simsiz şəbəkə AD mühitinə toxunursa), hətta fırıldaqçı bir işçi) daha real nəzər salmağa kömək edə bilər. Bu mərhələnin uğurundan asılı olaraq, müştəri testi sürətləndirmək və mümkün qədər çox ərazini əhatə etməyimizə imkan vermək üçün bizə domenə qoşulmuş bir hosta giriş və ya şəbəkə üçün bir sıra etimadnamə təmin edə bilər.

Aşağıda hazırda axtarmalı və seçdiyimiz qeyd alətinə qeyd etməli, mümkün olduqda isə skan/alət çıxışını fayllara yazmalı olduğumuz əsas məlumat nöqtələri verilmişdir.

#### Əsas Məlumat Nöqtələri

| Məlumat Nöqtəsi | Təsvir |
| :--- | :--- |
| **AD İstifadəçiləri** | Şifrə **spray** hücumu üçün hədəf ala biləcəyimiz etibarlı istifadəçi hesablarını təftiş etməyə çalışırıq. |
| **AD-ə Qoşulmuş Kompüterlər** | Əsas kompüterlərə **Domen Kontrollerləri**, fayl serverləri, SQL serverləri, veb serverlər, Exchange poçt serverləri, verilənlər bazası serverləri və s. daxildir. |
| **Əsas Xidmətlər** | **Kerberos, NetBIOS, LDAP, DNS** |
| **Zəif Hostlar və Xidmətlər** | Sürətli qələbə ola biləcək hər şey. (yəni, istismar etmək və möhkəmlənmək üçün asan host) |

-----

### Taktikalar, Texnikalar və Prosedurlar (TTPs)

Bir AD mühitini təftiş etmək plansız yaxınlaşdıqda çətin ola bilər. AD-də çoxlu məlumat saxlanılır və mütərəqqi mərhələlərdə baxılmasa, ayırd etmək çox vaxt apara bilər və çox güman ki, nələrisə buraxarıq. Özümüz üçün bir **oyun planı** tərtib etməli və onu hissə-hissə həll etməliyik. Hər kəs bir qədər fərqli işləyir, buna görə də daha çox təcrübə qazandıqca, bizim üçün ən yaxşı işləyən öz təkrarlana bilən metodologiyamızı inkişaf etdirməyə başlayacağıq. Necə davam etməyimizdən asılı olmayaraq, adətən eyni yerdə başlayır və eyni məlumat nöqtələrini axtarırıq. Bu bölmədə və sonrakı hissələrdə bir çox alətlərlə təcrübə aparacağıq. Hər bir nümunəni təkrar etmək və hətta fərqli alətlərlə nümunələri yenidən yaratmağa çalışmaq vacibdir ki, onların necə fərqli işlədiyini öyrənək, onların sintaksisini mənimsəyək və hansı yanaşmanın bizim üçün ən yaxşı olduğunu tapaq.

Biz əvvəlcə şəbəkədəki hər hansı hostların **passiv identifikasiyası** ilə başlayacağıq, ardından hər bir host haqqında daha çox məlumat tapmaq üçün nəticələrin **aktiv təsdiqlənməsi** ilə davam edəcəyik (hansı xidmətlər işləyir, adlar, potensial zəifliklər və s.). Hansı hostların mövcud olduğunu bildikdən sonra, onlardan maraqlı məlumatlar əldə etmək üçün bu hostları zondlamağa (probing) davam edə bilərik. Bu tapşırıqları yerinə yetirdikdən sonra, dayanmalı və qruplaşmalı, əldə etdiyimiz məlumatlara baxmalıyıq. Bu zaman, ümid edirik ki, domenə qoşulmuş bir hosta möhkəmlənmək üçün bir sıra etimadnamə və ya bir istifadəçi hesabı olacaq və ya Linux hücum hostumuzdan etimadnaməli təftişə başlamaq imkanımız olacaq.

Gəlin bu təftişdə bizə kömək edəcək bir neçə alət və texnikaya baxaq.

-----

### Hostların Müəyyənləşdirilməsi

Əvvəlcə bir az vaxt sərf edib şəbəkəyə qulaq asaq və nə baş verdiyini görək. **Wireshark** və **TCPDump** istifadə edərək "qulağımızı naqilə söykəyə" və hansı hostları və şəbəkə trafikinin növlərini tuta biləcəyimizi görə bilərik. Bu, xüsusilə qiymətləndirmə yanaşması "qara qutu" olduqda faydalıdır. Biz bəzi ARP sorğularını və cavablarını, MDNS-i və digər əsas lay-iki paketlərini (kommutasiya edilmiş şəbəkədə olduğumuz üçün cari yayım domenində məhdudlaşırıq) görürük, bəzilərini aşağıda görə bilərik. Bu, müştərinin şəbəkə quruluşu haqqında bizə bir neçə məlumat verən əla başlanğıcdır.

Ən aşağıya sürüşdürün, hədəfi işə salın, **xfreerdp** istifadə edərək Linux hücum hostuna qoşulun və trafik tutmağa başlamaq üçün **Wireshark**ı işə salın.

**ea-attack01** üzərində Wiresharkı başladın:

```bash
┌─[htb-student@ea-attack01]─[~]
└──╼ $sudo -E wireshark
```

```
11:28:20.487      Main Warn QStandardPaths: runtime directory '/run/user/1001' is not owned by UID 0, but a directory permissions 0700 owned by UID 1001 GID 1002
<SNIP>
```

**Wireshark Çıxışı**

<img width="871" height="399" alt="image" src="https://github.com/user-attachments/assets/3b3374f5-213f-4649-b655-e299b395e795" />

**ARP paketləri** bizi bu hostlardan xəbərdar edir: **172.16.5.5, 172.16.5.25, 172.16.5.50, 172.16.5.100** və **172.16.5.125**.

<img width="1026" height="247" alt="image" src="https://github.com/user-attachments/assets/6120215e-e175-4a74-a4db-8d1df8f91ac3" />

**MDNS** bizi **ACADEMY-EA-WEB01** hostundan xəbərdar edir.

Əgər bizdə GUI olmayan bir host varsa (bu, tipikdir), eyni funksiyaları yerinə yetirmək üçün **tcpdump**, **net-creds** və **NetMiner** və s. istifadə edə bilərik. Biz həmçinin **tcpdump** istifadə edərək tutulmuş məlumatı `.pcap` faylına yaza, onu başqa bir hosta köçürə və Wiresharkda aça bilərik.

**Tcpdump Çıxışı**

```bash
nijatmansimov@htb[/htb]$ sudo tcpdump -i ens224 
```

<img width="1150" height="824" alt="image" src="https://github.com/user-attachments/assets/5705983b-c133-40cb-99cc-fc5eecb5801e" />

Şəbəkə trafikini dinləmək və tutmaq üçün tək bir doğru yol yoxdur. Şəbəkə məlumatlarını emal edə biləcək çoxlu alətlər var. Wireshark və tcpdump yalnız istifadəsi ən asan və ən çox tanınanlardan bir neçəsidir. Üzərində olduğunuz hostdan asılı olaraq, Windows 10-un bütün nəşrlərinə əlavə edilmiş **pktmon.exe** kimi, artıq daxili şəbəkə monitorinq alətiniz ola bilər. Test üçün bir qeyd olaraq, tutduğunuz PCAP trafikini yadda saxlamaq həmişə yaxşı bir fikirdir. Daha çox ipucu axtarmaq üçün sonradan ona yenidən baxa bilərsiniz və bu, hesabatlarınızı yazarkən daxil etmək üçün əla əlavə məlumat təşkil edir.

Şəbəkə trafikinə ilk baxışımız MDNS və ARP vasitəsilə bir neçə hosta işarə etdi. İndi şəbəkə trafikini təhlil etmək və domendə başqa bir şeyin olub olmadığını müəyyən etmək üçün **Responder** adlı bir alətdən istifadə edək.

**Responder** LLMNR, NBT-NS və MDNS sorğu və cavablarını dinləmək, təhlil etmək və zəhərləmək üçün qurulmuş bir alətdir. Onun daha çox funksiyası var, lakin hələlik istifadə etdiyimiz tək şey alətin **Təhlil (Analyze) rejimidir**. Bu, şəbəkəyə passiv şəkildə qulaq asacaq və zəhərlənmiş paketlər göndərməyəcək. Bu aləti sonrakı bölmələrdə daha dərindən əhatə edəcəyik.

**Responder'ı Başlatmaq**

```bash
sudo responder -I ens224 -A 
```

<img width="1150" height="824" alt="image" src="https://github.com/user-attachments/assets/64ef2180-f5b7-489d-bf9a-29e65178c221" />

**Responder Nəticələri**

Responder-i passiv təhlil rejimi ilə işə saldığımız zaman, sessiyamızda sorğuların axdığını görəcəyik. Aşağıda əvvəlki Wireshark tutmalarımızda qeyd olunmayan bir neçə unikal host tapdığımızı görə bilərik. Gözəl bir hədəf IP və DNS host adları siyahısı yaratmağa başladığımız üçün bunları qeyd etməyə dəyər.

Passiv yoxlamalarımız bizə daha dərindən təftiş etmək üçün qeyd etməli olduğumuz bir neçə host verdi. İndi **fping** istifadə edərək subnetin sürətli bir **ICMP sweep**-i ilə başlayan bəzi **aktiv yoxlamalar** edək.

**FPing** bizə standart `ping` tətbiqi ilə oxşar imkanlar təqdim edir, çünki o, bir hosta çatmaq və onunla qarşılıqlı əlaqə qurmaq üçün ICMP sorğularından və cavablarından istifadə edir. **fping** bir anda birdən çox hosta qarşı ICMP paketləri göndərmək qabiliyyətinə və onun skriptlə işləmə imkanına görə seçilir. Həmçinin, o, bir hosta çoxsaylı sorğuların geri qayıtmasını gözləmək əvəzinə, dövri şəkildə hostları sorğulayaraq, **round-robin** rejimində işləyir. Bu yoxlamalar daxili şəbəkədə başqa nəyin aktiv olduğunu müəyyən etməyə kömək edəcək. ICMP hər şeyi tapmaq üçün bir yol deyil, lakin nəyin mövcud olduğu barədə ilkin fikir əldə etmək üçün asan bir yoldur. Digər açıq portlar və aktiv protokollar sonradan hədəfləmək üçün yeni hostlara işarə edə bilər. Gəlin onu işdə görək.

**FPing Aktiv Yoxlamalar**

Burada **fping**-i bir neçə bayraqla işə salacağıq: **a** aktiv olan hədəfləri göstərmək üçün, **s** skanın sonunda statistikanı çap etmək üçün, **g** CIDR şəbəkəsindən hədəf siyahısı yaratmaq üçün və **q** hər bir hədəf üçün nəticələri göstərməmək üçün.

```bash
nijatmansimov@htb[/htb]$ fping -asgq 172.16.5.0/23
```

```
172.16.5.5
172.16.5.25
172.16.5.50
172.16.5.100
172.16.5.125
172.16.5.200
172.16.5.225
172.16.5.238
172.16.5.240

      510 targets
        9 alive
      501 unreachable
        0 unknown addresses

     2004 timeouts (waiting for response)
     2013 ICMP Echos sent
        9 ICMP Echo Replies received
     2004 other ICMP received

 0.029 ms (min round trip time)
 0.396 ms (avg round trip time)
 0.799 ms (max round trip time)
      15.366 sec (elapsed real time)
```

Yuxarıdakı əmr `/23` şəbəkəsində hansı hostların aktiv olduğunu təsdiqləyir və bunu hədəf siyahısındakı hər bir IP üçün nəticələrlə terminalı spam etmədən sakitcə edir. Biz uğurlu nəticələri və passiv yoxlamalarımızdan əldə etdiyimiz məlumatları **Nmap** ilə daha ətraflı skan etmək üçün bir siyahıda birləşdirə bilərik. `fping` əmrindən hücum hostumuz daxil olmaqla 9 "aktiv host" görə bilərik.

**Qeyd:** Laboratoriya şəbəkəsinin ölçüsünə görə hədəf şəbəkədəki skan nəticələri bu bölmənin əmr çıxışından fərqlənəcək. Hələ də bu alətlərin necə işlədiyini tətbiq etmək və bu laboratoriyada aktiv olan hər bir hostu qeyd etmək üçün hər bir nümunəni təkrar etməyə dəyər.

-----

### Nmap Skanlaması

Şəbəkəmizdə aktiv hostların siyahısına sahib olduğumuz üçün, bu hostları daha da təftiş edə bilərik. Biz hər bir hostun hansı xidmətləri işlətdiyini müəyyən etməyə, **Domen Kontrollerləri** və **veb serverlər** kimi kritik hostları müəyyənləşdirməyə və daha sonra zondlamaq üçün potensial zəif hostları tapmağa çalışırıq. AD-yə diqqət yetirdiyimiz üçün, geniş bir skan etdikdən sonra, **DNS, SMB, LDAP** və **Kerberos** kimi AD xidmətləri ilə birlikdə görülən standart protokollara diqqət yetirməyimiz ağıllı olardı. Aşağıda sadə bir Nmap skanının sürətli bir nümunəsi verilmişdir.

```bash
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```

`-A` (**Agressive scan options**) skanı bir neçə funksiyanı yerinə yetirəcək. Ən vaciblərindən biri, veb xidmətlərini, domen xidmətlərini və s. daxil etmək üçün məşhur portların sürətli təftişidir. `hosts.txt` faylımız üçün Responder və fping-dən əldə etdiyimiz bəzi nəticələr üst-üstə düşdü (adı və IP ünvanını tapdıq), buna görə də sadə saxlamaq üçün skan üçün yalnız IP ünvanı `hosts.txt`-ə daxil edildi.

**NMAP Nəticələrinin Əsas Məqamları**

```
Nmap scan report for inlanefreight.local (172.16.5.5)
Host is up (0.069s latency).
Not shown: 987 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Simple DNS Plus
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2022-04-04 15:12:06Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: INLANEFREIGHT.LOCAL0., Site: Default-First-Site-Name)
|_ssl-date: 2022-04-04T15:12:53+00:00; -1s from scanner time.
| ssl-cert: Subject:
| Subject Alternative Name: DNS:ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
| Issuer: commonName=INLANEFREIGHT-CA
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-03-30T22:40:24
| Not valid after:  2023-03-30T22:40:24
| MD5:   3a09 d87a 9ccb 5498 2533 e339 ebe3 443f
|_SHA-1: 9731 d8ec b219 4301 c231 793e f913 6868 d39f 7920
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: INLANEFREIGHT.LOCAL0., Site: Default-First-Site-Name)
<SNIP>  
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: INLANEFREIGHT.LOCAL0., Site: Default-First-Site-Name)
3269/tcp open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: INLANEFREIGHT.LOCAL0., Site: Default-First-Site-Name)
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info:
|   Target_Name: INLANEFREIGHT
|   NetBIOS_Domain_Name: INLANEFREIGHT
|   NetBIOS_Computer_Name: ACADEMY-EA-DC01
|   DNS_Domain_Name: INLANEFREIGHT.LOCAL
|   DNS_Computer_Name: ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL
|   DNS_Tree_Name: INLANEFREIGHT.LOCAL
|   Product_Version: 10.0.17763
|_  System_Time: 2022-04-04T15:12:45+00:00
<SNIP>
5357/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Service Unavailable
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: Host: ACADEMY-EA-DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
```

Skanlarımız bizə **NetBIOS** və **DNS** tərəfindən istifadə edilən adlandırma standartını təqdim etdi, bəzi hostlarda **RDP**-nin açıq olduğunu görə bilərik və onlar bizi **INLANEFREIGHT.LOCAL** domeninin əsas Domen Kontrolleri (**ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL**) istiqamətinə yönəltdilər. Aşağıdakı nəticələr ehtimal ki, köhnəlmiş bir hostla bağlı maraqlı nəticələr göstərir (cari laboratoriyamızda yoxdur).

```
nijatmansimov@htb[/htb]$ nmap -A 172.16.5.100

Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-08 13:42 EDT
Nmap scan report for 172.16.5.100
Host is up (0.071s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 7.5
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Microsoft-IIS/7.5
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  https?
445/tcp   open  microsoft-ds Windows Server 2008 R2 Standard 7600 microsoft-ds
1433/tcp  open  ms-sql-s     Microsoft SQL Server 2008 R2 10.50.1600.00; RTM
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2022-04-08T17:38:25
|_Not valid after:  2052-04-08T17:38:25
|_ssl-date: 2022-04-08T17:43:53+00:00; 0s from scanner time.
| ms-sql-ntlm-info: 
|   Target_Name: INLANEFREIGHT
|   NetBIOS_Domain_Name: INLANEFREIGHT
|   NetBIOS_Computer_Name: ACADEMY-EA-CTX1
|   DNS_Domain_Name: INLANEFREIGHT.LOCAL
|   DNS_Computer_Name: ACADEMY-EA-CTX1.INLANEFREIGHT.LOCAL
|_  Product_Version: 6.1.7600
Host script results:
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| ms-sql-info: 
|   172.16.5.100:1433: 
|     Version: 
|       name: Microsoft SQL Server 2008 R2 RTM
|       number: 10.50.1600.00
|       Product: Microsoft SQL Server 2008 R2
|       Service pack level: RTM
|       Post-SP patches applied: false
|_    TCP port: 1433
|_nbstat: NetBIOS name: ACADEMY-EA-CTX1, NetBIOS user: <unknown>, NetBIOS MAC: 00:50:56:b9:c7:1c (VMware)
| smb-os-discovery: 
|   OS: Windows Server 2008 R2 Standard 7600 (Windows Server 2008 R2 Standard 6.1)
|   OS CPE: cpe:/o:microsoft:windows_server_2008::-
|   Computer name: ACADEMY-EA-CTX1
|   NetBIOS computer name: ACADEMY-EA-CTX1\x00
|   Domain name: INLANEFREIGHT.LOCAL
|   Forest name: INLANEFREIGHT.LOCAL
|   FQDN: ACADEMY-EA-CTX1.INLANEFREIGHT.LOCAL
|_  System time: 2022-04-08T10:43:48-07:00

<SNIP>
```

Yuxarıdakı çıxışdan görə bilərik ki, köhnəlmiş əməliyyat sistemi (çıxışa əsasən Windows 7, 8 və ya Server 2008) işləyən potensial bir hostumuz var. Bu, bizi maraqlandırır, çünki bu AD mühitində köhnə əməliyyat sistemlərinin işlədiyi deməkdir. Bu, həmçinin **EternalBlue**, **MS08-067** və digərləri kimi köhnə **exploit**-lərin işləməsi və bizə **SYSTEM** səviyyəli bir **shell** təmin etməsi potensialı deməkdir. Köhnə proqram təminatı və ya istifadə müddəti bitmiş əməliyyat sistemlərini işlədən hostların olması qəribə səslənsə də, böyük müəssisə mühitlərində hələ də yayğındır. Tez-tez bəzi proseslərin və ya avadanlıqların, məsələn, bir istehsal xətti və ya HVAC-ın köhnə əməliyyat sistemi üzərində qurulduğunu və uzun müddətdir mövcud olduğunu görəcəksiniz. Bu cür avadanlıqları şəbəkədən ayırmaq baha başa gəlir və bir təşkilata zərər verə bilər, buna görə də köhnə hostlar çox vaxt yerində qalır. Onlar, böyük ehtimalla, bu sistemlərin ətrafında **Firewall**-lar, **IDS/IPS** və digər monitorinq və mühafizə həllərindən ibarət sərt bir xarici qabıq qurmağa çalışacaqlar. Əgər onlardan birinə yol tapa bilsəniz, bu, böyük bir işdir və sürətli və asan bir möhkəmlənmə ola bilər. Lakin, köhnə sistemləri istismar etməzdən əvvəl, hücum sistemin qeyri-sabitliyinə səbəb ola biləcəyi və ya bir xidməti və ya hostu dayandıra biləcəyi halında, müştərimizi xəbərdar etməli və yazılı razılığını almalıyıq. Onlar sadəcə müşahidə etməyimizi, hesabat verməyimizi və sistemi aktiv şəkildə istismar etmədən irəliləməyimizi üstün tuta bilərlər.

Bu skanların nəticələri bizə təkcə host skanlaması deyil, həm də potensial domen təftiş yollarını haradan axtarmağa başlayacağımıza dair ipucu verəcək. Bir domen istifadəçi hesabına yol tapmalıyıq. Nəticələrimizə baxaraq, domen xidmətlərini (DC01, MX01, WS01 və s.) yerləşdirən bir neçə server tapdıq. İndi nəyin mövcud olduğunu və hansı xidmətlərin işlədiyini bildiyimizə görə, həmin serverləri sorğulaya və istifadəçiləri təftiş etməyə cəhd edə bilərik. Nmap skanlarını yerinə yetirərkən ən yaxşı təcrübə olaraq `-oA` bayrağından istifadə etməyi unutmayın. Bu, skan nəticələrimizin qeyd məqsədləri üçün bir neçə formatda və manipulyasiya edilə bilən və digər alətlərə ötürülə bilən formatlarda olmasını təmin edəcək.

Hansı skanları işlətdiyimizin və onların necə işlədiyinin fərqində olmalıyıq. Bəzi Nmap skriptləşdirilmiş skanlar, sistemin qeyri-sabitliyinə səbəb ola biləcək və ya onu oflayn edərək müştəri üçün problemlər yarada biləcək aktiv zəiflik yoxlamalarını bir hosta qarşı işə salır. Məsələn, sensorlar və ya məntiq nəzarətçiləri kimi cihazları olan bir şəbəkəyə qarşı böyük bir kəşfiyyat skanı aparmaq, potensial olaraq onları həddən artıq yükləyə bilər və müştərinin sənaye avadanlıqlarını poza bilər, bu da məhsulun və ya imkanın itirilməsinə səbəb olur. Müştəri mühitində işlətməzdən əvvəl istifadə etdiyiniz skanları başa düşməyə vaxt ayırın.

Çox güman ki, daha sonrakı təftiş üçün bu nəticələrə qayıdacağıq, buna görə də onları unutmayın. Bizim möhkəmlənmək və əsl əyləncəyə başlamaq üçün bir domen istifadəçi hesabına və ya domenə qoşulmuş bir hostda **SYSTEM** səviyyəli girişə yol tapmalıyıq. Gəlin bir istifadəçi hesabı tapmağa başlayaq.

-----

### İstifadəçilərin Müəyyənləşdirilməsi

Əgər müştərimiz bizə testə başlamaq üçün bir istifadəçi təmin etməsə (bu, tez-tez baş verir), ya bir istifadəçi üçün **açıq mətn etimadnamələri** və ya **NTLM parol heşi** əldə etməklə, ya da domenə qoşulmuş bir hostda **SYSTEM shell** və ya bir domen istifadəçisi kontekstində bir shell əldə etməklə domendə möhkəmlənməyin bir yolunu tapmalıyıq. Etibarlı etimadnamələri olan bir istifadəçi əldə etmək, daxili sızma testinin erkən mərhələlərində çox vacibdir. Bu giriş (hətta ən aşağı səviyyədə) təftiş aparmaq və hətta hücumlar etmək üçün bir çox imkanlar açır. Gəlin qiymətləndirməmizdə daha sonra istifadə etmək üçün domendə etibarlı istifadəçilərin siyahısını toplamağa başlaya biləcəyimiz bir yola baxaq.

#### Kerbrute - Daxili AD İstifadəçi Adının Təftişi

**Kerbrute** domen hesabı təftişi üçün daha **gizli** bir seçim ola bilər. O, Kerberos pre-autentifikasiya uğursuzluqlarının tez-tez loqları və ya xəbərdarlıqları işə salmayacağı faktından istifadə edir. Kerbrute-u **Insidetrust**-dan **jsmith.txt** və ya **jsmith2.txt** istifadəçi siyahıları ilə birlikdə istifadə edəcəyik. Bu repozitoriya, autentifikasiya olunmamış perspektivdən başlayarkən istifadəçiləri təftiş etməyə cəhd edərkən son dərəcə faydalı ola biləcək bir çox fərqli istifadəçi siyahılarını ehtiva edir. Kerbrute-u daha əvvəl tapdığımız DC-yə yönəldə və ona bir wordlist (söz siyahısı) verə bilərik. Alət sürətlidir və tapılan hesabların etibarlı olub-olmaması barədə nəticələr veriləcək, bu da şifrə **spray** hücumları kimi hücumları başlatmaq üçün əla başlanğıc nöqtəsidir, hansını ki, bu modulun sonrakı hissəsində ətraflı əhatə edəcəyik.

Kerbrute ilə başlamaq üçün, Linux, Windows və Mac-dan test üçün alətin əvvəlcədən tərtib edilmiş **binar** fayllarını yükləyə bilərik və ya özümüz tərtib edə bilərik. Bu, bir müştəri mühitinə daxil etdiyimiz hər hansı bir alət üçün ümumiyyətlə ən yaxşı təcrübədir. Seçdiyimiz sistemdə istifadə etmək üçün binar faylları tərtib etmək üçün əvvəlcə repozitoriyanı **klonlayırıq**:

**Kerbrute GitHub Repozitoriyasını Klonlamaq**

```bash
nijatmansimov@htb[/htb]$ sudo git clone https://github.com/ropnop/kerbrute.git
```

```
Cloning into 'kerbrute'...
remote: Enumerating objects: 845, done.
remote: Counting objects: 100% (47/47), done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 845 (delta 18), reused 28 (delta 10), pack-reused 798
Receiving objects: 100% (845/845), 419.70 KiB | 2.72 MiB/s, done.
Resolving deltas: 100% (371/371), done.
```

`make help` yazmaq mövcud tərtib seçimlərini göstərəcək.

**Tərtib Seçimlərini Siyahılamaq**

```bash
nijatmansimov@htb[/htb]$ make help
```

```
help:          Show this help.
windows:     Make Windows x86 and x64 Binaries
linux:       Make Linux x86 and x64 Binaries
mac:         Make Darwin (Mac) x86 and x64 Binaries
clean:       Delete any binaries
all:         Make Windows, Linux and Mac x86/x64 Binaries
```

Biz yalnız bir binar faylı tərtib etməyi seçə bilərik və ya `make all` yazıb Linux, Windows və Mac sistemlərində istifadə üçün hər birindən birini tərtib edə bilərik (hər biri üçün x86 və x64 versiyası).

**Çoxsaylı Platformalar və Arxitekturalar üçün Tərtib etmək**

```bash
nijatmansimov@htb[/htb]$ sudo make all
```

```
go: downloading github.com/spf13/cobra v1.1.1
go: downloading github.com/op/go-logging v0.0.0-20160315200505-970db520ece7
go: downloading github.com/ropnop/gokrb5/v8 v8.0.0-20201111231119-729746023c02
go: downloading github.com/spf13/pflag v1.0.5
go: downloading github.com/jcmturner/gofork v1.0.0
go: downloading github.com/hashicorp/go-uuid v1.0.2
go: downloading golang.org/x/crypto v0.0.0-20201016220609-9e8e0b390897
go: downloading github.com/jcmturner/rpc/v2 v2.0.2
go: downloading github.com/jcmturner/dnsutils/v2 v2.0.0
go: downloading github.com/jcmturner/aescts/v2 v2.0.0
go: downloading golang.org/x/net v0.0.0-20200114155413-6afb5195e5aa
cd /tmp/kerbrute
rm -f kerbrute kerbrute.exe kerbrute kerbrute.exe kerbrute.test kerbrute.test.exe kerbrute.test kerbrute.test.exe main main.exe
rm -f /root/go/bin/kerbrute
Done.
Building for windows amd64..

<SNIP>
```

Yeni yaradılmış `dist` qovluğu tərtib edilmiş binar fayllarımızı ehtiva edəcək.

**dist-də Tərtib Edilmiş Binar Faylları Siyahılamaq**

```bash
nijatmansimov@htb[/htb]$ ls dist/
```

```
kerbrute_darwin_amd64  kerbrute_linux_386  kerbrute_linux_amd64  kerbrute_windows_386.exe  kerbrute_windows_amd64.exe
```

Daha sonra binar faylı düzgün işlədiyindən əmin olmaq üçün sınaqdan keçirə bilərik. Biz hədəf mühitdə təmin edilmiş **Parrot Linux** hücum hostunda x64 versiyasından istifadə edəcəyik.

**kerbrute\_linux\_amd64 Binar Faylını Sınaqdan Keçirmək**

```bash
nijatmansimov@htb[/htb]$ ./kerbrute_linux_amd64 
```

```
    __           __             __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/           
                                          
Version: dev (9cfb81e) - 02/17/22 - Ronnie Flathers @ropnop

This tool is designed to assist in quickly bruteforcing valid Active Directory accounts through Kerberos Pre-Authentication.
It is designed to be used on an internal Windows domain with access to one of the Domain Controllers.
Warning: failed Kerberos Pre-Auth counts as a failed login and WILL lock out accounts

Usage:
  kerbrute [command]
  
  <SNIP>
```

Aləti sistemin istənilən yerindən asanlıqla əlçatan etmək üçün onu **PATH**-a əlavə edə bilərik.

**Aləti PATH-a Əlavə etmək**

```bash
nijatmansimov@htb[/htb]$ echo $PATH
/home/htb-student/.local/bin:/snap/bin:/usr/sandbox/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/usr/share/games:/usr/local/sbin:/usr/sbin:/sbin:/snap/bin:/usr/local/sbin:/usr/sbin:/sbin:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games:/home/htb-student/.dotnet/tools
```

**Binar Faylı Köçürmək**

```bash
nijatmansimov@htb[/htb]$ sudo mv kerbrute_linux_amd64 /usr/local/bin/kerbrute
```

İndi sistemin istənilən yerindən `kerbrute` yaza bilərik və alətə daxil ola biləcəyik. Sisteminizdə yuxarıdakı addımları izləməkdən və tətbiq etməkdən çekinməyin. İndi alətdən istifadə edərək ilkin istifadəçi siyahısını toplamaq üçün bir nümunəyə baxaq.

**Kerbrute ilə İstifadəçiləri Təftiş etmək**

```bash
nijatmansimov@htb[/htb]$ kerbrute userenum -d INLANEFREIGHT.LOCAL --dc 172.16.5.5 jsmith.txt -o valid_ad_users
```

```
2021/11/17 23:01:46 >  Using KDC(s):
2021/11/17 23:01:46 >    172.16.5.5:88
2021/11/17 23:01:46 >  [+] VALID USERNAME:        jjones@INLANEFREIGHT.LOCAL
2021/11/17 23:01:46 >  [+] VALID USERNAME:        sbrown@INLANEFREIGHT.LOCAL
2021/11/17 23:01:46 >  [+] VALID USERNAME:        tjohnson@INLANEFREIGHT.LOCAL
2021/11/17 23:01:50 >  [+] VALID USERNAME:        evalentin@INLANEFREIGHT.LOCAL

 <SNIP>
 
2021/11/17 23:01:51 >  [+] VALID USERNAME:        sgage@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:        jshay@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:        jhermann@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:        whouse@INLANEFREIGHT.LOCAL
2021/11/17 23:01:51 >  [+] VALID USERNAME:        emercer@INLANEFREIGHT.LOCAL
2021/11/17 23:01:52 >  [+] VALID USERNAME:        wshepherd@INLANEFREIGHT.LOCAL
2021/11/17 23:01:56 >  Done! Tested 48705 usernames (56 valid) in 9.940 seconds
```

Çıxışımızdan görə bilərik ki, **INLANEFREIGHT.LOCAL** domenində **56 istifadəçini** təsdiqlədik və bunu etmək cəmi bir neçə saniyə çəkdi. İndi biz bu nəticələri götürə və hədəfli şifrə **spray** hücumlarında istifadə etmək üçün bir siyahı qura bilərik.

-----

### Potensial Zəifliklərin Müəyyənləşdirilməsi

Yerli sistem hesabı **NT AUTHORITY\\SYSTEM** Windows əməliyyat sistemlərində daxili bir hesabdır. O, ƏS-də ən yüksək səviyyəli girişə malikdir və əksər Windows xidmətlərini işlətmək üçün istifadə olunur. Üçüncü tərəf xidmətlərinin də standart olaraq bu hesab kontekstində işləməsi çox yayğındır. Domenə qoşulmuş bir hostda bir **SYSTEM** hesabı, mahiyyətcə başqa bir istifadəçi hesabı növü olan kompüter hesabını təqlid edərək Active Directory-ni təftiş edə biləcək. Domen mühitində **SYSTEM** səviyyəli girişə malik olmaq, demək olar ki, bir domen istifadəçi hesabına sahib olmaqla bərabərdir.

Bir hostda **SYSTEM** səviyyəli giriş əldə etməyin bir neçə yolu var, bunlara daxildir, lakin məhdudlaşmır:

  * **MS08-067, EternalBlue** və ya **BlueKeep** kimi uzaq Windows istismarları.
  * **SYSTEM** hesabı kontekstində işləyən bir xidməti sui-istifadə etmək və ya **Juicy Potato** istifadə edərək xidmət hesabının `SeImpersonate` imtiyazlarını sui-istifadə etmək. Bu tip hücum köhnə Windows ƏS-lərdə mümkündür, lakin Windows Server 2019-da həmişə mümkün olmur.
  * Windows əməliyyat sistemlərində yerli imtiyaz yüksəltmə qüsurları, məsələn, **Windows 10 Task Scheduler 0-day**.
  * Yerli hesabla domenə qoşulmuş bir hostda admin girişi əldə etmək və **Psexec** istifadə edərək bir **SYSTEM** `cmd` pəncərəsini başlatmaq.

Domenə qoşulmuş bir hostda **SYSTEM** səviyyəli giriş əldə etməklə siz aşağıdakı hərəkətləri yerinə yetirə biləcəksiniz, lakin bunlarla məhdudlaşmırsınız:

  * Daxili alətlər və ya **BloodHound** və **PowerView** kimi hücum alətlərindən istifadə edərək domendə təftiş aparmaq.
  * Eyni domendə **Kerberoasting / ASREPRoasting** hücumlarını həyata keçirmək.
  * **Net-NTLMv2 hash**-larını toplamaq və ya **SMB relay** hücumlarını həyata keçirmək üçün **Inveigh** kimi alətləri işə salmaq.
  * İmtiyazlı bir domen istifadəçi hesabını qaçırmaq üçün **token impersonation** (token təqlidi) həyata keçirmək.
  * **ACL** hücumlarını həyata keçirmək.

#### Xəbərdarlıq

Bir alət seçərkən testin əhatə dairəsini və üslubunu yadda saxlayın. Əgər siz **qaçınmayan** bir sızma testi aparırsınızsa, hər şey açıq şəkildə və müştərinin heyəti sizin orada olduğunuzu bilirsə, nə qədər səs-küy çıxarmağınızın adətən fərqi yoxdur. Lakin, **qaçınan** bir sızma testi, **adversarial qiymətləndirmə** və ya **red team** tapşırığı zamanı siz potensial bir hücumçunun Alətlərini, Taktikalarını və Prosedurlarını təqlid etməyə çalışırsınız. Bunu nəzərə alaraq, **gizlilik** narahatlıq doğurur. Bütün bir şəbəkəyə **Nmap** atmaq tam olaraq sakit deyil və bir sızma testində ümumiyyətlə istifadə etdiyimiz bir çox alət savadlı və hazırlıqlı bir SOC və ya Blue Teamer üçün həyəcan siqnallarını işə salacaq. Həmişə qiymətləndirmənizin məqsədini başlamazdan əvvəl yazılı şəkildə müştəri ilə dəqiqləşdirdiyinizdən əmin olun.

-----

### Gəlin Bir İstifadəçi Tapaq

Növbəti bir neçə bölmədə, **LLMNR/NBT-NS Zəhərlənməsi** və **şifrə spray** kimi texnikalardan istifadə edərək bir domen istifadəçi hesabı axtaracağıq. Bu hücumlar möhkəmlənmək üçün əla yollardır, lakin ehtiyatla və alətlərin və texnikaların anlaşılması ilə həyata keçirilməlidir. İndi qiymətləndirməmizin növbəti mərhələsinə keçə bilmək və domenin bütün mümkün səhvlərini və qüsurlarını tapmaq üçün bir istifadəçi hesabı axtaraq.
