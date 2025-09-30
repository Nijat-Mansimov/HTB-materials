# İstifadəçi İdarəetməsi (User Management)

Səmərəli istifadəçi idarəetməsi, Linux sistem idarəetməsinin təməl bir aspektidir. Administratorlar tez-tez müvafiq giriş nəzarətlərini tətbiq etmək üçün **yeni istifadəçi hesabları yaratmalı** və ya mövcud istifadəçiləri **xüsusi qruplara təyin etməlidirlər**. Bundan əlavə, fərqli imtiyazlar tələb edən tapşırıqlar üçün **əmrləri fərqli bir istifadəçi kimi icra etmək** tez-tez zəruridir. Məsələn, müəyyən qruplar sistem təhlükəsizliyini və bütövlüyünü qorumaq üçün vacib olan xüsusi fayllara və ya qovluqlara baxmaq və ya onları dəyişdirmək üçün müstəsna icazələrə malik ola bilər. Bu qabiliyyət, sistemin nasazlıqlarını aradan qaldırmaq və ya audit məqsədləri üçün kritik əhəmiyyətə malik ola biləcək daha ətraflı məlumatları yerli olaraq maşında toplamağımıza imkan verir.

Məsələn, təsəvvür edin ki, şirkətinizə Alex adlı yeni bir işçi qoşulur və öz tapşırıqlarını yerinə yetirmək üçün ona Linux əsaslı bir iş stansiyası verilir. Bir sistem administratoru olaraq, Alex üçün bir istifadəçi hesabı yaratmalı və onu layihə faylları və ya inkişaf vasitələri kimi zəruri resurslara giriş imkanı verən müvafiq qruplara əlavə etməlisiniz. Əlavə olaraq, Alexin müəyyən tapşırıqları yerinə yetirmək üçün **yüksəldilmiş imtiyazlarla** və ya **fərqli bir istifadəçi kimi** əmrləri icra etməli olduğu vəziyyətlər ola bilər.

-----

## İstifadəçi Kimi İcra Etmə

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ cat /etc/shadow
cat: /etc/shadow: Permission denied
```

**/etc/shadow** faylı, bütün istifadəçi hesabları üçün **şifrələnmiş parol məlumatlarını** saxlayan kritik bir sistem faylıdır. Təhlükəsizlik səbəblərinə görə, həssas autentifikasiya məlumatlarına icazəsiz girişi önləmək üçün **yalnız `root` istifadəçisi** tərəfindən oxuna və yazıla bilər.

Yüksəldilmiş imtiyazlar tələb edən tapşırıqları yerinə yetirmək üçün istifadəçilər **`sudo`** əmrindən istifadə edə bilərlər. **`sudo`** əmri, **"superuser do"** (superistifadəçi etsin) sözünün qısaltması olub, icazə verilmiş istifadəçilərə **başqa bir istifadəçinin**, adətən superistifadəçinin və ya **`root`**-un təhlükəsizlik imtiyazları ilə əmrləri icra etməyə imkan verir. Bu, istifadəçilərə sistem təhlükəsizliyini qorumaq üçün ən yaxşı təcrübə olan **`root` istifadəçisi kimi daxil olmadan** inzibati tapşırıqları yerinə yetirməyə imkan verir. **`sudo`** icazələrini "Linux Təhlükəsizliyi" bölməsində daha ətraflı araşdıracağıq.

### `root` Kimi İcra Etmə

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo cat /etc/shadow
root:<SNIP>:18395:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
<SNIP>
```

İstifadəçi idarəetməsini daha yaxşı başa düşməyə və onunla işləməyə kömək edəcək bir siyahı:

| Əmr | Təsvir |
| :--- | :--- |
| **`sudo`** | Əmri fərqli bir istifadəçi kimi icra edin. |
| **`su`** | **`su`** köməkçi proqramı (utility) **PAM** vasitəsilə müvafiq istifadəçi etimadnamələrini tələb edir və həmin istifadəçi ID-sinə keçir (defolt istifadəçi **superistifadəçidir**). Daha sonra bir qabıq (shell) icra olunur. |
| **`useradd`** | Yeni istifadəçi yaradır və ya defolt yeni istifadəçi məlumatlarını yeniləyir. |
| **`userdel`** | Bir istifadəçi hesabını və əlaqəli faylları silir. |
| **`usermod`** | Bir istifadəçi hesabını dəyişdirir. |
| **`addgroup`** | Sistemə bir qrup əlavə edir. |
| **`delgroup`** | Sistemdən bir qrupu çıxarır. |
| **`passwd`** | İstifadəçi parolunu dəyişdirir. |

-----

## Təcrübənin Əhəmiyyəti

İstifadəçi hesablarının, icazələrin və autentifikasiya mexanizmlərinin necə işlədiyini başa düşmək, həssas nöqtələri müəyyənləşdirməyə, səhv konfiqurasiyalardan istifadə etməyə və bir sistemin təhlükəsizlik vəziyyətini effektiv şəkildə qiymətləndirməyə imkan verir. İstifadəçi idarəetməsində bacarıq qazanmağın ən təsirli yolu, **fərdi əmrləri müxtəlif seçimləri ilə birlikdə nəzarətli bir mühitdə praktikada tətbiq etməkdir**.

Müxtəlif əmrlərlə sərbəst şəkildə təcrübə edin və onların funksionallığını araşdırın. Nəyə nail olmaq istədiyinizə qərar verərkən yaradıcılığınızın sizi idarə etməsinə icazə vermək vacibdir. Bu istifadəçi idarəetmə vasitələrini əvvəlki bölmələrdən əldə etdiyiniz biliklərlə birləşdirərək, nə qədər öyrəndiyinizi dərk edəcəksiniz. Linux sistemi haqqındakı anlayışınızı tətbiq edin: **yeni istifadəçi hesabları yaradın, bu istifadəçilər üçün fayllar və qovluqlar qurun, faylları seçin, xüsusi elementləri oxuyun və süzün və onları yaratdığınız yeni istifadəçilərin fayllarına və qovluqlarına yönləndirin.** Hərtərəfli araşdırmaqdan çəkinməyin. Hədəf sisteminizdə düzəldilə bilməyən heç nə yoxdur və hətta bir şey səhv getsə belə, hədəfi sıfırlamaq və özünüzə güvənənə qədər yenidən başlamaq imkanınız var.

Bu alətlərlə işləyərkən hansı maraqlı nəticələrə rast gəldiniz? 🤔
