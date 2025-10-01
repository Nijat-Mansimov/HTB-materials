## Açar Mübadiləsi Mexanizmləri

**Açar mübadiləsi metodları** iki tərəf arasında **kriptoqrafik açarları təhlükəsiz şəkildə mübadilə etmək** üçün istifadə olunur. Bu, bir çox kriptoqrafik protokolların vacib bir hissəsidir, çünki rabitəni qorumaq üçün istifadə olunan şifrələmənin təhlükəsizliyi **açarların gizliliyinə** əsaslanır. Hər birinin özünəməxsus xüsusiyyətləri və güclü tərəfləri olan **bir çox açar mübadiləsi metodu** mövcuddur. Bəzi açar mübadiləsi metodları digərlərindən daha təhlükəsizdir və uyğun metod vəziyyətin xüsusi şərtlərindən və tələblərindən asılıdır.

Bu metodlar adətən iki tərəfin onların arasındakı rabitəni şifrələyən **paylaşılan gizli açar** üzərində **qeyri-təhlükəsiz rabitə kanalı** vasitəsilə razılığa gəlməsinə imkan verməklə işləyir. Bu, adətən, **riyazi funksiyanın xüsusiyyətlərinə əsaslanan hesablama** və ya **açarın sadə manipulyasiyası seriyası** kimi bir növ **riyazi əməliyyat** istifadə edilərək həyata keçirilir.

### Diffie-Hellman

Ümumi açar mübadiləsi metodlarından biri, iki tərəfin **əvvəlki rabitə və ya paylaşılan xüsusi məlumat olmadan paylaşılan gizli açar üzərində razılığa gəlməsinə** imkan verən **Diffie-Hellman açar mübadiləsidir**. O, iki tərəfin aralarında mesajları şifrələmək və deşifrə etmək üçün istifadə edilə bilən **paylaşılan gizli açar yaratması** konsepsiyasına əsaslanır. O, tez-tez **veb trafikini qorumaq üçün istifadə olunan Transport Layer Security (TLS) protokolu** kimi **təhlükəsiz rabitə kanallarının qurulması** üçün əsas kimi istifadə olunur.

**Diffie-Hellman** açar mübadiləsinin əsas məhdudiyyətlərindən biri onun **Ortada Adam (MITM) hücumlarına** qarşı həssas olmasıdır. **MITM hücumunda**, rabitəni ələ keçirir, tərəflərdən biri olaraq özümüzü göstərir, fərqli bir gizli açar yaradır və hər iki tərəfi ondan istifadə etməyə məcbur edirik. Bu, hücum edənə tərəflər arasında göndərilən **mesajları oxumağa və dəyişdirməyə** imkan verir.

Digər bir məhdudiyyət isə, paylaşılan gizli açarı yaratmaq üçün **nisbətən böyük miqdarda CPU gücü** tələb etməsidir. Bu, onu **aşağı güclü cihazlarda** və ya açarın tez bir zamanda yaradılması lazım olduğu vəziyyətlərdə **qeyri-praktik** edə bilər.

### RSA (Rivest–Shamir–Adleman)

Digər bir açar mübadiləsi metodu, paylaşılan gizli açar yaratmaq üçün **böyük sadə ədədlərin xüsusiyyətlərindən** istifadə edən **Rivest–Shamir–Adleman (RSA) alqoritmidir**. Bu metod **böyük sadə ədədləri bir-birinə vurmağın nisbətən asan, lakin nəticə ədədi yenidən sadə vuruqlarına ayırmağın çətin** olması faktına əsaslanır. Bu ikisindən başqa, baxmalı olduğumuz başqaları da var. O, həmçinin təhlükəsiz rabitə və məlumatların qorunmasını tələb edən bir çox digər tətbiqlərdə və protokollarda geniş istifadə olunur, o cümlədən, lakin bunlarla məhdudlaşmayaraq:

