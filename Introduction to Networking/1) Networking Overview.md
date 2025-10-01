## Şəbəkəyə Ümumi Baxış

Şəbəkə iki kompüterin bir-biri ilə əlaqə qurmasına imkan verir. Şəbəkəni asanlaşdırmaq üçün istifadə edilə bilən geniş çeşidli **topologiyalar** (mesh/tree/star), **mühitlər** (ethernet/fiber/koaksial/simsiz) və **protokollar** (TCP/UDP/IPX) mövcuddur. Təhlükəsizlik peşəkarları üçün şəbəkələşməni başa düşmək vacibdir, çünki şəbəkədə bir səhv olduqda, bu, səssiz ola bilər və bizə nəsə bir şeyi gözdən qaçırmağımıza səbəb ola bilər.

---

Böyük, **yastı bir şəbəkə** (*flat network*) qurmaq həddindən artıq çətin deyil və ən azından əməliyyat baxımından etibarlı bir şəbəkə ola bilər. Lakin yastı bir şəbəkə, qapısında kilid olduğu üçün onu təhlükəsiz hesab edən, torpaq sahəsində ev tikməyə bənzəyir. Daha kiçik şəbəkələr yaradaraq və onların əlaqə qurmasını təmin edərək, biz **müdafiə qatları** əlavə edə bilərik. Şəbəkədə **pivoting** (bir şəbəkədən digərinə keçid) çətin deyil, lakin bunu tez və səssiz etmək çətindir və hücumçuları yavaşlatacaq. Ev ssenarisinə qayıdaq, gəlin aşağıdakı nümunələri nəzərdən keçirək:

**Nümunə № 1**

Daha kiçik şəbəkələr qurmaq və onların ətrafına **Giriş Nəzarət Siyahıları (Access Control Lists - ACLs)** qoymaq, mülkün sərhədi ətrafına xüsusi giriş və çıxış nöqtələri yaradan hasar çəkməyə bənzəyir. Bəli, bir hücumçu hasarın üstündən tullana bilər, lakin bu şübhəli görünür və yayğın deyil, bu da onun zərərli fəaliyyət kimi tez aşkarlanmasına imkan verir. **Niyə printer şəbəkəsi serverlərlə HTTP üzərindən danışır?**

**Nümunə № 2**

Hər bir şəbəkənin məqsədini xəritələşdirməyə və sənədləşdirməyə vaxt ayırmaq, mülkün ətrafına işıqlar qoyaraq bütün fəaliyyətin görünməsini təmin etməyə bənzəyir. **Niyə printer şəbəkəsi ümumiyyətlə internetlə danışır?**

**Nümunə № 3**

Pəncərələrin ətrafında kolların olması, pəncərəni açmağa çalışan insanlar üçün bir çəkindirici amildir. Eynilə **Suricata** və ya **Snort** kimi **Müdaxilə Aşkarlama Sistemləri (Intrusion Detection Systems - IDS)** şəbəkə skanlarını işə salmağa qarşı bir çəkindiricidir. **Niyə printer şəbəkəsindən bir port skanı başladı?**

Bu nümunələr axmaq səslənə bilər və yuxarıdakıların hamısını etmək üçün bir printeri bloklamaq **sağlam düşüncədir**. Lakin, əgər printer **"yastı /24 şəbəkəsindədirsə"** və bir **DHCP** ünvanı alırsa, bu cür məhdudiyyətləri ona tətbiq etmək çətin ola bilər.

---

## Hekayə Vaxtı - Bir Penetratorun Gözdən Qaçırması

Əksər şəbəkələr **"/24"** altşəbəkəsindən istifadə edir, o qədər ki, bir çox **Penetrasiya Sınağı Mütəxəssisi** yoxlamadan bu altşəbəkə maskasını (255.255.255.0) təyin edir. **"/24"** şəbəkəsi IP ünvanının ilk üç okteti eyni olduğu müddətcə kompüterlərin bir-biri ilə danışmasına imkan verir (məsələn: 192.168.1.xxx). Altşəbəkə maskasını **"/25"** olaraq təyin etmək bu diapazonu yarıya bölür və kompüter yalnız "öz yarısında" olan kompüterlərlə danışa biləcək. Biz elə Penetrasiya Sınağı hesabatları görmüşük ki, qiymətləndirici **Domain Controller-in (DC)** oflayn olduğunu iddia edib, halbuki reallıqda o, sadəcə başqa bir şəbəkədə olub. Şəbəkə strukturu təxminən belə idi:

