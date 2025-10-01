WHOIS
WHOIS, qeydiyyatdan keçirilmiş internet resursları haqqında məlumat saxlayan verilənlər bazalarına daxil olmaq üçün nəzərdə tutulmuş geniş istifadə olunan sorğu və cavab protokoludur. Əsasən domen adları ilə əlaqələndirilsə də, WHOIS həmçinin IP ünvan blokları və avtonom sistemlər haqqında məlumat verə bilər. Bunu internet üçün nəhəng bir telefon kitabçası kimi düşünün — müxtəlif onlayn aktivlərin sahibinin və ya məsul şəxslərinin kim olduğunu yoxlamağa imkan verir.

```
WHOIS
nijatmansimov@htb[/htb]$ whois inlanefreight.com

[...]
Domain Name: inlanefreight.com
Registry Domain ID: 2420436757_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.registrar.amazon
Registrar URL: https://registrar.amazon.com
Updated Date: 2023-07-03T01:11:15Z
Creation Date: 2019-08-05T22:43:09Z
[...]
```

Hər WHOIS qeydi adətən aşağıdakı məlumatları ehtiva edir:

* **Domain Name:** Domen adı özü (məsələn, example.com)
* **Registrar:** Domenin qeydiyyatdan keçirildiyi şirkət (məsələn, GoDaddy, Namecheap)
* **Registrant Contact:** Domeni qeydiyyatdan keçirən şəxs və ya təşkilat.
* **Administrative Contact:** Domenin idarə edilməsinə cavabdeh şəxs.
* **Technical Contact:** Domenlə əlaqəli texniki məsələləri idarə edən şəxs.
* **Creation and Expiration Dates:** Domenin qeydiyyat tarixi və müddətinin bitmə tarixi.
* **Name Servers:** Domen adını IP ünvanına çevirən serverlər.

## WHOIS-in Tarixi

WHOIS-in tarixi əhəmiyyətli dərəcədə Elizabeth Feinler-in vizyonu və fədakarlığı ilə bağlıdır — o, erkən internetin formalaşmasında mühüm rol oynayan kompüter alimi idi.

1970-ci illərdə Feinler və Stanford Tədqiqat İnstitutunun Şəbəkə Məlumat Mərkəzində (NIC) işləyən komandası ARPANET-də (müasir internetin ilkin forması) artan sayda şəbəkə resurslarını izləmək və idarə etmək üçün sistemə ehtiyac olduğunu başa düşdülər. Onların həlli WHOIS kataloqunun yaradılması oldu — bu, şəbəkə istifadəçiləri, host adları və domen adları haqqında məlumatları saxlayan sadə, lakin inqilabi verilənlər bazası idi.

(Əgər internet tarixindən maraqlı bir parçanı genişləndirmək istəsəniz, klikləyə bilərsiniz.)

## Niyə WHOIS Veb Kəşfiyyat üçün Vacibdir

WHOIS məlumatları penetrasiya testləri zamanı kəşfiyyat mərhələsində qiymətli məlumat mənbəyi kimi xidmət edir. Bu, hədəf təşkilatın rəqəmsal izinin və potensial zəifliklərinin aşkar edilməsinə kömək edir:

* **Açar şəxslərin müəyyən edilməsi:** WHOIS qeydləri tez-tez domenin idarə edilməsinə cavabdeh şəxslərin adlarını, e-poçt ünvanlarını və telefon nömrələrini göstərir. Bu məlumat sosial mühəndislik hücumları və ya phishing kampaniyaları üçün istifadə oluna bilər.
* **Şəbəkə infrastrukturu haqqında məlumatların aşkar edilməsi:** Name serverlər və IP ünvanlar kimi texniki detallər hədəfin şəbəkə infrastrukturuna dair ipucları verir. Bu, penetrasiya testçilərinə potensial giriş nöqtələri və ya yanlış konfiqurasiyalar barədə kömək edə bilər.
* **Tarixi məlumatların təhlili:** WhoisFreaks kimi xidmətlər vasitəsilə WHOIS-in keçmiş qeydlərinə çıxış domenin sahibliyində, əlaqə məlumatlarında və ya texniki detallarda zamanla baş verən dəyişiklikləri üzə çıxara bilər. Bu, hədəfin rəqəmsal mövcudluğunun necə dəyişdiyini izləmək üçün faydalıdır.
