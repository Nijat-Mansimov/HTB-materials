## Xarici Kəşfiyyat və Nömrələmə Prinsipləri

Hər hansı bir sızma testinə başlamazdan əvvəl, **hədəfinizin xarici kəşfiyyatını** (reconnaissance) aparmaq faydalı ola bilər. Bu, bir çox fərqli funksiyaları yerinə yetirə bilər, məsələn:

  * Müştəri tərəfindən **əhatə sənədində (scoping document)** sizə verilən məlumatların **doğrulanması**
  * Uzaqdan işləyərkən **müvafiq əhatəyə** qarşı hərəkətlər etdiyinizə əmin olmaq
  * **Sızmış etimadnamələr** kimi, testinizin nəticəsinə təsir edə biləcək, **ictimaiyyətə açıq** məlumatların axtarılması

Bunu belə düşünün; biz müştərimiz üçün mümkün olan ən hərtərəfli testi təmin etmək üçün **"torpağın düzülüşünü"** (lay of the land) öyrənməyə çalışırıq. Bu, həm də dünyada potensial **məlumat sızıntılarını** və **pozuntu məlumatlarını** (breach data) müəyyən etmək deməkdir. Bu, müştərinin əsas veb-saytından və ya sosial mediadan bir **istifadəçi adı formatını** öyrənmək qədər sadə ola bilər. Həmçinin, kod göndərmələrində (code pushes) buraxılmış etimadnamələr üçün **GitHub repozitoriyalarını** skan etmək, sənədlərdə **intranetə** və ya uzaqdan əlçatan saytlara aparan **linkləri** ovlamaq və sadəcə olaraq korporativ mühitin necə konfiqurasiya edildiyi barədə bizə ipucu verə biləcək hər hansı bir məlumatı axtarmaq qədər dərinə də gedə bilərik.

-----

## Nə Axtarırıq?

Xarici kəşfiyyat apararkən, axtarmalı olduğumuz bir neçə **əsas məqam** var. Bu məlumatlar həmişə ictimaiyyətə açıq olmaya bilər, lakin nələrin mövcud olduğunu görmək ehtiyatlı olardı. Sızma testi zamanı ilişsək, **passiv kəşfiyyat** yolu ilə əldə edilə bilənlərə geri baxmaq, bizi irəliləmək üçün lazım olan itələyişi verə bilər, məsələn, **VPN** və ya digər xaricə baxan xidmətə daxil olmaq üçün istifadə edilə biləcək şifrə pozuntusu məlumatları. Aşağıdakı cədvəl öhdəliyimizin bu mərhələsində axtaracağımız **"Nə"** hissəsini vurğulayır.

| Məlumat Nöqtəsi | Təsvir |
| :--- | :--- |
| **IP Sahəsi** | Hədəfimiz üçün **Etibarlı ASN**, təşkilatın ictimaiyyətə açıq infrastrukturu üçün istifadə olunan **netbloklar**, **bulud mövcudluğu** və hostinq provayderləri, **DNS rekord girişləri**, və s. |
| **Domen Məlumatları** | IP məlumatlarına, DNS-ə və sayt qeydiyyatlarına əsaslanır. Domeni kim idarə edir? Hədəfimizə bağlı hər hansı **alt domen** varmı? Hər hansı ictimaiyyətə açıq **domen xidmətləri** (Mailserverlər, DNS, Veb-saytlar, VPN portalları, və s.) mövcuddurmu? Hansı növ **müdafiələrin** (SIEM, AV, IPS/IDS istifadədə, və s.) yerində olduğunu müəyyən edə bilərikmi? |
| **Sxem Formatı** | Təşkilatın **e-poçt hesablarını**, **AD istifadəçi adlarını**, və hətta **şifrə siyasətlərini** aşkar edə bilərikmi? Xaricə baxan xidmətləri **şifrə çiləmə (password spraying)**, **etimadnamə doldurma (credential stuffing)**, **kobud qüvvə (brute forcing)** və s. üçün test etmək üçün istifadə edə biləcəyimiz etibarlı istifadəçi adları siyahısını qurmaq üçün bizə məlumat verə biləcək hər hansı bir şey. |
| **Məlumat Açıqlamaları** | Məlumat açıqlamaları üçün, hədəfə aydınlıq gətirməyə kömək edən hər hansı bir məlumat üçün ictimaiyyətə açıq sənədləri (**.pdf, .ppt, .docx, .xlsx, və s.**) axtaracağıq. Məsələn, **intranet sayt siyahıları**, **istifadəçi metadataları**, **paylaşımları**, və ya mühitdəki digər kritik proqram təminatı və ya avadanlıqları ehtiva edən hər hansı nəşr olunmuş fayllar (ictimai **GitHub reposuna** itələnmiş etimadnamələr, məsələn, bir PDF-in metadatasında daxili **AD istifadəçi adı formatı**). |
| **Pozuntu Məlumatları** | Bir hücumçunun dayaq qazanmasına kömək edə biləcək hər hansı ictimaiyyətə açıqlanmış **istifadəçi adları, şifrələr**, və ya digər kritik məlumatlar. |

