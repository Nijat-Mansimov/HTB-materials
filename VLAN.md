### VLAN-lar

Təsəvvür edin ki, **XQ** adlı bir startup şirkəti ofisdə istifadə olunacaq şəbəkəni yaratmaq üçün bir sistem inzibatçısı işə götürdü. Büdcə məhdudiyyətlərinə görə onlar yalnız bir **switch** və **router** ala bildilər. XQ-nun sistem inzibatçısı dedi ki, şəbəkədə veb və verilənlər bazası serverləri ilə yanaşı, müxtəlif şöbələrin işçiləri də istifadə edəcək. Təcrübəli şəbəkə təhlükəsizliyi mütəxəssisi olan inzibatçı dərhal daxildən edilə biləcək hücumları, xüsusilə də **broadcast traffic**-dən istifadə edən, məsələn, **broadcast storm** kimi hücumları düşündü. Buna görə də, problemi həll etmək üçün şəbəkəni **Virtual Local Area Networks (VLANs)** vasitəsilə məntiqi olaraq bölməyə qərar verdi. VLAN konseptual olaraq bir switch-i kiçik mini-switch-lərə parçalayır.

**VLAN** — switch-də müəyyən olunmuş portlara qoşulmuş şəbəkə nöqtələrinin məntiqi qruplaşdırılmasıdır. VLAN şəbəkələrin seqmentləşdirilməsinə imkan verir, beləliklə, şəbəkə inzibatçıları şəbəkəni komanda, funksiya, şöbə və ya tətbiqə görə seqmentləşdirə bilərlər, bu zaman istifadəçilərin və ya cihazların fiziki yerləşmələri problem yaratmır. Bir VLAN üzərindən göndərilən **broadcast** paketi digər VLAN üzvlərinə çatmır. Hər bir VLAN ayrıca **broadcast domeni** hesab edildiyi üçün öz **subnet**-inə malik olmalıdır. Məsələn, XQ şirkətində sistem inzibatçısı şəbəkəni şöbələrə görə aşağıdakı kimi seqmentləşdirə bilər:

| Şöbə      | VLAN ID | Subnet         |
| --------- | ------- | -------------- |
| Serverlər | VLAN 10 | 192.168.1.0/24 |
| Rəhbərlik | VLAN 20 | 192.168.2.0/24 |
| Maliyyə   | VLAN 30 | 192.168.3.0/24 |
| HR        | VLAN 40 | 192.168.4.0/24 |
| Marketinq | VLAN 50 | 192.168.5.0/24 |
| Dəstək    | VLAN 60 | 192.168.6.0/24 |

**VLAN istifadəsinin üstünlükləri:**

* **Daha yaxşı təşkilatlanma:** Şəbəkə inzibatçıları cihazları ortaq xüsusiyyətlərinə görə qruplaşdıra bilər.
* **Təhlükəsizliyin artırılması:** Şəbəkə seqmentasiyası icazəsiz şəxslərin digər VLAN-lardakı paketləri izləməsinin qarşısını alır.
* **Sadələşdirilmiş idarəetmə:** İstifadəçinin fiziki yeri inzibatçı üçün problem yaratmır.
* **Performansın artması:** Broadcast trafiki azaldığı üçün şəbəkədə daha çox bant genişliyi qalır.

**Cisco switch-ləri** 1-dən 4094-ə qədər VLAN ID-lərindən istifadə etməyə imkan verir (0 və 4095 rezervdir və istifadə oluna bilməz). 1–1005 arası normal diapazon VLAN-lardır. VLAN 1 — **default VLAN** olaraq bilinir və dəyişdirilməməli və ya silinməməlidir. 1002–1005 VLAN-ları **Token Ring** və **FDDI** üçün rezerv edilib. 1006–4094 isə **uzadılmış diapazon VLAN-lar** sayılır. Normal diapazon VLAN-ları (2–1001) haqqında bütün konfiqurasiyalar **vlan.dat** faylında saxlanılır. Bu VLAN-lar üçün ad, tip, vəziyyət və MTU (maksimum ötürmə vahidi) parametrləri təyin oluna bilər. Uzadılmış diapazon VLAN-larda isə dəyişikliklər yadda saxlanılmır.

---

### VLAN üzvlükləri

Şəbəkə inzibatçıları switch portlarını VLAN-lara **statik** və ya **dinamik** şəkildə təyin edə bilərlər.

