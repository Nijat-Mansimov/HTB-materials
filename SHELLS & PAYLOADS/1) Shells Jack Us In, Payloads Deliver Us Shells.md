## Şelllər Bizə Giriş Verir, Faydalı Yüklər (Payloads) Bizə Şelllər Çatdırır

Bir **şell** (shell) kompüter istifadəçisinə sistemə təlimat daxil etmək və mətn çıxışını görmək üçün interfeys təmin edən bir proqramdır (məsələn, **Bash**, **Zsh**, **cmd** və **PowerShell**). Penetration test edənlər və informasiya təhlükəsizliyi mütəxəssisləri olaraq, şellə sahib olmaq, adətən, bir zəifliyin istismar edilməsi və ya hədəf hosta interaktiv giriş əldə etmək üçün təhlükəsizlik tədbirlərinin yan keçilməsi nəticəsidir. Bir icra və ya yeni bir təcrübə sessiyasını müzakirə edən insanların istifadə etdiyi aşağıdakı ifadələri eşitmiş və ya oxumuş ola bilərik:

* "Mən bir şell tutdum."
* "Mən bir şell 'partlatdım' (popped a shell)!"
* "Mən bir şellə 'düşdüm' (dropped into a shell)!"
* "Mən içəridəyəm!"

Tipik olaraq, bu ifadələr, həmin şəxsin bir sistemdəki zəifliyi uğurla istismar etdiyini və hədəf kompüterin əməliyyat sisteminin **uzaqdan idarə olunan şellinə** (remote shell) giriş əldə edə bildiyini bildirir. Bu, penetration test edənlərin zəif bir maşına daxil olarkən əsas hədəfidir. Biz görəcəyik ki, bu modulun əksər hissəsi sadalama (enumeration) və perspektivli istismarların müəyyən edilməsindən **sonra** gələn mərhələlərə fokuslanacaq.

### Niyə Şell Əldə Etməliyik?

Yadda saxlayaq ki, şell bizə **əməliyyat sisteminə (OS)**, **sistem əmrlərinə** və **fayl sisteminə** birbaşa giriş verir. Beləliklə, giriş əldə etsək, imtiyazları yüksəltməyimizə, künc dəyişdirməyimizə (pivot), fayl köçürməyimizə və daha çox şeyə imkan verən vektorları tapmaq üçün sistemi araşdırmağa başlaya bilərik. Əgər bir şell sessiyası qura bilməsək, hədəf maşında nə qədər irəliləyə biləcəyimiz olduqca məhdudlaşır.

Bir şell qurmaq həm də sistemdə **davamlılığı (persistence)** qorumağımıza imkan verir və bizə işləmək üçün daha çox vaxt verir. Bu, tezliklə növbəti nümayişlərdə görəcəyimiz kimi, **hücum alətlərimizi** istifadə etməyi, **məlumatları sızdırmağı (exfiltrate)**, hücumumuzun bütün detallarını **toplamağı**, **saxlamağı** və **sənədləşdirməyi** asanlaşdıra bilər.

Qeyd etmək vacibdir ki, şell qurmaq demək olar ki, həmişə əməliyyat sisteminin **Əmr Sətri İnterfeysinə (CLI)** daxil olmaq deməkdir və bu, VNC və ya RDP üzərindən qrafik şellə uzaqdan daxil olmaqdan fərqli olaraq, bizi **daha çətin nəzərə çarpan** edə bilər. Əmr sətri interfeyslərində bacarıqlı olmaq digər əhəmiyyətli üstünlükləri təmin edir: onlar **qrafik şellərdən daha çətin aşkar edilir**, **əməliyyat sistemində daha sürətli naviqasiya etməyə** imkan verir və **əməliyyatlarımızı avtomatlaşdırmağı asanlaşdırır**. Biz bu modul boyunca şelllərə aşağıdakı perspektivlərdən baxacağıq:

| Perspektiv | Təsvir |
| :--- | :--- |
| **Kompüter (Computing)** | Bir PC-də tapşırıqları idarə etmək və təlimatları daxil etmək üçün istifadə olunan mətn əsaslı istifadəçi mühiti. Bash, Zsh, cmd və PowerShell-i düşünün. |
| **İstismar və Təhlükəsizlik** | Şell, adətən, bir zəifliyin istismar edilməsi və ya təhlükəsizlik tədbirlərinin yan keçilməsi nəticəsində hosta interaktiv giriş əldə edilməsidir. Buna misal olaraq, uzaqdan hostun cmd-prompt-a giriş əldə etmək üçün Windows hostunda **EternalBlue** zəifliyinin işə salınmasını göstərmək olar. |
| **Veb** | Bu bir az fərqlidir. **Veb şell** (web shell) standart şellə bənzəyir, lakin o, (adətən, bir fayl və ya skript yükləmə qabiliyyəti kimi) bir zəifliyi istismar edir ki, bu da hücum edənə təlimatlar vermək, faylları oxumaq və onlara daxil olmaq və potensial olaraq əsas hostda dağıdıcı əməliyyatlar həyata keçirmək üçün bir yol təmin edir. Veb şellin idarə edilməsi, adətən, brauzer pəncərəsindəki skripti çağırmaqla həyata keçirilir. |

### Faydalı Yüklər (Payloads) Bizə Şelllər Çatdırır

Ümumilikdə İT sənayesində, **faydalı yük (payload)** bir neçə fərqli şəkildə müəyyən edilə bilər:

* **Şəbəkə:** Müasir kompüter şəbəkələrini keçən bir paketdəki çərçivələnmiş məlumat hissəsi.
* **Əsas Kompüter:** Faydalı yük, görüləcək əməliyyatı təyin edən təlimat dəstinin hissəsidir (Başlıqlar və protokol məlumatları xaric).
* **Proqramlaşdırma:** Proqramlaşdırma dili təlimatı tərəfindən istinad edilən və ya daşınan məlumat hissəsi.
* **İstismar və Təhlükəsizlik:** Faydalı yük, bir kompüter sistemindəki zəifliyi istismar etmək niyyəti ilə hazırlanmış **koddur**. "Faydalı yük" termini, ransomware daxil olmaqla, lakin bununla məhdudlaşmayaraq, müxtəlif zərərli proqram (malware) növlərini təsvir edə bilər.

Bu modulda, biz özümüzə hosta giriş vermək və zəif sistemlərlə **uzaqdan şell sessiyaları** qurmaq kontekstində bir çox müxtəlif növ **faydalı yüklər** və onların çatdırılma üsulları ilə işləyəcəyik.
