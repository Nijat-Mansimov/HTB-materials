Sertifikatların Şəffaflıq Jurnalları
İnternetin geniş kütləsində etibar nazik bir əmtəədir. Bu etibarın təməl daşlarından biri Secure Sockets Layer/Transport Layer Security (SSL/TLS) protokoludur ki, bu da brauzeriniz ilə vebsayt arasındakı kommunikasiyağı şifrələyir. SSL/TLS-in mərkəzində isə rəqəmsal sertifikat dayanır — vebsaytın kimliyini təsdiq edən və təhlükəsiz, şifrələnmiş əlaqəni təmin edən kiçik fayl.

Lakin bu sertifikatların verilməsi və idarə edilməsi prosesi tamamilə etibarlı deyil. Hücumçular qanunsuz və ya səhv verilmiş sertifikatlardan istifadə edərək qanuni vebsaytları imitasiyalamaq, həssas məlumatları ələ keçirmək və ya zərərli proqram yaymaq imkanına malikdirlər. Budur Certificate Transparency (CT) jurnalları işə düşür.

Certificate Transparency Jurnalları nədir?
Certificate Transparency (CT) jurnalları SSL/TLS sertifikatlarının verilişini qeydə alan ictimai, yalnız əlavə edilən (append-only) ledgers-lərdir. Hər dəfə Sertifikat Hakimi (CA) yeni sertifikat verdikdə, onu bir neçə CT jurnalına təqdim etməlidir. Müstəqil təşkilatlar bu jurnalları saxlayır və hər kəsin yoxlaması üçün açıqdır.

CT jurnallarını sertifikatların qlobal reyestri kimi düşünün. Onlar hər bir vebsayt üçün verilmiş hər SSL/TLS sertifikatının şəffaf və doğrulanabilən qeydinə imkan verir. Bu şəffaflıq bir neçə vacib məqsədə xidmət edir:

* **Qanunsuz Sertifikatların Erkən Aşkarlanması:** CT jurnallarını izləməklə təhlükəsizlik tədqiqatçıları və vebsayt sahibləri şübhəli və ya səhv verilmiş sertifikatları tez müəyyən edə bilərlər. Qanunsuz sertifikat — etibarlı Sertifikat Hakimi tərəfindən verilmiş icazəsiz və ya saxta rəqəmsal sertifikatdır. Bunları erkən tapmaq sertifikatları ləğv etmək və zərərli istifadənin qarşısını almaq üçün sürətli tədbirlər görməyə imkan verir.
* **Sertifikat Hakimləri üçün Hesabatlılıq:** CT jurnalları CA-ları onların sertifikat vermə praktikalarına görə hesabatlı edir. Əgər bir CA qayda və standartlara zidd sertifikat verərsə, bu jurnalarda açıq şəkildə görünəcək və nəticədə sanksiyalar və ya etibarın itirilməsi ilə üzləşə bilər.
* **Veb PKI-nın Gücləndirilməsi:** Veb PKI (Public Key Infrastructure) onlayn təhlükəsiz ünsiyyətin əsas etibarn sistemidir. CT jurnalları sertifikatların ictimai nəzarəti və doğrulanması mexanizmini təqdim edərək Veb PKI-nın təhlükəsizliyini və bütövlüyünü gücləndirməyə kömək edir.

CT Jurnalları necə işləməsinin texniki parçasını genişləndirmək üçün klikləyin (istəsəniz).

CT Jurnalları və Veb Kəşfiyyat
Certificate Transparency jurnalları alt domen enumerasiyasında digər metodlara nisbətən unikal üstünlük təklif edir. Brute-force və ya söz siyahısına əsaslanan yanaşmalar alt domen adlarını təxmin etməyə və ya proqnozlaşdırmağa əsaslanarkən, CT jurnalları domen və onun alt domenləri üçün verilmiş sertifikatların dəqiq qeydlərini təqdim edir. Bu o deməkdir ki, siz söz siyahınızın məhdudiyyəti və ya brute-force alqoritminin effektivliyi ilə məhdudlaşmırsınız. Bunun əvəzinə, siz domenin tarixi və əhatəli görünüşünə çıxış əldə edirsiniz — burada aktiv olmayan və ya asanlıqla təxmin edilə bilməyən alt domenlər də daxil ola bilər.

