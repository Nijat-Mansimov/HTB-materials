Əlbəttə, mətnin Azərbaycan dilinə tam tərcüməsi aşağıdakı kimidir:

-----

## Qorunan Arxivlərin Sındırılması

Fərdi fayllardan əlavə, biz tez-tez parol ilə qorunan **arxivlərə** və **sıxılmış fayllara** — məsələn, ZIP fayllarına — rast gələcəyik.

Bir inzibati şirkətin əməkdaşı rolunu götürək və bir müştərinin müxtəlif formatlarda, məsələn, Excel, PDF, Word və müvafiq təqdimatda təhlilin xülasəsini tələb etdiyini təsəvvür edək. Bir yanaşma bu faylları ayrı-ayrılıqda göndərmək olardı. Lakin, biz bu ssenarini eyni vaxtda çoxsaylı layihələri idarə edən böyük bir təşkilata tətbiq etsək, bu fayl ötürmə metodu yorucu ola bilər və fərdi faylların səhv yerləşdirilməsinə səbəb ola bilər. Belə hallarda, əməkdaşlar adətən arxiv fayllarına etibar edirlər ki, bu da onlara lazım olan sənədləri strukturlu şəkildə (adətən alt qovluqlardan istifadə etməklə) təşkil etməyə, sonra isə onları tək, konsolidə edilmiş bir fayla sıxmağa imkan verir.

Çox növ arxiv faylı mövcuddur. Daha çox rast gəlinən fayl genişlənmələrindən bəziləri **tar**, **gz**, **rar**, **zip**, **vmdb/vmx**, **cpt**, **truecrypt**, **bitlocker**, **kdbx**, **deb**, **7z** və **gzip** daxildir.

Arxiv fayl növlərinin hərtərəfli siyahısını **FileInfo** saytında tapa bilərsiniz. Onları əl ilə yazmaqdansa, biz həmçinin verilənləri bir əmr xətti vasitəsilə sorğulaya, lazım olduqda filtrləri tətbiq edə və nəticələri bir fayla yaza bilərik. Yazı hazırlanarkən, vebsayt **365** arxiv fayl növünü sadalayır.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
.mint
.zhelp
.b6z
.fzpz
.zst
.apz
.ufs.uzip
.vrpackage
.sfg
.gzip
.xapk
.rar
.pkg.tar.xz
<SNIP>
```

Qeyd edək ki, bütün arxiv növləri yerli parol qorumasını dəstəkləmir və belə hallarda faylları şifrələmək üçün tez-tez əlavə alətlərdən istifadə olunur. Məsələn, TAR faylları adətən **openssl** və ya **gpg** istifadə edilərək şifrələnir.

Arxiv formatları və şifrələmə alətlərinin geniş çeşidini nəzərə alaraq, bu bölmə yalnız xüsusi arxiv növlərini sındırmaq üçün metodların seçiminə fokuslanacaq. Parol qorunan arxivlər üçün, biz adətən fayllardan parol heşlərini çıxarmaq üçün ixtisaslaşdırılmış skriptlərə ehtiyac duyuruq ki, bunlar da oflayn sındırma cəhdlərində istifadə oluna bilsin.

-----

## ZIP Fayllarının Sındırılması

**ZIP** formatı bir çox faylı bir faylda sıxmaq üçün Windows mühitlərində tez-tez geniş istifadə olunur. Şifrələnmiş ZIP faylını sındırma prosesi, artıq gördüyümüzə bənzəyir, yalnız heşləri çıxarmaq üçün fərqli bir skript istifadə etmək fərqi ilə.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ zip2john ZIP.zip > zip.hash
nijatmansimov@htb[/htb]$ cat zip.hash
ZIP.zip/customers.csv:$pkzip2$1*2*2*0*2a*1e*490e7510*0*42*0*2a*490e*409b*ef1e7feb7c1cf701a6ada7132e6a5c6c84c032401536faf7493df0294b0d5afc3464f14ec081cc0e18cb*$/pkzip2$:customers.csv:ZIP.zip::ZIP.zip
```

