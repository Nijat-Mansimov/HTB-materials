# Fayl Sistemi İdarəetməsi (File System Management)

Linux sistemlərində fayl sistemlərinin idarə edilməsi, məlumatların diskdə və ya digər yaddaş cihazlarında təşkili, saxlanması və saxlanması daxil olan vacib bir vəzifədir. Linux, **ext2**, **ext3**, **ext4**, **XFS**, **Btrfs** və **NTFS** daxil olmaqla bir çox fərqli fayl sistemini dəstəkləyən çoxfunksiyalı bir əməliyyat sistemidir. Bu fayl sistemlərinin hər biri unikal xüsusiyyətlərə malikdir və xüsusi istifadə hallarına uyğundur. Ən yaxşı fayl sistemi seçimi, proqramın və ya istifadəçinin xüsusi tələblərindən asılıdır, məsələn:

  * **`ext2`**: Jurnallaşdırma qabiliyyətləri olmayan daha köhnə bir fayl sistemidir, bu da onu müasir sistemlər üçün daha az uyğun edir, lakin müəyyən aşağı yüklü ssenarilərdə (məsələn, USB disklər) hələ də faydalıdır.
  * **`ext3`** və **`ext4`**: Jurnallaşdırma (çökmələrdən bərpa etməyə kömək edir) ilə daha inkişaf etmişdir və **`ext4`** əksər müasir Linux sistemləri üçün defolt seçimdir, çünki performans, etibarlılıq və böyük fayl dəstəyi balansını təklif edir.
  * **`Btrfs`**: Anlıq şəkil çəkmə (**snapshotting**) və daxili məlumat bütövlüyü yoxlamaları kimi qabaqcıl xüsusiyyətləri ilə tanınır, bu da onu mürəkkəb yaddaş quraşdırmaları üçün ideal edir.
  * **`XFS`**: Böyük fayllarla işləməkdə üstündür və yüksək performansa malikdir. Yüksək I/O tələbləri olan mühitlər üçün ən uyğundur.
  * **`NTFS`**: Əvvəlcə Windows üçün hazırlanmışdır, həm Linux, həm də Windows sistemlərində işləməli olan **dual-boot** sistemləri və ya xarici disklərlə işləyərkən uyğunluq üçün faydalıdır.

Fayl sistemi seçərkən, tətbiqin və ya istifadəçinin ehtiyaclarını təhlil etmək vacibdir; **performans**, **məlumat bütövlüyü**, **uyğunluq** və **yaddaş tələbləri** kimi amillər qərarı təsir edəcək.

Linux-un fayl sistemi arxitekturası **Unix modelinə** əsaslanır, iyerarxik bir quruluşda təşkil edilmişdir. Bu quruluş bir neçə komponentdən ibarətdir, ən kritik olanları **`inode`**-lardır. **`Inodes`** hər bir fayl və qovluq haqqında **metadata** (icazələr, sahiblik, ölçü və zaman möhürləri daxil olmaqla) saxlayan məlumat strukturlarıdır. Inodes faylın faktiki məlumatını və ya adını saxlamır, lakin onlar faylın məlumatlarının diskdə saxlandığı bloklara **göstəriciləri** ehtiva edir.

**`Inode` cədvəli** bu inode-ların bir kolleksiyasıdır, mahiyyətcə Linux kernelinin sistemdəki hər bir faylı və qovluğu izləmək üçün istifadə etdiyi bir **verilənlər bazası** kimi çıxış edir. Bu quruluş əməliyyat sisteminə fayllara səmərəli daxil olmağa və onları idarə etməyə imkan verir. Inode-ları anlamaq və idarə etmək, Linux-da fayl sistemi idarəetməsinin kritik bir aspektidir, xüsusilə disk faktiki yaddaş tutumu bitməmişdən əvvəl **inode yeri bitən** ssenarilərdə.

Bir analogiya istifadə edək: Linux fayl sistemini bir **kitabxana** kimi düşünün. **`Inodes`** kitabxananın kataloq sistemindəki **kart indeksləri** (inode cədvəli) kimidir. Hər bir kart kitab (fayl) haqqında ətraflı məlumat, onun adı, müəllifi, yeri və digər detalları ehtiva edir, lakin faktiki kitabı yox. **`Inode` cədvəli** kitabxanaya (əməliyyat sistemi) kitabları (faylları) sürətlə tapmağa və idarə etməyə kömək edən bütün **kataloqdur**.

Linux-da fayllar bir neçə əsas növdən birində saxlanıla bilər:

  * **Adi Fayllar (Regular files)**
  * **Qovluqlar (Directories)**
  * **Simvolik Keçidlər (Symbolic links)**

### Adi Fayllar

