# İcazələrin İdarə Edilməsi (Permission Management)

Linuxda icazələr, fayl və qovluqlara (directories) girişi idarə edən açarlar kimidir. Bu icazələr, bir təşkilat daxilində xüsusi şəxslərə və komandalara paylanan açarlar kimi, **həm istifadəçilərə, həm də qruplara** təyin edilir. Hər bir istifadəçi birdən çox qrupa aid ola bilər və bir qrupun hissəsi olmaq əlavə giriş hüquqları verir, istifadəçilərə fayl və qovluqlar üzərində xüsusi əməliyyatlar aparmağa imkan verir.

Hər bir fayl və qovluğun bir **sahibi (istifadəçi)** var və bir **qrup** ilə əlaqələndirilir. Bu fayllar üçün icazələr həm sahib, həm də qrup üçün müəyyən edilir, hansı əməliyyatların—oxumaq, yazmaq və ya icra etmək kimi—icazəli olduğunu təyin edir. Yeni bir fayl və ya qovluq yaratdığınızda, o avtomatik olaraq "sizin" olur və aid olduğunuz qrupla əlaqələndirilir, eynilə bir şirkət daxilindəki layihənin defolt olaraq sizin komandanızın nəzarətinə verilməsi kimi.

Mahiyyət etibarilə, Linux icazələri, müəyyən resurslara kimin daxil ola və ya dəyişdirə biləcəyini diktə edən bir qaydalar və ya açarlar dəsti kimi çıxış edir, sistem daxilində təhlükəsizliyi və düzgün əməkdaşlığı təmin edir.

-----

## Qovluq Naviqasiyası və İcrası

Bir istifadəçi bir Linux qovluğunun məzmununa daxil olmaq istədikdə, bu, içəri girməzdən əvvəl bir qapını açmağa bənzəyir. Bir qovluğun içinə **"keçmək" (traverse)** və ya naviqasiya etmək üçün istifadəçinin əvvəlcə doğru açara sahib olması lazımdır—bu açar qovluq üzərindəki **icra etmə (`x`) icazəsidir**. Bu icazə olmadan, qovluğun məzmunu istifadəçiyə görünən olsa belə, ora daxil ola və ya içində hərəkət edə bilməyəcəklər.

Başqa sözlə, bir qovluq üzərində **icra etmə icazəsinə** sahib olmaq, içəridəki otaqlara daxil olmaq üçün bir dəhlizdən keçməyə icazəyə sahib olmaq kimidir. Bu, içəridəkiləri görməyə və ya dəyişdirməyə imkan vermir, lakin qovluğun strukturuna daxil olmaq və onu araşdırmaq imkanı verir. Bu icazə olmadan istifadəçi qovluğun məzmununa daxil ola bilməz və bunun əvəzinə **"İcazə Rədd Edildi" (Permission Denied)** xətası ilə qarşılaşacaq.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ ls -l
drw-rw-r-- 3 cry0l1t3 cry0l1t3 4096 Jan 12 12:30 scripts

cry0l1t3@htb[/htb]$ ls -al mydirectory/
ls: cannot access 'mydirectory/script.sh': Permission denied
ls: cannot access 'mydirectory/..': Permission denied
ls: cannot access 'mydirectory/subdirectory': Permission denied
ls: cannot access 'mydirectory/.': Permission denied
total 0
d????????? ? ? ? ?            ? .
d????????? ? ? ? ?            ? ..
-????????? ? ? ? ?            ? script.sh
d????????? ? ? ? ?            ? subdirectory
```

Qeyd etmək vacibdir ki, **icra etmə icazələri** istifadəçinin giriş səviyyəsindən asılı olmayaraq bir qovluqda naviqasiya etmək üçün zəruridir. Həmçinin, bir qovluq üzərində **icra etmə icazələri** istifadəçiyə qovluq daxilindəki hər hansı bir faylı icra etməyə və ya dəyişdirməyə imkan vermir, yalnız qovluğun məzmununda naviqasiya etməyə və ona daxil olmağa imkan verir.

Qovluq daxilindəki faylları icra etmək üçün istifadəçinin müvafiq fayl üzərində **icra etmə icazələrinə** ehtiyacı var. Bir qovluğun məzmununu (fayl və alt qovluq yaratmaq, silmək və ya adını dəyişdirmək) dəyişdirmək üçün istifadəçinin qovluq üzərində **yazma icazələrinə** ehtiyacı var.

-----

## İcazə Sistemi

Linux sistemlərində bütün icazə sistemi **oktal say sisteminə** əsaslanır və əsasən bir fayl və ya qovluğa təyin oluna bilən üç fərqli icazə növü var:

  * **r** - **Oxuma (Read)**
  * **w** - **Yazma (Write)**
  * **x** - **İcra etmə (Execute)**

İcazələr növbəti nümunədə göstərildiyi kimi uyğun icazələrlə **sahib**, **qrup** və **digərləri** üçün təyin edilə bilər.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ ls -l /etc/passwd
- rwx rw- r--   1 root root 1641 May  4 23:42 /etc/passwd
- --- --- ---   |  |    |    |   |__________|
|  |   |   |    |  |    |    |        |_ Tarix (Date)
|  |   |   |    |  |    |    |__________ Fayl Ölçüsü (File Size)
|  |   |   |    |  |    |_______________ Qrup (Group)
|  |   |   |    |  |____________________ İstifadəçi (User)
|  |   |   |    |_______________________ Sərt keçidlərin sayı (Number of hard links)
|  |   |   |_ Digərlərinin icazəsi (yalnız oxuma)
|  |   |_____ Qrupun icazələri (oxuma, yazma)
|  |_________ Sahibin icazələri (oxuma, yazma, icra etmə)
|____________ Fayl növü (- = Fayl, d = Qovluq, l = Keçid, ... )
```

