## Şəbəkə Modelləri

İki şəbəkə modeli, məlumatların bir hostdan digərinə ötürülməsini və kommunikasiyasını təsvir edir ki, bunlar **ISO/OSI modeli** və **TCP/IP modeli** adlanır. Bu, ötürülən **Bitləri** bizim üçün oxunaqlı məzmunda təmsil edən sözügedən **qatların** (*layers*) sadələşdirilmiş təsviridir.

<img width="2576" height="1533" alt="image" src="https://github.com/user-attachments/assets/01bf474c-8261-4dbe-a575-164d0f47e108" />

## OSI Modeli

**OSI** modeli, tez-tez **ISO/OSI qat modeli** kimi xatırlanır, sistemlər arasındakı əlaqəni təsvir etmək və müəyyənləşdirmək üçün istifadə edilə bilən bir istinad modelidir. İstinad modelinin hər biri aydın şəkildə ayrılmış vəzifələrə malik **yeddi** fərdi qatı var.

**OSI** termini **Open Systems Interconnection** (*Açıq Sistemlərin Qarşılıqlı Əlaqəsi*) modelini ifadə edir, bu model **Beynəlxalq Telekommunikasiya İttifaqı (ITU)** və **Beynəlxalq Standartlaşdırma Təşkilatı (ISO)** tərəfindən nəşr edilmişdir. Buna görə də, OSI modeli tez-tez **ISO/OSI** qat modeli adlandırılır.

---

## TCP/IP Modeli

**TCP/IP** (**Transmission Control Protocol/Internet Protocol**) bir çox şəbəkə protokolu üçün ümumi bir termindir. Protokollar İnternetdə məlumat paketlərinin kommutasiyası və daşınması üçün məsuliyyət daşıyır. İnternet tamamilə **TCP/IP** protokol ailəsinə əsaslanır. Lakin, TCP/IP yalnız bu iki protokola aid deyil, adətən bütün bir protokol ailəsi üçün ümumi bir termin kimi istifadə olunur.

Məsələn, **ICMP (Internet Control Message Protocol)** və ya **UDP (User Datagram Protocol)** bu protokol ailəsinə aiddir. Protokol ailəsi fərdi və ya ictimai şəbəkədə məlumat paketlərinin daşınması və kommutasiyası üçün zəruri funksiyaları təmin edir.

---

## ISO/OSI vs. TCP/IP

**TCP/IP**, hostların İnternetə qoşulmasına imkan verən bir rabitə protokoludur. O, İnternetdəki tətbiqlər daxilində və tətbiqlər tərəfindən istifadə olunan **Transmission Control Protocol**-a istinad edir. **OSI**-dən fərqli olaraq, o, ümumi qaydalara əməl edildiyi təqdirdə, riayət edilməli olan qaydaların yüngülləşdirilməsinə imkan verir.

**OSI** isə, əksinə, şəbəkə və son istifadəçilər arasında bir rabitə şlüzüdür (*gateway*). OSI modeli adətən istinad modeli adlandırılır, çünki o daha yenidir və daha geniş istifadə olunur. O, həm də **sərt protokolları** və məhdudiyyətləri ilə tanınır.

---

## Paket Ötürmələri

Qatlı bir sistemdə, bir qatdakı cihazlar **protokol məlumat vahidi (protocol data unit - PDU)** adlanan fərqli bir formatda məlumat mübadiləsi aparır. Məsələn, kompüterdə bir veb-sayta baxmaq istədikdə, uzaq server proqramı əvvəlcə tələb olunan məlumatları **tətbiq qatına** (*application layer*) ötürür. Məlumatlar qat-qat işlənir, hər bir qat öz təyin olunmuş funksiyalarını yerinə yetirir. Daha sonra məlumat, təyinat serveri və ya başqa bir cihaz onu qəbul edənə qədər şəbəkənin **fiziki qatı** vasitəsilə ötürülür. Məlumat yenidən qatlar vasitəsilə marşrutlaşdırılır, hər bir qat öz təyin olunmuş əməliyyatlarını yerinə yetirir, nəticədə qəbuledici proqram təminatı məlumatı istifadə edir.

<img width="2576" height="1533" alt="image" src="https://github.com/user-attachments/assets/a0ced941-c303-4eea-9189-6bc9ddb3bca3" />

Ötürmə zamanı, hər bir qat yuxarı qatdan gələn **PDU**-ya (Protokol Məlumat Vahidinə) paketi idarə edən və müəyyənləşdirən bir **başlıq** (*header*) əlavə edir. Bu proses **kapsullama** (*encapsulation*) adlanır. Başlıq və məlumat birlikdə növbəti qat üçün **PDU**-nu təşkil edir. Proses, məlumatın qəbulediciyə ötürüldüyü **Fiziki Qata** (*Physical Layer*) və ya **Şəbəkə Qatına** (*Network Layer*) qədər davam edir. Qəbuledici prosesi tərs çevirir və məlumatları başlıq məlumatları ilə hər bir qatda **açır** (*unpacks*). Bundan sonra, tətbiq nəhayət məlumatı istifadə edir. Bütün məlumatlar göndərilənə və qəbul edilənə qədər bu proses davam edir.

<img width="2650" height="959" alt="image" src="https://github.com/user-attachments/assets/f43c76e4-3213-496a-83c2-7671ddae4255" />

Bizim üçün, penetrasiya sınaqçıları kimi, hər iki istinad modeli də faydalıdır. **TCP/IP** ilə biz bütün əlaqənin necə qurulduğunu tez başa düşə bilərik, **ISO** ilə isə onu hissə-hissə ayırıb ətraflı təhlil edə bilərik. Bu, tez-tez müəyyən şəbəkə trafikini dinləyə və ələ keçirə bildiyimiz zaman baş verir. Daha sonra biz bu trafiki müvafiq şəkildə təhlil etməliyik ki, bu da **Şəbəkə Trafikinin Təhlili** modulunda daha ətraflı nəzərdən keçiriləcək. Buna görə də, biz hər iki istinad modeli ilə tanış olmalı, onları ən yaxşı şəkildə başa düşməli və mənimsəməliyik.





















