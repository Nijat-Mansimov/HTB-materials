## Şifrələnmiş Faylların Sındırılması

Fayl şifrələnməsindən istifadə həm **şəxsi**, həm də **peşəkar** kontekstlərdə tez-tez laqeyd qalır. Hətta bu gün də iş müraciətləri, hesab bildirişləri və ya müqavilələr olan e-poçtlar tez-tez şifrələnmədən göndərilir - bəzən bu, hüquqi qaydaların pozulması deməkdir. Məsələn, Avropa İttifaqı daxilində **Ümumi Məlumatların Qorunması Qaydası (GDPR)** fərdi məlumatların həm ötürülmə zamanı, həm də saxlanıldığı yerdə şifrələnməsini tələb edir. Buna baxmayaraq, **məxfi** mövzuları müzakirə etmək və ya **həssas** məlumatları e-poçt vasitəsilə ötürmək hələ də standart təcrübə olaraq qalır ki, bu da bu rabitə kanallarından istifadə etmək niyyətində olan hücumçular tərəfindən ələ keçirilə bilər.

Daha çox şirkət təlim proqramları və təhlükəsizlik maarifləndirmə seminarları vasitəsilə İT təhlükəsizlik infrastrukturunu gücləndirdiyinə görə, əməkdaşların həssas faylları şifrələməsi getdikcə adi hala çevrilir. Bununla belə, şifrələnmiş fayllar hələ də düzgün söz siyahıları və alətlər kombinasiyası ilə sındırıla və onlara daxil oluna bilər. Bir çox hallarda, fərdi faylları və ya qovluqları təhlükəsiz şəkildə saxlamaq üçün **simmetrik şifrələmə** alqoritmlərindən, məsələn, **AES-256**-dan istifadə olunur. Bu üsulda, həm şifrələmə, həm də deşifrələmə üçün eyni açardan istifadə edilir. Faylların ötürülməsi üçün adətən **asimmerik şifrələmə** tətbiq olunur ki, bu da iki fərqli açardan istifadə edir: göndərən faylı alıcının **açıq açarı** ilə şifrələyir, alıcı isə müvafiq **xüsusi açardan** istifadə edərək onu deşifrələyir.

İndiyə qədər biz xüsusilə parol heşlərinin sındırılmasına diqqət yetirmişik. Növbəti iki bölmədə diqqətimizi parol qorunan fayl və arxivlərə hücum etməklə bağlı texnikalara yönəldəcəyik.

-----

## Şifrələnmiş Faylların Axtarışı

Bir çox fərqli genişlənmələr şifrələnmiş fayllara uyğundur—faydalı istinad siyahısını **FileInfo** saytında tapa bilərsiniz. Nümunə olaraq, Linux sistemində yayılmış şifrələnmiş faylları tapmaq üçün istifadə edə biləcəyimiz bu əmri nəzərdən keçirək:

```bash
$ for ext in $(echo ".xls .xls* .xltx .od* .doc .doc* .pdf .pot .pot* .pp*");do echo -e "\nFile extension: " $ext; find / -name *$ext 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
File extension:  .xls

File extension:  .xls*

File extension:  .xltx

File extension:  .od*
/home/cry0l1t3/Docs/document-temp.odt
/home/cry0l1t3/Docs/product-improvements.odp
/home/cry0l1t3/Docs/mgmt-spreadsheet.ods
...SNIP...
```

Əgər sistemdə tanımadığımız fayl genişlənmələrinə rast gəlsək, onların arxasında duran texnologiyanı araşdırmaq üçün axtarış sistemlərindən istifadə edə bilərik. Axı, yüzlərlə fərqli fayl genişlənməsi var və heç kimdən onların hamısını əzbər bilməsi gözlənilmir.

-----

## SSH Açarlarının Axtarışı

Bəzi faylların, məsələn, SSH açarlarının standart fayl genişlənməsi yoxdur. Belə hallarda, faylları standart məzmununa, məsələn, başlıq və altbilgi dəyərlərinə görə müəyyən etmək mümkün ola bilər. Məsələn, SSH xüsusi açarları həmişə `-----BEGIN [...SNIP...] PRIVATE KEY-----` ilə başlayır. Post-istismar (post-exploitation) zamanı fayl sistemini rekursiv olaraq axtarmaq üçün `grep` kimi alətlərdən istifadə edə bilərik.

```bash
$ grep -rnE '^\-{5}BEGIN [A-Z0-9]+ PRIVATE KEY\-{5}$' /* 2>/dev/null
/home/jsmith/.ssh/id_ed25519:1:-----BEGIN OPENSSH PRIVATE KEY-----
/home/jsmith/.ssh/SSH.private:1:-----BEGIN RSA PRIVATE KEY-----
/home/jsmith/Documents/id_rsa:1:-----BEGIN OPENSSH PRIVATE KEY-----
<SNIP>
```

Bəzi SSH açarları bir parol ifadəsi (passphrase) ilə şifrələnir. Köhnə PEM formatları ilə, istifadə olunan şifrələmə metodunu ehtiva edən başlıq əsasında SSH açarının şifrələnib-şifrələnmədiyini bilmək mümkün idi. Lakin müasir SSH açarları şifrələnsə də, şifrələnməsə də eyni görünür.

```bash
$ cat /home/jsmith/.ssh/SSH.private
-----BEGIN RSA PRIVATE KEY-----
Proc-Type: 4,ENCRYPTED
DEK-Info: AES-128-CBC,2109D25CC91F8DBFCEB0F7589066B2CC

8Uboy0afrTahejVGmB7kgvxkqJLOczb1I0/hEzPU1leCqhCKBlxYldM2s65jhflD
4/OH4ENhU7qpJ62KlrnZhFX8UwYBmebNDvG12oE7i21hB/9UqZmmHktjD3+OYTsD
<SNIP>
```