-----

## İcazələrin Dəyişdirilməsi

İcazələri **`chmod`** əmrindən, icazə qrupu istinadlarından (**`u`** - sahibi, **`g`** - Qrup, **`o`** - digərləri, **`a`** - Bütün istifadəçilər) və təyin olunmuş icazələri əlavə etmək və ya silmək üçün bir **`[+]`** və ya **`[-]`** istifadə edərək dəyişdirə bilərik. Aşağıdakı nümunədə, **`shell`** adlı bir faylımız olduğunu fərz edək və bu skriptin sahibi tərəfindən idarə olunduğu, icra edilə bilən olmadığı və bütün istifadəçilər üçün oxuma/yazma icazələri ilə təyin edildiyi şəkildə icazələri dəyişdirmək istəyirik.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ ls -l shell
-rwxr-x--x   1 cry0l1t3 htbteam 0 May  4 22:12 shell
```

Daha sonra bütün istifadəçilər üçün **oxuma** icazələrini tətbiq edə və nəticəni görə bilərik.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ chmod a+r shell && ls -l shell
-rwxr-xr-x   1 cry0l1t3 htbteam 0 May  4 22:12 shell
```

Bütün digər istifadəçilər üçün icazələri **yalnız oxumaq** olaraq **oktal dəyər təyinatından** istifadə edərək də təyin edə bilərik.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ chmod 754 shell && ls -l shell
-rwxr-xr--   1 cry0l1t3 htbteam 0 May  4 22:12 shell
```

İcazə təyinatının necə hesablandığını daha yaxşı başa düşmək üçün onunla əlaqəli bütün təsvirlərə baxaq.

Müntəzəm İfadələr

```
İkili Göstərim (Binary Notation):       4 2 1  |  4 2 1  |  4 2 1
----------------------------------------------------------
İkili Təmsil (Binary Representation):   1 1 1  |  1 0 1  |  1 0 0
----------------------------------------------------------
Oktal Dəyər (Octal Value):              7    |    5    |    4
----------------------------------------------------------
İcazə Təmsili (Permission Representation): r w x  |  r - x  |  r - -
```

Əgər **İkili Göstərim**-dən **İkili Təmsil**-də təyin edilmiş bitləri birlikdə toplasaq, **Oktal Dəyəri** alırıq. **İcazə Təmsili** isə təyin edilmiş icazələri asanlıqla tanımaq üçün üç simvoldan istifadə edərək **İkili Təmsil**-də təyin olunmuş bitləri təmsil edir.

-----

## Sahibin Dəyişdirilməsi

Bir fayl və ya qovluğun **sahibini və/və ya qrup təyinatlarını** dəyişdirmək üçün **`chown`** əmrindən istifadə edə bilərik. Sintaksis aşağıdakı kimidir:

Sintaksis - chown

```bash
cry0l1t3@htb[/htb]$ chown <istifadəçi>:<qrup> <fayl/qovluq>
```

Bu nümunədə, "shell" istənilən ixtiyari fayl və ya qovluqla əvəz edilə bilər.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ chown root:root shell && ls -l shell
-rwxr-xr--   1 root root 0 May  4 22:12 shell
```

-----

## SUID və SGID

