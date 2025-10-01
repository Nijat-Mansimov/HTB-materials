Virtual Hosts
DNS trafiki düzgün serverə yönəltdikdən sonra daxil olan sorğuların necə emal ediləcəyini müəyyən etmək üçün veb server konfiqurasiyası vacib olur. Apache, Nginx və ya IIS kimi veb serverlər bir serverdə bir neçə vebsayt və ya tətbiq yerləşdirməyə imkan verəcək şəkildə dizayn olunmuşdur. Onlar bunu virtual hosting vasitəsilə həyata keçirirlər ki, bu da onlara domenlər, alt domenlər və ya fərqli məzmunlu ayrıca vebsaytlar arasında fərqləndirməyə imkan verir.

Virtual Hostlar Necə İşləyir: VHostlar və Alt Domenləri Anlamaq
Virtual hosting-in əsas prinsipi veb serverlərin eyni IP ünvanını paylaşan bir neçə vebsayt və ya tətbiqi ayırd edə bilməsidir. Bu, hər bir veb brauzer tərəfindən göndərilən hər HTTP sorğusunda daxil olan **Host** başlığından istifadə etməklə əldə edilir.

VHostlar ilə alt domenlər arasındakı əsas fərq onların Domain Name System (DNS) və veb serverin konfiqurasiyası ilə əlaqəsidir.

* **Alt Domenlər:** Bunlar əsas domen adının genişlənməsidir (məsələn, `blog.example.com` `example.com`-un alt domenidir). Alt domenlərin adətən öz DNS qeydləri olur və bunlar əsas domenlə eyni IP-ə və ya fərqli bir IP-ə işarə edə bilər. Onlar vebsaytın müxtəlif bölmələrini və ya xidmətlərini təşkil etmək üçün istifadə edilə bilər.
* **Virtual Hostlar (VHostlar):** Virtual hostlar veb server daxilində konfiqurasiyalardır və bir serverdə bir neçə vebsayt və ya tətbiqin yerləşdirilməsinə imkan verir. Onlar top-level domenlərlə (məsələn, `example.com`) və ya alt domenlərlə (məsələn, `dev.example.com`) əlaqələndirilə bilər. Hər bir virtual hostun öz ayrıca konfiqurasiyası ola bilər və bu, sorğuların necə idarə olunmasına dəqiq nəzarət etməyə imkan verir.

Əgər bir virtual hostun DNS qeydi yoxdursa, onu lokal maşınızdakı hosts faylını düzəldərək hələ də əldə etmək olar. Hosts faylı domen adını əl ilə bir IP ünvanına xəritələməyə imkan verir və DNS həllini atlaya bilər.

Vebsaytların tez-tez DNS qeydlərində görünməyən və ictimaiyyət üçün açıq olmayan alt domenləri olur. Bu alt domenlər yalnız daxili şəkildə və ya xüsusi konfiqurasiyalar vasitəsilə əlçatan ola bilər. VHost fuzzing adlı texnika məlum bir IP ünvanına müxtəlif host adları sınayaraq ictimai və qeyri-ictimai alt domenləri və VHostları aşkar etmək üçün istifadə olunur.

Virtual hostlar yalnız alt domenlərdən deyil, fərqli domenlərdən də istifadə edəcək şəkildə konfiqurasiya edilə bilər. Məsələn:

```apacheconf
# Example of name-based virtual host configuration in Apache
<VirtualHost *:80>
    ServerName www.example1.com
    DocumentRoot /var/www/example1
</VirtualHost>

<VirtualHost *:80>
    ServerName www.example2.org
    DocumentRoot /var/www/example2
</VirtualHost>

<VirtualHost *:80>
    ServerName www.another-example.net
    DocumentRoot /var/www/another-example
</VirtualHost>
```

Burada `example1.com`, `example2.org` və `another-example.net` eyni serverdə yerləşdirilən ayrı domenlərdir. Veb server soruşulan domen adına əsaslanaraq düzgün məzmunu təqdim etmək üçün Host başlığından istifadə edir.

Server VHost Axtarışı
Aşağıda veb serverin Host başlığına əsaslanaraq düzgün məzmunu necə müəyyən etdiyini göstərən proses təsvir edilir:


<img width="2613" height="1632" alt="image" src="https://github.com/user-attachments/assets/fa97f83a-42b7-4fb6-8eaf-7f9ba74dd878" />

Brauzer Vebsayta Sorğu Göndərir: Brauzerinizə bir domen adı (məsələn, [www.inlanefreight.com](http://www.inlanefreight.com)) daxil etdiyiniz zaman o, həmin domenin IP ünvanı ilə əlaqələndirilmiş veb serverə HTTP sorğusu göndərir.
Host Başlığı Domeni Aşkar Edir: Brauzer sorğuda domen adını Host başlığında daxil edir; bu başlıq veb serverə hansı vebsaytın tələb olunduğunu bildirən bir etikettir.
Veb Server Virtual Host-u Müəyyənləşdirir: Veb server sorğunu qəbul edir, Host başlığını yoxlayır və tələb olunan domen adı üçün uyğun giriş tapmaq məqsədilə virtual host konfiqurasiyasına müraciət edir.
Düzgün Məzmunun Xidmət Edilməsi: Düzgün virtual host konfiqurasiyası müəyyən olunduqda, veb server həmin vebsayta aid sənədləri və resursları öz document root-dan götürərək brauzerə HTTP cavabı kimi göndərir.
Əsasən, Host başlığı bir keçid funksiyasını yerinə yetirir və veb serverin brauzer tərəfindən tələb olunan domen adına əsaslanaraq dinamik şəkildə hansı vebsaytı təqdim edəcəyini müəyyənləşdirməsinə imkan verir.

### Virtual Hosting Növləri

Üç əsas virtual hosting növü var və hər birinin öz üstünlükləri və çatışmazlıqları mövcuddur:

* **Ad-əsaslı Virtual Hosting (Name-Based):** Bu üsul yalnız HTTP Host başlığına əsaslanaraq vebsaytları fərqləndirir. Ən yayılmış və çevik metoddur, çünki bir neçə IP ünvanına ehtiyac olmur. Xərci azdır, qurulması asandır və müasir veb serverlərin əksəriyyəti tərəfindən dəstəklənir. Lakin veb serverin name-based virtual hosting-i dəstəkləməsi tələb olunur və SSL/TLS kimi bəzi protokollarla məhdudiyyətlər ola bilər.
* **IP-əsaslı Virtual Hosting (IP-Based):** Bu növ hostinq hər vebsayta unikal IP ünvanı təyin edir. Server sorğu göndərilən IP ünvanına əsaslanaraq hansı vebsaytı təqdim edəcəyini müəyyən edir. Host header-a asılı deyil, hər hansı protokolla istifadə oluna bilər və saytlar arasında daha yaxşı ayrı-seçkilik təmin edir. Lakin bir neçə IP ünvanına ehtiyac olduğu üçün bu üsul baha və miqyaslanması çətin ola bilər.
* **Port-əsaslı Virtual Hosting (Port-Based):** Müxtəlif vebsaytlar eyni IP üzərində fərqli portlarla əlaqələndirilir. Məsələn, bir sayt 80 portunda, digər sayt 8080 portunda ola bilər. Port-əsaslı virtual hosting IP ünvanları məhdud olduqda istifadə edilə bilər, lakin adı-əsaslı virtual hosting qədər yayılmış və istifadəçi üçün rahat deyil; URL-də port nömrəsinin göstərilməsini tələb edə bilər.

### Virtual Host Aşkarlama Vasitələri

HTTP başlıqlarının manual təhlili və tərs DNS yoxlamaları effektiv ola bilər, lakin ixtisaslaşmış virtual host aşkar etmə vasitələri prosesi avtomatlaşdırır və daha geniş əhatəli edir. Bu vasitələr hədəf serveri sınaqdan keçirmək və potensial virtual hostları üzə çıxartmaq üçün müxtəlif texnikalardan istifadə edir.

Aşağıdakı alətlər virtual host aşkarlığında kömək edir:

| Alət            | Təsvir                                                                                                                         | Xüsusiyyətlər                                                                          |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| **gobuster**    | Çoxməqsədli alət; direktoriyaların/faylların brute-force üçün geniş istifadə olunur və virtual host aşkarlamada da təsirlidir. | Sürətli, bir neçə HTTP metodu dəstəkləyir, xüsusi söz siyahılarını istifadə edə bilir. |
| **Feroxbuster** | Gobuster-ə bənzər, lakin Rust ilə yazılmışdır; sürəti və çevikliyi ilə tanınır.                                                | Rekursiya dəstəyi, wildcard aşkarlama, müxtəlif filterlər.                             |
| **ffuf**        | Host başlığını fuzzing edərək virtual host aşkarlama üçün istifadə oluna bilən sürətli web fuzzer.                             | Fərdiləşdirilə bilən söz siyahısı girişi və filter seçimləri.                          |

### gobuster

Gobuster direktoriyaların və faylların brute-forcinqi üçün geniş istifadə olunan çevik alətdir, lakin virtual host aşkarlamada da mükəmməldir. O, müəyyən IP ünvanına müxtəlif Host başlıqları ilə HTTP sorğuları sistematik şəkildə göndərir və cavabları təhlil edərək etibarlı virtual hostları müəyyənləşdirir.

Host başlıqlarını brute-force etmək üçün hazırlamalı olduğunuz bir neçə şey var:

* **Hədəfin Müəyyənləşdirilməsi:** İlk öncə hədəf veb serverin IP ünvanını müəyyənləşdirin. Bu DNS sorguları və ya digər kəşfiyyat texnikaları ilə edilə bilər.
* **Söz Siyahısının Hazırlanması:** Potensial virtual host adlarını ehtiva edən söz siyahısı hazırlayın. SecLists kimi əvvəlcədən hazırlanmış söz siyahısından istifadə edə bilərsiniz və ya hədəfin sənayesi, adlandırma konvensiyaları və s. əsasında öz xüsusi siyahınızı yaradın.

gobuster ilə vhost brute-force əmri ümumiyyətlə belə görünür:

```
Virtual Hosts
nijatmansimov@htb[/htb]$ gobuster vhost -u http://<target_IP_address> -w <wordlist_file> --append-domain
```

* `-u` flaqı hədəf URL-i göstərir ( `<target_IP_address>` yerinə həqiqi IP-ni yazın).
* `-w` flaqı söz siyahısı faylını göstərir ( `<wordlist_file>` yerinə söz siyahısının yolunu yazın).
* `--append-domain` flaqı hər sözü əsas domenə əlavə edir.

Gobuster-in yeni versiyalarında `--append-domain` flaqı vhost aşkar edərkən hər sözü əsas domenə əlavə etmək üçün tələb olunur. Bu flaq Gobuster-in hər sözdən düzgün virtual host adları yaratmasını təmin edir, bu isə potensial alt domenlərin dəqiq enumerasiyası üçün vacibdir. Köhnə versiyalarda bu funksiya fərqli şəkildə həyata keçirilirdi və `--append-domain` flaqı lazım olmayıb və ya mövcud olmaya bilər; alət ya əsasında domeni avtomatik əlavə edirdi, ya da virtual host yaratmaq üçün başqa mexanizm istifadə edirdi.

Gobuster tapdığı potensial virtual hostları çıxışa verəcək. Nəticələri diqqətlə təhlil edin və aşkar olunan virtual hostların mövcudluğunu və funksionallığını təsdiqləmək üçün əlavə araşdırma aparın.

Bilməyə dəyər bir neçə əlavə arqument:

* `-t` flaqından istifadə edərək skanı daha sürətli etmək üçün thread sayını artıra bilərsiniz.
* `-k` flaqı SSL/TLS sertifikat səhvlərini görməzdən gəlməyə imkan verir.
* `-o` flaqı çıxışı fayla yazmaq üçün istifadə olunur.

```
Virtual Hosts
nijatmansimov@htb[/htb]$ gobuster vhost -u http://inlanefreight.htb:81 -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:             http://inlanefreight.htb:81
[+] Method:          GET
[+] Threads:         10
[+] Wordlist:        /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
[+] User Agent:      gobuster/3.6
[+] Timeout:         10s
[+] Append Domain:   true
===============================================================
Starting gobuster in VHOST enumeration mode
===============================================================
Found: forum.inlanefreight.htb:81 Status: 200 [Size: 100]
[...]
Progress: 114441 / 114442 (100.00%)
===============================================================
Finished
===============================================================
```





