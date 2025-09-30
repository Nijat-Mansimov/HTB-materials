# Paket İdarəetməsi (Package Management)

İstər bir sistem administratoru olaraq işləyərkən, istər evdə öz Linux maşınlarımızı idarə edərkən, istərsə də seçdiyimiz penetrasiya testi distribütorunu qurarkən/təkmilləşdirərkən/saxlayarkən, mövcud Linux paket menecerləri və onlardan paketləri **quraşdırmaq, yeniləmək və ya silmək** üçün istifadə etməyin müxtəlif yollarını yaxşı bilmək çox vacibdir. **Paketlər** proqram təminatının ikilik fayllarını (binaries), konfiqurasiya fayllarını, asılılıqlar (dependencies) haqqında məlumatları ehtiva edən və yeniləmələri və təkmilləşdirmələri izləyən arxivlərdir. Əksər paket idarəetmə sistemlərinin təmin etdiyi xüsusiyyətlər bunlardır:

  * Paketlərin yüklənməsi
  * Asılılıqların həlli (Dependency resolution)
  * Standart ikilik paket formatı
  * Ümumi quraşdırma və konfiqurasiya yerləri
  * Əlavə sistemlə əlaqəli konfiqurasiya və funksionallıq
  * Keyfiyyətə nəzarət

Biz **".deb"**, **".rpm"** və digər kimi müxtəlif fayl növlərini əhatə edən bir çox fərqli paket idarəetmə sistemlərindən istifadə edə bilərik. Paket idarəetməsinin tələbi, quraşdırılacaq proqram təminatının müvafiq paket kimi mövcud olmasıdır. Adətən bu, Linux distribütorları altında mərkəzi şəkildə yaradılır, təklif olunur və idarə olunur. Bu yolla, proqram təminatı birbaşa sistemə inteqrasiya olunur və onun müxtəlif qovluqları sistem boyunca paylanır. Paket idarəetmə proqramı sistem quraşdırılması üçün zəruri dəyişiklikləri paketin özündən alır və sonra paketi uğurla quraşdırmaq üçün bu dəyişiklikləri tətbiq edir. Əgər paket idarəetmə proqramı, paketin düzgün işləməsi üçün hələ quraşdırılmamış əlavə paketlərin tələb olunduğunu aşkar edərsə, bir **asılılıq (dependency)** daxil edilir və ya administratoru xəbərdar edir, ya da məsələn, çatışmayan proqram təminatını bir **havuzdan (repository)** yenidən yükləməyə və əvvəlcədən quraşdırmağa çalışır.

Əgər quraşdırılmış bir proqram təminatı silinibsə, paket idarəetmə sistemi paketin məlumatlarını yenidən götürür, konfiqurasiyasına əsasən onu dəyişdirir və faylları silir. Bunun üçün istifadə edə biləcəyimiz müxtəlif paket idarəetmə proqramları var. Budur bu cür proqramların nümunələrinin siyahısı:

| Əmr | Təsvir |
| :--- | :--- |
| **`dpkg`** | **`dpkg`** Debian paketlərini quraşdırmaq, yaratmaq, silmək və idarə etmək üçün bir alətdir. **`dpkg`** üçün əsas və daha istifadəçi dostu ön ucu **`aptitude`**-dur. |
| **`apt`** | **`apt`** paket idarəetmə sistemi üçün **yüksək səviyyəli əmr sətri interfeysi** təqdim edir. |
| **`aptitude`** | **`aptitude`** **`apt`**-ə alternativdir və paket menecerinə yüksək səviyyəli interfeysdir. |
| **`snap`** | Snap paketlərini quraşdırın, konfiqurasiya edin, yeniləyin və silin. Snaps bulud, serverlər, masaüstü kompüterlər və əşyaların interneti üçün ən son tətbiqlərin və köməkçi proqramların təhlükəsiz paylanmasına imkan verir. |
| **`gem`** | **`gem`** Ruby üçün standart paket meneceri olan RubyGems-in ön ucudur. |
| **`pip`** | **`pip`** Debian arxivində mövcud olmayan Python paketlərini quraşdırmaq üçün tövsiyə olunan bir Python paket quraşdırıcısıdır. O, versiyaya nəzarət depoları (hal-hazırda yalnız Git, Mercurial və Bazaar depoları) ilə işləyə bilər, çıxışı geniş şəkildə qeyd edir və quraşdırmaya başlamazdan əvvəl bütün tələbləri yükləyərək qismən quraşdırmaların qarşısını alır. |
| **`git`** | **`git`** həm yüksək səviyyəli əməliyyatları, həm də daxili hissələrə tam girişi təmin edən qeyri-adi zəngin əmr dəstinə malik, sürətli, miqyaslana bilən, paylanmış versiyaya nəzarət sistemidir. |

