**İnteraktiv Shell-lərin Yaradılması**

Əvvəlki bölmənin sonunda, hədəflə shell sessiyası qurmuşduq. Əvvəlcə shell-imiz məhdud idi (bəzən həbs shell-i də deyilir), ona görə də daha çox əmrə və işləmək üçün prompta giriş əldə etmək üçün python-dan istifadə edərək TTY bourne shell yaratdıq. Bu, Hack The Box-da və real iş mühitlərində təcrübə etdikcə özümüzü daha çox tapa biləcəyimiz bir vəziyyət olacaq.

Ola bilər ki, məhdud shell olan bir sistemə düşək və Python quraşdırılmayıb. Bu hallarda, interaktiv shell yaratmaq üçün bir neçə müxtəlif metoddan istifadə edə biləcəyimizi bilmək yaxşıdır. Gəlin onlardan bəzilərinə nəzər salaq.

Nəzərə alın ki, nə vaxt `/bin/sh` və ya `/bin/bash` görsək, bu, həmin sistemdə mövcud olan shell interpretator dili ilə əlaqəli ikili fayl ilə əvəz edilə bilər. Əksər Linux sistemlərində, sistemdə yerli olaraq bourne shell (`/bin/sh`) və bourne again shell (`/bin/bash`) ilə qarşılaşacağıq.

**/bin/sh -i**

Bu əmr, yolda göstərilən shell interpretatorunu interaktiv rejimdə (`-i`) icra edəcək.

**İnteraktiv**
```bash
/bin/sh -i
sh: no job control in this shell
sh-4.2$
```

**Perl**

Əgər sistemdə Perl proqramlaşdırma dili mövcuddursa, bu əmrlər göstərilən shell interpretatorunu icra edəcək.

**Perl-dən Shell-ə**
```bash
perl —e 'exec "/bin/sh";'
```
```bash
perl: exec "/bin/sh";
```
Birbaşa yuxarıdakı əmr scriptdən işlədilməlidir.

**Ruby**

Əgər sistemdə Ruby proqramlaşdırma dili mövcuddursa, bu əmr göstərilən shell interpretatorunu icra edəcək:

**Ruby-dən Shell-ə**
```bash
ruby: exec "/bin/sh"
```
Birbaşa yuxarıdakı əmr scriptdən işlədilməlidir.

**Lua**

Əgər sistemdə Lua proqramlaşdırma dili mövcuddursa, aşağıdakı tam əmrdən istifadə edərək `os.execute` metodu ilə göstərilən shell interpretatorunu icra edə bilərik:

**Lua-dan Shell-ə**
```bash
lua: os.execute('/bin/sh')
```
Birbaşa yuxarıdakı əmr scriptdən işlədilməlidir.

**AWK**

AWK, əksər UNIX/Linux əsaslı sistemlərdə mövcud olan C oxşarı nümunə skan edən və emal edən dildir, developerlar və sistem administratorları tərəfindən hesabatlar yaratmaq üçün geniş istifadə olunur. O, həmçinin interaktiv shell yaratmaq üçün istifadə edilə bilər. Bu, aşağıdakı qısa awk scriptində göstərilib:

**AWK-dan Shell-ə**
```bash
awk 'BEGIN {system("/bin/sh")}'
```

**Find**

Find, əksər Unix/Linux sistemlərində mövcud olan, müxtəlif meyarlardan istifadə edərək fayl və qovluqları axtarmaq və nəzərdən keçirmək üçün geniş istifadə olunan bir əmrdir. O, həmçinin tətbiqləri icra etmək və shell interpretatorunu işə salmaq üçün istifadə edilə bilər.

**Shell üçün Find-dan istifadə**
```bash
find / -name faylinadi -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```
Find əmrindən bu istifadə `-name` seçimindən sonra sadalanan hər hansı faylı axtarır, sonra `awk` (`/bin/awk`) icra edir və shell interpretatorunu icra etmək üçün awk bölməsində müzakirə etdiyimiz eyni scripti işlədir.

**Shell Başlatmaq üçün Exec-dən istifadə**
```bash
find . -exec /bin/sh \; -quit
```
Find əmrindən bu istifadə, shell interpretatorunu birbaşa işə salmaq üçün icra seçimindən (`-exec`) istifadə edir. Əgər find göstərilən faylı tapa bilmirsə, onda heç bir shell əldə edilməyəcək.

**VIM**

Bəli, məşhur əmr sətiri əsaslı mətn redaktoru VIM-in daxilindən shell interpretator dilini təyin edə bilərik. Bu metoddan istifadə etmək ehtiyacı duyacağımız çox spesifik bir vəziyyətdir, amma ehtiyat halda bilmək yaxşıdır.

**Vim-dən Shell-ə**
```bash
vim -c ':!/bin/sh'
```

**Vim Escape**
```bash
vim
:set shell=/bin/sh
:shell
```

**İcra İcazələri Məsələləri**

Yuxarıda sadalanan bütün seçimləri bilməyə əlavə olaraq, shell sessiyasının hesabı ilə malik olduğumuz icazələrə diqqət etməliyik. Hər hansı bir fayl və ya ikili fayl üzərində hesabımızın malik olduğu fayl xassələrini və icazələri sadalamaq üçün həmişə bu əmri işlətməyə cəhd edə bilərik:

**İcazələr**
```bash
ls -la <faylın/yaxud/ikilinin/yolu>
```

Həmçinin, düşdüyümüz hesabın hansı sudo icazələrinə malik olduğunu yoxlamaq üçün bu əmri işlətməyə cəhd edə bilərik:

**Sudo -l**
```bash
sudo -l
```
```bash
Matching Defaults entries for apache on ILF-WebSrv:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User apache may run the following commands on ILF-WebSrv:
    (ALL : ALL) NOPASSWD: ALL
```

Yuxarıdakı `sudo -l` əmrinin işləməsi üçün sabit bir interaktiv shell lazımdır. Əgər tam shell-də deyilsinizsə və ya qeyri-sabit bir shell-dəsinizsə, ondan heç bir cavab ala bilməyə bilərsiniz. Təkcə icazələri nəzərə almaq bizə hansı əmrləri icra edə biləcəyimizi göstərməyəcək, həm də imtiyazları artırmağa imkan verə biləcək potensial vektorlar haqqında fikir verməyə başlaya bilər.
