## Simsiz Şəbəkələr (Wireless Networks)

Simsiz şəbəkələr şəbəkə qovşaqları arasında **simsiz məlumat bağlantılarından** istifadə edən kompüter şəbəkələridir. Bu şəbəkələr noutbuklar, smartfonlar və planşetlər kimi cihazlara **fiziki əlaqələrə** (kablolara) ehtiyac olmadan bir-biri ilə və İnternetlə əlaqə qurmağa imkan verir.

Simsiz şəbəkələr cihazlar arasında məlumat ötürmək üçün **radio tezliyi (RF) texnologiyasından** istifadə edir. Simsiz şəbəkədəki hər bir cihaz məlumatları **RF siqnallarına** çevirən və onları hava üzərindən göndərən bir **simsiz adapterə** malikdir. Şəbəkədəki digər cihazlar bu siqnalları öz simsiz adapterləri ilə qəbul edir və məlumatlar sonra yenidən istifadə edilə bilən formaya çevrilir. Onlar istifadə olunan texnologiyadan asılı olaraq müxtəlif diapazonlarda işləyə bilər. Məsələn, ev və ya kiçik ofis kimi kiçik bir sahəni əhatə edən **yerli şəbəkə (LAN)**, bir neçə yüz fut məsafəyə malik olan **WiFi** adlı simsiz texnologiyadan istifadə edə bilər. Digər tərəfdən, **simsiz geniş ərazi şəbəkəsi (WWAN)** isə hüceyrəvi məlumatlar ($3\text{G}, 4\text{G LTE}, 5\text{G}$) kimi mobil telekommunikasiya texnologiyasından istifadə edə bilər ki, bu da bütün bir şəhər və ya region kimi daha böyük bir ərazini əhatə edə bilər.

Buna görə də, simsiz şəbəkəyə qoşulmaq üçün bir cihaz şəbəkənin diapazonunda olmalı və şəbəkə adı və şifrəsi kimi **düzgün şəbəkə parametrləri** ilə konfiqurasiya edilməlidir. Qoşulduqdan sonra cihazlar bir-biri ilə və İnternetlə əlaqə qura bilər ki, bu da istifadəçilərə onlayn resurslara daxil olmağa və məlumat mübadiləsi aparmağa imkan verir.

Cihazlar arasında rabitə bir **WiFi** şəbəkəsində **$2.4\text{ GHz}$** və ya **$5\text{ GHz}$** diapazonlarında RF üzərindən baş verir. Bir noutbuk kimi bir cihaz şəbəkə üzərindən məlumat göndərmək istədikdə, əvvəlcə ötürmə icazəsi tələb etmək üçün **Simsiz Giriş Nöqtəsi (WAP)** ilə əlaqə qurur. **WAP** simsiz şəbəkəni **simli şəbəkəyə** birləşdirən və şəbəkəyə girişi idarə edən router kimi mərkəzi bir cihazdır. WAP icazə verdikdən sonra, ötürücü cihaz məlumatları **RF siqnalları** kimi göndərir ki, bunlar da şəbəkədəki digər cihazların simsiz adapterləri tərəfindən qəbul edilir. Məlumatlar sonra yenidən istifadə edilə bilən formaya çevrilir və müvafiq tətbiqə və ya sistemə ötürülür.

RF siqnalının gücünə və onun gedə biləcəyi məsafəyə ötürücünün gücü, maneələrin mövcudluğu və mühitdəki RF səs-küyünün sıxlığı kimi amillər təsir edir. Beləliklə, etibarlı rabitəni təmin etmək üçün **WiFi** şəbəkələri bu çətinlikləri aradan qaldırmaq üçün *yayılmış spektr ötürülməsi* və *səhv korreksiyası* kimi texnikalardan istifadə edir.

---

## Wi-Fi Əlaqəsi

