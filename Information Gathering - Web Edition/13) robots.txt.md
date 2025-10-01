robots.txt
Təsəvvür edin ki, siz böyük bir ev partiyasına qonaqsınız. Sərbəstsiniz, insanlarla ünsiyyət qura və ətrafı araşdıra bilərsiniz, amma bəzən “Private” (Şəxsi) kimi işarələnmiş otaqlar olur və siz oraya girməməyiniz gözlənilir. Bu, veb crawling dünyasında robots.txt-in funksiyasına bənzəyir. Bu, botlar üçün virtual “etiket qaydası” kimi çıxış edir, hansı veb sayt hissələrinə daxil ola biləcəklərini və hansı yerlərin qadağan olduğunu göstərir.

### robots.txt nədir?

Texniki baxımdan, robots.txt sadə bir mətn faylıdır və veb saytın kök direktoriyasında yerləşir (məsələn, [www.example.com/robots.txt](http://www.example.com/robots.txt)). Bu fayl, Robots Exclusion Standard-a (Veb crawler-ların bir veb sayt ziyarət edərkən davranış qaydaları) riayət edir. Faylda “direktivlər” şəklində göstərişlər olur ki, botlara saytın hansı hissələrinə daxil ola biləcəkləri və hansılarına girməyəcəkləri deyilir.

### robots.txt necə işləyir

robots.txt-dəki direktivlər adətən müəyyən user-agent-lərə yönəlib, yəni fərqli bot növlərinin identifikatorları. Məsələn, bir direktiv belə görünə bilər:

```txt
User-agent: *
Disallow: /private/
```

Bu direktiv bütün user-agent-lara (buradakı * wildcard kimi çıxış edir) bildirir ki, /private/ ilə başlayan URL-lərə daxil olmaq qadağandır. Digər direktivlər xüsusi direktoriyalara və fayllara icazə verə bilər, serveri yükləməmək üçün crawl gecikmələri təyin edə bilər və ya səmərəli crawling üçün sitemap-lara keçid təmin edə bilər.

### robots.txt Strukturunu Anlamaq

robots.txt faylı sadə mətn sənədidir və veb saytın kök direktoriyasında yerləşir. Fayl sadə struktura malikdir, hər bir göstəriş bloku (“record”) boş sətirlə ayrılır. Hər blokun iki əsas komponenti var:

* **User-agent:** Bu sətir hansı crawler və ya bot üçün qaydaların tətbiq ediləcəyini göstərir. Wildcard (*) bütün botlar üçün tətbiq ediləcək qaydaları göstərir. Eyni zamanda spesifik user-agent-lər hədəflənə bilər, məsələn, “Googlebot” (Google-un crawler-ı) və ya “Bingbot” (Microsoft-un crawler-ı).
* **Directives (Direktivlər):** Bu sətirlər müəyyən edilmiş user-agent-ə konkret göstəriş verir.

#### Ümumi direktivlər:

| Direktiv    | Təsviri                                                                                           | Nümunə                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Disallow    | Botun daxil olmaması lazım olan yollar və ya nümunələri göstərir.                                 | Disallow: /admin/ (admin direktoriyasına giriş qadağan)                             |
| Allow       | Botun müəyyən yollar və ya nümunələrə daxil olmasına icazə verir, hətta Disallow altında olsa da. | Allow: /public/ (public direktoriyasına giriş icazəli)                              |
| Crawl-delay | Botun ardıcıl sorğuları arasında gecikmə (saniyə ilə) təyin edir, serveri yükləməmək üçün.        | Crawl-delay: 10 (sorğular arasında 10 saniyə gecikmə)                               |
| Sitemap     | Daha səmərəli crawling üçün XML sitemap URL-i göstərir.                                           | Sitemap: [https://www.example.com/sitemap.xml](https://www.example.com/sitemap.xml) |

### Niyə robots.txt-ə hörmət etmək vacibdir?

robots.txt məcburi deyil (qeyri-qanuni bot onu ignor edə bilər), lakin əksər legitim crawler-lar və axtarış motorları onun direktivlərinə əməl edir. Bu bir neçə səbəbə görə vacibdir:

* **Serverləri yükləməkdən qorunmaq:** Botların müəyyən sahələrə girişini məhdudlaşdırmaqla, veb sayt sahibləri serverlərin çox yüklənməsinin qarşısını ala bilərlər.
* **Həssas məlumatların qorunması:** robots.txt şəxsi və ya gizli məlumatların axtarış motorları tərəfindən indekslənməsinin qarşısını ala bilər.
* **Hüquqi və etik uyğunluq:** Bəzən robots.txt direktivlərini nəzərə almamaq saytın xidmət şərtlərinin pozulması və ya hüquqi məsələ ola bilər, xüsusən müəllif hüququ və ya şəxsi məlumatlara girişdə.

### Veb Kəşfiyyatda robots.txt

Veb kəşfiyyat üçün robots.txt qiymətli bir məlumat mənbəyidir. Bu fayldakı direktivlərə hörmət edərək, təhlükəsizlik mütəxəssisləri hədəf veb saytın strukturu və potensial zəiflikləri haqqında mühüm məlumatlar əldə edə bilərlər:

* **Gizli direktoriyaların aşkarlanması:** robots.txt-də qadağan olunmuş yollar tez-tez sayt sahibinin axtarış motorlarından qorumaq istədiyi direktoriyalara və fayllara işarə edir. Bu gizli sahələrdə həssas məlumatlar, backup fayllar, admin panellər və ya hücumçunun maraq göstərə biləcəyi digər resurslar ola bilər.
* **Veb sayt strukturunun xəritələnməsi:** İcazə verilən və qadağan olunmuş yolları analiz edərək, təhlükəsizlik mütəxəssisləri saytın strukturunun sadə xəritəsini yarada bilərlər. Bu, əsas naviqasiyadan kənarda olan səhifələri və funksiyaları ortaya çıxara bilər.
* **Crawler tuzaqlarının aşkarlanması:** Bəzi saytlar bilərəkdən zərərli botları cəlb etmək üçün robots.txt-də “honeypot” direktoriyalar əlavə edirlər. Bu cür tuzaqları aşkar etmək hədəfin təhlükəsizlik tədbirləri haqqında məlumat verə bilər.

### robots.txt nümunəsi:

```txt
User-agent: *
Disallow: /admin/
Disallow: /private/
Allow: /public/

User-agent: Googlebot
Crawl-delay: 10

Sitemap: https://www.example.com/sitemap.xml
```

Bu faylda:

* Bütün user-agent-lar /admin/ və /private/ direktoriyalarına daxil ola bilməz.
* Bütün user-agent-lar /public/ direktoriyasına daxil ola bilər.
* Googlebot üçün sorğular arasında 10 saniyə gecikmə təyin olunub.
* Daha asan crawling üçün sitemap URL-i göstərilib.

Bu robots.txt faylını analiz edərək, saytın ehtimal ki, /admin/ altında admin paneli və /private/ direktoriyasında bəzi şəxsi məzmunu olduğunu təxmin etmək olar.
