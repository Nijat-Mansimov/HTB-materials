**John The Ripper-ə Giriş**

John the Ripper (həmçinin **JtR** və ya **john**) kobud qüvvə və lüğət hücumları daxil olmaqla müxtəlif hücumlar vasitəsilə parolları sındırmaq üçün istifadə olunan məşhur nüfuzetmə testi alətidir. O, əvvəlcə UNIX əsaslı sistemlər üçün hazırlanmış və ilk dəfə 1996-cı ildə buraxılmış açıq mənbəli proqram təminatıdır. Müxtəlif imkanlarına görə təhlükəsizlik sənayesinin əsas alətinə çevrilmişdir. **"jumbo"** variantı, performans optimallaşdırmaları, çoxdilli söz siyahıları kimi əlavə funksiyaları və 64-bit arxitekturalar üçün dəstəyi olduğu üçün istifadəmiz üçün tövsiyə olunur. Bu versiya parolları daha yüksək dəqiqlik və sürətlə sındıra bilir. JtR-ə müxtəlif növ faylları və heşləri JtR tərəfindən istifadə edilə bilən formatlara çevirmək üçün müxtəlif alətlər daxildir. Bundan əlavə, proqram təminatı cari təhlükəsizlik tendensiyaları və texnologiyaları ilə ayaqlaşmaq üçün müntəzəm olaraq yenilənir.

-----

### Sındırma Rejimləri

#### Tək Sındırma Rejimi (*Single crack mode*)

**Tək sındırma rejimi** (*Single crack mode*) əsasən Linux etibarlı məlumatlarını hədəfləyərkən faydalı olan, qaydalara əsaslanan bir sındırma texnikasıdır. O, qurbanın istifadəçi adına, ev qovluğunun adına və **GECOS** dəyərlərinə (tam ad, otaq nömrəsi, telefon nömrəsi və s.) əsaslanaraq parol namizədləri yaradır. Bu sətirlər parollarda görünən ümumi sətir dəyişikliklərini tətbiq edən böyük bir qaydalar toplusuna qarşı işlədilir (məsələn, əsl adı **Bob Smith** olan bir istifadəçi parolu olaraq **Smith1** istifadə edə bilər).

**Qeyd:** Linux autentifikasiya prosesi, eləcə də sındırma qaydaları, sonrakı bölmələrdə ətraflı şəkildə nəzərdən keçiriləcəkdir. Aşağıdakı nümunə nümayiş məqsədləri üçün sadələşdirilib.

Təsəvvür edin ki, biz hücum edənlər olaraq aşağıdakı məzmunlu **passwd** faylı ilə qarşılaşdıq:

`r0lf:$6$ues25dIanlctrWxg$nZHVz2z4kCy1760Ee28M1xtHdGoy0C2cYzZ8l2sVa1kIa8K9gAcdBP.GI6ng/qA4oaMrgElZ1Cb9OeXO4Fvy3/:0:0:Rolf Sebastian:/home/r0lf:/bin/bash`

Faylın məzmununa əsaslanaraq, qurbanın **r0lf** istifadəçi adına, **Rolf Sebastian** əsl adına və **/home/r0lf** ev qovluğuna sahib olduğu qənaətinə gəlmək olar. Tək sındırma rejimi bu məlumatı parol namizədləri yaratmaq və onları heşə qarşı sınamaq üçün istifadə edəcək. Biz hücumu aşağıdakı əmrlə işə sala bilərik:

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ john --single passwd
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes

Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
[...SNIP...]        (r0lf)     
1g 0:00:00:00 DONE 1/3 (2025-04-10 07:47) 12.50g/s 5400p/s 5400c/s 5400C/s NAITSABESFL0R..rSebastiannaitsabeSr
Use the "--show" option to display all of the cracked passwords reliably
Session completed.
```

Bu halda, parol heşi uğurla sındırıldı.

#### Söz Siyahısı Rejimi (*Wordlist mode*)

**Söz siyahısı rejimi** (*Wordlist mode*) bir lüğət hücumu ilə parolları sındırmaq üçün istifadə olunur, yəni təmin edilmiş söz siyahısındakı bütün parolları parol heşinə qarşı sınaqdan keçirir. Əmrin əsas sintaksisi aşağıdakı kimidir:

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ john --wordlist=<wordlist_file> <hash_file>
```