Cihaz həmçinin **şəbəkə adı** / **Service Set Identifier (SSID)** və **şifrə** kimi düzgün şəbəkə parametrləri ilə konfiqurasiya edilməlidir. Buna görə də, routerə qoşulmaq üçün noutbuk **IEEE 802.11** adlı simsiz şəbəkə protokolundan istifadə edir. Bu protokol simsiz cihazların bir-biri ilə və **WAP**-larla necə əlaqə qurmasının texniki təfərrüatlarını müəyyən edir. Bir cihaz bir **WiFi** şəbəkəsinə qoşulmaq istədikdə, əlaqə prosesini başlamaq üçün WAP-a bir sorğu göndərir. Bu sorğu **əlaqə sorğusu freymi** (*connection request frame*) və ya **assosiasiya sorğusu** (*association request*) kimi tanınır və **IEEE 802.11** simsiz şəbəkə protokolu istifadə edilərək göndərilir. Əlaqə sorğusu freymi aşağıdakıları daxil edən, lakin bunlarla məhdudlaşmayan müxtəlif məlumat sahələrini ehtiva edir:

| Sahə | Təsvir |
| :--- | :--- |
| **MAC ünvanı** | Cihazın simsiz adapteri üçün **unikal identifikator**. |
| **SSID** | Şəbəkə adı, həmçinin **WiFi** şəbəkəsinin **Service Set Identifier**-ı kimi tanınır. |
| **Dəstəklənən məlumat sürətləri** | Cihazın əlaqə qura biləcəyi məlumat sürətlərinin siyahısı. |
| **Dəstəklənən kanallar** | Cihazın əlaqə qura biləcəyi **kanalların (tezliklərin)** siyahısı. |
| **Dəstəklənən təhlükəsizlik protokolları** | Cihazın istifadə edə biləcəyi **WPA2/WPA3** kimi təhlükəsizlik protokollarının siyahısı. |

Cihaz daha sonra bu məlumatdan istifadə edərək simsiz adapterini konfiqurasiya edir və **WAP**-a qoşulur. Əlaqə qurulduqdan sonra, cihaz **WAP** və digər şəbəkə cihazları ilə əlaqə qura bilər. O, həmçinin simli şəbəkəyə bir şlüz (*gateway*) kimi çıxış edən **WAP** vasitəsilə İnternetə və digər onlayn resurslara daxil ola bilər. Lakin, **SSID** yayımlanmasını (*broadcasting*) deaktiv etməklə **gizlədilə** bilər. Bu o deməkdir ki, həmin xüsusi **WAP**-ı axtaran cihazlar onun **SSID**-ni müəyyən edə bilməyəcəklər. Buna baxmayaraq, **SSID** hələ də autentifikasiya paketində tapıla bilər.

**IEEE 802.11** protokolu ilə yanaşı, **WiFi** şəbəkəsində cihazlara IP ünvanlarını təyin etmək, cihazlar arasında trafiki marşrutlaşdırmaq və təhlükəsizlik təmin etmək kimi vəzifələri yerinə yetirmək üçün **TCP/IP, DHCP və WPA2** kimi digər şəbəkə protokolları və texnologiyaları da istifadə edilə bilər.

### WEP Challenge-Response Əl Sıxması

*Challenge-response* əl sıxması (*handshake*) **WEP** təhlükəsizlik protokolundan istifadə edən simsiz şəbəkədə bir **WAP** və klient cihazı arasında təhlükəsiz əlaqə qurmaq üçün bir prosesdir. Bu, cihazı autentifikasiya etmək və təhlükəsiz əlaqə qurmaq üçün **WAP** və klient cihazı arasında paket mübadiləsini əhatə edir.

| Addım | Kim | Təsvir |
| :--- | :--- | :--- |
| **1** | **Klient** | Giriş tələb edərək **WAP**-a bir **assosiasiya sorğusu** paketi göndərir. |
| **2** | **WAP** | Klientə bir **assosiasiya cavabı** paketi ilə cavab verir ki, bu da bir **challenge string** (*sınaq sətiri*) ehtiva edir. |
| **3** | **Klient** | *Challenge string*-ə bir cavab və **paylaşılan gizli açar** hesablayır və onu **WAP**-a geri göndərir. |
| **4** | **WAP** | Eyni **paylaşılan gizli açar** ilə *challenge*-ə gözlənilən cavabı hesablayır və klientə bir **autentifikasiya cavabı** paketi göndərir. |

