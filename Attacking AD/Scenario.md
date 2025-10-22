Biz CAT-5 Security üçün Penetrasiya Testçilərik. Komanda ilə bir neçə uğurlu müşahidə dövründən sonra daha təcrübəli üzvlər bizim öz başımıza qiymətləndirməyə başlayanda nə qədər yaxşı işləyə biləcəyimizi görmək istəyirlər. Komanda rəhbəri bizə yerinə yetirməli olduğumuzları ətraflı izah edən aşağıdakı e‑poçtu göndərdi.

Tapşırıq e‑poçtu

<img width="1368" height="555" alt="image" src="https://github.com/user-attachments/assets/33d846d9-9384-49a7-857d-7336bd74845b" />

Bu modul bizə bu tapşırıqlarla bağlı (**həm əvvəlki, həm də yeni qazanılmış**) bacarıqlarımızı tətbiq etməyə imkan verəcək. Bu modul üçün yekun qiymətləndirmə, **Inlanefreight** şirkətinə qarşı iki daxili sızma sınağının icrasıdır. Bu qiymətləndirmələr zamanı, müştərilərin tez-tez tələb etdiyi kimi, birincisi xarici sızma mövqeyindən başlayan daxili sızma sınağı və ikincisi daxili şəbəkədəki hücum qutusundan başlayan sınaq üzərində işləyəcəyik. Bacarıq qiymətləndirmələrinin tamamlanması, yuxarıdakı əhatə sənədində və tapşırıq e-poçtunda qeyd olunan tapşırıqların uğurla başa çatdığını göstərir.

Bunu edərək, bir çox **avtomatlaşdırılmış və əl ilə AD (Active Directory) hücum və nömrələmə konsepsiyalarını** möhkəm mənimsədiyimizi, geniş alətlər dəsti ilə tanışlığımızı və təcrübəmizi, həmçinin qiymətləndirməni irəli aparmaq üçün kritik qərarlar qəbul etmək məqsədilə AD mühitindən toplanmış məlumatları şərh etmək bacarığımızı nümayiş etdirmiş olacağıq. Bu modulun məzmunu, hər kəsin daxili sızma sınaqlarını uğurla yerinə yetirməsi üçün zəruri olan **əsas nömrələmə konsepsiyalarını** əhatə etmək üçün nəzərdə tutulmuşdur. Daha təkmil modullarda əhatə olunacaq AD-yə yönəlmiş materiallar üçün əsas kimi bəzi daha qabaqcıl konsepsiyalar üzərində işləyərkən, ən çox yayılmış hücum metodlarının bir çoxunu ətraflı şəkildə nəzərdən keçirəcəyik.

Aşağıda, müştəri tərəfindən təqdim olunan bütün vacib məlumatları ehtiva edən, iştirak üçün tamamlanmış əhatə sənədini tapa bilərsiniz.

---

## Qiymətləndirmənin Əhatəsi

Aşağıda müəyyən edilmiş IP-lər, hostlar və domenlər qiymətləndirmənin əhatəsini təşkil edir.

### Əhatə Dairəsində Olanlar

| Diapazon/Domen | Təsvir |
| :--- | :--- |
| **INLANEFREIGHT.LOCAL** | Müştəri domeni, AD və veb xidmətləri daxil olmaqla. |
| **LOGISTICS.INLANEFREIGHT.LOCAL** | Müştəri alt domeni. |
| **FREIGHTLOGISTICS.LOCAL** | Inlanefreight-in sahib olduğu törəmə şirkət. INLANEFREIGHT.LOCAL ilə xarici meşə etibarı (external forest trust) var. |
| **172.16.5.0/23** | Əhatə dairəsində olan daxili alt şəbəkə. |

### Əhatə Dairəsindən Kənar Olanlar

* INLANEFREIGHT.LOCAL-un hər hansı digər alt domenləri.
* FREIGHTLOGISTICS.LOCAL-un hər hansı alt domenləri.
* Hər hansı fişinq və ya sosial mühəndislik hücumları.
* Açıq şəkildə qeyd olunmayan hər hansı digər IP-lər/domenlər/alt domenlər.
* Bu modulda göstərilən passiv nömrələmə xaricində real dünyadakı `https://www.inlanefreight.com` veb-saytına qarşı hər hansı növ hücumlar.

