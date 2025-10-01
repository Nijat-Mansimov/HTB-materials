## Şəbəkə Topologiyaları

**Şəbəkə topologiyası** bir şəbəkədəki cihazların tipik bir düzülüşü və **fiziki** və ya **məntiqi** əlaqəsidir. Kompüterlər, şəbəkədən aktiv istifadə edən **hostlardır**, məsələn, **müştərilər** (*clients*) və **serverlər**. Onlar həmçinin, daha sonrakı bölmələrdə ətraflı müzakirə edəcəyimiz, paylama funksiyasına malik olan və bütün şəbəkə hostlarının bir-biri ilə məntiqi əlaqə qura bilməsini təmin edən **şəbəkə komponentlərini** də (məsələn, **sviçlər**, **körpülər** və **routerlər**) əhatə edir. Şəbəkə topologiyası istifadə ediləcək komponentləri və ötürmə mühitinə giriş metodlarını müəyyən edir.

Cihazları birləşdirmək üçün istifadə olunan **ötürmə mühitinin düzülüşü** şəbəkənin **fiziki topologiyasıdır**. Keçirici və ya şüşə lif mühitləri üçün bu, kabelləşmə planına, **qovşaqların** (*nodes*) mövqelərinə və qovşaqlar ilə kabelləşmə arasındakı əlaqələrə aiddir. Əksinə, **məntiqi topologiya** siqnalların şəbəkə mühitində necə fəaliyyət göstərməsi və ya məlumatların şəbəkə üzərindən bir cihazdan cihazların fiziki əlaqəsinə necə ötürülməsidir.

Bütün şəbəkə topologiyası sahəsini üç sahəyə bölə bilərik:

1.  **Əlaqələr**
    | Kabelli Əlaqələr | Simsiz Əlaqələr |
    | :--- | :--- |
    | Koaksial kabelləşmə | Wi-Fi |
    | Şüşə lif kabelləşmə | Mobil rabitə (*Cellular*) |
    | Burulmuş cüt kabelləşmə | Peyk (*Satellite*) |
    | və digərləri | və digərləri |

2.  **Qovşaqlar (Nodes) - Şəbəkə İnterfeys Nəzarətçiləri (NICs)**
    * Təkrarlayıcılar (*Repeaters*)
    * Hablar (*Hubs*)
    * Körpülər (*Bridges*)
    * Sviçlər (*Switches*)
    * Router/Modem
    * Şlüzlər (*Gateways*)
    * Firewallar

Şəbəkə qovşaqları, mühitdəki elektrik, optik və ya radio siqnallarının ötürücüləri və qəbulediciləri ilə **ötürmə mühitinin əlaqə nöqtələridir**. Bir qovşaq bir kompüterə qoşula bilər, lakin müəyyən növlərdə bir qovşaqda yalnız bir mikro-nəzarətçi ola bilər və ya heç bir proqramlaşdırıla bilən cihaz olmaya bilər.

3.  **Təsnifatlar**
    Topologiyanı bir şəbəkənin virtual forması və ya **strukturu** kimi təsəvvür edə bilərik. Bu forma şəbəkədəki cihazların faktiki fiziki düzülüşünə uyğun gəlməyə bilər. Buna görə də bu topologiyalar ya **fiziki** ya da **məntiqi** ola bilər. Məsələn, bir **LAN**-dakı kompüterlər yataq otağında bir dairə şəklində düzülə bilər, lakin faktiki **üzük topologiyasına** (*ring topology*) sahib olması çox çətindir.

Şəbəkə topologiyaları aşağıdakı səkkiz əsas növə bölünür:

* Nöqtədən-Nöqtəyə (*Point-to-Point*)
* Şin (*Bus*)
* Ulduz (*Star*)
* Üzük (*Ring*)
* Tor (*Mesh*)
* Ağac (*Tree*)
* Hibrid (*Hybrid*)
* Zəncirvari (*Daisy Chain*)

Daha mürəkkəb şəbəkələr yuxarıda qeyd olunan əsas topologiyalardan iki və ya daha çoxunun **hibridləri** kimi qurula bilər.

---

### Nöqtədən-Nöqtəyə (Point-to-Point)

İki host arasında xüsusi bir əlaqəyə malik olan ən sadə şəbəkə topologiyası **nöqtədən-nöqtəyə** topologiyasıdır. Bu topologiyada, birbaşa və sadə fiziki əlaqə yalnız **iki host** arasında mövcuddur. Bu iki cihaz bu əlaqələrdən qarşılıqlı əlaqə üçün istifadə edə bilər.

Nöqtədən-nöqtəyə topologiyaları ənənəvi telefoniyanın əsas modelidir və **P2P** (**Peer-to-Peer** arxitekturası) ilə qarışdırılmamalıdır.

<img width="2576" height="684" alt="image" src="https://github.com/user-attachments/assets/ebd6070e-3d0a-4808-80b8-6cc0a40c2f38" />

## Şin (Bus)

**Şin topologiyasında** bütün hostlar bir ötürmə mühiti vasitəsilə birləşdirilir. Hər bir host ötürmə mühitinə və onun üzərindən ötürülən siqnallara çıxışa malikdir. Onun üzərindəki prosesləri idarə edən heç bir **mərkəzi şəbəkə komponenti** yoxdur. Bunun üçün ötürmə mühiti, məsələn, bir **koaksial kabel** ola bilər.

Mühit bütün digərləri ilə paylaşıldığı üçün, yalnız **bir host göndərə** bilər və bütün digərləri yalnız məlumatları qəbul edə, qiymətləndirə və özü üçün nəzərdə tutulub-tutulmadığını görə bilər.

<img width="2576" height="1294" alt="image" src="https://github.com/user-attachments/assets/2f62e173-9482-447d-9e84-5749ef3b0dd8" />

## Ulduz (Star)
Ulduz topologiyası bütün hostlarla əlaqə saxlayan bir şəbəkə komponentidir. Hər bir host ayrı bir əlaqə vasitəsilə **mərkəzi şəbəkə komponentinə** qoşulur. Bu, adətən bir **router**, bir **hab** və ya bir **sviç** olur. Bunlar məlumat paketləri üçün **yönləndirmə funksiyasını** yerinə yetirirlər. Bunu etmək üçün, məlumat paketləri qəbul edilir və təyinat yerinə göndərilir. Bütün məlumatlar və əlaqələr onun üzərindən keçdiyi üçün mərkəzi şəbəkə komponentindəki məlumat trafikinin həcmi çox yüksək ola bilər.

<img width="2576" height="1707" alt="image" src="https://github.com/user-attachments/assets/8eac804c-a8aa-49dc-8e48-8562931df972" />

## Üzük (Ring)

**Fiziki** üzük topologiyası elədir ki, hər bir host və ya qovşaq iki kabel ilə üzüyə qoşulur:
* Biri **gələn** siqnallar üçün
* Digəri **çıxan** siqnallar üçün.

Bu o deməkdir ki, hər bir hosta bir kabel gəlir və bir kabel çıxır. Üzük topologiyası adətən aktiv bir şəbəkə komponenti tələb etmir. Ötürmə mühitinə nəzarət və giriş bütün stansiyaların riayət etdiyi bir protokol vasitəsilə tənzimlənir.

**Məntiqi** üzük topologiyası fiziki ulduz topologiyasına əsaslanır, burada qovşaqdakı bir paylayıcı (distributor) bir portdan digərinə yönləndirməklə üzüyü simulyasiya edir.

Məlumat əvvəlcədən müəyyən edilmiş bir ötürmə istiqamətində ötürülür. Adətən, ötürmə mühitinə giriş mərkəzi stansiyadan bir bərpa sistemi və ya **token** (jeton) istifadə edilərək stansiyadan stansiyaya ardıcıl olaraq həyata keçirilir. Token, **iddia token prosesi** (*claim token process*) əsasında işləyən, davamlı olaraq bir istiqamətdə üzük şəbəkəsindən keçən bir bit nümunəsidir.

<img width="2576" height="1746" alt="image" src="https://github.com/user-attachments/assets/12f1f83e-4536-47e7-b69b-a37970ae48b4" />

## Tor (Mesh)

Tor şəbəkələrində çoxsaylı qovşaqlar **fiziki** səviyyədə əlaqələr və **məntiqi** səviyyədə marşrutlaşdırma barədə qərar verir. Buna görə də, tor strukturlarının sabit topologiyası yoxdur. Əsas konsepsiyadan iki əsas struktur var: **tam torlu** (*fully meshed*) və **qismən torlu** (*partially meshed*) struktur.

**Tam torlu strukturdakı** şəbəkədə hər bir host digər hostlarla birləşdirilir. Bu o deməkdir ki, hostlar bir-biri ilə torlanmışdır. Bu texnika əsasən **yüksək etibarlılığı** və **bant genişliyini** təmin etmək üçün **WAN** və ya **MAN**-da istifadə olunur.

Bu quruluşda, routerlər kimi vacib şəbəkə qovşaqları bir-biri ilə şəbəkələşdirilə bilər. Əgər bir router sıradan çıxarsa, digərləri problemsiz işləməyə davam edə bilər və şəbəkə çoxsaylı əlaqələr sayəsində bu uğursuzluğu udmaq qabiliyyətinə malikdir.

Tam torlu topologiyanın hər bir qovşağı eyni **marşrutlaşdırma funksiyalarına** malikdir və şəbəkə şlüzünə yaxınlıq və trafik yükləri ilə əlaqəli olaraq əlaqə qura biləcəyi qonşu qovşaqları bilir.

**Qismən torlu strukturlarda** son nöqtələr yalnız bir əlaqə ilə birləşdirilir. Şəbəkə topologiyasının bu növündə, xüsusi qovşaqlar dəqiq bir başqa qovşağa qoşulur və bəzi digər qovşaqlar nöqtədən-nöqtəyə əlaqə ilə iki və ya daha çox digər qovşağa qoşulur.

<img width="2576" height="2024" alt="image" src="https://github.com/user-attachments/assets/41b8de25-53cf-4295-80f3-57eb0cee14b6" />

## Ağac (Tree)

**Ağac topologiyası** daha geniş yerli şəbəkələrin bu struktura malik olduğu **genişləndirilmiş ulduz topologiyasıdır**. Bu, xüsusilə bir neçə topologiya birləşdirildikdə faydalıdır. Məsələn, bu topologiya tez-tez daha böyük şirkət binalarında istifadə olunur.

Həm **spanning tree**-yə (əhatə edən ağac) uyğun məntiqi ağac strukturları, həm də fiziki ağac strukturları mövcuddur. Hab iyerarxiyası ilə **strukturlaşdırılmış kabelləşməyə** əsaslanan modul müasir şəbəkələr də ağac quruluşuna malikdir. Ağac topologiyaları həmçinin **genişzolaqlı şəbəkələr** və **şəhər şəbəkələri** (**MAN**) üçün də istifadə olunur.

<img width="2576" height="1715" alt="image" src="https://github.com/user-attachments/assets/73ffc1f6-7f11-4160-a626-46b0694fa6f4" />

## Hibrid (Hybrid)

**Hibrid şəbəkələr** iki və ya daha çox topologiyanı birləşdirir, nəticədə yaranan şəbəkə heç bir standart topologiyanı təqdim etmir. Məsələn, bir ağac şəbəkəsi, bir-biri ilə əlaqəli şin şəbəkələri vasitəsilə ulduz şəbəkələrinin birləşdirildiyi bir hibrid topologiyasını təmsil edə bilər. Lakin, başqa bir ağac şəbəkəsi ilə əlaqəli olan bir ağac şəbəkəsi topoloji olaraq hələ də bir ağac şəbəkəsi olaraq qalır. Bir hibrid topologiya həmişə **iki fərqli** əsas şəbəkə topologiyası bir-biri ilə birləşdirildikdə yaranır.

<img width="2576" height="1726" alt="image" src="https://github.com/user-attachments/assets/730acf6e-f201-4665-93d9-d25b924542c4" />

## Zəncirvari (Daisy Chain)

**Zəncirvari topologiyada** (*daisy chain topology*) bir neçə host bir qovşaqdan digərinə bir kabel yerləşdirməklə birləşdirilir.

Bu, bir əlaqə zənciri yaratdığı üçün, eyni zamanda bir neçə aparat komponentinin ardıcıl birləşdirildiyi **zəncirvari konfiqurasiya** (*daisy-chain configuration*) kimi də tanınır. Bu növ şəbəkələşmə tez-tez avtomatlaşdırma texnologiyasında (**CAN**) rast gəlinir.

Zəncirvari birləşmə, struktur olsa da, fiziki düzülüşdən müstəqil edilə bilən **token prosedurlarından** (*token procedures*) fərqli olaraq, qovşaqların **fiziki düzülüşünə** əsaslanır. Siqnal, kompüter sisteminə əvvəlki qovşaqları vasitəsilə bir komponentə göndərilir və oradan geri göndərilir.

<img width="2576" height="1716" alt="image" src="https://github.com/user-attachments/assets/260ae55e-1415-4df1-adc2-57d979e92b63" />












