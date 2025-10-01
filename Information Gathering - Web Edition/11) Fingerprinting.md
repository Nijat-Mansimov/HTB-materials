Barmaq İzi (Fingerprinting)
Barmaq izi vebsaytın və ya veb tətbiqin işlədiyi texnologiyalar haqqında texniki məlumatları çıxarmağa yönəlib. İnsan barmaq izi kimi unikal olduğu üçün, veb serverlər, əməliyyat sistemləri və proqram komponentlərinin rəqəmsal imzaları hədəfin infrastrukturunu və potensial təhlükəsizlik zəifliklərini ortaya çıxara bilər. Bu bilik hücumçulara müəyyən texnologiyalara xas zəifliklərdən istifadə etməyə və hücumları buna uyğun optimallaşdırmağa imkan verir.

Barmaq izi web kəşfiyyatının təməl daşıdır, çünki:

* **Hədəfə Yönəlik Hücumlar:** İstifadə olunan texnologiyaları bildikdə, hücumçular yalnız həmin sistemlərə xas zəifliklərə fokuslana bilər. Bu, uğurlu kompromisin şansını artırır.
* **Yanlış Konfiqurasiyaların Aşkarlanması:** Barmaq izi köhnəlmiş, səhv konfiqurasiya olunmuş və ya default ayarlara sahib proqramları aşkar edə bilər.
* **Hədəflərin Prioritetləşdirilməsi:** Çoxsaylı potensial hədəflər olduqda, barmaq izi daha zəif və ya dəyərli məlumat saxlayan sistemləri ön plana çıxarmağa kömək edir.
* **Ətraflı Profilin Qurulması:** Barmaq izi nəticələrini digər kəşfiyyat tapıntıları ilə birləşdirmək hədəfin infrastrukturunun tam mənzərəsini yaratmağa kömək edir.

### Barmaq izi Texnikaları

Veb server və texnologiyaların barmaq izini çıxarmaq üçün bir neçə üsul mövcuddur:

* **Banner Toplama (Banner Grabbing):** Veb serverlər və xidmətlər tərəfindən təqdim olunan bannerləri analiz etmək. Bunlar adətən server proqramı və versiyasını göstərir.
* **HTTP Başlıqlarının Analizi:** Hər veb səhifə sorğusu və cavabı ilə gələn HTTP başlıqlarında çoxlu məlumat var. `Server` başlığı server proqramını göstərir, `X-Powered-By` isə əlavə texnologiyaları, məsələn, skript dillərini və framework-ləri göstərə bilər.
* **Xüsusi Cavabları Problamaq:** Hədəfə xüsusi hazırlanmış sorğular göndərərək unikal cavablar almaq, hansı texnologiya və versiyanın istifadə olunduğunu aşkar edə bilər.
* **Səhifə Məzmununu Analiz Etmək:** Veb səhifənin strukturu, skriptləri və elementləri altında işləyən texnologiyalar haqqında ipucları verə bilər. Məsələn, copyright header proqram təminatını göstərə bilər.

### Barmaq izi Alətləri

Aşağıdakı alətlər barmaq izi çıxarmağı avtomatlaşdırır və müxtəlif texnologiyaları, CMS-ləri, əməliyyat sistemlərini aşkar etməyə kömək edir:

| Alət           | Təsvir                                                                  | Xüsusiyyətlər                                                                  |
| -------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| **Wappalyzer** | Veb texnologiya profilini çıxaran brauzer genişlənməsi və onlayn xidmət | CMS, framework, analytics və s. texnologiyaları müəyyənləşdirir                |
| **BuiltWith**  | Veb texnologiya profili                                                 | Ətraflı texnologiya stack hesabatları təqdim edir                              |
| **WhatWeb**    | Komanda xətti aləti                                                     | Geniş imza bazasından istifadə edərək müxtəlif texnologiyaları müəyyənləşdirir |
| **Nmap**       | Şəbəkə skaneri                                                          | Skriptlərlə xidmət və əməliyyat sistemi barmaq izi çıxara bilir                |
| **Netcraft**   | Veb təhlükəsizlik xidmətləri                                            | Texnologiya, hosting provayder və təhlükəsizlik hesabatları təqdim edir        |
| **wafw00f**    | Web Application Firewall (WAF) aşkar edən alət                          | Veb saytın WAF istifadə edib-etmədiyini və konfiqurasiyasını müəyyən edir      |

### inlanefreight.com saytında Barmaq İzi

#### Banner Grabbing

Serverdən birbaşa məlumat toplamaq üçün `curl -I` (və ya `--head`) istifadə olunur, bu HTTP başlıqları əldə edir:

```
nijatmansimov@htb[/htb]$ curl -I inlanefreight.com
HTTP/1.1 301 Moved Permanently
Date: Fri, 31 May 2024 12:07:44 GMT
Server: Apache/2.4.41 (Ubuntu)
Location: https://inlanefreight.com/
Content-Type: text/html; charset=iso-8859-1
```

Server Apache/2.4.41 (Ubuntu) olduğunu göstərir. Sonra HTTPS-ə yönləndirmə baş verir:

```
nijatmansimov@htb[/htb]$ curl -I https://inlanefreight.com
HTTP/1.1 301 Moved Permanently
Server: Apache/2.4.41 (Ubuntu)
X-Redirect-By: WordPress
Location: https://www.inlanefreight.com/
```

Burada WordPress yönləndirməni həyata keçirir.

```
nijatmansimov@htb[/htb]$ curl -I https://www.inlanefreight.com
HTTP/1.1 200 OK
Server: Apache/2.4.41 (Ubuntu)
Link: <https://www.inlanefreight.com/index.php/wp-json/>; rel="https://api.w.org/"
Content-Type: text/html; charset=UTF-8
```

`wp-json` prefiksi WordPress istifadə edildiyini göstərir.

#### WAF Aşkarlanması (wafw00f)

```
nijatmansimov@htb[/htb]$ wafw00f inlanefreight.com
[+] The site https://inlanefreight.com is behind Wordfence (Defiant) WAF.
```

Sayt Wordfence WAF ilə qorunur.

#### Nikto

Nikto veb serveri skan edir və barmaq izi çıxarır:

```
nijatmansimov@htb[/htb]$ nikto -h inlanefreight.com -Tuning b
+ Server: Apache/2.4.41 (Ubuntu)
+ /wp-login.php: WordPress login səhifəsi aşkarlandı
+ SSL Info: TLS_AES_256_GCM_SHA384, issuer: Let's Encrypt
+ /license.txt: potensial məlumat açıqlaması
```

**Əsas nəticələr:**

* IPv4 və IPv6 ünvanları var
* Apache/2.4.41 (Ubuntu) serveri işləyir
* WordPress quraşdırılıb
* Bir neçə təhlükəsizlik zəifliyi və məlumat açıqlamaları mövcuddur (məsələn, `license.txt` və `x-redirect-by` header).