* Server Gateway: **10.20.0.1/25**
* Domain Controller: **10.20.0.10/25**
* Client Gateway: **10.20.0.129/25**
* Client Workstation: **10.20.0.200/25**
* Pentester IP: **10.20.0.252/24** (Gateway **10.20.0.1** olaraq təyin edilmişdir)

Pentester **Client Workstations** ilə əlaqə qurdu və **Impacket** vasitəsilə bir iş stansiyasının şifrəsini oğurladığı üçün əla iş gördüyünü düşündü. Lakin, **şəbəkəni başa düşməməsi** səbəbindən, heç vaxt Client Şəbəkəsindən çıxa bilmədi və verilənlər bazası serverləri kimi daha **"yüksək dəyərli"** hədəflərə çata bilmədi. İnşallah, bu sizə qəribə gəlirsə, modulun sonunda bu ifadəyə qayıdıb onu başa düşə biləcəksiniz!

---

## Əsas Məlumat

Gəlin **Evdən İşləmə (Work From Home)** quruluşunun necə işləyə biləcəyinə dair aşağıdakı yüksək səviyyəli diaqrama nəzər salaq.

<img width="2576" height="2228" alt="image" src="https://github.com/user-attachments/assets/541fb667-4941-430f-80ec-f33ef828ad36" />

## Şəbəkəyə Baxışın Davamı

Bütün internet, nümunədə göstərilən və **"Ev Şəbəkəsi"** və **"Şirkət Şəbəkəsi"** kimi qeyd olunan bir çox altşəbəkələrə əsaslanır. **Şəbəkələşməni** bir kompüter tərəfindən göndərilən və digəri tərəfindən alınan məktub və ya paketlərin çatdırılması kimi təsəvvür edə bilərik.

Təsəvvür edək ki, **"Ev Şəbəkəmizdən"** bir şirkətin veb saytına daxil olmaq istəyirik. Bu halda, biz onların **"Şirkət Şəbəkəsində"** yerləşən veb saytı ilə məlumat mübadiləsi aparırıq. Məktub və ya paket göndərməkdə olduğu kimi, paketlərin hara getməli olduğunu bilirik. Brauzerimizə daxil etdiyimiz veb sayt ünvanı və ya **Uniform Resource Locator (URL)** həm də **Tam Təyin olunmuş Domen Adı (Fully Qualified Domain Name - FQDN)** kimi tanınır.

**URL** və **FQDN** arasındakı fərq budur:

* **FQDN** (`www.hackthebox.eu`) yalnız **"binanın"** ünvanını göstərir,
* **URL** (`https://www.hackthebox.eu/example?floor=2&office=dev&employee=17`) isə həm də **"mərtəbəni,"** **"ofisi,"** **"poçt qutusunu"** və paketin nəzərdə tutulduğu müvafiq **"işçini"** göstərir.

Dəqiq təsvirləri və tərifləri digər bölmələrdə daha aydın və dəqiq müzakirə edəcəyik.

Fakt budur ki, biz ünvanı bilirik, lakin ünvanın dəqiq coğrafi yerini bilmirik. Bu vəziyyətdə, **poçt şöbəsi** dəqiq yeri müəyyən edə bilər və sonra paketləri istənilən yerə yönləndirir. Buna görə də, bizim poçt şöbəmiz paketlərimizi **İnternet Xidmət Təminatçımızı (ISP)** təmsil edən **əsas poçt şöbəsinə** yönləndirir.

Şəbəkələşmədə "İnternetə" qoşulmaq üçün istifadə etdiyimiz poçt şöbəmiz bizim **routerimizdir**.

Paketimizi poçt şöbəmiz (router) vasitəsilə göndərən kimi, paket **əsas poçt şöbəsinə** (ISP) ötürülür. Bu əsas poçt şöbəsi bu ünvanın harada yerləşdiyini **ünvan reyestrində/telefon kitabçasında (Domain Name Service - DNS)** axtarır və müvafiq **coğrafi koordinatları (IP ünvanı)** qaytarır. İndi ünvanın dəqiq yerini bildiyimizə görə, paketimiz əsas poçt şöbəmiz vasitəsilə birbaşa ora göndərilir.

Veb server bizim veb saytlarının necə göründüyünə dair sorğu ilə paketimizi aldıqdan sonra, veb server **"Şirkət Şəbəkəsinin"** poçt şöbəsi (router) vasitəsilə veb saytın təqdimatı üçün məlumatları olan paketi göstərilən geri ünvanımıza (bizim IP ünvanımıza) geri göndərir.

---

## Əlavə Qeydlər

