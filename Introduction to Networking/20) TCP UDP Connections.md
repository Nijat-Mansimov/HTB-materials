## TCP/UDP Əlaqələri

**Transmission Control Protocol (TCP)** və **User Datagram Protocol (UDP)** hər ikisi internetdə məlumat və verilənlərin ötürülməsi üçün istifadə olunan protokollardır. Tipik olaraq, **TCP** əlaqələri **veb-səhifələr və e-poçtlar** kimi **vacib məlumatları** ötürür. Əksinə, **UDP** əlaqələri **video axını (*streaming*) və ya onlayn oyunlar** kimi **real-time məlumatları** ötürür.

**TCP** bir **əlaqəyə yönəldilmiş** (*connection-oriented*) protokoldur ki, bir kompüterdən digərinə göndərilən **bütün məlumatların alındığını təmin edir**. Bu, zəng başa çatana qədər hər iki tərəfin əlaqədə qaldığı bir telefon danışığına bənzəyir. Məlumat göndərilərkən bir səhv baş verərsə, alıcı geri bir mesaj göndərir ki, göndərən əskik məlumatı yenidən göndərə bilsin. Bu, **TCP**-ni **etibarlı və UDP-dən daha yavaş** edir, çünki ötürmə və səhvlərin bərpası üçün daha çox vaxt tələb olunur.

Digər tərəfdən, **UDP** bir **əlaqəsiz** (*connectionless*) protokoldur. O, **etibarlılıqdan daha çox sürətin vacib olduğu** hallarda, məsələn, video axını və ya onlayn oyunlar üçün istifadə olunur. **UDP** ilə alınan məlumatın **tam və səhvsiz olduğuna dair heç bir yoxlama yoxdur**. Məlumat göndərilərkən bir səhv baş verərsə, alıcı bu əskik məlumatı almayacaq və onu yenidən göndərmək üçün geri heç bir mesaj göndərilməyəcək. **UDP** ilə bəzi məlumatlar **itirilə** bilər, lakin ümumi ötürmə **daha sürətli** olur.

-----

## IP Paketi

Bir **Internet Protocol (IP) paketi** **Open Systems Interconnection (OSI)** modelinin şəbəkə qatı tərəfindən məlumatları bir kompüterdən digərinə ötürmək üçün istifadə olunan verilənlər sahəsidir. O, **başlıq** (*header*) və **yük** (*payload*), yəni **faktiki yük məlumatlarından** ibarətdir.

Biz IP paketini bir zərfdə göndərilən bir məktub kimi də düşünə bilərik. Zərf **başlığı** ehtiva edir ki, bu da **göndərən və alıcı** haqqında məlumatı, həmçinin məktubun hansı poçt şöbələri vasitəsilə göndərilməli olduğuna dair **marşrutlaşdırma təlimatlarını** əhatə edir. Məktubun özü isə **yük**, yəni **faktiki yük məlumatlarıdır**.

### IP Başlığı

IP paketinin başlığı **vacib məlumatları** ehtiva edən bir neçə sahədən ibarətdir.

| Sahə | Təsvir |
| :--- | :--- |
| **Version** | **IP protokolunun** hansı versiyasının istifadə olunduğunu göstərir. |
| **Internet Header Length** | Başlığın ölçüsünü **$32$-bitlik sözlərlə** göstərir. |
| **Class of Service** | Məlumatın ötürülməsinin **nə qədər vacib** olduğunu bildirir. |
| **Total length** | Paketin **baytlarla ümumi uzunluğunu** göstərir. |
| **Identification (ID)** | Paket daha kiçik hissələrə **parçalanarkən** (*fragmented*) bu parçaları müəyyən etmək üçün istifadə olunur. |
| **Flags** | **Parçalanmanı** göstərmək üçün istifadə olunur. |
| **Fragment Offset** | Cari parçasının paketdə **harada yerləşdiyini** göstərir. |
| **Time to Live** | Paketin şəbəkədə **nə qədər qala biləcəyini** göstərir. |
| **Protocol** | Məlumatı ötürmək üçün **hansı protokolun** istifadə olunduğunu göstərir, məsələn, **TCP** və ya **UDP**. |
| **Checksum** | Başlıqdakı **səhvləri aşkar etmək** üçün istifadə olunur. |
| **Source/Destination** | Paketin **haradan göndərildiyini və hara göndərildiyini** göstərir. |
| **Options** | Marşrutlaşdırma üçün **könüllü məlumatları** ehtiva edir. |
| **Padding** | Paketi tam söz uzunluğuna çatdırmaq üçün doldurur. |

