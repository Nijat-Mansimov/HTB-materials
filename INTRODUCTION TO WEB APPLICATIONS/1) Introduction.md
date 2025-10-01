## Giriş

**Veb tətbiqləri** (*Web applications*) **veb brauzerlərində** işləyən **interaktiv tətbiqlərdir**. Veb tətbiqləri adətən işləmək və qarşılıqlı əlaqələri idarə etmək üçün **klient-server arxitekturasını** qəbul edir. Onlar tipik olaraq **klient tərəfində** (*brauzerdə*) işləyən **ön interfeys** (*front end*) komponentlərinə (yəni, vebsayt interfeysi və ya "istifadəçinin gördüyü") və **server tərəfində** (*back end* server/verilənlər bazalarında) işləyən digər **arxa fon** (*back end*) komponentlərinə (veb tətbiqinin mənbə kodu) malikdirlər.

Bu, təşkilatlara **dizayn və funksionallığı üzərində demək olar ki, tam real-time nəzarətlə** güclü tətbiqləri yerləşdirməyə imkan verir, eyni zamanda **dünya üzrə əlçatan** olurlar. Tipik veb tətbiqlərin bəzi nümunələrinə **Gmail** kimi **onlayn e-poçt xidmətləri**, **Amazon** kimi **onlayn pərakəndə satıcılar** və **Google Docs** kimi **onlayn mətn prosessorları** daxildir.

<img width="2301" height="529" alt="image" src="https://github.com/user-attachments/assets/c03479db-f2b4-4ec7-a15e-25053c7bb63a" />

Veb tətbiqləri **Google** və ya **Microsoft** kimi nəhəng provayderlərə aid deyil, lakin **istənilən veb tərtibatçısı** tərəfindən inkişaf etdirilə və internetdəki **ümumi hostinq xidmətlərindən** hər hansı birində yerləşdirilə bilər ki, internetdəki **hər kəs** tərəfindən istifadə olunsun. Buna görə də bu gün bütün internetdə **milyonlarla veb tətbiqi** mövcuddur və **milyardlarla istifadəçi** onlarla hər gün qarşılıqlı əlaqədə olur.

---

## Veb Tətbiqləri vs. Vebsaytlar

Keçmişdə biz **statik** və **real-time dəyişdirilə bilməyən** vebsaytlarla qarşılıqlı əlaqədə olurduq. Bu o deməkdir ki, ənənəvi vebsaytlar **müəyyən məlumatları təmsil etmək üçün statik olaraq yaradılmışdı** və bu məlumat bizim qarşılıqlı əlaqəmizlə dəyişməzdi. Vebsaytın məzmununu dəyişmək üçün uyğun səhifə tərtibatçılar tərəfindən **əllə redaktə edilməli** idi. Bu tip statik səhifələr **funksiyaları ehtiva etmir** və buna görə də **real-time dəyişikliklər yaratmır**. Bu tip vebsayt **Web 1.0** olaraq da bilinir.

<img width="2364" height="1081" alt="image" src="https://github.com/user-attachments/assets/5b3c9e16-9fcd-4090-8d3e-eb593dfad0ec" />

Digər tərəfdən, əksər vebsaytlar istifadəçi qarşılıqlı əlaqəsinə əsaslanan **dinamik məzmun** təqdim edən **veb tətbiqləri** və ya **Web 2.0** işlədir. Başqa bir əhəmiyyətli fərq ondadır ki, veb tətbiqləri **tamamilə funksionaldır** və son istifadəçi üçün müxtəlif funksiyaları yerinə yetirə bilər, halbuki veb saytlarında bu tip funksionallıq yoxdur. Ənənəvi vebsaytlar və veb tətbiqləri arasındakı digər əsas fərqlər bunlardır:

* **Modulyar** olmaq
* **İstənilən ekran ölçüsündə** işləmək
* **Optimallaşdırılmadan** istənilən platformada işləmək

***

## Veb Tətbiqləri vs. Yerli Əməliyyat Sistemi Tətbiqləri (Native OS Applications)

