# Müntəzəm İfadələr (Regular Expressions)

**Müntəzəm İfadələr (RegEx)** mətn və ya fayllarda naxışları axtarmaq üçün dəqiq planlar hazırlamaq sənəti kimidir. Onlar sizə məlumatları inanılmaz dəqiqliklə tapmağa, əvəz etməyə və manipulyasiya etməyə imkan verir. RegEx-i, məlumatları təhlil etmək, girişi yoxlamaq və ya qabaqcıl axtarış əməliyyatlarını yerinə yetirmək kimi, sizə lazım olanı dəqiq axtarmağa imkan verən **yüksək səviyyədə tənzimlənə bilən bir süzgəc** kimi düşünün.

Əsasən, müntəzəm ifadə bir axtarış naxışını birlikdə təşkil edən simvollar və simvollar ardıcıllığıdır. Bu naxışlar tez-tez **metasimbollar** adlanan xüsusi simvolları əhatə edir, hansılar ki, hərfi mətni təmsil etməkdən daha çox axtarışın strukturunu müəyyənləşdirir. Məsələn, metasimbollar sizə rəqəmləri, hərfləri və ya müəyyən bir naxışa uyğun gələn istənilən simvolu axtardığınızı göstərməyə imkan verir.

RegEx **`grep`** və ya **`sed`** kimi bir çox proqramlaşdırma dillərində və alətlərində mövcuddur, bu da onu bizim alətlər dəstimizdə çox yönlü və güclü bir vasitəyə çevirir.

-----

## Qruplaşdırma (Grouping)

Digər şeylər arasında, regex bizə axtarılan naxışları qruplaşdırmaq imkanı verir. Əsasən, regex **üç fərqli mötərizə** ilə fərqlənən üç fərqli konsepsiyaya əməl edir:

### Qruplaşdırma Operatorları

| Operatorlar | Təsvir |
| :---: | :--- |
| **1 (a)** | **Dairəvi mötərizələr** bir regexin hissələrini **qruplaşdırmaq** üçün istifadə olunur. Mötərizələrin daxilində birlikdə işlənməli olan əlavə naxışları təyin edə bilərsiniz. |
| **2 [a-z]** | **Kvadrat mötərizələr** **simvol siniflərini** təyin etmək üçün istifadə olunur. Mötərizələrin daxilində axtarılacaq simvolların siyahısını göstərə bilərsiniz. |
| **3 {1,10}** | **Burma mötərizələr** **kəmiyyət bildiricilərini (quantifiers)** təyin etmək üçün istifadə olunur. Mötərizələrin daxilində əvvəlki naxışın neçə dəfə təkrarlanacağını göstərən bir rəqəm və ya bir aralıq (range) göstərə bilərsiniz. |
| **4 |** | Həmçinin **VƏ YA operatoru** adlanır və iki ifadədən biri uyğun gəldikdə nəticələri göstərir. |
| **5 .\*** | **VƏ operatoruna** bənzər şəkildə işləyir və yalnız hər iki ifadə mövcud olduqda və müəyyən edilmiş qaydada uyğun gəldikdə nəticələri göstərir. |

Tutaq ki, **VƏ YA operatorundan** istifadə edirik. RegEx verilmiş axtarış parametrlərindən birini axtarır. Növbəti nümunədə, **`my`** və ya **`false`** sözlərini ehtiva edən sətirləri axtarırıq. Bu operatorlardan istifadə etmək üçün **`grep`**-də **`-E`** seçimi istifadə edərək **genişləndirilmiş regex** (extended regex) tətbiq etməlisiniz.

### VƏ YA operatoru (OR operator)

Müntəzəm İfadələr

```bash
cry0l1t3@htb:~$ grep -E "(my|false)" /etc/passwd
lxd:x:105:65534::/var/lib/lxd/:/bin/false
pollinate:x:109:1::/var/cache/pollinate:/bin/false
mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false
```

İki axtarış parametrindən biri həmişə üç sətirdə baş verdiyi üçün, bütün üç sətir uyğun olaraq göstərilir. Lakin, **VƏ operatorundan** istifadə etsək, eyni axtarış parametrləri üçün fərqli bir nəticə əldə edəcəyik.

### VƏ operatoru (AND operator)

Müntəzəm İfadələr

```bash
cry0l1t3@htb:~$ grep -E "(my.*false)" /etc/passwd
mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false
```

Bu əmrlə əsasən dediyimiz odur ki, biz həm **`my`** həm də **`false`** görmək istədiyimiz bir sətir axtarırıq. Sadələşdirilmiş bir nümunə, **`grep`**-i iki dəfə istifadə etmək və bu şəkildə görünməkdir:

Müntəzəm İfadələr

```bash
cry0l1t3@htb:~$ grep -E "my" /etc/passwd | grep -E "false"
mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false
```