Biz xarici kəşfiyyatın **niyəsini** və **nəyini** müzakirə etdik; gəlin indi **harada** və **necə** hissəsinə keçək.

-----

## Harada Axtarırıq?

Yuxarıdakı məlumat nöqtələri siyahımız bir çox fərqli yollarla toplana bilər. Qiymətləndirməmiz üçün həyati əhəmiyyət kəsb edən məlumatları əldə etmək üçün istifadə edə biləcəyimiz yuxarıdakı məlumatların bəzilərini və ya hamısını təmin edə biləcək bir çox fərqli veb-sayt və alətlər var. Aşağıdakı cədvəl istifadə edilə biləcək bir neçə potensial resursu və nümunəni sadalayır.

| Resurs | Nümunələr |
| :--- | :--- |
| **ASN / IP qeydiyyatçıları** | IANA, Amerikaları axtarmaq üçün **arin**, Avropada axtarış üçün **RIPE**, **BGP Toolkit** |
| **Domen Qeydiyyatçıları & DNS** | **Domaintools**, **PTRArchive**, **ICANN**, sözügedən domendə və ya 8.8.8.8 kimi yaxşı tanınan DNS serverlərinə qarşı əl ilə DNS rekord sorğuları. |
| **Sosial Media** | **Linkedin**, **Twitter**, **Facebook**, bölgənizin əsas sosial media saytları, xəbər məqalələri, və təşkilat haqqında tapa biləcəyiniz hər hansı müvafiq məlumat. |
| **İctimaiyyətə Açıq Şirkət Veb-saytları** | Çox vaxt bir korporasiyanın ictimai veb-saytında əlaqəli məlumatlar yerləşdirilmiş olur. Xəbər məqalələri, yerləşdirilmiş sənədlər, və "**Haqqımızda**" və "**Bizimlə Əlaqə**" səhifələri də qızıl mədəni ola bilər. |
| **Bulud və Dev Saxlama Sahələri** | **GitHub**, **AWS S3 buckets** & **Azure Blog** saxlama konteynerləri, **"Dorklar"** istifadə edərək Google axtarışları |
| **Pozuntu Məlumat Mənbələri** | Hər hansı korporativ e-poçt hesablarının ictimai pozuntu məlumatlarında görünüb-görünmədiyini müəyyən etmək üçün **HaveIBeenPwned**, təmiz mətn şifrələri və ya oflayn sındırmağa cəhd edə biləcəyimiz heşlər olan korporativ e-poçtları axtarmaq üçün **Dehashed**. Daha sonra bu şifrələri AD autentifikasiyasından istifadə edə biləcək hər hansı açıq qalan giriş portallarına (Citrix, RDS, OWA, O365, VPN, VMware Horizon, xüsusi tətbiqlər, və s.) qarşı sınaya bilərik. |

### Ünvan Sahələrinin Tapılması

<img width="967" height="459" alt="image" src="https://github.com/user-attachments/assets/41a6457d-653b-4028-baac-a2efaf365325" />

