## Subnetləmə (Subnetting)

IPv4 ünvanlarının ünvan diapazonunun bir neçə kiçik ünvan diapazonuna bölünməsinə **subnetləmə** deyilir.

**Altşəbəkə (Subnet)** eyni şəbəkə ünvanına malik IP ünvanlarından istifadə edən bir şəbəkənin məntiqi seqmentidir. Altşəbəkəni böyük bir bina dəhlizində etiketlənmiş bir giriş kimi düşünə bilərik. Məsələn, bu, bir şirkət binasının müxtəlif şöbələrini ayıran şüşə qapı ola bilər. Subnetləmə köməyi ilə biz xüsusi bir altşəbəkəni özümüz yarada və ya müvafiq şəbəkənin aşağıdakı xülasəsini tapa bilərik:

* **Şəbəkə ünvanı**
* **Yayımlama ünvanı** (Broadcast address)
* **Birinci host**
* **Sonuncu host**
* **Hostların sayı**

Nümunə olaraq aşağıdakı IPv4 ünvanını və altşəbəkə maskasını götürək:

* **IPv4 Ünvanı:** $192.168.12.160$
* **Altşəbəkə Maskası:** $255.255.255.192$
* **CIDR:** $192.168.12.160/26$

Biz artıq bilirik ki, bir IP ünvanı **şəbəkə hissəsinə** və **host hissəsinə** bölünür.

---

### Şəbəkə Hissəsi (Network Part)

| Təfərrüatlar | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | Onluq |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IPv4** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $1010\ 0000$ | $192.168.12.160/26$ |
| **Altşəbəkə maskası** | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $1100\ 0000$ | $255.255.255.192$ |
| **Bitlər** | $/8$ | $/16$ | $/24$ | $/32$ | |

Subnetləmədə biz altşəbəkə maskasını IPv4 ünvanı üçün bir **şablon** (*template*) kimi istifadə edirik. Altşəbəkə maskasındakı **1-bitlərdən** bilirik ki, IPv4 ünvanındakı hansı bitlər **dəyişdirilə bilməz**. Bunlar **sabitdir** və buna görə də altşəbəkənin yerləşdiyi "əsas şəbəkəni" müəyyənləşdirir.

---

### Host Hissəsi (Host Part)

| Təfərrüatlar | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | Onluq |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IPv4** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $1010\ 0000$ | $192.168.12.160/26$ |
| **Altşəbəkə maskası** | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $1100\ 0000$ | $255.255.255.192$ |
| **Bitlər** | $/8$ | $/16$ | $/24$ | $/32$ | |

**Host hissəsindəki** bitlər **birinci** və **sonuncu** ünvana dəyişdirilə bilər. Birinci ünvan müvafiq altşəbəkə üçün **şəbəkə ünvanıdır**, sonuncu ünvan isə **yayımlama ünvanıdır** (*broadcast address*).

**Şəbəkə ünvanı** məlumat paketinin çatdırılması üçün həyati əhəmiyyətə malikdir. Əgər **şəbəkə ünvanı** mənbə və təyinat ünvanı üçün eynidirsə, məlumat paketi eyni altşəbəkə daxilində çatdırılır. Əgər şəbəkə ünvanları fərqlidirsə, məlumat paketi **defolt şlüz** (*default gateway*) vasitəsilə başqa bir altşəbəkəyə yönləndirilməlidir.

**Altşəbəkə maskası** bu ayrımın harada baş verdiyini müəyyən edir.

### Şəbəkə və Host Hissələrinin Ayrılması

| Təfərrüatlar | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | Onluq |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IPv4** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $10**|**10\ 0000$ | $192.168.12.160/26$ |
| **Altşəbəkə maskası** | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $11**|**00\ 0000$ | $255.255.255.192$ |
| **Bitlər** | $/8$ | $/16$ | $/24$ | $/32$ | |

---

### Şəbəkə Ünvanı (Network Address)

İndi IPv4 ünvanının **host hissəsindəki** bütün bitləri **$0$**-a təyin etsək, müvafiq altşəbəkənin **şəbəkə ünvanını** alırıq.

