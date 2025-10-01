## Vendor Spesifik Məlumat

**Cisco IOS** **Cisco** şəbəkə cihazlarının (routerlər və kommutatorlar kimi) əməliyyat sistemidir. O, şəbəkə cihazlarını idarə etmək və işlətmək üçün tələb olunan xüsusiyyətlər və xidmətlər təqdim edir. Bu əməliyyat sistemi xüsusiyyətlərinə, dəstəyinə və performansına görə dəyişən fərqli versiya və buraxılışlarda mövcuddur. O, müasir şəbəkələrin işləməsi üçün tələb olunan bir neçə xüsusiyyət təklif edir, məsələn, lakin bunlarla məhdudlaşmayaraq:

  * **IPv6 dəstəyi**
  * **Xidmət Keyfiyyəti (QoS)**
  * **Şifrələmə və autentifikasiya** kimi təhlükəsizlik xüsusiyyətləri
  * **Virtual Özəl LAN Xidməti (VPLS)**
  * **Virtual Marşrutlaşdırma və Yönləndirmə (VRF)** kimi virtualizasiya xüsusiyyətləri

**Cisco IOS** istifadə olunan şəbəkə cihazı və avadanlığından asılı olaraq bir neçə yolla idarə edilə bilər. Ən çox istifadə olunan metod **əmrlər sətri interfeysidir (CLI)** ki, bu da **qrafik istifadəçi interfeysində (GUI)** idarə oluna bilər. Bundan əlavə, o, şəbəkə əməliyyatları üçün tələb olunan müxtəlif şəbəkə protokollarını və xidmətlərini dəstəkləyir. Bunlara daxildir:

| Protokol Növü | Təsvir |
| :--- | :--- |
| **Marşrutlaşdırma protokolları** | Şəbəkədə məlumat paketlərini marşrutlaşdırmaq üçün istifadə olunan **OSPF** və **BGP** kimi protokollar. |
| **Kommutasiya protokolları** | Şəbəkədə kommutatorları konfiqurasiya etmək və idarə etmək üçün istifadə olunan **VLAN Trunking Protocol (VTP)** və **Spanning Tree Protocol (STP)** kimi protokollar. |
| **Şəbəkə xidmətləri** | Şəbəkədəki klientləri avtomatik olaraq **IP ünvanları** və digər şəbəkə konfiqurasiyaları ilə təmin etmək üçün istifadə olunan **Dynamic Host Configuration Protocol (DHCP)** kimi xidmətlər. |
| **Təhlükəsizlik xüsusiyyətləri** | Şəbəkə resurslarına girişi idarə etmək və təhlükəsizlik təhdidlərinin qarşısını almaq üçün istifadə olunan **Girişə Nəzarət Siyahıları (ACLs)** kimi xüsusiyyətlər. |

**Cisco IOS**-da müxtəlif məqsədlər üçün fərqli növ şifrələr istifadə olunur, məsələn:

| Şifrə Növü | Təsvir |
| :--- | :--- |
| **User** | **User** şifrəsi **Cisco IOS**-a daxil olmaq üçün istifadə olunur. O, şəbəkə cihazına və onun xüsusiyyətlərinə girişi məhdudlaşdırmaq üçün istifadə olunur. |
| **Enable Password** | **Enable Password** "enable" rejiminə daxil olmaq üçün istifadə olunur. "Enable" rejimi **qabaqcıl funksiyalara və parametrlərə** girişinizin olduğu rejimdir. |
| **Secret** | **Secret** müəyyən funksiyalara və xidmətlərə girişi qorumaq üçün bir şifrədir. O, tez-tez uzaqdan idarəetmə alətlərinə və xidmətlərinə girişi məhdudlaşdırmaq üçün istifadə olunur. |
| **Enable Secret** | **Enable Secret** "enable" rejiminə girişi qorumaq üçün istifadə olunan **əlavə təhlükəsiz şifrədir** və **əlavə qorunma** təmin etmək üçün **şifrələnmiş şəkildə saxlanılır**. |

**Cisco IOS** cihazları **SSH** və ya **Telnet** üçün konfiqurasiya edilə bilər. Beləliklə, ona uzaqdan daxil olmaq mümkündür. Aldığımız cavabdan, onun həqiqətən də bir **Cisco IOS** olduğunu müəyyən edə bilərik, çünki **"User Access Verification"** mesajı ilə cavab verir.

```
nijatmansimov@htb[/htb]$ telnet 10.129.10.2
Trying 10.129.10.2...
Connected to 10.129.10.2.
Escape character is '^]'.


User Access Verification

Password:
```

-----

## VLANs (Virtual Yerli Şəbəkələr)

