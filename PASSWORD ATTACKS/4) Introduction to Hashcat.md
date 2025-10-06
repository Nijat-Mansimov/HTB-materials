**Hashcat-ə Giriş**

Hashcat Linux, Windows və macOS üçün məşhur bir parol sındırma alətidir. 2009-cu ildən 2015-ci ilə qədər xüsusi proqram təminatı idi, lakin o vaxtdan bəri açıq mənbə olaraq buraxılmışdır. Fantastik GPU dəstəyinə malik olaraq, müxtəlif heşləri sındırmaq üçün istifadə edilə bilər. JtR-ə bənzər şəkildə, hashcat parol heşlərinə səmərəli şəkildə hücum etmək üçün istifadə edilə bilən çoxsaylı hücum (sındırma) rejimlərini dəstəkləyir.

Hashcat-i işə salmaq üçün istifadə olunan ümumi sintaksis aşağıdakı kimidir:

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ hashcat -a 0 -m 0 <hashes> [wordlist, rule, mask, ...]
```

Yuxarıdakı əmrdə:

  * **-a** **hücum rejimini** (*attack mode*) təyin etmək üçün istifadə olunur
  * **-m** **heş növünü** (*hash type*) təyin etmək üçün istifadə olunur
  * **\<hashes\>** ya bir heş sətri, ya da eyni növ bir və ya daha çox parol heşi ehtiva edən bir fayldır
  * **\[wordlist, rule, mask, ...]** hücum rejimindən asılı olan əlavə arqumentlər üçün yer tutucudur

-----

### Heş Növləri (*Hash types*)

Hashcat yüzlərlə fərqli heş növünü dəstəkləyir, onların hər birinə bir ID təyin olunur. Əlaqəli ID-lərin siyahısı **hashcat --help** əmrini işə salmaqla yaradıla bilər.

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ hashcat --help
...SNIP...

- [ Hash modes ] -      # | Name                                                       | Category  
======+============================================================+======================================
    900 | MD4                                                        | Raw Hash
      0 | MD5                                                        | Raw Hash
    100 | SHA1                                                       | Raw Hash
   1300 | SHA2-224                                                   | Raw Hash
   1400 | SHA2-256                                                   | Raw Hash
  10800 | SHA2-384                                                   | Raw Hash
   1700 | SHA2-512                                                   | Raw Hash
  17300 | SHA3-224                                                   | Raw Hash
  17400 | SHA3-256                                                   | Raw Hash
  17500 | SHA3-384                                                   | Raw Hash
  17600 | SHA3-512                                                   | Raw Hash
   6000 | RIPEMD-160                                                 | Raw Hash
...SNIP...
```

Hashcat veb-saytı naməlum bir heş növünü əl ilə müəyyən etməyə və müvafiq Hashcat heş rejiminin identifikatorunu təyin etməyə kömək edə biləcək **nümunə heşlərin** hərtərəfli siyahısını yerləşdirir.

Alternativ olaraq, **hashID** **-m** arqumentini təyin etməklə hashcat heş növünü tez bir zamanda müəyyən etmək üçün istifadə edilə bilər.

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ hashid -m '$1$FNr44XZC$wQxY6HHLrgrGX0e1195k.1'
Analyzing '$1$FNr44XZC$wQxY6HHLrgrGX0e1195k.1'
[+] MD5 Crypt [Hashcat Mode: 500]
[+] Cisco-IOS(MD5) [Hashcat Mode: 500]
[+] FreeBSD MD5 [Hashcat Mode: 500]
```

-----

### Hücum Rejimləri (*Attack modes*)

Hashcat bir çox fərqli hücum rejiminə malikdir, bunlara **lüğət** (*dictionary*), **maska** (*mask*), **kombinator** (*combinator*) və **assosiasiya** (*association*) daxildir. Bu bölmədə ilk ikisinə nəzər salacağıq, çünki onlar ehtimal ki, istifadə etməli olacağınız ən ümumi rejimlərdir.

#### Lüğət Hücumu (*Dictionary attack*)

**Lüğət hücumu** (**-a 0**) adından göründüyü kimi, bir lüğət hücumudur. İstifadəçi giriş olaraq parol heşlərini və bir söz siyahısını təqdim edir və Hashcat doğru olan tapılana qədər və ya siyahı tükənənə qədər siyahıdakı hər bir sözü potensial bir parol kimi sınaqdan keçirir.

Nümunə olaraq, SQL verilənlər bazasından aşağıdakı parol heşini çıxardığımızı təsəvvür edin: **e3e3ec5831ad5e7288241960e5d4fdb8**. Əvvəlcə, bunu **MD5** heşi olaraq müəyyən edə bilərik, hansının ki, heş ID-si **0**-dır. Bu heşi **rockyou.txt** söz siyahısından istifadə edərək sındırmağa cəhd etmək üçün aşağıdakı əmr istifadə ediləcək:

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ hashcat -a 0 -m 0 e3e3ec5831ad5e7288241960e5d4fdb8 /usr/share/wordlists/rockyou.txt
...SNIP...               

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: e3e3ec5831ad5e7288241960e5d4fdb8
...
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
...
Candidates.#1....: 010292 -> spongebob9
...
Stopped: Sat Apr 19 08:58:46 2025
```

