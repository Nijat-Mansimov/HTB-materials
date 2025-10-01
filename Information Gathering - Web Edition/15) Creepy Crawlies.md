**Qorxulu Böcəklər (Creepy Crawlies)**
Veb tarama (web crawling) geniş və mürəkkəbdir, amma bu yola tək çıxmaq məcburiyyətində deyilsiniz. Sizə kömək edəcək çoxsaylı veb tarama alətləri mövcuddur; hər birinin öz güclü tərəfləri və ixtisaslaşdığı sahələr var. Bu alətlər tarama prosesini avtomatlaşdırır, onu daha sürətli və effektiv edir və sizə çıxarılan məlumatları analiz etməyə fokuslanmaq üçün vaxt verir.

**Populyar Veb Crawler-lər**

* **Burp Suite Spider:** Burp Suite — geniş istifadə olunan veb tətbiq testi platforması — aktiv crawler olan Spider komponentini ehtiva edir. Spider veb tətbiqləri xəritələşdirmədə, gizli məzmunu aşkar etməkdə və potensial zəiflikləri üzə çıxarmaqda yaxşıdır.
* **OWASP ZAP (Zed Attack Proxy):** ZAP pulsuz, açıq mənbə veb tətbiq təhlükəsizliyi scanner-dir. O, avtomatlaşdırılmış və əl ilə istifadə rejimlərində işləyə bilir və veb tətbiqləri taramaq və mümkün zəiflikləri aşkar etmək üçün spider komponentini ehtiva edir.
* **Scrapy (Python Framework):** Scrapy özəl veb crawler-lər qurmaq üçün çevik və miqyaslana bilən Python çərçivəsidir. O, veb saytlarından strukturlaşdırılmış məlumat çıxarmaq, kompleks tarama situasiyalarını idarə etmək və məlumat emalını avtomatlaşdırmaq üçün zəngin funksiyalar təqdim edir. Fleksibliyi onu məqsədyönlü kəşfiyyat tapşırıqları üçün ideal edir.
* **Apache Nutch (Scalable Crawler):** Nutch Java ilə yazılmış, yüksək extensible və miqyaslana bilən açıq mənbə veb crawler-dir. O, bütün internet üzrə geniş miqyaslı crawlları idarə etmək və ya müəyyən domenlərə fokuslanmaq üçün nəzərdə tutulub. Quraşdırma və konfiqurasiya üçün daha çox texniki bilik tələb etsə də, onun gücü və çevikliyi böyük miqyaslı kəşfiyyat layihələri üçün qiymətli alət edir.

Hansı aləti seçməyinizdən asılı olmayaraq, etik və məsuliyyətli tarama təcrübələrinə riayət etmək vacibdir. Xüsusilə geniş və ya müdaxiləedici skanlar planlaşdırırsınızsa, hər zaman sayt sahibindən icazə alın. Saytın server resurslarına diqqətli olun və həddindən artıq sorğularla onları yükləməkdən çəkinin.

**Scrapy**
Biz Scrapy-dən və `inlanefreight.com` üçün hazırlanmış xüsusi bir spiderdən istifadə edəcəyik. Əgər tarama/spidering texnikaları haqqında daha çox məlumat istəyirsinizsə, "Using Web Proxies" moduluna baxın — o da CBBH paketinin bir hissəsidir.

**Scrapy-nin Quraşdırılması**
Başlamazdan əvvəl sisteminizdə Scrapy-nin quraşdırıldığından əmin olun. Əgər quraşdırılmayıbsa, Python paketi meneceri pip ilə asanlıqla quraşdıra bilərsiniz:

```bash
Creepy Crawlies
nijatmansimov@htb[/htb]$ pip3 install scrapy
```

Bu əmr Scrapy və onun asılılıqlarını yükləyib quraşdıracaq və spidermizi yaratmaq üçün mühiti hazırlayacaq.

**ReconSpider**
Əvvəlcə terminalınızda aşağıdakı əmri işlədərək xüsusi scrapy spidəri ReconSpider-i yükləyin və onu cari iş qovluğuna çıxarın:

```bash
Creepy Crawlies
nijatmansimov@htb[/htb]$ wget -O ReconSpider.zip https://academy.hackthebox.com/storage/modules/144/ReconSpider.v1.2.zip
nijatmansimov@htb[/htb]$ unzip ReconSpider.zip 
```

Fayllar çıxarıldıqdan sonra ReconSpider.py faylını aşağıdakı əmr ilə işə sala bilərsiniz:

```bash
Creepy Crawlies
nijatmansimov@htb[/htb]$ python3 ReconSpider.py http://inlanefreight.com
```

`inlanefreight.com`-u taramaq istədiyiniz domenlə əvəz edin. Spider hədəfi tarayaraq dəyərli məlumatlar toplayacaq.

**results.json**
ReconSpider.py işləndikdən sonra məlumatlar `results.json` adlı JSON faylına saxlanacaq. Bu faylı istənilən mətn redaktoru ilə aça bilərsiniz. Aşağıda yaradılan JSON faylının strukturu göstərilmişdir:

```json
{
    "emails": [
        "lily.floid@inlanefreight.com",
        "cvs@inlanefreight.com",
        ...
    ],
    "links": [
        "https://www.themeansar.com",
        "https://www.inlanefreight.com/index.php/offices/",
        ...
    ],
    "external_files": [
        "https://www.inlanefreight.com/wp-content/uploads/2020/09/goals.pdf",
        ...
    ],
    "js_files": [
        "https://www.inlanefreight.com/wp-includes/js/jquery/jquery-migrate.min.js?ver=3.3.2",
        ...
    ],
    "form_fields": [],
    "images": [
        "https://www.inlanefreight.com/wp-content/uploads/2021/03/AboutUs_01-1024x810.png",
        ...
    ],
    "videos": [],
    "audio": [],
    "comments": [
        "<!-- #masthead -->",
        ...
    ]
}
```

JSON-dəki hər bir açar hədəf veb saytından çıxarılan fərqli məlumat növünü təmsil edir:

| Açar (JSON Key)  | Təsviri                                                          |
| ---------------- | ---------------------------------------------------------------- |
| `emails`         | Domen üzərindən tapılmış e‑poçt ünvanları siyahısı.              |
| `links`          | Tapılmış linklərin URL‑ləri.                                     |
| `external_files` | PDF kimi xarici faylların URL‑ləri.                              |
| `js_files`       | Sayt tərəfindən istifadə olunan JavaScript fayllarının URL‑ləri. |
| `form_fields`    | Domen üzərində tapılmış forma sahələri (bu nümunədə boşdur).     |
| `images`         | Saytdan tapılmış şəkillərin URL‑ləri.                            |
| `videos`         | Tapılmış video URL‑ləri (bu nümunədə boşdur).                    |
| `audio`          | Tapılmış audio fayl URL‑ləri (bu nümunədə boşdur).               |
| `comments`       | Mənbə kodunda tapılmış HTML şərhləri.                            |

Bu JSON strukturunu araşdırmaqla veb tətbiqinin arxitekturası, məzmunu və əlavə yoxlamalar üçün potensial maraq nöqtələri barədə dəyərli məlumatlar əldə edə bilərsiniz.
