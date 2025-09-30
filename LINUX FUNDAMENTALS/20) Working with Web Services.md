# Veb Xidmətlərlə İşləmək (Working with Web Services)

Veb inkişafında başqa bir həlledici element, brauzerlər və veb serverlər arasındakı **əlaqədir**. Linux əməliyyat sistemində veb server qurmaq, **Nginx**, **IIS** və **Apache** daxil olmaqla bir neçə yolla edilə bilər. Bunlar arasında, **Apache** ən geniş istifadə olunan veb serverlərdən biridir. Apache-ni vebsaytınızı işə salan, vebsaytınız və ziyarətçiləriniz arasında hamar əlaqəni təmin edən **mühərrik** kimi düşünün.

<img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/99547212-7d22-49b4-a047-d1ec8bcc5e99" />

Apache-ni bir evin təməli və çərçivəsi kimi də düşünə bilərik. Eynilə bir evə müxtəlif otaqlar əlavə edə və ya xüsusiyyətləri fərdiləşdirə bildiyiniz kimi, Apache də hər biri müəyyən bir məqsəd üçün nəzərdə tutulmuş **modullarla** genişləndirilə bilər, istər əlaqəni təmin etmək, istər trafikin istiqamətini dəyişdirmək, istərsə də məzmunu dinamik şəkildə formalaşdırmaq - sanki bir interyer dizayneri ehtiyaclarınıza uyğun otaqları yenidən düzəldir.

Apache-nin əsl gücü onun **modulluğundadır** — o, xüsusi tapşırıqları yerinə yetirmək üçün müxtəlif modullarla fərdiləşdirilə və genişləndirilə bilər. Məsələn, **`mod_ssl`** məlumatları şifrələyərək brauzer və veb server arasındakı əlaqəni təmin edən bir qıfıl qutusu kimi fəaliyyət göstərir. **`mod_proxy`** modulu, xüsusilə proksi serverləri qurarkən, sorğuları düzgün ünvana yönəldən bir yol hərəkəti nəzarətçisi kimidir. **`mod_headers`** və **`mod_rewrite`** kimi digər modullar, brauzer və server arasında hərəkət edən məlumatlara incə nəzarət verir, bu da axının istiqamətini tənzimləmək kimi HTTP başlıqlarını və URL-ləri dərhal dəyişdirməyə imkan verir.

Statik veb məzmunu ilə işləməkdən əlavə, Apache **server tərəfi skript dilləri** vasitəsilə dinamik veb səhifələrin yaradılmasını da dəstəkləyir. Ümumiyyətlə istifadə olunan dillərə **PHP**, **Perl** və **Ruby** daxildir, lakin siz Python, JavaScript, Lua və ya hətta .NET kimi digərlərini də istifadə edə bilərsiniz. Bu skript dilləri, pərdə arxasında yaradıcı alətlər kimi xidmət edir, məzmunu dinamik şəkildə yaradır və vebsaytın interaktiv və həssas olmasını təmin edir.

Əgər hələ etməmisinizsə, gəlin Apache-ni quraq:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install apache2 -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
Suggested packages:
  apache2-doc apache2-suexec-pristine | apache2-suexec-custom
The following NEW packages will be installed:
  apache2
0 upgraded, 1 newly installed, 0 to remove and 17 not upgraded.
Need to get 95,1 kB of archives.
After this operation, 535 kB of additional disk space will be used.
Get:1 http://de.archive.ubuntu.com/ubuntu bionic-updates/main amd64 apache2 amd64 2.4.29-1ubuntu4.13 [95,1 kB]
Fetched 95,1 kB in 0s (270 kB/s)   
<SNIP>
```

İndi, serveri **`apache2ctl`**, **`systemctl`** və ya **`service`** əmrlərindən istifadə edərək başlada bilərik. Həmçinin bir **`apache2`** ikilisi (binary) də mövcuddur, lakin adətən serveri birbaşa başlamaq üçün istifadə edilmir (bu, defolt konfiqurasiyada ətraf mühit dəyişənlərinin istifadəsi ilə əlaqədardır).

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo systemctl start apache2
```

