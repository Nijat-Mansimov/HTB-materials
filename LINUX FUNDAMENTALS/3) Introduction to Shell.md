### Shell-ə Giriş – Sənədləşmə

Linux shell-in necə istifadə olunacağını öyrənmək çox vacibdir, çünki çox sayda server Linux əsaslıdır. Bu serverlər tez-tez Linux seçilir, çünki Windows serverlərinə nisbətən daha az səhv ehtimalı vardır. Məsələn, veb serverlərin çoxu Linux-a əsaslanır.

Əməliyyat sistemini effektiv idarə etmək üçün onun əsas hissəsini, yəni **Shell**-i başa düşmək və mənimsəmək lazımdır.

Windows-dan Linux-a keçərkən istifadəçilər bunu ilk baxışda belə qavraya bilər:

Shell istifadəçilərə əmrlər vasitəsilə əməliyyat sistemi ilə birbaşa ünsiyyət qurmağa imkan verir və sistemin idarə olunmasını çox çevik və effektiv edir.

<img width="1065" height="280" alt="image" src="https://github.com/user-attachments/assets/52592d3e-6c53-4443-83b2-e071e4b4f0d3" />

### Linux Terminalı

#### Terminal nədir?

Linux terminalı, həmçinin **shell** və ya **komanda sətri** adlandırılır, istifadəçilərlə kompüter sisteminin nüvəsi (**kernel**) arasında mətn əsaslı giriş/çıxış (**I/O**) interfeysi təmin edir. “Console” termini də tipikdir, lakin o, pəncərəni deyil, mətn rejimində ekranı nəzərdə tutur. Terminal pəncərəsində sistemi idarə etmək üçün əmrlər icra oluna bilər.

Shell-i mətn əsaslı bir **GUI** kimi düşünə bilərik. Burada biz əmrlər daxil etməklə digər qovluqlara keçmək, fayllarla işləmək və sistemdən məlumat əldə etmək kimi fəaliyyətlər icra edirik, lakin shell-in imkanları daha genişdir.

---

#### Terminal Emulyatorları

**Terminal emulyasiyası** – terminalın funksiyasını təqlid edən proqram təminatıdır. Bu, qrafik istifadəçi interfeysi (**GUI**) daxilində mətn əsaslı proqramlardan istifadə etməyə imkan verir. Bundan əlavə, bir terminal daxilində əlavə terminallar kimi işləyən **komanda sətri interfeysləri (CLI)** də mövcuddur. Qısaca desək, terminal shell tərcüməçisinə interfeys rolunu oynayır.

---

#### Analogi ilə izah

Böyük bir ofis binasını təsəvvür edin:

* **Shell** – şirkətin bütün məlumatlarını və əmrlərini emal edən əsas **server otağıdır**.
* **Terminal** – server otağı ilə əlaqə nöqtəsi olan **resepsion masasıdır**. Siz resepsiona (terminala) yaxınlaşırsınız və sorğunuzu çatdırırsınız, o isə bunu server otağına (shell-ə) ötürür.

İndi isə təsəvvür edin ki, siz uzaqdan işləyirsiniz. Terminal emulyasiya proqramı kompüter ekranınızda qrafik interfeys (**GUI**) üzərində **virtual resepsion masası** rolunu oynayır və sizə ofisdə fiziki olaraq iştirak etmədən server otağı ilə əlaqə qurmağa imkan verir.

Əlavə olaraq, bir terminal daxilində işləyən **CLI** terminalları – ekranınızda eyni anda birdən çox **virtual resepsion masası** açmaq kimidir. Onların hər biri server otağına ayrıca tapşırıqlar göndərə bilər, lakin hamısı eyni əsas interfeys üzərindən işləyir.

---

#### Terminal Emulyatorları və Multipleksorlar

Terminal üçün emulyatorlar və **multipleksorlar** faydalı genişlənmələrdir. Onlar terminal ilə işləmək üçün müxtəlif üsullar və funksiyalar təqdim edir:

* terminal pəncərəsini bölmək,
* birdən çox qovluqda işləmək,
* müxtəlif iş sahələri yaratmaq,
* paralel sessiyalar idarə etmək və s.

Məsələn, **Tmux** adlı terminal multipleksorunun istifadəsi təxminən belə görünə bilər:

<img width="1088" height="632" alt="image" src="https://github.com/user-attachments/assets/d4601343-20ff-4230-b8a0-6369a8af6864" />

### Shell

Linux-da ən çox istifadə olunan shell **Bourne-Again Shell (BASH)**-dır və bu, **GNU layihəsinin** bir hissəsidir. GUI vasitəsilə edə bildiyimiz hər şeyi shell üzərindən də icra edə bilərik. Shell bizə proqramlar və proseslərlə qarşılıqlı əlaqə qurmaq üçün daha çox imkan yaradır və məlumatı daha sürətli əldə etməyə şərait yaradır. Bundan əlavə, bir çox prosesi kiçik və ya böyük **skriptlər** vasitəsilə asanlıqla avtomatlaşdırmaq olar ki, bu da əl işini xeyli sadələşdirir.

Bash-dan başqa həmçinin digər shell-lər də mövcuddur:

* **Tcsh / Csh**
* **Ksh**
* **Zsh**
* **Fish shell** və digərləri.




