Buna baxmayaraq, bəzi paketlər itə bilər, buna görə də sözdə **CRC** *checksum* inteqrasiya edilmişdir. **Cyclic Redundancy Check (CRC)**, simsiz rabitədə məlumatların korrupsiyasına qarşı qorunmaq üçün **WEP** protokolunda istifadə olunan bir **səhv aşkarlama mexanizmidir**. **CRC** dəyəri paketin məlumatlarına əsaslanaraq simsiz şəbəkə üzərindən ötürülən hər bir paket üçün hesablanır. O, məlumatların bütövlüyünü yoxlamaq üçün istifadə olunur. Təyinat cihazı paketi qəbul etdikdə, **CRC** dəyəri yenidən hesablanır və orijinal dəyərlə müqayisə edilir. Əgər dəyərlər uyğun gəlirsə, məlumatlar heç bir səhvsiz uğurla ötürülmüşdür. Lakin, əgər dəyərlər uyğun gəlmirsə, məlumatlar korrupsiyaya uğramışdır və yenidən ötürülməlidir.

**CRC** mexanizminin dizaynında bir qüsur var ki, bu da **şifrələmə açarını bilmədən** tək bir paketi **deşifrə etməyimizə** imkan verir. Bunun səbəbi, **CRC** dəyərinin şifrələnmiş məlumatlar əvəzinə paketdəki **açıq mətn** (*plaintext*) məlumatı istifadə edilərək hesablanmasıdır. **WEP**-də **CRC** dəyəri şifrələnmiş məlumatla birlikdə paketin başlığına daxil edilir. Təyinat cihazı paketi qəbul etdikdə, məlumatların heç bir səhvsiz uğurla ötürülməsini təmin etmək üçün **CRC** dəyəri yenidən hesablanır və orijinal dəyərlə müqayisə edilir. Lakin, biz məlumat şifrələnmiş olsa belə, paketdəki **açıq mətn** məlumatını müəyyən etmək üçün **CRC**-dən istifadə edə bilərik.

---

## Təhlükəsizlik Xüsusiyyətləri

**WiFi** şəbəkələri icazəsiz girişdən qorunmaq və şəbəkə üzərindən ötürülən məlumatların məxfiliyini və bütövlüyünü təmin etmək üçün bir neçə təhlükəsizlik xüsusiyyətinə malikdir. Əsas təhlükəsizlik xüsusiyyətlərindən bəziləri aşağıdakıları əhatə edir, lakin bunlarla məhdudlaşmır:

* **Şifrələmə** (*Encryption*)
* **Girişə Nəzarət** (*Access Control*)
* **Firewall**

### Şifrələmə

Simsiz şəbəkələr üzərindən ötürülən məlumatların məxfiliyini qorumaq üçün müxtəlif **şifrələmə alqoritmlərindən** istifadə edə bilərik. **WiFi** şəbəkələrində ən çox yayılmış şifrələmə alqoritmləri **Wired Equivalent Privacy (WEP)**, **WiFi Protected Access 2 (WPA2)** və **WiFi Protected Access 3 (WPA3)**-dir.

### Girişə Nəzarət

**WiFi** şəbəkələri defolt olaraq müəyyən **autentifikasiya metodlarından** istifadə edərək səlahiyyətli cihazların şəbəkəyə qoşulmasına icazə vermək üçün konfiqurasiya edilir. Lakin, bu metodlar səlahiyyətli cihazları müəyyən etmək üçün **şifrə** və ya **unikal identifikator (MAC ünvanı)** tələb etməklə dəyişdirilə bilər.

### Firewall

**Firewall** əvvəlcədən müəyyən edilmiş təhlükəsizlik qaydalarına əsaslanaraq daxil olan və çıxan şəbəkə trafikini idarə edən bir təhlükəsizlik sistemidir. Məsələn, **WiFi** routerlərində tez-tez **İnternetdən daxil olan trafiki blok edə bilən** və müxtəlif növ *kibermüdafiələrə* qarşı qoruya bilən daxili *firewall*-lar olur.

---

## Şifrələmə Protokolları