Təsəvvür edin: **XQ** adlı bir *startup* bir ofisli şirkətləri üçün şəbəkə yaratmaq üçün bir şəbəkə administratoru işə götürdü və büdcə məhdudiyyətləri səbəbindən yalnız bir **kommutator** və bir **router** ala bilirlər. **XQ**-nun sistem administratoru, şəbəkədə **veb və verilənlər bazası serverlərini** yerləşdirməklə yanaşı, fərqli departamentlərdən olan işçilərin də ondan istifadə edəcəyini bildirdi. Təcrübəli bir şəbəkə təhlükəsizliyi mütəxəssisi olaraq, şəbəkə administratoru dərhal bir *insider*-in həyata keçirə biləcəyi təhlükəsizlik hücumları, xüsusən də **yayımlanma trafikindən sui-istifadə edən (məsələn, *broadcast storms*)** hücumlar haqqında düşündü. Buna görə də, bu problemi həll etmək üçün şəbəkə administratoru şəbəkəni **Virtual Yerli Şəbəkələrlə (VLANs)** məntiqi olaraq seqmentləşdirməyə, bir kommutatoru **konseptual olaraq daha kiçik *mini-kommutatorlara*** ayırmağa qərar verdi.

Bir **VLAN** birdən çox **fiziki LAN seqmentini** əhatə edə bilən məntiqi yayımlanma domenləri yaratmaqla şəbəkələrin seqmentləşdirilməsinə imkan verən, bir kommutator üzərində müəyyən edilmiş portlara qoşulmuş **şəbəkə son nöqtələrinin məntiqi qruplaşdırılmasıdır**. **VLAN**-larla, şəbəkə administratorları son nöqtələrin və istifadəçilərin fiziki yerindən narahat olmadan **komanda, funksiya, departament və ya tətbiq** kimi amillərə əsaslanaraq şəbəkələri seqmentləşdirə bilərlər. Bir **VLAN** üzərindən göndərilən yayımlanma paketi başqa bir **VLAN**-ın üzvü olan hər hansı digər son nöqtəyə çatmır. Hər bir **VLAN** bir yayımlanma domeni hesab edildiyi üçün, onun **öz altşəbəkəsi** olmalıdır; məsələn, XQ tərəfindən müqavilə bağlanmış şəbəkə administratoru şəbəkəni departamentlərə görə seqmentləşdirə bilər:

| Departament | VLAN ID | Altşəbəkə |
| :--- | :--- | :--- |
| **Servers** | VLAN 10 | $192.168.1.0/24$ |
| **C-Level** | VLAN 20 | $192.168.2.0/24$ |
| **Finance** | VLAN 30 | $192.168.3.0/24$ |
| **HR** | VLAN 40 | $192.168.4.0/24$ |
| **Marketing** | VLAN 50 | $192.168.5.0/24$ |
| **Support** | VLAN 60 | $192.168.6.0/24$ |

**VLAN**-lardan istifadə edərkən çoxsaylı faydalar əldə edilir, o cümlədən:

  * **Daha Yaxşı Təşkilat:** Şəbəkə administratorları son nöqtələri bölüşdükləri hər hansı bir ümumi xüsusiyyətə əsaslanaraq qruplaşdıra bilərlər.
  * **Artan Təhlükəsizlik:** Şəbəkə seqmentasiyası icazəsiz üzvlərin digər **VLAN**-larda şəbəkə paketlərini *sniff* etməsinə imkan vermir.
  * **Sadələşdirilmiş İdarəetmə:** Şəbəkə administratorları bir son nöqtənin fiziki yerləri barədə narahat olmaq məcburiyyətində deyillər.
  * **Artan Performans:** Bütün son nöqtələr üçün azaldılmış yayımlanma traffiki ilə, şəbəkə tərəfindən istifadə üçün daha çox bant genişliyi mövcud olur.

**Cisco** kommutatorları **VLAN ID/nömrələrini 1-4094** təmin edir (**0** və **4095** ayrılmış ID-lərdir və istifadə edilə bilməz); **ID-lər 1-1005** (**VLAN 1** **defolt VLAN** kimi tanınır və dəyişdirilməməli/silinməməlidir) **normal-range VLAN**-lar kimi tanınır, **ID-lər 1002-1005** **Token Ring** və **Fiber Distributed Data Interface (FDDI) VLAN**-ları üçün ayrılmışdır, halbuki **ID-lər 1006-4094** **extended-range VLAN**-lar kimi tanınır. Defolt olaraq, **normal-range VLAN**-lar üçün tətbiq olunan hər hansı bir fərdiləşdirmə **VLAN** verilənlər bazasında (**vlan.dat** faylı) saxlanılır, **extended-range VLAN**-lardan fərqli olaraq, onların fərdiləşdirmələri saxlanılmır. **vlan.dat**-da saxlanılan **VLAN-lar 2-1001** ad, növ, vəziyyət və maksimum ötürmə vahidi (**MTU**) daxil olmaqla parametrlərə malik ola bilər.

### VLAN Üzvlüyü

