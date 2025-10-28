Aşağıda göndərdiyin bütün mətnın **səliqəli, bölmələnmiş və oxunaqlı** Azərbaycan dilində versiyasını təqdim edirəm. Hər bölmə başlıqlarla ayrılıb, əmrlər və tövsiyələr kod blokları ilə vurğulanıb — müsahibə, SOC playbook və notlar üçün bir sənəd şəklində istifadə edə bilərsən.

---

# Təhlükəsizlik Qeydləri — Qısa və Texniki İzah (Səliqəli Format)

---

## 1. LLMNR və NBT-NS nədir?

**Sual:** LLMNR (Link-Local Multicast Name Resolution) və NBT-NS (NetBIOS Name Service) nədir?
**Cavab:**

* **LLMNR**: Lokal DNS işləmədiyi zaman cihazın domen adını həll etmək üçün istifadə etdiyi link-local adlı protokoldur — şəbəkədəki digər hostlara multicast sorğu göndərir.
* **NBT-NS**: Əgər LLMNR işləməsə, NetBIOS adı ilə bağlı sorğular üçün NBT-NS (NetBIOS Name Service) istifadə olunur. Nəticədə, ad həlli üçün LLMNR → NBT-NS ardıcıllığına baxıla bilər.

---

## 2. LLMNR və NBT-NS vasitəsilə hansı hücumlar həyata keçirilə bilər?

**Cavab:**

* Bu protokollar **SMB relay** və **NetNTLMv2 hash** oğurlama hücumlarına (capture & relay) yol açır.
* Hücumçu şəbəkədə LLMNR/NBT-NS üçün sahte (poisoned) cavab verərək SMB bağlantısı tələb edə bilər və hostun NetNTLMv2 hash-ini ələ keçirə bilər.
* NetNTLMv2 hash-ləri crack etmək nisbətən asandır və daha sonra lateral movement və ya hesab oğurluğu üçün istifadə oluna bilər.

---

## 3. SMB relay hücumunun qarşısını necə almaq olar?

**Cavab (qısa):**

1. **LLMNR və NBT-NS-i blokla** (şəbəkədə və ya GPO yoluyla mümkün olduqda).
2. **NetBIOS (NBT)** üçün qlobal GPO ilə tam bloklama mümkün olmadığından, hər cihazda network adapter parametrlərindən deaktiv etmək daha məntiqlidir — və ya GPO ilə PowerShell skripti tətbiq edərək avtomatlaşdırmaq lazımdır.
3. **SMB signing** və **LDAP/SMB channel binding** aktivləşdirin.
4. NTLM relay üçün **Extended Protection for Authentication (EPA)** tətbiq edin.

**Qeyd:** LLMNR-i yalnız GPO ilə bloklamaq mümkündür; NetBIOS-u isə cihaz səviyyəsində konfiqurasiya və ya GPO ilə skriptləşdirmə tələb olunur.

---

## 4. SOC vəziyyəti: Domain Controller-dan gələn çoxsaylı Event ID 4625 hadisələri

**Sual:** Domain Controller-dan çoxsaylı "Event ID 4625 – An account failed to log on" hadisələri gəlir; müxtəlif istifadəçi hesabları olsa da hamısı eyni IP-dən və qısa vaxt aralığında. Nələr edərsən?
**Cavab (təklif olunan addımlar):**

1. **Triage & ilkin analiz**

   * Hər 4625 hadisəsinin `Account Name`, `Workstation Name`, `Source IP` və `Time` sahələrini çıxar.
2. **Pattern axtarışı**

   * Eyni IP-dən eyni vaxt intervalında fərqli hesablar üzərində uğursuz giriş cəhdləri — bu, brute-force / credential stuffing / relay göstəricisi ola bilər.
3. **Əlavə loglara bax**

   * Əgər SMB-dən başqa LDAP və ya Kerberos ilə bağlı səylər varsa, `Event ID 4771` (Kerberos pre-auth failed) və `4624`(successful) və `4648` (explicit creds) kimi hadisələrə bax.
4. **Kontekst zənginləşdirmə (Enrichment)**

   * Mənbə IP üçün reverse DNS, coğrafi lokasiya, asset owner və asset criticality.
5. **Containment & bloklama**

   * Şübhəli IP-ni firewall / IDS/IPS / WAF səviyyəsində məhdudlaşdır və şübhəli hesabları (və ya IP) müvəqqəti blokla.
6. **Eskalasiyə & IR**

   * Tier-2 / Incident Response komandalarına ötür və lazım gələrsə müvafiq hesablarda parol sıfırlama və MFA tətbiq et.
7. **Post-incident**

   * İnsident root cause təhlili, zəruri yamaların tətbiqi, SIEM qaydalarının və playbook-ların yenilənməsi.

---

## 5. PowerShell versiyaları və hücumların aşkar edilməsi

**Sual:** PowerShell-in köhnə versiyalarını (məsələn v2) istifadə edən hücumları necə aşkar edərsən və qarşısını necə alarsan?
**Cavab (hunting & prevention):**

