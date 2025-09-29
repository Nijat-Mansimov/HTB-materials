### Linux Strukturu – Sənədləşmə

#### Ümumi Məlumat

Linux, bildiyiniz kimi, şəxsi kompüterlərdə, serverlərdə və hətta mobil cihazlarda istifadə olunan əməliyyat sistemidir. Lakin Linux kiber təhlükəsizlik sahəsində əsas sütunlardan biridir, öz möhkəmliyi, çevikliyi və açıq mənbəli olması ilə tanınır. Bu bölmədə Linux-un strukturu, tarixi, fəlsəfəsi, arxitekturası və fayl sistemi iyerarxiyası izah ediləcək — hər bir kiber təhlükəsizlik mütəxəssisi üçün vacib biliklərdir. Bunu yeni avtomobil üçün ilk sürücülük dərsi kimi düşünə bilərsiniz: avtomobilin əsasını, hansı komponentlərdən ibarət olduğunu və niyə hazırkı vəziyyətdə olduğunu öyrənirsiniz.

Linux Windows, macOS, iOS və ya Android kimi bir əməliyyat sistemidir. Əməliyyat sistemi (OS) kompüterin bütün aparat resurslarını idarə edən proqram təminatıdır və proqram tətbiqləri ilə aparat komponentləri arasında əlaqəni təmin edir. Digər əməliyyat sistemlərindən fərqli olaraq, Linux müxtəlif paylamalarda — tez-tez "distro" adlandırılan — mövcuddur. Bu paylamalar müxtəlif ehtiyac və istifadəçi tələblərinə uyğunlaşdırılmış Linux versiyalarıdır.

---

#### Tarixi

Linux nüvəsi və əməliyyat sisteminin yaradılmasına bir sıra hadisələr səbəb olmuşdur:

* 1970: Ken Thompson və Dennis Ritchie Unix əməliyyat sistemini yaratdılar.
* 1977: Berkeley Software Distribution (BSD) buraxıldı, lakin Unix kodunun AT&T-yə məxsus olması səbəbindən inkişaf məhdudlaşdırıldı.
* 1983: Richard Stallman GNU layihəsini başladı və GNU General Public License (GPL) yaradıldı.
* 1991: Linus Torvalds Linux nüvəsini yaratdı. Nüvə C dilində yazılmış və GNU GPL v2 altında lisenziyalaşdırılmışdır.

Linux 600-dən çox paylamada mövcuddur. Ən məşhurları: Ubuntu, Debian, Fedora, OpenSUSE, elementary, Manjaro, Gentoo, RedHat və Linux Mint.

Linux digər əməliyyat sistemlərinə nisbətən daha təhlükəsiz hesab edilir, keçmişdə kernel zəiflikləri olmuş olsa da, bu gün daha az rast gəlinir. Linux Windows-a nisbətən zərərli proqramlara daha az həssasdır və tez-tez yenilənir. Linux həmçinin çox sabitdir və istifadəçiyə yüksək performans təqdim edir. Lakin yeni başlayanlar üçün bir qədər çətin ola bilər və Windows qədər aparat drayverlərinə malik deyil.

Linux açıq mənbəli olduğundan mənbə kodu hər kəs tərəfindən dəyişdirilə və kommersiya və ya qeyri-kommersiya məqsədi ilə paylaşıla bilər. Linux əsaslı əməliyyat sistemləri serverlərdə, əsas çərçivələrdə, masaüstlərdə, routerlərdə, televizorlar, video oyun konsolları və daha çox cihazlarda işləyir. Android əməliyyat sistemi də Linux nüvəsinə əsaslandığı üçün Linux ən geniş yayılmış əməliyyat sistemidir.

Linux Windows, iOS, Android və ya macOS kimi bir əməliyyat sistemidir. OS kompüterin bütün aparat resurslarını idarə edir və proqram ilə aparat arasında kommunikasiya yaradır. Linux-un müxtəlif paylamaları mövcuddur, bu da onu Windows-un müxtəlif versiyalarına bənzədir.

İnteraktiv mühitdə biz Pwnbox-a çıxış əldə edirik, bu Parrot OS-un özəlləşdirilmiş versiyasıdır. Bu modul vasitəsilə işləyəcəyimiz əsas əməliyyat sistemi olacaq. Parrot OS Debian əsaslıdır və təhlükəsizlik, məxfilik və inkişaf üzərinə fokuslanır.

---

#### Linux-u şirkət kimi təsəvvür edin