Parol heşlərini sındırmaq üçün istifadə olunan söz siyahısı faylı (və ya faylları) hər sətirdə bir söz olmaqla, düz mətn formatında olmalıdır. Birdən çox söz siyahısı vergüllə ayrılaraq göstərilə bilər. Xüsusi və ya daxili qaydalar **--rules** arqumentindən istifadə edilərək göstərilə bilər. Bunlar rəqəmləri əlavə etmək, hərfləri böyütmək və xüsusi simvollar əlavə etmək kimi çevrilmələr istifadə edərək parol namizədləri yaratmaq üçün tətbiq edilə bilər.

#### Artan Rejim (*Incremental mode*)

**Artan rejim** (*Incremental mode*) statistik bir modelə (Markov zəncirləri) əsaslanaraq parol namizədləri yaradan güclü, kobud qüvvə üslubunda bir parol sındırma rejimidir. O, müəyyən bir simvol dəsti tərəfindən müəyyən edilmiş bütün simvol kombinasiyalarını sınaqdan keçirmək, təlim məlumatlarına əsaslanaraq daha çox ehtimal olunan parolları prioritetləşdirmək üçün nəzərdə tutulmuşdur.

Bu rejim ən əhatəli, eyni zamanda ən çox vaxt aparan rejimdir. Söz siyahısı rejimindən fərqli olaraq, dinamik olaraq parol təxminləri yaradır və əvvəlcədən təyin edilmiş söz siyahısına etibar etmir. Tamamilə təsadüfi kobud qüvvə hücumlarından fərqli olaraq, Artan rejim təhsilə əsaslanan təxminlər etmək üçün statistik bir modeldən istifadə edir, nəticədə sadə kobud qüvvə hücumlarından xeyli daha səmərəli bir yanaşma olur.

Əsas sintaksis:

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ john --incremental <hash_file>
```

Varsayılan olaraq, JtR simvol dəstlərini və parol uzunluqlarını təyin edən konfiqurasiya faylında (john.conf) əvvəlcədən təyin edilmiş artan rejimlərdən istifadə edir. Xüsusi simvollardan və ya xüsusi nümunələrdən istifadə edən parolları hədəfləmək üçün bunları fərdiləşdirə və ya özünüzünküləri təyin edə bilərsiniz.

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ grep '# Incremental modes' -A 100 /etc/john/john.conf
# Incremental modes
# This is for one-off uses (make your own custom.chr).
# A charset can now also be named directly from command-line, so no config
# entry needed: --incremental=whatever.chr
[Incremental:Custom]
File = $JOHN/custom.chr
MinLen = 0
# The theoretical CharCount is 211, we've got 196.
[Incremental:UTF8]
File = $JOHN/utf8.chr
MinLen = 0
CharCount = 196

# This is CP1252, a super-set of ISO-8859-1.
# The theoretical CharCount is 219, we've got 203.
[Incremental:Latin1]
File = $JOHN/latin1.chr
MinLen = 0
CharCount = 203

[Incremental:ASCII]
File = $JOHN/ascii.chr
MinLen = 0
MaxLen = 13
CharCount = 95

...SNIP...
```

**Qeyd:** Bu rejim, xüsusilə uzun və ya mürəkkəb parollar üçün resurs tutumlu və yavaş ola bilər. Simvol dəstini və uzunluğunu fərdiləşdirmək performansı yaxşılaşdıra və hücumu fokuslaya bilər.

-----

### Heş Formatlarının Müəyyən Edilməsi

Bəzən parol heşləri naməlum bir formatda görünə bilər və hətta John the Ripper (JtR) də onları tam əminliklə müəyyən edə bilməz. Məsələn, aşağıdakı heşi nəzərdən keçirin:

`193069ceb0461e1d40d216e32c79c704`