Hurricane Electric tərəfindən hostlanan **BGP-Toolkit** bir təşkilata hansı **ünvan bloklarının** təyin olunduğunu və hansı **ASN** daxilində yerləşdiyini araşdırmaq üçün fantastik bir resursdur. Sadəcə bir domen və ya IP ünvanı daxil edin və alət tapa biləcəyi hər hansı bir nəticəni axtaracaq. Bu məlumatlardan çox şey öyrənə bilərik. Bir çox böyük korporasiyalar çox vaxt öz infrastrukturlarını **özləri host** edəcəklər və böyük ayaq izləri olduğu üçün öz ASN-lərinə sahib olacaqlar. Bu, adətən daha kiçik təşkilatlar və ya yeni yaranan şirkətlər üçün belə olmayacaq. Araşdırdığınız zaman bunu yadda saxlayın, çünki daha kiçik təşkilatlar veb-saytlarını və digər infrastrukturlarını çox vaxt **başqasının sahəsində** host edəcəklər (məsələn, Cloudflare, Google Cloud, AWS, və ya Azure). Bu infrastrukturun harada yerləşdiyini başa düşmək testimiz üçün **olduqca vacibdir**. Əhatəmizdən kənar infrastrukturlarla qarşılıqlı əlaqədə **olmadığımızdan** əmin olmalıyıq. Kiçik bir təşkilata qarşı sızma testi edərkən diqqətli olmasaq, bilməyərəkdən həmin infrastrukturu paylaşan **başqa bir təşkilata zərər** verə bilərik. Sizin müştəri ilə test etmək üçün razılaşmanız var, eyni serverdəki başqaları ilə və ya provayderlə yox. Özü tərəfindən hostlanan və ya 3-cü tərəf tərəfindən idarə olunan infrastrukturla bağlı suallar **əhatə prosesi** zamanı həll edilməli və aldığınız hər hansı əhatə sənədlərində açıq şəkildə göstərilməlidir.

Bəzi hallarda, müştərinizin test etməzdən əvvəl üçüncü tərəf hostinq provayderindən **yazılı təsdiq** alması lazım ola bilər. AWS kimi digərlərinin sızma testlərinin aparılması üçün **xüsusi təlimatları** var və bəzi xidmətlərini test etmək üçün əvvəlcədən təsdiq tələb etmirlər. Oracle kimi digərləri, Bulud Təhlükəsizliyi Testi Bildirişi (Cloud Security Testing Notification) təqdim etməyinizi xahiş edirlər. Bu cür addımlar **şirkət rəhbərliyiniz, hüquq qrupunuz, müqavilələr qrupunuz** və s. tərəfindən həll edilməlidir. Şübhə edirsinizsə, qiymətləndirmə zamanı əmin olmadığınız hər hansı xaricə baxan xidmətə hücum etməzdən əvvəl **məsələni qaldırın**. Hər hansı bir hosta (həm daxili, həm də xarici) hücum etmək üçün **açıq icazəmizin** olmasını təmin etmək bizim məsuliyyətimizdir və dayanıb əhatəni yazılı şəkildə dəqiqləşdirmək heç vaxt zərər vermir.

### DNS

**DNS** əhatəmizi **doğrulamaq** və müştərinin əhatə sənədində açıqlamadığı **əlçatan hostlar** haqqında məlumat əldə etmək üçün əla bir yoldur. **domaintools** və **viewdns.info** kimi saytlar başlamaq üçün əla yerlərdir. DNS həllindən (resolution) tutmuş DNSSEC üçün test etməyə və saytın daha məhdud ölkələrdə əlçatan olub-olmadığına qədər bir çox rekord və digər məlumatları geri ala bilərik. Bəzən əhatədən kənar, lakin maraqlı görünən əlavə hostlar tapa bilərik. Bu halda, hər hansı birinin həqiqətən əhatəyə daxil edilməli olub-olmadığını görmək üçün bu siyahını müştərimizə təqdim edə bilərik. Həmçinin, əhatə sənədlərində sadalanmayan, lakin **əhatədə olan IP ünvanlarında** yerləşən və buna görə də **icazəli olan** maraqlı **alt domenlər** də tapa bilərik.

**Viewdns.info**

<img width="1336" height="736" alt="image" src="https://github.com/user-attachments/assets/2b502e7d-475b-4c63-8338-fdb0409bf51e" />