Standart istifadəçi və qrup icazələrinə əlavə olaraq, Linux fayllar üzərində **İstifadəçi ID-ni Təyin Et (SUID)** və **Qrup ID-ni Təyin Et (SGID)** bitləri vasitəsilə xüsusi icazələri konfiqurasiya etməyə imkan verir. Bu bitlər, istifadəçilərə müəyyən proqramları **başqa bir istifadəçinin və ya qrupun imtiyazları ilə** işə salmağa imkan verən **müvəqqəti giriş icazələri** kimi fəaliyyət göstərir. Məsələn, administratorlar istifadəçilərə xüsusi tətbiqlər üçün **yüksək səviyyəli hüquqlar** vermək üçün **SUID** və ya **SGID** istifadə edə bilərlər, bu da istifadəçinin özünün normalda onlara sahib olmasa belə, zəruri icazələrlə tapşırıqların yerinə yetirilməsinə imkan verir.

Bu icazələrin olması faylın icazə dəstindəki adi **`x`**-in yerinə **`s`** ilə göstərilir. **SUID** və ya **SGID** biti təyin olunmuş bir proqram icra edildikdə, onu işə salan istifadəçinin deyil, faylın sahibinin və ya qrupunun icazələri ilə işləyir. Bu, müəyyən sistem tapşırıqları üçün faydalı ola bilər, lakin diqqətlə istifadə edilmədikdə potensial təhlükəsizlik riskləri də yaradır.

Ümumi risklərdən biri, tətbiqin tam funksionallığı ilə tanış olmayan administratorların **SUID** və ya **SGID** bitlərini fərq qoymadan təyin etməsidir. Məsələn, əgər **SUID** biti interfeysindən bir qabığı (shell) işə salmaq funksiyasını ehtiva edən **`journalctl`** kimi bir proqrama tətbiq olunarsa, bu proqramı işə salan istənilən istifadəçi **`root`** olaraq bir qabığı icra edə bilər. Bu, onlara sistem üzərində tam nəzarət verir və əhəmiyyətli bir təhlükəsizlik boşluğu yaradır. Bu və digər bu cür tətbiqlər haqqında daha çox məlumatı **GTFObins**-də tapa bilərsiniz.

-----

## Yapışqan Bit (Sticky Bit)

Linuxdakı **yapışqan bitlər** (sticky bits) **paylaşılan yerlərdəki fayllar üzərindəki qıfıllar** kimidir. Bir qovluğa təyin edildikdə, yapışqan bit əlavə bir təhlükəsizlik qatı əlavə edir, **başqalarının qovluğa girişi olsa belə, yalnız müəyyən şəxslərin faylları dəyişdirə və ya silə bilməsini** təmin edir.

Bir çox insanın daxil ola və eyni alətlərdən istifadə edə biləcəyi, lakin hər kəsin yalnız özünün (və ya menecerin) aça biləcəyi öz çekmecəsi olduğu bir ortaq iş yerini təsəvvür edin. Yapışqan bit bu çekmecələr üzərindəki bir qıfıl kimi çıxış edir, başqalarının məzmunu dəyişdirməsinə mane olur. Paylaşılan bir qovluqda bu o deməkdir ki, yalnız **faylın sahibi, qovluğun sahibi və ya `root` istifadəçisi** (sistem administratoru) faylları silə və ya adını dəyişdirə bilər. Digər istifadəçilər hələ də qovluğa daxil ola bilər, lakin sahib olmadıqları faylları dəyişdirə bilməzlər.

Bu xüsusiyyət, bir neçə istifadəçinin birlikdə işlədiyi ictimai qovluqlar kimi paylaşılan mühitlərdə xüsusilə faydalıdır. Yapışqan biti təyin etməklə, vacib faylların səlahiyyəti olmayan biri tərəfindən təsadüfən və ya qəsdən dəyişdirilmədiyini təmin edir, əməkdaşlıq iş yerlərinə vacib bir təhlükəsizlik tədbiri əlavə edir.

Müntəzəm İfadələr

```bash
cry0l1t3@htb[/htb]$ ls -l
drw-rw-r-t 3 cry0l1t3 cry0l1t3 4096 Jan 12 12:30 scripts

cry0l1t3@htb[/htb]$ ls -ld reports
drw-rw-r-T 3 cry0l1t3 cry0l1t3 4096 Jan 12 12:32 reports
```

Bu nümunədə, hər iki qovluğun **yapışqan bitinin** təyin edildiyini görürük. Lakin, **`reports`** qovluğunda **böyük `T`**, **`scripts`** qovluğunda isə **kiçik `t`** var.

Əgər yapışqan bit **böyük hərflə (`T`)** yazılıbsa, bu o deməkdir ki, **bütün digər istifadəçilərin icra etmə (`x`) icazələri yoxdur** və buna görə də qovluğun məzmununu görə və ya oradan hər hansı bir proqramı işə sala bilməzlər. **Kiçik hərflə yazılmış yapışqan bit (`t`)** isə **icra etmə (`x`) icazələrinin təyin edildiyi** yapışqan bitdir.