Heşi çıxardıqdan sonra, onu istənilən parol siyahısı ilə sındırmaq üçün JtR-dən istifadə edə bilərik.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ john --wordlist=rockyou.txt zip.hash
nijatmansimov@htb[/htb]$ john zip.hash --show
ZIP.zip/customers.csv:1234:customers.csv:ZIP.zip::ZIP.zip

1 password hash cracked, 0 left
```

-----

## OpenSSL ilə Şifrələnmiş GZIP Fayllarının Sındırılması

Bir faylın parol qorunub-qorunmadığı hər zaman dərhal aydın olmur, xüsusən də fayl genişlənməsi yerli olaraq parol qorumasını dəstəkləməyən bir formata uyğun gəldikdə. Əvvəlcə müzakirə edildiyi kimi, **openssl** **GZIP** formatında faylları şifrələmək üçün istifadə oluna bilər. Bir faylın faktiki formatını müəyyən etmək üçün, onun məzmunu haqqında ətraflı məlumat verən **file** əmrindən istifadə edə bilərik. Məsələn:

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ file GZIP.gzip 
GZIP.gzip: openssl enc'd data with salted password
```

OpenSSL ilə şifrələnmiş faylları sındırarkən, çoxsaylı yanlış müsbətlər və ya düzgün parolu müəyyən etməkdə tam uğursuzluq daxil olmaqla, müxtəlif çətinliklərlə qarşılaşa bilərik. Bunu azaltmaq üçün, daha etibarlı bir yanaşma, məzmunu birbaşa çıxarmağa cəhd edən, yalnız düzgün parol tapılarsa müvəffəq olan bir **for** dövrü daxilində **openssl** alətini istifadə etməkdir.

Aşağıdakı bir əmr xətti bir neçə GZIP ilə əlaqəli səhv mesajları yarada bilər ki, bunlar etibarlı şəkildə nəzərə alınmaya bilər. Əgər bu nümunədə olduğu kimi düzgün parol siyahısı istifadə edilərsə, biz arxivdən uğurla çıxarılmış başqa bir fayl görəcəyik.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now

gzip: stdin: not in gzip format
tar: Child returned status 1
tar: Error is not recoverable: exiting now
<SNIP>
```

**for** dövrü bitdikdən sonra, mövcud qovluqda yeni çıxarılmış fayl olub-olmadığını yoxlaya bilərik.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ ls
customers.csv  GZIP.gzip  rockyou.txt
```

-----

## BitLocker ilə Şifrələnmiş Disklərin Sındırılması

**BitLocker** Microsoft tərəfindən Windows əməliyyat sistemi üçün hazırlanmış tam disk şifrələmə xüsusiyyətidir. Windows Vista-dan bəri mövcuddur, o, **AES** şifrələmə alqoritmini ya 128-bit, ya da 256-bit açar uzunluqları ilə istifadə edir. Əgər BitLocker üçün istifadə olunan parol və ya PİN unudulubsa, şifrəni açma prosesi hələ də bərpa açarı — quraşdırma prosesi zamanı yaradılan 48 rəqəmli sətir — istifadə edilərək həyata keçirilə bilər.

