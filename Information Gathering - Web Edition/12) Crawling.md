Crawling
Crawling, tez-tez spidering adlandırılır, World Wide Web-i sistemli şəkildə avtomatlaşdırılmış şəkildə gəzən prosesdir. Tırtılın torunu gəzdiyi kimi, veb crawler bir səhifədən digərinə keçərək linkləri izləyir və məlumat toplayır. Bu crawler-lar əsasən əvvəlcədən müəyyən edilmiş alqoritmlərdən istifadə edən botlardır və veb səhifələri kəşf edir və indeksləyir, bu da onları axtarış motorları vasitəsilə və ya məlumat analizi və veb kəşfiyyat kimi digər məqsədlər üçün əlçatan edir.

### Veb Crawler-lar Necə İşləyir

Veb crawler-in əsas işləmə prinsipi sadə, lakin güclüdür. O, ilkin URL ilə başlayır, yəni crawl ediləcək başlanğıc veb səhifə. Crawler bu səhifəni götürür, məzmununu analiz edir və bütün linkləri çıxarır. Sonra bu linkləri növbəyə əlavə edir və onları da crawl edir, prosesi təkrarlayaraq davam etdirir. Ölçüsünə və konfiqurasiyasına görə, crawler bütün vebsaytı və ya hətta internetin böyük bir hissəsini araşdıra bilər.

**Əsas səhifə (Homepage):** Siz link1, link2 və link3 olan əsas səhifədən başlayırsınız.

```
Homepage
├── link1
├── link2
└── link3
```

**link1-ə ziyarət:** link1-ə ziyarət zamanı əsas səhifə, link2 və həmçinin link4 və link5 görünür.

```
link1 Page
├── Homepage
├── link2
├── link4
└── link5
```

**Crawling-ə davam:** Crawler bu linkləri sistemli şəkildə izləyərək bütün əlçatan səhifələri və onların linklərini toplamağa davam edir.

Bu nümunə göstərir ki, veb crawler linkləri sistemli şəkildə izləyərək məlumat kəşf edir və toplayır, bu isə potensial linkləri təxmin etməyə əsaslanan fuzzing-dən fərqlidir.

Crawling üçün iki əsas strategiya növü mövcuddur.

## Breadth-First Crawling

<img width="2613" height="1632" alt="image" src="https://github.com/user-attachments/assets/a1ca1960-d606-41ad-9ddc-e3975854386f" />

Genişlik üzrə Crawling (Breadth-First Crawling)
Genişlik üzrə crawling bir veb saytın genişliyini dərindən əvvəl araşdırmağa üstünlük verir. O, ilkin səhifədəki bütün linkləri crawl etməklə başlayır, sonra həmin səhifələrdəki linklərə keçir və bu proses belə davam edir. Bu, bir veb saytın strukturunu və məzmununu ümumi şəkildə anlamaq üçün faydalıdır.

### Dərinlik üzrə Crawling (Depth-First Crawling)

Flowchart göstərir ki, Seed URL səhifə 1-ə, sonra səhifə 2-yə aparır. Səhifə 2 səhifə 3-ə bağlanır, hansı ki səhifə 4 və səhifə 5-ə ayrılır.

Əksinə, dərinlik üzrə crawling genişlikdən əvvəl dərinliyə üstünlük verir. O, bir link yolunu mümkün qədər izləyir, sonra geri dönərək digər yolları araşdırır. Bu, spesifik məzmun tapmaq və ya bir veb saytın strukturuna daha dərin daxil olmaq üçün faydalı ola bilər.

Hansı strategiyanın seçilməsi crawling prosesinin xüsusi məqsədlərinə bağlıdır.

---

### Dəyərli Məlumatların Çıxarılması

Crawler-lar müxtəlif məlumatları çıxara bilər, hər biri kəşfiyyat prosesində xüsusi məqsədə xidmət edir:

* **Linklər (Daxili və Xarici):** Bunlar vebin əsas tikinti bloklarıdır, səhifələri bir sayt daxilində (daxili linklər) və digər saytlarla (xarici linklər) birləşdirir. Crawler-lar bu linkləri diqqətlə toplayır, saytın strukturunu xəritələşdirməyə, gizli səhifələri kəşf etməyə və xarici resurslarla əlaqələri müəyyən etməyə imkan verir.
* **Şərhlər:** Bloglar, forumlar və digər interaktiv səhifələrdəki şərh bölmələri məlumat üçün qızıl mədən ola bilər. İstifadəçilər tez-tez şərhlərində təsadüfən həssas detalları, daxili prosesləri və ya zəifliklərə işarələri açıqlayırlar.
* **Metadata:** Metadata, məlumat haqqında məlumatdır. Veb səhifələr kontekstində buna səhifə başlıqları, təsvirlər, açar sözlər, müəllif adları və tarixlər daxildir. Bu metadata səhifənin məzmunu, məqsədi və kəşfiyyat hədəflərinə uyğunluğu haqqında qiymətli kontekst təmin edə bilər.
* **Həssas Fayllar:** Veb crawler-lar veb saytında təsadüfən açıq qalmış həssas faylları aktiv axtarmaq üçün konfiqurasiya edilə bilər. Bunlara backup faylları (.bak, .old), konfiqurasiya faylları (web.config, settings.php), log faylları (error_log, access_log) və şifrələr, API açarları və digər gizli məlumatlar daxil ola bilər. Çıxarılan faylların, xüsusən backup və konfiqurasiya fayllarının diqqətlə təhlili verilənlər bazası etimadnamələri, şifrələmə açarları və hətta mənbə kod parçaları kimi həssas məlumatları aşkara çıxara bilər.

---

### Kontekstin Önəmi

Çıxarılan məlumatın əhatə etdiyi konteksti başa düşmək çox önəmlidir.

Məsələn, spesifik proqram versiyasına aid bir şərh öz-özlüyündə əhəmiyyətsiz görünə bilər. Lakin digər kəşflərlə—məsələn, metadatalarda qeyd edilmiş köhnəlmiş versiya və ya crawling vasitəsilə tapılmış potensial zəif konfiqurasiya faylı ilə birləşdirildikdə—bu kritik bir zəiflik göstəricisinə çevrilə bilər.

Çıxarılan məlumatın əsl dəyəri nöqtələri birləşdirib hədəfin rəqəmsal mənzərəsini tam şəkildə təsvir etməkdədir.

Məsələn, çıxarılan linklərin siyahısı ilkin baxışda adi görünə bilər. Lakin diqqətlə araşdırdıqda müəyyən bir nümunə müşahidə edilir: bir neçə URL /files/ adlı kataloqa işarə edir. Bu marağı oyadır və kataloqa əl ilə daxil olmağa qərar verirsiniz. Təəccüblə görürsünüz ki, kataloq gəzintisi aktivdir və backup arxivləri, daxili sənədlər və potensial həssas məlumatlar açıqdır. Bu kəşf yalnız fərdi linklərə baxmaqla mümkün olmazdı; kontekstual təhlil bu kritik nəticəyə gətirib çıxardı.

Eyni şəkildə, görünüşdə əhəmiyyətsiz şərhlər digər kəşflərlə birləşdirildikdə əhəmiyyət qazana bilər. Məsələn, bir şərh "file server"dən bəhs edə bilər və ilkin baxışda xəbərdarlıq yaratmaya bilər. Lakin /files/ kataloqunun tapılması ilə birləşdirildikdə, file server-in ictimai şəkildə əlçatan olması ehtimalını gücləndirir, bu da həssas və ya gizli məlumatların açıla biləcəyini göstərir.

Buna görə də məlumatların təhlilinə holistik yanaşmaq, müxtəlif məlumat nöqtələri arasındakı əlaqələri və onların kəşfiyyat məqsədlərinə potensial təsirlərini nəzərə almaq vacibdir.