Şəbəkə administratorları bir kommutatorun portlarını **VLAN**-lara **statik** və ya **dinamik** olaraq təyin edə bilərlər. **Statik VLAN təyinatı**, ən sadə və ən yaygın metod olub, hər bir portu kommutatorun **şəbəkə əməliyyat sistemi** istifadə edərək əl ilə bir **VLAN**-a təyin etməyi əhatə edir; bu, bütün kommutatorlar üçün ayrı-ayrılıqda edilməlidir (bu portlara qoşulan son nöqtələrin **VLAN**-ların mövcudluğundan xəbərsiz olduğunu yadda saxlamaq vacibdir). Bunun əksinə olaraq, **dinamik VLAN təyinatı** bir son nöqtənin **VLAN** üzvlüyünü **MAC ünvanlarına** və ya protokollara əsaslanaraq avtomatik olaraq müəyyən edir. Sistem administratoru **MAC ünvanlarını mərkəzləşdirilmiş VLAN idarəetmə xidmətində/verilənlər bazasında**, məsələn, **VLAN Membership Policy Server (VMPS)** xidmətində qeyd edə bilər və sonra kommutator həmin xüsusi **MAC** ünvanına malik son nöqtənin **VLAN**-ını müəyyən etmək üçün **VMPS** verilənlər bazasını sorğulayır. Elastikliyinə və mobilliyinə baxmayaraq, **dinamik VLAN**-lar idarəetmə xərcini artırır.

Təhlükəsizlik baxımından, **statik VLAN**-lar daha təhlükəsiz seçimdir, çünki bir port əl ilə dəyişdirilməyincə əbədi olaraq müəyyən bir **VLAN ID**-yə bağlanmış olacaq. **Dinamik VLAN**-lar üçün, bir hücumçu **macchanger** kimi alətlərdən istifadə edərək **legitim son nöqtələrin MAC ünvanını saxtalaşdıra** və onların **VLAN**-larına üzvlük əldə edə bilər, nəticədə onlardan keçən bütün şəbəkə trafikini *sniff* edə bilər.

### Access və Trunk Portları

Bir **VLAN**-ı dəstəkləyən kommutator üzərindəki hər hansı bir port ya **access port** ya da **trunk port** olmalıdır. **Access portları** yalnız bir **VLAN**-a aiddir və onun trafikini daşıya bilər (və ya bəzi hallarda ikisini, ikincisi **səs trafikini** üçün); bir **access portuna** gələn hər hansı bir trafik portun təyin olunduğu **VLAN**-a aid olduğu güman edilir. Digər tərəfdən, **trunk portları** eyni zamanda **birdən çox VLAN**-ı daşıya bilər; **trunk əlaqələri** ($2$) kommutator (və ya bir kommutator və router) üzərindəki iki **trunk portunu** birləşdirərək birdən çox **VLAN**-dan məlumatın kommutatorlar arasında ötürülməsinə imkan verir.

### VLAN İdentifikasiyası

Standart **$802.3$ Ethernet** freymləri **VLAN** məlumatını ehtiva etmir; buna görə də, kommutatorlar və digər **VLAN**-ı dəstəkləyən cihazlar **VLAN**-ı dəstəkləyən cihazlardan keçərkən paketlə əlaqəli bütün **VLAN** məlumatlarını izləmək üçün bir mexanizmə ehtiyac duyurlar. Bunu həyata keçirmək üçün iki əsas **trunking metodu** istifadə olunur: **ISL** və **IEEE $802.1\text{Q}$**.

#### Inter-Switch Link (ISL)

**Inter-Switch Link (ISL)** **VLAN**-ı dəstəkləyən cihazlar arasında *trunking* üçün istifadə olunan **Cisco-ya məxsus** bir protokoldur. Baxmayaraq ki, **ISL** ilk *trunking* metodlarından biridir ($802.1\text{Q}$-dan əvvəl), **köhnəlmişdir** və müasir **Cisco** kommutatorlarında (və routerlərində) geniş istifadə edilmir. Əvəzində, əksəriyyəti yalnız geniş qəbul edilmiş **$802.1\text{Q}$**-nı dəstəkləyir. **ISL** **orijinal Ethernet başlığı** və **VLAN etiketi** daxil olmaqla bütün **Ethernet** freymini kapsullayır, öz **$26$ baytlıq başlığını** və **$4$ baytlıq trailerini** əlavə edir.

#### IEEE 802.1Q

Müxtəlif şəbəkə avadanlıqları satıcılarından **VLAN** texnologiyalarının **qarşılıqlı işləməsini** təmin etmək üçün **Elektrik və Elektronika Mühəndisləri İnstitutu (IEEE)** $1998$-ci ildə **$802.1\text{Q}$** spesifikasiyasını inkişaf etdirdi. **IEEE 802** komitəsi **$802.3$ Ethernet** freym formatını bir cüt **$2$ baytlıq sahə**, **TPID** və **TCI**-ni (üç alt sahədən ibarətdir: **PCP, DEI və VID**) əlavə etməklə dəyişdirməli oldu ki, nəticədə **VLAN**-a uyğun **$802.1\text{Q}$ Ethernet** freymi yarandı.