Bir fikir əldə etməyin bir yolu, JtR-in **nümunə heş sənədləşməsinə** və ya **PentestMonkey tərəfindən bu siyahıya** baxmaqdır. Hər iki mənbə çoxsaylı nümunə heşləri, eləcə də müvafiq JtR formatını sadalayır. Başqa bir seçim, potensial formatları təklif etmək üçün təchiz edilmiş heşləri daxili siyahıya qarşı yoxlayan **hashID** kimi bir alətdən istifadə etməkdir. **-j** bayrağını əlavə etməklə, hashID, heş formatına əlavə olaraq, müvafiq JtR formatını da sadalayacaq:

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ hashid -j 193069ceb0461e1d40d216e32c79c704
Analyzing '193069ceb0461e1d40d216e32c79c704'
[+] MD2 [JtR Format: md2]
[+] MD5 [JtR Format: raw-md5]
[+] MD4 [JtR Format: raw-md4]
[+] Double MD5 
[+] LM [JtR Format: lm]
[+] RIPEMD-128 [JtR Format: ripemd-128]
[+] Haval-128 [JtR Format: haval-128-4]
[+] Tiger-128 
[+] Skein-256(128) 
[+] Skein-512(128) 
[+] Lotus Notes/Domino 5 [JtR Format: lotus5]
[+] Skype 
[+] Snefru-128 [JtR Format: snefru-128]
[+] NTLM [JtR Format: nt]
[+] Domain Cached Credentials [JtR Format: mscach]
[+] Domain Cached Credentials 2 [JtR Format: mscach2]
[+] DNSSEC(NSEC3) 
[+] RAdmin v2.x [JtR Format: radmin]
```

Təəssüf ki, nümunəmizdə heşin hansı formatda olduğu hələ də tamamilə aydın deyil. Bəzən belə olacaq və bu, sadəcə olaraq bir pentester kimi qarşılaşacağınız "problemlərdən" biridir. Çox vaxt heşin haradan gəldiyi konteksti format haqqında əsaslı bir qərar vermək üçün kifayət edəcəkdir. Bu xüsusi nümunədə, heş formatı **RIPEMD-128**-dir.

JtR yüzlərlə heş formatını dəstəkləyir, bunlardan bəziləri aşağıdakı cədvəldə verilmişdir. Hədəf heşlərin hansı formatda olduğunu JtR-ə bildirmək üçün **--format** arqumenti təmin edilə bilər.

| Heş Formatı | Nümunə Əmri | Təsvir |
| :--- | :--- | :--- |
| afs | `john --format=afs [...] <hash_file>` | AFS (Andrew Fayl Sistemi) parol heşləri |
| bfegg | `john --format=bfegg [...] <hash_file>` | Eggdrop IRC botlarında istifadə olunan bfegg heşləri |
| bf | `john --format=bf [...] <hash_file>` | Blowfish əsaslı crypt(3) heşləri |
| bsdij | `john --format=bsdi [...] <hash_file>` | BSDi crypt(3) heşləri |
| crypt(3) | `john --format=crypt [...] <hash_file>` | Ənənəvi Unix crypt(3) heşləri |
| des | `john --format=des [...] <hash_file>` | Ənənəvi DES əsaslı crypt(3) heşləri |
| ... | ... | ... |
| NT | `john --format=nt [...] <hash_file>` | NT (Windows NT) parol heşləri |
| raw-md5 | `john --format=raw-md5 [...] <hash_file>` | Xam MD5 parol heşləri |
| raw-sha256 | `john --format=raw-sha256 [...] <hash_file>` | Xam SHA256 parol heşləri |
| zip | `john --format=zip [...] <hash_file>` | ZIP (WinZip) parol heşləri |
| **...** | **...** | **...SNIP...** |

-----

### Faylların Sındırılması

JtR ilə parol qorunan və ya şifrələnmiş faylları sındırmaq da mümkündür. JtR ilə faylları emal etmək və JtR ilə uyğun heşlər yaratmaq üçün istifadə edilə bilən çoxsaylı **"2john"** alətləri gəlir. Bu alətlər üçün ümumi sintaksis belədir:

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ <tool> <file_to_crack> > file.hash
```

JtR ilə birlikdə gələn bəzi alətlər bunlardır:

| Alət | Təsvir |
| :--- | :--- |
| **pdf2john** | PDF sənədlərini John üçün çevirir |
| **ssh2john** | SSH özəl açarlarını John üçün çevirir |
| **mscash2john** | MS Cash heşlərini John üçün çevirir |
| **keychain2john** | OS X açarlıq fayllarını John üçün çevirir |
| **rar2john** | RAR arxivlərini John üçün çevirir |
| **zip2john** | ZIP arxivlərini John üçün çevirir |
| **office2john** | MS Office sənədlərini John üçün çevirir |
| ... | ...SNIP... |

Daha böyük bir kolleksiya **Pwnbox**-da tapıla bilər:

```
  John The Ripper-ə Giriş
nijatmansimov@htb[/htb]$ locate *2john*
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
...SNIP...
```