**Adi fayllar** ən çox yayılmış növdür və adətən mətn məlumatlarından (məsələn, ASCII) və/və ya ikili məlumatlardan (məsələn, şəkillər, səs və ya icra edilə bilən fayllar) ibarətdir. Onlar yalnız kök qovluğunda deyil, bütün fayl sistemi boyu müxtəlif qovluqlarda yerləşir. **Kök qovluğu** (`/`) sadəcə iyerarxik qovluq ağacının zirvəsidir və fayllar bu quruluş daxilində istənilən qovluqda mövcud ola bilər.

### Qovluqlar

**Qovluqlar** digər fayllar (həm adi fayllar, həm də digər qovluqlar) üçün **konteynerlər** kimi çıxış edən xüsusi fayl növləridir. Bir fayl bir qovluqda saxlanıldıqda, həmin qovluq faylın **valideyn qovluğu** adlanır. Qovluqlar, faylların kolleksiyalarını idarə etmək üçün səmərəli bir yol təmin edərək, Linux fayl sistemində faylları təşkil etməyə kömək edir.

### Simvolik Keçidlər

Adi fayllar və qovluqlardan əlavə, Linux həm də digər fayllara və ya qovluqlara **qısa yollar** və ya **istinadlar** kimi çıxış edən **simvolik keçidləri** (`symlinks`) dəstəkləyir. Simvolik keçidlər faylın özünü təkrarlamadan fayl sisteminin müxtəlif yerlərində yerləşən fayllara sürətli girişi təmin edir. Symlinks, müxtəlif yerlərdəki vacib fayllara işarə edərək girişi sadələşdirmək və ya mürəkkəb qovluq strukturlarını təşkil etmək üçün istifadə edilə bilər.

Hər bir istifadəçi kateqoriyası fərqli **icazə səviyyələrinə** malik ola bilər. Məsələn, faylın sahibi onu **oxumaq**, **yazmaq** və **icra etmək** icazəsinə malik ola bilər, digərləri isə yalnız oxumaq icazəsinə malik ola bilər. Bu icazələr hər bir kateqoriya üçün müstəqildir, yəni bir istifadəçinin icazələrindəki dəyişikliklər digərlərinə təsir etməyə bilər.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ls -i
total 0
10678872 -rw-r--r--  1 cry0l1t3  htb  234123 Feb 14 19:30 myscript.py
10678869 -rw-r--r--  1 cry0l1t3  htb   43230 Feb 14 11:52 notes.txt
```

-----

## Disklər və Drayvlar (Disks & Drives)

Linux-da disk idarəetməsi, sərt disklər, bərk-hallı disklər və çıxarıla bilən yaddaş cihazları daxil olmaqla fiziki yaddaş cihazlarının idarə edilməsini əhatə edir. Linux-da disk idarəetməsi üçün əsas alət, bir drayvda **bölmələr** (**partitions**) yaratmağa, silməyə və idarə etməyə imkan verən **`fdisk`**-dir. O, həmçinin hər bir bölmənin ölçüsü və növü daxil olmaqla, bölmə cədvəli haqqında məlumatları göstərə bilər. Linux-da bir drayvın bölmələrə ayrılması, fiziki yaddaş sahəsinin ayrı, məntiqi hissələrə bölünməsini əhatə edir. Hər bir bölmə daha sonra **ext4**, **NTFS** və ya **FAT32** kimi xüsusi bir fayl sistemi ilə formatlana bilər və ayrı bir fayl sistemi kimi **qoşula bilər** (**mounted**). Linux-da ən çox yayılmış bölmə alətləri həm də **`fdisk`**, **`gpart`** və **`GParted`**-dir.

### Fdisk

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo fdisk -l
Disk /dev/vda: 160 GiB, 171798691840 bytes, 335544320 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x5223435f

Device     Boot     Start       End   Sectors  Size Id Type
/dev/vda1  * 2048 158974027 158971980 75.8G 83 Linux
/dev/vda2       158974028 167766794   8792767  4.2G 82 Linux swap / Solaris

Disk /dev/vdb: 452 KiB, 462848 bytes, 904 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

-----

## Qoşulma (Mounting)

Hər bir məntiqi bölmə və ya yaddaş drayvı fayl sistemində müəyyən bir qovluğa təyin edilməlidir. Bu proses **qoşulma** (**mounting**) adlanır. Qoşulma, bir drayvı və ya bölməni bir qovluğa **əlaqələndirməyi** əhatə edir, bu da onun məzmununu ümumi fayl sistemi iyerarxiyası daxilində əlçatan edir. Bir drayv bir qovluğa qoşulduqdan sonra (həmçinin **qoşulma nöqtəsi** də adlanır), sistemdəki hər hansı digər qovluq kimi daxil oluna və istifadə edilə bilər.

**`mount`** əmri adətən fayl sistemlərini əl ilə qoşmaq üçün istifadə olunur. Lakin, müəyyən fayl sistemlərinin və ya bölmələrin sistem işə düşəndə avtomatik qoşulmasını istəyirsinizsə, onları **`/etc/fstab`** faylında müəyyən edə bilərsiniz. Bu fayl fayl sistemlərini və onların əlaqəli qoşulma nöqtələrini, oxuma/yazma icazələri və fayl sistemi növləri kimi seçimlərlə birlikdə sadalayır, xüsusi drayvların və ya bölmələrin əl müdaxiləsi olmadan işə salınarkən əlçatan olmasını təmin edir.

### İşə Salınarkən Qoşulmuş Fayl Sistemləri

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ cat /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a device; this may
# be used with UUID= as a more robust way to name devices that works even if
# disks are added and removed. See fstab(5).
#
# <file system>                      <mount point>  <type>  <options>  <dump>  <pass>
UUID=3d6a020d-...SNIP...-9e085e9c927a /              btrfs   subvol=@,defaults,noatime,nodiratime,nodatacow,space_cache,autodefrag 0 1
UUID=3d6a020d-...SNIP...-9e085e9c927a /home          btrfs   subvol=@home,defaults,noatime,nodiratime,nodatacow,space_cache,autodefrag 0 2
UUID=21f7eb94-...SNIP...-d4f58f94e141 swap           swap    defaults,noatime 0 0
```