**Yerli Əməliyyat Sistemi (yerli OS) tətbiqlərindən** fərqli olaraq, veb tətbiqləri **platformadan asılı deyil** və istənilən əməliyyat sistemində bir brauzerdə işləyə bilər. Bu veb tətbiqlər istifadəçinin sistemində **quraşdırılmalı deyil**, çünki bu veb tətbiqləri və onların funksionallığı **uzaq serverdə uzaqdan icra edilir** və buna görə də son istifadəçinin sərt diskində **heç bir yer tutmur**.

Veb tətbiqlərinin yerli OS tətbiqləri üzərindəki başqa bir üstünlüyü **versiya birliyidir** (*version unity*). Veb tətbiqinə daxil olan bütün istifadəçilər **eyni versiyadan** və **eyni veb tətbiqindən** istifadə edirlər ki, bu da **hər bir istifadəçiyə yeniləmələr göndərmədən** davamlı olaraq yenilənə və dəyişdirilə bilər. Veb tətbiqləri **hər platforma üçün fərqli *build*-lər hazırlamadan tək bir yerdə** (*vebserverdə*) yenilənə bilər ki, bu da texniki xidmət və dəstək xərclərini kəskin şəkildə azaldır və dəyişiklikləri fərdi olaraq bütün istifadəçilərə çatdırmaq ehtiyacını aradan qaldırır.

Digər tərəfdən, yerli OS tətbiqlərinin veb tətbiqləri üzərində müəyyən üstünlükləri var ki, bunlar əsasən onların **əməliyyat sürəti** və **yerli əməliyyat sistemi kitabxanalarından və yerli avadanlıqdan istifadə etmək qabiliyyətidir**. Yerli tətbiqlər yerli OS kitabxanalarından istifadə etmək üçün qurulduğundan, **yüklənməsi və onlarla qarşılıqlı əlaqə daha sürətlidir**. Bundan əlavə, yerli tətbiqlər adətən veb tətbiqlərindən **daha qabliyyətlidir**, çünki əməliyyat sisteminə daha dərin inteqrasiyaya malikdirlər və **yalnız brauzerin imkanları ilə məhdudlaşmırlar**.

Bununla belə, son zamanlarda **hibrid** və **proqressiv veb tətbiqləri** daha da geniş yayılır. Onlar yerli OS imkanlarından və resurslarından istifadə edərək veb tətbiqlərini işlətmək üçün müasir çərçivələrdən (*frameworks*) istifadə edirlər ki, bu da onları adi veb tətbiqlərindən **daha sürətli** və **daha qabliyyətli** edir.

***

## Veb Tətbiqinin Paylanması

Dünya üzrə təşkilatlar tərəfindən istifadə olunan və hər bir təşkilatın ehtiyaclarına cavab vermək üçün **fərdiləşdirilə bilən** bir çox **açıq mənbəli** (*open-source*) veb tətbiqləri mövcuddur. Bəzi ümumi açıq mənbəli veb tətbiqlərinə daxildir:

* **WordPress**
* **OpenCart**
* **Joomla**

Həmçinin, adətən müəyyən bir təşkilat tərəfindən inkişaf etdirilən və sonra başqa bir təşkilata satılan və ya təşkilatlar tərəfindən **abunə planı modeli** ilə istifadə olunan xüsusi **'qapalı mənbəli'** (*closed source*) veb tətbiqləri də var. Bəzi ümumi qapalı mənbəli veb tətbiqlərinə daxildir:

* **Wix**
* **Shopify**
* **DotNetNuke**

***

## Veb Tətbiqlərinin Təhlükəsizlik Riskləri

**Veb tətbiqi hücumları** geniş yayılmışdır və ölçülərindən asılı olmayaraq, **veb mövcudluğu** olan əksər təşkilatlar üçün problem yaradır. Axı, onlar adətən **internet bağlantısı və veb brauzeri olan hər kəs tərəfindən** istənilən ölkədən əlçatandır və adətən **geniş hücum səthi** təklif edirlər. Veb tətbiqlərini **skan etmək və hücum etmək** üçün bir çox **avtomatlaşdırılmış alətlər** mövcuddur ki, yanlış əllərdə əhəmiyyətli zərər verə bilər. Veb tətbiqləri daha mürəkkəb və təkmil olduqca, onların dizaynına **kritik zəifliklərin** daxil edilməsi ehtimalı da artır.

