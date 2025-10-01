## TCP/IP Modeli

**TCP/IP modeli** də qatlı bir istinad modelidir, tez-tez **İnternet Protokol Dəsti** (*Internet Protocol Suite*) adlanır. **TCP/IP** termini **Transmission Control Protocol** (**TCP**) və **Internet Protocol** (**IP**) adlı iki protokolun adlarından yaranır. **IP** **OSI** qat modelinin **Şəbəkə qatında** (**Qat 3**) yerləşir, **TCP** isə **Nəqliyyat qatında** (**Qat 4**) yerləşir.

| Qat | Funksiya |
| :--- | :--- |
| **4. Tətbiq (Application)** | Tətbiq Qatı, tətbiqlərə digər qatların xidmətlərinə daxil olmağa imkan verir və tətbiqlərin məlumat mübadiləsi üçün istifadə etdiyi protokolları müəyyənləşdirir. |
| **3. Nəqliyyat (Transport)** | Nəqliyyat Qatı, Tətbiq Qatı üçün (**TCP**) seans və (**UDP**) datagram xidmətlərini təmin etmək üçün məsuliyyət daşıyır. |
| **2. İnternet (Internet)** | İnternet Qatı, **host ünvanlaması**, **paketləmə** və **marşrutlaşdırma** funksiyaları üçün məsuliyyət daşıyır. |
| **1. Keçid (Link)** | Keçid Qatı, **TCP/IP paketlərini şəbəkə mühitinə yerləşdirmək** və şəbəkə mühitindən müvafiq paketləri qəbul etmək üçün məsuliyyət daşıyır. TCP/IP, şəbəkəyə giriş metodu, freym formatı və mühitdən asılı olmayaraq işləmək üçün nəzərdə tutulmuşdur. |

**TCP/IP** ilə hər bir tətbiq istənilən şəbəkə üzərindən məlumat ötürə və mübadilə edə bilər və qəbuledicinin harada yerləşməsinin fərqi yoxdur. **IP** məlumat paketinin təyinat yerinə çatmasını təmin edir, **TCP** isə məlumat ötürülməsinə nəzarət edir və məlumat axını ilə tətbiq arasındakı əlaqəni təmin edir. **TCP/IP** və **OSI** arasındakı əsas fərq, bəziləri birləşdirilmiş olan qatların sayıdır.

<img width="2576" height="1533" alt="image" src="https://github.com/user-attachments/assets/41c803b1-5e58-490d-a5e6-c7e6e4d38304" />

The most important tasks of TCP/IP are:

Task	Protocol	Description
Logical Addressing	IP	Due to many hosts in different networks, there is a need to structure the network topology and logical addressing. Within TCP/IP, IP takes over the logical addressing of networks and nodes. Data packets only reach the network where they are supposed to be. The methods to do so are network classes, subnetting, and CIDR.
Routing	IP	For each data packet, the next node is determined in each node on the way from the sender to the receiver. This way, a data packet is routed to its receiver, even if its location is unknown to the sender.
Error & Control Flow	TCP	The sender and receiver are frequently in touch with each other via a virtual connection. Therefore control messages are sent continuously to check if the connection is still established.
Application Support	TCP	TCP and UDP ports form a software abstraction to distinguish specific applications and their communication links.
Name Resolution	DNS	DNS provides name resolution through Fully Qualified Domain Names (FQDN) in IP addresses, enabling us to reach the desired host with the specified name on the internet.
