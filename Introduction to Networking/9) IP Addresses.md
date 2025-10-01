## IP Ünvanları

Şəbəkədə yerləşən hər bir host **Media Access Control** ünvanı (**MAC**) adlanan ünvanla müəyyən edilə bilər. Bu, həmin şəbəkə daxilində məlumat mübadiləsinə imkan verərdi. Əgər uzaq host başqa bir şəbəkədə yerləşirsə, **MAC** ünvanı haqqında məlumat əlaqə qurmaq üçün kifayət deyil. İnternetdə ünvanlama **IPv4** və/və ya **IPv6** ünvanı vasitəsilə həyata keçirilir, bu da **şəbəkə ünvanı** və **host ünvanından** ibarətdir.

Kiçik bir şəbəkə, məsələn, ev kompüter şəbəkəsi, və ya bütün İnternet olmasının fərqi yoxdur. **IP ünvanı** məlumatların düzgün qəbulediciyə çatdırılmasını təmin edir. **MAC** və **IPv4 / IPv6** ünvanlarının təsvirini belə təsəvvür edə bilərik:

* **IPv4 / IPv6** - qəbuledicinin binasının unikal **poçt ünvanını və rayonunu** təsvir edir.
* **MAC** - qəbuledicinin **dəqiq mərtəbəsini və mənzilini** təsvir edir.

Tək bir IP ünvanının birdən çox qəbulediciyə müraciət etməsi (**broadcasting**) və ya bir cihazın birdən çox IP ünvanına cavab verməsi mümkündür. Lakin, hər bir IP ünvanının şəbəkə daxilində yalnız **bir dəfə** təyin olunması təmin edilməlidir.

---

## IPv4 Strukturu

IP ünvanlarının təyin edilməsinin ən ümumi metodu, **32-bitlik ikilik ədəddən** ibarət olan **IPv4**-dür, bu ədəd **0-255** diapazonunda olan **8-bitlik qruplardan (oktetlərdən)** ibarət **4 baytda** birləşdirilmişdir. Bunlar nöqtələrlə ayrılmış və **nöqtəli-onluq göstəriş** (*dotted-decimal notation*) kimi təqdim olunan daha asan oxunan onluq ədədlərə çevrilir.

Beləliklə, bir IPv4 ünvanı bu şəkildə görünə bilər:

| Göstəriş | Təqdimat |
| :--- | :--- |
| **İkilik** (*Binary*) | $0111\ 1111.0000\ 0000.0000\ 0000.0000\ 0001$ |
| **Onluq** (*Decimal*) | $127.0.0.1$ |

Hər bir şəbəkə interfeysinə (şəbəkə kartları, şəbəkə printerləri və ya routerlər) unikal bir IP ünvanı təyin edilir.

**IPv4** formatı $4,294,967,296$ unikal ünvana imkan verir. IP ünvanı **host hissəsi** və **şəbəkə hissəsinə** bölünür. **Host hissəsi** evdə **router** və ya bir administrator tərəfindən təyin edilir. Müvafiq **şəbəkə administratoru** isə **şəbəkə hissəsini** təyin edir. İnternetdə bu, unikal IP-ləri ayıran və idarə edən **IANA**-dır.

Əvvəllər burada əlavə təsnifat aparılırdı. IP şəbəkə blokları **A - E siniflərinə** bölündü. Fərqli siniflər host və şəbəkə paylarının müvafiq uzunluqlarına görə fərqlənirdi.

