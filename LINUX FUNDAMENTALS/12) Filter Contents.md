# Məzmunun Süzülməsi (Filter Contents)

Əvvəlki bölümdə, bir proqramın çıxışını əlavə emal üçün başqa bir proqrama necə yönləndirəcəyimizi araşdırdıq. İndi isə mətn redaktorunu açmağa ehtiyac olmadan faylları birbaşa əmr sətrindən oxumaqdan danışaq.

Bunun üçün iki güclü alət var – **`more`** və **`less`**. Bunlar **pager** (səhifələyicilər) kimi tanınır və sizə bir faylın məzmununu interaktiv şəkildə, bir dəfəyə bir ekran olmaqla görməyə imkan verir. Hər iki alət oxşar məqsədə xidmət etsə də, funksionallıqda bəzi fərqləri var ki, bunlara daha sonra toxunacağıq.

**`more`** və **`less`** istifadə edərək, böyük faylları asanlıqla sürüşdürə, mətn axtara və faylın özünü dəyişdirmədən irəli və ya geri gedə bilərsiniz. Bu, xüsusilə bir ekrana səliqəli şəkildə sığmayan böyük loq faylları və ya mətn faylları ilə işləyərkən faydalıdır.

Bu bölümün məqsədi məzmunu süzməyi və əvvəlki əmrlərdən yönləndirilmiş çıxışı idarə etməyi öyrənməkdir. Amma süzməyə başlamazdan əvvəl, süzməni daha səmərəli və güclü etmək üçün xüsusi olaraq hazırlanmış bəzi əsas alətlər və əmrlərlə tanış olmalıyıq.

Əmrlərin çıxışını süzməyə başlamazdan əvvəl, mətn üzərində səmərəli işləməyə və manipulyasiya etməyə kömək edəcək bir neçə təməl aləti araşdıraq. Bu alətlər böyük miqdarda məlumatla işləyərkən və ya axtarış, sıralama və ya məlumatların emalını əhatə edən tapşırıqları avtomatlaşdırmaq lazım gəldikdə çox vacibdir.

Gəlin bu alətlərin praktikada necə işlədiyini başa düşmək üçün bəzi nümunələrə baxaq.

-----

## **`more`**

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | more
```

Linuxdakı **/etc/passwd** faylı sistemdəki istifadəçilər üçün bir telefon kitabçası kimidir. O, istifadəçi adı, istifadəçi ID-si, qrup ID-si, ana qovluğu və istifadə etdikləri defolt (ilkin) qabıq (shell) kimi detalları ehtiva edir.

**`cat`** istifadə edərək məzmunu oxuduqdan və onu **`more`** alətinə yönləndirdikdən sonra, yuxarıda qeyd olunan səhifələyici açılır və biz avtomatik olaraq faylın əvvəlindən başlayırıq.

Məzmunun Süzülməsi

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
--More--
```

**[Q]** düyməsi ilə bu səhifələyicidən çıxa bilərik. Çıxışın terminalda qaldığını görəcəyik.

-----

## **`less`**

İndi **`less`** alətinə nəzər salsaq, man səhifəsində onun **`more`** alətindən daha çox xüsusiyyətə malik olduğunu görəcəyik.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ less /etc/passwd
```

Təqdimat **`more`** ilə demək olar ki, eynidir.

Məzmunun Süzülməsi

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
:
```

**[Q]** düyməsi ilə **`less`**-i bağladıqda, gördüyümüz çıxışın, **`more`**-dan fərqli olaraq, terminalda qalmadığını görəcəyik.

-----

## **`head`**

Bəzən bizi yalnız faylın əvvəlindəki və ya sonundakı xüsusi məsələlər maraqlandıracaq. Əgər yalnız faylın **ilk sətirlərini** almaq istəyiriksə, **`head`** alətindən istifadə edə bilərik. Başqa cür göstərilmədiyi təqdirdə, **`head`** susmaya görə verilmiş faylın və ya girişin ilk on sətrini çap edir.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ head /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
```

-----

## **`tail`**

Əgər yalnız bir faylın və ya nəticələrin **son hissələrini** görmək istəyiriksə, **`head`**-in əks tərəfi olan **`tail`** istifadə edə bilərik, bu da **son on sətri** qaytarır.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ tail /etc/passwd

miredo:x:115:65534::/var/run/miredo:/usr/sbin/nologin
usbmux:x:116:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
rtkit:x:117:119:RealtimeKit,,,:/proc:/usr/sbin/nologin
nm-openvpn:x:118:120:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
nm-openconnect:x:119:121:NetworkManager OpenConnect plugin,,,:/var/lib/NetworkManager:/usr/sbin/nologin
pulse:x:120:122:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
beef-xss:x:121:124::/var/lib/beef-xss:/usr/sbin/nologin
lightdm:x:122:125:Light Display Manager:/var/lib/lightdm:/bin/false
do-agent:x:998:998::/home/do-agent:/bin/false
user6:x:1000:1000:,,,:/home/user6:/bin/bash
```