Bu, həm də IP/ASN axtarışlarımızdan tapılan bəzi məlumatları **doğrulamaq** üçün əla bir yoldur. Domen haqqında tapılan bütün məlumatlar aktual olmayacaq və gördüklərimizi təsdiqləyə bilən yoxlamalar aparmaq həmişə yaxşı bir təcrübədir.

### İctimai Məlumat

**Sosial media** təşkilatın necə qurulduğu, hansı avadanlıqdan istifadə etdiyi, potensial proqram təminatı və təhlükəsizlik tətbiqləri, sxemi və daha çoxu haqqında bizə ipucu verə biləcək maraqlı məlumatların **xəzinəsi** ola bilər. Bu siyahının üstündə **LinkedIn, Indeed.com, və Glassdoor** kimi işlə bağlı saytlar var. Sadə **iş elanları** çox vaxt bir şirkət haqqında çox şey aşkar edir. Məsələn, aşağıdakı iş elanına nəzər salın. Bu, bir **SharePoint Administratoru** üçündür və bir çox şey barədə bizə ipucu verə bilər. Elandan şirkətin bir müddətdir SharePoint istifadə etdiyini və təhlükəsizlik proqramları, ehtiyat nüsxə və fəlakətin bərpası və daha çox haqqında danışdıqları üçün **yetkin bir proqrama** sahib olduğunu görə bilərik. Bu elanda bizim üçün maraqlı olan, şirkətin çox güman ki, **SharePoint 2013** və **SharePoint 2016** istifadə etdiyini görə bilərik. Bu o deməkdir ki, onlar yerində təkmilləşdirmiş ola bilərlər, potensial olaraq daha yeni versiyalarda mövcud olmayan **zəiflikləri** oyunda buraxa bilərlər. Bu həm də öhdəliklərimiz zamanı SharePoint-in **fərqli versiyalarına** rast gələ biləcəyimiz deməkdir.

**Sharepoint Admin İş Elanı**

<img width="1021" height="392" alt="image" src="https://github.com/user-attachments/assets/bd3afaa4-405d-4db7-99a5-b2a1a51262f8" />

**İş elanları** və ya **sosial media** kimi ictimai məlumatları **endirməyin**. Sadəcə nə yayımladıqlarından bir təşkilat haqqında çox şey öyrənə bilərsiniz və yaxşı niyyətli bir yazı sızma testçiləri olaraq bizim üçün əlaqəli məlumatları açıqlaya bilər.

Təşkilat tərəfindən hostlanan **veb-saytlar** da məlumat qazmaq üçün əla yerlərdir. Biz **əlaqə e-poçtlarını, telefon nömrələrini, təşkilati cədvəlləri, nəşr olunmuş sənədləri** və s. toplaya bilərik. Bu saytlar, xüsusilə də **yerləşdirilmiş sənədlər**, tez-tez **daxili infrastruktura** və ya başqa cür bilməyəcəyiniz **intranet saytlarına** linklərə sahib ola bilər. Domen quruluşunun bir şəklini formalaşdırmağa çalışarkən bu cür detallar üçün ictimaiyyətə açıq hər hansı bir məlumatı yoxlamaq **sürətli qələbələr** ola bilər. **GitHub, AWS bulud yaddaşı**, və digər veb-hostlu platformaların artan istifadəsi ilə məlumatlar da **bilmədən sızdırıla** bilər. Məsələn, bir layihə üzərində işləyən bir dev təsadüfən **bəzi etimadnamələri** və ya qeydləri kodun buraxılışına **sərt kodlaşdırılmış** (hardcoded) buraxa bilər. O məlumatı harada axtaracağınızı bilirsinizsə, bu sizə **asan bir qələbə** verə bilər. Bu, saatlarla və ya günlərlə şifrə çiləmə və kobud qüvvə ilə etimadnamələri sınamaqla, yoxsa yüksək imtiyazlara sahib ola biləcək developer etimadnamələri ilə **sürətli bir dayaq** qazanmaq arasındakı fərq demək ola bilər. **Trufflehog** kimi alətlər və **Greyhat Warfare** kimi saytlar bu çörək qırıntılarını tapmaq üçün fantastik resurslardır.