* **Məxfiliyi və autentifikasiyanı** təmin etmək üçün mesajları şifrələmək və imzalamaq
* **Secure Socket Layer (SSL)** və **TLS** protokolları kimi **şəbəkələr üzərində ötürülən məlumatları** qorumaq
* **Elektron sənədlər və digər rəqəmsal məlumatlar** üçün həqiqiliyi və bütövlüyü təmin etmək üçün istifadə olunan **rəqəmsal imzaları** yaratmaq və yoxlamaq
* **Kerberos şəbəkə autentifikasiya sistemi** tərəfindən istifadə olunan **Public Key Cryptography for Initial Authentication in Kerberos (PKINIT)** protokolu kimi **istifadəçiləri və cihazları autentifikasiya etmək**
* **Şəxsi məlumatların və məxfi sənədlərin şifrələnməsi** kimi **həssas məlumatları** qorumaq

### ECDH (Elliptic Curve Diffie-Hellman)

**Elliptic Curve Diffie-Hellman (ECDH)**, paylaşılan gizli açar yaratmaq üçün **Elliptik Əyri Kriptoqrafiyasından** istifadə edən **Diffie-Hellman** açar mübadiləsinin bir variantıdır. O, **ənənəvi Diffie-Hellman alqoritmindən daha səmərəli və təhlükəsiz** olmaq üstünlüyünə malikdir, o cümlədən, lakin bunlarla məhdudlaşmayaraq:

* **TLS protokolu** kimi **təhlükəsiz rabitə kanallarının qurulması**
* **Xüsusi açarlar təhlükə altına düşsə belə, keçmiş rabitələrin aşkar edilə bilməməsini** təmin edən **irəli məxfiliyin** (*forward secrecy*) təmin edilməsi
* **VPN-lərdə istifadə olunan İnternet Açar Mübadiləsi (IKE) protokolu** kimi **istifadəçilərin və cihazların autentifikasiyası**

### ECDSA (Elliptic Curve Digital Signature Algorithm)

**Elliptic Curve Digital Signature Algorithm (ECDSA)**, açar mübadiləsində iştirak edən tərəfləri autentifikasiya edə bilən **rəqəmsal imzalar** yaratmaq üçün **Elliptik Əyri Kriptoqrafiyasından** istifadə edir.

Gəlin bu alqoritmləri ümumiləşdirək və müqayisə edək:

| Alqoritm | Qısaltma | Təhlükəsizlik |
| :--- | :--- | :--- |
| **Diffie-Hellman** | DH | Nisbətən təhlükəsiz və hesablama baxımından səmərəli |
| **Rivest–Shamir–Adleman** | RSA | Geniş istifadə olunur və təhlükəsiz hesab edilir, lakin hesablama baxımından intensivdir |
| **Elliptic Curve Diffie-Hellman** | ECDH | Ənənəvi Diffie-Hellman ilə müqayisədə **təkmilləşdirilmiş təhlükəsizlik** təmin edir |
| **Elliptic Curve Digital Signature Algorithm** | ECDSA | **Rəqəmsal imza** yaradılması üçün təkmilləşdirilmiş **təhlükəsizlik və səmərəlilik** təmin edir |

---

## İnternet Açar Mübadiləsi (IKE)

**İnternet Açar Mübadiləsi (IKE)** **VPN-lərdə** istifadə olunanlar kimi **təhlükəsiz rabitə seanslarını qurmaq və saxlamaq** üçün istifadə olunan bir protokoldur. O, açarları təhlükəsiz şəkildə mübadilə etmək və təhlükəsizlik parametrlərini razılaşdırmaq üçün **Diffie-Hellman açar mübadiləsi alqoritminin** və **digər kriptoqrafik texnikaların** birləşməsindən istifadə edir. Bundan əlavə, o, **bir çox VPN həllinin əsas komponentidir**, çünki **VPN klienti və serveri arasında açarların və digər təhlükəsizlik məlumatlarının təhlükəsiz mübadiləsinə** imkan verir. Bu, VPN-ə məlumatların təhlükəsiz şəkildə ötürülə biləcəyi **şifrələnmiş tunel** qurmağa imkan verir.