<img width="1872" height="1188" alt="image" src="https://github.com/user-attachments/assets/aa86984b-e320-4e60-866d-732312492ff3" />

**Tag Protocol Identifier (TPID)** **$16$-bitlik** bir sahədir və **Ethernet** freymı-nın **$802.1\text{Q}$ etiketli freym** olduğunu müəyyən etmək üçün həmişə **$0\text{x}8100$** olaraq təyin edilir. **Tag Control Information (TCI)** **$16$-bitlik** bir sahədir və **Priority Code Point (PCP)**, **Drop Eligible Indicator (DEI)** (əvvəllər **Canonical Format Indicator (CFI)** kimi tanınırdı) və **VLAN Identifier (VID)**-i ehtiva edir. **VLAN**-larla əlaqəli əsas sahə, **TCI**-nin aşağı dərəcəli **$12$-bitini** tutan **VID**-dir. **$12$ bit** olduğu üçün **$2^{12} - 2 = 4094$ VLAN ID-yə** imkan verir (yadınızdadırsa, **$0$** və **$4095$** ayrılmışdır). Buna görə də, **$802.1\text{Q}$ etiketli freym $4094$ VLAN** üçün məlumat ehtiva edə bilər; tək bir paket daxilində **çoxsaylı $802.1\text{Q}$ etiketlərinin** daxil edilməsi təcrübəsi **$802.1\text{ad}$** tərəfindən tətbiq edilən **İkiqat Etiketləmə (Double Tagging)** kimi tanınır. **VLAN etiketləməsi** bir **$802.1\text{Q}$ Ethernet başlığına VLAN məlumatının daxil edilməsi** prosesidir, halbuki **VLAN etiketinin çıxarılması** (**VLAN untagging**) bir **$802.1\text{Q}$ etiketli Ethernet freymindən VLAN məlumatının çıxarılması** və paketin təyinat portlarına yönləndirilməsi prosesidir.

-----

## VLAN-ı Dəstəkləyən NIC-lər (VLAN-Capable NICs)

Kompüterlərə/serverlərə qoşulmuş bəzi **şəbəkə interfeys kartları (NICs)** **VLAN etiketləməsini** dəstəkləyir. Gəlin **Linux** və **Windows** istifadə edərək bir **NIC**-ə **VLAN ID**-nin necə təyin olunduğuna baxaq.

### Linux-da NIC-lərə VLAN Təyin Edilməsi

**Linux**-da **VLAN** yaratmaq, **ana interfeys** (*parent interface*) adlanan başqa bir interfeysin üzərində interfeys yaratmaqla həyata keçirilir. Bu **VLAN interfeysi** təyin olunmuş **VLAN ID** ilə paketləri etiketləyəcək, qayıdan paketlər isə **etiketsiz** (*untagged*) olacaq.

**Linux**-da bir şəbəkə adapterinə **VLAN** təyin etmək üçün **ip, nmcli** və **vconfig** (köhnəlmiş) kimi bir çox alətdən istifadə edilə bilər. Lakin, əvvəlcə **Kernel**-in **$802.1\text{Q}$ modulu** ilə yükləndiyinə əmin olmalıyıq:

```bash
nijatmansimov@htb[/htb]$ sudo modprobe 8021q
```

Daha sonra, **$802.1\text{q}$**-nun uğurla yükləndiyinə əmin olmaq üçün **lsmod** istifadə edə bilərik:

```bash
nijatmansimov@htb[/htb]$ lsmod | grep 8021
8021q                  40960  0
garp                   16384  1 8021q
mrp                    20480  1 8021q
```

İndi, üzərində **VLAN** interfeysi yaradacağımız **fiziki Ethernet interfeysinin** adını tapmalıyıq ki, bu da **eth0**-dır:

```bash
nijatmansimov@htb[/htb]$ ip a
<SNIP>
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether a6:ba:3b:08:3a:36 brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    altname ens3
    inet 94.2X.5X.72/22 brd 94.237.51.255 scope global dynamic eth0
       valid_lft 83489sec preferred_lft 83489sec
    inet6 fe80::a4ba:3bff:fe08:3a36/64 scope link 
       valid_lft forever preferred_lft forever
```

Sonra, **eth0** üzərində, məsələn, istənilən **VLAN**-ın üzvü olan **20**-ni yaratmaq üçün **vconfig** istifadə edəcəyik:

```bash
nijatmansimov@htb[/htb]$ sudo vconfig add eth0 20
Warning: vconfig is deprecated and might be removed in the future, please migrate to ip(route2) as soon as possible!
```

Əvəzinə **ip** istifadə etmək üçün:

```bash
sudo ip link add link eth0 name eth0.20 type vlan id 20
```

