## Front End vs. Back End

Biz **ön interfeys** (*front end*) və **arxa fon** (*back end*) veb inkişafı terminlərini və ya **Full Stack** veb inkişafı terminini eşitmiş ola bilərik ki, bu da həm **ön**, həm də **arxa fon** veb inkişafına aiddir. Bu terminlər veb inkişafı dövrünün əsas hissəsini təşkil etdiyi üçün veb tətbiqi inkişafı ilə **sinonim** olmaqdadır. Lakin, bu terminlər bir-birindən çox fərqlidir, çünki hər biri veb tətbiqinin bir tərəfinə aiddir və hər biri fərqli sahələrdə funksiya göstərir və əlaqə qurur.

### Front End (Ön İnterfeys)

Bir veb tətbiqinin **ön interfeysi** (*front end*) istifadəçinin **birbaşa veb brauzeri** (*klient tərəfi*) vasitəsilə qarşılıqlı əlaqədə olduğu komponentləri ehtiva edir. Bu komponentlər bir veb tətbiqinə daxil olarkən baxdığımız **veb səhifəsinin mənbə kodunu** təşkil edir və adətən **HTML, CSS** və **JavaScript**-i əhatə edir ki, bu da daha sonra brauzerlərimiz tərəfindən **real-time** tərcümə edilir.

<img width="1434" height="553" alt="image" src="https://github.com/user-attachments/assets/676c9050-5316-4ecc-bae6-b0fb92e628fd" />

Bu, istifadəçinin gördüyü və qarşılıqlı əlaqədə olduğu hər şeyi əhatə edir, məsələn, səhifənin əsas elementləri, o cümlədən **başlıq və mətn HTML**, **bütün elementlərin dizaynı və animasiyası CSS**, və **səhifənin hər bir hissəsinin hansı funksiyanı yerinə yetirməsi JavaScript**.

Müasir veb tətbiqlərində **ön interfeys komponentləri** istənilən **ekran ölçüsünə uyğunlaşmalı** və istənilən cihazda **istənilən brauzer daxilində işləməlidir**. Bu, adətən **müəyyən bir platforma və ya əməliyyat sistemi** üçün yazılan **arxa fon komponentlərinin** əksinədir. Əgər bir veb tətbiqinin ön interfeysi yaxşı **optimallaşdırılmayıbsa**, bütün veb tətbiqini **yavaş və cavab verməyən** edə bilər. Bu halda, bəzi istifadəçilər **hosting serverinin** və ya **internetlərinin yavaş olduğunu** düşünə bilər, halbuki problem tamamilə **klient tərəfində, istifadəçinin brauzerində** yerləşir. Buna görə də bir veb tətbiqinin ön interfeysi **əksər platformalar, cihazlar (o cümlədən mobil\!) və ekran ölçüləri üçün optimallaşdırılmalıdır**.

Ön interfeys kodu inkişafından başqa, aşağıdakılar **ön interfeys veb tətbiqi inkişafı** ilə əlaqəli digər tapşırıqlardır:

  * **Vizual Konsept Veb Dizaynı**
  * **İstifadəçi İnterfeysi (UI) dizaynı**
  * **İstifadəçi Təcrübəsi (UX) dizaynı**

Ön interfeys kodlaşdırması ilə məşğul olmaq üçün bizə əlçatan bir çox sayt var. Bir nümunə **budur**. Burada biz **redaktorla** oynaya, mətn yaza və formatlaya və nəticədə bizim üçün yaradılan **HTML, CSS** və **JavaScript**-i görə bilərik. Çox sadə bu nümunəni redaktorun sağ tərəfinə kopyalayın/yapışdırın:

```html
<p><strong>Welcome to Hack The Box Academy</strong><strong></strong></p><p></p><p><em>This is some italic text.</em></p><p></p><p><span style="color: #0000ff;">This is some blue text.</span></p><p></p><p></p>
```

Sadə HTML kodunun solda necə göstərildiyini izləyin. Formatlaşdırma, başlıqlar, rənglər və s. ilə oynayın və kodun dəyişməsini izləyin.

-----

## Back End (Arxa Fon)

Bir veb tətbiqinin **arxa fonu** (*back end*) **bütün əsas veb tətbiqi funksiyalarını** idarə edir, bunların hamısı **veb tətbiqinin düzgün işləməsi** üçün lazım olan hər şeyi emal edən **arxa fon serverində icra olunur**. Bu, bizim **heç vaxt görə bilməyəcəyimiz və ya birbaşa qarşılıqlı əlaqədə olmayacağımız** hissədir, lakin **arxa fon olmadan** bir vebsayt sadəcə **statik veb səhifələr toplusudur**.

Veb tətbiqləri üçün **dörd əsas arxa fon komponenti** var:

| Komponent | Təsvir |
| :--- | :--- |
| **Arxa fon Serverləri** | Bütün digər komponentləri host edən **aparat və əməliyyat sistemidir** və adətən **Linux, Windows** kimi əməliyyat sistemlərində və ya **Konteynerlər** (*Containers*) istifadə edilərək işlədilir. |
| **Veb Serverləri** | Veb serverləri **HTTP sorğularını və əlaqələrini** idarə edir. Bəzi nümunələr **Apache, NGINX** və **IIS**-dir. |
| **Verilənlər Bazaları** | Verilənlər bazaları (DB-lər) veb tətbiqinin **məlumatlarını saxlayır və əldə edir**. Relaativ (*Relational*) verilənlər bazalarının bəzi nümunələri **MySQL, MSSQL, Oracle, PostgreSQL**, qeyri-relaativ (*non-relational*) verilənlər bazalarının nümunələri isə **NoSQL** və **MongoDB**-dir. |
| **İnkişaf Çərçivələri** (*Development Frameworks*) | İnkişaf Çərçivələri əsas **Veb Tətbiqi Məntiqini** inkişaf etdirmək üçün istifadə olunur. Bəzi tanınmış çərçivələrə **Laravel (PHP), ASP.NET (C\#), Spring (Java), Django (Python)** və **Express (NodeJS JavaScript)** daxildir. |

<img width="1580" height="791" alt="image" src="https://github.com/user-attachments/assets/7f0ef063-41ad-49df-b8d1-5b18844e4297" />

Arxa fonun hər bir komponentini, məsələn, **Docker** kimi xidmətlərdən istifadə edərək, **öz təcrid olunmuş serverində** və ya **təcrid olunmuş konteynerlərində** host etmək də mümkündür. Məntiqi ayrılığı qorumaq və zəifliklərin təsirini azaltmaq üçün, verilənlər bazası kimi veb tətbiqinin fərqli komponentləri **bir Docker konteynerində** quraşdırıla bilər, əsas veb tətbiqi isə **başqa bir konteynerdə** quraşdırıla bilər, bununla da hər bir hissə digər konteynerlərə təsir edə biləcək potensial zəifliklərdən **təcrid olunur**. Həmçinin hər birini **öz xüsusi serverində** ayırmaq da mümkündür ki, bu da **daha çox resurs tələb edən** və texniki xidmət üçün **vaxt aparan** ola bilər. Lakin, bu, sözügedən **biznes vəziyyətindən** və veb tətbiqinin **dizaynından/funksionallığından** asılıdır.

Arxa fon komponentləri tərəfindən yerinə yetirilən əsas işlərdən bəziləri bunlardır:

* Veb tətbiqinin **arxa fonunun əsas məntiqini və xidmətlərini** inkişaf etdirmək
* Veb tətbiqinin **əsas kodunu və funksionallıqlarını** inkişaf etdirmək
* **Arxa fon verilənlər bazasını** inkişaf etdirmək və saxlamaq
* Veb tətbiqi tərəfindən istifadə ediləcək **kitabxanaları** inkişaf etdirmək və tətbiq etmək
* Veb tətbiqi üçün **texniki/biznes ehtiyaclarını** tətbiq etmək
* Ön interfeys komponentləri ilə rabitə üçün **əsas API-ləri** tətbiq etmək
* Uzaq serverləri və **bulud xidmətlərini** veb tətbiqinə inteqrasiya etmək

---

## Ön/Arxa Fonun Təhlükəsizliyini Təmin Etmək

Əksər hallarda **fərdi funksiyaları və kodun strukturunu təhlil etmək** üçün arxa fon koduna girişimiz olmasa da, bu, tətbiqi **zəiflikdən qorunmuş** etmir. Məsələn, müxtəlif **injeksiya hücumları** (*injection attacks*) ilə hələ də istismar edilə bilər.

Tutaq ki, bir veb tətbiqində **axtarış funksiyamız** var ki, axtarış sorğularımızı səhv emal edir. Bu halda, biz **xüsusi üsullardan** istifadə edərək sorğuları elə manipulyasiya edə bilərik ki, **xüsusi verilənlər bazası məlumatlarına icazəsiz giriş** əldə edək (**SQL injeksiyaları**) və ya hətta veb tətbiqi vasitəsilə **əməliyyat sistemi əmrlərini icra edək** ki, bu da **Əmr İnjeksiyaları** (*Command Injections*) kimi tanınır.

Daha sonra **ön və arxa fonda istifadə olunan hər bir komponentin təhlükəsizliyini necə təmin edəcəyimizi** müzakirə edəcəyik. Ön interfeys komponentlərinin **mənbə koduna tam girişimiz** olduqda, zəiflikləri tapmaq üçün **kod nəzərdən keçirilməsi** (*code review*) həyata keçirə bilərik ki, bu da **Ağ Qutu Pentest** (*Whitebox Pentesting*) adlanır.

Digər tərəfdən, arxa fon komponentlərinin mənbə kodu **arxa fon serverində saxlanılır**, buna görə də default olaraq ona girişimiz yoxdur, bu da bizi yalnız **Qara Qutu Pentest** (*Blackbox Pentesting*) kimi tanınan şeyi yerinə yetirməyə məcbur edir. Lakin, görəcəyimiz kimi, bəzi veb tətbiqləri **açıq mənbəlidir**, yəni onların mənbə koduna girişimiz var. Bundan əlavə, **Yerli Fayl Daxiletmə** (*Local File Inclusion*) kimi bəzi zəifliklər **arxa fon serverindən mənbə kodunu əldə etməyimizə** imkan verə bilər. Bu mənbə kodu əlimizdə olarsa, biz tətbiqin necə işlədiyini daha yaxşı başa düşmək, mənbə kodunda **həssas məlumatlar (məsələn, şifrələr) tapmaq** və hətta mənbə koduna giriş olmadan tapılması çətin və ya **qeyri-mümkün** olacaq zəiflikləri tapmaq üçün arxa fon komponentləri üzərində kod nəzərdən keçirilməsi həyata keçirə bilərik.

Nüfuzetmə sınağı mütəxəssisləri olaraq bizim üçün vacib olan veb tərtibatçılarının etdiyi **ən çox yayılmış $20$ səhv** aşağıdakılardır:

| № | Səhv |
| :--- | :--- |
| 1. | Etibarsız Məlumatların Verilənlər Bazasına Daxil Olmasına İcazə Vermək |
| 2. | Sistemə Bütünlükdə Diqqət Yetirmək |
| 3. | Şəxsi İnkişaf Etdirilmiş Təhlükəsizlik Metodlarını Yaratmaq |
| 4. | Təhlükəsizliyə Son Addımınız Kimi Baxmaq |
| 5. | Açıq Mətndə Şifrə Saxlamağı İnkişaf Etdirmək |
| 6. | Zəif Şifrələr Yaratmaq |
| 7. | Şifrələnməmiş Məlumatları Verilənlər Bazasında Saxlamaq |
| 8. | Klient Tərəfinə Həddindən Artıq Güvənmək |
| 9. | Çox Optimist Olmaq |
| 10. | URL Yolu Adı Vasitəsilə Dəyişənlərə İcazə Vermək |
| 11. | Üçüncü Tərəf Koduna Etibar Etmək |
| 12. | *Hard-code* edilmiş *backdoor* hesabları |
| 13. | Yoxlanılmamış SQL injeksiyaları |
| 14. | Uzaqdan fayl daxiletmələri |
| 15. | Təhlükəsiz olmayan məlumat emalı |
| 16. | Məlumatları düzgün şifrələməkdə uğursuzluq |
| 17. | Təhlükəsiz kriptoqrafik sistemdən istifadə etməmək |
| 18. | $8$-ci qatı görməzlikdən gəlmək |
| 19. | İstifadəçi əməliyyatlarını nəzərdən keçirmək |
| 20. | Veb Tətbiqi Divarı (*Web Application Firewall*) səhv konfiqurasiyaları |

Bu səhvlər veb tətbiqləri üçün **OWASP Top 10** zəifliklərinə gətirib çıxarır ki, bunları da digər modullarda müzakirə edəcəyik:

| № | Zəiflik |
| :--- | :--- |
| 1. | Pozulmuş Giriş Nəzarəti (*Broken Access Control*) |
| 2. | Kriptoqrafik Uğursuzluqlar (*Cryptographic Failures*) |
| 3. | İnjeksiya (*Injection*) |
| 4. | Təhlükəsiz Olmayan Dizayn (*Insecure Design*) |
| 5. | Təhlükəsizlik Səhv Konfiqurasiyası (*Security Misconfiguration*) |
| 6. | Zəif və Köhnəlmiş Komponentlər (*Vulnerable and Outdated Components*) |
| 7. | İdentifikasiya və Autentifikasiya Uğursuzluqları (*Identification and Authentication Failures*) |
| 8. | Proqram Təminatı və Məlumat Bütövlüyü Uğursuzluqları (*Software and Data Integrity Failures*) |
| 9. | Təhlükəsizlik Qeydlərinin (*Logging*) və Monitorinqinin Uğursuzluqları (*Security Logging and Monitoring Failures*) |
| 10. | Server Tərəfindən Sorğu Saxtalaşdırılması (*Server-Side Request Forgery - SSRF*) |

Bu qüsurlar və zəifliklərlə tanış olmağa başlamaq vacibdir, çünki onlar gələcək veb və hətta qeyri-veb ilə əlaqəli modullarda əhatə etdiyimiz bir çox məsələlər üçün **əsas** təşkil edir. *Pentester* olaraq, biz bu zəiflikləri müştərilərimiz üçün **səlahiyyətlə müəyyən etmək, istismar etmək və izah etmək** qabiliyyətinə malik olmalıyıq.

---

Veb tətbiqlərinin ön və arxa fon komponentləri arasında olan fərqi və onların təhlükəsizliyini necə yoxlayacağınızı indi daha yaxşı başa düşdünüzmü? 🤔






