**Wired Equivalent Privacy (WEP)** və **WiFi Protected Access (WPA)** bir **WiFi** şəbəkəsi üzərindən ötürülən məlumatları qoruyan şifrələmə protokollarıdır. **WPA** **Advanced Encryption Standard (AES)** daxil olmaqla fərqli şifrələmə alqoritmlərindən istifadə edə bilər.

### WEP

**WEP** məlumatları şifrələmək üçün **$40$-bit** və ya **$104$-bitlik** açar istifadə edir, halbuki **AES istifadə edən WPA $128$-bitlik** açar istifadə edir. **Daha uzun açarlar** daha möhkəm şifrələmə təmin edir və hücumlara daha davamlıdır. Lakin, o, bir hücumçunun şəbəkə üzərindən ötürülən məlumatları deşifrə etməsinə imkan verə bilən müxtəlif hücumlara qarşı **həssasdır**. Bundan əlavə, **WEP** daha yeni cihazlar və əməliyyat sistemləri ilə **uyğun deyil** və ümumiyyətlə artıq təhlükəsiz hesab edilmir. Nəhayət, **WEP RC4 şifrələmə alqoritmini** istifadə edir ki, bu da onu hücumlara qarşı **həssas** edir.

Lakin, **WEP** autentifikasiya üçün **paylaşılan açar** (*shared key*) istifadə edir, yəni eyni açar şifrələmə və autentifikasiya üçün istifadə olunur. **WEP** protokolunun iki versiyası var:

* **WEP-40 / WEP-64**
* **WEP-104**

**WEP-40**, həmçinin **WEP-64** kimi tanınır, **$40$-bitlik** (gizli) açar istifadə edir, halbuki **WEP-104 $104$-bitlik** açar istifadə edir. Açar **İlkinləşdirmə Vektoru (IV)** və **gizli açara** bölünür.

**IV** şifrələnmiş məlumatla birlikdə paketin başlığına daxil edilən kiçik bir dəyərdir və həm **WEP-40**, həm də **WEP-104** üçün açarı yaratmaq üçün istifadə olunur və hər bir açarın **unikal** olmasını təmin etmək üçün daxil edilir. Gizli açar məlumatları şifrələmək üçün istifadə olunan bir sıra **təsadüfi bitlərdir**. Lakin, **WEP-104** **$80$-bit** uzunluğunda gizli açara malikdir. Fərqləri aydın görmək üçün aşağıdakı cədvələ baxaq:

| Protokol | IV | Gizli Açar |
| :--- | :--- | :--- |
| **WEP-40/WEP-64** | $24$-bit | $40$-bit |
| **WEP-104** | $24$-bit | $80$-bit |

Lakin, **WEP**-də **IV** nisbətən kiçik olduğundan, biz onu *kobud qüvvə* (*brute force*) ilə sınaqdan keçirə bilərik, onun üçün mümkün olan hər bir simvol birləşməsini yoxlaya və düzgün dəyəri müəyyən edə bilərik. Bundan sonra, biz onu paketdəki məlumatları deşifrə etmək üçün istifadə edə bilərik. Bu, bizə simsiz şəbəkə üzərindən ötürülən məlumatlara daxil olmağa və potensial olaraq şəbəkənin təhlükəsizliyini pozmağa imkan verir.

### WPA

**WPA** **ən yüksək təhlükəsizlik səviyyəsini** təmin edir və **WEP** kimi eyni növ hücumlara qarşı həssas deyil. Bundan əlavə, **WPA** **Pre-Shared Key (PSK)** və ya $802.1\text{X}$ autentifikasiya serveri kimi daha təhlükəsiz autentifikasiya metodlarından istifadə edir ki, bu da icazəsiz girişə qarşı daha güclü qorunma təmin edir. Baxmayaraq ki, daha köhnə cihazlar **WPA**-nı dəstəkləməyə bilər, o, əksər cihazlar və əməliyyat sistemləri ilə uyğundur. Bütün simsiz şəbəkələr, xüsusilə ofislər kimi **kritik infrastrukturda**, ümumiyyətlə **ən azı WPA2** və ya hətta **WPA3** şifrələməsini tətbiq etməlidir.

---

## Autentifikasiya Protokolları