SSH açarının şifrələnib-şifrələnmədiyini anlamağın bir yolu, açarı `ssh-keygen` ilə oxumağa cəhd etməkdir.

```bash
$ ssh-keygen -yf ~/.ssh/id_ed25519 
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIpNefJd834VkD5iq+22Zh59Gzmmtzo6rAffCx2UtaS6
```

Aşağıda göstərildiyi kimi, parol qorunan bir SSH açarını oxumağa cəhd etmək, istifadəçidən parol ifadəsi soruşacaq:

```bash
$ ssh-keygen -yf ~/.ssh/id_rsa
Enter passphrase for "/home/jsmith/.ssh/id_rsa": 
```

-----

## Şifrələnmiş SSH Açarlarının Sındırılması

Əvvəlki bölmədə qeyd edildiyi kimi, JtR-də fayllardan heşləri çıxarmaq üçün bir çox fərqli skript var - hansıları ki, daha sonra sındırmağa cəhd edə bilərik. Bu skriptləri sistemimizdə aşağıdakı əmrdən istifadə edərək tapa bilərik:

```bash
$ locate *2john*
/usr/bin/bitlocker2john
/usr/bin/dmg2john
/usr/bin/gpg2john
/usr/bin/hccap2john
/usr/bin/keepass2john
/usr/bin/putty2john
/usr/bin/racf2john
/usr/bin/rar2john
/usr/bin/uaf2john
/usr/bin/vncpcap2john
/usr/bin/wlanhcx2john
/usr/bin/wpapcap2john
/usr/bin/zip2john
/usr/share/john/1password2john.py
/usr/share/john/7z2john.pl
/usr/share/john/DPAPImk2john.py
/usr/share/john/adxcsouf2john.py
/usr/share/john/aem2john.py
/usr/share/john/aix2john.pl
/usr/share/john/aix2john.py
/usr/share/john/andotp2john.py
/usr/share/john/androidbackup2john.py
<SNIP>
```

Məsələn, şifrələnmiş bir SSH açarı üçün müvafiq heşi əldə etmək üçün **ssh2john.py** Python skriptindən istifadə edə, sonra isə onu sındırmağa cəhd etmək üçün JtR-dən istifadə edə bilərik.

```bash
$ ssh2john.py SSH.private > ssh.hash
$ john --wordlist=rockyou.txt ssh.hash
Using default input encoding: UTF-8
Loaded 1 password hash (SSH [RSA/DSA/EC/OPENSSH (SSH private keys) 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 2 OpenMP threads
Note: This format may emit false positives, so it will keep trying even after
finding a possible candidate.
Press 'q' or Ctrl-C to abort, almost any other key for status
1234         (SSH.private)
1g 0:00:00:00 DONE (2022-02-08 03:03) 16.66g/s 1747Kp/s 1747Kc/s 1747KC/s Knightsing..Babying
Session completed
```

Daha sonra nəticələnən heşə baxa bilərik:

```bash
$ john ssh.hash --show
SSH.private:1234

1 password hash cracked, 0 left
```

-----

## Parol Qorunan Sənədlərin Sındırılması

Karyeramız boyunca, girişi səlahiyyətli şəxslərlə məhdudlaşdırmaq üçün parol qorunan çox müxtəlif sənədlərlə rastlaşacağıq. Bu gün, əksər hesabatlar, sənədləşdirmələr və məlumat vərəqləri adətən Microsoft Office sənədləri və ya PDF-lər kimi paylanır. John the Ripper (JtR) bütün yayılmış Office sənəd formatlarından parol heşlərini çıxarmaq üçün istifadə edilə bilən **office2john.py** adlı bir Python skriptini ehtiva edir. Bu heşlər daha sonra JtR və ya Hashcat-ə oflayn sındırma üçün verilə bilər. Sındırma proseduru digər heş növləri ilə uyğun qalır.

```bash
$ office2john.py Protected.docx > protected-docx.hash
$ john --wordlist=rockyou.txt protected-docx.hash
$ john protected-docx.hash --show
Protected.docx:1234

1 password hash cracked, 0 left
```

PDF fayllarının sındırılması prosesi olduqca oxşardır, çünki biz sadəcə **office2john.py**-ni **pdf2john.py** ilə əvəz edirik.

```bash
$ pdf2john.py PDF.pdf > pdf.hash
$ john --wordlist=rockyou.txt pdf.hash
$ john pdf.hash --show
PDF.pdf:1234

1 password hash cracked, 0 left
```

Bu prosesdə əsas çətinliklərdən biri, parol qorunan faylları və giriş nöqtələrini uğurla sındırmaq üçün ilkin şərt olan parol siyahılarının yaradılması və dəyişdirilməsidir. Bir çox hallarda, standart və ya ictimaiyyətə məlum olan parol siyahısından istifadə etmək artıq kifayət deyil, çünki bu cür siyahılar tez-tez daxili təhlükəsizlik mexanizmləri tərəfindən tanınır və bloklanır. Bu faylları sındırmaq daha çətin ola bilər - və ya ağlabatan bir müddət ərzində ümumiyyətlə sındırılmaz - çünki istifadəçilərdən getdikcə daha uzun, təsadüfi yaradılmış parollar və ya mürəkkəb parol ifadələri seçmələri tələb olunur. Buna baxmayaraq, parol qorunan sənədləri sındırmağa cəhd etmək tez-tez dəyərlidir, çünki onlar daha çox giriş əldə etmək üçün istifadə edilə biləcək həssas məlumatları ehtiva edə bilər.
