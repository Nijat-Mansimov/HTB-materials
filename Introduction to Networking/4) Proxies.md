## Proksilər (Proxies)

Bir çox insanın proksinin nə olduğu barədə fərqli fikirləri var:

* **Təhlükəsizlik Peşəkarları** **HTTP Proksilərinə** (**BurpSuite**) və ya **SOCKS/SSH Proksi** (**Chisel, ptunnel, sshuttle**) ilə **pivoting** (şəbəkədaxili keçid) etməyə üstünlük verirlər.
* **Veb İnkişafçılar** zərərli trafiki bloklamaq üçün **Cloudflare** və ya **ModSecurity** kimi proksilərdən istifadə edirlər.
* **Orta İnsanlar** bir proksinin öz yerini **gizlətmək** və başqa bir ölkənin **Netflix** kataloğuna daxil olmaq üçün istifadə edildiyini düşünə bilərlər.
* **Hüquq-Mühafizə Orqanları** tez-tez proksiləri qeyri-qanuni fəaliyyətlə əlaqələndirirlər.

Yuxarıdakı nümunələrin hamısı doğru deyil. **Proksi** bir cihaz və ya xidmət əlaqənin **ortasında oturduqda** və **vasitəçi** kimi çıxış etdikdə baş verir. **Vasitəçi** kritik məlumat parçasıdır, çünki bu, ortadakı cihazın **trafikin məzmununu yoxlaya** bilməsi deməkdir. **Vasitəçi** ola bilmə qabiliyyəti olmadan, cihaz texniki olaraq **şlüz** (*gateway*), proksi deyil.

Yuxarıdakı suala qayıdaq, orta insanın proksi haqqında səhv fikri var, çünki onlar çox güman ki, öz yerlərini gizlətmək üçün **VPN** istifadə edirlər ki, bu da texniki olaraq proksi deyil. Əksər insanlar IP Ünvanı dəyişdikdə bunun proksi olduğunu düşünürlər və əksər hallarda onları düzəltməmək daha yaxşıdır, çünki bu, ümumi və zərərsiz bir yanlış fikirdir. Onları düzəltmək, **tablar vs boşluqlar**, **emacs vs vim** mövzularına sürüklənən və ya onların **nano** istifadəçisi olduğunu öyrənməklə nəticələnən daha uzun bir söhbətə səbəb ola bilər.

Əgər bunu yadda saxlamaqda çətinlik çəkirsinizsə, proksilər demək olar ki, həmişə **OSI Modelinin 7-ci Qatında** (*Layer 7*) işləyəcək. Bir çox proksi xidməti növü var, lakin əsas olanlar bunlardır:

* **Xüsusi Proksi** / **İrəli Proksi** (*Forward Proxy*)
* **Tərs Proksi** (*Reverse Proxy*)
* **Şəffaf Proksi** (*Transparent Proxy*)

---

## Xüsusi Proksi / İrəli Proksi (Forward Proxy)

**İrəli Proksi**, əksər insanların proksi olaraq təsəvvür etdiyi şeydir. İrəli Proksi, bir müştəri bir kompüterə sorğu göndərdikdə və həmin kompüter sorğunu yerinə yetirdikdə baş verir.

Məsələn, bir korporativ şəbəkədə, həssas kompüterlərin İnternetə birbaşa çıxışı olmaya bilər. Bir veb-sayta daxil olmaq üçün onlar bir proksi (və ya veb filtri) vasitəsilə keçməlidirlər. Bu, zərərli proqramlara qarşı **inanılmaz dərəcədə güclü müdafiə xətti** ola bilər, çünki bu, yalnız veb filtrdən yan keçməli deyil (asan), həm də **proksi haqqında məlumatlı** (*proxy aware*) olmalı və ya qeyri-ənənəvi **C2**-dən (zərərli proqramın tapşırıq məlumatlarını alması üçün bir yol) istifadə etməlidir. Əgər təşkilat yalnız **FireFox**-dan istifadə edirsə, proksi haqqında məlumatlı zərərli proqramın olma ehtimalı çox aşağıdır.

**Internet Explorer, Edge** və ya **Chrome** kimi veb brauzerlər standart olaraq **"Sistem Proksi"** tənzimləmələrinə əməl edirlər. Əgər zərərli proqram **WinSock**-dan (Yerli Windows API) istifadə edirsə, çox güman ki, əlavə kod olmadan proksi haqqında məlumatlı olacaq. **Firefox** **WinSock**-dan istifadə etmir və əvəzində istənilən əməliyyat sistemində eyni kodu istifadə etməyə imkan verən **libcurl**-dan istifadə edir. Bu o deməkdir ki, zərərli proqram Firefox-u axtarmalı və proksi tənzimləmələrini çəkməli olacaq ki, zərərli proqramın bunu etməsi çox az ehtimaldır.