**Lightweight Extensible Authentication Protocol (LEAP)** və **Protected Extensible Authentication Protocol (PEAP)** simsiz şəbəkələri qorumaq üçün istifadə olunan autentifikasiya protokollarıdır. Onlar simsiz şəbəkədə cihazları autentifikasiya etmək üçün təhlükəsiz bir metod təmin edir və tez-tez əlavə təhlükəsizlik qatı təmin etmək üçün **WEP** və ya **WPA** ilə birlikdə istifadə olunur.

**LEAP** və **PEAP** hər ikisi müxtəlif şəbəkə kontekstlərində istifadə olunan autentifikasiya çərçivəsi olan **Extensible Authentication Protocol (EAP)**-a əsaslanır. Lakin, **LEAP** və **PEAP** arasındakı əsas fərqlərdən biri autentifikasiya prosesini necə qorumalarıdır.

* **LEAP** autentifikasiya üçün **paylaşılan açar** (*shared key*) istifadə edir, yəni **eyni açar** **şifrələmə və autentifikasiya** üçün istifadə olunur.
    * Açar təhlükə altına düşərsə, bu, şəbəkəyə giriş əldə etməyimiz üçün nisbətən asan ola bilər.

* Lakin, **PEAP tunnellənmiş Transport Layer Security (TLS)** adlı daha təhlükəsiz bir autentifikasiya metodu istifadə edir. Bu metod bir **rəqəmsal sertifikat** istifadə edərək cihaz və **WAP** arasında təhlükəsiz bir əlaqə qurur və **şifrələnmiş tunel** autentifikasiya prosesini qoruyur. Bu, icazəsiz girişə qarşı daha möhkəm qorunma təmin edir və hücumlara daha davamlıdır.

### TACACS+

Bir simsiz şəbəkədə, bir **simsiz giriş nöqtəsi (WAP)** **Terminal Access Controller Access-Control System Plus (TACACS+)** serverinə bir autentifikasiya sorğusu göndərdikdə, sorğunun məxfiliyini və bütövlüyünü qorumaq üçün ehtimal ki, **bütün sorğu paketi şifrələnəcəkdir**.

**TACACS+** routerlər və kommutatorlar kimi şəbəkə cihazlarına daxil olan istifadəçiləri autentifikasiya etmək və avtorizasiya etmək üçün istifadə olunan bir protokoldur. Bir **WAP TACACS+** serverinə bir autentifikasiya sorğusu göndərdikdə, sorğu adətən istifadəçinin etibarnamələrini və sessiya haqqında digər məlumatları ehtiva edir.

Autentifikasiya sorğusunun şifrələnməsi, bu həssas məlumatın sorğuyu ələ keçirə biləcək icazəsiz tərəflər üçün görünmədiyini təmin etməyə kömək edir. Eyni zamanda, sorğunun dəyişdirilməsinin və ya onu özlərinin zərərli bir sorğusu ilə əvəz edilməsinin qarşısını almağa kömək edir.

Autentifikasiya sorğusunu şifrələmək üçün **SSL/TLS** və ya **IPSec** kimi bir neçə şifrələmə metodu istifadə edilə bilər. İstifadə olunan xüsusi şifrələmə metodu **TACACS+** serverinin konfiqurasiyasından və **WAP**-ın imkanlarından asılı ola bilər.

### Disassosiasiya Hücumu

**Disassosiasiya Hücumu** bir **WAP** və onun klientləri arasındakı rabitəni pozmağı hədəfləyən, bir və ya daha çox klientə **disassosiasiya freymləri** (*disassociation frames*) göndərən bir **simsiz şəbəkə hücumunun** bir növüdür.

**WAP** bir klientin şəbəkədən ayrılması üçün disassosiasiya freymlərindən istifadə edir. Bir **WAP** bir klientə disassosiasiya freymi göndərdikdə, klient şəbəkədən ayrılacaq və şəbəkədən istifadə etməyə davam etmək üçün yenidən qoşulmalı olacaq.