Bundan əlavə, CT jurnalları köhnə və ya vaxtı keçmiş sertifikatlarla əlaqəli alt domenləri üzə çıxara bilər. Bu alt domenlər köhnəlmiş proqram təminatı və ya konfiqurasiyalar saxlayaraq istismara daha həssas ola bilər.

Əsasən, CT jurnalları exhaustive brute-force və ya söz siyahılarının tamlığına güvənmədən alt domenlər kəşf etmək üçün etibarlı və effektiv yol təqdim edir. Onlar domenin tarixçəsinə baxmaq üçün unikal pəncərə açır və əks halda gizli qala biləcək alt domenləri aşkar edə bilər — bu da sizin kəşfiyyat imkanlarınızı əhəmiyyətli dərəcədə genişləndirir.

CT Jurnallarında Axtarış
CT jurnallarında axtarış üçün iki populyar seçim var:

| Alət       | Əsas Xüsusiyyətlər                                                                                                                     | İstifadə Halları                                                                                                      | Üstünlüklər                                            | Mənfi cəhətlər                                  |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ | ----------------------------------------------- |
| **crt.sh** | İstifadəçi dostu veb interfeysi, domen üzrə sadə axtarış, sertifikat detalları və SAN (Subject Alternative Name) girişlərini göstərir. | Tez və asan axtarışlar, alt domenlərin müəyyənləşdirilməsi, sertifikat veriliş tarixçəsinin yoxlanması.               | Pulsuz, istifadə etmək asandır, qeydiyyat tələb etmir. | Məhdud filter və təhlil imkanları.              |
| **Censys** | İnternetə bağlı cihazlar üçün güclü axtarış mühərriki, domen, IP, sertifikat atributları üzrə inkişaf etmiş filtrləmə.                 | Sertifikatların dərin təhlili, yanlış konfiqurasiyaların identifikasiyası, əlaqəli sertifikatları və hostları tapmaq. | Geniş məlumat və filtrləmə imkanları, API çıxışı.      | Qeydiyyat tələb olunur (pulsuz tier mövcuddur). |

crt.sh axtarışı
crt.sh rahat veb interfeysi təklif etsə də, avtomatlaşdırılmış axtarışlar üçün onun API-sindən terminaldan da istifadə edə bilərsiniz. Gəlin `facebook.com` üçün bütün `dev` alt domenlərini `curl` və `jq` ilə necə tapmaq lazım olduğuna baxaq:

```
Certificate Transparency Logs
nijatmansimov@htb[/htb]$ curl -s "https://crt.sh/?q=facebook.com&output=json" | jq -r '.[] 
 | select(.name_value | contains("dev")) | .name_value' | sort -u
 
*.dev.facebook.com
*.newdev.facebook.com
*.secure.dev.facebook.com
dev.facebook.com
devvm1958.ftw3.facebook.com
facebook-amex-dev.facebook.com
facebook-amex-sign-enc-dev.facebook.com
newdev.facebook.com
secure.dev.facebook.com
```

* `curl -s "https://crt.sh/?q=facebook.com&output=json"`: Bu əmr `crt.sh`-dən `facebook.com` uyğunluğunda sertifikatlar üçün JSON çıxışı alır.
* `jq -r '.[] | select(.name_value | contains("dev")) | .name_value'`: Bu hissə JSON nəticələrini filtr edir, `name_value` sahəsində `dev` stringini ehtiva edən qeydləri seçir. `-r` flaqı jq-ya xam (raw) string çıxışı verməyi bildirir.
* `sort -u`: Bu nəticələri əlifba sırasına görə sıralayır və təkrarlananları silir.