Korporativ mühitlərdə, icazəsiz girişi önləmək üçün şirkət tərəfindən verilmiş cihazlarda şəxsi məlumatları, sənədləri və ya qeydləri saxlamaq üçün bəzən virtual disklərdən istifadə olunur. BitLocker ilə şifrələnmiş diski sındırmaq üçün, **bitlocker2john** adlı bir skriptdən dörd fərqli heşi çıxarmaq üçün istifadə edə bilərik: ilk ikisi BitLocker paroluna, sonrakı ikisi isə bərpa açarına uyğundur. Bərpa açarı çox uzun və təsadüfi yaradıldığı üçün, ümumiyyətlə onu təxmin etmək praktik deyil — əgər qismən məlumat yoxdursa. Buna görə də, biz ilk heş ($bitlocker$0$...) istifadə edərək parolun sındırılmasına fokuslanacağıq.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hash
nijatmansimov@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash
nijatmansimov@htb[/htb]$ cat backup.hash
$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f
```

Heş yaradıldıqdan sonra, onu sındırmaq üçün **JtR** və ya **hashcat** istifadə edilə bilər. Bu nümunə üçün, biz **hashcat** ilə prosedura baxacağıq. **$bitlocker$0$...** heşi ilə əlaqəli hashcat rejimi **-m 22100**-dür. Biz heşi təmin edir, söz siyahısını göstərir və heş rejimini müəyyən edirik. Bu şifrələmə güclü AES şifrələməsindən istifadə etdiyi üçün, sındırma prosesi avadanlıq performansından asılı olaraq xeyli vaxt apara bilər.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ hashcat -a 0 -m 22100 '$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f' /usr/share/wordlists/rockyou.txt
<SNIP>
$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f:1234qwer                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 22100 (BitLocker)
Hash.Target......: $bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$10...8ec54f
Time.Started.....: Sat Apr 19 17:49:25 2025 (1 min, 56 secs)
Time.Estimated...: Sat Apr 19 17:51:21 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       25 H/s (9.28ms) @ Accel:64 Loops:4096 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 2880/14344385 (0.02%)
Rejected.........: 0/2880 (0.00%)
Restore.Point....: 2816/14344385 (0.02%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:1044480-1048576
Candidate.Engine.: Device Generator
Candidates.#1....: pirate -> soccer9
Hardware.Mon.#1..: Util:100%
Started: Sat Apr 19 17:49:05 2025
Stopped: Sat Apr 19 17:51:22 2025
```

Parolu uğurla sındırdıqdan sonra, şifrələnmiş diskə daxil ola bilərik.

-----

## Windows-da BitLocker ilə Şifrələnmiş Disklərin Qoşulması

Windows-da BitLocker ilə şifrələnmiş virtual diski qoşmaq üçün ən asan üsul **.vhd** faylının üzərinə iki dəfə klikləməkdir. O, şifrələndiyi üçün Windows əvvəlcə bir səhv göstərəcək. Qoşulduqdan sonra, sadəcə BitLocker həcminin üzərinə iki dəfə klikləyin ki, parol soruşulsun.

-----

## Linux (və ya macOS)-da BitLocker ilə Şifrələnmiş Disklərin Qoşulması

<img width="1304" height="562" alt="image" src="https://github.com/user-attachments/assets/79a40389-6eed-43a9-980e-ba5a68836c73" />

BitLocker ilə şifrələnmiş diskləri Linux (və ya macOS)-da da qoşmaq mümkündür. Bunu etmək üçün, **dislocker** adlı bir alətdən istifadə edə bilərik. Əvvəlcə, paketi **apt** istifadə edərək quraşdırmalıyıq:

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ sudo apt-get install dislocker
```

Sonra, VHD-ni qoşmaq üçün istifadə edəcəyimiz iki qovluq yaradırıq.

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ sudo mkdir -p /media/bitlocker
nijatmansimov@htb[/htb]$ sudo mkdir -p /media/bitlockermount
```

Daha sonra, VHD-ni **dövrə cihazı (loop device)** kimi konfiqurasiya etmək üçün **losetup** istifadə edir, **dislocker** istifadə edərək diski deşifrələyir və nəhayət deşifrələnmiş həcmi qoşuruq:

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ sudo losetup -f -P Backup.vhd
nijatmansimov@htb[/htb]$ sudo dislocker /dev/loop0p2 -u1234qwer -- /media/bitlocker
nijatmansimov@htb[/htb]$ sudo mount -o loop /media/bitlocker/dislocker-file /media/bitlockermount
```

Əgər hər şey düzgün edilibsə, indi faylları nəzərdən keçirə bilərik:

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ cd /media/bitlockermount/
nijatmansimov@htb[/htb]$ ls -la
```

Qoşulmuş diskdəki faylları təhlil etdikdən sonra, aşağıdakı əmrlərdən istifadə edərək onu ayırd edə bilərik (unmount):

```bash
Cracking Protected Archives
nijatmansimov@htb[/htb]$ sudo umount /media/bitlockermount
nijatmansimov@htb[/htb]$ sudo umount /media/bitlocker
```
