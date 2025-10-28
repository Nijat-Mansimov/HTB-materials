1) LLMNR (link local name resolution) ve NBT-NS(NetBIOS name service) nedir?
Cavab: lokal DNS fail oldugu zaman cihaz domain resolve etmek ucun LLMNR istifade ederek diger hostlardan sorusur eger LLMNR fail olsa bu zaman NBT-NS ise dusur.

2) LLMNR ve NBT-NS istifade ederek hansi hucumlari reallasdirmaq olar?
Cavab: SMB relay attacklarini yol acir bir basa hostun NetNTLMv2 hash'ni elde ede bilirik ve onu crack etmek cox sadedir

3) SMB relay attackinin qarsinin nece almaq olar?
Cavab: ilk olaraq tebii ki bloklamaq gelir agla ama GP daxilinde yalnizca LLMNR bloklamaq mumkundur NetBIOS'u ise her cihazin daxilinde network
adapter propertilerinden bloklamaq lazimdi birbasa GPO ile mumkun deyil. bunun ucun maksimum bir powershell script yazilib GPo elave edilmelidir

4) SOC monitorinq sistemində Domain Controller-dan gələn çoxsaylı "Event ID 4625 – An account failed to log on" hadisələrini müşahidə edirsən. Bu hadisələr müxtəlif istifadəçi hesablarına aid olsa da, hamısı eyni IP ünvanından və qısa vaxt aralığında baş verir.
Cavab: 
Event ID 4625 hadisələrini analiz edərdim — hansı hesabların təsirləndiyini, eyni IP-dən gəlib-gəlmədiyini və vaxt intervalını yoxlayardım.

Əgər hücum SMB üzərindən yox, LDAP üzərindən gəlirsə, Event ID 4771 – Kerberos pre-authentication failed hadisələrinə də baxardım.

Hücum qaynağını müəyyən etmək üçün qaynağın IP ünvanını DNS reverse lookup və ya SIEM üzərindən enrichment edərdim

5) Sual: Bir işçi hostda PowerShell-in bir neçə versiyasının quraşdırıla biləcəyini və bəzi hallarda hücumçuların PowerShell 2.0 və ya daha köhnə versiyaları çağıraraq sistemdəki PowerShell loqlarından yayınmağa çalışdıqlarını öyrəndiniz. Siz SOC analitikisiniz — bu vəziyyəti necə aşkar edər, nə kimi axtarış/hunt qaydaları tətbiq edər, hansı log və konfiqurasiyalara üstünlük verər və belə hücumların qarşısını almaq üçün nə tövsiyə edərdiniz?

Proses yaradılması üzrə sorğu: powershell.exe və command-line içində -Version 2 və ya -v 2 kimi ifadələrə baxmaq.

PowerShell v2 deaktivləşdirilməsi (mümkün olduqda) — köhnə versiyanın zərərsiz olduğu halda belə, onu saxlamaq riski artırır. (İdarəçi yolla və ya Group Policy ilə tətbiq edilməlidir.)

6) Tesevvur et ki ACL evasion attack bas veribdi
eli adinda istifadeci ogurlanib ve onun elnur adinda istifadeci uzerinde parolunu deyismek huququ var
elnurun parolunu deyisir ve elnurun da help desk qrupunda GenericWrite accessi var yeni ozunu help desk qrupuna elave etmek huququ var ora elave edir 
Help Desk Level qrupu Information Technology qrupuna daxildir. Yəni, bu qrupa daxil olan istənilən istifadəçi həm də Information Technology qrupunun bütün hüquqlarını qazanır.
Information Technology qrupunun aysel istifadəçisi üzərində GenericAll (Tam Ümumi Nəzarət) hüququ var. Bu, ayselin şifrəsini dəyişməyə və ya Kerberoasting hücumunu həyata keçirməyə imkan verir.

Cavab:
Təhlükəli ACL-ləri yoxlamaq və çıxarmaq
Qrup üzvlüyünə nəzarət edin
ACL dəyişiklikləri üçün audit və monitor
Enabling the Advanced Security Audit Policy can help in detecting unwanted changes
Enabling the Advanced Security Audit Policy can help in detecting unwanted changes, especially Event ID 5136: A directory service object was modified which would indicate that the domain object was modified, which could be indicative of an ACL attack. If we look at the event log after modifying the ACL of the domain object, we will see some event ID 5136 created:

Əla 💪 — indi dəqiq anlayıram nə istəyirsən:
Sənə **Bleeding Edge Vulnerabilities** bölməsinə (yəni **NoPac, PrintNightmare, PetitPotam**) əsaslanan **sadə, lakin texniki SOC Analyst müsahibə sualları** lazımdır — yəni CVE-lərin detalları yox, **təhlükənin tanınması və qarşısının alınması (prevention/detection)** yönümlü, real intervü səviyyəsində.

Aşağıda bu mövzuya uyğun **10 konkret SOC sualı və cavabı** təqdim edirəm 👇
Hər biri qısa, lakin texniki və real müsahibə üçün praktik səviyyədədir.

---

## 🧠 **SOC Analyst – Bleeding Edge Vulnerabilities üzrə Suallar və Cavablar**

---

