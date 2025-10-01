## IPv6 Ünvanları

**IPv6** IPv4-ün varisidir. IPv4-dən fərqli olaraq, **IPv6** ünvanı **$128$ bit** uzunluğundadır. **Prefiks** host və şəbəkə hissələrini müəyyən edir. **Internet Assigned Numbers Authority (IANA)** IPv4 və IPv6 ünvanlarının və onların əlaqəli şəbəkə hissələrinin təyin edilməsinə cavabdehdir. Uzun müddətdə, **IPv6**-nın hələ də İnternetdə üstünlük təşkil edən IPv4-ü tamamilə əvəz etməsi gözlənilir. Prinsipcə, lakin IPv4 və IPv6 eyni vaxtda istifadəyə verilə bilər (**Dual Stack**).

IPv6 ardıcıl olaraq **ucdan-uca** (*end-to-end*) prinsipini izləyir və **NAT**-a ehtiyac olmadan istənilən son cihazlar üçün ictimai əlçatan IP ünvanları təmin edir. Nəticədə, bir interfeys birdən çox IPv6 ünvanına malik ola bilər və birdən çox interfeysin təyin olunduğu xüsusi IPv6 ünvanları da var.

**IPv6** IPv4 üzərində bir çox yeni xüsusiyyətlərə və bir çox başqa üstünlüklərə malik bir protokoldur:

* Daha böyük ünvan sahəsi
* Ünvanın özünü konfiqurasiyası (**SLAAC**)
* Hər interfeys üçün birdən çox IPv6 ünvanı
* Daha sürətli marşrutlaşdırma
* Ucdan-uca şifrələmə (**IPsec**)
* $4$ GBayta qədər məlumat paketləri

| Xüsusiyyətlər | IPv4 | IPv6 |
| :--- | :--- | :--- |
| **Bit uzunluğu** | $32$-bit | $128$ bit |
| **OSI qatı** | Şəbəkə Qatı | Şəbəkə Qatı |
| **Ünvanlama diapazonu** | $\sim 4.3$ milyard | $\sim 340$ undesillion |
| **Təmsil** | İkilik (*Binary*) | Onaltılıq (*Hexadecimal*) |
| **Prefiks göstərişi** | $10.10.10.0/24$ | $fe80::dd80:b1a9:6687:2d3b/64$ |
| **Dinamik ünvanlama** | DHCP | SLAAC / DHCPv6 |
| **IPsec** | Könüllü (*Optional*) | Məcburi (*Mandatory*) |

IPv6 ünvanlarının üç fərqli növü var:

| Növ | Təsvir |
| :--- | :--- |
| **Unicast** | Tək bir interfeys üçün ünvanlar. |
| **Anycast** | Birdən çox interfeys üçün ünvanlar, lakin onlardan yalnız biri paketi qəbul edir. |
| **Multicast** | Birdən çox interfeys üçün ünvanlar, onlardan hamısı eyni paketi qəbul edir. |

**Qeyd:** IPv4-dən fərqli olaraq, IPv6 **yayımlama (broadcast) ünvanını** aradan qaldırır. Bunun əvəzinə, IPv6 çoxsaylı qovşaqlarla kəşfiyyatı və əlaqəni dəstəkləmək üçün **multicast ünvanlarından** istifadə edir.

---

## Onaltılıq Sistem (Hexadecimal System)

**Onaltılıq sistem (hex)** ikilik təmsili daha oxunaqlı və anlaşılmaz etmək üçün istifadə olunur. Biz onluq sistemlə yalnız **$10$** ($0-9$) və ikilik sistemlə **$2$** ($0 / 1$) vəziyyəti tək bir simvolla göstərə bilərik. İkilik və onluq sistemdən fərqli olaraq, biz onaltılıq sistemdən istifadə edərək bir simvolla **$16$** ($0-F$) vəziyyəti göstərə bilərik.

