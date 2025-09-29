### Prompt Təsviri

**Bash prompt** başa düşülməsi sadədir. Susmaya görə, bu prompt bizə aşağıdakı məlumatları göstərir:

* istifadəçi adımız (**username**) – biz kimik,
* kompüterimizin adı (**hostname**) – hansı cihazda işləyirik,
* hazırda işlədiyimiz qovluq (**current working directory**).

Bu, ekranda sistemin hazır olduğunu göstərən bir mətn sətridir. Prompt yeni sətrdə görünür və imlec (**cursor**, yanıb-sönən xətt və ya kvadrat) onun ardınca yerləşir ki, biz əmri daxil edə bilək.

Prompt istifadəçiyə faydalı məlumatlar verməsi üçün fərdiləşdirilə bilər. Format, məsələn, belə görünə bilər:

```
<username>@<hostname><current working directory>$
```

İstifadəçinin ev qovluğu (**home directory**) tilde (`~`) ilə işarələnir və biz sistemə daxil olduğumuz zaman susmaya görə açılan qovluq olur:

```
<username>@<hostname>[~]$
```

Bu nümunədəki **dollar işarəsi ($)** istifadəçini göstərir. Əgər biz **root** kimi daxil olsaq, bu işarə **hash (#)** ilə əvəzlənir:

```
root@htb[/htb]#
```

Bəzən hədəf sistemdə shell işə saldığımızda istifadəçi adı, host adı və ya qovluq göstərilməyə bilər. Bu, ətraf mühitdəki **PS1 dəyişəninin** düzgün qurulmaması ilə əlaqəli ola bilər. Belə halda prompt aşağıdakı kimi görünür:

* **İmtiyazsız istifadəçi (user):**

```
$
```

* **İmtiyazlı istifadəçi (root):**

```
#
```

---

#### PS1 dəyişəni

Linux sistemlərində **PS1 dəyişəni** terminaldakı komanda promptunun necə görünəcəyini idarə edir. Bu dəyişən hər dəfə əmri yazmaq üçün sistem hazır olduqda gördüyümüz mətni təyin edən bir **şablon** kimidir. PS1-i fərdiləşdirərək istifadəçi adı, host adı, cari qovluq və hətta rənglər və xüsusi simvollar əlavə edə bilərik.

Bu, komanda sətrini həm daha informativ, həm də daha vizual cazibədar etməyə imkan verir.

Məsələn, prompt-u fərdiləşdirərək aşağıdakı məlumatları göstərə bilərik:

* İP ünvanı
* tarix və saat
* sonuncu əmrin uğurlu olub-olmaması
* tam qovluq yolu və s.

Bu cür dəyişikliklər xüsusilə **penetrasiya testlərində** faydalıdır, çünki hərəkətləri izləmək və sənədləşdirmək asanlaşır. Həmçinin, `.bash_history` faylı vasitəsilə işlədilmiş əmrləri tarix və vaxta görə izləmək mümkündür.

---

#### Fərdiləşdirmə nümunələri

Prompt fərdiləşdirilməsi üçün `.bashrc` faylında xüsusi simvollardan və dəyişənlərdən istifadə olunur.

Məsələn:

* `\u` – istifadəçi adı
* `\h` – host adı
* `\w` – cari qovluğun tam yolu

---

#### Xüsusi simvollar cədvəli

| Simvol         | Təsviri                                    |
| -------------- | ------------------------------------------ |
| `\d`           | Tarix (Mon Feb 6)                          |
| `\D{%Y-%m-%d}` | Tarix (YYYY-MM-DD)                         |
| `\H`           | Tam host adı                               |
| `\j`           | Shell tərəfindən idarə olunan işlərin sayı |
| `\n`           | Yeni sətir                                 |
| `\r`           | Carriage return                            |
| `\s`           | Shell-in adı                               |
| `\t`           | Cari vaxt (24 saatlıq, HH:MM:SS)           |
| `\T`           | Cari vaxt (12 saatlıq, HH:MM:SS)           |
| `\@`           | Cari vaxt                                  |
| `\u`           | İstifadəçi adı                             |
| `\w`           | Cari qovluğun tam yolu                     |

---

#### Nəticə

Prompt-un fərdiləşdirilməsi terminal təcrübəsini daha **faydalı** və **səmərəli** edə bilər. Bu, həm də **troubleshooting** zamanı sistemin vəziyyəti haqqında əhəmiyyətli məlumat verir.

Bundan əlavə, terminal mühitini rəng sxemləri, şriftlər və digər parametrlərlə fərdiləşdirmək mümkündür.

Windows GUI-də olduğu kimi burada da biz hansı istifadəçi ilə daxil olduğumuzu, hansı kompüterdə işlədiyimizi və hansı qovluqda olduğumuzu görürük. Bash prompt isə tamamilə ehtiyaclarımıza uyğun olaraq dəyişdirilə və fərdiləşdirilə bilər.

Bu mövzunun dərin fərdiləşdirməsi modulun əhatəsindən kənardır, lakin **bash-prompt-generator** və **powerline** kimi alətlər bizə öz ehtiyaclarımıza uyğun prompt qurmağa imkan verir.