Linux-u inkişaf edən bir şirkət kimi təsəvvür edin: onun komponentləri xüsusi vəzifələri olan işçilərdir. Arxitektura bu işçilərin departamentlərə necə bölündüyünü və effektiv şəkildə necə ünsiyyət qurduğunu göstərir. Filosofiya isə şirkətin mədəniyyətini və əsas dəyərlərini təmsil edir, işçilərin fərdi və kollektiv şəkildə işləməsini, sadəlik, şəffaflıq və əməkdaşlıq prinsiplərinə riayət etməsini təmin edir.

---

#### Filosofiya

Linux fəlsəfəsi sadəlik, modulyarlıq və açıqlıq üzərində qurulub. Bu, bir işi yaxşı yerinə yetirən kiçik, tək məqsədli proqramların yaradılmasını təşviq edir. Proqramlar birləşdirilərək mürəkkəb əməliyyatları yerinə yetirə bilər. Linux aşağıdakı beş əsas prinsipə əməl edir:

| Prinsip                                            | İzah                                                                                    |
| -------------------------------------------------- | --------------------------------------------------------------------------------------- |
| Hər şey fayldır                                    | Linux-da çalışan bütün xidmətlərin konfiqurasiya faylları mətn fayllarında saxlanılır.  |
| Kiçik, tək məqsədli proqramlar                     | Proqramlar bir işi yaxşı yerinə yetirir və birlikdə işləyə bilər.                       |
| Proqramları zəncirləmə qabiliyyəti                 | Müxtəlif alətlərin birləşdirilməsi mürəkkəb əməliyyatları yerinə yetirməyə imkan verir. |
| Məhdud istifadəçi interfeyslərindən qaçınmaq       | Linux əsasən shell/terminal ilə işləyir və istifadəçiyə daha çox nəzarət verir.         |
| Konfiqurasiya məlumatları mətn faylında saxlanılır | Məsələn, `/etc/passwd` faylı bütün istifadəçiləri saxlayır.                             |

---

#### Komponentlər

| Komponent       | İzah                                                                                                                                |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Bootloader      | Kompüter işə düşərkən əməliyyat sistemini yükləyən kod parçası. Parrot Linux-da GRUB istifadə olunur.                               |
| OS Kernel       | Əməliyyat sisteminin əsas nüvəsi. Aparat resurslarını idarə edir.                                                                   |
| Daemons         | Arxa planda işləyən xidmətlərdir. Məqsəd: cədvəl, çap və multimedia funksiyalarını düzgün işlətmək.                                 |
| OS Shell        | Komanda xətti interfeysi. İstifadəçi əmrlərini nüvəyə ötürür. Ən çox istifadə olunan shell-lər: Bash, Tcsh/Csh, Ksh, Zsh, Fish.     |
| Graphics Server | Qrafik alt-sistem (X-server), qrafik proqramların lokal və ya uzaq işləməsinə imkan verir.                                          |
| Window Manager  | Qrafik istifadəçi interfeysi (GUI). Məsələn, GNOME, KDE, MATE, Unity, Cinnamon. Masaüstü mühiti fayl və veb brauzerləri əhatə edir. |
| Utilities       | İstifadəçi və ya digər proqramlar üçün xüsusi funksiyalar yerinə yetirən proqramlar.                                                |

---

#### Linux Arxitekturası

| Təbəqə         | İzah                                                                                  |
| -------------- | ------------------------------------------------------------------------------------- |
| Hardware       | RAM, CPU, sabit disk və digər periferiya cihazları.                                   |
| Kernel         | Aparat resurslarını virtualizasiya edir və proseslər arasında qarşıdurmaları azaldır. |
| Shell          | Komanda xətti interfeysi, istifadəçi əmrlərini nüvəyə ötürür.                         |
| System Utility | OS funksiyalarını istifadəçiyə təqdim edir.                                           |

---

#### Fayl Sistemi İyerarxiyası

Linux fayl sistemi ağac iyerarxiyası ilə qurulub və **Filesystem Hierarchy Standard (FHS)** sənədlərində göstərilib. Əsas kataloqlar:

* `/` – Kök kataloq, bütün fayl sistemi buradan başlayır.
* `/bin` – Əsas icra edilə bilən proqramlar.
* `/sbin` – Sistem idarəetmə proqramları.
* `/etc` – Konfiqurasiya faylları.
* `/home` – İstifadəçilərin ev kataloqları.
* `/var` – Dəyişən məlumat faylları, loglar.
* `/usr` – İstifadəçi proqramları və kitabxanaları.
* `/tmp` – Müvəqqəti fayllar.
* `/lib` – Sistem kitabxanaları.
* `/dev` – Cihaz faylları.

<img width="2576" height="1656" alt="image" src="https://github.com/user-attachments/assets/5856f4f1-d13b-41df-a085-dff1fb9e792f" />


