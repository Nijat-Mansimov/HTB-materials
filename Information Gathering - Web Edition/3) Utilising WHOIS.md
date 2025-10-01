WHOIS istifadə etmək
Gəlin WHOIS məlumatlarının dəyərini göstərmək üçün üç ssenarini nəzərdən keçirək.

Ssenari 1: Fişinq təhqiqatı
Bir şirkətdə bir neçə işçiyə göndərilmiş şübhəli e-poçt eyni zamanda e-mail təhlükəsizlik qapısı tərəfindən işarələnir. E-poçtda şirkətin bankından göndərildiyi iddia olunur və alıcılardan hesab məlumatlarını yeniləmək üçün linkə klikləmələri xahiş olunur. Təhlükəsizlik analitiki e-poçtu araşdırır və e-poçtda göstərilən domen üzərində WHOIS axtarışı ilə başlayır.

WHOIS qeydi aşağıdakıları göstərir:

* Qeydiyyat tarixi: Domen bir neçə gün əvvəl qeydiyyatdan keçirilib.
* Qeydiyyatçı: Qeydiyyatçının məlumatları məxfilik xidməti arxasında gizlədilib.
* Ad serverləri: Ad serverləri zərərli fəaliyyətlər üçün tez-tez istifadə olunan məlum “bulletproof” hosting provayderi ilə əlaqəlidir.

Bu amalların kombinasiyası analitik üçün ciddi qırmızı bayraqlar qaldırır. Yeni qeydiyyat tarixi, gizlədilmiş qeydiyyatçı məlumatları və şübhəli hosting fişinq kampaniyasını güclü şəkildə göstərir. Analitik dərhal şirkətin IT şöbəsini domeni bloklamaq barədə xəbərdar edir və işçilərə bu saxtakarlıq barədə xəbərdarlıq edir.

Hosting provayderinə və əlaqəli IP ünvanlarına dair əlavə araşdırma təhlükə aktoru tərəfindən istifadə olunan əlavə fişinq domenlərini və ya infrastrukturunu üzə çıxara bilər.

Ssenari 2: Zərərli proqramların analizi
Bir təhlükəsizlik tədqiqatçısı şəbəkədə bir neçə sistemi yoluxduran yeni zərərli proqram nümunəsini təhlil edir. Zərərli proqram komandaları almaq və oğurlanan məlumatları xaricə ötürmək üçün uzaq serverlə əlaqə qurur. Təhlükə aktorunun infrastrukturuna dair məlumat əldə etmək üçün tədqiqatçı C2 (command-and-control) serveri ilə əlaqəli domen üzərində WHOIS axtarışı aparır.

WHOIS qeydi aşağıdakıları göstərir:

* Qeydiyyatçı: Domen anonimlik üçün tanınan pulsuz e-poçt xidmətindən istifadə edən fərdi şəxsə qeydiyyatdan keçirilib.
* Məkan: Qeydiyyatçının ünvanı kibercinayətin geniş yayıldığı ölkədə yerləşir.
* Qeydiyyatçı şirkət (Registrar): Domen zəif sui-istifadə siyasəti olan bir registrar vasitəsilə qeydiyyatdan keçirilib.

Bu məlumatlara əsaslanaraq, tədqiqatçı C2 serverinin ehtimal ki, kompromatlaşdırılmış və ya “bulletproof” (sui-istifadəyə davamlı) serverdə yerləşdirildiyini qənaətinə gəlir. Tədqiqatçı daha sonra hosting provayderini müəyyənləşdirmək və zərərli fəaliyyət barədə onlara xəbər vermək üçün WHOIS məlumatlarından istifadə edir.

Ssenari 3: Təhlükə kəşfiyyatı hesabatı
Bir kibertəhlükəsizlik şirkəti maliyyə institutlarını hədəf alan məşhur və peşəkar bir təhlükə aktor qrupunun fəaliyyətlərini izləyir. Analitiklər qrupun keçmiş kampaniyalarına aid bir neçə domen üzərində WHOIS məlumatları toplayaraq geniş təhlükə kəşfiyyatı hesabatı hazırlaşdırırlar.

WHOIS qeydlərini təhlil etməklə analitiklər aşağıdakı naxışları aşkar edirlər:

* Qeydiyyat tarixləri: Domenlər çox zaman böyük hücumlardan qısa müddət əvvəl qruplar şəklində qeydiyyatdan keçirilir.
* Qeydiyyatçılar: Qeydiyyatçılar müxtəlif ləqəblər və saxta identliklərdən istifadə edirlər.
* Ad serverləri: Domenlər tez-tez eyni ad serverlərini paylaşır, bu isə ümumi infrastrukturu göstərir.
* Silinmə tarixi: Bir çox domen hücumlardan sonra götürülüb, bu da əvvəlki hüquq-mühafizə və ya təhlükəsizlik müdaxilələrini göstərir.

