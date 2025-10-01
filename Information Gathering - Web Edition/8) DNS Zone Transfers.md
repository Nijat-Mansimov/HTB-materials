DNS Zona Transferləri
Brute-force faydalı yanaşma ola bilsə də, alt domenləri aşkar etmək üçün daha az müdaxiləli və potensial olaraq daha səmərəli bir üsul — **DNS zona transferləri** mövcuddur. Bu mexanizm ad serverləri arasında DNS qeydlərinin köçürülməsi üçün nəzərdə tutulub, lakin yanlış konfiqurasiya olunarsa, maraqlı gözlər üçün məlumat xəzinəsinə çevrilə bilər.

Zona transferi nədir
DNS zona transferi əsasən bir zona (domen və onun alt domenləri) daxilindəki bütün DNS qeydlərinin bir ad serverindən digərinə bütövlükdə köçürülməsidir. Bu proses DNS serverləri arasında uyğunluq və ehtiyat nüsxəliliyin saxlanması üçün vacibdir. Lakin kifayət qədər qorunmadığı halda, icazəsiz şəxslər bütün zona faylını yükləyə bilər ki, bu da alt domenlərin tam siyahısını, onların əlaqəli IP ünvanlarını və digər həssas DNS məlumatlarını ortaya çıxarar.


<img width="2613" height="1632" alt="image" src="https://github.com/user-attachments/assets/94314395-664c-4cae-a7e5-59ddafe0350d" />

Zona Transferi Sorğusu (AXFR): İkincil DNS server prosesə əsas serverə zona transfer sorğusu göndərərək başlayır. Bu sorğu adətən **AXFR** (Tam Zona Transferi) növündən istifadə edir.
SOA Qeydinin Transferi: Sorğu alındıqda (və ehtimal ki, ikincil server autentifikasiya edildikdən sonra) əsas server öz **Start of Authority (SOA)** qeydini göndərir. SOA qeydi zonanın serial nömrəsi daxil olmaqla zonaya dair vacib məlumatları ehtiva edir; bu, ikincil serverə onun zona məlumatının aktuallığını müəyyən etməyə kömək edir.
DNS Qeydlərinin Göndərilməsi: Daha sonra əsas server zonadakı bütün DNS qeydlərini bir-bir ikincil serverə köçürür. Buraya A, AAAA, MX, CNAME, NS və domenin alt domenlərini, mail serverlərini, ad serverlərini və digər konfiqurasiyaları müəyyən edən digər qeydlər daxildir.
Zona Transferinin Tamamlanması: Bütün qeydlər köçürüldükdən sonra əsas server zona transferinin sonunu bildirir. Bu bildiriş ikincil serverə onun zonanın tam surətini aldığına dair məlumat verir.
Təsdiq (ACK): İkincil server əsas serverə uğurlu qəbul və emalı təsdiq edən bir təsdiq mesajı göndərir. Bu zona transferi prosesini tamamlayır.

Zona Transferinin Zəifliyi
Zona transferləri legitim DNS idarəçiliyi üçün vacib olsa da, yanlış konfiqurasiya olunmuş DNS server bu prosesi əhəmiyyətli bir təhlükəsizlik zəifliyinə çevirə bilər. Problemin mərkəzində zona transferini kimin başlata biləcəyini idarə edən giriş nəzarətləri dayanır.

İnternetin ilk dövrlərində DNS serverdən zona transferi istəməyə hər bir klientin icazəli olması adi hal idi. Bu açıq yanaşma inzibati işləri asanlaşdırsa da, böyük bir təhlükəsizlik boşluğu açdı. Bu o demək idi ki, istənilən şəxs — o cümlədən zərərli aktorlar — DNS serverə onun zona faylının tam surətini tələb edə bilərdi; bu fayl həssas məlumatlarla zəngin olurdu.

İcazəsiz zona transferindən əldə olunan məlumat hücumçu üçün çox dəyərli ola bilər. O, hədəfin DNS infrastrukturasının tam xəritəsini ortaya çıxardır, o cümlədən:

* **Alt domenlər:** Əsas vebsaytda birbaşa əlaqələndirilməyən və ya başqa yollarla asan aşkar olunmayan tam alt domen siyahısı. Bu gizli alt domenlər inkişaf serverlərini, sınaq mühitlərini, inzibati panelləri və ya digər həssas resursları yerləşdirə bilər.
* **IP Ünvanları:** Hər alt domenə aid IP ünvanlar, əlavə kəşfiyyat və ya hücumlar üçün potensial hədəflər təqdim edir.
* **Ad Server Qeydləri:** Domen üçün səlahiyyətli ad serverlər barədə detalları, hosting provayderini və mümkün konfiqurasiya səhvlərini üzə çıxarır.

Yenidənqurma (Remediation)
Xoşbəxtlikdən, bu zəiflik barədə məlumat artıb və əksər DNS server administratorları riski azaltmışlar. Müasir DNS serverlər adətən zona transferlərinə yalnız etibarlı ikincil serverlərə icazə verəcək şəkildə konfiqurasiya olunur ki, həssas zona məlumatı məxfi qalsın.

Lakin insan səhvi və ya köhnə təcrübələr nəticəsində yanlış konfiqurasiyalar hələ də baş verə bilər. Buna görə də (düzgün icazə ilə) zona transferi cəhd etmək hələ də qiymətli kəşfiyyat texnikası sayılır. Uğursuz olsa belə, cəhd DNS serverin konfiqurasiyası və təhlükəsizlik vəziyyəti barədə məlumat verə bilər.

Zona Transferlərinin İstismarı
Tam zona transferi tələb etmək üçün `dig` əmrindən istifadə edə bilərsiniz:

```
DNS Zone Transfers
nijatmansimov@htb[/htb]$ dig axfr @nsztm1.digi.ninja zonetransfer.me
```

Bu əmr `dig`-ə `zonetransfer.me` domeni üçün `nsztm1.digi.ninja` ad serverindən tam zona transferi (axfr) tələb etməyi bildirir. Əgər server yanlış konfiqurasiya olunub və transferə icazə verirsə, domen üçün bütün DNS qeydlərinin tam siyahısını, o cümlədən bütün alt domenləri alacaqsınız.

Nümunə:

```
DNS Zone Transfers
nijatmansimov@htb[/htb]$ dig axfr @nsztm1.digi.ninja zonetransfer.me

; <<>> DiG 9.18.12-1~bpo11+1-Debian <<>> axfr @nsztm1.digi.ninja zonetransfer.me
; (1 server found)
;; global options: +cmd
zonetransfer.me.	7200	IN	SOA	nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.	300	IN	HINFO	"Casio fx-700G" "Windows XP"
zonetransfer.me.	301	IN	TXT	"google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.	7200	IN	MX	0 ASPMX.L.GOOGLE.COM.
...
zonetransfer.me.	7200	IN	A	5.196.105.14
zonetransfer.me.	7200	IN	NS	nsztm1.digi.ninja.
zonetransfer.me.	7200	IN	NS	nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me. 301 IN	TXT	"6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN	SRV	0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200	IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN	AFSDB	1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200	IN	A	127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN	AFSDB	1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A	202.14.81.230
...
;; Query time: 10 msec
;; SERVER: 81.4.108.41#53(nsztm1.digi.ninja) (TCP)
;; WHEN: Mon May 27 18:31:35 BST 2024
;; XFR size: 50 records (messages 1, bytes 2085)
```

`zonetransfer.me` xidməti zona transferlərinin risklərini nümayiş etdirmək üçün xüsusi olaraq qurulub, buna görə `dig` əmrinin tam zona qeydlərini qaytarması gözləniləndir.