Bu əmrlərin hər ikisi **eth0.20@eth0** adlı yeni bir interfeys yaradacaq:

```bash
nijatmansimov@htb[/htb]$ ip a
<SNIP>
4: eth0.20@eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether a6:ba:3b:08:3a:36 brd ff:ff:ff:ff:ff:ff
```

Daha sonra, yerli şəbəkə daxilində **VLAN 20** olan ünvanlara təyin edilmiş **altşəbəkəyə** əsaslanaraq, interfeysə bir **IP ünvanı** təyin etməli və sonra onu **işə salmalıyıq**:

```bash
nijatmansimov@htb[/htb]$ sudo ip addr add 192.168.1.1/24 dev eth0.20
nijatmansimov@htb[/htb]$ sudo ip link set up eth0.20
```

Nəhayət, interfeysin **up** vəziyyətinə keçib-keçmədiyini yoxlaya bilərik:

```bash
nijatmansimov@htb[/htb]$ ip a | grep eth0.20
4: eth0.20@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    inet 192.168.1.1/24 scope global eth0.20
```

-----

### Windows-da NIC-lərə VLAN Təyin Edilməsi

**Windows**-da, **VLAN etiketləməsini** dəstəkləyən fiziki şəbəkə adapterinə **VLAN** təyin etmək üçün əvvəlcə **Aygıt Yöneticisi** (*Device Manager*) açılmalıdır:

<img width="1920" height="1017" alt="image" src="https://github.com/user-attachments/assets/cb8fe489-7b76-45b7-a968-0f9013f1ef70" />

Sonra VLAN-a təyin etmək istədiyimiz Ethernet interfeysi üçün Xüsusiyyətlər üzərinə klikləməliyik:

<img width="2970" height="1672" alt="image" src="https://github.com/user-attachments/assets/6c67af84-0d90-4426-978b-f6ef16487e50" />

Advanced daxilində dəyər təyin edə biləcəyimiz VLAN ID xüsusiyyəti olacaq. OK düyməsini basdıqdan sonra, əgər adapter VLAN təyin etməyi dəstəkləyirsə, o qurulacaq; əks halda, pəncərə bağlanacaq və bu hostdan gələn heç bir paketə VLAN teqi əlavə edilməyəcək:

<img width="2918" height="1646" alt="image" src="https://github.com/user-attachments/assets/7b8e19fe-488d-4233-9cd7-9b69f7cf5430" />

GUI-yə güvənmək əvəzinə, biz **PowerShell**-dən istifadə edə bilərik. Əvvəlcə, **Get-NetAdapter** Cmdlet-dən istifadə edərək mövcud olan bütün fiziki şəbəkə adapterlərinin adlarını alaq:

```powershell
PS C:\> Get-NetAdapter | Format-Table -AutoSize

Name                                           InterfaceDescription                                                          ifIndex Status             MacAddress              LinkSpeed
----                                           --------------------                                                          ------- ------             ----------              ---------
VirtualBox Host-Only Network  VirtualBox Host-Only Ethernet Adapter                                        20 Up                    0A-00-27-10-42-15       1 Gbps
Ethernet 2                                 ASIX AX88772B USB2.0 to Fast Ethernet Adapter                            55 Up                    90-EB-78-14-21-7F    100 Mbps
Bluetooth Network Connection  Bluetooth Device (Personal Area Network)                                   18 Disconnected   38-41-25-E8-DE-2D        3 Mbps
Wi-Fi                                         Intel(R) Wireless-AC 9560 160MHz                                                12 Disconnected   8E-36-6A-7A-BA-6A 866.7 Mbps
```

Əvvəllər biz **Device Manager**-dən **Ethernet 2**-ni **VLAN 10**-a təyin etmək üçün istifadə etmişdik; interfeysin **VLAN ID**-sini almaq üçün **-DisplayName** bayrağı ilə **Get-NetAdapaterAdvancedProperty** Cmdlet-dən və **"vlan id"** dəyərindən istifadə edə bilərik:

```powershell
PS C:\> Get-NetAdapterAdvancedProperty -DisplayName "vlan id"

Name                      DisplayName                    DisplayValue                   RegistryKeyword RegistryValue
----                      -----------                    ------------                   --------------- -------------
Ethernet 2                VLAN ID                        10                                     VLAN_ID               {10}
```

Biz həmçinin **Set-NetAdapter** Cmdlet-dən və **-VlanID** bayrağından istifadə edərək fiziki şəbəkə ünvanının **VLAN ID**-sini təyin edə bilərik; bu güclü Cmdlet həmçinin **MAC ünvanları** kimi interfeyslərin digər xüsusiyyətlərini fərdiləşdirmək üçün də istifadə edilə bilər:

```powershell
PS C:\> Set-NetAdapter -Name "Ethernet 2" -VlanID 10
```