### Detection / Hunt qaydaları:

* **Proses yaradılması üzrə axtarış:** `powershell.exe` prosesinin command-line parametrlərində `-Version 2`, `-v 2` və ya `-NoProfile -EncodedCommand` kimi şübhəli flag-lərə bax.
* **Event loglar:**

  * Windows PowerShell əmrləri üçün `Event ID 4103`/`4104` (Script Block Logging enabled olduqda).
  * Process Creation üçün `4688` (vs. Sysmon Event ID 1) — command line ilə birlikdə.
* **EncodedCommand / AMSI bypass:** `-EncodedCommand` istifadə edənləri xüsusi qayda ilə tut.

### Prevention tövsiyələri:

* Köhnə PowerShell versiyasını mümkündürsə **deaktiv et** (PowerShell v2 disable).
* **Script Block Logging** və **Module Logging** aktiv et (Group Policy ilə).
* **Constrained Language Mode**, AppLocker/WDAC ilə qeyri-məqbul skriptlərin icrasını məhdudlaşdır.
* EDR ilə PowerShell fəaliyyətini izləyib anomaliyaları bildir.

**Praktik axtarış nümunəsi (process creation):**

```text
powershell.exe ... -Version 2
powershell.exe ... -v 2
powershell.exe -EncodedCommand <...>
```

---

## 6. ACL Evasion Attack — Senari və nə etmək lazımdır

**Senari qısa:**

* `eli` adlı istifadəçi oğurlanıb.
* `eli` `elnur` üzərində parol dəyişmə hüququna malikdir.
* `eli` `elnur`un parolunu dəyişir.
* `elnur` Help Desk qrupunda `GenericWrite` icazəsinə malikdir və `elnur` özünü Help Desk qrupuna daxil etmə hüququna sahibdir.
* Help Desk qrupuna daxil olmaq Information Technology (IT) qrupuna da aidiyət verir.
* IT qrupunun üzvü `aysel` üzərində `GenericAll` (tam nəzarət) hüququna malikdir → nəticədə `eli` → `elnur` → Help Desk → IT → `aysel` üzərindən tam nəzarət və Kerberoast/şifrə dəyişmə mümkün olur.

**Aşkar etmə və müdafiə addımları (SOC və IR üçün):**

### Detection:

1. **ACL dəyişiklikləri üçün audit:** `Advanced Security Audit Policy` aktiv et. Vacib Event ID:

   * `5136` — A directory service object was modified (ACL dəyişikliklərini göstərir).
2. **Group membership dəyişiklikləri:** Event ID `4728/4729/4732/4733` (group add/remove) üçün SIEM qaydaları.
3. **Computer / Account attribute dəyişiklikləri:** 4741/4742 və s.
4. **Anomaliya detection:** Qısa müddətdə yüksək imtiyazların şəbəkə daxilində bir neçə hop ilə ötürülməsi.
5. **Kerberoasting / SPN sorğuları:** Kimsə çox sayda kerberoast-able SPN hesabları üçün sorğu edirsə, bu göstəricidir.

### Preventive / Remedial measures:

1. **ACL-ləri yoxla və düzəlt:** Şübhəli `GenericWrite/GenericAll` icazələrini inzibati qaydada təftiş et və mümkün olduqda least-privilege prinsipi tətbiq et.
2. **Group membership governance:** Help Desk və IT qruplarının üzvlərini və onlara təsir edən delegasiyaları nəzərdən keçir.
3. **Change control və monitoring:** ACL və group membership dəyişiklikləri üçün avtomatik bildirişlər və onay sistemi.
4. **Audit və logging:** `5136` kimi hadisələri korrelyasiya edən qaydalar əlavə et.
5. **MFA və parol siyasətləri:** Yüksək imtiyazlı hesablar üçün MFA məcburi et və parolları gücləndir.
6. **Hesab mütəmadi təmizləmə & least privilege review.**

---

## 7. Müsahibə üçün (Bleeding Edge Vulnerabilities mövzusundan) — Qısa, Texniki və Praktik 10 sual + cavab

Aşağıda NoPac, PrintNightmare və PetitPotam və ümumi detection/prevention ilə bağlı **qısa, texniki və praktik** suallar və gözlənilən cavablar var — SOC analyst müsahibələri üçün istifadə et.

---

### Sual 1 — NoPac hücumunu SIEM-də necə aşkar edərdin?

**Cavab:** AD event-lərindən `4741` (Computer created) və `4742` (Computer renamed) qaydalarını monitorinq et; kompüter adı DC ilə uyğunlaşırsa şübhə doğur. Alert: “Computer account created/renamed by non-admin user”.

---

### Sual 2 — NoPac qarşısında ən vacib önləyici tədbir nədir?

**Cavab:** Microsoft 2021 patch-lərini tətbiq et və `ms-DS-MachineAccountQuota` dəyərini 0 ilə limitlə (adi istifadəçilərin kompüter hesabı yaratma hüququnu qaldır).

