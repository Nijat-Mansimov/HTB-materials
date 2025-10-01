## Şəbəkə Növləri

Hər bir şəbəkə fərqli şəkildə qurulur və fərdi şəkildə tənzimlənə bilər. Bu səbəbdən, bu şəbəkələri kateqoriyalara bölmək üçün istifadə edilə bilən **növlər** və **topologiyalar** inkişaf etdirilmişdir. Bütün şəbəkə növləri haqqında oxuyarkən, bəzi şəbəkə növləri coğrafi diapazonu əhatə etdiyi üçün bir az **məlumat yüklənməsi** (*information overload*) ola bilər. Təcrübədə bəzi terminologiyaları nadir hallarda eşidirik, buna görə də bu bölmə **Ümumi Terminlər** və **Kitab Terminləri** olaraq ayrılacaq. Kitab terminlərini bilmək yaxşıdır, çünki bir e-poçt serverinin 500 mildən uzun e-poçtları çatdıra bilməməsi ilə bağlı sənədləşdirilmiş tək bir hal olub, lakin bir şəbəkə imtahanına hazırlaşmırsınızsa, onları tələb üzrə söyləməyiniz gözlənilmir.

***

### Ümumi Terminologiya

| Şəbəkə Növü | Tərif |
| :--- | :--- |
| **Geniş Sahə Şəbəkəsi (WAN)** | İnternet |
| **Yerli Şəbəkə (LAN)** | Daxili Şəbəkələr (Məsələn: Ev və ya Ofis) |
| **Simsiz Yerli Şəbəkə (WLAN)** | Wi-Fi üzərindən əlçatan Daxili Şəbəkələr |
| **Virtual Şəxsi Şəbəkə (VPN)** | Birdən çox şəbəkə sahəsini bir **LAN**-a birləşdirir |

#### WAN

**WAN (Wide Area Network)** ümumiyyətlə **İnternet** adlanır. Şəbəkə avadanlıqları ilə işləyərkən, tez-tez bir **WAN Ünvanı** və bir **LAN Ünvanı** olur. WAN ünvanı, ümumiyyətlə, İnternet tərəfindən daxil olunan ünvandır. Bununla birlikdə, bu, İnternetlə məhdudlaşmır; WAN sadəcə bir yerə birləşdirilmiş çox sayda LAN-dır. Bir çox böyük şirkət və ya hökumət qurumu "Daxili WAN" (həmçinin **İntranet**, **Airgap Şəbəkəsi** və s. adlanır) sahib olacaq. Ümumiyyətlə, şəbəkənin WAN olub olmadığını müəyyənləşdirməyin əsas yolu **BGP** kimi **WAN-a Xas** bir marşrutlaşdırma protokolundan istifadə etmək və istifadə olunan IP sxeminin **RFC 1918** daxilində olub-olmamasını yoxlamaqdır ($10.0.0.0/8$, $172.16.0.0/12$, $192.168.0.0/16$).

#### LAN / WLAN

**LAN-lar (Local Area Network)** və **WLAN-lar (Wireless Local Area Network)** adətən yerli istifadə üçün nəzərdə tutulmuş **IP Ünvanları** təyin edəcək (**RFC 1918**, $10.0.0.0/8$, $172.16.0.0/12$, $192.168.0.0/16$). Bəzi hallarda, bəzi kolleclər və ya otellər kimi, LAN-larına qoşulmaqla sizə marşrutlaşdırıla bilən (internet) IP Ünvanı təyin edilə bilər, lakin bu daha az yaygındır. WLAN-ların kabelsiz məlumat ötürmə qabiliyyətini təqdim etməsi xaricində LAN və ya WLAN arasında heç bir fərq yoxdur. Bu, əsasən **təhlükəsizlik təyinatıdır**.

#### VPN

**Virtual Şəxsi Şəbəkələrin (VPN)** üç əsas növü var, lakin hər üçünün də eyni məqsədi var: istifadəçinin sanki fərqli bir şəbəkəyə qoşulmuş kimi hiss etməsini təmin etmək.

