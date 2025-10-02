Fayl Köçürmələri

**Giriş**
Hədəf sistemə faylları ötürməyin lazım olduğu çoxsaylı vəziyyətlər mövcuddur. Aşağıdakı ssenarini nəzərdən keçirək.

**Səhnəni qurmaq**

* Bir engagement zamanı IIS veb serverində məhdudiyyətsiz fayl yükləmə zəifliyi vasitəsilə uzaq kod icrası (RCE) əldə edilir.
* Əvvəlcə web shell yüklənir, daha sonra daha geniş ətraflı kəşfiyyat və privilege escalation üçün reverse shell göndərilir.

**Problemlər və məhdudiyyətlər**

* PowerShell istifadə edilərək *PowerUp.ps1* skriptini ötürməyə çalışılır, lakin Application Control Policy səbəbindən PowerShell bloklanır.
* Lokal əl ilə aparılan enumerasiya nəticəsində `SeImpersonatePrivilege` əldə edildiyi müəyyən edilir.
* Yüksəltmə üçün PrintSpoofer alətinin icrası məqsədilə hədəf maşına binary köçürülməlidir.
* `certutil` vasitəsilə GitHub-dan birbaşa fayl endirməyə çalışılır, amma təşkilatın güclü veb məzmun filtri səbəbindən GitHub, Dropbox, Google Drive və bənzər saytlar bloklanır.
* FTP server qurularaq Windows FTP client ilə köçürmə cəhd edilir, lakin şəbəkə firewall-u TCP 21 portu üzrə çıxış trafikinə icazə vermir.
* Impacket-in `smbserver` aləti ilə paylaşılan qovluq yaradılır və TCP 445 (SMB) üçün çıxış trafikinə icazə olduğu aşkar edilir. Bu üsulla binary hədəf maşına uğurla kopyalanır və nəticədə administrator səviyyəsinə yüksəlmə həyata keçirilir.

**Nəzərə alınmalı məqamlar**

* Fayl ötürmələri və şəbəkə davranışı qiymətləndirmə zamanı məqsədlərə çatmaq üçün vacibdir.
* Host səviyyəsində tətbiq ağ siyahısı (application whitelisting), AV/EDR və digər nəzarətlər fəaliyyətinizi məhdudlaşdıra bilər.
* Şəbəkə cihazları (firewall, IDS/IPS və s.) müəyyən portları, protokolları və qeyri-adi əməliyyatları izləyib bloklaya bilər.

**Xülasə**

* Fayl ötürməsi hər bir əməliyyat sisteminin əsas funksiyalarındandır və bunun üçün müxtəlif alətlər mövcuddur.
* Lakin sərt idarəetmə siyasətlərinə malik mühitlərdə bu alətlər bloklana və ya monitorinq edilə bilər; buna görə müxtəlif alternativ texnikalar haqqında bilgi və bacarıq vacibdir.
* Bu modul Windows və Linux sistemlərində ümumi mövcud alətlərdən istifadə edən bir sıra fayl ötürmə texnikalarını əhatə edir — siyahı tam deyil, lakin praktiki təcrübə üçün yaxşı baza təmin edir.

**Tövsiyə**

* Moduldakı praktiki tapşırıqları (target Windows və Linux maşınları ilə) icra edin, müxtəlif ötürmə üsullarını yoxlayın və hər bir üsulun hansı şərtlərdə daha faydalı olduğunu qeyd edin.
* Bu texnikaları HTB Academy-nin digər modullarında və HTB platformasındakı box/lab-larda tətbiq edərək təcrübənizi genişləndirin.