Uğurlu bir veb tətbiqi hücumu **əhəmiyyətli itkilərə** və **kütləvi biznes fasilələrinə** səbəb ola bilər. Veb tətbiqləri digər **həssas məlumatları yerləşdirə biləcək serverlərdə** işlədiyi və tez-tez **həssas istifadəçi və ya korporativ məlumatları** ehtiva edən **verilənlər bazalarına** qoşulduğu üçün, bir veb saytına uğurla hücum edilərsə, **bütün bu məlumatlar təhlükə altına düşə bilər**. Buna görə də, veb tətbiqlərindən istifadə edən hər hansı bir biznes üçün bu tətbiqləri **zəifliklər üçün düzgün şəkildə sınaqdan keçirmək** və yamaların **qüsuru düzəltdiyini və bilmədən yeni qüsurlar yaratmadığını** yoxlayaraq dərhal **yamaq** (*patch*) etmək vacibdir.

**Veb tətbiqinin nüfuzetmə sınağı** (*penetration testing*) öyrənilməsi getdikcə kritikləşən bir bacarıqdır. İnternetə baxan (və daxili) veb tətbiqlərini qorumaq istəyən hər bir təşkilat tez-tez veb tətbiqi sınaqlarından keçməli və **hər inkişaf həyat dövrü mərhələsində təhlükəsiz kodlaşdırma təcrübələrini** tətbiq etməlidir. Veb tətbiqlərini düzgün şəkildə *pentest* etmək üçün onların **necə işlədiyini, necə inkişaf etdirildiyini** və istifadə olunan texnologiyalardan asılı olaraq tətbiqin hər bir qatında və komponentində **hansı riskin** olduğunu anlamalıyıq.

Biz həmişə **fərqli şəkildə dizayn edilmiş və konfiqurasiya edilmiş** müxtəlif veb tətbiqləri ilə qarşılaşacağıq. Veb tətbiqlərini sınaqdan keçirmək üçün ən aktual və geniş istifadə olunan metodlardan biri **OWASP Veb Təhlükəsizliyi Sınaq Bələdçisidir** (*OWASP Web Security Testing Guide*).

Ən ümumi prosedurlardan biri veb tətbiqinin **ön interfeys** (*front end*) komponentlərini, məsələn, **HTML, CSS** və **JavaScript**-i (həmçinin **ön interfeys üçlüyü** (*front end trinity*) kimi tanınır) nəzərdən keçirməyə başlamaq və **Həssas Məlumatların İfşası** (*Sensitive Data Exposure*) və **Saytlararası Skriptinq (XSS)** kimi zəiflikləri tapmağa cəhd etməkdir. Bütün ön interfeys komponentləri hərtərəfli sınaqdan keçirildikdən sonra, adətən **veb tətbiqinin əsas funksionallığını** və **brauzer ilə vebserver arasındakı qarşılıqlı əlaqəni** nəzərdən keçirərək vebserverin istifadə etdiyi **texnologiyaları sadalayır** və **istismar edilə bilən qüsurları** axtarırıq. Biz əhatə dairəsini maksimuma çatdırmaq və mümkün olan hər bir hücum ssenarisini nəzərdən keçirmək üçün veb tətbiqlərini adətən **həm autentifikasiyasız, həm də autentifikasiya olunmuş** perspektivdən qiymətləndiririk (əgər tətbiqin giriş funksionallığı varsa).

***

## Veb Tətbiqlərinə Hücum Etmək