Alternativ olaraq, zərərli proqram **DNS**-i C2 mexanizmi kimi istifadə edə bilər, lakin bir təşkilat DNS-i monitorinq edirsə (**Sysmon** istifadə edərək bu asanlıqla edilir), bu tip trafik tez bir zamanda aşkarlanmalıdır.

İrəli Proksiyə başqa bir nümunə **Burp Suite**-dir, çünki əksər insanlar onu HTTP Sorğularını yönləndirmək üçün istifadə edirlər. Lakin, bu tətbiq HTTP Proksilərinin İsveçrə Ordu bıçağıdır və **tərs proksi** və ya **şəffaf proksi** olaraq konfiqurasiya edilə bilər!

## Forward Proxy

<img width="2576" height="1496" alt="image" src="https://github.com/user-attachments/assets/f3cce028-0595-407f-8cd6-37267fb3f99b" />

## Tərs Proksi (Reverse Proxy)

Yəqin ki, təxmin etdiyiniz kimi, **tərs proksi**, **İrəli Proksinin** əksidir. O, çıxan sorğuları filtrləmək üçün deyil, **gələn sorğuları** filtrləmək üçün nəzərdə tutulub. Tərs Proksi ilə ən ümumi məqsəd, bir ünvanda dinləmək və onu **qapalı bir şəbəkəyə** yönləndirməkdir.

Bir çox təşkilat **CloudFlare**-dən istifadə edir, çünki onların əksər **DDOS Hücumlarına** tab gətirə bilən güclü şəbəkəsi var. Cloudflare-dən istifadə edərək, təşkilatlar veb-serverlərinə göndərilən trafikin həcmini (və növünü) filtrləmək üçün bir yola sahib olurlar.

**Penetrasiya Sınağı Mütəxəssisləri** yoluxmuş son nöqtələrdə tərs proksiləri konfiqurasiya edəcəklər. Yoluxmuş son nöqtə bir portda dinləyəcək və porta qoşulan hər hansı bir müştərini yoluxmuş son nöqtə vasitəsilə **hücumçuya geri göndərəcək**. Bu, **firewalları** aşmaq və ya **qeydlərdən yayınmaq** üçün faydalıdır. Təşkilatlarda xarici veb sorğularını izləyən **IDS (Müdaxilə Aşkarlama Sistemləri)** ola bilər. Əgər hücumçu **SSH** üzərindən təşkilata giriş əldə edərsə, tərs proksi veb sorğularını SSH Tuneli vasitəsilə göndərə və IDS-dən yayına bilər.

Başqa bir ümumi Tərs Proksi, **Veb Tətbiqi Firewalı (WAF)** olan **ModSecurity**-dir. Veb Tətbiqi Firewalları veb sorğularını **zərərli məzmun** üçün yoxlayır və zərərli olarsa sorğunun qarşısını alır. Əgər bu barədə daha çox öyrənmək istəyirsinizsə, əla başlanğıc nöqtəsi olduğu üçün **ModSecurity Core Rule Set** haqqında oxumağı tövsiyə edirik. Cloudflare də bir WAF kimi fəaliyyət göstərə bilər, lakin bunu etmək bəzi təşkilatların istəməyəcəyi **HTTPS Trafikini deşifrə etməyə** imkan verməyi tələb edir.

<img width="2576" height="1526" alt="image" src="https://github.com/user-attachments/assets/d7ddf7b2-9aad-4a7d-ae60-1a598181a40a" />

## (Qeyri-) Şəffaf Proksi (Non-Transparent / Transparent Proxy)

Bütün bu proksi xidmətləri ya **şəffaf** (*transparent*), ya da **qeyri-şəffaf** (*non-transparent*) şəkildə fəaliyyət göstərir.

**Şəffaf proksi** olduqda, müştəri onun mövcudluğundan xəbərsiz olur. Şəffaf proksi müştərinin İnternetə yönələn kommunikasiya sorğularını ələ keçirir və əvəzedici instansiya kimi çıxış edir. Xaricdən baxdıqda, şəffaf proksi, qeyri-şəffaf proksi kimi, kommunikasiya tərəfdaşı kimi davranır.

Əgər bu **qeyri-şəffaf proksidirsə**, biz onun mövcudluğu barədə məlumatlı olmalıyıq. Bu məqsədlə, bizə və istifadə etmək istədiyimiz proqrama **xüsusi bir proksi konfiqurasiyası** verilir ki, bu da İnternetə gedən trafikin əvvəlcə proksiyə ünvanlanmasını təmin edir. Əgər bu konfiqurasiya mövcud deyilsə, biz proksi vasitəsilə əlaqə qura bilmərik. Lakin, proksi adətən digər şəbəkələrə yeganə kommunikasiya yolunu təmin etdiyi üçün, müvafiq proksi konfiqurasiyası olmadan İnternetlə əlaqə ümumiyyətlə kəsilir.