Biz fərqli şəbəkələrdə **birdən çox IP ünvanına** malik olan bir kompüter görə bilərik. Burada **IP ID** sahəsinə diqqət yetirməliyik. O, bir IP paketi **daha kiçik hissələrə parçalanarkən** (*fragmented*) bu parçaları müəyyən etmək üçün istifadə olunur. O, **$0$ ilə $65535$** arasında unikal bir nömrəyə malik olan **$16$-bitlik** bir sahədir.

Əgər bir kompüter **birdən çox IP ünvanına** malikdirsə, **IP ID** sahəsi kompüterdən göndərilən hər paket üçün **fərqli**, lakin **çox oxşar** olacaq. **TCPdump**-da şəbəkə trafiqi buna bənzəyə bilər:

### Şəbəkə Sniffing

```
IP 10.129.1.100.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1337
IP 10.129.1.100.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1338
IP 10.129.1.100.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1339
IP 10.129.2.200.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1340
IP 10.129.2.200.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1341
IP 10.129.2.200.5060 > 10.129.1.1.5060: SIP, length: 1329, id 1342
```

Çıxışdan görə bilərik ki, **iki fərqli IP ünvanı** $10.129.1.1$ IP ünvanına paketlər göndərir. Lakin, **IP ID**-dən paketlərin **ardıcıl** olduğunu görə bilərik. Bu, bu iki IP ünvanının şəbəkədə **eyni hosta aid olduğunu** qəti şəkildə göstərir.

### IP Record-Route Sahəsi

IP başlıqındakı **Record-Route sahəsi** də təyinat cihazına gedən yolu qeyd edir. Təyinat cihazı **ICMP Echo Reply** paketini geri göndərərkən, paketin keçdiyi bütün cihazların IP ünvanları IP başlığının **Record-Route sahəsində** siyahılanır. Bu, məsələn, aşağıdakı əmri istifadə etdiyimiz zaman baş verir:

```bash
nijatmansimov@htb[/htb]$ ping -c 1 -R 10.129.143.158
PING 10.129.143.158 (10.129.143.158) 56(124) bytes of data.
64 bytes from 10.129.143.158: icmp_seq=1 ttl=63 time=11.7 ms
RR: 10.10.14.38
        10.129.0.1
        10.129.143.158
        10.129.143.158
        10.10.14.1
        10.10.14.38


--- 10.129.143.158 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 11.688/11.688/11.688/0.000 ms
```

Çıxış göstərir ki, bir **ping sorğusu** göndərilib və təyinat cihazından bir cavab alınıb və həmçinin **ICMP Echo Request** paketinin IP başlığındakı **Record-Route sahəsi** göstərilir. **Record Route** sahəsi **ICMP Echo Request** paketinin təyinat cihazına gedən yolda **keçdiyi bütün cihazların IP ünvanlarını** ehtiva edir. Bu halda, **Record-Route** sahəsi IP ünvanlarını ehtiva edir:

  * $10.10.14.38$
  * $10.129.0.1$
  * $10.129.143.158$
  * $10.129.143.158$
  * $10.10.14.1$
  * $10.10.14.38$

**Traceroute** aləti də **TCP zaman aşımı** (*timeout*) metodunu istifadə edərək marşrutu daha dəqiq izləmək üçün istifadə edilə bilər. Bu proses necə işləyir:

1.  Biz IP başlıqında **TTL dəyəri $1$** olan **TCP SYN** paketini təyinat cihazına göndəririk.
2.  **TTL dəyəri $1$-dən böyük** olan **TCP SYN** paketi bir routerə çatdıqda, **TTL dəyəri $1$ vahid azaldılır** və paket növbəti cihaza yönləndirilir. Əgər **TTL dəyəri $1$ olan TCP SYN** paketi bir routerə çatırsa, paket **atılır** və router bizə geri **ICMP Time-Exceeded** paketi göndərir.
3.  Biz **ICMP Time-Exceeded** paketini alırıq və paketi göndərən routerin **IP ünvanını** qeyd edirik.
4.  Bundan sonra, biz **TTL-i $1$ vahid artıraraq** təyinat cihazına başqa bir **TCP SYN** paketi göndəririk.
5.  Proses **TCP SYN** paketi təyinat hostuna çatana və hədəfdən **TCP SYN/ACK** və ya **TCP RST** cavabı alınana qədər təkrarlanır. Təyinat cihazından cavab aldıqdan sonra, marşrutu izlədiyimizi bilirik və *traceroute* prosesini başa çatdırırıq.