Bu diaqramda göstərilən şirkət şəbəkəsinin beş ayrı şəbəkədən ibarət olmasını ümid edərdim!

* **Veb Server** **DMZ-də (Demilitarized Zone - Demilitarizasiya Edilmiş Zona)** olmalıdır, çünki internetdəki müştərilər veb saytla əlaqə qura bilər, bu da onun güzəştə getmə ehtimalını artırır. Onu ayrıca bir şəbəkədə yerləşdirmək administratorlara veb server və digər cihazlar arasında şəbəkə qorumaları tətbiq etməyə imkan verir.
* **İş Stansiyaları** (*Workstations*) öz şəbəkələrində olmalıdır və ideal bir dünyada, hər bir iş stansiyası digər iş stansiyaları ilə danışmasının qarşısını alan **Host-Based Firewall** qaydasına sahib olmalıdır. Əgər bir İş Stansiyası Serverlə eyni şəbəkədədirsə, **spoofing** və ya **ortadakı adam (man in the middle)** kimi şəbəkə hücumları daha böyük bir problemə çevrilir.
* **Switch** və **Router** **"İdarəetmə Şəbəkəsində"** olmalıdır. Bu, iş stansiyalarının bu cihazlar arasındakı hər hansı bir əlaqəyə qulaq asmasının qarşısını alır. Mən tez-tez Penetrasiya Sınağı keçirmişəm və **OSPF (Open Shortest Path First)** reklamlarını görmüşəm. Routerin **etibarlı şəbəkəsi** olmadığı üçün, daxili şəbəkədə olan hər kəs zərərli bir reklam göndərə və istənilən şəbəkəyə qarşı **ortadakı adam** hücumu həyata keçirə bilərdi.
* **IP Telefonlar** öz şəbəkələrində olmalıdır. Təhlükəsizlik baxımından bu, kompüterlərin ünsiyyətə qulaq asmasının qarşısını almaq üçündür. Təhlükəsizlikdən əlavə, telefonlar **gecikmənin/lagın** vacib olması baxımından unikaldır. Onları öz şəbəkələrində yerləşdirmək, şəbəkə administratorlarına yüksək gecikmənin qarşısını almaq üçün onların trafikini daha asanlıqla prioritetləşdirməyə imkan verir.
* **Printerlər** öz şəbəkələrində olmalıdır. Bu qəribə səslənə bilər, lakin bir printeri təmin etmək demək olar ki, qeyri-mümkündür. Windows-un işləmə tərzi səbəbindən, əgər bir printer çap işi zamanı bir kompüterə autentifikasiyanın tələb olunduğunu bildirirsə, həmin kompüter **NTLMv2** autentifikasiyası cəhdi edəcək ki, bu da parolların oğurlanmasına səbəb ola bilər. Bundan əlavə, bu cihazlar **davamlılıq** üçün əladır və ümumiyyətlə onlara tonlarla həssas məlumat göndərilir.

---

## Əyləncəli Hekayə

COVID zamanı, mənə ştatlararası **Fiziki Penetrasiya Sınağı** tapşırıldı və mənim ştatımda **"evdə qalma"** əmri var idi. Sınaqdan keçirdiyim şirkətdə ofisdə minimal heyət var idi. Mən bahalı bir printer almağa qərar verdim və ona **tərs qabıq (reverse shell)** yerləşdirmək üçün istismar etdim ki, şəbəkəyə qoşulanda mənə bir qabıq (uzaqdan giriş) göndərsin. Sonra printeri şirkətə göndərdim və heyətə ofisə gəldikləri üçün təşəkkür edən və bir neçə gün WFH üçün nələrisə evə aparmaq istəsələr, printerin onlara daha sürətli çap etməyə və ya skan etməyə imkan verəcəyini izah edən bir **fişinq e-poçtu** göndərdim. Printer demək olar ki, dərhal qoşuldu və onların **domen administratorunun kompüteri** öz etimadnamələrini printerə göndərəcək qədər nəzakətli oldu!

Əgər müştəri **təhlükəsiz bir şəbəkə** qurmuş olsaydı, bu hücum bir çox səbəbə görə mümkün olmazdı:

* Printer **internetlә danışa bilməməliydi**.
* İş stansiyası **445-ci port üzərindən printerlə əlaqə qura bilməməliydi**.
* Printer **iş stansiyalarına əlaqələri başlada bilməməliydi**. Bəzi hallarda, printer/skaner kombinasiyaları skan edilmiş sənədləri e-poçtla göndərmək üçün bir poçt serveri ilə əlaqə qura bilməlidir.


















