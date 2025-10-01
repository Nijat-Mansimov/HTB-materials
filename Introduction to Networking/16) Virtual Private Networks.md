## Virtual Özəl Şəbəkələr (VPN)

**Virtual Özəl Şəbəkə (VPN)** xüsusi bir şəbəkə ilə uzaq bir cihaz arasında **təhlükəsiz və şifrələnmiş əlaqəyə** imkan verən bir texnologiyadır. Bu, uzaq maşına birbaşa xüsusi şəbəkəyə daxil olmağa imkan verir, şəbəkənin resurslarına və xidmətlərinə **təhlükəsiz və məxfi giriş** təmin edir. Məsələn, bir administrator başqa yerdən daxili serverləri idarə etməlidir ki, işçilər daxili xidmətlərdən istifadə etməyə davam edə bilsinlər. Bir çox şirkət serverlərə girişi məhdudlaşdırır, buna görə də klientlər bu serverlərə yalnız yerli şəbəkədən daxil ola bilərlər. **VPN** bu zaman işə düşür, administrator İnternet vasitəsilə **VPN** serverinə qoşulur, özünü autentifikasiya edir və beləliklə, **şifrələnmiş tunel** yaradır ki, başqaları məlumat ötürülməsini oxuya bilməsin. Bundan əlavə, administratorun kompüterinə də daxili serverlərə daxil ola və idarə edə biləcəyi **yerli (daxili) IP ünvanı** təyin edilir. Administratorlar ümumiyyətlə şirkətin şəbəkəsinə **təhlükəsiz və sərfəli** uzaqdan giriş təmin etmək üçün **VPN**-lərdən istifadə edirlər. **VPN** adətən **Point-to-Point Tunneling Protocol (PPTP)** **VPN** əlaqələri üçün **TCP/1723** portundan və **IKEv1** və **IKEv2** **VPN** əlaqələri üçün **UDP/500** portundan istifadə edir.

Bu, işçilərə evləri kimi uzaq yerlərdən və ya səyahət edərkən şəbəkəyə və onun **e-poçt** və **fayl serverləri** kimi resurslarına daxil olmağa imkan verir. Administratorların **VPN**-lərdən istifadə etməsinin bir neçə səbəbi var. **VPN**-lər uzaq cihazla xüsusi şəbəkə arasındakı əlaqəni **şifrələyir**, bu da hücum edənlər üçün **həssas məlumatları ələ keçirməyi və oğurlamağı** xeyli çətinləşdirir. Bununla, bütün rabitə daha təhlükəsiz olur.

Digər bir səbəb isə, **VPN**-lərin işçilərə İnternet bağlantısı olduğu müddətcə hər yerdən xüsusi şəbəkəyə və onun resurslarına **uzaqdan daxil olmağa** imkan verməsidir. Bu, xüsusilə səyahət edən və ya evdən işləyən işçilər üçün faydalıdır. Əlavə olaraq, **VPN**-lər **icarəyə verilmiş xətlər** və ya **xüsusi əlaqələr** kimi digər uzaqdan giriş həllərindən **daha sərfəli** ola bilər, çünki uzaq istifadəçiləri xüsusi şəbəkəyə qoşmaq üçün **ictimai İnternetdən** istifadə edirlər.

Bundan əlavə, biz **VPN**-lərdən filiallar kimi **çoxsaylı uzaq yerləri** tək bir xüsusi şəbəkəyə qoşmaq üçün istifadə edə bilərik ki, bu da şəbəkə resurslarını idarə etməyi və onlara daxil olmağı asanlaşdırır. Lakin, bir **VPN**-in işləməsi üçün bir neçə komponent və tələb lazımdır:

| Tələb | Təsvir |
| :--- | :--- |
| **VPN Klienti** | Bu, uzaq cihazda quraşdırılır və **VPN** serveri ilə **VPN** əlaqəsini qurmaq və saxlamaq üçün istifadə olunur. Məsələn, bu, **OpenVPN** klienti ola bilər. |
| **VPN Serveri** | **VPN** klientlərindən **VPN** əlaqələrini qəbul etmək və **VPN** klientləri ilə xüsusi şəbəkə arasında trafiki marşrutlaşdırmaq üçün məsul olan bir kompüter və ya şəbəkə cihazıdır. |
| **Şifrələmə** | **VPN** əlaqələri əlaqəni qorumaq və ötürülən məlumatları mühafizə etmək üçün **AES** və **IPsec** kimi müxtəlif **şifrələmə alqoritmləri** və protokolları istifadə edilərək şifrələnir. |
| **Autentifikasiya** | Təhlükəsiz əlaqə qurmaq üçün **VPN** serveri və klienti **paylaşılan sirr**, **sertifikat** və ya başqa bir autentifikasiya metodu istifadə edərək bir-birini autentifikasiya etməlidir. |

**VPN** klienti və serveri bu portlardan **VPN** əlaqəsini qurmaq və saxlamaq üçün istifadə edir. **TCP/IP qatında**, **VPN** əlaqəsi adətən **VPN** trafikini şifrələmək və autentifikasiya etmək üçün **Encapsulating Security Payload (ESP)** protokolundan istifadə edir. Bu, **VPN** klientinə və serverinə **ictimai İnternet üzərindən məlumatları təhlükəsiz şəkildə mübadilə etməyə** imkan verir.

---

## Internet Protocol Security (IPsec)