-----

## IP Yükü (IP Payload)

**Yük** (həmçinin **IP Məlumatı** (*IP Data*) adlanır) paketin **faktiki yüküdür**. O, **TCP** və ya **UDP** kimi ötürülən müxtəlif protokollardan gələn məlumatları ehtiva edir, eynilə zərfdəki məktubun məzmunu kimi.

### TCP

**TCP paketləri**, həmçinin **seqmentlər** (*segments*) kimi tanınır, başlıqlar və yüklər adlanan bir neçə hissəyə bölünür. **TCP seqmentləri** göndərilən **IP paketi** daxilində bükülür.

Başlıq **vacib məlumatları** ehtiva edən bir neçə sahədən ibarətdir:

  * **Source port** paketin göndərildiyi kompüteri göstərir.
  * **Destination port** paketin hansı kompüterə göndərildiyini göstərir.
  * **Sequence number** məlumatların hansı ardıcıllıqla göndərildiyini göstərir.
  * **Confirmation number** bütün məlumatların uğurla alındığını təsdiqləmək üçün istifadə olunur.
  * **Control flags** paketin bir mesajın sonunu qeyd etdiyini, məlumatın alındığına dair bir təsdiq olduğunu və ya məlumatın təkrarlanması üçün bir sorğu ehtiva etdiyini göstərir.
  * **Window size** alıcının nə qədər məlumat ala biləcəyini göstərir.
  * **Checksum** başlıqdakı və yükdəki səhvləri aşkar etmək üçün istifadə olunur.
  * **Urgent Pointer** alıcıya yükdə vacib məlumat olduğunu xəbərdar edir.

Yük paketin **faktiki yüküdür** və iki nəfər arasındakı söhbətin məzmunu kimi, ötürülən məlumatları ehtiva edir.

### UDP

**UDP** iki host arasında **datagramları** (kiçik məlumat paketləri) ötürür. Bu, **əlaqəsiz protokoldur**, yəni məlumat göndərməzdən əvvəl göndərən və alıcı arasında **əlaqə qurulmasına ehtiyac yoxdur**. Əvəzində, məlumat əvvəlcədən heç bir əlaqə olmadan birbaşa hədəf hostuna göndərilir.

**Traceroute UDP** ilə istifadə edildikdə, **UDP datagram paketi** hədəf cihaza çatanda biz **Destination Unreachable** və **Port Unreachable** mesajı alacağıq. Ümumiyyətlə, **UDP paketləri** **Unix** hostlarında *traceroute* istifadə edərək göndərilir.

-----

## Kor Spoofing (Blind Spoofing)

**Kor Spoofing**, bir hücumçunun hədəf cihazlar tərəfindən geri göndərilən **faktiki cavabları görmədən** şəbəkədə **yanlış məlumatlar göndərdiyi** bir məlumat manipulyasiyası hücumudur. O, **yanlış mənbə və təyinat ünvanlarını** göstərmək üçün **IP başlığı sahəsini manipulyasiya etməyi** əhatə edir. Məsələn, biz hədəf hosta **yanlış mənbə və təyinat port nömrələri** və **yanlış Başlanğıc Ardıcıllıq Nömrəsi (ISN)** olan bir **TCP paketi** göndəririk. **ISN** bir əlaqədəki **ilk TCP paketinin ardıcıllıq nömrəsini** müəyyən etmək üçün istifadə olunan **TCP başlığındakı** bir sahədir. **ISN** bir **TCP paketi** göndərən tərəfindən təyin edilir və ilk paketin **TCP başlığında** alıcıya göndərilir. Bu, hədəf hostun bizimlə əlaqəni almadan **əlaqə qurmasına** səbəb ola bilər.

Bu hücum adətən **şəbəkə əlaqələrinin bütövlüyünü pozmaq** və ya **şəbəkə cihazları arasındakı əlaqələri kəsmək** üçün istifadə olunur. O, həmçinin **şəbəkə trafikini izləmək** və ya **şəbəkə cihazları tərəfindən göndərilən məlumatları ələ keçirmək** üçün də istifadə edilə bilər.
.