Hazırda qoşulmuş fayl sistemlərinə baxmaq üçün **`mount`** əmrini heç bir arqument olmadan istifadə edə bilərik. Çıxış, qoşulmuş bütün fayl sistemlərinin siyahısını, o cümlədən cihaz adı, fayl sistemi növü, qoşulma nöqtəsi və seçimləri göstərəcək.

### Qoşulmuş Drayvları Siyahılamaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ mount
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=4035812k,nr_inodes=1008953,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=814580k,mode=755,inode64)
/dev/vda1 on / type btrfs (rw,noatime,nodiratime,nodatasum,nodatacow,space_cache,autodefrag,subvolid=257,subvol=/@)
```

Bir fayl sistemini qoşmaq üçün **`mount`** əmrini, ardınca cihaz adını və qoşulma nöqtəsini istifadə edə bilərik. Məsələn, cihaz adı **`/dev/sdb1`** olan bir USB drayvını **`/mnt/usb`** qovluğuna qoşmaq üçün aşağıdakı əmri istifadə edərdik:

### Bir USB Drayvı Qoşmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo mount /dev/sdb1 /mnt/usb
nijatmansimov@htb[/htb]$ cd /mnt/usb && ls -l
total 32
drwxr-xr-x 1 root root   18 Oct 14  2021 'Account Takeover'
drwxr-xr-x 1 root root   18 Oct 14  2021 'API Key Leaks'
drwxr-xr-x 1 root root   18 Oct 14  2021 'AWS Amazon Bucket S3'
drwxr-xr-x 1 root root   34 Oct 14  2021 'Command Injection'
drwxr-xr-x 1 root root   18 Oct 14  2021 'CORS Misconfiguration'
drwxr-xr-x 1 root root   52 Oct 14  2021 'CRLF Injection'
drwxr-xr-x 1 root root   30 Oct 14  2021 'CSRF Injection'
drwxr-xr-x 1 root root   18 Oct 14  2021 'CSV Injection'
drwxr-xr-x 1 root root 1166 Oct 14  2021 'CVE Exploits'
...SNIP...
```

Linux-da bir fayl sistemini **ayırmaq** (**unmount**) üçün, ayırmaq istədiyimiz fayl sisteminin qoşulma nöqtəsi ilə birlikdə **`umount`** əmrini istifadə edə bilərik. Qoşulma nöqtəsi, fayl sisteminin qoşulduğu və bizə əlçatan olduğu fayl sistemindəki yerdir. Məsələn, əvvəllər **`/mnt/usb`** qovluğuna qoşulmuş USB drayvını ayırmaq üçün aşağıdakı əmri istifadə edərdik:

### Ayırmaq (Unmount)

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo umount /mnt/usb
```

Qeyd etmək vacibdir ki, bir fayl sistemini ayırmaq üçün **kifayət qədər icazələrə** malik olmalıyıq. Həmçinin, işləyən bir proses tərəfindən istifadə olunan bir fayl sistemini ayıra bilmərik. Fayl sistemini istifadə edən işləyən proseslərin olmadığından əmin olmaq üçün, fayl sistemindəki açıq faylları siyahılamaq üçün **`lsof`** əmrindən istifadə edə bilərik.

Müntəzəm İfadələr

```bash
cry0l1t3@htb:~$ lsof | grep cry0l1t3
vncserver 6006        cry0l1t3  mem       REG      0,24       402274 /usr/bin/perl (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24      1554101 /usr/lib/locale/aa_DJ.utf8/LC_COLLATE (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402326 /usr/lib/x86_64-linux-gnu/perl-base/auto/POSIX/POSIX.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402059 /usr/lib/x86_64-linux-gnu/perl/5.32.1/auto/Time/HiRes/HiRes.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24      1444250 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402327 /usr/lib/x86_64-linux-gnu/perl-base/auto/Socket/Socket.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402324 /usr/lib/x86_64-linux-gnu/perl-base/auto/IO/IO.so (path dev=0,26)
...SNIP...
```

Əgər fayl sistemini istifadə edən hər hansı bir proses tapsaq, fayl sistemini ayıra bilməzdən əvvəl onları dayandırmalıyıq. Əlavə olaraq, **`/etc/fstab`** faylına bir giriş əlavə etməklə sistem söndürüldükdə bir fayl sistemini avtomatik ayıra bilərik. `/etc/fstab` faylı sistemdə qoşulmuş bütün fayl sistemləri haqqında məlumatları, o cümlədən işə salınma zamanı avtomatik qoşulma seçimlərini və digər qoşulma seçimlərini ehtiva edir. Bir fayl sistemini avtomatik ayırmaq üçün, həmin fayl sistemi üçün `/etc/fstab` faylındakı girişə **`noauto`** seçimini əlavə etməliyik. Bu, məsələn, aşağıdakı kimi görünəcək:

### Fstab Faylı

```txt
/dev/sda1 / ext4 defaults 0 0
/dev/sda2 /home ext4 defaults 0 0
/dev/sdb1 /mnt/usb ext4 rw,noauto,user 0 0
192.168.1.100:/nfs /mnt/nfs nfs defaults 0 0
```

-----

## Svap (SWAP)

**Svap sahəsi** Linux-da yaddaş idarəetməsinin vacib bir hissəsidir və xüsusilə mövcud fiziki yaddaşın (**RAM**) tam istifadə edildiyi zaman hamar sistem performansını təmin etməkdə kritik rol oynayır. Sistem fiziki yaddaşı tükəndikdə, kernel aktiv olmayan yaddaş səhifələrini (dərhal istifadə edilməyən məlumatları) **svap sahəsinə** köçürür və aktiv proseslər üçün RAM-ı boşaldır. Bu proses **svap etmə** (**swapping**) adlanır.

### Svap Sahəsinin Yaradılması

Svap sahəsi ya əməliyyat sisteminin quraşdırılması zamanı qurula bilər, ya da daha sonra **`mkswap`** və **`swapon`** əmrlərindən istifadə edilərək əlavə edilə bilər.

  * **`mkswap`**: Linux svap sahəsi yaradaraq bir cihazı və ya faylı svap sahəsi kimi istifadə etmək üçün hazırlamaq üçün istifadə olunur.
  * **`swapon`**: Svap sahəsini aktivləşdirir, sistemin onu istifadə etməsinə imkan verir.

### Svap Sahəsinin Ölçüləndirilməsi və İdarə Edilməsi

**Svap sahəsinin ölçüsü** sabit deyil və sisteminizin fiziki yaddaşından və nəzərdə tutulan istifadəsindən asılıdır. Məsələn, daha az RAM-ı olan və ya yaddaş tələb edən proqramları işlədən bir sistemə daha çox svap sahəsi lazım ola bilər. Lakin, çoxlu RAM-a malik müasir sistemlər xüsusi istifadə hallarına əsasən daha az və ya heç svap sahəsinə ehtiyac duymaya bilər.

Svap sahəsini qurarkən, onu fayl sisteminin qalan hissəsindən ayrı, **xüsusi bir bölməyə** və ya **fayla** ayırmaq vacibdir. Bu, **fragmentasiyanın** qarşısını alır və lazım olduqda svap sahəsinin səmərəli istifadəsini təmin edir. Əlavə olaraq, həssas məlumatlar müvəqqəti olaraq svap sahəsində saxlanıla biləcəyi üçün, potensial məlumat ifşasından qorunmaq üçün svap sahəsini **şifrələmək** tövsiyə olunur.

### Qış Yuxusu (Hibernation) üçün Svap Sahəsi

Fiziki yaddaşı genişləndirməkdən başqa, svap sahəsi həm də **qış yuxusu** (**hibernation**) üçün istifadə olunur. Qış yuxusu, sistemin vəziyyətini (açıq proqramlar və proseslər daxil olmaqla) svap sahəsinə saxlayan və sistemi söndürən bir güc qənaət xüsusiyyətidir. Sistem yenidən işə salındıqda, əvvəlki vəziyyətini svap sahəsindən bərpa edir və qaldığı yerdən davam edir.