* **Statik VLAN təyinatı** — ən sadə və ən geniş yayılmış üsuldur. Burada hər port inzibatçı tərəfindən müəyyən VLAN-a əl ilə təyin olunur. Bu, bütün switch-lərdə ayrıca edilməlidir. Portlara qoşulan cihazlar VLAN mövcudluğundan xəbərsiz olur.
* **Dinamik VLAN təyinatı** — avtomatik olaraq cihazın VLAN üzvlüyünü müəyyən edir. Bu zaman **MAC ünvanları** və ya protokollardan istifadə olunur. MAC ünvanları **VLAN Membership Policy Server (VMPS)** bazasında qeydiyyata alınır. Switch VMPS bazasını sorğulayaraq cihazın VLAN-ını təyin edir.

Dinamik VLAN-lar çevik olsa da, idarəetmə baxımından əlavə yük yaradır. Təhlükəsizlik baxımından isə **statik VLAN-lar daha etibarlıdır**, çünki port yalnız bir VLAN ID-yə bağlı olur və əl ilə dəyişdirilmədikcə dəyişmir. Dinamik VLAN-larda isə hücumçu **macchanger** kimi alətlərlə başqa cihazların MAC ünvanlarını saxtalaşdıraraq onların VLAN-ına qoşula bilər və trafiki izləyə bilər.

---

### Access və Trunk portlar

VLAN dəstəkləyən switch-dəki bütün portlar ya **access port**, ya da **trunk port** ola bilər.

* **Access portlar** yalnız bir VLAN-a (bəzi hallarda əlavə olaraq səs VLAN-ına) aid ola bilər. Port üzərindən gələn bütün trafik həmin VLAN-a məxsus hesab olunur.
* **Trunk portlar** eyni anda bir neçə VLAN-ın trafiki daşıya bilir. Trunk bağlantıları iki switch və ya switch–router arasında qurulur və bir neçə VLAN məlumatının ötürülməsinə imkan verir.

---

### VLAN identifikasiyası

Standart **802.3 Ethernet freymləri** VLAN məlumatı daşımır. Buna görə VLAN dəstəkləyən cihazların paketin hansı VLAN-a aid olduğunu izləmək üçün əlavə mexanizmə ehtiyacı var. Bunun üçün əsas iki trunking üsulu mövcuddur: **ISL** və **IEEE 802.1Q**.

#### Inter-Switch Link (ISL)

**ISL** — Cisco-ya məxsus trunking protokoludur. İlk trunking metodlarından biridir (802.1Q-dan əvvəl yaradılıb). Hazırda köhnəlmiş sayılır və müasir Cisco cihazlarında geniş istifadə olunmur. ISL bütün Ethernet freymini (header də daxil olmaqla) kapsullaşdırır və üzərinə 26 baytlıq header və 4 baytlıq trailer əlavə edir.

#### IEEE 802.1Q

Müxtəlif istehsalçıların cihazlarının VLAN texnologiyaları ilə uyğun işləməsini təmin etmək üçün IEEE 1998-ci ildə **802.1Q** standartını hazırladı. Bu standart Ethernet freyminə iki 2-baytlıq sahə — **TPID** və **TCI** əlavə etdi. TCI isə üç alt sahədən ibarətdir: **PCP, DEI və VID**. Bu dəyişiklik nəticəsində VLAN dəstəkləyən **802.1Q Ethernet freymləri** meydana gəldi.

<img width="1872" height="1188" alt="image" src="https://github.com/user-attachments/assets/d2d5e650-36c6-4b78-a094-393b6bf034a5" />

### Tag Protocol Identifier (TPID) və Tag Control Information (TCI)

**Tag Protocol Identifier (TPID)** — hər zaman **0x8100** dəyərinə malik olan 16-bitlik sahədir və Ethernet freyminin **802.1Q ilə tag edilmiş** freym olduğunu göstərir.

**Tag Control Information (TCI)** isə 16-bitlik sahədir və özündə bunları saxlayır:

* **Priority Code Point (PCP)**
* **Drop Eligible Indicator (DEI)** (əvvəllər **Canonical Format Indicator (CFI)** kimi tanınırdı)
* **VLAN Identifier (VID)**