Apache işə salındıqdan sonra, brauzerimizdən **defolt səhifəyə** (**http://localhost**) gedirik. Defolt olaraq, Apache HTTP portu **80** üzərində xidmət göstərəcək və brauzeriniz də HTTP URI daxil etdiyiniz zaman defolt olaraq bu porta keçəcək (əks halda göstərilmədikcə).

Bu, quraşdırmadan sonra defolt səhifədir və veb serverin düzgün işlədiyini təsdiqləmək üçün xidmət edir.

Əgər **Pwnbox** istifadə edirsinizsə, Apache-ni başlamağa çalışarkən bir səhvlə qarşılaşa bilərsiniz; bu, port **80**-in başqa bir xidmət tərəfindən məşğul olması ilə əlaqədardır. Veb serverimiz üçün **alternativ bir port** təyin etmək üçün **`/etc/apache2/ports.conf`** faylını redaktə edə bilərik. Burada, biz onu **8080** portuna qurduq.

Müntəzəm İfadələr

```bash
  GNU nano 2.9.3                                             /etc/apache2/ports.conf
# If you just change the port or add more ports here, you will likely also
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf
Listen 8080

<IfModule ssl_module>
Listen 443
</IfModule>

<IfModule mod_gnutls.c>
Listen 443
</IfModule>

^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line  M-E Redo
```

İndi Apache-ni yenidən başladaraq **`http://localhost:8080`**-a baxa bilərik və ya yoxlamaq üçün **`curl`** kimi əmr sətri alətini istifadə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ curl -I http://localhost:8080
HTTP/1.1 200 OK
Date: Mon, 04 Nov 2024 21:18:50 GMT
Server: Apache/2.4.62 (Debian)
Last-Modified: Mon, 07 Oct 2024 06:39:39 GMT
ETag: "29cd-623dd48f6dd5a"
Accept-Ranges: bytes
Content-Length: 10701
Vary: Accept-Encoding
Content-Type: text/html
```

-----

## Əmr Sətri Alətləri ilə Əlaqə

Veb serverlərlə işləməyin başqa bir vacib aspekti, **`curl`** və **`wget`** kimi əmr sətri alətlərindən istifadə edərək onlarla əlaqə qurmağı öyrənməkdir. Bu alətlər, veb serverdə yerləşdirilmiş bir veb səhifənin məzmununu sistematik şəkildə təhlil etmək istədiyimiz zaman inanılmaz dərəcədə faydalıdır. Onları terminalınız daxilində şəxsi veb brauzerləriniz kimi düşünün, bu da sizə birbaşa əmr sətrindən veb məzmunu əldə etməyə və onunla qarşılıqlı əlaqə qurmağa imkan verir.

Məsələn, bir veb səhifəni yükləyən və onun ehtiva etdiyi bütün URL-ləri çıxaran sadə bir **bash skripti** yaza bilərik. Bu, bir məlumat dənizinə tor atmağa və bizə lazım olan xüsusi keçidləri çıxarmağa bənzəyir. Belə skriptlər **veb skrapinq**, **avtomatlaşdırılmış test** və ya vebsayt dəyişikliklərinin monitorinqi kimi tapşırıqlar üçün güclüdür.

Bununla birlikdə, əsas məqsədimiz hazırda Linux ilə tanış olmaq olduğu üçün, bu cür skriptləri digər modullarda görmək, qurmaq və istifadə etmək imkanınız olacaq. Hələlik, **`curl`** və **`wget`** istifadə edərək bir veb serverlə necə qarşılıqlı əlaqə qura biləcəyimizə diqqət edək.

### cURL

**cURL** bizə **HTTP**, **HTTPS**, **FTP**, **SFTP**, **FTPS** və ya **SCP** kimi protokollar üzərindən qabıqdan (**shell**) faylları köçürməyə imkan verən bir alətdir və ümumiyyətlə, əmr sətri vasitəsilə vebsaytları uzaqdan idarə etmək və test etmək imkanı verir. Uzaq serverlərin məzmunundan əlavə, müştəri və serverin əlaqəsinə baxmaq üçün fərdi sorğulara da baxa bilərik. Adətən, cURL əksər Linux sistemlərində artıq quraşdırılmışdır. Bu, daha sonra bəzi prosesləri daha da asanlaşdıra biləcəyi üçün bu alətlə tanış olmağımız üçün başqa bir kritik səbəbdir.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ curl http://localhost
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <title>Apache2 Ubuntu Default Page: It works</title>
    <style type="text/css" media="screen">
...SNIP...
```

Başlıq teqində, brauzerimizdəki ilə eyni mətni görə bilərik. Bu, bizə vebsaytın **mənbə kodunu** yoxlamağa və ondan məlumat almağa imkan verir. Daha dəqiq desək, `curl` vebsaytın səhifə mənbəyini **STDOUT** kimi qaytarır. Vizual, estetik vebsaytlar yaratmaq üçün HTML, CSS və Javascript-i göstərən bir brauzerlə vebsayta baxmaqdan fərqli olaraq. Buna baxmayaraq, başqa bir modulda bu mövzuya qayıdacağıq.

### Wget

**`curl`**-a alternativ olaraq **`wget`** aləti var. Bu alətlə **FTP** və ya **HTTP** serverlərindən faylları birbaşa terminaldan yükləyə bilərik və o, möhkəm bir yükləmə meneceri kimi xidmət edir. Əgər **`wget`**-dən eyni şəkildə istifadə etsək, **`curl`**-dan fərqi, vebsayt məzmununun yüklənməsi və **yerli olaraq saxlanmasıdır**, aşağıdakı nümunədə göstərildiyi kimi.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ wget http://localhost
--2020-05-15 17:43:52--  http://localhost/
Resolving localhost (localhost)... 127.0.0.1
Connecting to localhost (localhost)|127.0.0.1|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 10918 (11K) [text/html]
Saving to: 'index.html'

index.html                 100%[=======================================>]  10,66K  --.-KB/s    in 0s      

2020-05-15 17:43:52 (33,0 MB/s) - ‘index.html’ saved [10918/10918]
```

### Python 3

Məlumat köçürməsinə gəldikdə tez-tez istifadə olunan başqa bir seçim **Python 3**-ün istifadəsidir. Bu vəziyyətdə, veb serverin kök qovluğu, serveri başlamaq üçün əmrin icra edildiyi yerdir. Bu nümunə üçün, WordPress-in quraşdırıldığı və "readme.html" ehtiva edən bir qovluqdayıq. İndi, gəlin Python 3 veb serverini başladaq və brauzer istifadə edərək ona daxil ola biləcəyimizi görək.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

Əgər indi Python 3 veb serverimizin hadisələrinə baxsaq, hansı sorğuların edildiyini görə bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ python3 -m http.server
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
127.0.0.1 - - [15/May/2020 17:56:29] "GET /readme.html HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/css/install.css?ver=20100228 HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.png HTTP/1.1" 200 -
127.0.0.1 - - [15/May/2020 17:56:29] "GET /wp-admin/images/wordpress-logo.svg?ver=20131107 HTTP/1.1" 200 -
```

Penetrasiya testində, biz tez-tez **yaradıcı problem həlli** və **qeyri-standart düşüncə** tələb edən çətinliklərlə qarşılaşırıq. Əvvəllər məşğul olmadığınız ssenarilərlə qarşılaşacaqsınız, bu da yalnız yeni bir şey öyrənmək deyil, həm də tədqiqat və innovativ düşüncə yolu ilə özünüzə həll yollarını tapmaq deməkdir.

Unutmayın, bu, bir imtahan deyil, bir **öyrənmə prosesidir**. Öz araşdırmanızı aparmaq və fərqli yanaşmaları araşdırmaq, bacarıq dəstinizi genişləndirmək üçün vacibdir. Əslində, bu səylər sahədəki təcrübənizi və uyğunlaşma qabiliyyətinizi qurmaqda əsas rol oynayacaq. Bu nöqtədən etibarən, qarşılaşacağınız tapşırıqlar bilərəkdən sizi rahatlıq zonasından kənara itələyəcəkdir. Bu, sizin öyrənməyinizi sürətləndirmək və daha sürətli inkişaf etməyinizə kömək etmək üçün nəzərdə tutulmuşdur.

Bu çətinliklərlə üzləşdikcə, tez-tez tək bir həll yolu olmayan **real dünya vəziyyətlərinin** öhdəsindən gəlmək üçün lazım olan bacarıqları inkişaf etdirəcəksiniz. Kəşf və öyrənmənin bu prosesini qəbul edin, çünki bu, inkişaf etməyin ən yaxşı yoludur.