| Sinif | Şəbəkə Ünvanı | Birinci Ünvan | Sonuncu Ünvan | Altşəbəkə Maskası | CIDR | Altşəbəkələr | IP-lər |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| A | $1.0.0.0$ | $1.0.0.1$ | $127.255.255.255$ | $255.0.0.0$ | $/8$ | $127$ | $16,777,214 + 2$ |
| B | $128.0.0.0$ | $128.0.0.1$ | $191.255.255.255$ | $255.255.0.0$ | $/16$ | $16,384$ | $65,534 + 2$ |
| C | $192.0.0.0$ | $192.0.0.1$ | $223.255.255.255$ | $255.255.255.0$ | $/24$ | $2,097,152$ | $254 + 2$ |
| D | $224.0.0.0$ | $224.0.0.1$ | $239.255.255.255$ | Multicast | Multicast | Multicast | Multicast |
| E | $240.0.0.0$ | $240.0.0.1$ | $255.255.255.255$ | Qorunmuş (*reserved*) | Qorunmuş | Qorunmuş | Qorunmuş |

---

## Altşəbəkə Maskası (Subnet Mask)

Bu siniflərin kiçik şəbəkələrə daha da ayrılması **altşəbəkələşmə** (*subnetting*) köməyi ilə aparılır. Bu ayrılma, bir IPv4 ünvanı qədər uzun olan **şəbəkə maskaları** (*netmasks*) vasitəsilə həyata keçirilir. Siniflərdə olduğu kimi, bu da IP ünvanı daxilində hansı bit mövqelərinin **şəbəkə hissəsi** və ya **host hissəsi** kimi çıxış etdiyini təsvir edir.

| Sinif | Şəbəkə Ünvanı | Birinci Ünvan | Sonuncu Ünvan | Altşəbəkə Maskası | CIDR | Altşəbəkələr | IP-lər |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| A | $1.0.0.0$ | $1.0.0.1$ | $127.255.255.255$ | $255.0.0.0$ | $/8$ | $127$ | $16,777,214 + 2$ |
| B | $128.0.0.0$ | $128.0.0.1$ | $191.255.255.255$ | $255.255.0.0$ | $/16$ | $16,384$ | $65,534 + 2$ |
| C | $192.0.0.0$ | $192.0.0.1$ | $223.255.255.255$ | $255.255.255.0$ | $/24$ | $2,097,152$ | $254 + 2$ |
| D | $224.0.0.0$ | $224.0.0.1$ | $239.255.255.255$ | Multicast | Multicast | Multicast | Multicast |
| E | $240.0.0.0$ | $240.0.0.1$ | $255.255.255.255$ | Qorunmuş | Qorunmuş | Qorunmuş | Qorunmuş |

---

## Şəbəkə və Şlüz Ünvanları (Network and Gateway Addresses)

**IP-lər sütununda** əlavə olunan **iki** əlavə **IP** sözügedən **şəbəkə ünvanı** və **yayımlama (broadcast) ünvanı** üçün qorunub. Başqa bir vacib rol **defolt şlüzə** (*default gateway*) aiddir ki, bu da şəbəkələri və sistemləri fərqli protokollarla əlaqələndirən və ünvanları və ötürmə metodlarını idarə edən **routerin** IPv4 ünvanının adıdır. **Defolt şlüzə** altşəbəkədəki **ilk və ya son təyin edilə bilən IPv4 ünvanının** təyin edilməsi adi haldır. Bu, texniki bir tələb deyil, lakin bütün ölçülərdəki şəbəkə mühitlərində **de-fakto standart** halına gəlmişdir.

| Sinif | Şəbəkə Ünvanı | Birinci Ünvan | Sonuncu Ünvan | Altşəbəkə Maskası | CIDR | Altşəbəkələr | IP-lər |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| A | $1.0.0.0$ | $1.0.0.1$ | $126.255.255.255$ | $255.0.0.0$ | $/8$ | $126$ | $16,777,214 + 2$ |
| B | $128.0.0.0$ | $128.0.0.1$ | $191.255.255.255$ | $255.255.0.0$ | $/16$ | $16,384$ | $65,534 + 2$ |
| C | $192.0.0.0$ | $192.0.0.1$ | $223.255.255.255$ | $255.255.255.0$ | $/24$ | $2,097,152$ | $254 + 2$ |
| D | $224.0.0.0$ | $224.0.0.1$ | $239.255.255.255$ | Multicast | Multicast | Multicast | Multicast |
| E | $240.0.0.0$ | $240.0.0.1$ | $255.255.255.255$ | Qorunmuş | Qorunmuş | Qorunmuş | Qorunmuş |

---

## Yayımlama Ünvanı (Broadcast Address)

**Yayımlama (broadcast) IP ünvanının** vəzifəsi bir şəbəkədəki bütün cihazları bir-biri ilə əlaqələndirməkdir. Şəbəkədə **yayımlama**, şəbəkənin bütün iştirakçılarına ötürülən və cavab tələb etməyən bir mesajdır. Bu yolla, bir host bir məlumat paketini eyni zamanda şəbəkənin bütün digər iştirakçılarına göndərir və bunu edərkən öz **IP ünvanını** bildirir ki, qəbuledicilər onunla əlaqə qurmaq üçün bu ünvandan istifadə edə bilsinlər. Bu, **yayımlama** üçün istifadə olunan **sonuncu IPv4 ünvanıdır**.

| Sinif | Şəbəkə Ünvanı | Birinci Ünvan | Sonuncu Ünvan | Altşəbəkə Maskası | CIDR | Altşəbəkələr | IP-lər |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| A | $1.0.0.0$ | $1.0.0.1$ | $127.255.255.255$ | $255.0.0.0$ | $/8$ | $127$ | $16,777,214 + 2$ |
| B | $128.0.0.0$ | $128.0.0.1$ | $191.255.255.255$ | $255.255.0.0$ | $/16$ | $16,384$ | $65,534 + 2$ |
| C | $192.0.0.0$ | $192.0.0.1$ | $223.255.255.255$ | $255.255.255.0$ | $/24$ | $2,097,152$ | $254 + 2$ |
| D | $224.0.0.0$ | $224.0.0.1$ | $239.255.255.255$ | Multicast | Multicast | Multicast | Multicast |
| E | $240.0.0.0$ | $240.0.0.1$ | $255.255.255.255$ | Qorunmuş | Qorunmuş | Qorunmuş | Qorunmuş |

---

## İkilik Sistem (Binary System)

**İkilik sistem** (*binary system*) onluq sistemin ($0$ - $9$) əksinə olaraq, yalnız iki fərqli vəziyyətdən istifadə edən və iki rəqəmlə ($0$ və $1$) təmsil olunan bir ədədlər sistemidir.

Gördüyümüz kimi, bir IPv4 ünvanı $4$ oktetə bölünür. Hər bir **oktet** **8 bitdən** ibarətdir. Oktetdəki bir bitin hər bir mövqeyi xüsusi bir onluq dəyərə malikdir. Aşağıdakı IPv4 ünvanını nümunə olaraq götürək:

**IPv4 Ünvanı:** $192.168.10.39$

Budur **birinci oktetin** necə göründüyünə dair bir nümunə:

| 1-ci Oktet - Dəyər: 192 |
| :---: |
| Dəyərlər: $128$ $64$ $32$ $16$ $8$ $4$ $2$ $1$ |
| İkilik: $1$ $1$ $0$ $0$ $0$ $0$ $0$ $0$ |

Əgər bitin $1$ olaraq təyin olunduğu hər bir oktet üçün bütün bu dəyərlərin cəmini hesablasaq, cəm aşağıdakı kimi alınır:

| Oktet | Dəyərlər | Cəm |
| :--- | :--- | :--- |
| **1-ci** | $128 + 64 + 0 + 0 + 0 + 0 + 0 + 0$ | $= \mathbf{192}$ |
| **2-ci** | $128 + 0 + 32 + 0 + 8 + 0 + 0 + 0$ | $= \mathbf{168}$ |
| **3-cü** | $0 + 0 + 0 + 0 + 8 + 0 + 2 + 0$ | $= \mathbf{10}$ |
| **4-cü** | $0 + 0 + 32 + 0 + 0 + 4 + 2 + 1$ | $= \mathbf{39}$ |