Bu alətlərin təklif etdiyi mövcud seçimləri araşdırmaq və onlarla təcrübə aparmaq çox faydalı olardı.

-----

## **`sort`**

Hansı nəticələr və fayllarla işlədiyinizdən asılı olaraq, onlar nadir hallarda sıralanmış olur. Daha yaxşı bir ümumi görünüş əldə etmək üçün tez-tez istənilən nəticələri **əlifba** və ya **rəqəm ardıcıllığı** ilə sıralamaq lazımdır. Bunun üçün **`sort`** adlı bir alətdən istifadə edə bilərik.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | sort

_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
dovecot:x:114:117:Dovecot mail server,,,:/usr/lib/dovecot:/usr/sbin/nologin
dovenull:x:115:118:Dovecot login user,,,:/nonexistent:/usr/sbin/nologin
ftp:x:113:65534::/srv/ftp:/usr/sbin/nologin
games:x:5:60:games:/usr/games:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
htb-student:x:1002:1002::/home/htb-student:/bin/bash
<SNIP>
```

İndi göründüyü kimi, çıxış artıq **`root`** ilə başlamır, əksinə **əlifba sırası** ilə düzülmüşdür.

-----

## **`grep`**

Bir çox hallarda, təyin etdiyimiz **naxışlara uyğun gələn xüsusi nəticələri axtarmaq** lazım gələcək. Bu məqsədlə ən çox istifadə olunan alətlərdən biri **`grep`**-dir, bu da naxış axtarışı üçün geniş çeşidli güclü xüsusiyyətlər təmin edir. Məsələn, **`grep`** istifadə edərək **defolt qabığı `/bin/bash` olaraq təyin edilmiş istifadəçiləri** axtara bilərik.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep "/bin/bash"

root:x:0:0:root:/root:/bin/bash
mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
htb-student:x:1002:1002::/home/htb-student:/bin/bash
```

Bu, **`grep`**-in əvvəlcədən təyin olunmuş naxışlara əsaslanaraq məlumatları səmərəli şəkildə süzmək üçün necə tətbiq oluna biləcəyinə dair sadə bir nümunədir. Başqa bir imkan isə **xüsusi nəticələri istisna etməkdir**. Bunun üçün **`grep`** ilə **`-v`** seçimindən istifadə olunur. Növbəti nümunədə, standart qabığı **`/bin/false`** və ya **`/usr/bin/nologin`** adları ilə ləğv etmiş bütün istifadəçiləri istisna edirik.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin"

root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
postgres:x:111:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
user6:x:1000:1000:,,,:/home/user6:/bin/bash
```

-----

## **`cut`**

Fərqli simvollara malik xüsusi nəticələr ayırıcılar (delimiters) kimi ayrıla bilər. Burada **xüsusi ayırıcıları necə çıxarmağı** və sətirdəki sözləri **müəyyən edilmiş mövqedə** necə göstərməyi bilmək faydalıdır. Bunun üçün istifadə edilə bilən alətlərdən biri **`cut`**-dır. Buna görə də **`-d`** seçimindən istifadə edirik və ayırıcı (delimiter) olaraq **iki nöqtə (:) simvolunu** təyin edirik və **`-f`** seçimi ilə çıxış etmək istədiyimiz mövqeyi müəyyən edirik.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f1

root
sync
postgres
mrb3n
cry0l1t3
htb-student
```

-----

## **`tr`**

Xətdəki müəyyən simvolları, bizim təyin etdiyimiz simvollarla **əvəz etmək** üçün başqa bir imkan **`tr`** alətidir. Birinci seçim olaraq, hansı simvolu əvəz etmək istədiyimizi, ikinci seçim olaraq isə onu hansı simvolla əvəz etmək istədiyimizi təyin edirik. Növbəti nümunədə, **iki nöqtə (:) simvolunu boşluqla əvəz edirik**.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " "

root x 0 0 root /root /bin/bash
sync x 4 65534 sync /bin /bin/sync
postgres x 111 117 PostgreSQL administrator,,, /var/lib/postgresql /bin/bash
mrb3n x 1000 1000 mrb3n /home/mrb3n /bin/bash
cry0l1t3 x 1001 1001  /home/cry0l1t3 /bin/bash
htb-student x 1002 1002  /home/htb-student /bin/bash
```

-----

## **`column`**

Axtarış nəticələri tez-tez aydın olmayan bir təqdimata malik olduğundan, **`column`** aləti **`-t`** seçimindən istifadə edərək belə nəticələri **cədvəl şəklində** göstərmək üçün çox uyğundur.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | column -t

root          x  0    0     root                 /root                /bin/bash
sync          x  4    65534 sync                 /bin                 /bin/sync
postgres      x  111  117   PostgreSQL           administrator,,,     /var/lib/postgresql    /bin/bash
mrb3n         x  1000 1000  mrb3n                /home/mrb3n          /bin/bash
cry0l1t3      x  1001 1001  /home/cry0l1t3       /bin/bash
htb-student   x  1002 1002  /home/htb-student    /bin/bash
```

