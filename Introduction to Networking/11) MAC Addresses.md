## MAC Ünvanları

Şəbəkədəki hər bir hostun öz **48-bitlik (6 oktet)** **Media Access Control (MAC)** ünvanı var ki, bu da onaltılıq (*hexadecimal*) formatda təmsil olunur. **MAC** bizim şəbəkə interfeyslərimiz üçün **fiziki ünvandır**. MAC ünvanı üçün bir neçə fərqli standart var:

* **Ethernet** (IEEE 802.3)
* **Bluetooth** (IEEE 802.15)
* **WLAN** (IEEE 802.11)

Bunun səbəbi, **MAC** ünvanının bir hostun fiziki əlaqəsinə (**şəbəkə kartı, Bluetooth və ya WLAN adapteri**) müraciət etməsidir. Hər bir şəbəkə kartının öz fərdi MAC ünvanı var ki, bu da istehsalçının aparat tərəfində bir dəfə konfiqurasiya edilir, lakin hər zaman, ən azı müvəqqəti olaraq dəyişdirilə bilər.

Gəlin belə bir MAC ünvanının nümunəsinə baxaq:

| **MAC ünvanı:** | $DE:AD:BE:EF:13:37$ |
| :---: | :---: |
| | $DE-AD-BE-EF-13-37$ |
| | $DEAD.BEEF.1337$ |

| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **İkilik** (*Binary*) | $1101\ 1110$ | $1010\ 1101$ | $1011\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| **Onaltılıq** (*Hex*) | $DE$ | $AD$ | $BE$ | $EF$ | $13$ | $37$ |

Bir IP paketi çatdırılarkən, onun **qat 2**-də təyinat hostunun fiziki ünvanına və ya marşrutlaşdırmaya cavabdeh olan **routerə / NAT**-a ünvanlanması lazımdır. Hər bir paketin bir **göndərici ünvanı** və bir **təyinat ünvanı** var.

MAC ünvanı cəmi **$6$ baytdan** ibarətdir. Birinci yarısı (**3 bayt / 24 bit**) müvafiq istehsalçılar üçün **Institute of Electrical and Electronics Engineers (IEEE)** tərəfindən müəyyən edilmiş sözdə **Organization Unique Identifier (OUI)** adlanır.

| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **İkilik** | $1101\ 1110$ | $1010\ 1101$ | $1011\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| **Onaltılıq** | **DE** | **AD** | **BE** | $EF$ | $13$ | $37$ |

MAC ünvanının son yarısı **Individual Address Part** və ya **Network Interface Controller (NIC)** adlanır ki, bu da istehsalçılar tərəfindən təyin edilir. İstehsalçı bu bit ardıcıllığını yalnız bir dəfə təyin edir və beləliklə, tam ünvanın **unikal** olmasını təmin edir.

| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **İkilik** | $1101\ 1110$ | $1010\ 1101$ | $1011\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| **Onaltılıq** | $DE$ | $AD$ | $BE$ | **EF** | **13** | **37** |

Əgər IP təyinat ünvanına malik bir host eyni altşəbəkədə yerləşirsə, çatdırılma birbaşa hədəf kompüterin fiziki ünvanına edilir. Lakin, bu host fərqli bir altşəbəkəyə aiddirsə, Ethernet freymi məsuliyyət daşıyan **routerin (defolt şlüz)** **MAC ünvanına** ünvanlanır. Əgər Ethernet freymindəki təyinat ünvanı routerin öz **qat 2 ünvanı** ilə uyğun gəlirsə, router freymi daha yüksək qatlara ötürəcək. **Address Resolution Protocol (ARP)** IPv4-də IP ünvanları ilə əlaqəli MAC ünvanlarını müəyyən etmək üçün istifadə olunur.

IPv4 ünvanlarında olduğu kimi, MAC ünvanı üçün də müəyyən qorunmuş sahələr mövcuddur. Bunlara, məsələn, MAC üçün yerli diapazon daxildir.

