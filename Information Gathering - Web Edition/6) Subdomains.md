**Alt Domenlər**

DNS qeydlərini araşdırarkən əsasən əsas domenə (məsələn, example.com) və onunla bağlı məlumatlara diqqət yetirmişik. Lakin bu əsas domenin altında potensial alt domenlər şəbəkəsi mövcuddur. Bu alt domenlər əsas domenin genişlənmələri olub, tez-tez veb saytın müxtəlif bölmələrini və funksionallıqlarını təşkil etmək və ayırmaq üçün yaradılır. Məsələn, bir şirkət blog üçün `blog.example.com`, onlayn mağaza üçün `shop.example.com` və ya e-poçt xidmətləri üçün `mail.example.com` istifadə edə bilər.

---

### **Veb Kəşfiyyat üçün Önəmi**

Alt domenlər tez-tez əsas veb saytla birbaşa əlaqəsi olmayan qiymətli məlumatlar və resurslar saxlayır. Bunlara aşağıdakılar daxil ola bilər:

* **İnkişaf və Sınaq Mühitləri:** Şirkətlər tez-tez yeni xüsusiyyətləri və yeniləmələri əsas sayta yerləşdirmədən əvvəl test etmək üçün alt domenlərdən istifadə edirlər. Bu mühitlərdə təhlükəsizlik tədbirləri daha az ola bilər və bəzən zəifliklər və ya həssas məlumatlar ifşa ola bilər.
* **Gizli Giriş Panelləri:** Alt domenlər inzibati panelləri və ya digər giriş səhifələrini yerləşdirə bilər ki, bunlar ictimaiyyət üçün nəzərdə tutulmayıb. Hücumçular icazəsiz giriş əldə etmək üçün bunları cəlbedici hədəf kimi görə bilərlər.
* **Köhnə Tətbiqlər:** Unudulmuş, köhnə veb tətbiqlər alt domenlərdə qala bilər və bunlar məlum zəifliklərə malik köhnəlmiş proqram təminatı ehtiva edə bilər.
* **Həssas Məlumat:** Alt domenlər təsadüfən gizli sənədləri, daxili məlumatları və ya konfiqurasiya fayllarını ifşa edə bilər ki, bu da hücumçular üçün qiymətli ola bilər.

---

### **Alt Domen Enumerasiyası**

Alt domen enumerasiyası, bu alt domenləri sistematik olaraq müəyyənləşdirmək və siyahıya almaq prosesidir. DNS baxımından alt domenlər adətən A (və ya IPv6 üçün AAAA) qeydləri ilə təmsil olunur, hansı ki, alt domen adını müvafiq IP ünvanına uyğunlaşdırır. Bundan əlavə, CNAME qeydləri alt domenlər üçün alias yaratmaq və onları digər domenlərə və ya alt domenlərə yönləndirmək üçün istifadə oluna bilər. Alt domen enumerasiyası üçün iki əsas yanaşma mövcuddur:

---

#### **1. Aktiv Alt Domen Enumerasiyası**

Bu, alt domenləri ortaya çıxarmaq üçün birbaşa hədəf domenin DNS serverləri ilə qarşılıqlı əlaqəni əhatə edir. Bir üsul DNS zona transferi etməkdir; yanlış konfiqurasiya edilmiş server tamamlayıcı alt domen siyahısını təsadüfən sızdıra bilər. Lakin təhlükəsizlik tədbirlərinin sərtləşdirilməsi səbəbindən bu nadir hallarda uğurludur.

Daha yayılmış aktiv texnika brute-force enumerasiyadır. Bu üsul potensial alt domen adlarını sistematik olaraq hədəf domenə qarşı sınamağı əhatə edir. `dnsenum`, `ffuf` və `gobuster` kimi vasitələr bu prosesi avtomatlaşdırır, ümumi alt domen adlarının söz siyahılarından və ya müəyyən nümunələrə əsaslanaraq xüsusi yaradılmış siyahılardan istifadə edir.

---

#### **2. Passiv Alt Domen Enumerasiyası**

Bu, hədəf domenin DNS serverlərinə birbaşa sorğu göndərmədən alt domenləri aşkar etmək üçün xarici məlumat mənbələrindən istifadə edir.

* **Certificate Transparency (CT) qeydləri:** SSL/TLS sertifikatlarının ictimai anbarlarıdır. Bu sertifikatlar tez-tez Subject Alternative Name (SAN) sahəsində əlaqəli alt domenlərin siyahısını ehtiva edir, potensial hədəflər üçün zəngin bir mənbə təmin edir.
* **Axtarış Mühərrikləri:** Google və DuckDuckGo kimi axtarış motorlarından istifadə edərək alt domenləri kəşf etmək mümkündür. Məsələn, `site:` operatorunu istifadə edərək yalnız hədəf domenə aid alt domenləri filtr edə bilərsiniz.
* **Onlayn Verilənlər Bazaları və Alətlər:** Müxtəlif mənbələrdən DNS məlumatlarını toplayan vasitələr alt domenləri hədəf serverə birbaşa sorğu göndərmədən tapmağa imkan verir.

Hər iki üsulun öz güclü və zəif tərəfləri vardır. Aktiv enumerasiya daha çox nəzarət və ətraflı kəşf imkanları təqdim edir, lakin daha çox aşkar oluna bilər. Passiv enumerasiya daha gizli olur, amma bütün mövcud alt domenləri aşkar etməyə bilər. Hər iki yanaşmanı birləşdirmək daha əhatəli və effektiv alt domen enumerasiyası strategiyası təmin edir.