Biz bir təşkilatın **xarici nömrələnməsi** və **kəşfiyyatı** haqqında bir qədər vaxt sərf etdik, lakin bu, tapmacanın yalnız bir hissəsidir. OSINT və xarici nömrələməyə daha ətraflı giriş üçün "Footprinting and OSINT: Corporate Recon" modullarına baxın.

Bu nöqtəyə qədər müzakirələrimiz əsasən **passiv** olmuşdur. Sızma testinə irəlilədikcə, tapdığınız məlumatları doğrulayaraq və daha çox məlumat üçün domendə sorğu apararaq daha çox **əl ilə** (hands-on) işləyəcəksiniz. Gəlin bir dəqiqə **nömrələmə prinsiplərini** və bu hərəkətləri yerinə yetirmək üçün necə bir proses qura biləcəyimizi müzakirə edək.

-----

## Ümumi Nömrələmə Prinsipləri

Məqsədimizin **hədəfimizi daha yaxşı başa düşmək** olduğunu nəzərə alaraq, içəri daxil olmaq üçün potensial bir yol təmin edə biləcək hər mümkün yolu axtarırıq. **Nömrələmə** özü sızma testi boyunca bir neçə dəfə təkrarlayacağımız **təkrarlanan bir prosesdir**. Müştərinin əhatə sənədindən başqa, bu, əsas məlumat mənbəyimizdir, ona görə də **heç bir daşı çevrilməmiş** buraxmadığımızdan əmin olmaq istəyirik. Nömrələməyə başlayarkən, əvvəlcə **passiv resurslardan** istifadə edəcəyik, əhatə dairəsində **geniş başlayıb daralacağıq**. Passiv nömrələmənin ilkin gedişini tükəndirdikdən sonra, nəticələri nəzərdən keçirməli və daha sonra **aktiv nömrələmə mərhələmizə** keçməliyik.

### Nümunə Nömrələmə Prosesi

Artıq nömrələməyə aid xeyli konsepsiyaları əhatə etdik. Gəlin hər şeyi bir araya gətirməyə başlayaq. Hər hansı bir ağır skan (əhatədən kənar olan Nmap və ya zəiflik skanları kimi) etmədən **inlanefreight.com** domenində nömrələmə taktikalarımızı tətbiq edəcəyik. Əvvəlcə Netblok məlumatlarımızı yoxlamaqla başlayacağıq və nə tapa biləcəyimizi görəcəyik.

#### ASN/IP və Domen Məlumatları üçün Yoxlama

<img width="962" height="581" alt="image" src="https://github.com/user-attachments/assets/10e6deba-abef-4c1b-9e21-28ca97a16f50" />

Bu ilk baxışdan, artıq bəzi **maraqlı məlumatlar** əldə etdik. BGP.he hesabat verir:

  * **IP Ünvanı:** $134.209.24.248$
  * **Mail Serveri:** `mail1.inlanefreight.com`
  * **Ad Serverləri:** `NS1.inlanefreight.com` və `NS2.inlanefreight.com`

Hələlik, onun çıxışından bizə lazım olan budur. Inlanefreight böyük bir korporasiya deyil, ona görə də öz ASN-ə sahib olmasını gözləmirdik. İndi bu məlumatların bəzilərini doğrulayaq.

#### Viewdns Nəticələri

<img width="1014" height="435" alt="image" src="https://github.com/user-attachments/assets/f304203a-56f6-4c7a-bd47-79c1d9d08b2a" />

Yuxarıdakı sorğuda, hədəfimizin IP ünvanını doğrulamaq üçün **viewdns.info** istifadə etdik. Hər iki nəticə **uyğun gəlir**, bu da yaxşı bir əlamətdir. İndi nəticələrimizdəki iki ad serverini doğrulamaq üçün başqa bir yolla cəhd edək.

```bash
 External Recon and Enumeration Principles
nijatmansimov@htb[/htb]$ nslookup ns1.inlanefreight.com

Server:		192.168.186.1
Address:	192.168.186.1#53

Non-authoritative answer:
Name:	ns1.inlanefreight.com
Address: 178.128.39.165

nslookup ns2.inlanefreight.com
Server:		192.168.86.1
Address:	192.168.86.1#53

Non-authoritative answer:
Name:	ns2.inlanefreight.com
Address: 206.189.119.186
```

