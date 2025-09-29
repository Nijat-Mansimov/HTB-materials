# Faylları Redaktə Etmək

Faylları və qovluqları yaratmağı öyrəndikdən sonra, bu fayllarla işləməyə keçək. Linux-da faylları redaktə etməyin bir neçə yolu var, ən çox istifadə olunan mətn redaktorlarından bəziləri Vi və Vim-dir. Lakin, əvvəlcə daha sadə başa düşülən Nano redaktorundan başlayaq.

Nano ilə fayl yaratmaq və redaktə etmək üçün redaktoru işə salarkən birinci parametr kimi fayl adını göstərə bilərsiniz. Məsələn, `notes.txt` adlı yeni fayl yaratmaq və açmaq üçün belə bir əmrdən istifadə edirik:

```bash
nijatmansimov@htb[/htb]$ nano notes.txt
```

Bu əmr Nano redaktorunu açacaq və dərhal `notes.txt` faylını redaktə etməyə başlaya bilərsiniz. Nano-nun sadə interfeysi (həmçinin "pager" adlanır) mətn fayllarını tez redaktə etmək üçün idealdır, xüsusilə yeni başlayanlar üçün.

## Nano Redaktoru

Nano redaktorunda ekran belə görünür:

```
GNU nano 2.9.3                                    notes.txt                                              

^G Get Help    ^O Write Out   ^W Where Is    ^K Cut Text    ^J Justify     ^C Cur Pos     M-U Undo
^X Exit        ^R Read File   ^\ Replace     ^U Uncut Text  ^T To Spell    ^_ Go To Line  M-E Redo
```

Buradakı `^` işarəsi `[CTRL]` düyməsini göstərir. Məsələn, `[CTRL + W]` düymələrini basanda ekranın aşağı hissəsində "Search:" sətri görünür və burada axtarmaq istədiyiniz sözü daxil edə bilərsiniz. Sonra `[ENTER]` basanda kursor həmin sözü tapacaq.

Faylı saxlamaq üçün `[CTRL + O]` basın və fayl adını təsdiqləyin `[ENTER]`. Faylı saxlamağı bitirdikdən sonra `[CTRL + X]` ilə redaktordan çıxın.

### Shell-də Fayl Məzmununu Görmək

Faylın məzmununu görmək üçün `cat` əmrindən istifadə edə bilərsiniz:

```bash
nijatmansimov@htb[/htb]$ cat notes.txt
Here we can type everything we want and make our notes.
```

Linux sistemlərində bəzi fayllar penetrasiya testçiləri üçün çox faydalı ola bilər. Məsələn, `/etc/passwd` faylı sistemdəki istifadəçilər haqqında məlumat (istifadəçi adı, UID, GID və ev qovluğu) saxlayır. Tarixi olaraq bu fayl parol hash-lərini də saxlayırdı, indi isə `/etc/shadow` faylında daha sərt icazələrlə saxlanılır. Yanlış icazələr təhlükəli məlumatların açıqlanmasına və ya yüksəldilmiş hüquq imkanlarına səbəb ola bilər.

## Vim

Vim də açıq mənbə redaktorudur, Nano kimi ASCII mətnləri redaktə edir. Vi-nin təkmilləşdirilmiş variantıdır. Vim çox güclü və çevik redaktordur, əlavə proqramlarla (grep, awk, sed və s.) birləşdirilərək daha kompleks əməliyyatları asanlıqla yerinə yetirir.

Vim modallıq prinsipi ilə işləyir, yəni mətni və əmrləri ayırd edə bilir. Vim-də əsas 6 rejim mövcuddur:

| Rejim   | Təsviri                                                                                              |
| ------- | ---------------------------------------------------------------------------------------------------- |
| Normal  | Bütün daxilolmalar redaktor əmri kimi qəbul olunur. Fayl redaktəyə əlavə olunmur.                    |
| Insert  | Yazdığınız simvollar kursorun mövqeyinə əlavə olunur.                                                |
| Visual  | Mətnin bir hissəsini seçmək və vizual olaraq vurğulamaq üçün istifadə olunur.                        |
| Command | Redaktorun alt sətrində tək sətrlik əmrlər daxil edilir (məsələn, mətnin dəyişdirilməsi, silinməsi). |
| Replace | Yeni daxil olunan mətn mövcud mətn üzərində yazılır.                                                 |
| Ex      | Ex redaktoru davranışını təqlid edir, bir neçə əmri ardıcıl işlətməyə imkan verir.                   |

Vim redaktorunda `:` yazıb `q` əmri ilə çıxmaq mümkündür:

```
:q
```

Vim həmçinin `vimtutor` adlı məşq imkanına malikdir. Bu, redaktoru öyrənmək və təcrübə keçmək üçün əla vasitədir:

```bash
nijatmansimov@htb[/htb]$ vimtutor
```

Bu təlimçi ilə praktik məşqlər apararaq Vim-də rahat işləməyə başlayacaqsınız. İlk başda çətin görünsə də, öyrəndikdən sonra Vim çox sürətli və səmərəli olacaq.
