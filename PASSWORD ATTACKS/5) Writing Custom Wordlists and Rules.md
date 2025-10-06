**Fərdi Söz Siyahıları və Qaydalar Yazmaq**

Bir çox istifadəçi parollarını təhlükəsizlikdən çox **sadəliyə** əsaslanaraq yaradır. Bu insan meylini (hansı ki, tez-tez təhlükəsizlik tədbirlərini zəiflədir) azaltmaq üçün, xüsusi parol tələblərini tətbiq etmək üçün sistemlərdə parol siyasətləri həyata keçirilə bilər. Məsələn, bir sistem böyük hərflərin, xüsusi simvolların və rəqəmlərin daxil edilməsini tələb edə bilər. Əksər parol siyasətləri minimum uzunluğu — adətən səkkiz simvol — tələb edir və müəyyən edilmiş hər bir kateqoriyadan ən azı bir simvol olmasını şərtləndirir.

Əvvəlki bölmələrdə biz sadə parolları təxmin etməkdə uğurlu olduq. Lakin, bu texnikaları istifadəçilərin daha mürəkkəb parollar yaratmasını tələb edən sistemlərə tətbiq etmək xeyli çətinləşir.

Təəssüf ki, parol siyasətləri mövcud olsa belə, istifadəçilərin zəif parollar yaratma meyli davam edir. Əksər fərdlər parol yaradarkən proqnozlaşdırıla bilən nümunələrə əməl edirlər, tez-tez daxil olunan xidmətlə sıx bağlı olan sözləri daxil edirlər. Məsələn, bir çox işçi şirkətin adını özündə əks etdirən parollar seçir. Şəxsi üstünlüklər və maraqlar da əhəmiyyətli rol oynayır — bunlar ev heyvanlarına, dostlara, idman növlərinə, hobbilərə və gündəlik həyatın digər aspektlərinə istinadları əhatə edə bilər. Əsas OSINT (Açıq Mənbə Kəşfiyyatı) texnikaları bu cür şəxsi məlumatları üzə çıxarmaqda olduqca təsirli ola bilər və parol təxmin etməkdə kömək edə bilər. OSINT haqqında daha çox məlumat **OSINT: Korporativ Kəşfiyyat modulu**-nda tapa bilərsiniz.

İstifadəçilər ən ümumi parol siyasətlərinə uyğunlaşmaq üçün adətən parollarına aşağıdakı əlavələrdən istifadə edirlər:

| Təsvir | Parol Sintaksisi |
| :--- | :--- |
| İlk hərf böyükdür | **Password** |
| Rəqəmlər əlavə etmək | **Password123** |
| İl əlavə etmək | **Password2022** |
| Ay əlavə etmək | **Password02** |
| Son simvol nida işarəsidir | **Password2022\!** |
| Xüsusi simvollar əlavə etmək | **P@ssw0rd2022\!** |

İstifadəçilərin parollarını mümkün qədər sadə saxlamağa meylli olduğunu bilərək, ehtimal olunan zəif parollar yaratmaq üçün qaydalar yarada bilərik. **WP Engine** tərəfindən verilən statistikaya görə, əksər parollar **on** simvoldan uzun deyil. Bir yanaşma, ən azı beş simvol uzunluğunda olan tanış terminləri — məsələn, ev heyvanlarının adları, hobbilər, şəxsi üstünlüklər və ya digər ümumi maraqlar — seçməkdir. Məsələn, bir istifadəçi tək bir sözü (məsələn, cari ayın adını) seçsə, cari ili əlavə etsə və sonuna bir xüsusi simvol qoysa, nəticə tipik on simvolluq parol tələbini ödəyə bilər. Əksər təşkilatların müntəzəm parol dəyişikliyini tələb etdiyini nəzərə alsaq, bir istifadəçi sadəcə ayın adını dəyişdirərək və ya tək bir rəqəmi artıraraq parolunu dəyişə bilər.

Tək bir girişə malik parol siyahısından istifadə edərək sadə bir nümunəyə baxaq.

```
  Fərdi Söz Siyahıları və Qaydalar Yazmaq
nijatmansimov@htb[/htb]$ cat password.list
password
```