İndi doğrulama və test üçün siyahımıza əlavə etmək üçün **iki yeni IP ünvanımız** var. Onlarla hər hansı bir əlavə hərəkət etməzdən əvvəl, testiniz üçün **əhatədə** olduqlarına əmin olun. Məqsədlərimiz üçün, faktiki IP ünvanları skan üçün əhatədə olmazdı, lakin maraqlı məlumatları ovlamaq üçün hər hansı bir veb-sayta **passiv olaraq** baxa bilərik. Hələlik, DNS-dən domen məlumatlarını nömrələməklə bu qədər. Gəlin ictimaiyyətə açıq məlumatlara nəzər salaq.

Inlanefreight bu modul üçün istifadə etdiyimiz **uydurma bir şirkətdir**, buna görə də real sosial media mövcudluğu yoxdur. Lakin, real olsaydı, faydalı məlumatlar üçün **LinkedIn, Twitter, Instagram, və Facebook** kimi saytları yoxlayardıq. Bunun əvəzinə, inlanefreight.com veb-saytını araşdırmağa keçəcəyik.

Aparatdığımız ilk yoxlama hər hansı bir **sənəd** axtarışı idi. Axtarış olaraq `filetype:pdf inurl:inlanefreight.com` istifadə edərək, PDF-ləri axtarırıq.

#### Fayl Ovçuluğu

<img width="922" height="377" alt="image" src="https://github.com/user-attachments/assets/3db9ebe2-1815-4a01-ae22-f0ba2b9b0a49" />

Bir sənəd çıxdı, buna görə də sənədi və onun yerini qeyd etməli və qazmaq üçün **yerli olaraq bir nüsxəsini yükləməliyik**. Hər hansı bir faylı, ekran görüntülərini, skan çıxışını, alət çıxışını və s. onlarla rastlaşan kimi və ya onları yaratdığımız kimi **saxlamaq** həmişə ən yaxşısıdır. Bu, mümkün qədər **hərtərəfli qeyd** aparmağımıza və harada nə gördüyümüzü unutmaq və ya kritik məlumatları itirmək riskini almamağımıza kömək edir. Next, let's look for any email addresses we can find.

#### E-poçt Ünvanı Ovçuluğu

<img width="889" height="420" alt="image" src="https://github.com/user-attachments/assets/97603111-651e-4018-bf67-c8f51c5d1bc6" />

`intext:"@inlanefreight.com" inurl:inlanefreight.com` dorkunu istifadə edərək, veb-saytda bir e-poçt ünvanının sonuna bənzər hər hansı bir nümunəni axtarırıq. Bir əlaqə səhifəsi ilə bir ümidverici nəticə çıxdı. Səhifəyə baxdığımızda (aşağıda təsvir edilmişdir), bir neçə işçinin və onların əlaqə məlumatlarının böyük bir siyahısını görə bilərik. Bu məlumat faydalı ola bilər, çünki bu insanların ən azı çox güman ki, **aktiv** olduqlarını və hələ də şirkətdə işlədiklərini müəyyən edə bilərik.

**E-poçt Dork Nəticələri**

<img width="1161" height="552" alt="image" src="https://github.com/user-attachments/assets/63ece3c4-4be3-44dd-ba5e-6ead9506375b" />

Əlaqə səhifəsinə baxaraq, qlobal miqyasda fərqli ofislərdə olan heyət üçün bir neçə e-poçt görə bilərik. İndi onların **e-poçt adlandırma konvensiyası** (`ad.soyad`) və bəzi insanların təşkilatda harada işlədiyi barədə bir fikrimiz var. Bu, daha sonra **şifrə çiləmə hücumlarında** və ya **sosial mühəndislik/fişinq** öhdəliyimizin əhatə dairəsinə daxil olsaydı, faydalı ola bilərdi.

### İstifadəçi Adı Yığımı

Biz **linkedin2username** kimi bir alətdən istifadə edərək bir şirkətin LinkedIn səhifəsindən məlumatları sıyırıb (scrape) və müxtəlif **istifadəçi adları mashupları** (`flast, first.last, f.last`, və s.) yarada bilərik ki, bu da potensial **şifrə çiləmə hədəfləri** siyahımıza əlavə edilə bilər.