İkilikdən onluğa tam təmsil aşağıdakı kimi görünəcək:

| IPv4 - İkilik Göstəriş |
| :---: |
| Oktet: **1-ci** **2-ci** **3-cü** **4-cü** |
| İkilik: $1100\ 0000 . 1010\ 1000 . 0000\ 1010 . 0010\ 0111$ |
| Onluq: $192 . 168 . 10 . 39$ |
| **IPv4 Ünvanı:** $192.168.10.39$ |

Bu toplama hər bir oktet üçün baş verir ki, bu da **IPv4 ünvanının** onluq təmsilini verir. Altşəbəkə maskası eyni şəkildə hesablanır.

| IPv4 - Onluqdan İkiliyə |
| :---: |
| Dəyərlər: $128$ $64$ $32$ $16$ $8$ $4$ $2$ $1$ |
| İkilik: $1$ $1$ $1$ $1$ $1$ $1$ $1$ $1$ |

| Oktet | Dəyərlər | Cəm |
| :--- | :--- | :--- |
| **1-ci** | $128 + 64 + 32 + 16 + 8 + 4 + 2 + 1$ | $= \mathbf{255}$ |
| **2-ci** | $128 + 64 + 32 + 16 + 8 + 4 + 2 + 1$ | $= \mathbf{255}$ |
| **3-cü** | $128 + 64 + 32 + 16 + 8 + 4 + 2 + 1$ | $= \mathbf{255}$ |
| **4-cü** | $0 + 0 + 0 + 0 + 0 + 0 + 0 + 0$ | $= \mathbf{0}$ |

| Altşəbəkə Maskası |
| :---: |
| Oktet: **1-ci** **2-ci** **3-cü** **4-cü** |
| İkilik: $1111\ 1111 . 1111\ 1111 . 1111\ 1111 . 0000\ 0000$ |
| Onluq: $255 . 255 . 255 . 0$ |
| **IPv4 Ünvanı:** $192.168.10.39$ |
| **Altşəbəkə maskası:** $255.255.255.0$ |

---

## CIDR

**Classless Inter-Domain Routing (CIDR)**, təqdimat metodudur və IPv4 ünvanı ilə şəbəkə sinifləri (A, B, C, D, E) arasındakı sabit təyinatı əvəz edir. Bölünmə **altşəbəkə maskasına** və ya **CIDR şəkilçisinə** (*CIDR suffix*) əsaslanır ki, bu da IPv4 ünvan sahəsinin bit səviyyəsində bölünməsinə və beləliklə, **istənilən ölçüdə altşəbəkələrə** imkan verir. **CIDR şəkilçisi** IPv4 ünvanının əvvəlindən neçə bitin şəbəkəyə aid olduğunu göstərir. Bu, altşəbəkə maskasındakı **1-bitlərin** sayını göstərməklə **altşəbəkə maskasını** təmsil edən bir göstərişdir.

Nümunə olaraq aşağıdakı IPv4 ünvanı və altşəbəkə maskası üzərində dayanaq:

* **IPv4 Ünvanı:** $192.168.10.39$
* **Altşəbəkə maskası:** $255.255.255.0$

İndi IPv4 ünvanının və altşəbəkə maskasının tam təmsili belə görünəcək:

**CIDR:** $192.168.10.39/24$

Buna görə də, CIDR şəkilçisi altşəbəkə maskasındakı **bütün birlərin cəmidir**.

| IPv4 Ünvanları |
| :---: |
| Oktet: **1-ci** **2-ci** **3-cü** **4-cü** |
| İkilik: $1111\ 1111 . 1111\ 1111 . 1111\ 1111 . 0000\ 0000$ ($/24$) |
| Onluq: $255 . 255 . 255 . 0$ |