1.  **Site-To-Site VPN (Sahədən Sahəyə VPN)**: Həm müştəri, həm də server **Şəbəkə Cihazlarıdır**, tipik olaraq ya **Routerlər** ya da **Firewallar**, və tam şəbəkə diapazonlarını bölüşürlər. Bu, ən çox şirkət şəbəkələrini İnternet üzərindən birləşdirmək üçün istifadə olunur, bu da bir çox yerin sanki yerli imiş kimi İnternet üzərindən əlaqə qurmasına imkan verir.
2.  **Remote Access VPN (Uzaqdan Giriş VPN)**: Bu, müştəri kompüterinin sanki müştərinin şəbəkəsində imiş kimi davranan virtual interfeys yaratmasını əhatə edir. Hack The Box **OpenVPN**-dən istifadə edir, bu da bizə laboratoriyalara daxil olmağa imkan verən **TUN Adapteri** yaradır. Bu VPN-ləri təhlil edərkən nəzərə alınmalı vacib bir məqam, VPN-ə qoşulduqda yaranan **marşrutlaşdırma cədvəlidir** (*routing table*). Əgər VPN yalnız xüsusi şəbəkələr üçün marşrutlar yaradırsa (məsələn: $10.10.10.0/24$), buna **Split-Tunnel VPN** deyilir, yəni İnternet bağlantısı VPN-dən kənara çıxmır. Bu, İnternet bağlantınızın monitorinqi ilə bağlı məxfilik narahatlığı olmadan Lab-a daxil olmaq imkanı verdiyi üçün Hack The Box üçün əladır. Lakin, bir şirkət üçün **split-tunnel** VPN-lər adətən ideal deyil, çünki maşın zərərli proqramla yoluxmuşsa, şəbəkə əsaslı aşkarlama üsulları çox güman ki, işləməyəcək, çünki həmin trafik İnternetə çıxır.
3.  **SSL VPN**: Bu, mahiyyət etibarilə veb brauzerimiz daxilində edilən bir VPN-dir və veb brauzerlər hər şeyi edə bildiyi üçün getdikcə daha çox yayılır. Tipik olaraq, bunlar tətbiqləri və ya bütün masaüstü seanslarını veb brauzerinizə yayımlayacaq. Bunun əla nümunəsi HackTheBox **Pwnbox** olacaq.

***

### Kitab Terminologiyası

| Şəbəkə Növü | Tərif |
| :--- | :--- |
| **Qlobal Sahə Şəbəkəsi (GAN)** | Qlobal şəbəkə (İnternet) |
| **Metropolitan Sahə Şəbəkəsi (MAN)** | Regional şəbəkə (birdən çox LAN) |
| **Simsiz Fərdi Şəbəkə (WPAN)** | Fərdi şəbəkə (Bluetooth) |

#### GAN

**İnternet** kimi dünya miqyasında bir şəbəkə **Qlobal Sahə Şəbəkəsi (GAN)** kimi tanınır. Lakin, İnternet bu cür yeganə kompüter şəbəkəsi deyil. Beynəlxalq səviyyədə fəaliyyət göstərən şirkətlər də bir neçə **WAN**-ı əhatə edən və şirkət kompüterlərini dünya miqyasında birləşdirən **təcrid olunmuş şəbəkələrə** malikdirlər. GAN-lar geniş sahə şəbəkələrinin **şüşə lif infrastrukturundan** istifadə edir və onları beynəlxalq sualtı kabellər və ya peyk ötürülməsi ilə birləşdirir.

#### MAN

**Metropolitan Sahə Şəbəkəsi (MAN)** coğrafi yaxınlıqda bir neçə **LAN-ı** birləşdirən genişzolaqlı telekommunikasiya şəbəkəsidir. Bir qayda olaraq, bunlar kirayə xətləri vasitəsilə MAN-a qoşulan bir şirkətin fərdi filiallarıdır. Yüksək performanslı routerlər və şüşə liflərə əsaslanan yüksək performanslı bağlantılardan istifadə olunur ki, bu da İnternetdən əhəmiyyətli dərəcədə daha yüksək **məlumat ötürmə sürətinə** imkan verir. İki uzaq qovşaq arasındakı ötürmə sürəti bir **LAN** daxilindəki əlaqə ilə müqayisə edilə bilər.

Beynəlxalq fəaliyyət göstərən şəbəkə operatorları MAN-lar üçün infrastrukturu təmin edir. **Metropolitan Sahə Şəbəkələri** kimi kabelləşdirilmiş şəhərlər **Geniş Sahə Şəbəkələrinə (WAN)** supra-regional və **Qlobal Sahə Şəbəkələrinə (GAN)** beynəlxalq səviyyədə inteqrasiya oluna bilər.

#### PAN / WPAN

Smartfonlar, planşetlər, noutbuklar və ya masaüstü kompüterlər kimi müasir son cihazlar məlumat mübadiləsini təmin etmək üçün **ad hoc** şəbəkə yaratmaq üçün birləşdirilə bilər. Bu, **Fərdi Şəbəkə (Personal Area Network - PAN)** şəklində kabel vasitəsilə edilə bilər.

**Simsiz Fərdi Şəbəkə (Wireless Personal Area Network - WPAN)** simsiz variantı **Bluetooth** və ya **Wireless USB** texnologiyalarına əsaslanır. Bluetooth vasitəsilə qurulan simsiz fərdi şəbəkə **Piconet** adlanır. PAN-lar və WPAN-lar adətən yalnız bir neçə metr məsafəyə yayılır və buna görə də ayrı otaqlarda və ya hətta binalarda cihazları birləşdirmək üçün uyğun deyil.

**Əşyaların İnterneti (Internet of Things - IoT)** kontekstində, WPAN-lar aşağı məlumat sürətləri ilə nəzarət və monitorinq tətbiqləri ilə əlaqə qurmaq üçün istifadə olunur. **Insteon**, **Z-Wave** və **ZigBee** kimi protokollar xüsusi olaraq ağıllı evlər və ev avtomatlaşdırması üçün nəzərdə tutulmuşdur.