**Internet Protocol Security (IPsec)** internet rabitəsi üçün **şifrələmə və autentifikasiya** təmin edən bir şəbəkə təhlükəsizlik protokoludur. O, internet rabitəsi üçün **şifrələmə və autentifikasiya** təmin edən güclü və geniş istifadə olunan bir təhlükəsizlik protokoludur və hər bir **IP** paketinin məlumat yükünü şifrələməklə və paketin bütövlüyünü və həqiqiliyini yoxlamaq üçün istifadə olunan bir **autentifikasiya başlığı (AH)** əlavə etməklə işləyir. **IPsec** şifrələmə və autentifikasiya təmin etmək üçün iki protokolun birləşməsindən istifadə edir:

* **Authentication Header (AH):** Bu protokol **IP** paketləri üçün **bütövlük və həqiqilik** təmin edir, lakin **şifrələmə** təmin etmir. O, hər bir **IP** paketinin üzərinə **autentifikasiya başlığı** əlavə edir ki, bu da paketin **dəyişdirilmədiyini yoxlamaq** üçün istifadə oluna bilən **kriptoqrafik checksum** ehtiva edir.

* **Encapsulating Security Payload (ESP):** Bu protokol **IP** paketləri üçün **şifrələmə və könüllü autentifikasiya** təmin edir. O, hər bir **IP** paketinin məlumat yükünü şifrələyir və könüllü olaraq, **AH**-a bənzər bir **autentifikasiya başlığı** əlavə edir.

**IPsec** iki rejimdə istifadə edilə bilər.

| Rejim | Təsvir |
| :--- | :--- |
| **Transport Rejimi** | Bu rejimdə, **IPsec** hər bir **IP** paketinin **məlumat yükünü** şifrələyir və autentifikasiya edir, lakin **IP başlığını** şifrələmir. Bu, adətən iki host arasında **ucdan-uca rabitəni qorumaq** üçün istifadə olunur. |
| **Tunnel Rejimi** | Bu rejimdə, **IPsec IP başlığı** daxil olmaqla **bütün IP paketini** şifrələyir və autentifikasiya edir. Bu, adətən iki şəbəkə arasında **VPN tuneli** yaratmaq üçün istifadə olunur. |

Məsələn, bir administrator aralarına bir **firewall** qoya bilər. Bir **firewall** xaricindəki bir **VPN** klientindən içəridəki bir **VPN** serverinə **IPsec VPN** trafikini asanlaşdırmaq üçün, firewall aşağıdakı protokollara icazə verməlidir:

| Protokol | Port | Təsvir |
| :--- | :--- | :--- |
| **Internet Protocol (IP)** | **UDP/50-51** | Bu, bütün internet rabitəsinin əsasını təmin edən əsas protokoldur. O, **VPN** klienti və **VPN** serveri arasında məlumat paketlərini marşrutlaşdırmaq üçün istifadə olunur. |
| **Internet Key Exchange (IKE)** | **UDP/500** | **IKE** **VPN** klienti və **VPN** serveri arasında təhlükəsiz rabitəni qurmaq və saxlamaq üçün istifadə olunan bir protokoldur. O, **Diffie-Hellman açar mübadiləsi** alqoritminə əsaslanır və **VPN** trafikini şifrələmək və deşifrə etmək üçün istifadə edilə bilən **paylaşılan gizli açarları** razılaşdırmaq və qurmaq üçün istifadə olunur. |
| **Encapsulating Security Payload (ESP)** | **UDP/4500** | **ESP** həm də **IP** datagramları üçün **şifrələmə və autentifikasiya** təmin edən bir protokoldur. O, **IKE** ilə razılaşdırılmış açarlardan istifadə edərək **VPN** klienti və **VPN** serveri arasında **VPN** trafikini şifrələmək üçün istifadə olunur. |

Bu protokollar **IPsec VPN** trafikinin asanlaşdırılması üçün zəruridir, çünki ictimai İnternet üzərindən təhlükəsiz rabitə üçün tələb olunan təhlükəsizliyi və şifrələməni təmin edirlər. Bu protokollar olmadan, **VPN** trafikinin ələ keçirilməsi və dəyişdirilməsi mümkün olardı.

---

## Point-to-Point Tunneling Protocol (PPTP)

**Point-to-Point Tunneling Protocol (PPTP)** **VPN** klientinin və serverinin arasında təhlükəsiz bir tunel yaradaraq və bu tunel daxilində ötürülən məlumatları kapsullayaraq **VPN**-lərin yaradılmasına imkan verən bir şəbəkə protokoludur. Əslində **Point-to-Point Protocol (PPP)**-nin bir uzantısı olan **PPTP** bir çox əməliyyat sistemi tərəfindən dəstəklənir.

Lakin, məlum **zəiflikləri** səbəbindən **PPTP** artıq **təhlükəsiz hesab edilmir**. O, **IP, IPX** və ya **NetBEUI** kimi protokolları **IP** vasitəsilə tunelləyə bilər, lakin əsasən **L2TP/IPsec, IPsec/IKEv2** və **OpenVPN** kimi **daha təhlükəsiz VPN protokolları** ilə əvəz edilmişdir. $2012$-ci ildən bəri, **PPTP**-nin istifadəsi azalmışdır, çünki onun autentifikasiya metodu **MSCHAPv2**, ixtisaslaşdırılmış avadanlıqlarla asanlıqla qırıla bilən **köhnəlmiş DES şifrələməsindən** istifadə edir.

