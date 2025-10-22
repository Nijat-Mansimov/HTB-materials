Target(s): 10.129.43.69 (ACADEMY-EA-ATTACK01) 

Life Left: 105 minute(s)  
 SSH to 10.129.43.69 (ACADEMY-EA-ATTACK01) with user "htb-student" and password "HTB_@cademy_stdnt!"

## QUESTION
What host is running "Microsoft SQL Server 2019 15.00.2000.00"? (IP address, not Resolved name)

CAVAB:
ilk olaraq butun aktiv hostlari tapiriq ve her birini bir fayla save edirik

```shell
fping -agq 172.16.5.0/23
```

daha sonra butun sebekeye scan atiriq

```shell
sudo nmap -v -A -iL hosts.txt -oN /home/htb-student/Documents/host-enum
```

netice ucun oldugu ucun birbasa asagidaki metni qoyuram

Çox yaxşı! Bu Nmap nəticələrindən əldə edilən **dəyərli məlumatları** aşağıdakı kimi sistemləşdirək:

---

## 1. Şəbəkə Vəziyyəti və Domen Məlumatları

* **Domen:** Hər iki Windows hostu **INLANEFREIGHT.LOCAL** domeninin üzvüdür. Bu, bütün hostların mərkəzi Active Directory (AD) mühitində olduğunu göstərir.
* **Active Directory Server (Domain Controller):**
    * **IP Ünvanı:** **172.16.5.5**
    * **Hostname:** **inlanefreight.local** və ya **ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL**
    * **Təyinatı:** Bu, domen nəzarətçisi (Domain Controller - **DC**) olaraq təyin olunur.
* **İkinci Windows Host:**
    * **IP Ünvanı:** **172.16.5.130**
    * **Hostname:** **ACADEMY-EA-FILE.INLANEFREIGHT.LOCAL**
    * **Təyinatı:** "FILE" adına görə ehtimal ki, bu, fayl və ya digər infrastruktur xidmətlərini (məsələn, SQL Server) idarə edən serverdir.
* **Linux Host:**
    * **IP Ünvanı:** **172.16.5.225**
    * **OS:** **Linux** (OpenSSH 8.4p1 Debian 5)
    * **Təyinatı:** Üzərində SSH və RDP (xrdp) var.

---

## 2. Host Məlumatları üzrə Əsas Portlar və Servislər

### 172.16.5.5 (Domain Controller - ACADEMY-EA-DC01)

Bu host Active Directory funksionallığı üçün kritik servislərə malikdir:

| Port | Servis | Versiya / Məlumat | Əhəmiyyəti |
| :---: | :--- | :--- | :--- |
| **53/tcp** | **domain** | Simple DNS Plus | **DNS** xidməti. Domen adlarının həlli üçün istifadə olunur. |
| **88/tcp** | **kerberos-sec** | Microsoft Windows Kerberos | **Kerberos** kimlik doğrulaması. AD mühitində kəşfiyyat üçün vacibdir. |
| **389/tcp** | **ldap** | Microsoft Windows Active Directory LDAP | **LDAP** xidməti. İstifadəçilər, qruplar və digər AD obyektləri haqqında məlumat almaq üçün əsas giriş nöqtəsi. |
| **445/tcp** | **microsoft-ds** | SMB 3.1.1 (Mesaj imzalanması tələb olunur) | **SMB** (Server Message Block) fayl və printer paylaşımı. |
| **464/tcp** | **kpasswd5** | | Kerberos şifrə dəyişdirmə. |
| **636/tcp** | **ssl/ldap** | Microsoft Windows Active Directory LDAP | **LDAPS** (Şifrəli LDAP). |
| **3268/tcp** | **ldap** | Microsoft Windows Active Directory LDAP | **Global Catalog** LDAP xidməti. |
| **3269/tcp** | **ssl/ldap** | Microsoft Windows Active Directory LDAP | **Global Catalog** LDAPS xidməti. |
| 3389/tcp | ms-wbt-server | Microsoft Terminal Services | **Uzaq Masaüstü (RDP)** girişi. |

**Diqqətəlayiq:** Bu server **Domain Controller**-dır. Bütün AD məlumatları (istifadəçilər, heşlər) buradadır. Xüsusilə **Kerberos (88)** və **LDAP (389, 636, 3268, 3269)** hücumları üçün ilkin hədəfdir (məsələn, AS-REPRoast, Kerberoasting).

### 172.16.5.130 (File Server - ACADEMY-EA-FILE)

Bu host fayl və verilənlər bazası xidmətlərinə malikdir:

| Port | Servis | Versiya / Məlumat | Əhəmiyyəti |
| :---: | :--- | :--- | :--- |
| **80/tcp** | **http** | Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP) | Əsas web server. |
| **1433/tcp** | **ms-sql-s** | Microsoft SQL Server **2019 15.00.2000.00** | Yüksək dəyərli hədəf! Mümkün giriş cəhdləri, verilənlər bazası kəşfiyyatı və ya SQL injeksiya üçün hədəf ola bilər. |
| 808/tcp | ccproxy-http? | | Naməlum xidmət, əlavə kəşfiyyat tələb olunur. |
| 3389/tcp | ms-wbt-server | Microsoft Terminal Services | **Uzaq Masaüstü (RDP)** girişi. |
| 16001/tcp | mc-nmf | .NET Message Framing | Naməlum .NET tətbiqi. |

**Diqqətəlayiq:** **MS SQL Server 2019 (1433/tcp)** - daxil olmaq üçün bir giriş nöqtəsi ola bilər.

### 172.16.5.225 (Linux Host)

| Port | Servis | Versiya / Məlumat | Əhəmiyyəti |
| :---: | :--- | :--- | :--- |
| **22/tcp** | **ssh** | **OpenSSH 8.4p1 Debian 5** | SSH (Secure Shell) uzaqdan idarəetmə. İstifadəçi adları və şifrələrlə Brute-force və ya zəifliklərin axtarışı edilməlidir. |
| 3389/tcp | ms-wbt-server | **xrdp** | Linux üçün RDP serveri. |

---

## 3. Zəifliklər üçün Potensial Sahələr

1.  **Domen Kimlik Doğrulaması:** DC (172.16.5.5) üzərində **Kerberos** və **LDAP** portlarının açıq olması ilə istifadəçi adları və ya şifrə heşləri əldə etmək üçün hücumlar aparıla bilər.
2.  **Köhnəlmiş Sertifikat:** DC-dəki bəzi sertifikatların etibarlılıq müddəti (Not valid after: **2024-10-26**) hazırkı vaxtdan (2025-10-22) keçmişdir. Bu, bir zəiflik olmasa da, səhv konfiqurasiyaya işarə edir.
3.  **SQL Server:** 172.16.5.130 üzərindəki **Microsoft SQL Server 2019** (1433/tcp) zəif şifrələr və ya tətbiq zəiflikləri vasitəsilə kompromisə uğradıla bilər.
4.  **OpenSSH Versiyası:** OpenSSH **8.4p1 Debian 5** versiyası üçün mövcud zəifliklər yoxlanılmalıdır.


## ANSWER: 172.16.5.130

## QUESTION

Ən uyğun və birbaşa hostu təyin edən commonName dəyəri 3389/tcp portundan gəlir:

ACADEMY-EA-DC01.INLANEFREIGHT.LOCAL



