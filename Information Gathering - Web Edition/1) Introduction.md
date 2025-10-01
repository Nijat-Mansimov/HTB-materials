**Giriş**

**Veb Kəşfiyyat** ətraflı təhlükəsizlik qiymətləndirilməsinin təməlini təşkil edir.
Bu proses hədəf vebsayt və ya veb tətbiqi haqqında **sistemli** və **dəqiq** şəkildə məlumatların toplanmasını əhatə edir.

Bunu, daha dərin analiz və potensial istismar mərhələsinə başlamazdan öncəki **hazırlıq mərhələsi** kimi təsəvvür edin.

Nəticə etibarilə, Veb Kəşfiyyat **Penetrasiya Testi Prosesinin “Məlumat Toplama” mərhələsinin** ən vacib hissəsidir.

<img width="2058" height="747" alt="image" src="https://github.com/user-attachments/assets/29068b3b-5054-4157-844d-dd611ecc269d" />

**Veb Kəşfiyyatın Əsas Məqsədləri**

* **Aktivlərin müəyyən edilməsi:** Hədəfin bütün açıq resurslarının (veb səhifələr, subdomenlər, IP ünvanlar, istifadə olunan texnologiyalar) aşkarlanması. Bu addım hədəfin onlayn mövcudluğunun tam mənzərəsini təqdim edir.
* **Gizli məlumatların aşkar edilməsi:** Ehtiyatsızlıqla açıq qalan həssas məlumatların (backup faylları, konfiqurasiya faylları, daxili sənədlər) tapılması. Bu cür məlumatlar dəyərli ipucları və hücum üçün giriş nöqtələri yarada bilər.
* **Hücum səthinin təhlili:** Hədəfin istifadə etdiyi texnologiyaların, konfiqurasiyaların və potensial zəifliklərin araşdırılması. Bu, hədəfin zəif nöqtələrinin müəyyən olunmasına kömək edir.
* **Kəşfiyyat məlumatlarının toplanması:** Daha sonra istismar və ya sosial mühəndislik hücumları üçün istifadə edilə biləcək məlumatların toplanması (əsas işçilər, email ünvanları, davranış nümunələri).

Hücumçular bu məlumatlardan hədəfə uyğun hücumlar hazırlamaq üçün istifadə edir, zəiflikləri dəqiqliklə hədəf alır və təhlükəsizlik tədbirlərini aşırlar. Müdafiəçilər isə əksinə, bu üsullar vasitəsilə zəiflikləri vaxtında aşkarlayaraq yamayırlar.

---

## Kəşfiyyatın Növləri

Veb kəşfiyyat iki əsas metodologiyanı əhatə edir:

* **Aktiv kəşfiyyat**
* **Passiv kəşfiyyat**

Hər iki yanaşmanın öz üstünlükləri və çətinlikləri var. Onların fərqlərini anlamaq effektiv məlumat toplama üçün çox vacibdir.

---

### 🔴 Aktiv Kəşfiyyat

Aktiv kəşfiyyatda hücumçu hədəf sistemlə **birbaşa qarşılıqlı əlaqə** quraraq məlumat toplayır. Bu üsul daha çox məlumat versə də, **yüksək aşkarlanma riski** daşıyır.

| Texnika                    | Təsviri                                                    | Nümunə                                              | Alətlər                       | Aşkarlanma Riski |
| -------------------------- | ---------------------------------------------------------- | --------------------------------------------------- | ----------------------------- | ---------------- |
| **Port Scanning**          | Açıq portları və xidmətləri müəyyənləşdirmək               | Nmap ilə 80 (HTTP), 443 (HTTPS) portlarını yoxlamaq | Nmap, Masscan, Unicornscan    | Yüksək           |
| **Vulnerability Scanning** | Zəiflikləri tapmaq (köhnəlmiş proqram, səhv konfiqurasiya) | Nessus ilə SQLi, XSS yoxlamaq                       | Nessus, OpenVAS, Nikto        | Yüksək           |
| **Network Mapping**        | Şəbəkə topologiyasını müəyyənləşdirmək                     | traceroute ilə server yolunu izləmək                | Traceroute, Nmap              | Orta-Yüksək      |
| **Banner Grabbing**        | Xidmətlərin banner məlumatlarını oxumaq                    | HTTP bannerindən server versiyasını öyrənmək        | Netcat, curl                  | Aşağı            |
| **OS Fingerprinting**      | Hədəfin əməliyyat sistemini təyin etmək                    | Nmap `-O` ilə OS aşkar etmək                        | Nmap, Xprobe2                 | Aşağı            |
| **Service Enumeration**    | Xidmətlərin dəqiq versiyalarını müəyyənləşdirmək           | Nmap `-sV` ilə Apache/Nginx versiyasını tapmaq      | Nmap                          | Aşağı            |
| **Web Spidering**          | Saytı crawl edərək gizli resursları tapmaq                 | Burp Suite Spider ilə saytın strukturunu çıxarmaq   | Burp Suite, OWASP ZAP, Scrapy | Aşağı-Orta       |

Aktiv kəşfiyyat hədəfin təhlükəsizlik vəziyyəti haqqında **daha geniş mənzərə** təqdim edir, lakin IDS və firewall-lar tərəfindən asanlıqla aşkarlana bilər.

---

### 🟢 Passiv Kəşfiyyat

Passiv kəşfiyyatda hücumçu **birbaşa hədəflə əlaqə qurmaz**, yalnız açıq mənbələrdən məlumat toplayır. Bu üsul **çox daha gizli** hesab olunur.

| Texnika                   | Təsviri                                   | Nümunə                                     | Alətlər                           | Aşkarlanma Riski |
| ------------------------- | ----------------------------------------- | ------------------------------------------ | --------------------------------- | ---------------- |
| **Search Engine Queries** | Axtarış motorları ilə məlumat toplamaq    | Google-da “Target Name employees” axtarmaq | Google, Shodan, DuckDuckGo        | Çox Aşağı        |
| **WHOIS Lookups**         | Domen qeydiyyat məlumatlarını yoxlamaq    | WHOIS ilə domen sahibinin adını tapmaq     | whois, online lookup              | Çox Aşağı        |
| **DNS Analizi**           | Subdomenləri, MX serverləri aşkar etmək   | dig ilə subdomenlərin tapılması            | dig, nslookup, dnsrecon           | Çox Aşağı        |
| **Web Archive Analysis**  | Saytın keçmiş versiyalarını öyrənmək      | Wayback Machine ilə arxivə baxmaq          | Wayback Machine                   | Çox Aşağı        |
| **Social Media Analysis** | Sosial şəbəkələrdən məlumat toplamaq      | LinkedIn-də hədəf şirkət işçilərini tapmaq | LinkedIn, Twitter, OSINT alətləri | Çox Aşağı        |
| **Code Repositories**     | Açıq repolarda kod və şifrələrin axtarışı | GitHub-da hədəfə aid kod axtarmaq          | GitHub, GitLab                    | Çox Aşağı        |

Passiv kəşfiyyat daha **təhlükəsiz və görünməzdir**, amma toplanan məlumat yalnız açıq resurslarla məhdudlaşır.

---

## Nəticə

Bu modulda veb kəşfiyyat üçün istifadə olunan əsas texnika və alətlərə nəzər salacağıq. Başlanğıc olaraq **WHOIS**-dən başlayırıq. WHOIS protokolunu anlamaq domenlərin qeydiyyat məlumatlarına, sahiblik detalları və infrastruktur barədə əhəmiyyətli bilgilərə çıxış imkanı verir. Bu isə daha inkişaf etmiş kəşfiyyat metodlarına zəmin yaradır.




