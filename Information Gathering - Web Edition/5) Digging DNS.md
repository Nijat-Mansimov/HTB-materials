**DNS Araşdırması**

DNS əsasları və müxtəlif qeyd növləri ilə bağlı möhkəm anlayış əldə etdikdən sonra indi praktik hissəyə keçək. Bu bölmə DNS-i veb kəşfiyyat üçün necə istifadə etməyi öyrədəcək və texnika və vasitələrə diqqət yetirəcək.

---

### **DNS Vasitələri**

DNS kəşfiyyatı, DNS serverlərinə sorğu göndərmək və qiymətli məlumatlar çıxarmaq üçün xüsusi vasitələrdən istifadəni əhatə edir. Veb kəşfiyyat mütəxəssislərinin arsenalında ən populyar və çevik vasitələrdən bəziləri:

| Vasitə                          | Əsas Xüsusiyyətlər                                                                                       | İstifadə Halları                                                                                                    |
| ------------------------------- | -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| **dig**                         | Müxtəlif sorğu növlərini (A, MX, NS, TXT və s.) dəstəkləyən çevik DNS sorğu aləti                        | Manual DNS sorğuları, zonaların transferi (icazə varsa), DNS problemlərinin həlli, DNS qeydlərinin dərindən təhlili |
| **nslookup**                    | Əsasən A, AAAA və MX qeydləri üçün sadə DNS sorğu aləti                                                  | Sürətli domen həlli yoxlamaları                                                                                     |
| **host**                        | Qısa və səliqəli çıxış verən DNS sorğu aləti                                                             | A, AAAA və MX qeydlərinin tez yoxlanması                                                                            |
| **dnsenum**                     | Avtomatlaşdırılmış DNS enumerasiya aləti, sözlük hücumları, brute-force, zona transferləri (icazə varsa) | Alt domenlərin kəşfi və DNS məlumatlarının toplanması                                                               |
| **fierce**                      | Rekursiv axtarış və wildcard aşkarlama ilə DNS kəşfiyyatı və alt domen enumerasiyası                     | DNS kəşfiyyatı, alt domenlərin və potensial hədəflərin müəyyənləşdirilməsi                                          |
| **dnsrecon**                    | Çoxsaylı DNS kəşfiyyat texnikalarını birləşdirir və müxtəlif çıxış formatlarını dəstəkləyir              | Ətraflı DNS enumerasiyası, alt domenlərin müəyyənləşdirilməsi, DNS qeydlərinin toplanması                           |
| **theHarvester**                | Müxtəlif mənbələrdən, o cümlədən DNS qeydlərindən məlumat toplayan OSINT aləti                           | Domenlə bağlı e-poçt ünvanları, işçi məlumatları və digər məlumatların toplanması                                   |
| **Onlayn DNS Sorğu Xidmətləri** | DNS sorğularını icra etmək üçün istifadəçi dostu interfeyslər                                            | Komanda xətti vasitələri olmadıqda sürətli və asan DNS sorğuları, domen mövcudluğunu yoxlama                        |

---

### **Domain Information Groper (dig)**

`dig` əmri (Domain Information Groper) DNS serverlərinə sorğu göndərmək və müxtəlif DNS qeydlərini əldə etmək üçün çevik və güclü bir alətdir. Onun çevikliyi və detallı, fərdiləşdirilə bilən çıxışı onu ən çox istifadə olunan vasitəyə çevirir.

---

### **dig Əsas Əmrləri**

