**Alt Domen Brute-Force**

Alt domen Brute-Force Enumerasiyası, potensial alt domen adlarının əvvəlcədən hazırlanmış siyahılarından istifadə edən güclü aktiv alt domen aşkarlama texnikasıdır. Bu yanaşma sistematik şəkildə bu adları hədəf domenə qarşı sınaqdan keçirərək etibarlı alt domenləri müəyyən edir. Düzgün hazırlanmış söz siyahılarından istifadə etməklə, alt domenləri aşkarlama səylərinizin səmərəliliyini və effektivliyini əhəmiyyətli dərəcədə artıra bilərsiniz.

---

### **Prosesin Dörd Addımı**

1. **Söz Siyahısının Seçilməsi:**
   Proses potensial alt domen adlarını ehtiva edən bir söz siyahısının seçilməsi ilə başlayır. Bu siyahılar ola bilər:

   * **Ümumi Məqsədli:** Geniş yayılmış alt domen adlarını ehtiva edir (məsələn, dev, staging, blog, mail, admin, test). Bu yanaşma, hədəfin adlandırma konvensiyalarını bilmədiyiniz zaman faydalıdır.
   * **Hədəfə Yönəlik:** Müəyyən sənayelərə, texnologiyalara və ya hədəfə aid adlandırma nümunələrinə fokuslanır. Bu yanaşma daha səmərəli olub yanlış müsbət nəticələrin ehtimalını azaldır.
   * **Özəl:** Öz açar sözləriniz, nümunələriniz və ya digər mənbələrdən toplanan kəşfiyyat məlumatlarına əsaslanaraq öz söz siyahınızı yarada bilərsiniz.

2. **İterasiya və Sorğu Göndərmə:**
   Skript və ya alət söz siyahısında dönərək hər sözü və ya ifadəni əsas domenə əlavə edir (məsələn, example.com) və potensial alt domen adları yaradır (məsələn, dev.example.com, staging.example.com).

3. **DNS Sorğusu:**
   Hər potensial alt domen üçün DNS sorğusu göndərilir ki, IP ünvanına çevrilib-çevrilmədiyi yoxlansın. Adətən A və ya AAAA qeydləri istifadə olunur.

4. **Filtrləmə və Doğrulama:**
   Əgər alt domen uğurla həll olunursa, etibarlı alt domenlər siyahısına əlavə edilir. Əlavə doğrulama addımları, alt domenin mövcudluğunu və funksionallığını təsdiqləmək üçün həyata keçirilə bilər (məsələn, veb brauzer vasitəsilə ona daxil olmağa çalışmaq).

---

### **Brute-Force Üçün Alətlər**

| Alət            | Təsviri                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **dnsenum**     | Alt domenləri aşkarlamaq üçün lüğət və brute-force hücumlarını dəstəkləyən geniş DNS enumerasiya aləti.                  |
| **fierce**      | İstifadəsi asan, rekursiv alt domen aşkarlanması, wildcard aşkarlanması və sadə interfeys təqdim edir.                   |
| **dnsrecon**    | Bir neçə DNS kəşfiyyat texnikasını birləşdirən, fərdiləşdirilə bilən çıxış formatlarını təqdim edən çevik alət.          |
| **amass**       | Aktiv olaraq saxlanılan alt domen aşkarlama aləti, digər alətlərlə inteqrasiyası və geniş məlumat mənbələri ilə tanınır. |
| **assetfinder** | Sadə, amma effektiv alt domen tapma aləti, sürətli və yüngül skanlar üçün idealdır.                                      |
| **puredns**     | Güclü və çevik DNS brute-force aləti, nəticələri effektiv həll edir və filtrləyir.                                       |

---

### **DNSEnum**

`dnsenum` Perl-də yazılmış, geniş istifadə olunan və çox funksiyalı bir komanda xətti alətidir. Bu alət, hədəf domenin DNS infrastrukturu və potensial alt domenləri haqqında məlumat toplamaq üçün geniş imkanlar təqdim edir. Əsas funksiyaları:

* **DNS Qeydiyyatlarının Aşkarlanması:** A, AAAA, NS, MX və TXT qeydlərini əldə edərək hədəfin DNS konfiqurasiyasını geniş şəkildə görməyə imkan verir.
* **Zona Transferi Sınaqları:** Kəşf edilən name server-lərdən avtomatik zona transferi cəhdləri edir. Uğurlu cəhd tam DNS məlumatlarını ortaya çıxara bilər.
* **Alt Domen Brute-Force:** Söz siyahısı istifadə edərək alt domenləri brute-force ilə enumerasiya edə bilir.
* **Google Scraping:** DNS qeydlərində birbaşa görünməyən əlavə alt domenləri tapmaq üçün Google nəticələrini taraya bilər.
* **Tərs Sorğu (Reverse Lookup):** Verilmiş IP ünvanına aid domenləri müəyyən etmək üçün tərs DNS sorğuları edə bilər.
* **WHOIS Sorğuları:** Domenin sahibliyi və qeydiyyat detalları haqqında məlumat toplamaq üçün WHOIS sorğuları apara bilər.

---

### **Praktik Nümunə**

Hədəf domenimiz `inlanefreight.com` üçün alt domenləri enumerasiya etmək nümunəsi. Biz SecLists-dən `subdomains-top1million-20000.txt` söz siyahısını istifadə edəcəyik.

```bash
dnsenum --enum inlanefreight.com -f /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -r
```

* `dnsenum --enum inlanefreight.com`: Hədəf domeni və enumerasiya parametrlərini göstərir.
* `-f /usr/share/seclists/...`: Brute-force üçün istifadə olunan SecLists söz siyahısının yolu.
* `-r`: Rekursiv alt domen brute-force aktivləşdirir, yəni bir alt domen tapıldıqda onun alt domenləri də yoxlanacaq.

---

**Nümunə çıxış:**

```text
dnsenum VERSION:1.2.6

-----   inlanefreight.com   -----

Host's addresses:
__________________

inlanefreight.com.                       300      IN    A        134.209.24.248

[...]

Brute forcing with /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt:
_______________________________________________________________________________________

www.inlanefreight.com.                   300      IN    A        134.209.24.248
support.inlanefreight.com.               300      IN    A        134.209.24.248
[...]

done.
```

Bu nümunədə, `dnsenum` söz siyahısını istifadə edərək hədəf domenin alt domenlərini brute-force üsulu ilə tapdı.