### Etimadnamə Ovçuluğu

**Dehashed** pozuntu məlumatlarında **təmiz mətn etimadnamələrini** və **şifrə heşlərini** ovlamaq üçün əla bir vasitədir. Biz ya saytda, ya da API vasitəsilə sorğular aparan bir skriptdən istifadə edərək axtarış edə bilərik. Tipik olaraq, AD autentifikasiyasından istifadə edən xaricə baxan portallarda (və ya daxili) işləməyən istifadəçilər üçün bir çox köhnə şifrələr tapacağıq, lakin **bəxtimiz gətirə** bilər\! Bu, xarici və ya daxili şifrə çiləmə üçün **istifadəçi siyahısı yaratmaq** üçün faydalı ola biləcək başqa bir vasitədir.

*Qeyd: Məqsədlərimiz üçün, aşağıdakı nümunə məlumatları uydurmadır.*

```bash
 External Recon and Enumeration Principles
nijatmansimov@htb[/htb]$ sudo python3 dehashed.py -q inlanefreight.local -p

id : 5996447501
email : roger.grimes@inlanefreight.local
username : rgrimes
password : Ilovefishing!
hashed_password : 
name : Roger Grimes
vin : 
address : 
phone : 
database_name : ModBSolutions

id : 7344467234
email : jane.yu@inlanefreight.local
username : jyu
password : Starlight1982_!
hashed_password : 
name : Jane Yu
vin : 
address : 
phone : 
database_name : MyFitnessPal

<SNIP>
```

*Qeyd: Nümunədə istifadə olunan skript burada tapıla bilər. DeHashed-in API quruluşundakı dəyişikliklərə görə, dəyişikliklər lazım ola bilər. Alternativ olaraq, aşağıdakı skript istifadə edilə bilər. Skripti icra etməzdən əvvəl, onun funksionallığı ilə tanış olmaq **çox vacibdir**.*

İndi ki, buna yiyələndik, inlanefreight.com domeni ilə əlaqəli **digər nəticələri** axtarmağa çalışın. Nə tapa bilərsiniz? Saytda yerləşdirilmiş hər hansı başqa **faydalı fayllar, səhifələr**, və ya **məlumat** varmı? Bu bölmə, əhatə daxilində qaldığımızı və səlahiyyətli olmadığımız heç bir şeyi test etmədiyimizi və öhdəliyin vaxt məhdudiyyətləri daxilində qaldığımızı təmin etməklə, hədəfimizi **hərtərəfli təhlil etməyin** vacibliyini nümayiş etdirdi. Mən daxili şəbəkədə anonim bir nöqtədən dayaq qazanmaqda çətinlik çəkdiyim və müxtəlif xarici mənbələrdən (Google, LinkedIn scraping, Dehashed, və s.) istifadə edərək bir söz siyahısı yaratdığım, sonra isə standart bir domen istifadəçi hesabı üçün **etibarlı etimadnamələr** əldə etmək üçün **hədəfli daxili şifrə çiləmə** apardığım bir neçə qiymətləndirmə keçirmişəm. Sonrakı bölmələrdə görəcəyimiz kimi, daxili AD nömrələməsinin böyük əksəriyyətini sadəcə bir sıra **aşağı imtiyazlı domen istifadəçi etimadnamələri** və hətta bir çox hücumla yerinə yetirə bilərik. **Əlimizdə bir sıra etimadnamə olduqdan sonra əyləncə başlayır.** Gəlin daxili nömrələməyə keçək və qiymətləndirməmizin əhatə dairəsi və öhdəlik qaydalarına uyğun olaraq daxili **INLANEFREIGHT.LOCAL** domenini **passiv** və **aktiv** şəkildə təhlil etməyə başlayaq.


-----

## LAB 1

- While looking at inlanefreights public records; A flag can be seen. Find the flag and submit it. ( format == HTB{******} )

<img width="1163" height="944" alt="image" src="https://github.com/user-attachments/assets/428a59ad-31a7-4109-8a10-41eb50d21920" />


