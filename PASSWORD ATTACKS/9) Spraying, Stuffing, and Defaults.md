## Püskürtmə (Spraying), Doldurma (Stuffing) və Defoltlar (Varsayılanlar)

### Parol Püskürtmə (Password Spraying)

**Parol püskürtmə** (Password spraying) hər hansı bir hücumçunun bir çox fərqli istifadəçi hesabı üzərində **tək bir parolu** tətbiq etməyə çalışdığı bir növ kobud qüvvə (brute-force) hücumudur. Bu üsul, istifadəçilərin defolt və ya standart bir parolla başladığı mühitlərdə xüsusilə effektiv ola bilər. Məsələn, müəyyən bir şirkətdə administratorların yeni hesabları qurarkən ümumiyyətlə `'ChangeMe123!'` parollundan istifadə etdikləri məlumdursa, yenilənməmiş hesabları müəyyən etmək üçün bu parolu bütün istifadəçi hesablarında yoxlamağa dəyər.

Hədəf sistemdən asılı olaraq, parol püskürtmə hücumlarını həyata keçirmək üçün müxtəlif alətlərdən istifadə edilə bilər. Veb tətbiqləri üçün **Burp Suite** güclü bir seçimdir, Active Directory mühitləri üçün isə **NetExec** və ya **Kerbrute** kimi alətlər tez-tez istifadə olunur.

```bash
nijatmansimov@htb[/htb]$ netexec smb 10.100.38.0/24 -u <usernames.list> -p 'ChangeMe123!'
```

-----

### Hesab Məlumatlarının Doldurulması (Credential Stuffing)

**Hesab məlumatlarının doldurulması** (Credential stuffing) hücumçunun bir xidmətdən oğurlanmış hesab məlumatlarını digərlərinə daxil olmaq üçün sınadığı başqa bir növ kobud qüvvə hücumudur. Bir çox istifadəçi parollarını və istifadəçi adlarını müxtəlif platformalarda (məsələn, e-poçt, sosial media və korporativ sistemlər) təkrar istifadə etdiyi üçün bu hücumlar bəzən uğurlu olur. Parol püskürtməsində olduğu kimi, hesab məlumatlarının doldurulması da hədəf sistemdən asılı olaraq müxtəlif alətlər vasitəsilə həyata keçirilə bilər. Məsələn, bir məlumat bazası sızmasından əldə edilmiş `istifadəçi adı:parol` məlumatlarının siyahısı varsa, biz aşağıdakı sintaksisdən istifadə edərək bir SSH xidmətinə qarşı **hydra** ilə doldurma hücumu həyata keçirə bilərik:

```bash
nijatmansimov@htb[/htb]$ hydra -C user_pass.list ssh://10.100.38.23
```

-----

### Defolt (Varsayılan) Hesab Məlumatları

Routerlər, firewallar (təhlükəsizlik divarları) və məlumat bazaları kimi bir çox sistem **defolt hesab məlumatları** ilə təchiz olunur. Ən yaxşı təcrübə administratorların quraşdırma zamanı bu məlumatları dəyişdirməsini tələb etsə də, bəzən onlar dəyişdirilmədən qalır və bu, ciddi bir təhlükəsizlik riski yaradır.

Məlum defolt hesab məlumatlarının bir neçə siyahısı onlayn mövcud olsa da, prosesi avtomatlaşdıran xüsusi alətlər də var. Geniş istifadə olunan bir nümunə, **Default Credentials Cheat Sheet** adlı alətdir ki, onu `pip3` ilə quraşdıra bilərik.

```bash
nijatmansimov@htb[/htb]$ pip3 install defaultcreds-cheat-sheet
```

Quraşdırıldıqdan sonra, müəyyən bir məhsul və ya təchizatçı ilə əlaqəli məlum defolt hesab məlumatlarını axtarmaq üçün **`creds`** əmrindən istifadə edə bilərik.

```bash
nijatmansimov@htb[/htb]$ creds search linksys
+---------------+---------------+------------+
| Product       |    username   |  password  |
+---------------+---------------+------------+
| linksys       |    <blank>    |  <blank>   |
| linksys       |    <blank>    |   admin    |
| linksys       |    <blank>    | epicrouter |
| linksys       | Administrator |   admin    |
| linksys       |     admin     |  <blank>   |
| linksys       |     admin     |   admin    |
| linksys       |    comcast    |    1234    |
| linksys       |      root     |  orion99   |
| linksys       |      user     |  tivonpw   |
| linksys (ssh) |     admin     |   admin    |
| linksys (ssh) |     admin     |  password  |
| linksys (ssh) |    linksys    |  <blank>   |
| linksys (ssh) |      root     |   admin    |
+---------------+---------------+------------+
```

İctimaiyyətə açıq siyahılar və alətlərlə yanaşı, defolt hesab məlumatları tez-tez bir xidmətin qurulması üçün tələb olunan addımları sadalayan məhsul sənədlərində (documentation) də tapıla bilər. Bəzi cihazlar və tətbiqlər istifadəçidən quraşdırma zamanı bir parol təyin etməsini istəsə də, digərləri defolt — və çox vaxt zəif — bir paroldan istifadə edir.

Təsəvvür edək ki, biz bir müştərinin şəbəkəsində istifadə olunan müəyyən tətbiqləri müəyyən etmişik. Defolt hesab məlumatlarını onlayn araşdırdıqdan sonra, onları `istifadəçi adı:parol` formatında yeni bir siyahıda birləşdirə və giriş cəhdi üçün yuxarıda qeyd olunan **`hydra`** əmrini təkrar istifadə edə bilərik.

Tətbiqlərdən başqa, defolt hesab məlumatları tez-tez routerlər (yönləndiricilər) ilə də əlaqələndirilir. Belə siyahılardan biri aşağıdakı cədvəldə verilmişdir. Router hesab məlumatlarının dəyişdirilmədən qalması daha az ehtimal olunsa da (çünki bu cihazlar şəbəkə təhlükəsizliyi üçün kritikdir), səhvlər baş verir. Məsələn, daxili test mühitlərində istifadə olunan routerlər defolt parametrlərlə qala bilər və əlavə giriş əldə etmək üçün istismar edilə bilər.

| Router Brendi | Defolt IP Ünvanı | Defolt İstifadəçi Adı | Defolt Parol |
| :--- | :--- | :--- | :--- |
| 3Com | [http://192.168.1.1](http://192.168.1.1) | admin | Admin |
| Belkin | [http://192.168.2.1](http://192.168.2.1) | admin | admin |
| BenQ | [http://192.168.1.1](http://192.168.1.1) | admin | Admin |
| D-Link | [http://192.168.0.1](http://192.168.0.1) | admin | Admin |
| Digicom | [http://192.168.1.254](http://192.168.1.254) | admin | Michelangelo |
| Linksys | [http://192.168.1.1](http://192.168.1.1) | admin | Admin |
| Netgear | [http://192.168.0.1](http://192.168.0.1) | admin | password |
