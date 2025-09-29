# Fayl Deskriptorları və Yönləndirmələr

Unix/Linux əməliyyat sistemlərində **fayl deskriptoru (FD)** kernel tərəfindən idarə olunan bir istinaddır və sistemin Giriş/Çıxış (I/O) əməliyyatlarını idarə etməsinə imkan verir. Fayl deskriptoru açıq fayl, socket və ya digər hər hansı I/O resursunu təmsil edən unikal identifikatordur. Windows əməliyyat sistemlərində buna **file handle** deyilir. Əsasən, fayl deskriptoru sistemin aktiv I/O bağlantılarını izləmə üsuludur; məsələn, fayldan oxumaq və ya fayla yazmaq.

Bunu belə təsəvvür edin: palto qarderobuna qoyanda sizə verilən bilet nömrəsi kimi. Bilet (fayl deskriptoru) sizin paltonuza (fayl və ya resursa) olan əlaqənizi göstərir və paltonu geri götürmək istədiyiniz zaman (I/O əməliyyatı) biletinizi attendant-ə (əməliyyat sistemi) təqdim edirsiniz. Attendant də bilir ki, paltonuz haradadır (hansı resursa fayl deskriptoru istinad edir). Bilet olmadan, paltonu digər paltolar arasından səmərəli tapmaq mümkün deyil; eyni qayda ilə, fayl deskriptoru olmadan əməliyyat sistemi hansı resursa əməliyyat edəcəyini bilmir. Növbəti nümunələrdə fayl deskriptorlarının əhəmiyyətini daha yaxşı anlayacaqsınız.

## Linux-da standart fayl deskriptorları

| Məqsəd                 | Deskriptor |
| ---------------------- | ---------- |
| Giriş məlumatları üçün | STDIN – 0  |
| Çıxış məlumatları üçün | STDOUT – 1 |
| Xəta ilə bağlı çıxış   | STDERR – 2 |

## STDIN və STDOUT

`cat` əmri ilə nümunəyə baxaq. `cat` işləyərkən, biz proqramın standart girişinə (STDIN - FD 0, yaşıl ilə işarələnmiş) məlumat veririk; məsələn, burada `"SOME INPUT"` var. [ENTER] düyməsini basdıqdan sonra, bu məlumat standart çıxışa (STDOUT - FD 1, qırmızı ilə işarələnmiş) geri göndərilir və terminalda göstərilir.

<img width="836" height="228" alt="image" src="https://github.com/user-attachments/assets/1e3aec0a-cc35-446f-80ae-1256034ef1bc" />

## STDOUT və STDERR

Növbəti nümunədə `find` əmri istifadə edərək, standart çıxışın (STDOUT - FD 1, yaşıl ilə işarələnmiş) və standart xətanın (STDERR - FD 2, qırmızı ilə işarələnmiş) necə göstərildiyini görəcəyik.

```bash
nijatmansimov@htb[/htb]$ find /etc/ -name shadow
```

<img width="832" height="239" alt="image" src="https://github.com/user-attachments/assets/ef5f3079-831d-4497-9403-96d9c091a0a6" />

Bu halda xəta "Permission denied" kimi işarələnir və göstərilir. Bunu səhvlər üçün fayl deskriptorunu (FD 2 - STDERR) "/dev/null" ünvanına yönləndirərək yoxlaya bilərik. Beləliklə, yaranan səhvləri "null device"ə yönləndiririk və orada bütün məlumatlar atılır.

```bash
nijatmansimov@htb[/htb]$ find /etc/ -name shadow 2>/dev/null
```

<img width="832" height="174" alt="image" src="https://github.com/user-attachments/assets/944d337d-6f82-4659-8198-436b48b2861e" />

STDOUT-u fayla yönləndirmək
İndi görürük ki, əvvəllər "Permission denied" ilə göstərilən bütün səhvlər (STDERR) artıq görünmür. İndi yalnız standart çıxışı (STDOUT) görürük və bunu standart səhvlər olmadan yalnız standart çıxışı ehtiva edəcək `results.txt` adlı fayla yönləndirə bilərik.

```bash
nijatmansimov@htb[/htb]$ find /etc/ -name shadow 2>/dev/null > results.txt
```

<img width="832" height="227" alt="image" src="https://github.com/user-attachments/assets/1c7aa321-e07c-4cb4-96fb-87621d2056cf" />

STDOUT və STDERR-i Ayrı Fayllara Yönləndirmək
Son nümunədə ">" işarəsindən əvvəl rəqəm istifadə etmədiyimizi görmüş ola bilərsiniz. Bunun səbəbi əvvəlcə bütün standart səhvləri "null device"-ə yönləndirməyimizdir, beləliklə, yalnız standart çıxışı (FD 1 - STDOUT) alırıq. Daha dəqiq olmaq üçün standart səhv (FD 2 - STDERR) və standart çıxışı (FD 1 - STDOUT) fərqli fayllara yönləndirəcəyik.

```bash
nijatmansimov@htb[/htb]$ find /etc/ -name shadow 2> stderr.txt 1> stdout.txt
```

<img width="833" height="297" alt="image" src="https://github.com/user-attachments/assets/06aead4e-44e8-41fd-a156-6590e87b85a1" />

STDIN-i Yönləndirmək
Artıq gördüyümüz kimi, fayl deskriptorları ilə birlikdə səhvləri və çıxışı ">" işarəsi ilə yönləndirə bilərik. Bu, "<" işarəsi ilə də işləyir. Lakin "<" işarəsi standart giriş üçün istifadə olunur (FD 0 - STDIN). Bu işarələri bir ox kimi düşünə bilərik, hansı ki, məlumatın "haradan" və "haraya" yönləndirilməli olduğunu göstərir. "stdout.txt" faylının məzmununu STDIN kimi istifadə etmək üçün cat əmri istifadə olunur.

```bash
nijatmansimov@htb[/htb]$ cat < stdout.txt
```

<img width="833" height="203" alt="image" src="https://github.com/user-attachments/assets/5675d787-3094-44be-b56d-2119ccc69b5e" />

STDOUT-u Fayla Yönləndirmək və Əlavə Etmək
">" işarəsini istifadə edərək STDOUT-u yönləndirdikdə, əgər fayl mövcud deyilsə, avtomatik olaraq yaradılır. Əgər fayl artıq varsa, təsdiq istəmədən üzərinə yazılır. Mövcud fayla STDOUT-u əlavə etmək istəsək, ">>" işarəsini istifadə edə bilərik.

```bash
nijatmansimov@htb[/htb]$ find /etc/ -name passwd >> stdout.txt 2>/dev/null
```

<img width="834" height="236" alt="image" src="https://github.com/user-attachments/assets/8ed25d7a-572c-4420-97d2-cf46e599dd4b" />


















