Biz Hashcat-dən potensial adların və etiketlərin siyahılarını xüsusi mutasiya qaydaları ilə birləşdirərək fərdi söz siyahıları yaratmaq üçün istifadə edə bilərik. Hashcat simvolları, sözləri və onların çevrilmələrini təyin etmək üçün xüsusi bir sintaksisdən istifadə edir. Tam sintaksis rəsmi **Hashcat qaydalara əsaslanan hücum sənədlərində** sənədləşdirilmişdir, lakin aşağıdakı nümunələr Hashcat-in giriş sözlərini necə dəyişdirdiyini anlamaq üçün kifayətdir.

| Funksiya | Təsvir |
| :--- | :--- |
| **:** | Heç nə etmə |
| **l** | Bütün hərfləri kiçilt |
| **u** | Bütün hərfləri böyüt |
| **c** | İlk hərfi böyüt və digərlərini kiçilt |
| **sXY** | X-in bütün hallarını Y ilə əvəz et |
| **$\!** | Sona nida simvolu əlavə et |

Hər bir qayda yeni bir sətirdə yazılır və verilmiş bir sözün necə çevrilməli olduğunu müəyyənləşdirir. Yuxarıda göstərilən funksiyaları bir fayla yazsaq, belə görünə bilər:

```
  Fərdi Söz Siyahıları və Qaydalar Yazmaq
nijatmansimov@htb[/htb]$ cat custom.rule
:
c
so0
c so0
sa@
c sa@
$!
c$!
so0$!
sa@$!
c so0$!
c sa@$!
so0 sa@
c so0 sa@
```

Biz **custom.rule**-dakı qaydaları **password.list**-dəki hər bir sözə tətbiq etmək və dəyişdirilmiş nəticələri **mut\_password.list**-də saxlamaq üçün aşağıdakı əmrdən istifadə edə bilərik.

```
  Fərdi Söz Siyahıları və Qaydalar Yazmaq
nijatmansimov@htb[/htb]$ hashcat --force password.list -r custom.rule --stdout | sort -u > mut_password.list
```

Bu halda, tək giriş sözü on beş dəyişdirilmiş variant yaradacaq.

```
  Fərdi Söz Siyahıları və Qaydalar Yazmaq
nijatmansimov@htb[/htb]$ cat mut_password.list
password
Password
passw0rd
Passw0rd
p@ssword
P@ssword
P@ssw0rd
password!
Password!
passw0rd!
p@ssword!
Passw0rd!
P@ssword!
p@ssw0rd!
P@ssw0rd!
```

Hashcat və JtR hər ikisi parol generasiyası və sındırılması üçün istifadə edilə bilən əvvəlcədən qurulmuş qayda siyahıları ilə gəlir. Ən təsirli və geniş istifadə olunan qayda dəstlərindən biri, tez-tez uğurlu parol təxminləri ilə nəticələnən ümumi çevrilmələri tətbiq edən **best64.rule**-dır. Qeyd etmək vacibdir ki, parol sındırılması və fərdi söz siyahılarının yaradılması əksər hallarda bir təxmin oyunudur. Şirkət adı, coğrafi bölgə, sənaye və istifadəçilərin parol yaradarkən seçə biləcəyi digər mövzular və ya açar sözlər kimi amilləri nəzərə alaraq, parol siyasəti haqqında məlumatımız varsa, bunu daralda və daha məqsədyönlü təxminlər edə bilərik. İstisnalar, əlbəttə ki, parolların sızdırıldığı və birbaşa əldə edildiyi halları əhatə edir.

-----

### CeWL istifadə edərək Söz Siyahıları Yaratmaq

Bir şirkətin veb-saytından potensial sözləri skan etmək və onları ayrıca bir siyahıda saxlamaq üçün **CeWL** (*Custom Word List Generator*) adlı bir alətdən istifadə edə bilərik. Daha sonra bu siyahını istədiyimiz qaydalarla birləşdirərək fərdi parol siyahısı yarada bilərik — bu, bir işçi üçün doğru parolu ehtiva etmək ehtimalı daha yüksəkdir. Biz skan dərinliyi ( **-d**), sözün minimum uzunluğu ( **-m**), tapılan sözlərin kiçik hərflərlə saxlanılması (**--lowercase**) və nəticələri saxlamaq istədiyimiz fayl (**-w**) kimi bəzi parametrləri müəyyən edirik.

```
  Fərdi Söz Siyahıları və Qaydalar Yazmaq
nijatmansimov@htb[/htb]$ cewl https://www.inlanefreight.com -d 4 -m 6 --lowercase -w inlane.wordlist
nijatmansimov@htb[/htb]$ wc -l inlane.wordlist
326
```