-----

## **`awk`**

Gördüyümüz kimi, **"postgres"** istifadəçisi üçün sətirdə bir sütun artıqdır. Belə nəticələri ayırd etməyi mümkün qədər sadə saxlamaq üçün, sətirin **birinci ($1)** və **sonuncu ($NF)** nəticəsini göstərməyə imkan verən **(g)awk** proqramlaşdırması faydalıdır.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}'

root /bin/bash
sync /bin/sync
postgres /bin/bash
mrb3n /bin/bash
cry0l1t3 /bin/bash
htb-student /bin/bash
```

-----

## **`sed`**

Bütün faylda və ya standart girişdə **xüsusi adları dəyişmək** istədiyimiz anlar gələcək. Bunun üçün istifadə edə biləcəyimiz alətlərdən biri **`sed`** adlanan **axın redaktorudur (stream editor)**. Bunun ən çox yayılmış istifadələrindən biri **mətnin əvəz edilməsidir (substitution)**. Burada **`sed`** təyin etdiyimiz naxışları (regular expressions - regex) axtarır və onları yenə də təyin etdiyimiz başqa bir naxışla əvəz edir. Gəlin son nəticələrə sadiq qalaq və **"bin"** sözünü **"HTB"** ilə əvəz etmək istədiyimizi deyək.

Əvvəldəki **"s"** bayrağı **əvəzetmə əmrini (substitute command)** bildirir. Sonra əvəz etmək istədiyimiz naxışı göstəririk. Əyri xətdən (slash, /) sonra, üçüncü mövqedə əvəzetmə kimi istifadə etmək istədiyimiz naxışı daxil edirik. Nəhayət, **"g"** bayrağından istifadə edirik, bu da **bütün uyğun gəlmələri əvəz etmək** (global) üçün nəzərdə tutulub.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'

root /HTB/bash
sync /HTB/sync
postgres /HTB/bash
mrb3n /HTB/bash
cry0l1t3 /HTB/bash
htb-student /HTB/bash
```

-----

## **`wc`**

Son olaraq, neçə uğurlu uyğunluğun olduğunu bilmək tez-tez faydalı olacaq. Sətirləri və ya simvolları əl ilə saymaqdan yayınmaq üçün **`wc`** alətindən istifadə edə bilərik. **`-l`** seçimi ilə yalnız sətirlərin sayıldığını göstəririk.

Məzmunun Süzülməsi

```bash
nijatmansimov@htb[/htb]$ cat /etc/passwd | grep -v "false\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l

6
```

-----

## Təcrübə (Practice)

Unutmayın ki, səyahətiniz boyunca istifadə edə biləcəyiniz və daxil edə biləcəyiniz saysız-hesabsız başqa alətlər mövcuddur. Fərdi üstünlüklərinizə və iş axınlarınıza daha uyğun gələn seçimləri kəşf edə biləcəyiniz üçün bacarıq dəstinizi genişləndirmək üçün **xüsusi tapşırıqlar üçün alternativ alətləri araşdırmaq** şiddətlə tövsiyə olunur. Sərt məhdudiyyətlər yoxdur, buna görə də fərqli imkanları araşdırmaqdan və cəmiyyət daxilində paylaşılan zəngin resurslardan faydalanmaqdan çəkinməyin.

Əgər bu alətlər və onların funksiyaları ilə tanış deyilsinizsə, əvvəlcə bu qədər çox fərqli alətlə məşğul olmaq bir az çətin ola bilər. Özünüzə vaxt ayırın və alətlərlə təcrübə edin. **man səhifələrinə (`man <alət>`) baxın** və ya onların **kömək hissəsini (`<alət> -h` / `<alət> --help`) çağırın**. Bütün alətlərlə tanış olmağın ən yaxşı yolu **təcrübə etməkdir**. Onları mümkün qədər tez-tez istifadə etməyə çalışın və qısa müddətdən sonra bir çox şeyi **intuitiv** şəkildə süzə biləcəksiniz.

Budur süzmə bacarıqlarımızı inkişaf etdirmək və terminal və əmrlərlə daha yaxından tanış olmaq üçün istifadə edə biləcəyimiz bir neçə əlavə məşq. İşləməli olduğumuz fayl hədəfimizdəki **/etc/passwd** faylıdır və yuxarıda göstərilən istənilən əmrdən istifadə edə bilərik. Məqsədimiz yalnız **xüsusi məzmunu süzmək və göstərməkdir**. Faylı oxuyun və məzmununu elə süzün ki, yalnız bunları görək:
