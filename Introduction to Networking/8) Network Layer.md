## Şəbəkə Qatı (Network Layer)

**OSI**-nin **Şəbəkə qatı** (**Qat 3**) məlumat paketlərinin mübadiləsini idarə edir, çünki bu paketlər birbaşa qəbulediciyə yönləndirilə bilməz və buna görə də **marşrutlaşdırma qovşaqları** ilə təmin edilməlidir. Məlumat paketləri hədəfə çatana qədər qovşaqdan qovşağa ötürülür. Bunu həyata keçirmək üçün, **şəbəkə qatı** fərdi şəbəkə qovşaqlarını müəyyənləşdirir, əlaqə kanallarını qurur və təmizləyir, həmçinin marşrutlaşdırma və məlumat axınının idarə edilməsinə nəzarət edir. Paketləri göndərərkən, ünvanlar qiymətləndirilir və məlumatlar şəbəkə vasitəsilə qovşaqdan qovşağa yönləndirilir. Qovşaqlarda adətən **L3**-dən yuxarı qatlarda məlumatların işlənməsi baş vermir. **Ünvanlara** əsaslanaraq, **marşrutlaşdırma** və **marşrutlaşdırma cədvəllərinin qurulması** həyata keçirilir.

Qısaca, o, aşağıdakı funksiyalar üçün məsuliyyət daşıyır:

* **Məntiqi Ünvanlama**
* **Marşrutlaşdırma** (*Routing*)

**OSI**-nin hər bir qatında **protokollar** müəyyən edilir və bu protokollar müvafiq qatda əlaqə üçün **qaydalar toplusunu** təmsil edir. Onlar yuxarıdakı və ya aşağıdakı qatların protokolları üçün şəffafdır. Bəzi protokollar bir neçə qatın tapşırıqlarını yerinə yetirir və iki və ya daha çox qatı əhatə edir. Bu qatda ən çox istifadə olunan protokollar bunlardır:

* **IPv4 / IPv6**
* **IPsec**
* **ICMP**
* **IGMP**
* **RIP**
* **OSPF**

O, paketlərin mənbədən təyinat yerinə **altşəbəkə daxilində və ya xaricində** marşrutlaşdırılmasını təmin edir. Bu iki altşəbəkənin fərqli ünvan sxemləri və ya uyğun olmayan ünvan növləri ola bilər. Hər iki halda da, məlumat ötürülməsi bütün rabitə şəbəkəsindən keçir və şəbəkə qovşaqları arasında marşrutlaşdırmanı əhatə edir. Göndərici və qəbuledici arasında fərqli altşəbəkələr səbəbindən birbaşa əlaqə həmişə mümkün olmadığı üçün, paketlər yolda olan **qovşaqlardan (routerlərdən)** irəli göndərilməlidir. İrəli göndərilən paketlər daha yüksək qatlara çatmır, lakin onlara **yeni bir aralıq təyinat yeri** təyin olunur və növbəti qovşağa göndərilir.