**IKE** həmçinin **istifadəçilərin və cihazların autentifikasiyası** kimi digər məqsədlər üçün də istifadə edilə bilər. O, adətən **açar mübadiləsi və rəqəmsal imzalar üçün RSA alqoritmi** və **məlumat şifrələməsi üçün Advanced Encryption Standard (AES)** kimi digər protokollar və alqoritmlərlə birlikdə istifadə olunur.

**IKE** ya **əsas rejimdə (main mode)**, ya da **aqressiv rejimdə (aggressive mode)** işləyir. Bu rejimlər açar mübadiləsi prosesinin ardıcıllığını və parametrlərini müəyyən edir və **IKE** seansının təhlükəsizliyinə və performansına təsir göstərə bilər.

### Əsas Rejim (Main Mode)

**Əsas Rejim (Main Mode)** **IKE** üçün **defolt rejimdir** və ümumiyyətlə **aqressiv rejimdən daha təhlükəsiz** hesab olunur. Əsas rejimdə açar mübadiləsi prosesi **üç fazada** həyata keçirilir, hər birində fərqli bir təhlükəsizlik parametrləri və açarlar mübadilə edilir. Bu, **daha çox çeviklik və təhlükəsizlik** təmin edir, lakin aqressiv rejimə nisbətən **daha yavaş performans** ilə nəticələnə bilər.

### Aqressiv Rejim (Aggressive Mode)

**Aqressiv Rejim (Aggressive Mode)**, açar mübadiləsi üçün tələb olunan **gediş-gəliş sayını və mesaj mübadiləsini azaltmaqla daha sürətli performans** təmin edən **IKE** üçün alternativ bir rejimdir. Bu rejimdə, açar mübadiləsi prosesi **iki fazada** həyata keçirilir, bütün təhlükəsizlik parametrləri və açarlar birinci fazada mübadilə edilir. Lakin, bu, **daha sürətli performans** təmin edə bilər, lakin **agressiv rejim identifikasiya qorumasını təmin etmədiyi** üçün əsas rejimə nisbətən **IKE** seansının təhlükəsizliyini azalda bilər.

### Əvvəlcədən Paylaşılan Açarlar (Pre-Shared Keys - PSK)

**IKE**-də **Əvvəlcədən Paylaşılan Açar (PSK)**, açar mübadiləsində iştirak edən iki tərəf arasında **paylaşılan gizli dəyərdir**. Bu açar tərəfləri **autentifikasiya etmək** və sonrakı rabitəni şifrələyən **paylaşılan gizli sirri** qurmaq üçün istifadə olunur. **PSK**-nın istifadəsi **IKE**-də **könüllüdür** və istifadə edilib-edilməməsi vəziyyətin xüsusi tələblərindən və məhdudiyyətlərindən asılıdır. Lakin, bir **PSK** istifadə edilərsə, açar mübadiləsi prosesi başlamazdan əvvəl **iki tərəf arasında təhlükəsiz şəkildə mübadilə edilməlidir**. Bu, **təhlükəsiz kənar kanal** (*out-of-band channel*), məsələn, ayrı bir rabitə kanalı vasitəsilə və ya açarı fiziki olaraq mübadilə etməklə edilə bilər.

**PSK** istifadə etməyin əsas üstünlüyü, tərəflərin bir-birini autentifikasiya etməsinə imkan verməklə **əlavə təhlükəsizlik qatı** təmin etməsidir. Lakin, **PSK** istifadə etməyin bəzi məhdudiyyətləri və çatışmazlıqları da var. Məsələn, **açarı təhlükəsiz şəkildə mübadilə etmək çətin ola bilər** və **əgər açar MITM hücumu vasitəsilə təhlükə altına düşərsə, IKE seansının təhlükəsizliyi pozula bilər**.

