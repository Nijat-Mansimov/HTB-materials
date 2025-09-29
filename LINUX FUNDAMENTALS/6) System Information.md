### Sistem Məlumatı

İndi terminal və shell-dən istifadə etməklə rahat olmaq üçün bir az praktiki məşqlərə başlayaq. Yadda saxla ki, lazım olduqda kömək üçün həmişə `-h`, `--help` və ya `man` komandalarından istifadə edə bilərsən.

Müxtəlif Linux sistemləri ilə işləyəcəyimiz üçün onların quruluşunu — sistem detalları, proseslər, şəbəkə konfiqurasiyaları, istifadəçilər/istifadəçi ayarları və qovluqlar, həmçinin onların parametrlərini anlamaq vacibdir. Aşağıda bu məlumatı toplamağa kömək edən əsas alətlərin siyahısı verilib. Onların çoxu əvvəlcədən quraşdırılıb. Bununla belə, bu bilik yalnız gündəlik Linux tapşırıqları üçün deyil, həm də təhlükəsizlik konfiqurasiyalarını qiymətləndirmək, zəiflikləri müəyyən etmək və potensial təhlükəsizlik risklərinin qarşısını almaq üçün çox vacibdir.

| Komanda    | İzah                                                                                                  |
| ---------- | ----------------------------------------------------------------------------------------------------- |
| `whoami`   | Cari istifadəçi adını göstərir.                                                                       |
| `id`       | İstifadəçinin şəxsiyyətini qaytarır.                                                                  |
| `hostname` | Cari host sisteminin adını göstərir və ya təyin edir.                                                 |
| `uname`    | Əməliyyat sistemi və aparat haqqında əsas məlumatları çap edir.                                       |
| `pwd`      | İş qovluğunun adını qaytarır.                                                                         |
| `ifconfig` | Şəbəkə interfeysinə ünvan təyin etmək və/və ya parametrləri konfiqurasiya etmək üçün istifadə olunur. |
| `ip`       | Routing, şəbəkə cihazları, interfeyslər və tunelləri göstərmək və idarə etmək üçün istifadə olunur.   |
| `netstat`  | Şəbəkə statusunu göstərir.                                                                            |
| `ss`       | Soketləri araşdırmaq üçün başqa bir vasitədir.                                                        |
| `ps`       | Proseslərin statusunu göstərir.                                                                       |
| `who`      | Sistemdə kimlərin daxil olduğunu göstərir.                                                            |
| `env`      | Ətraf mühit dəyişənlərini çap edir və ya komanda icra edir.                                           |
| `lsblk`    | Blok cihazlarını siyahıya alır.                                                                       |
| `lsusb`    | USB cihazlarını siyahıya alır.                                                                        |
| `lsof`     | Açıq faylları siyahıya alır.                                                                          |
| `lspci`    | PCI cihazlarını siyahıya alır.                                                                        |

Səhifənin sonuna qədər sürüşdürək, hədəf maşını işə salaq, sonra SSH vasitəsilə ona qoşulaq. Daha sonra isə bölmədə göstərilən misalları izləməyə və onları təkrarlamağa çalışaq.

---

### SSH ilə Giriş

Secure Shell (SSH) müştərilərə uzaq kompüterlərdə əmrləri və hərəkətləri icra etməyə imkan verən protokoldur. Linux əsaslı host və serverlərdə, eləcə də digər Unix-oxşar əməliyyat sistemlərində SSH daimi quraşdırılmış standart alətlərdən biridir və bir çox administratorlar tərəfindən kompüteri uzaqdan konfiqurasiya və idarə etmək üçün üstünlük verilən seçimdir. Bu, daha köhnə və özünü sübut etmiş protokoldur, qrafik interfeys (GUI) tələb etmir və təklif etmir. Buna görə də çox səmərəli işləyir və çox az resurs tələb edir. Biz bu cür bağlantılardan aşağıdakı bölmələrdə və digər laboratoriya məşqlərində istifadə edəcəyik.

Hədəfimizə qoşulmaq üçün aşağıdakı komandanı işlədə bilərik:

```bash
nijatmansimov@htb[/htb]$ ssh htb-student@[IP address]
```

---

### Hostname

`hostname` komandası sadəcə olaraq daxil olduğumuz kompüterin adını çap edir.

```bash
nijatmansimov@htb[/htb]$ hostname
nixfund
```

---

### Whoami

Bu sadə komanda həm Windows, həm də Linux sistemlərində istifadə edilə bilər və cari istifadəçi adını göstərir. Təhlükəsizlik qiymətləndirilməsi zamanı hostda reverse shell əldə etdikdə, ilk öyrənməli olduğumuz şey hansı istifadəçi ilə işlədiyimizi bilməkdir.

```bash
cry0l1t3@htb[/htb]$ whoami
cry0l1t3
```

---

### Id

`id` komandası `whoami`-dən genişdir və istifadəçinin qruplarını və ID-lərini göstərir. Penetrasiya testçiləri üçün bu maraqlıdır, çünki istifadəçinin hansı qruplarda olduğunu görmək olar (məsələn `sudo` qrupu). Sistem administratorları üçün isə audit zamanı hansı istifadəçilərin artıq icazələrə sahib olduğunu yoxlamaq üçün faydalıdır.

```bash
cry0l1t3@htb[/htb]$ id
uid=1000(cry0l1t3) gid=1000(cry0l1t3) groups=1000(cry0l1t3),1337(hackthebox),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)
```

---

### Uname

`uname` əmri sistem haqqında məlumat çap edir. `man uname` yazaraq bütün seçimləri görə bilərik.

Ən çox istifadə olunanlar:

* `uname -a` → bütün məlumatı çap edir (kernel, host adı, versiya, arxitektura və s.).
* `uname -r` → kernel versiyasını göstərir (exploit axtarışı üçün çox önəmlidir).

```bash
cry0l1t3@htb[/htb]$ uname -a
Linux box 4.15.0-99-generic #100-Ubuntu SMP Wed Apr 22 20:32:56 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
```

```bash
cry0l1t3@htb[/htb]$ uname -r
4.15.0-99-generic
```

---

### Qeyd

Bu komandaların hamısını öyrənmək və `man` səhifələrini oxumaq çox faydalıdır. Çünki bu biliklər təkcə gündəlik Linux işlərində deyil, həm də zəifliklər və yanlış konfiqurasiyaları tapmaq üçün istifadə olunur.