Tək bir söz siyahısı parol heşini sındırmaq üçün tez-tez kifayət olmur. JtR-də olduğu kimi, hətta daha çox təxmin yaratmaq üçün parollara xüsusi dəyişikliklər etmək üçün **qaydalar** (*rules*) istifadə edilə bilər. Hashcat ilə gələn qayda faylları adətən **/usr/share/hashcat/rules** altında yerləşir:

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ ls -l /usr/share/hashcat/rules
total 2852
-rw-r--r-- 1 root root 309439 Apr 24  2024 Incisive-leetspeak.rule
-rw-r--r-- 1 root root  35802 Apr 24  2024 InsidePro-HashManager.rule
...
-rw-r--r-- 1 root root    933 Apr 24  2024 best64.rule
...
```

Başqa bir nümunə olaraq, SQL verilənlər bazasından əlavə bir md5 heşinin sızdırıldığını təsəvvür edin: **1b0556a75770563578569ae21392630c**. Biz onu tək **rockyou.txt** istifadə edərək sındıra bilmədik, buna görə də növbəti cəhddə bəzi ümumi qaydalara əsaslanan çevrilmələri tətbiq edə bilərik. Sınaqdan keçirə biləcəyimiz bir qayda dəsti **best64.rule**-dır, hansı ki, 64 standart parol dəyişikliyini ehtiva edir — məsələn, rəqəmləri əlavə etmək və ya simvolları "leet" ekvivalentləri ilə əvəz etmək. Bu cür hücumu həyata keçirmək üçün, aşağıda göstərildiyi kimi, əmrə **-r \<ruleset\>** seçimini əlavə edərik:

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ hashcat -a 0 -m 0 1b0556a75770563578569ae21392630c /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule
...SNIP...

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 1b0556a75770563578569ae21392630c
...
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
...
Candidates.#1....: slimshady -> drousd
...
Stopped: Sat Apr 19 09:16:37 2025
```

#### Maska Hücumu (*Mask attack*)

**Maska hücumu** (**-a 3**) açar fəzasının (*keyspace*) istifadəçi tərəfindən açıq şəkildə təyin olunduğu bir növ kobud qüvvə hücumudur. Məsələn, bir parolun səkkiz simvol uzunluğunda olduğunu bilsək, hər mümkün kombinasiyanı sınamaq əvəzinə, altı hərf və ardından iki rəqəm kombinasiyalarını sınaqdan keçirən bir maska təyin edə bilərik.

Maska, hər biri daxili və ya xüsusi bir simvol dəstini təmsil edən bir simvollar ardıcıllığını birləşdirməklə təyin olunur. Hashcat bir neçə daxili simvol dəstini ehtiva edir:

| Simvol | Simvol Dəsti |
| :--- | :--- |
| **?l** | abcdefghijklmnopqrstuvwxyz (kiçik hərflər) |
| **?u** | ABCDEFGHIJKLMNOPQRSTUVWXYZ (böyük hərflər) |
| **?d** | 0123456789 (rəqəmlər) |
| **?s** | «boşluq»\!"\#$%&'()\*+,-./:;\<=\>?@\[\]^\_\`{ (simvollar) |
| **?a** | ?l?u?d?s (bütün yuxarıdakılar) |
| **?b** | 0x00 - 0xff (bütün 8-bit baytlar) |

Xüsusi simvol dəstləri **-1**, **-2**, **-3** və **-4** arqumentləri ilə təyin edilə bilər, daha sonra **?1**, **?2**, **?3** və **?4** ilə istinad edilir.

Deyək ki, biz xüsusi olaraq böyük hərflə başlayan, dörd kiçik hərflə, bir rəqəmlə və sonra bir simvolla davam edən parolları sınamaq istəyirik. Nəticədə yaranan hashcat maskası **?u?l?l?l?l?d?s** olacaq.

```
  Hashcat-ə Giriş
nijatmansimov@htb[/htb]$ hashcat -a 3 -m 0 1e293d6912d074c0fd15844d803400dd '?u?l?l?l?l?d?s'
...SNIP...

Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Hash.Target......: 1e293d6912d074c0fd15844d803400dd
...
Guess.Mask.......: ?u?l?l?l?l?d?s [7]
...
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
...
Candidates.#1....: Uayvf7- -> Dikqn5!
...
Stopped: Sat Apr 19 09:43:08 2025
```