| Yerli Diapazon (*Local Range*) |
| :---: |
| $02:00:00:00:00:00$ |
| $06:00:00:00:00:00$ |
| $0A:00:00:00:00:00$ |
| $0E:00:00:00:00:00$ |

Bundan əlavə, birinci oktetdəki son iki bit başqa bir vacib rol oynaya bilər. Bildiyimiz kimi, son bit iki vəziyyətə, $0$ və $1$-ə malik ola bilər. Son bit MAC ünvanını **Unicast ($0$)** və ya **Multicast ($1$)** kimi müəyyən edir. **Unicast** ilə, göndərilən paketin yalnız bir xüsusi hosta çatacağı deməkdir.

| MAC Unicast |
| :---: |
| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| İkilik | $1101\ 111\mathbf{0}$ | $1010\ 1101$ | $1011\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| Onaltılıq | $DE$ | $AD$ | $BE$ | $EF$ | $13$ | $37$ |

**Multicast** ilə, paket yalnız yerli şəbəkədəki bütün hostlara bir dəfə göndərilir, bu hostlar isə konfiqurasiyalarına əsasən paketi qəbul edib-etməmək qərarını verirlər. **Multicast** ünvanı, sabit oktet dəyərlərinə malik olan **yayımlama (broadcast)** ünvanı kimi unikal bir ünvandır. Şəbəkədə **yayımlama** bir nöqtədən şəbəkənin bütün üzvlərinə eyni zamanda ötürülən və yayılmış bir çağırışı təmsil edir. Bu, əsasən paketin qəbuledicisinin ünvanı hələ məlum olmadıqda istifadə olunur. Buna misal olaraq **ARP** (MAC ünvanları üçün) və **DHCP** (IPv4 ünvanları üçün) protokollarını göstərmək olar.

Hər bir oktetin müəyyən edilmiş dəyərləri yaşıl rənglə qeyd edilmişdir.

| MAC Multicast |
| :---: |
| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| İkilik | $0000\ 000\mathbf{1}$ | $0000\ 0000$ | $0101\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| Onaltılıq | **01** | **00** | **5E** | $EF$ | $13$ | $37$ |

| MAC Broadcast |
| :---: |
| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| İkilik | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ | $1111\ 1111$ |
| Onaltılıq | **FF** | **FF** | **FF** | **FF** | **FF** | **FF** |

Birinci oktetdəki sondan ikinci bit, onun **IEEE** tərəfindən müəyyən edilmiş **qlobal OUI** (*Global OUI*) olub-olmadığını və ya **yerli olaraq idarə olunan** (*locally administrated*) MAC ünvanı olub-olmadığını müəyyən edir.

| Qlobal OUI |
| :---: |
| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| İkilik | $1101\ 11\mathbf{0}0$ | $1010\ 1101$ | $1011\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| Onaltılıq | $DC$ | $AD$ | $BE$ | $EF$ | $13$ | $37$ |

| Yerli olaraq İdarə olunan (*Locally Administrated*) |
| :---: |
| Təmsil | 1-ci Oktet | 2-ci Oktet | 3-cü Oktet | 4-cü Oktet | 5-ci Oktet | 6-cı Oktet |
| İkilik | $1101\ 11\mathbf{1}0$ | $1010\ 1101$ | $1011\ 1110$ | $1110\ 1111$ | $0001\ 0011$ | $0011\ 0111$ |
| Onaltılıq | $DE$ | $AD$ | $BE$ | $EF$ | $13$ | $37$ |

---

## MAC Ünvanı Hücum Vektorları

MAC ünvanları **dəyişdirilə/manipulyasiya edilə** və ya **aldadıla (spoofed)** bilər və buna görə də, onlar təhlükəsizlik və ya identifikasiyanın yeganə vasitəsi kimi etibar edilməməlidir. Şəbəkə administratorları potensial hücumlardan qorunmaq üçün şəbəkə seqmentasiyası və güclü autentifikasiya protokolları kimi əlavə təhlükəsizlik tədbirləri tətbiq etməlidirlər.

MAC ünvanları vasitəsilə istismar edilə bilən bir neçə hücum vektoru mövcuddur:

* **MAC aldatması (*spoofing*):** Bu, adətən şəbəkəyə icazəsiz giriş əldə etmək üçün bir cihazın MAC ünvanını başqa bir cihazın ünvanı ilə uyğunlaşdırmaq üçün dəyişdirilməsini əhatə edir.
* **MAC daşqınları (*flooding*):** Bu, bir şəbəkə kommutatoruna (*switch*) fərqli MAC ünvanları ilə çoxlu paket göndərilməsini əhatə edir, bu da onun MAC ünvanı cədvəlinin tutumuna çatmasına səbəb olur və faktiki olaraq düzgün işləməsinin qarşısını alır.
* **MAC ünvanı filtrlənməsi:** Bəzi şəbəkələr yalnız xüsusi MAC ünvanlarına malik cihazlara girişə icazə vermək üçün konfiqurasiya edilə bilər ki, biz də aldadılmış MAC ünvanından istifadə edərək şəbəkəyə giriş əldə etməyə cəhd edərək bunu istismar edə bilərik.

---

## Address Resolution Protocol (ARP)

**Address Resolution Protocol (ARP)** bir şəbəkə protokoludur. O, **şəbəkə qatının (qat 3) IP ünvanını** **keçid qatının (qat 2) MAC ünvanına** çevirmək üçün istifadə olunan şəbəkə rabitəsinin vacib bir hissəsidir. O, **Yerli Şəbəkə (LAN)** üzərində cihazlar arasında əlaqəni asanlaşdırmaq üçün bir hostun IP ünvanını onun müvafiq MAC ünvanı ilə əlaqələndirir. Bir LAN-da bir cihaz başqa bir cihazla əlaqə qurmaq istədikdə, təyinat IP ünvanını və öz MAC ünvanını ehtiva edən **yayımlama (broadcast) mesajı** göndərir. Uyğun IP ünvanına malik cihaz öz MAC ünvanı ilə cavab verir və iki cihaz daha sonra MAC ünvanlarından istifadə edərək birbaşa əlaqə qura bilər. Bu proses **ARP həlli** kimi tanınır.

ARP şəbəkə rabitə prosesinin vacib bir hissəsidir, çünki cihazlara IP ünvanları əvəzinə MAC ünvanlarından istifadə edərək məlumat göndərməyə və qəbul etməyə imkan verir ki, bu da daha səmərəli ola bilər. İstifadə edilə bilən iki növ sorğu mesajı var:

* **ARP Sorğusu (*Request*)**
    Bir cihaz bir LAN-da başqa bir cihazla əlaqə qurmaq istədikdə, təyinat cihazının IP ünvanını MAC ünvanına çevirmək üçün bir **ARP sorğusu** göndərir. Sorğu LAN-dakı bütün cihazlara **yayımlanır** və təyinat cihazının IP ünvanını ehtiva edir. Uyğun IP ünvanına malik cihaz öz MAC ünvanı ilə cavab verir.
* **ARP Cavabı (*Reply*)**
    Bir cihaz bir ARP sorğusu aldıqda, öz MAC ünvanı ilə sorğu göndərən cihaza bir **ARP cavabı** göndərir. Cavab mesajı həm sorğu göndərən, həm də cavab verən cihazların IP və MAC ünvanlarını ehtiva edir.

### ARP Sorğularının Tshark-da Çəkilişi

| MAC Ünvanları |
| :--- |
| $1 \quad 0.000000 \quad 10.129.12.100 \rightarrow 10.129.12.255 \quad \text{ARP } 60 \quad \text{Who has } 10.129.12.101? \quad \text{Tell } 10.129.12.100$ |
| $2 \quad 0.000015 \quad 10.129.12.101 \rightarrow 10.129.12.100 \quad \text{ARP } 60 \quad 10.129.12.101 \text{ is at } AA:AA:AA:AA:AA:AA$ |
| $3 \quad 0.000030 \quad 10.129.12.102 \rightarrow 10.129.12.255 \quad \text{ARP } 60 \quad \text{Who has } 10.129.12.103? \quad \text{Tell } 10.129.12.102$ |
| $4 \quad 0.000045 \quad 10.129.12.103 \rightarrow 10.129.12.102 \quad \text{ARP } 60 \quad 10.129.12.103 \text{ is at } BB:BB:BB:BB:BB:BB$ |