Onunla təcrübə aparmaq üçün **virtual maşınımızı (VM) yerli olaraq qurmaq** çox tövsiyə olunur. Gəlin yerli VM-mizdə bir az təcrübə edək və onu bir neçə əlavə paketlə genişləndirək. Əvvəlcə, **`apt`** istifadə edərək **`git`**-i quraşdıraq.

-----

## Təkmil Paket Meneceri (APT)

Debian əsaslı Linux distribütorları **APT** paket menecerindən istifadə edir. Bir paket, birdən çox **".deb"** faylını ehtiva edən bir arxiv faylıdır. **`dpkg`** köməkçi proqramı əlaqəli **".deb"** faylından proqramları quraşdırmaq üçün istifadə olunur. **APT** proqramları yeniləməyi və quraşdırmağı asanlaşdırır, çünki bir çox proqramın **asılılıqları** (dependencies) var. Müstəqil bir **".deb"** faylından bir proqram quraşdırarkən, asılılıq problemləri ilə qarşılaşa bilərik və bir və ya birdən çox əlavə paketi yükləyib quraşdırmalıyıq. **APT** bir proqramı quraşdırmaq üçün lazım olan bütün asılılıqları birlikdə paketləyərək bu prosesi asan və daha səmərəli edir.

Hər bir Linux distribütoru tez-tez yenilənən proqram təminatı **havuzlarından (repositories)** istifadə edir. Bir proqramı yenilədikdə və ya yeni bir proqram quraşdırdıqda, sistem bu havuzlardan istənilən paketi sorğulayır. Havuzlar **sabit (stable)**, **test (testing)** və ya **qeyri-sabit (unstable)** olaraq etiketlənə bilər. Əksər Linux distribütorları ən sabit və ya **"əsas" (main)** havuzdan istifadə edir. Buna **/etc/apt/sources.list** faylının məzmununa baxaraq nəzarət edilə bilər. Parrot OS üçün havuz siyahısı **/etc/apt/sources.list.d/parrot.list** yerindədir.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ cat /etc/apt/sources.list.d/parrot.list
# parrot repository
# this file was automatically generated by parrot-mirror-selector
deb http://htb.deb.parrot.sh/parrot/ rolling main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling main contrib non-free
deb http://htb.deb.parrot.sh/parrot/ rolling-security main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling-security main contrib non-free
```

APT **APT cache** adlanan bir verilənlər bazasından istifadə edir. Bu, sistemimizdə quraşdırılmış paketlər haqqında məlumatı oflayn təmin etmək üçün istifadə olunur. Məsələn, bütün **Impacket** ilə əlaqəli paketləri tapmaq üçün APT keşini axtara bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ apt-cache search impacket
impacket-scripts - Links to useful impacket scripts examples
polenum - Extracts the password policy from a Windows system
python-pcapy - Python interface to the libpcap packet capture library (Python 2)
python3-impacket - Python3 module to easily build and dissect network protocols
python3-pcapy - Python interface to the libpcap packet capture library (Python 3)
```

Daha sonra bir paket haqqında əlavə məlumatlara baxa bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ apt-cache show impacket-scripts
Package: impacket-scripts
Version: 1.4
Architecture: all
Maintainer: Kali Developers <devel@kali.org>
Installed-Size: 13
Depends: python3-impacket (>= 0.9.20), python3-ldap3 (>= 2.5.0), python3-ldapdomaindump
Breaks: python-impacket (<< 0.9.18)
Replaces: python-impacket (<< 0.9.18)
Priority: optional
Section: misc
Filename: pool/main/i/impacket-scripts/impacket-scripts_1.4_all.deb
Size: 2080
<SNIP>
```

Həmçinin bütün **quraşdırılmış paketləri** siyahılaşdıra bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ apt list --installed
Listing... Done
accountsservice/rolling,now 0.6.55-2 amd64 [installed,automatic]
adapta-gtk-theme/rolling,now 3.95.0.11-1 all [installed]
adduser/rolling,now 3.118 all [installed]
adwaita-icon-theme/rolling,now 3.36.1-2 all [installed,automatic]
aircrack-ng/rolling,now 1:1.6-4 amd64 [installed,automatic]
<SNIP>
```