Lakin, unutmayın ki, bu əməliyyat yalnız şəbəkə interfeysi bu funksiyanı dəstəkləyirsə uğurla tamamlanır; əks halda, **PowerShell** interfeysin bunu dəstəkləmədiyini göstərən bir **səhv** verəcəkdir.

-----

## VLAN Etiketli Trafikin Təhlili

Biz **Wireshark**-dan istifadə edərək **vlan** filtri ilə şəbəkədə **VLAN etiketli trafiki** müəyyən edə və təhlil edə bilərik. Məsələn, bir şəbəkə paketi *dump*-ını təhlil edərkən, **$802.1\text{Q}$ etiketləməsi** olan paketləri **vlan** filtrindən istifadə edərək yoxlaya bilərik:

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/e9b5ed69-e49f-4034-8fe9-542f9530fb54" />

Bundan əlavə, biz xüsusi VLAN ID-si olan paketləri axtara bilərik; məsələn, VLAN 10-a malik paketləri axtarmaq üçün vlan.id == 10 filtrindən istifadə edə bilərik:

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/ede40e54-f477-487d-8fd1-088968a1e011" />

Bundan əlavə, bir paket *dump*-ından istifadə olunan **VLAN ID**-lərini sadalamaq üçün **tshark**-dan istifadə edə bilərik:

```bash
nijatmansimov@htb[/htb]$ tshark -r "The Ultimate PCAP v20221220.pcapng" -T fields -e vlan.id | sort -n -u
1
2
3
7
10
20
30
40
50
60
70
80
90
121
125
224
```

-----

## Təhlükəsizlik Nəticələri və VLAN Hücumları

Şəbəkənin təhlükəsizlik vəziyyətini yaxşılaşdırmasına baxmayaraq, rəqiblər hələ də **VLAN**-lar tərəfindən irəli sürülən müdafiə mexanizmlərini **yan keçə** bilərlər. Müasir *switched* şəbəkələrdə **VLAN**-ların istifadəsi çoxsaylı üstünlüklər (məsələn, sadələşdirilmiş şəbəkə baxımı və yaxşılaşdırılmış performans) gətirsə də, həm də müxtəlif **VLAN hücumlarına** səbəb olan **potensial təhlükəsizlik riskləri** təqdim edir. Bu hücumların əsas metodologiyalarını anlamaq və şəbəkələri qorumaq üçün praktik **yumşaltma yanaşmalarını** tətbiq etmək vacibdir.

### VLAN Hopping

**VLAN Hopping** hücumları bir **VLAN**-dan gələn trafikin router köməyi olmadan **başqa bir VLAN** tərəfindən görülməsinə imkan verir. O, **Cisco**-nun iki **Cisco** cihazı arasında **trunk əlaqəsinin** avtomatik formalaşdırılması üçün istifadə olunan bir protokol olan **Dynamic Trunking Protocol (DTP)**-dan istifadə edir. Bir rəqibə əksər kommutator portlarında **defolt olaraq aktivləşdirilmiş avtomatik *trunking* portu xüsusiyyətindən** yararlanmaq üçün bir hostu **kommutator kimi təqlid etmək/hərəkət etmək** üçün konfiqurasiya etməlidir. **VLAN Hopping**-dən istifadə etmək üçün, rəqib **DTP** aktivləşdirilmiş həmin xüsusi portda kommutatora **fiziki olaraq qoşula** bilməlidir. Rəqib həmin xüsusi porta qoşulmuş bir hostu **$802.1\text{Q}$ siqnalını və DTP paketlərini saxtalaşdırmaq** üçün konfiqurasiya edərək bu əlaqədən sui-istifadə edə bilər. Uğurlu olarsa, kommutator nəticədə rəqibin hostu ilə **trunk əlaqəsi** quracaq və **yalnız müəyyən bir VLAN üçün deyil**, bütün şəbəkə paketlərini ifşa edəcək.

Biz **VLAN Hopping** hücumlarını həyata keçirmək üçün **Yersinia** kimi alətlərdən istifadə edə bilərik.

<img width="2874" height="1528" alt="image" src="https://github.com/user-attachments/assets/fc82257a-bc9c-4cb8-ae1e-798f49af284a" />

## Cüt Etiketli VLAN Hopping Hücumu

**Cüt Etiketli VLAN Hopping hücumu** **VLAN**-lara qarşı getdikcə **daha mürəkkəb** bir hücumdur. Baxmayaraq ki, **VLAN cüt etiketləməsi** **İnternet Xidmət Təminatçıları (ISP-lər)** kimi qurumların istifadə etdiyi **legitim bir təcrübədir** (onlar artıq **VLAN etiketli** olan klientlərdən gələn trafiki daşıyarkən daxili olaraq öz **VLAN**-larından istifadə edə bilərlər), rəqiblər də bundan sui-istifadə etməyə cəhd edə bilərlər. **Cüt Etiketli VLAN Hopping hücumunda**, bir rəqib **artıq $802.1\text{Q}$ etiketinə malik olan bir Ethernet freymi** daxilində **gizli $802.1\text{Q}$ etiketi** yerləşdirir, bu da freymin **orijinal $802.1\text{Q}$ etiketi tərəfindən müəyyən edilməyən** fərqli bir **VLAN**-a getməsinə imkan verir.