Birinci və üçüncü sətirlərdəki "Who has" mesajı, bir cihazın göstərilən IP ünvanı üçün MAC ünvanını tələb etdiyini göstərir, ikinci və dördüncü sətirlər isə təyinat cihazının MAC ünvanı ilə **ARP cavabını** göstərir.

Lakin, o, həm də şəbəkədəki trafiki ələ keçirmək və ya manipulyasiya etmək üçün istifadə edilə bilən **ARP Aldatması (*Spoofing*)** kimi hücumlara qarşı həssasdır. Lakin, belə hücumlardan qorunmaq üçün firewallar və müdaxilə aşkarlama sistemləri kimi təhlükəsizlik tədbirlərini tətbiq etmək vacibdir.

**ARP aldatması**, həmçinin **ARP keşinin zəhərlənməsi** (*ARP cache poisoning*) və ya **ARP zəhər marşrutlaşdırması** (*ARP poison routing*) kimi tanınır, **Ettercap** və ya **Cain & Abel** kimi alətlərdən istifadə edərək bir LAN üzərindən **saxta ARP mesajları** göndərdiyimiz bir hücumdur. Məqsəd, leqal cihaz üçün nəzərdə tutulmuş trafiki ələ keçirməyə effektiv şəkildə imkan vermək üçün öz MAC ünvanımızı şirkətin şəbəkəsindəki leqal bir cihazın IP ünvanı ilə əlaqələndirməkdir. Məsələn, bu aşağıdakı kimi görünə bilər:

| MAC Ünvanları |
| :--- |
| $1 \quad 0.000000 \quad 10.129.12.100 \rightarrow 10.129.12.101 \quad \text{ARP } 60 \quad 10.129.12.101 \text{ is at } AA:AA:AA:AA:AA:AA$ |
| $2 \quad 0.000015 \quad 10.129.12.100 \rightarrow 10.129.12.255 \quad \text{ARP } 60 \quad \text{Who has } 10.129.12.101? \quad \text{Tell } 10.129.12.100$ |
| $3 \quad 0.000030 \quad 10.129.12.101 \rightarrow 10.129.12.100 \quad \text{ARP } 60 \quad 10.129.12.101 \text{ is at } BB:BB:BB:BB:BB:BB$ |
| $4 \quad 0.000045 \quad 10.129.12.100 \rightarrow 10.129.12.101 \quad \text{ARP } 60 \quad 10.129.12.101 \text{ is at } AA:AA:AA:AA:AA:AA$ |

Birinci və dördüncü sətirlər **bizim** ($10.129.12.100$) hədəfə **saxta ARP mesajları** göndərərək öz MAC ünvanımızı onun IP ünvanı ($10.129.12.101$) ilə əlaqələndirdiyimizi göstərir. İkinci və üçüncü sətirlər hədəfin bir ARP sorğusu göndərdiyini və MAC ünvanımıza cavab verdiyini göstərir. Bu, hədəfin ARP keşini zəhərlədiyimizi və hədəf üçün nəzərdə tutulmuş bütün trafikin indi **bizim MAC ünvanımıza** göndəriləcəyini göstərir.

Biz **ARP zəhərlənməsindən** həssas məlumatları oğurlamaq, trafiki yönləndirmək və ya **MITM hücumları** (*Man-in-the-Middle*) başlatmaq kimi müxtəlif fəaliyyətləri həyata keçirmək üçün istifadə edə bilərik. Lakin, ARP aldatmasından qorunmaq üçün **IPSec** və ya **SSL** kimi təhlükəsiz şəbəkə protokollarından istifadə etmək və firewallar və müdaxilə aşkarlama sistemləri kimi təhlükəsizlik tədbirlərini tətbiq etmək vacibdir.