---

## İstifadə Olunan Metodlar

Aşağıdakı metodlar Inlanefreight və onun sistemlərini qiymətləndirmək üçün səlahiyyətlidir:

### Xarici Məlumat Toplama (Passiv Yoxlamalar)

Xarici məlumat toplama, internetdən şirkət haqqında toplana biləcək məlumatlarla bağlı riskləri nümayiş etdirmək üçün icazəlidir. Real dünya hücumunu simulyasiya etmək üçün, **CAT-5** və onun qiymətləndiriciləri, bu sənəddə verilənlər xaricində Inlanefreight haqqında əvvəlcədən verilmiş heç bir məlumat olmadan internetdə **anonim bir perspektivdən** xarici məlumat toplama işləri aparacaqlar.

CAT-5, daxili sınaqlara kömək edə biləcək və Inlanefreight üçün risk yarada biləcək açıq şəkildə əlçatan məlumatları müəyyən etmək üçün **açıq mənbə resurslarından** müxtəlif dərəcələrdə məlumat toplamaqla **passiv nömrələmə** aparacaq. İnternetə baxan "real-dünya" IP ünvanlarına və ya veb-sayta qarşı heç bir **aktiv nömrələmə, port skanları və ya hücumlar** həyata keçirilməyəcək.

### Daxili Sınaq

Daxili qiymətləndirmə hissəsi, Inlanefreight-in əməliyyat sahəsi daxilindən hücum vektorlarını təqlid etməyə çalışaraq daxili hostlar və xidmətlər (xüsusilə Active Directory) üzərindəki zəifliklərlə əlaqəli riskləri nümayiş etdirmək üçün nəzərdə tutulmuşdur.

Real dünya hücumunu simulyasiya etmək üçün, CAT-5, bu sənədlərdə təqdim olunan və xarici sınaqlardan aşkar edilən məlumatlar xaricində əvvəlcədən heç bir məlumat olmadan **etibarsız bir insayder perspektivindən** qiymətləndirmə aparacaq. Sınaq, daxili şəbəkədə **anonim bir mövqedən** başlayacaq, məqsəd **domen istifadəçi etimadnamələrini** əldə etmək, daxili domenin nömrələnməsini aparmaq, dayaq nöqtəsi qazanmaq və əhatə dairəsində olan bütün daxili domenləri kompromat etmək məqsədinə çatmaq üçün yanlara və şaquli istiqamətdə hərəkət etməkdir. Kompüter sistemləri və şəbəkə əməliyyatları qəsdən kəsilməyəcək.

### Parol Sınağı

Inlanefreight cihazlarından əldə edilmiş və ya təşkilat tərəfindən təqdim edilmiş **parol faylları**, deşifrə üçün **oflayn iş stansiyalarına** yüklənə bilər və daha çox giriş əldə etmək və qiymətləndirmə məqsədlərinə çatmaq üçün istifadə edilə bilər. Əldə edilmiş parol faylı və ya deşifrə edilmiş parollar heç bir zaman rəsmi olaraq qiymətləndirmədə iştirak etməyən şəxslərə açıqlanmayacaq. Bütün məlumatlar CAT-5-ə məxsus və təsdiq edilmiş sistemlərdə etibarlı şəkildə saxlanılacaq və müqavilədə müəyyən edilmiş müddət ərzində saxlanılacaq.

---

Biz bu əhatə sənədlərini sizə təqdim etdik ki, bu tipli sənədləşmə üslubunu görməyə öyrəşəsiniz. İnformasiya Təhlükəsizliyi Karyeralarınızda irəlilədikcə, xüsusilə hücum tərəfində, bu tipli məlumatları təsvir edən **Əhatə Sənədləri** və **İcra Qaydaları (RoE)** sənədləri almaq adi hal olacaq.

### Mərhələ Hazırdır

İndi bu modul üçün əhatə dairəmiz aydın şəkildə müəyyən edildiyinə görə, Active Directory nömrələmə və hücum vektorlarını araşdırmağa başlaya bilərik. İndi, Inlanefreight-ə qarşı **passiv xarici nömrələmə** aparmağa başlayaq.