VLAN-larla bağlı əsas sahə **VID**-dir və TCI-nin aşağıdakı 12 bitini tutur. 12 bit olduğuna görə **2¹² – 2 = 4096** VLAN ID mövcuddur (0 və 4095 rezerv olunub). Buna görə də **802.1Q ilə tag edilmiş freym** 4094 VLAN haqqında məlumat daşıya bilir. Bir paketdə birdən çox 802.1Q tag yerləşdirmək praktikasına **Double Tagging** deyilir və bu üsul **802.1ad** ilə təqdim olunmuşdur.

**VLAN tagging** — VLAN məlumatının 802.1Q Ethernet başlığında yerləşdirilməsi prosesidir.
**VLAN untagging** — 802.1Q ilə tag edilmiş Ethernet freymindən VLAN məlumatının çıxarılması və paketin təyin olunmuş portlara yönləndirilməsi prosesidir.

---

### VLAN dəstəkləyən NIC-lər

Bəzi kompüter/server şəbəkə adapterləri (**NIC**) VLAN tagging-i dəstəkləyir. Gəlin, Linux və Windows sistemlərində şəbəkə adapterinə VLAN ID təyin etmə prosesinə baxaq.

---

### Linux-da NIC-lərə VLAN təyin etmək

Linux-da VLAN yaratmaq üçün əsas interfeys üzərində yeni interfeys yaradılır, bu interfeysə **parent interface** deyilir. VLAN interfeysi paketləri təyin olunmuş VLAN ID ilə tag edəcək, cavab paketləri isə untagged şəkildə gələcək.

Linux-da şəbəkə adapterinə VLAN təyin etmək üçün **ip**, **nmcli** və **vconfig** (köhnəlib) kimi alətlərdən istifadə etmək olar. Əvvəlcə Kernel-də **802.1Q modulunun** yükləndiyinə əmin olmalıyıq:

```
nijatmansimov@htb[/htb]$ sudo modprobe 8021q
```

Sonra **lsmod** ilə 8021q modulunun yükləndiyini yoxlaya bilərik:

```
nijatmansimov@htb[/htb]$ lsmod | grep 8021
8021q                  40960  0
garp                   16384  1 8021q
mrp                    20480  1 8021q
```

Daha sonra VLAN interfeysini yaradacağımız fiziki şəbəkə interfeysinin adını tapmalıyıq, məsələn **eth0**:

```
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

Sonra, məsələn, VLAN 20 üçün **eth0** üzərində yeni interfeys yarada bilərik:

```
nijatmansimov@htb[/htb]$ sudo vconfig add eth0 20
```

```
Warning: vconfig is deprecated and might be removed in the future, please migrate to ip(route2) as soon as possible!
```

`ip` vasitəsilə isə belə etmək olar:

```
sudo ip link add link eth0 name eth0.20 type vlan id 20
```

Bu əmrlər yeni **eth0.20@eth0** interfeysini yaradacaq:

```
nijatmansimov@htb[/htb]$ ip a

<SNIP>
4: eth0.20@eth0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether a6:ba:3b:08:3a:36 brd ff:ff:ff:ff:ff:ff
```

Daha sonra VLAN 20 üçün təyin olunmuş subnet-ə uyğun olaraq IP ünvan verib interfeysi aktivləşdirmək lazımdır:

```
nijatmansimov@htb[/htb]$ sudo ip addr add 192.168.1.1/24 dev eth0.20
nijatmansimov@htb[/htb]$ sudo ip link set up eth0.20
```

Nəhayət, interfeysin aktiv olduğunu yoxlaya bilərik:

```
nijatmansimov@htb[/htb]$ ip a | grep eth0.20
4: eth0.20@eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    inet 192.168.1.1/24 scope global eth0.20
```

---

### Windows-da NIC-lərə VLAN təyin etmək

Windows-da VLAN tagging-i dəstəkləyən fiziki şəbəkə adapterinə VLAN ID təyin etmək üçün əvvəlcə **Device Manager** açılmalıdır.

<img width="1920" height="1017" alt="image" src="https://github.com/user-attachments/assets/bec5c606-3442-4786-babe-47472b81f3e8" />

Sonra VLAN təyin etmək istədiyimiz Ethernet interfeysi üçün Properties üzərinə klikləmək lazımdır:

<img width="2970" height="1672" alt="image" src="https://github.com/user-attachments/assets/eb059fb4-c650-4ca6-b320-ee589c7122c6" />

**Advanced** bölməsində **VLAN ID** xassəsi olacaq və biz ona bir dəyər təyin edə bilərik. **OK** düyməsini kliklədikdən sonra, əgər adapter VLAN təyinatını dəstəkləyirsə, VLAN tətbiq olunacaq; əks halda pəncərə bağlanacaq və bu host-dan gedən paketlərə heç bir VLAN tag əlavə edilməyəcək.

<img width="2918" height="1646" alt="image" src="https://github.com/user-attachments/assets/7c669aa5-8be8-41fa-bf75-f9dde45448fa" />

GUI-yə etibar etmək əvəzinə, **PowerShell** istifadə edə bilərik. Əvvəlcə **Get-NetAdapter** Cmdlet-i ilə mövcud bütün fiziki şəbəkə adapterlərinin adlarını əldə edək:

Əvvəl **Device Manager** vasitəsilə Ethernet 2-ni VLAN 10-a təyin etmişdik; interfeysin VLAN ID-sini əldə etmək üçün isə **Get-NetAdapterAdvancedProperty** Cmdlet-i və **-DisplayName** flag-i ilə **vlan id** istifadə edə bilərik:

```
PS C:\> Get-NetAdapterAdvancedProperty -DisplayName "vlan id"

Name                      DisplayName                    DisplayValue                   RegistryKeyword RegistryValue
----                      -----------                    ------------                   --------------- -------------
Ethernet 2                VLAN ID                        10                                     VLAN_ID               {10}
```

Fiziki şəbəkə interfeysinin VLAN ID-sini təyin etmək üçün **Set-NetAdapter** Cmdlet-i və **-VlanID** flag-i istifadə olunur. Bu Cmdlet həmçinin interfeyslərin digər xassələrini, məsələn, MAC ünvanlarını da dəyişdirməyə imkan verir:

```
PS C:\> Set-NetAdapter -Name "Ethernet 2" -VlanID 10
```

Yadda saxlayın ki, bu əməliyyat yalnız şəbəkə interfeysi VLAN təyinatını dəstəklədikdə uğurla həyata keçiriləcək; əks halda PowerShell interfeysin bu funksionallığı dəstəkləmədiyini bildirən xəta verəcək.

---

### VLAN Tag edilmiş Trafikin Analizi

Wireshark istifadə edərək şəbəkədə VLAN tag edilmiş trafiki müəyyənləşdirib analiz edə bilərik. Məsələn, 802.1Q tag-lı paketləri yoxlamaq üçün **vlan** filterindən istifadə olunur:

```
vlan
```

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/4fc1556a-2fa7-4dc7-b79c-e76f21172e9a" />

Bundan əlavə, müəyyən VLAN ID-li paketləri axtara bilərik; məsələn, VLAN 10-a malik paketləri tapmaq üçün **vlan.id == 10** filterindən istifadə olunur:

```
vlan.id == 10
```

<img width="2880" height="1800" alt="image" src="https://github.com/user-attachments/assets/be7846df-f2f1-4404-8c56-71cbc0d733fe" />

Bundan əlavə, paket dump-dan istifadə olunan VLAN ID-ləri siyahıya almaq üçün **tshark**-dan istifadə edə bilərik:

```
VLANs
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

### Təhlükəsizlik nəticələri və VLAN hücumları

Şəbəkənin təhlükəsizlik vəziyyətini yaxşılaşdırmağa baxmayaraq, düşmənlər hələ də VLAN-ların təmin etdiyi qoruyucu mexanizmləri aşmağın yollarını tapa bilərlər. Müasir switch-lənmiş şəbəkələrdə VLAN-lardan istifadə bir sıra üstünlüklər (məsələn, şəbəkə idarəetməsinin sadələşdirilməsi və performansın yaxşılaşması) gətirsə də, bu eyni zamanda potensial təhlükəsizlik risklərini də ortaya çıxarır və müxtəlif VLAN hücumlarına yol açır. Bu hücumların əsas metodologiyalarını başa düşmək və şəbəkələri qorumaq üçün praktik aradan qaldırma yanaşmalarını tətbiq etmək vacibdir.

#### VLAN Hopping

**VLAN hopping** hücumu bir VLAN-dan olan trafikin router köməyi olmadan digər VLAN tərəfindən görünməsinə imkan verir. Bu hücum Cisco-nun Dynamic Trunking Protocol (DTP) protokolundan sui-istifadə edir — bu protokol iki Cisco cihazı arasında trunk link-in avtomatik danışıqlarını həyata keçirmək üçün istifadə olunur. Hücum edən şəxs avtomatik trunk port xüsusiyyətindən (əksər switch portlarında standartla aktivdir) yararlanmaq üçün bir host-u switch kimi tənzimləməlidir. VLAN hopping-dən yararlanmaq üçün hücum edənin fiziki olaraq DTP aktiv olan switch portuna qoşula bilməsi lazımdır. Hücumçu bu bağlantıdan 802.1Q siqnallaşdırmasını və DTP paketlərini saxtalaşdırmaq üçün istifadə edə bilər. Əgər uğur qazansa, switch nəticədə hücumçunun host-u ilə trunk əlaqəsi yaradacaq və bunun nəticəsində yalnız konkret VLAN üçün deyil, bir neçə VLAN-ın paketləri ifşa olunacaq.

VLAN hopping hücumlarını həyata keçirmək üçün **Yersinia** kimi alətlərdən istifadə etmək olar:

<img width="2874" height="1528" alt="image" src="https://github.com/user-attachments/assets/0c2083a5-2664-42a8-8e3a-9186cafe69d7" />

Double-tagging VLAN hopping
Double-tagging VLAN hopping hücumu VLAN-lara qarşı getdikcə daha mürəkkəbləşən hücum növüdür. VLAN double-tagging bəzi təşkilatların, məsələn İnternet Xidmət Təminatçılarının (ISP) daxili istifadəsində qanuni praktikadır (onlar öz VLAN-larını daxili olaraq istifadə edərkən müştərilərdən gələn artıq VLAN tag-lı trafiki daşıya bilərlər), lakin düşmənlər bundan sui-istifadə etməyə çalışa bilərlər. Double-tagging VLAN hopping hücumunda hücumçu artıq 802.1Q tag-ı olan Ethernet freyminin içinə gizli bir 802.1Q tag yerləşdirir və bu, freymin orijinal 802.1Q tag-ının göstərmədiyi başqa bir VLAN-a yönləndirilməsinə imkan verir.

Hücumçu bu hücumu üç addımda həyata keçirə bilər. Qeyd etmək lazımdır ki, bu hücum yalnız hücum edənin trunk portun native VLAN-ı ilə eyni VLAN-da yerləşən porta qoşulduğu halda işləyir:

1. Hücumçu outer header-i onun VLAN ID-si olan (yəni trunk portun native VLAN-ı ilə eyni olan) double-tagged 802.1Q Ethernet freymini switch-ə göndərir. Tutaq ki, native VLAN VLAN 10-dur və hücumçunun çatmaq istədiyi VLAN VLAN 30-dur — orada zərərçəkən yerləşir.
2. Outer 4-baytlıq 802.1Q tag switch-ə gəlir və VLAN 10-a, yəni native VLAN-a yönəldilmiş kimi görülür. VLAN 10 tag-ı çıxarıldıqdan sonra freym bütün VLAN 10 portlarına göndərilir. Trunk portda VLAN 10 tag-ı strip edilir (çıxarılır) və paket native VLAN-ın bir hissəsi olduğundan yenidən tag-lanmır. Lakin VLAN 30 tag-ı hələ də bütöv qalır (çıxarılmır) və ilk switch onu yoxlamamışdır.
3. Ardınca switch yalnız hücumçunun göndərdiyi inner 802.1Q tag-ına baxacaq və freymin VLAN 30 üçün yönləndirilməli olduğuna qərar verəcək — bu hücumçunun seçdiyi VLAN-dır. İkinci switch indi freymi ya zərərçəkənin portuna birbaşa göndərəcək, ya da onu flood edəcək; bu, zərərçəkən host üçün mövcud MAC ünvanı cədvəlində giriş olub-olmamasından asılıdır.

Double-tagging VLAN hopping hücumunu həyata keçirmək üçün Yersinia ilə yanaşı **Scapy** də istifadə oluna bilər.

<img width="2880" height="1530" alt="image" src="https://github.com/user-attachments/assets/5389b501-5bb3-48c7-aafe-56046b6fc730" />