Bu məlumatlar analitiklərə təhlükə aktorunun taktikaları, texnikaları və prosedurları (TTP) barədə ətraflı profil yaratmağa imkan verir. Hesabat WHOIS məlumatlarına əsaslanan kompromat göstəricilərini (IOC) daxil edir ki, digər təşkilatlar gələcək hücumları aşkar etmək və bloklamaq üçün bunlardan istifadə edə bilsinlər.

WHOIS-dən istifadə etmək
whois əmrindən istifadə etməzdən əvvəl onun Linux sisteminizdə quraşdırıldığından əmin olmalısınız. Bu utilit linux paket menecerləri vasitəsilə mövcuddur və quraşdırılmayıbsa, sadəcə olaraq aşağıdakı əmrlərlə qurula bilər:

```
nijatmansimov@htb[/htb]$ sudo apt update
nijatmansimov@htb[/htb]$ sudo apt install whois -y
```

WHOIS məlumatlarına çıxışın ən sadə yolu whois əmr xətti alətindən istifadə etməkdir. Gəlin facebook.com üçün WHOIS axtarışı edək:

```
nijatmansimov@htb[/htb]$ whois facebook.com

   Domain Name: FACEBOOK.COM
   Registry Domain ID: 2320948_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.registrarsafe.com
   Registrar URL: http://www.registrarsafe.com
   Updated Date: 2024-04-24T19:06:12Z
   Creation Date: 1997-03-29T05:00:00Z
   Registry Expiry Date: 2033-03-30T04:00:00Z
   Registrar: RegistrarSafe, LLC
   Registrar IANA ID: 3237
   Registrar Abuse Contact Email: abusecomplaints@registrarsafe.com
   Registrar Abuse Contact Phone: +1-650-308-7004
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
   Name Server: A.NS.FACEBOOK.COM
   Name Server: B.NS.FACEBOOK.COM
   Name Server: C.NS.FACEBOOK.COM
   Name Server: D.NS.FACEBOOK.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2024-06-01T11:24:10Z <<<
```

```
Registry Registrant ID:
Registrant Name: Domain Admin
Registrant Organization: Meta Platforms, Inc.
```

facebook.com üçün WHOIS çıxışı bir neçə əsas detala işıq tutur:

**Domenin qeydiyyatı:**

* Registrar: RegistrarSafe, LLC
* Yaradılma tarixi: 1997-03-29
* Bitmə tarixi: 2033-03-30

Bu məlumatlar domenin RegistrarSafe, LLC vasitəsilə qeydiyyatdan keçirildiyini və uzun müddətdir aktiv olduğunu göstərir ki, bu da onun etibarlı və möhkəm onlayn mövcudluğunu göstərir. Uzaq bitmə tarixi onun uzunömürlülüyünü daha da təsdiqləyir.

**Domen sahibi:**

* Registrant/Admin/Tech Organization: Meta Platforms, Inc.
* Registrant/Admin/Tech Contact: Domain Admin

Bu məlumat facebook.com-un arxasında Meta Platforms, Inc.-in təşkilat kimi dayandığını və “Domain Admin”in domenlə əlaqəli məsələlər üçün əlaqə nöqtəsi olduğunu göstərir.

**Domen vəziyyəti:**

* clientDeleteProhibited, clientTransferProhibited, clientUpdateProhibited, serverDeleteProhibited, serverTransferProhibited, serverUpdateProhibited

Bu statuslar domenin həm client, həm də server tərəfdən icazəsiz dəyişikliklərə, köçürmələrə və ya silinmələrə qarşı qorunduğunu göstərir. Bu, domen üzərində təhlükəsizlik və nəzarətin yüksək olduğunu vurğulayır.

**Ad serverləri:**

* A.NS.FACEBOOK.COM, B.NS.FACEBOOK.COM, C.NS.FACEBOOK.COM, D.NS.FACEBOOK.COM

Bu ad serverlər facebook.com domeni daxilindədir və bu, Meta Platforms, Inc.-in DNS infrastrukturunu idarə etdiyini göstərir. Böyük təşkilatlar üçün DNS həllini idarə etmək və etibarlılığı təmin etmək üçün bu ümumi praktikadır.

Ümumilikdə, facebook.com üçün WHOIS çıxışı Meta Platforms, Inc. kimi böyük təşkilat tərəfindən idarə olunan, etibarlı və yaxşı qorunan bir domen üçün gözlənilənlə uyğun gəlir.

WHOIS qeydi domenlə bağlı əlaqə məlumatı təqdim etsə də, bu məlumat fərdi işçiləri və ya spesifik zəiflikləri birbaşa müəyyən etməkdə həmişə köməkçi olmaya bilər. Bu, WHOIS məlumatlarını digər kəşfiyyat texnikaları ilə birləşdirərək hədəfin rəqəmsal izini tam başa düşməyin vacibliyini göstərir.
