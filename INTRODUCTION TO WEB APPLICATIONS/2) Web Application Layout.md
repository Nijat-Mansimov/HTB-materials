## Veb Tətbiqi Layoutu

**Eyni** iki veb tətbiqi yoxdur. Müəssisələr veb tətbiqlərini **çoxsaylı istifadələr və auditoriyalar** üçün yaradırlar. Veb tətbiqləri fərqli şəkildə **dizayn və proqramlaşdırılır** və *back end* infrastrukturu bir çox fərqli yollarla qurula bilər. Veb tətbiqlərinin **pərdə arxasında** necə işləyə biləcəyini, veb tətbiqinin **strukturunu, komponentlərini** və şirkətin infrastrukturunda **necə qurula biləcəyini** anlamaq vacibdir.

Veb tətbiqi layoutları aşağıdakı üç əsas kateqoriya ilə ümumiləşdirilə bilən bir çox fərqli **qatdan** ibarətdir:

| Kateqoriya | Təsvir |
| :--- | :--- |
| **Veb Tətbiqi İnfrastrukturu** | Veb tətbiqinin **nəzərdə tutulduğu kimi işləməsi** üçün lazım olan **verilənlər bazası** kimi tələb olunan komponentlərin **strukturunu** təsvir edir. Veb tətbiqi **ayrı bir serverdə** işləmək üçün qurula bildiyindən, onun hansı verilənlər bazası serverinə daxil olması lazım olduğunu bilmək vacibdir. |
| **Veb Tətbiqi Komponentləri** | Bir veb tətbiqini təşkil edən komponentlər, veb tətbiqinin qarşılıqlı əlaqədə olduğu **bütün komponentləri** təmsil edir. Bunlar aşağıdakı üç sahəyə bölünür: **UI/UX** (*User Interface/User Experience*), **Klient** və **Server** komponentləri. |
| **Veb Tətbiqi Arxitekturası** | Arxitektura müxtəlif veb tətbiqi komponentləri arasındakı **bütün əlaqələri** əhatə edir. |

---

## Veb Tətbiqi İnfrastrukturu

Veb tətbiqləri bir çox fərqli infrastruktur quruluşlarından istifadə edə bilər. Bunlar həmçinin **modellər** adlanır. Ən ümumi olanlar aşağıdakı **dörd növə** qruplaşdırıla bilər:

1.  **Klient-Server**
2.  **Bir Server**
3.  **Çox Server - Bir Verilənlər Bazası**
4.  **Çox Server - Çox Verilənlər Bazası**

### Klient-Server

Veb tətbiqləri tez-tez **klient-server** modelini qəbul edir. Klient-server modelində, bir server veb tətbiqini **host edir** və ona daxil olmağa çalışan **istənilən klientə paylayır**.

<img width="1503" height="626" alt="image" src="https://github.com/user-attachments/assets/7a01eb66-5941-4b69-a44e-343962b8e052" />

Bu modeldə veb tətbiqlərinin **iki növ komponenti** var: **ön interfeysdə** (*front end*) olanlar ki, bunlar adətən **klient tərəfində** (*brauzerdə*) **tərcümə edilir və icra olunur**, və **arxa fonda** (*back end*) olan komponentlər ki, bunlar adətən **hosting serveri tərəfindən kompilyasiya edilir, tərcümə edilir və icra olunur**.

Klient veb tətbiqinin **URL-inə** (veb ünvanına, məsələn, `https://www.acme.local`) daxil olduqda, server **əsas veb tətbiqi interfeysini (UI)** istifadə edir. İstifadəçi bir düyməyə klik etdikdə və ya müəyyən bir funksiyanı tələb etdikdə, **brauzer serverə bir HTTP veb sorğusu göndərir**, hansı ki, server bu sorğuyu **tərcümə edir** və sorğunu tamamlamaq üçün **lazımi tapşırıqları** yerinə yetirir (məsələn, istifadəçinin daxil olması, səbətə bir element əlavə etmək, başqa bir səhifəyə keçmək və s.). Server tələb olunan məlumatlara sahib olduqdan sonra, **nəticəni klientin brauzerinə geri göndərir** və nəticəni **insan tərəfindən oxuna bilən** bir şəkildə göstərir.

