## OSI Modeli

**ISO/OSI** standartının müəyyənləşdirilməsində məqsəd, müxtəlif texniki sistemlərin müxtəlif cihazlar və texnologiyalar vasitəsilə əlaqə qurmasına imkan verən və **uyğunluğu** (*compatibility*) təmin edən bir istinad modeli yaratmaq idi. **OSI** modeli bu məqsədə çatmaq üçün bir-birinə iyerarxik olaraq əsaslanan **yeddi** fərqli qatdan istifadə edir. Bu qatlar, göndərilən paketlərin keçdiyi hər bir əlaqənin qurulması mərhələlərini təmsil edir. Bu şəkildə, əlaqənin necə qurulduğunu və vizual olaraq necə təşkil edildiyini izləmək üçün bir standart yaradılmışdır.

| Qat | Funksiya |
| :--- | :--- |
| **7. Tətbiq (Application)** | Digər vəzifələrlə yanaşı, bu qat məlumatların giriş və çıxışını idarə edir və tətbiq funksiyalarını təmin edir. |
| **6. Təqdimat (Presentation)** | Təqdimat qatının vəzifəsi, məlumatların sistemdən asılı təqdimatını tətbiqdən asılı olmayan bir formaya çevirməkdir. |
| **5. Seans (Session)** | Seans qatı iki sistem arasındakı məntiqi əlaqəni idarə edir və məsələn, əlaqə qırılmalarının və ya digər problemlərin qarşısını alır. |
| **4. Nəqliyyat (Transport)** | Qat 4 ötürülən məlumatların **ucdan-uca nəzarəti** (*end-to-end control*) üçün istifadə olunur. Nəqliyyat Qatı tıxac vəziyyətlərini aşkar edə və qarşısını ala, həmçinin məlumat axınlarını seqmentləşdirə bilər. |
| **3. Şəbəkə (Network)** | Şəbəkə qatında kommutasiya edilmiş şəbəkələrdə əlaqələr qurulur və paket kommutasiyalı şəbəkələrdə məlumat paketləri yönləndirilir. Məlumatlar göndəricidən qəbulediciyə qədər bütün şəbəkə üzərindən ötürülür. |
| **2. Məlumat Keçidi (Data Link)** | Qat 2-nin əsas vəzifəsi müvafiq mühit üzərində etibarlı və səhvsiz ötürmələri təmin etməkdir. Bu məqsədlə, Qat 1-dən gələn bit axınları bloklara və ya freymlərə bölünür. |
| **1. Fiziki (Physical)** | İstifadə olunan ötürmə texnikaları, məsələn, elektrik siqnalları, optik siqnallar və ya elektromaqnit dalğalarıdır. Qat 1 vasitəsilə ötürmə kabelli və ya simsiz ötürmə xətləri üzərində baş verir. |

**2-4-cü qatlar nəqliyyat yönümlü** (*transport oriented*), **5-7-ci qatlar isə tətbiq yönümlü** (*application oriented*) qatlardır. Hər bir qatda dəqiq müəyyən edilmiş tapşırıqlar yerinə yetirilir və qonşu qatlarla interfeyslər dəqiq təsvir edilir. Hər bir qat bilavasitə yuxarısındakı qat üçün istifadə ediləcək xidmətlər təklif edir. Bu xidmətləri təmin etmək üçün, qat özündən aşağıdakı qatın xidmətlərindən istifadə edir və öz qatının vəzifələrini yerinə yetirir.

Əgər iki sistem əlaqə qurursa, **OSI** modelinin bütün yeddi qatı ən azı **iki dəfə** işlənir, çünki həm göndərən, həm də qəbuledici qat modelini nəzərə almalıdır. Buna görə də, rabitənin təhlükəsizliyini, etibarlılığını və performansını təmin etmək üçün fərdi qatlarda çox sayda fərqli vəzifələr yerinə yetirilməlidir.

Bir tətbiq digər sistemə bir paket göndərdikdə, sistem yuxarıda göstərilən qatları **7-ci qatdan 1-ci qata** qədər işləyir və qəbuledici sistem alınan paketi **1-ci qatdan 7-ci qata** qədər açır.