Bu günlərdə demək olar ki, hər bir şirkətin, ölçüsündən asılı olmayaraq, xarici perimetri daxilində **bir və ya daha çox veb tətbiqi** var. Bu tətbiqlər sadə **statik vebsaytlardan**, **WordPress** kimi **Content Management Systems (CMS)** tərəfindən dəstəklənən **bloqlara** qədər, **əsas istifadəçilərdən super idarəçilərə qədər müxtəlif istifadəçi rollarını dəstəkləyən qeydiyyat/giriş funksionallığı** olan mürəkkəb tətbiqlərə qədər hər şey ola bilər. Hal-hazırda, birbaşa **məlum ictimai istismar** (*known public exploit*) vasitəsilə (məsələn, zəif xidmət və ya Windows uzaqdan kod icrası (RCE) zəifliyi kimi) istismar edilə bilən **xarici görünüşlü host** tapmaq çox yaygın deyil, baxmayaraq ki, bu baş verir. Veb tətbiqləri **geniş bir hücum səthi** təmin edir və onların dinamik təbiəti o deməkdir ki, onlar **davamlı olaraq dəyişir** (və nəzərdən qaçırılır!). Nisbətən sadə bir kod dəyişikliyi **fəlakətli bir zəifliyə** və ya **həssas məlumatlara giriş əldə etmək** və ya **vebserverdə** və ya verilənlər bazası serverləri kimi mühitdəki digər hostlarda **uzaqdan kod icrası** (*remote code execution*) əldə etmək üçün **zəncirlənə bilən** zəifliklər seriyasına səbəb ola bilər.

**Kod icrasına birbaşa səbəb ola biləcək qüsurların** tapılması nadir deyil, məsələn, **zərərli kodun yüklənməsinə** imkan verən bir fayl yükləmə forması və ya **uzaqdan kod icrasını əldə etmək** üçün istifadə edilə bilən **fayl daxiletmə zəifliyi** (*file inclusion vulnerability*). Müxtəlif növ veb tətbiqlərində hələ də olduqca yaygın olan yaxşı tanınan bir zəiflik **SQL İnjeksiya**-dır. Bu tip zəiflik **istifadəçi tərəfindən təqdim olunan girişin təhlükəsiz olmayan şəkildə işlənməsindən** yaranır. O, **həssas məlumatlara girişə**, **verilənlər bazası serverində faylların oxunmasına/yazılmasına** və hətta **uzaqdan kod icrasına** səbəb ola bilər.

Biz tez-tez **autentifikasiya üçün Active Directory** istifadə edən veb tətbiqlərində **SQL İnjeksiya zəiflikləri** tapırıq. Biz adətən bunu **şifrələri çıxarmaq** üçün istifadə edə bilməsək də (çünki Active Directory onları idarə edir), tez-tez **əksər və ya bütün Active Directory istifadəçi e-poçt ünvanlarını** çıxara bilərik ki, bu da çox vaxt onların **istifadəçi adları** ilə eynidir. Bu məlumat daha sonra **VPN** və ya **Microsoft Outlook Web Access/Microsoft O365** kimi **autentifikasiya üçün Active Directory istifadə edən veb portallarına qarşı şifrə *spray* hücumunu** (*password spraying*) həyata keçirmək üçün istifadə edilə bilər. Uğurlu bir şifrə *spray*-i çox vaxt **e-poçt kimi həssas məlumatlara girişə** və ya hətta **korporativ şəbəkə mühitinə birbaşa dayaq nöqtəsinə** (*foothold*) səbəb ola bilər.

Bu nümunə **tək bir veb tətbiqi zəifliyindən** yarana biləcək zərəri göstərir, xüsusilə də bir şirkətin xarici infrastrukturunun digər hissələrinə hücum etmək üçün istifadə edilə bilən məlumatları bir tətbiqdən çıxarmaq üçün **"zəncirlənmiş"** olduqda. Yaxşı təlim görmüş bir *infosec* mütəxəssisi **veb tətbiqləri haqqında dərin anlayışa** malik olmalı və **şəbəkə nüfuzetmə sınağı və Active Directory hücumlarını** həyata keçirdiyi qədər **veb tətbiqlərinə hücum etməkdə** də rahat olmalıdır. Veb tətbiqlərində güclü təməli olan bir nüfuzetmə sınağı mütəxəssisi tez-tez öz həmyaşıdlarından fərqlənə və başqalarının nəzərdən qaçıra biləcəyi qüsurları tapa bilər. Veb tətbiqi hücumlarının və təsirinin bir neçə real nümunəsi aşağıdakılardır:

| Qüsur | Real Həyat Senarisi |
| :--- | :--- |
| **SQL İnjeksiya** | Active Directory istifadəçi adlarını əldə etmək və **VPN** və ya **e-poçt portalına** qarşı şifrə *spray* hücumu həyata keçirmək. |
| **Fayl Daxiletmə** (*File Inclusion*) | **Uzaqdan kod icrası** əldə etmək üçün istifadə edilə bilən əlavə funksionallığı ifşa edən **gizli səhifə və ya kataloq tapmaq** üçün mənbə kodunu oxumaq. |
| **Məhdudiyyətsiz Fayl Yükləmə** (*Unrestricted File Upload*) | İstifadəçiyə **hər hansı bir fayl növünü** yükləməyə imkan verən (yalnız şəkillər deyil) bir profil şəkli yükləmə forması olan veb tətbiqi. Bu, **zərərli kod** yükləməklə **veb tətbiqi serverinə tam nəzarət** əldə etmək üçün istifadə edilə bilər. |
| **Təhlükəli Birbaşa Obyekt İstinadlandırması (IDOR)** (*Insecure Direct Object Referencing*) | **Pozulmuş giriş nəzarəti** (*broken access control*) kimi bir qüsurla birləşdirildikdə, bu, tez-tez başqa bir istifadəçinin **fayllarına və ya funksionallığına daxil olmaq** üçün istifadə edilə bilər. Bir nümunə, `/user/701/edit-profile` kimi bir səhifəyə baxaraq istifadəçi profilinizi redaktə etmək olardı. Əgər **701**-i **702**-yə dəyişə bilsək, başqa bir istifadəçinin profilini redaktə edə bilərik! |
| **Pozulmuş Giriş Nəzarəti** (*Broken Access Control*) | Başqa bir nümunə, istifadəçiyə yeni bir hesab qeydiyyatdan keçirməyə imkan verən bir tətbiqdir. Əgər hesab qeydiyyatı funksionallığı pis dizayn edilibsə, istifadəçi qeydiyyatdan keçərkən **imtiyaz yüksəldilməsi** (*privilege escalation*) həyata keçirə bilər. Yeni istifadəçi qeydiyyatdan keçərkən `username=bjones&password=Welcome1&email=bjones@inlanefreight.local&roleid=3` məlumatlarını təqdim edən **POST sorğusunu** nəzərdən keçirin. Bəs əgər biz **roleid** parametrini manipulyasiya edə və onu **0** və ya **1**-ə dəyişə bilsək? Biz bunun baş verdiyi real tətbiqləri görmüşük və **idarəçi istifadəçisini tez bir zamanda qeydiyyatdan keçirmək** və veb tətbiqinin **nəzərdə tutulmayan bir çox xüsusiyyətlərinə** daxil olmaq mümkün olmuşdur. |

Ümumi **veb tətbiqi hücumları** və onların nəticələri ilə tanış olmağa başlayın. Bu məqamda bu terminlərdən hər hansı biri qəribə səslənirsə narahat olmayın; irəlilədikcə və öyrənməyə **təkrarlanan yanaşmanı** (*iterative approach*) tətbiq etdikcə onlar aydınlaşacaq.

**Veb tətbiqlərini dərindən öyrənmək** və onların necə işlədiyini və bir çox fərqli tətbiq *stack*-i ilə tanış olmaq vacibdir. Akademiya səyahətimizdə, əsas HTB platformasında və real həyat qiymətləndirmələrində **veb tətbiqi hücumlarını** təkrar-təkrar görəcəyik. Gəlin daxil olaq və **veb tətbiqlərinin quruluşunu/funksiyasını** öyrənək ki, **daha məlumatlı hücumçular** olaq, öz həmyaşıdlarımızdan fərqlənək və başqalarının nəzərdən qaçıra biləcəyi qüsurları tapaq.




