| Təfərrüatlar | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | Onluq |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IPv4** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $10**|00\ 0000** | $192.168.12.128/26$ |
| **Altşəbəkə maskası** | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $11**|00\ 0000** | $255.255.255.192$ |
| **Bitlər** | $/8$ | $/16$ | $/24$ | $/32$ | |

---

### Yayımlama Ünvanı (Broadcast Address)

Əgər IPv4 ünvanının **host hissəsindəki** bütün bitləri **$1$**-ə təyin etsək, **yayımlama ünvanını** alırıq.

| Təfərrüatlar | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | Onluq |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IPv4** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $10**|11\ 1111** | $192.168.12.191/26$ |
| **Altşəbəkə maskası** | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $11**|00\ 0000** | $255.255.255.192$ |
| **Bitlər** | $/8$ | $/16$ | $/24$ | $/32$ | |

İndi bildiyimizə görə, **$192.168.12.128$** və **$192.168.12.191$** IPv4 ünvanları təyin edilib, bütün digər IPv4 ünvanları müvafiq olaraq **$192.168.12.129 - 190$** arasındadır. İndi bilirik ki, bu altşəbəkə bizə cəmi **$64 - 2$** (şəbəkə ünvanı və yayımlama ünvanı) və ya **$62$** IPv4 ünvanı təklif edir ki, bunları hostlarımıza təyin edə bilərik.

| Hostlar | IPv4 |
| :--- | :--- |
| **Şəbəkə Ünvanı** | $192.168.12.128$ |
| **Birinci Host** | $192.168.12.129$ |
| **Digər Hostlar** | $\dots$ |
| **Sonuncu Host** | $192.168.12.190$ |
| **Yayımlama Ünvanı** | $192.168.12.191$ |

---

## Altşəbəkələrin Kiçik Şəbəkələrə Bölünməsi

İndi fərz edək ki, administratorlar olaraq, bizə təyin olunmuş altşəbəkəni **$4$ əlavə altşəbəkəyə** bölmək tapşırığı verilib. Beləliklə, altşəbəkələri yalnız **ikilik sistemə** əsaslanaraq bölə biləcəyimizi bilmək vacibdir.

| Üst (Exponent) | Dəyər |
| :--- | :--- |
| $2^0$ | $1$ |
| $2^1$ | $2$ |
| $2^2$ | $\mathbf{4}$ |
| $2^3$ | $8$ |
| $2^4$ | $16$ |
| $2^5$ | $32$ |
| $2^6$ | $64$ |
| $2^7$ | $128$ |
| $2^8$ | $256$ |

Buna görə də, bildiyimiz **$64$ hostu $4$-ə bölə** bilərik. **$4$** ikilik sistemdə $2^2$ üstünə bərabərdir, beləliklə biz altşəbəkə maskası üçün onu uzatmalı olduğumuz bitlərin sayını tapırıq. Beləliklə, aşağıdakı parametrləri bilirik:

* **Altşəbəkə:** $192.168.12.128/26$
* **Tələb olunan Altşəbəkələr:** $4$

İndi altşəbəkə maskamızı **$2$ bit** artırırıq/genişləndiririk, **$/26$-dan $/28$-ə** və belə görünür:

| Təfərrüatlar | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | Onluq |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **IPv4** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $1000|0000$ | $192.168.12.128/28$ |
| **Altşəbəkə maskası** | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $1111|0000$ | $255.255.255.240$ |
| **Bitlər** | $/8$ | $/16$ | $/24$ | $/32$ | |

Sonra, bizə mövcud olan **$64$ IPv4 ünvanını $4$ hissəyə** bölə bilərik:

| Hostlar | Riyaziyyat | Altşəbəkələr | Hər altşəbəkə üçün Host diapazonu |
| :--- | :--- | :--- | :--- |
| $64$ | $64 / 4$ | $= \mathbf{16}$ |

Beləliklə, hər bir altşəbəkənin nə qədər böyük olacağını bilirik. İndi bizə verilən şəbəkə ünvanından ($192.168.12.128$) başlayırıq və **$16$ hostu $4$ dəfə** əlavə edirik:

| Altşəbəkə № | Şəbəkə Ünvanı | Birinci Host | Sonuncu Host | Yayımlama Ünvanı | CIDR |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **1** | $192.168.12.128$ | $192.168.12.129$ | $192.168.12.142$ | $192.168.12.143$ | $192.168.12.128/28$ |
| **2** | $192.168.12.144$ | $192.168.12.145$ | $192.168.12.158$ | $192.168.12.159$ | $192.168.12.144/28$ |
| **3** | $192.168.12.160$ | $192.168.12.161$ | $192.168.12.174$ | $192.168.12.175$ | $192.168.12.160/28$ |
| **4** | $192.168.12.176$ | $192.168.12.177$ | $192.168.12.190$ | $192.168.12.191$ | $192.168.12.176/28$ |

---

## Zehni Subnetləmə (Mental Subnetting)

Subnetləmədə çoxlu riyaziyyat varmış kimi görünə bilər, lakin hər bir oktet özünü təkrarlayır və hər şey ikinin qüvvətidir, buna görə də çox şey yadda saxlamağa ehtiyac yoxdur. Ediləcək ilk şey, hansı oktetin dəyişdiyini müəyyən etməkdir.

| 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet |
| :--- | :--- | :--- | :--- |
| $/8$ | $/16$ | $/24$ | $/32$ |

Bu dörd rəqəmi yadda saxlamaqla IP ünvanının hansı oktetinin dəyişə biləcəyini müəyyən etmək mümkündür. **Şəbəkə Ünvanı** $192.168.1.1/25$ verildikdə, $192.168.2.4$-ün eyni şəbəkədə olmayacağı dərhal aydındır, çünki **$/25$** altşəbəkəsi yalnız dördüncü oktetin dəyişə biləcəyi deməkdir.

Növbəti hissə, hər bir altşəbəkənin nə qədər böyük ola biləcəyini şəbəkəni **səkkizə bölərək** və **qalıq**-a baxaraq müəyyən edir. Bu, həm də **Modulo Əməliyyatı (%)** adlanır və kriptologiyada çox istifadə olunur. Əvvəlki nümunəmiz **$/25$**-i nəzərə alsaq, **$(25 \% 8)$** $1$ olardı. Bunun səbəbi, səkkizin $25$-ə üç dəfə yerləşməsidir ($8 \times 3 = 24$). $1$ artıq qalır ki, bu da şəbəkə maskası üçün ayrılmış şəbəkə bitidir. Bir IP ünvanının hər oktetində cəmi səkkiz bit var. Əgər biri şəbəkə maskası üçün istifadə edilirsə, tənlik $2^{(8-1)}$ və ya $2^7$, yəni $128$ olur. Aşağıdakı cədvəldə bütün rəqəmlər var.

| Qalıq | Rəqəm | Üstlü Forma | Bölmə Forması |
| :--- | :--- | :--- | :--- |
| $0$ | $256$ | $2^8$ | $256$ |
| $1$ | $128$ | $2^7$ | $256/2$ |
| $2$ | $64$ | $2^6$ | $256/2/2$ |
| $3$ | $32$ | $2^5$ | $256/2/2/2$ |
| $4$ | $16$ | $2^4$ | $256/2/2/2/2$ |
| $5$ | $8$ | $2^3$ | $256/2/2/2/2/2$ |
| $6$ | $4$ | $2^2$ | $256/2/2/2/2/2/2$ |
| $7$ | $2$ | $2^1$ | $256/2/2/2/2/2/2/2$ |

İkinin səkkizə qədər olan qüvvətlərini yadda saxlamaqla, bu ani bir hesablama halına gələ bilər. Ancaq unudularsa, $256$-nı qalıq sayının yarısı qədər bölməyi yadda saxlamaq daha sürətli ola bilər.

Bunun çətin hissəsi, şəbəkədə $0$-ın bir rəqəm olması və boş olmaması səbəbindən faktiki IP Ünvan diapazonunu əldə etməkdir. Beləliklə, **$128$ IP ünvanına malik $/25$**-imizdə, birinci diapazon **$192.168.1.0-127$**-dir. Birinci ünvan şəbəkə, sonuncu isə yayımlama ünvanıdır ki, bu da istifadə edilə bilən IP Sahəsinin **$192.168.1.1-126$** olacağı deməkdir. Əgər IP Ünvanımız $128$-dən yuxarı düşsəydi, onda **istifadə edilə bilən IP sahəsi** $192.168.1.129-254$ olardı ($128$ şəbəkə və $255$ yayımlamadır).

