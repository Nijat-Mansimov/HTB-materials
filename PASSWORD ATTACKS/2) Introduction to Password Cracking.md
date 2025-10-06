**Parolların Sındırılmasına Giriş**

Parollar saxlanılarkən, hücum edənin əlinə düşəcəyi təqdirdə bir qədər müdafiə təmin etmək üçün adətən **heşlənir** (*hashed*). **Heşləmə** (*Hashing*) ixtiyari sayda giriş baytını (tipik olaraq) sabit ölçülü çıxışa çevirən riyazi funksiyadır; ümumi heş funksiyası nümunələri **MD5** və **SHA-256**-dır.

Məsələn, **Soccer06\!** parolu. Müvafiq **MD5** və **SHA-256** heşləri aşağıdakı əmrlərlə yaradıla bilər:

```
  Parolların Sındırılmasına Giriş
bmdyy@htb:~$ echo -n Soccer06! | md5sum
40291c1d19ee11a7df8495c4cccefdfa  -
bmdyy@htb:~$ echo -n Soccer06! | sha256sum
a025dc6fabb09c2b8bfe23b5944635f9b68433ebd9a1a09453dd4fee00766d93  -
```

Heş funksiyaları **bir istiqamətdə** işləmək üçün nəzərdə tutulmuşdur. Bu o deməkdir ki, yalnız heşə əsaslanaraq orijinal parolu tapmaq mümkün olmamalıdır. Hücum edənlər bunu etməyə çalışdıqda, buna **parol sındırılması** (*password cracking*) deyilir. Ümumi texnikalar **qurğuşun cədvəllərindən** (*rainbow tables*) istifadə etmək, **lüğət hücumlarını** (*dictionary attacks*) həyata keçirmək və adətən son çarə olaraq **kobud qüvvə hücumlarını** (*brute-force attacks*) həyata keçirməkdir.

**Qurğuşun Cədvəlləri (*Rainbow Tables*)**

Qurğuşun cədvəlləri müəyyən bir heş funksiyası üçün giriş və çıxış dəyərlərinin böyük, əvvəlcədən tərtib edilmiş xəritələridir. Parolun müvafiq heşi artıq xəritələnmişsə, bu cədvəllər parolu çox tez müəyyən etmək üçün istifadə edilə bilər.

| Parol | MD5 Heşi |
| :--- | :--- |
| 123456 | e10adc3949ba59abbe56e057f20f883e |
| 123458 | 27ccb0eea8a706c4c34a16891f84e7b1 |
| 123456789 | 25f9e794323b453885f5181f1b624d0b |
| password | 5f4dcc3b5aa765d61d8327deb882cf99 |
| iloveyou | f25a2fc72690b780b2a14e140ef6a9e0 |
| princess | 8afa847f50a716e64932d995c8e7435a |
| 1234567f | cea920f7412b5da7be0cf42b8c93759 |
| rockyou | f806fc5a2a0d5ba2471600758452799c |
| 123456782 | 5d55ad283aa400af464c76d713c07ad |
| abc123 | e99a18c428cb38d5f260853678922e03 |
| ... | ...SNIP... |
| ... | ...SNIP... |

Qurğuşun cədvəlləri belə güclü bir hücum olduğu üçün **duzlama** (*salting*) istifadə olunur. Kriptoqrafik mənada **duz** (*salt*), heşlənmədən əvvəl parolla əlavə edilən təsadüfi bayt ardıcıllığıdır. Təsiri maksimuma çatdırmaq üçün duzlar təkrar istifadə edilməməlidir, məsələn, bir verilənlər bazasında saxlanılan bütün parollar üçün. Məsələn, **Th1sIsTh3S@lt\_** duzu eyni parolun əvvəlinə əlavə edilərsə, MD5 heşi indi belə olacaq:

```
  Parolların Sındırılmasına Giriş
nijatmansimov@htb[/htb]$ echo -n Th1sIsTh3S@lt_Soccer06! | md5sum
90a10ba83c04e7996bc53373170b5474  -
```