Biz hücumu yerləşdiyimiz yerdən və şəbəkə təhlükəsizliyi tədbirlərindən asılı olaraq şəbəkənin **daxilindən** və ya **xaricindən** başlata bilərik. Bu hücumun məqsədi **WAP** və onun klientləri arasındakı rabitəni pozmaqdır ki, bu da klientlərin ayrılmasına və istifadəçilərə narahatlıq və ya pozuntuya səbəb ola bilər. Biz onu həm də **MITM** (*Man-in-the-Middle*) hücumu kimi digər hücumlara başlanğıc kimi istifadə edə bilərik, klientləri şəbəkəyə yenidən qoşulmağa məcbur edərək və potensial olaraq onları daha çox hücumlara məruz qoyaraq.

---

## Simsiz Təhlükəsizliyin Gücləndirilməsi (Wireless Hardening)

Simsiz şəbəkələri qorumaq üçün bir çox fərqli yollar var. Lakin, simsiz şəbəkələrin təhlükəsizliyini **dramatik şəkildə artırmaq** üçün nəzərə alınmalı olan bəzi nümunələr var. Bunlar aşağıdakılardır, lakin bunlarla məhdudlaşmır:

* **Yayımlanmanı deaktiv etmək** (*Disabling broadcasting*)
* **WiFi Protected Access (WPA)**
* **MAC filtrləməsi** (*MAC filtering*)
* **EAP-TLS tətbiq etmək** (*Deploying EAP-TLS*)

### Yayımlanmanı Deaktiv Etmək

**SSID**-nin yayımlanmasını deaktiv etmək, şəbəkəni aşkar etməyi və ona qoşulmağı çətinləşdirməklə bir **WAP**-ı gücləndirməyə kömək edə bilən bir təhlükəsizlik tədbiridir. **SSID** yayımlandıqda, şəbəkənin mövcudluğunu reklam etmək üçün **WAP** tərəfindən müntəzəm olaraq ötürülən *beacon* freymlərinə daxil edilir. **SSID**-nin yayımlanmasını deaktiv etməklə, **WAP** *beacon* freymlərini ötürməyəcək və şəbəkə şəbəkəyə hələ qoşulmamış cihazlar üçün **görünməyəcək**.

### WPA

Yenə də, **WPA** simsiz rabitə üçün **güclü şifrələmə və autentifikasiya** təmin edir, icazəsiz şəbəkə girişinə və həssas məlumatların ələ keçirilməsinə qarşı qorunmağa kömək edir. **WPA**-nın iki əsas versiyası var:

* **WPA-Personal** (Ev və kiçik biznes şəbəkələri üçün nəzərdə tutulub)
* **WPA-Enterprise** (Daha böyük təşkilatlar üçün nəzərdə tutulub və klientlərin şəxsiyyətini yoxlamaq üçün **mərkəzləşdirilmiş autentifikasiya serverindən (məsələn, RADIUS və ya TACACS+)** istifadə edir)

### MAC Filtrləməsi

**MAC filtrləməsi** bir **WAP**-a **MAC ünvanlarına** əsaslanaraq xüsusi cihazlardan gələn əlaqələri qəbul etməyə və ya rədd etməyə imkan verən bir təhlükəsizlik tədbiridir. **WAP**-ı yalnız **təsdiqlənmiş MAC ünvanlarına** malik cihazlardan gələn əlaqələri qəbul etmək üçün konfiqurasiya etməklə, icazəsiz cihazların şəbəkəyə qoşulmasının qarşısını almaq mümkündür.

### EAP-TLS Tətbiq Etmək

**EAP-TLS** simsiz rabitələri autentifikasiya etmək və şifrələmək üçün istifadə olunan bir təhlükəsizlik protokoludur. O, klientlərin şəxsiyyətini yoxlamaq və təhlükəsiz əlaqələr qurmaq üçün **rəqəmsal sertifikatlardan** və **PKI**-dən istifadə edir. **EAP-TLS**-i tətbiq etmək, simsiz rabitələr üçün **güclü autentifikasiya və şifrələmə** təmin etməklə, **WAP**-ı gücləndirməyə kömək edə bilər ki, bu da şəbəkəyə icazəsiz girişdən və həssas məlumatların ələ keçirilməsindən qoruya bilər.