| Onluq | Onaltılıq | İkilik |
| :--- | :--- | :--- |
| $1$ | $1$ | $0001$ |
| $2$ | $2$ | $0010$ |
| $3$ | $3$ | $0011$ |
| $4$ | $4$ | $0100$ |
| $5$ | $5$ | $0101$ |
| $6$ | $6$ | $0110$ |
| $7$ | $7$ | $0111$ |
| $8$ | $8$ | $1000$ |
| $9$ | $9$ | $1001$ |
| $10$ | $A$ | $1010$ |
| $11$ | $B$ | $1011$ |
| $12$ | $C$ | $1100$ |
| $13$ | $D$ | $1101$ |
| $14$ | $E$ | $1110$ |
| $15$ | $F$ | $1111$ |

Gəlin bir IPv4 nümunəsinə baxaq, IPv4 ünvanının ($192.168.12.160$) onaltılıq təmsilinin necə görünəcəyinə.

| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet |
| :--- | :--- | :--- | :--- | :--- |
| **İkilik** | $1100\ 0000$ | $1010\ 1000$ | $0000\ 1100$ | $1010\ 0000$ |
| **Onaltılıq** | $C0$ | $A8$ | $0C$ | $A0$ |
| **Onluq** | $192$ | $168$ | $12$ | $160$ |

Ümumilikdə, IPv6 ünvanı **$16$ baytdan** ibarətdir. Uzunluğuna görə, bir **IPv6** ünvanı **onaltılıq** göstərişdə təmsil olunur. Buna görə də **$128$ bit $8$ bloka** vurulmuş $16$ bitə (və ya **$4$ hex** rəqəminə) bölünür. Bütün dörd hex rəqəmi qruplaşdırılır və IPv4-dəki sadə nöqtə (.) əvəzinə iki nöqtə (:) ilə ayrılır. Göstərişi sadələşdirmək üçün, biz bloklarda **qabaqcıl ən azı $4$ sıfırı** kənarda qoyuruq və onları **iki qoşa nöqtə (::)** ilə əvəz edə bilərik.

Bir IPv6 ünvanı belə görünə bilər:

* **Tam IPv6:** $fe80:0000:0000:0000:dd80:b1a9:6687:2d3b/64$
* **Qısa IPv6:** $fe80::dd80:b1a9:6687:2d3b/64$

Bir IPv6 ünvanı iki hissədən ibarətdir:

* **Şəbəkə Prefiksi** (*Network Prefix*) (şəbəkə hissəsi)
* **İnterfeys İdentifikatoru** (*Interface Identifier*) həmçinin **Şəkilçi** (*Suffix*) adlanır (host hissəsi)

**Şəbəkə Prefiksi** şəbəkəni, altşəbəkəni və ya ünvan diapazonunu müəyyən edir. **İnterfeys İdentifikatoru** interfeysin **$48$-bitlik MAC** ünvanından (daha sonra müzakirə edəcəyik) formalaşır və prosesdə **$64$-bitlik ünvana** çevrilir. Defolt prefiks uzunluğu **$/64$**-dür. Lakin, digər tipik prefikslər **$/32, /48$** və **$/56$**-dır. Əgər şəbəkələrimizdən istifadə etmək istəyiriksə, provayderimizdən **$/64$-dən** daha qısa bir prefiks (məsələn, **$/56$**) alırıq.

**RFC 5952**-də yuxarıda qeyd olunan IPv6 ünvanı göstərişi müəyyən edilmişdir:

* Bütün əlifba simvolları həmişə **kiçik hərflərlə** yazılır.
* Bir blokun bütün qabaqcıl sıfırları həmişə **kənarda qoyulur**.
* **$4$ sıfırdan (hex)** ibarət bir və ya daha çox ardıcıl blok **iki qoşa nöqtə (::)** ilə qısaldılır.
* İki qoşa nöqtəyə (::) qısaltma soldan başlayaraq yalnız **bir dəfə** yerinə yetirilə bilər.