Hal-hazırda qarşılıqlı əlaqədə olduğumuz bu vebsayt da **Hack The Box (vebserver)** tərəfindən inkişaf etdirilmiş və host edilmiş bir **veb tətbiqidir** və biz ona **veb brauzerimizdən (klient)** istifadə edərək daxil olur və qarşılıqlı əlaqədə oluruq.

Lakin, əksər veb tətbiqləri **klient-server ön-arxa fon arxitekturasını** istifadə etsə də, **bir çox dizayn tətbiqləri** mövcuddur.

---

## Bir Server (*One Server*)

Bu arxitekturada, **bütün veb tətbiqi** və ya hətta **bir neçə veb tətbiqi** və onların komponentləri, o cümlədən **verilənlər bazası** **tək bir serverdə** host edilir. Bu dizayn **sadə və tətbiqi asan olsa da**, həm də **ən riskli dizayndır**. 

<img width="1451" height="820" alt="image" src="https://github.com/user-attachments/assets/f550082c-0017-448b-b8ee-80eeb43cf015" />

Əgər bu arxitekturada bu serverdə yerləşdirilən **hər hansı bir veb tətbiqi təhlükə altına düşərsə**, o zaman **bütün veb tətbiqlərinin məlumatları** da təhlükə altına düşəcək. Bu dizayn "**bütün yumurtalar bir səbətdədir**" (*"all eggs in one basket"*) yanaşmasını təmsil edir, çünki host edilmiş veb tətbiqlərindən **hər hansı biri zəifdirsə**, **bütün vebserver zəif** hala gəlir.

Bundan əlavə, əgər **vebserver hər hansı bir səbəbdən sıradan çıxarsa** (*goes down*), problem həll olunana qədər **bütün host edilmiş veb tətbiqləri tamamilə əlçatmaz** olur.

---

## Çox Server - Bir Verilənlər Bazası (*Many Servers - One Database*)

Bu model **verilənlər bazasını öz verilənlər bazası serverinə ayırır** və veb tətbiqlərinin host edildiyi serverə məlumatları saxlamaq və əldə etmək üçün verilənlər bazası serverinə daxil olmasına imkan verir. Verilənlər bazası öz verilənlər bazası serverində ayrıldığı müddətcə, bu, **çox serverdən bir verilənlər bazasına** və **bir serverdən bir verilənlər bazasına** kimi görülə bilər. 

<img width="1992" height="1245" alt="image" src="https://github.com/user-attachments/assets/16ba1633-d50d-4015-949e-3a9690bc0c3f" />

Bu model bir neçə veb tətbiqinin **məlumatları aralarında sinxronizasiya etmədən** eyni məlumatlara daxil olmaq üçün tək bir verilənlər bazasına daxil olmasına imkan verə bilər. Veb tətbiqləri bir əsas tətbiqin təkrarı (*replication*) (yəni, əsas/ehtiyat) və ya **ümumi məlumatları paylaşan ayrı veb tətbiqləri** ola bilər.

Bu modelin əsas üstünlüyü (təhlükəsizlik baxımından) **seqmentasiyadır**, burada bir veb tətbiqinin əsas komponentlərinin hər biri **ayrıca yerləşir və host edilir**. Bir vebserver təhlükə altına düşərsə, digər vebserverlərə birbaşa təsir edilmir. Eynilə, verilənlər bazası təhlükə altına düşərsə (məsələn, **SQL injeksiya** zəifliyi vasitəsilə), veb tətbiqinin özünə birbaşa təsir edilmir. Aktivlərin seqmentasiyasından sonra hələ də tətbiq edilməli olan **giriş nəzarəti tədbirləri** var, məsələn, veb tətbiqinin girişini yalnız **nəzərdə tutulduğu kimi funksiyası üçün lazım olan məlumatlarla** məhdudlaşdırmaq.

---

## Çox Server - Çox Verilənlər Bazası (*Many Servers - Many Databases*)

Bu model **Çox Server, Bir Verilənlər Bazası** modeli üzərində qurulur. Lakin, verilənlər bazası serveri daxilində **hər bir veb tətbiqinin məlumatları ayrı bir verilənlər bazasında** host edilir. Veb tətbiqi yalnız **xüsusi məlumatlara** və **veb tətbiqləri arasında paylaşılan yalnız ümumi məlumatlara** daxil ola bilər. Həmçinin hər bir veb tətbiqinin verilənlər bazasını **öz ayrı verilənlər bazası serverində** host etmək də mümkündür. 