---

### Sual 3 — PrintNightmare üçün hansı Windows xidməti kritikdir və nəyi izləmək lazımdır?

**Cavab:** `Print Spooler (spoolsv.exe)` — `7045` (service installed) və `4688` (process creation) event-lərini izləyin; `spoolsv.exe` tərəfindən qeyri-adi DLL yüklənməsi və ya PowerShell çağırışları şübhəlidir.

---

### Sual 4 — PrintNightmare qarşısında əsas əməli tədbir nə ola bilər?

**Cavab:** DC-lərdə Print Spooler xidmətini dayandırın və ya mümkün olmadıqda sürücü yükləmələri üçün hüquqları məhdudlaşdırın; rəsmi KB yamaqlarını tətbiq edin.

---

### Sual 5 — PetitPotam hücumu nə edir və SOC bunu necə görməlidir?

**Cavab:** PetitPotam DC-ni EFSRPC çağırışı ilə NTLM autentifikasiyasına məcbur edərək sertifikat əldə etməyə və PKINIT yolları ilə TGT əldə etməyə imkan verir. SOC: DC-dən xarici hostlara gedən NTLM autentifikasiyasını izləməlidir.

---

### Sual 6 — PetitPotam üçün ən təsirli preventiv tədbir nədir?

**Cavab:** NTLM relay hücumlarına qarşı **Extended Protection for Authentication (EPA)** aktiv et, LDAP/SMB signing tətbiq et, SMBv1 və EFSRPC imkanlarını məhdudlaşdır.

---

### Sual 7 — Hangi Event ID-lər kritikdir (NoPac, PrintNightmare, PetitPotam üçün)?

**Cavab:**

* NoPac: `4741`, `4742`
* PrintNightmare: `7045`, `4688`
* PetitPotam: `4624` (NTLM logon), `4648` (explicit creds), və DC-dən gələn NTLM sorğuları.

---

### Sual 8 — Kerberos biletlərində anomaliyanı necə aşkarlamaq olar?

**Cavab:** Non-admin istifadəçilərin DC və yüksək imtiyazlı xidmətlər üçün TGT/TGS istəməsi anomaliya sayılır — SIEM-də “Unusual Kerberos service ticket request” qaydası qur.

---

### Sual 9 — SIEM qaydaları nümunəsi — nə yaratmalısan?

**Cavab:**

* “User created computer account outside IT OU”
* “Spoolsv.exe spawned child process (PowerShell/cmd.exe)”
* “NTLM authentication from Domain Controller to external host”
* “Unusual Kerberos TGT request by non-admin user”

---

### Sual 10 — Zəiflik aşkarladıqdan sonra SOC analitiki hansı əsas addımları atmalıdır?

**Cavab:** Alert triage → Log correlation → Containment (izolyasiya / bloklama) → Escalation to IR → Post-incident remediation & patch validation.

---

## 8. Əlavə: Faydalı əmrlər və monitoring qaydaları (qısa)

* **PowerShell detection (hunt):**

  ```text
  powershell.exe ... -Version 2
  powershell.exe ... -v 2
  powershell.exe -EncodedCommand <...>
  ```
* **Disable Print Spooler on DCs:**

  ```powershell
  Stop-Service Spooler
  Set-Service Spooler -StartupType Disabled
  ```
* **Check ms-DS-MachineAccountQuota (AD):**

  ```powershell
  Get-ADObject -SearchBase "CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=domain,DC=local" -Filter *
  ```

  (Admin əmrləri AD konfiqurasiyasına görə fərqli ola bilər.)

---

## 9. Nəticə və Tövsiyələr (qısa)

1. **Yamaqları vaxtında tətbiq et.** NoPac, PrintNightmare və PetitPotam üçün rəsmi Microsoft yamalarını prioritetləşdir.
2. **Least privilege** və qrup üzvlüklərinə nəzarət — ACL-ləri və delegation-ları təftiş et.
3. **SIEM ilə proaktiv monitorinq** — yuxarıda göstərilən Event ID-lər, proses yaradılması, Kerberos anomaliyaları üçün qaydalar qur.
4. **Network-level qorunma** — LLMNR/NBT-NS deaktivləşdir, SMB/LDAP signing və EPA aktiv et.
5. **EDR/AV konfiqurasiyası** — suspicious smbexec/printspooler davranışlarını bloklasın və alert versin.
6. **Audit & forensic readiness** — `Advanced Security Audit Policy` aktiv olsun və `5136`, `4741/4742`, `7045`, `4688`, `4624/4625/4771` kimi event-lar saxlanılsın.

---

Əgər istəyirsən, bu sənədi PDF və ya DOCX formatına çevirib yükləmək üçün hazırlaya bilərəm — həmçinin hər bölmə üçün qısa “playbook” (SIEM qaydaları + IR addımları) düzəldə bilərəm. Hansısını istərdin?