Əgər bəzi paketlər əskikdirsə, onları axtara və aşağıdakı əmrdən istifadə edərək quraşdıra bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install impacket-scripts -y
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed:
  impacket-scripts
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 2,080 B of archives.
After this operation, 13.3 kB of additional disk space will be used.
Get:1 https://euro2-emea-mirror.parrot.sh/mirrors/parrot rolling/main amd64 impacket-scripts all 1.4 [2,080 B]
Fetched 2,080 B in 0s (15.2 kB/s)
Selecting previously unselected package impacket-scripts.
(Reading database ... 378459 files and directories currently installed.)
Preparing to unpack .../impacket-scripts_1.4_all.deb ...
Unpacking impacket-scripts (1.4) ...
Setting up impacket-scripts (1.4) ...
Scanning application launchers
Removing duplicate launchers from Debian
Launchers are updated
```

-----

## Git

İndi **`git`** quraşdırıldığına görə, biz ondan **Github**-dan faydalı alətləri yükləmək üçün istifadə edə bilərik. Belə layihələrdən biri **'Nishang'** adlanır. Layihənin özü ilə daha sonra məşğul olacağıq və işləyəcəyik. Əvvəlcə **layihənin havuzuna (repository)** keçməli və **`git`**-dən istifadə edərək yükləməzdən əvvəl **Github keçidini kopyalamalıyıq**.

<img width="1117" height="358" alt="image" src="https://github.com/user-attachments/assets/29774087-ac1d-4636-bfbb-e56dde157a1c" />

Bununla belə, layihəni, onun skriptlərini və siyahılarını yükləməzdən əvvəl xüsusi bir qovluq yaratmalıyıq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ mkdir ~/nishang/ && git clone https://github.com/samratashok/nishang.git ~/nishang
Cloning into '/opt/nishang/'...
remote: Enumerating objects: 15, done.
remote: Counting objects: 100% (15/15), done.
remote: Compressing objects: 100% (13/13), done.
remote: Total 1691 (delta 4), reused 6 (delta 2), pack-reused 1676
Receiving objects: 100% (1691/1691), 7.84 MiB | 4.86 MiB/s, done.
Resolving deltas: 100% (1055/1055), done.
```

-----

## DPKG

Biz proqramları və alətləri **havuzlardan (repositories) ayrıca yükləyə** də bilərik. Bu nümunədə, Ubuntu 18.04 LTS üçün **`strace`** proqramını yükləyirik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ wget http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb
--2020-05-15 03:27:17--  http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb
Resolving archive.ubuntu.com (archive.ubuntu.com)... 91.189.88.142, 91.189.88.152, 2001:67c:1562::18, ...
Connecting to archive.ubuntu.com (archive.ubuntu.com)|91.189.88.142|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 333388 (326K) [application/x-debian-package]
Saving to: ‘strace_4.21-1ubuntu1_amd64.deb’

strace_4.21-1ubuntu1_amd64.deb       100%[===================================================================>] 325,57K  --.-KB/s    in 0,1s    

2020-05-15 03:27:18 (2,69 MB/s) - ‘strace_4.21-1ubuntu1_amd64.deb’ saved [333388/333388]
```

Bundan əlavə, indi paketi quraşdırmaq üçün həm **`apt`**-dən, həm də **`dpkg`**-dən istifadə edə bilərik. Biz artıq **`apt`** ilə işlədiyimiz üçün, növbəti nümunədə **`dpkg`**-yə müraciət edəcəyik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo dpkg -i strace_4.21-1ubuntu1_amd64.deb
(Reading database ... 154680 files and directories currently installed.)
Preparing to unpack strace_4.21-1ubuntu1_amd64.deb ...
Unpacking strace (4.21-1ubuntu1) over (4.21-1ubuntu1) ...
Setting up strace (4.21-1ubuntu1) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
```

Bununla da aləti artıq quraşdırdıq və düzgün işlədiyini yoxlaya bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ strace -h
usage: strace [-CdffhiqrtttTvVwxxy] [-I n] [-e expr]...
              [-a column] [-o file] [-s strsize] [-P path]...
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]
   or: strace -c[dfw] [-I n] [-e expr]... [-O overhead] [-S sortby]
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]

Output format:
  -a column      alignment COLUMN for printing syscall results (default 40)
  -i             print instruction pointer at time of syscall
```


