### **1️⃣ SOC analitiki NoPac hücumunu necə aşkar edə bilər?**

**Cavab:**
Active Directory loglarında **kompüter hesabının adının dəyişdirilməsi** (Event ID 4742) və ya **yeni kompüter hesabı yaradılması** (Event ID 4741) hadisələrinə baxmaq lazımdır.
Əgər bu əməliyyat **adi istifadəçi** tərəfindən edilibsə — bu, şübhəlidir.

**Əlavə:** SIEM qaydası → “User created or renamed a computer account”.

---

### **2️⃣ NoPac hücumuna qarşı hansı tədbirlər görülməlidir?**

**Cavab:**

* Microsoft-un **2021 dekabr yamasını** tətbiq etmək.
* Adi istifadəçilərin **kompüter hesabı yaratma icazəsini** məhdudlaşdırmaq (`ms-DS-MachineAccountQuota = 0`).
* “Computer account rename” fəaliyyətlərini monitorinqdə saxlamaq.

---

### **3️⃣ PrintNightmare zəifliyini SOC səviyyəsində necə aşkar etmək olar?**

**Cavab:**
Spooler xidməti (`spoolsv.exe`) tərəfindən **DLL yükləmə** və ya **CreateProcess** hadisələrini (Event ID 7045, 4688) izləmək.
Əgər `spoolsv.exe` qeyri-adi DLL-lər və ya PowerShell prosesləri başladırsa — exploit ehtimalı var.

---

### **4️⃣ PrintNightmare qarşısını almaq üçün hansı praktiki addım atılır?**

**Cavab:**

* **Print Spooler** xidmətini DC-lərdə tam dayandırmaq:

  ```
  Stop-Service Spooler
  Set-Service Spooler -StartupType Disabled
  ```
* Administrator olmayan istifadəçilərin printer sürücüsü yükləməsini məhdudlaşdırmaq.
* Microsoft-un **KB5004945** yamasını quraşdırmaq.

---

### **5️⃣ PetitPotam hücumunun əsas məqsədi nədir və SOC bunu necə görə bilər?**

**Cavab:**
PetitPotam **NTLM relay** vasitəsilə DC-nin kimlik məlumatlarını oğurlamaq üçün istifadə olunur.
SOC analitiki **NTLM autentifikasiya tələblərini** və **EFSRPC çağırışlarını** izləməlidir.
Əgər DC NTLM ilə xarici serverə autentifikasiya etməyə çalışırsa — bu şübhəlidir.

---

### **6️⃣ PetitPotam qarşısını almaq üçün ən effektiv üsul nədir?**

**Cavab:**

* **NTLM Relay** hücumlarını bloklamaq üçün **Extended Protection for Authentication (EPA)** aktiv etmək.
* **LDAP signing** və **SMB signing** aktiv olmalıdır.
* EFSRPC servisini və SMBv1 protokolunu deaktiv etmək.

---

### **7️⃣ SOC analitiki üçün hansı log Event ID-lər bu hücumlarla bağlı önəmlidir?**

**Cavab:**

| Hücum              | Vacib Event ID-lər                                |
| ------------------ | ------------------------------------------------- |
| **NoPac**          | 4741 (Computer created), 4742 (Computer renamed)  |
| **PrintNightmare** | 7045 (Service installed), 4688 (Process creation) |
| **PetitPotam**     | 4624 (NTLM Logon), 4648 (Explicit logon)          |

---

### **8️⃣ Kerberos biletləri ilə bağlı anomaliyalar SOC səviyyəsində necə görünür?**

**Cavab:**
Əgər **adi istifadəçi hesabı** DC və ya yüksək imtiyazlı servislər üçün **TGT/TGS bileti** istəyirsə — bu anomaliyadır.
SIEM-də “Unusual Kerberos service ticket request” qaydası ilə aşkarlanır.

---

### **9️⃣ SOC analitiki SIEM-də bu zəifliklər üçün hansı növ qaydalar yarada bilər?**

**Cavab:**

* “User created computer account outside IT OU”
* “Spoolsv.exe spawned child process (PowerShell, cmd.exe)”
* “NTLM authentication from DC to external host”
* “Unusual Kerberos TGT request by non-admin user”

---

### **🔟 Zəiflik aşkarladıqdan sonra SOC analitiki hansı addımları atmalıdır?**

**Cavab:**

1. **Alert triage** – hadisənin təcili dərəcəsini müəyyən etmək.
2. **Log correlation** – istifadəçi, IP və vaxtla əlaqəli digər logları birləşdirmək.
3. **Incident containment** – kompromit olunmuş hesabı və hostu təcrid etmək.
4. **Escalation** – Tier-2 və ya IR (Incident Response) komandalarına ötürmək.
5. **Patch validation** – müvafiq sistemin yamaqlarının tətbiqini yoxlamaq.

---

İstəsən, mən bu sualları **iki hissəyə bölüb** sənə ayrıca:

* 🧩 “Detection & Monitoring sualları”
* 🛡️ “Prevention & Mitigation sualları”

şəklində PDF və ya DOC formatında müsahibə vərəqi kimi tərtib edə bilərəm.
İstəyirsən onu hazırlayım?

