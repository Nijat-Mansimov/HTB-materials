DNS
Domen Ad Sistemi (DNS) internetin GPS-i kimi işləyir, onlayn səyahətinizi yadda qalan yerlərdən (domen adları) dəqiq rəqəmsal koordinatlara (IP ünvanlarına) yönləndirir. GPS-in istiqamət üçün ünvan adını enlik və uzunluq koordinatlarına çevirməsi kimi, DNS də insan tərəfindən oxuna bilən domen adlarını (məsələn, [www.example.com](http://www.example.com)) kompüterlərin ünsiyyət üçün istifadə etdiyi rəqəmsal IP ünvanlarına (məsələn, 192.0.2.1) çevirir.

Təsəvvür edin ki, şəhəri gəzmək üçün hər yerin dəqiq enlik və uzunluğunu əzbərləyirsiniz. Bu, olduqca çətin və səmərəsiz olardı. DNS bu mürəkkəbliyi aradan qaldırır və bizə yadda saxlamaq asan domen adlarından istifadə etməyə imkan verir. Brauzerinizə domen adını daxil etdiyiniz zaman DNS sizin navigatorunuz kimi fəaliyyət göstərir, müvafiq IP ünvanını tez tapır və sorğunuzu internetdə düzgün ünvana yönləndirir.

DNS olmadan onlayn dünyada hərəkət etmək xəritə və GPS olmadan sürücülük etmək kimi olardı – bu, çox çətin və səhvlərə meylli olardı.

**DNS necə işləyir**
Məsələn, [www.example.com](http://www.example.com) saytına daxil olmaq istəyirsiniz. Brauzerinizə bu dost domen adını daxil edirsiniz, amma kompüteriniz sözləri anlamır – o, rəqəmlərin dilində danışır, yəni IP ünvanları ilə. Beləliklə, kompüteriniz saytın IP ünvanını necə tapır? Burada DNS – internetin etibarlı tərcüməçisi – işə düşür.

1. **Kompüteriniz istiqamət soruşur (DNS Query):** Domen adını daxil etdikdə, kompüter əvvəlcə yaddaşında (cache) əvvəlki səfərlərdən IP ünvanını xatırlayır. Əgər xatırlamırsa, internet provayderinizin təmin etdiyi DNS resolverinə sorğu göndərir.

2. **DNS Resolver xəritəsini yoxlayır (Recursive Lookup):** Resolverin də öz cache-i var və IP tapmazsa, DNS iyerarxiyası boyunca səyahətə başlayır. O, root name server-ə sorğu göndərir, bu da internet kitabxanaçısı kimi fəaliyyət göstərir.

3. **Root Name Server istiqamət göstərir:** Root server dəqiq ünvana sahib olmasa da, bilən serveri göstərir – Top-Level Domain (TLD) name server.

4. **TLD Name Server daraldır:** TLD serveri regional xəritə kimidir. O, spesifik domen üçün (məsələn, example.com) səlahiyyətli name serveri bilir və resolveri ora yönləndirir.

5. **Səlahiyyətli Name Server ünvanı verir:** Səlahiyyətli name server son dayanacaqdır. O, doğru IP ünvanını saxlayır və resolverə geri göndərir.

6. **DNS Resolver məlumatı qaytarır:** Resolver IP ünvanını alır və kompüterinizə verir. Eyni zamanda bu məlumatı bir müddət yaddaşda saxlayır (cache).

7. **Kompüteriniz əlaqə yaradır:** Kompüter IP ünvanını bildiyi üçün veb serverə birbaşa qoşulur və brauzinqə başlayır.

**Hosts faylı**
Hosts faylı host adlarını IP ünvanlarına qoşmaq üçün istifadə olunan sadə bir mətn faylıdır. Bu, DNS prosesini atlayaraq domen adlarının yerli həllini təmin edir. Hosts faylı inkişaf, problemlərin həlli və ya saytların bloklanması üçün faydalı ola bilər.

* Windows: `C:\Windows\System32\drivers\etc\hosts`
* Linux/MacOS: `/etc/hosts`

Hər sətrin formatı belədir:

```
<IP Ünvanı>    <Hostname> [<Alias> ...]
```

Məsələn:

```
127.0.0.1       localhost
192.168.1.10    devserver.local
```

Hosts faylını redaktə etmək üçün mətn redaktorundan administrator/root hüquqları ilə istifadə edin. Dəyişikliklər dərhal qüvvəyə minir.

**Tez-tez istifadə halları:**

* Domeni lokal serverə yönləndirmək:

```
127.0.0.1       myapp.local
```

* IP ünvanla bağlantını test etmək:

```
192.168.1.20    testserver.local
```

* İstənməyən saytları bloklamaq:

```
0.0.0.0       unwanted-site.com
```

**DNS bir relay yarışı kimidir**
Kompüter domen adını alır və resolverə ötürür. Resolver root server, TLD server və səlahiyyətli serverə sorğunu ötürür. IP ünvanı tapıldıqdan sonra yenidən kompüterinizə çatdırılır və sayt açılır.

**Əsas DNS anlayışları**

* **Zona:** Domen ad məkanının müəyyən bir hissəsi və idarə edən şəxs/təşkilat tərəfindən idarə olunur.
* **Zona faylı:** DNS serverində yerləşən mətn faylıdır, domen adlarını IP ünvanlarına çevirmək üçün resurs qeydlərini (resource records) müəyyən edir.

**DNS resurs qeydləri və nümunələri:**

| Tip   | Tam adı               | Təsviri                                             | Nümunə (zone faylı)                                                                        |
| ----- | --------------------- | --------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| A     | Address Record        | Hostname-i IPv4 ünvanına qoşur                      | [www.example.com](http://www.example.com). IN A 192.0.2.1                                  |
| AAAA  | IPv6 Address Record   | Hostname-i IPv6 ünvanına qoşur                      | [www.example.com](http://www.example.com). IN AAAA 2001:db8::1                             |
| CNAME | Canonical Name Record | Hostname üçün alias yaradır                         | blog.example.com. IN CNAME webserver.example.net.                                          |
| MX    | Mail Exchange Record  | Domen üçün mail serverləri təyin edir               | example.com. IN MX 10 mail.example.com.                                                    |
| NS    | Name Server Record    | DNS zonasını səlahiyyətli name server-ə təhkim edir | example.com. IN NS ns1.example.com.                                                        |
| TXT   | Text Record           | Arbitrary mətn saxlayır, SPF və s.                  | example.com. IN TXT "v=spf1 mx -all"                                                       |
| SOA   | Start of Authority    | Zona və administrativ məlumatlar                    | example.com. IN SOA ns1.example.com. admin.example.com. 2024060301 10800 3600 604800 86400 |
| SRV   | Service Record        | Xidmət host və port nömrəsi                         | _sip._udp.example.com. IN SRV 10 5 5060 sipserver.example.com.                             |
| PTR   | Pointer Record        | Reverse DNS, IP-dən hostname                        | 1.2.0.192.in-addr.arpa. IN PTR [www.example.com](http://www.example.com).                  |

**“IN”** internet protokol ailəsini göstərir və deməkdir ki, qeyd internet protokollarına aiddir.

**DNS-nin web kəşfiyyatı üçün əhəmiyyəti**

* **Resursları aşkarlamaq:** Subdomenlər, mail serverləri, ad server qeydləri.
* **Şəbəkə infrastrukturunun xəritələnməsi:** NS və A qeydlərindən istifadə edərək hosting provayderləri və trafikin axını müəyyənləşdirilir.
* **Dəyişiklikləri izləmək:** Yeni subdomenlər və TXT qeydləri yeni giriş nöqtələri və ya təhlükəsizlik məlumatları barədə xəbər verir.