Duz gizli bir dəyər deyil — bir sistem autentifikasiya sorğusunu yoxlamağa getdikdə, parol heşinin uyğun gəlib-gəlmədiyini yoxlaya bilməsi üçün hansı duzun istifadə edildiyini bilməlidir. Buna görə də, duzlar adətən müvafiq heşlərin əvvəlinə əlavə edilir. Bu texnikanın qurğuşun cədvəllərinə qarşı işləməsinin səbəbi, doğru parol xəritələnmiş olsa belə, duz və parol birləşməsinin (xüsusilə duz çap edilə bilməyən simvolları ehtiva edirsə) yəqin ki, xəritələnməməsidir. Qurğuşun cədvəllərini yenidən effektiv etmək üçün, bir hücum edən hər bir mümkün duzu hesablamaq üçün öz xəritəsini yeniləməli olacaq. **Yalnız bir baytdan** ibarət olan bir duz, əvvəlki **15 milyard** girişin **3.84 trilyon** (256 faktor) olması demək olardı.

**Kobud Qüvvə Hücumu (*Brute-force attack*)**

**Kobud qüvvə** (*brute-force*) hücumu, doğru parol tapılana qədər hər mümkün hərf, rəqəm və simvol kombinasiyasını sınaqdan keçirməyi nəzərdə tutur. Aydındır ki, bu, xüsusilə uzun parollar üçün çox vaxt apara bilər - lakin qısa parollar (\<9 simvol) istehlak avadanlığında belə, həyata keçirilə bilən hədəflərdir. Kobud qüvvə ilə sındırma **100% effektiv** olan yeganə parol sındırma texnikasıdır - yəni, kifayət qədər vaxt verildikdə, istənilən parol bu texnika ilə sındırılacaqdır. Bununla belə, güclü parollar üçün nə qədər vaxt apardığına görə nadir hallarda istifadə olunur və adətən daha səmərəli olan **maska hücumları** (*mask attacks*) ilə əvəz olunur. Bu, növbəti bir neçə bölməni əhatə edəcəyimiz bir şeydir.

| Kobud Qüvvə Sınağı | MD5 Heşi |
| :--- | :--- |
| ...SNIP... | ...SNIP... |
| Sxejd | 2cdc813ef26e6d20c854adb107279338 |
| Sxeje | 7703349a1f943f9da6d1dfcda51f3b63 |
| Sxejf | db914f10854b97946046eabab2287178 |
| Sxejg | c0ceb70c0e0f2c3da94e75ae946f29dc |
| Sxejh | 4dca0d2b706e9344985d48f95e646ce8 |
| Sxeji | 66b5fa128df895d50b2d70353a7968a7 |
| Sxejj | dd7097ba514c136caac321e321b1b5ca |
| Sxejk | c0eb1193e62a7a57dec2fafd4177f7d9 |
| Sxejl | 5ad8e1282437da255b866d22339d1b53 |
| Sxejm | c4b95c1fe6d2a4f22620efd54c066664 |
| ...SNIP... | ...SNIP... |

**Qeyd:** Kobud qüvvə ilə sındırma sürəti istifadə olunan heşləmə alqoritmindən və avadanlıqdan çox asılıdır. Tipik bir şirkət noutbukunda, **hashcat** kimi bir alət MD5-ə hücum edərkən saniyədə **beş milyondan** çox parol təxmin edə bilər, eyni zamanda DCC2 heşini hədəfləyərkən saniyədə yalnız **on min** idarə edə bilər.

**Lüğət Hücumu (*Dictionary attack*)**

**Lüğət** (*Dictionary*) hücumu, başqa cür **söz siyahısı** (*wordlist*) hücumu kimi tanınır, parolları sındırmaq üçün ən **səmərəli** texnikalardan biridir, xüsusilə nüfuzetmə sınaqçılarının adətən etdiyi kimi vaxt məhdudiyyətləri altında işləyərkən. Hər mümkün simvol kombinasiyasını sınaqdan keçirmək əvəzinə, statistik cəhətdən ehtimal olunan parolları ehtiva edən bir siyahı istifadə olunur. Parol sındırılması üçün məşhur söz siyahıları **rockyou.txt** və **SecLists**-ə daxil olanlardır.

```
  Parolların Sındırılmasına Giriş
nijatmansimov@htb[/htb]$ head --lines=20 /usr/share/wordlists/rockyou.txt
123456
12345
123456789
password
iloveyou
princess
1234567
rockyou
12345678
abc123
nicole
daniel
babygirl
monkey
lovely
jessica
654321
michael
ashley
qwerty
```

**Qeyd:** **rockyou.txt** 2009-cu ildə **RockYou** veb-saytı sındırıldıqda sızdırılan **14 milyondan** çox real parolun siyahısıdır. Təəccüblüdür ki, şirkət bütün istifadəçi parollarını şifrələnməmiş saxlamaq qərarına gəlib\!