Bir rəqib bu hücumu üç addımı izləyərək həyata keçirə bilər. Nəzərə alın ki, bu hücum yalnız rəqib **trunk portunun *native VLAN*-ı ilə eyni olan VLAN-da yerləşən bir porta** qoşulmuşdursa işləyir:

1.  **Rəqib cüt etiketli $802.1\text{Q}$ Ethernet freymı-nı kommutatora göndərir**, burada **xarici başlığın VLAN ID-si** rəqibin **VLAN ID**-si ilə eynidir, hansı ki, bu da **trunk portunun *native VLAN*-ı** ilə eynidir. Fərz edək ki, *native VLAN* **VLAN 10**-dur və **VLAN 30** rəqibin çatmaq istədiyi, qurbanın yerləşdiyi **VLAN**-dır.
2.  **Xarici $4$ baytlıq $802.1\text{Q}$ etiketi kommutatora çatır** və o, **native VLAN** olan **VLAN 10** üçün təyin olunmuş kimi görünür. **VLAN 10** etiketi çıxarıldıqdan sonra, freym bütün **VLAN 10** portlarına yönləndirilir. **Trunk portunda VLAN 10 etiketi çıxarılır (silinir)** və paket **yenidən etiketlənmir**, çünki o, **native VLAN**-ın bir hissəsidir. Lakin, **VLAN 30 etiketi hələ də salamatdır (çıxarılmayıb)** və birinci kommutator tərəfindən yoxlanılmayıb.
3.  **Daha sonra, kommutator yalnız rəqibin göndərdiyi daxili $802.1\text{Q}$ etiketini nəzərdən keçirəcək** və freymin rəqibin seçdiyi **VLAN 30** üçün yönləndirilməsinə qərar verəcək. İndi, ikinci kommutator freymi ya birbaşa qurban portuna göndərəcək, ya da qurban hostu üçün mövcud **MAC ünvan cədvəli girişi** olub-olmamasından asılı olaraq onu daşdıracaq (*flood* edəcək).

**Scapy** **VLAN Hopping** hücumunu həyata keçirməyə imkan verir, **Yersinia** ilə yanaşı: 

<img width="2880" height="1530" alt="image" src="https://github.com/user-attachments/assets/cbc01083-9c0b-4a34-ad97-de94480ca55e" />

## VXLAN (Virtual eXtensible Local Area Network)

Əvvəllər qeyd etdik ki, **Ethernet** freymi daxilindəki **$802.1\text{Q}$ başlığındakı VID (VLAN ID)** sahəsi yalnız **$12$ bitdir**, bu da **$4094$ VLAN**-a icazə verir. **Kiçik şəbəkələr** üçün bu say kifayət ola bilsə də, **geniş seqmentasiya** tələb edən **məlumat mərkəzləri** və **bulud xidməti təminatçıları** üçün daha çoxu lazımdır. Bundan əlavə, mövcud **Layer 2** şəbəkələri **artıq yolların** səbəb olduğu şəbəkə *loop*-larının qarşısını almaq üçün **IEEE $802.1\text{D}$ Spanning Tree Protocol (STP)**-dan istifadə edir. Lakin, bəzi **məlumat mərkəzi operatorları** **STP** ilə bağlı **link bloklanması** kimi məhdudiyyətlərlə qarşılaşırlar ki, bu da mövcud portları azaldır və **çox yollu** (*multipathing*) vasitəsilə *resiliency*-nin qarşısını alır. Bu çətinliklər **Layer 2 fiziki infrastrukturuna** əsaslanan **virtuallaşdırılmış mühitlərdə** şəbəkə səmərəliliyinə mane olur. Belə mühitlərdə kritik tələb, hesablama, şəbəkə və yaddaş resurslarını səmərəli şəkildə ayırmaq üçün **Layer 2 şəbəkəsinin** bütün məlumat mərkəzi landşaftı və hətta məlumat mərkəzləri arasında **problemsiz miqyaslanmasıdır** (*scalability*). Buna baxmayaraq, **STP** kimi ənənəvi yanaşmalar, *loop*-suz bir topologiya təmin etsə də, **bir çox linki deaktiv edə** bilər ki, bu da problemi daha da ağırlaşdırır.