<img width="1875" height="1250" alt="image" src="https://github.com/user-attachments/assets/10fe2366-440d-4e1c-a80f-ce6bb52f3559" />

Bu dizayn həmçinin **artıqlıq** (*redundancy*) məqsədləri üçün də geniş istifadə olunur ki, **hər hansı bir veb serveri və ya verilənlər bazası sıradan çıxarsa**, **dayanma müddətini** mümkün qədər azaltmaq üçün onun yerinə bir **ehtiyat** (*backup*) işə düşsün. Baxmayaraq ki, bunu tətbiq etmək daha çətin ola bilər və düzgün işləməsi üçün **yük balanslaşdırıcıları** (*load balancers*) kimi alətlər tələb edə bilər, bu arxitektura **düzgün giriş nəzarəti tədbirləri** və **düzgün aktiv seqmentasiyası** sayəsində **təhlükəsizlik baxımından ən yaxşı seçimlərdən** biridir.

Bu modellərdən başqa, **serverless** veb tətbiqləri və ya **mikroxidmətlərdən** (*microservices*) istifadə edən veb tətbiqləri kimi **başqa veb tətbiqi modelləri** də mövcuddur.

***

## Veb Tətbiqi Komponentləri

Hər bir veb tətbiqinin fərqli sayda komponenti ola bilər. Buna baxmayaraq, əvvəllər qeyd olunan modellərin bütün komponentləri aşağıdakılara bölünə bilər:

* **Klient**
* **Server**
* **Vebserver**
* **Veb Tətbiqi Məntiqi** (*Web Application Logic*)
* **Verilənlər Bazası** (*Database*)
* **Xidmətlər** (*Services*) (Mikroxidmətlər - *Microservices*)
* **3-cü Tərəf İnteqrasiyaları** (*3rd Party Integrations*)
* **Funksiyalar** (*Functions*) (Serverless)

***

## Veb Tətbiqi Arxitekturası

Bir veb tətbiqinin komponentləri **üç fərqli qata** (**Üçqatlı Arxitektura** - *Three Tier Architecture* kimi də tanınır) bölünür.

| Qat | Təsvir |
| :--- | :--- |
| **Təqdimat Qatı** (*Presentation Layer*) | Tətbiq və sistemlə **əlaqə yaratmağa** imkan verən **UI proses komponentlərindən** ibarətdir. Bunlara **klient veb brauzer vasitəsilə daxil ola** bilər və **HTML, JavaScript və CSS** şəklində qaytarılır. |
| **Tətbiq Qatı** (*Application Layer*) | Bu qat **bütün klient sorğularının** (*veb sorğularının*) **düzgün emal edilməsini** təmin edir. **İcazə, imtiyazlar** və klientə ötürülən **məlumatlar** kimi müxtəlif meyarlar yoxlanılır. |
| **Məlumat Qatı** (*Data Layer*) | Məlumat qatı tələb olunan məlumatların **dəqiq harada saxlandığını** və **necə əldə edilə biləcəyini** müəyyən etmək üçün **tətbiq qatı ilə sıx əməkdaşlıq** edir. |

Bir veb tətbiqi arxitekturasının nümunəsi təxminən belə görünə bilər: 

<img width="1378" height="749" alt="image" src="https://github.com/user-attachments/assets/454cbf71-3b98-40b5-ae70-53d516a8f075" />

Bəzi veb serverlər **əməliyyat sistemi çağırışlarını və proqramlarını** işlədə bilər, məsələn, **IIS ISAPI** və ya **PHP-CGI**.

---

## Mikroxidmətlər (*Microservices*)

Biz mikroxidmətləri veb tətbiqinin **müstəqil komponentləri** kimi düşünə bilərik ki, əksər hallarda **yalnız bir tapşırıq** üçün proqramlaşdırılır. Məsələn, bir onlayn mağaza üçün əsas tapşırıqları aşağıdakı komponentlərə ayıra bilərik:

* **Qeydiyyat**
* **Axtarış**
* **Ödənişlər**
* **Reytinqlər**
* **Rəylər**

Bu komponentlər **klientlə** və **bir-biri ilə** əlaqə qurur. Bu mikroxidmətlər arasındakı rabitə **vəziyyətsizdir** (*stateless*), yəni sorğu və cavab **müstəqildir**. Bunun səbəbi, saxlanılan məlumatların müvafiq mikroxidmətlərdən **ayrı saxlanmasıdır**. Mikroxidmətlərin istifadəsi **tək bir biznes məqsədinə** yönəlmiş müxtəlif **avtomatlaşdırılmış funksiyaların toplusu** kimi qurulmuş **xidmət yönümlü arxitektura (SOA)** kimi qəbul edilir. Buna baxmayaraq, bu mikroxidmətlər **bir-birindən asılıdır**.