| Əmr                             | Təsvir                                                                                                 |
| ------------------------------- | ------------------------------------------------------------------------------------------------------ |
| `dig domain.com`                | Domen üçün standart A qeydi sorğusu icra edir                                                          |
| `dig domain.com A`              | Domenin IPv4 ünvanını (A qeydi) əldə edir                                                              |
| `dig domain.com AAAA`           | Domenin IPv6 ünvanını (AAAA qeydi) əldə edir                                                           |
| `dig domain.com MX`             | Domen üçün mail serverlərini (MX qeydləri) tapır                                                       |
| `dig domain.com NS`             | Domen üçün səlahiyyətli ad serverlərini müəyyənləşdirir                                                |
| `dig domain.com TXT`            | Domenə aid TXT qeydlərini əldə edir                                                                    |
| `dig domain.com CNAME`          | Domen üçün canonical name (CNAME) qeydi əldə edir                                                      |
| `dig domain.com SOA`            | Domen üçün start of authority (SOA) qeydi əldə edir                                                    |
| `dig @1.1.1.1 domain.com`       | Müəyyən bir ad serverinə sorğu göndərir (məsələn, 1.1.1.1)                                             |
| `dig +trace domain.com`         | DNS həll yolunu tam göstərir                                                                           |
| `dig -x 192.168.1.1`            | IP ünvanı üçün reverse lookup icra edir                                                                |
| `dig +short domain.com`         | Sorğunun qısa, sadə cavabını verir                                                                     |
| `dig +noall +answer domain.com` | Yalnız cavab bölməsini göstərir                                                                        |
| `dig domain.com ANY`            | Domen üçün bütün mövcud DNS qeydlərini əldə edir (Qeyd: Çox server ANY sorğularını görməzlikdən gəlir) |

⚠️ Diqqət: Bəzi serverlər çoxsaylı DNS sorğularını aşkar edib bloklaya bilər. Limitlərə hörmət edin və geniş DNS kəşfiyyatı aparmazdan əvvəl icazə alın.

---

### **DNS Sorğusunun Nümunəsi**

```
nijatmansimov@htb[/htb]$ dig google.com

; <<>> DiG 9.18.24-0ubuntu0.22.04.1-Ubuntu <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 16449
;; flags: qr rd ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0
;; WARNING: recursion requested but not available

;; QUESTION SECTION:
;google.com.                    IN      A

;; ANSWER SECTION:
google.com.             0       IN      A       142.251.47.142

;; Query time: 0 msec
;; SERVER: 172.23.176.1#53(172.23.176.1) (UDP)
;; WHEN: Thu Jun 13 10:45:58 SAST 2024
;; MSG SIZE  rcvd: 54
```

Bu çıxış `dig` komandası ilə google.com domeni üçün sorğunun nəticəsidir. Çıxış dörd əsas bölməyə bölünür:

1. **Header (Başlıq)**

   * `opcode: QUERY, status: NOERROR, id: 16449` → Sorğunun növü (QUERY), statusu (NOERROR) və unikal identifikatoru.
   * `flags: qr rd ad; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 0` → DNS header-dakı flag-lar və bölmələrdəki qeydlərin sayı.
   * `WARNING: recursion requested but not available` → Recursion istənib, amma server bunu dəstəkləmir.

2. **Question Section (Sorğu Bölməsi)**

   * `;google.com. IN A` → Sorğu: "google.com-un IPv4 ünvanı nədir?"

3. **Answer Section (Cavab Bölməsi)**

   * `google.com. 0 IN A 142.251.47.142` → Cavab: google.com üçün IP ünvanı 142.251.47.142-dir. `0` TTL-dir (cache müddəti).

4. **Footer (Alt Bölmə)**

   * `Query time: 0 msec` → Sorğunun işləmə müddəti
   * `SERVER: 172.23.176.1#53` → Cavabı verən DNS serveri və protokol
   * `WHEN: Thu Jun 13 10:45:58 SAST 2024` → Sorğunun tarixi
   * `MSG SIZE rcvd: 54` → Alınan DNS mesajının ölçüsü

EDNS səbəbiylə bəzi `dig` sorğularında opt pseudosection mövcud ola bilər; bu daha böyük mesaj ölçüləri və DNSSEC dəstəyi üçün istifadə olunur.

---

Əgər yalnız cavabı görmək istəyirsinizsə, `+short` opsiyasını istifadə edə bilərsiniz:

```
nijatmansimov@htb[/htb]$ dig +short hackthebox.com

104.18.20.126
104.18.21.126
```