**RFC7348**, **Layer 3 şəbəkəsi üzərində Layer 2 *overlay* sxemi** olan **Virtual eXtensible Local Area Network (VXLAN)**-ı təqdim etməklə **Layer 2 şəbəkələrindəki** bu problemlərə və məhdudiyyətlərə bir həll təklif edir. **VXLAN** xüsusi olaraq ənənəvi **Layer 2 şəbəkələrinin** məhdudiyyətlərini aradan qaldırmaq və **virtual maşınlar (VMs)** ilə **çox-kirayəçi** (*multi-tenant*) mühitdə **Layer 2 və Layer 3 məlumat mərkəzi şəbəkə infrastrukturlarının** tələblərinə cavab vermək üçün nəzərdə tutulmuşdur. Mövcud şəbəkə infrastrukturu üzərində işləyən **VXLAN Layer 2 şəbəkəsini problemsiz şəkildə genişləndirmək** üçün innovativ bir yol təqdim edir. Onun əsas məqsədi **Layer 2 şəbəkələrinin** geniş məlumat mərkəzi landşaftları arasında, hətta **birdən çox fiziki məlumat yeri** arasında miqyaslanmasını asanlaşdırmaqdır. Hər bir **VXLAN *overlay*-i VXLAN Segmenti** adlanır, yalnız **eyni VXLAN seqmentindəki VM-lərin** bir-biri ilə əlaqə qura bilməsini təmin edir ki, bu da şəbəkə izolyasiyasını və təhlükəsizliyini qoruyur. **$24$-bitlik seqment ID**, **VXLAN Network Identifier (VNI)** kimi tanınır, hər bir **VXLAN seqmentini unikal şəkildə müəyyən edir**. **VXLAN**-ın qəbul edilməsi eyni inzibati domen daxilində **$16$ milyon VXLAN seqmentinin** (*$2^{24}$*) birgə mövcud olmasına imkan verir, bu da müasir məlumat mərkəzləri və virtuallaşdırılmış mühitlər üçün miqyaslanma və çeviklik təmin edir.

-----

## Cisco Discovery Protocol (CDP)

**Cisco Discovery Protocol (CDP)**, **Cisco** cihazlarının (routerlər, kommutatorlar və körpülər kimi) **birbaşa qoşulmuş digər Cisco cihazları** haqqında məlumat toplamaq üçün istifadə etdiyi **Layer 2** şəbəkə protokoludur. Bu məlumat şəbəkənin **topologiyasını** aşkar etmək və izləmək, həmçinin şəbəkəni idarə etmək və problemləri aradan qaldırmaq üçün istifadə edilə bilər. Bu protokol adətən **Cisco** cihazlarında **aktivdir**, lakin ehtiyac olmadığı təqdirdə və ya təhlükəsizlik səbəbindən deaktiv edilməlidirsə, **deaktiv edilə** bilər.

### CDP Şəbəkə Trafiki

```
22:14:11.563654 CDPv2, ttl: 180s, checksum: 0xebc1 (incorrect -> 0x8b71), length: 180
        Device-ID (0x01), length: 14 bytes: 'router.inlanefreight.loc'
        Addresses (0x02), length: 8 bytes:
                IPv4 (0x01), length: 4: 10.129.100.1
        Port-ID (0x03), length: 9 bytes: 'Ethernet0/0'
        Capability (0x04), length: 4: (0x00000010): Router
        Version String (0x05), length: 27 bytes: 'Cisco IOS Software, C880 Software'
        Platform (0x06), length: 26 bytes: 'Cisco 881 (MPC8300) processor'
```

Göstərilən mesaj cihazın özü haqqında, məsələn, **cihaz adı, IP ünvanı, port adı və routerin funksionallığı** haqqında məlumat, həmçinin cihazın **əməliyyat sistemi və avadanlıq platforması** haqqında məlumat ehtiva edir. Bundan əlavə, birinci sətirdən **CDPv2** ilə **Cisco Discovery Protocol** ilə işlədiyimizi görə bilərik.

Müqayisə üçün, **Spanning Tree Protocol (STP)** adlı başqa bir protokola baxa bilərik. **STP** kommutatorlar arasında çoxsaylı əlaqələrə malik şəbəkədə ***loop*-ların olmamasını** təmin edən bir şəbəkə protokoludur. *Loop*-lar olmur və bu, məlumat paketlərinin bir *loop*-da dövran etməsinin və şəbəkəni sıxışdırmasının qarşısını alır.

### STP Şəbəkə Trafiki

```
22:14:11.563654 STP 802.1w, Rapid STP, Flags [Learn, Forward], bridge-id 8001.00:11:22:33:44:55.8000, length 43
        root-id 8001.AA:AA:AA:AA:AA:AA, cost 0, port-id 8001, message-age 0.00s, max-age 20.00s, hello-time 2.00s, forward-delay 15.00s
```

Bu nümunədə, **STP** mesajının **root kommutatoru, root kommutatorunun MAC ünvanı, mesajın göndərildiyi portun ID-si** və maksimum yaşlanma müddəti, *hello time* və *forward delay* kimi digər **konfiqurasiya parametrləri** haqqında məlumat ehtiva etdiyini görürük.