Başqa bir əsas və səmərəli mikroxidmət komponenti odur ki, onlar **fərqli proqramlaşdırma dillərində** yazıla və hələ də qarşılıqlı əlaqə qura bilər. Mikroxidmətlər **asan miqyaslanmadan** (*scaling*) və tətbiqlərin **daha sürətli inkişafından** faydalanır ki, bu da **innovasiyanı** təşviq edir və **yeni xüsusiyyətlərin bazara çıxarılmasını sürətləndirir**. Mikroxidmətlərin bəzi üstünlükləri bunlardır:

* **Çeviklik** (*Agility*)
* **Çevik miqyaslanma** (*Flexible scaling*)
* **Asan tətbiqetmə** (*Easy deployment*)
* **Təkrar istifadə edilə bilən kod** (*Reusable code*)
* **Dayanıqlılıq** (*Resilience*)

---

## Serverless (Serversiz)

**AWS, GCP, Azure** və digərləri kimi bulud provayderləri **serversiz arxitekturalar** təklif edir. Bu platformalar **serverlərin özləri haqqında narahat olmadan** belə veb tətbiqlərini qurmaq üçün **tətbiq çərçivələri** təqdim edir. Bu veb tətbiqləri daha sonra **vəziyyətsiz hesablama konteynerlərində** (*stateless computing containers*) (məsələn, Docker) işləyir. Bu tip arxitektura şirkətə **infrastrukturu idarə etmək məcburiyyətində qalmadan** tətbiqləri və xidmətləri qurmaq və yerləşdirmək üçün **çeviklik** verir; **bütün server idarəetməsi bulud provayderi tərəfindən edilir**, bu da tətbiqləri və verilənlər bazalarını işlətmək üçün lazım olan serverləri **təmin etmək, miqyaslandırmaq və saxlamaq ehtiyacını aradan qaldırır**.

---

## Arxitektura Təhlükəsizliyi

Hər hansı bir veb tətbiqində **nüfuzetmə sınağı** (*penetration test*) apararkən **veb tətbiqlərinin ümumi arxitekturasını** və hər bir veb tətbiqinin **xüsusi dizaynını** anlamaq vacibdir. Bir çox hallarda, fərdi veb tətbiqinin zəifliyi mütləq **proqramlaşdırma səhvindən deyil**, onun **arxitekturasındakı dizayn səhvindən** qaynaqlana bilər.

Məsələn, fərdi veb tətbiqinin bütün əsas funksionallığı təhlükəsiz şəkildə tətbiq edilmiş ola bilər. Lakin, onun dizaynında **düzgün giriş nəzarəti tədbirlərinin olmaması** (yəni, **Rola Əsaslanan Giriş Nəzarətinin (RBAC)** istifadəsi) səbəbindən, istifadəçilər **birbaşa daxil olmaları nəzərdə tutulmayan** bəzi **idarəçi xüsusiyyətlərinə** daxil ola bilər və ya hətta bu imtiyazlara sahib olmadan **başqa istifadəçilərin xüsusi məlumatlarına** daxil ola bilər. Bu tip problemi həll etmək üçün **əhəmiyyətli bir dizayn dəyişikliyi** tətbiq edilməli olacaq ki, bu da çox güman ki, **həm baha, həm də vaxt aparan** olacaq.

Başqa bir nümunə, bir zəifliyi istismar etdikdən və **arxa fon serveri üzərində nəzarət** qazandıqdan sonra **verilənlər bazasını tapa bilməməyimiz** olardı ki, bu da verilənlər bazasının **ayrı bir serverdə host edildiyi** anlamına gələ bilər. Biz verilənlər bazası məlumatlarının **yalnız bir hissəsini** tapa bilərik ki, bu da **bir neçə verilənlər bazasının** istifadə edildiyi anlamına gələ bilər. Buna görə də **təhlükəsizlik veb tətbiqinin inkişafının hər bir mərhələsində nəzərə alınmalı** və **nüfuzetmə sınaqları** veb tətbiqinin **inkişaf həyat dövrü** boyunca həyata keçirilməlidir. 






































