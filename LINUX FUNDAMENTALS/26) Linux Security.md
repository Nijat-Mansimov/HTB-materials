## Linux Təhlükəsizliyi Əsasları

Bütün kompüter sistemlərində müdaxilə riski var. Linux sistemləri Windows-a təsir edən viruslara qarşı daha az həssas olsa da və Active Directory domenə qoşulmuş hostlar qədər böyük hücum səthi təqdim etməsə də, hər hansı bir Linux sistemini təhlükəsizləşdirmək üçün müəyyən əsasları tətbiq etmək vacibdir.

### Əsas Təhlükəsizlik Tədbirləri

1.  **Sistemi Yeniləmək (Patching) 🔄:** Əməliyyat sistemini və quraşdırılmış paketləri yeniləməklə təhlükəsizlik boşluqlarını aradan qaldırmaq ən vacib addımdır.

    ```bash
    nijatmansimov@htb[/htb]$ apt update && apt dist-upgrade
    ```

    Bəzi kernel versiyalarının əl ilə yenilənməli olduğunu unutmamaq lazımdır.

2.  **Firewall və Şəbəkə Girişinə Nəzarət 🛡️:** Şəbəkə səviyyəsində firewall qaydaları düzgün qurulmayıbsa, hosta daxil olan/çıxan trafikin məhdudlaşdırılması üçün **Linux firewall** və/və ya **`iptables`** istifadə edilə bilər.

3.  **SSH Təhlükəsizliyi:**

      * **Şifrə ilə girişi qadağan etmək** (yalnız SSH açarları ilə girişə icazə vermək).
      * **`root` istifadəçisinin SSH vasitəsilə daxil olmasını qadağan etmək**.
      * **`root` istifadəçisi kimi idarəetməkdən qaçmaq**.

4.  **İmtiyazların İdarə Edilməsi:** İstifadəçi girişi **Ən Az İmtiyaz Prinsipinə** əsaslanmalıdır. İstifadəçiyə tam **`sudo`** hüquqları vermək əvəzinə, əgər bir istifadəçinin `root` kimi bir əmri yerinə yetirməsi lazımdırsa, həmin əmr **`sudoers`** konfiqurasiyasında dəqiq müəyyən edilməlidir.

5.  **Brute-Force Hücumlarından Müdafiə:** **`fail2ban`** kimi alətlər uğursuz giriş cəhdlərinin sayını sayır və maksimum cəhd sayına çatan hostların müvəqqəti bloklanmasını təmin edir.

6.  **Sistem Auditləri:** İmtiyazların artırılmasına (privilege escalation) kömək edə biləcək problemlərin mövcudluğunu yoxlamaq üçün sistemi dövri olaraq yoxlamaq vacibdir. Bunlara köhnəlmiş kernel, istifadəçi icazəsi problemləri, hər kəsə yazma hüququ olan fayllar, səhv konfiqurasiya edilmiş **`cron`** işləri və ya xidmətlər daxildir.

7.  **Kernel Təhlükəsizlik Modulları:** Linux sistemlərini daha da bağlamaq üçün **Security-Enhanced Linux (SELinux)** və ya **AppArmor** kimi **kernel təhlükəsizlik modulları** istifadə edilə bilər.

      * **SELinux**-da hər bir proses, fayl və obyektə bir **etiket** verilir və **kernel** tərəfindən tətbiq edilən siyasət qaydaları bu etiketli proseslər və obyektlər arasında girişi idarə edir. Bu, çox **dəqiq (granular)** giriş nəzarətləri təmin edir.

8.  **Əlavə Təhlükəsizlik Tənzimləmələri:**

      * Bütün **lazımsız xidmətləri və proqram təminatını aradan qaldırmaq** və ya deaktiv etmək.
      * **Şifrələnməmiş identifikasiya** mexanizmlərinə (unencrypted authentication) əsaslanan xidmətləri aradan qaldırmaq.
      * **NTP**-nin aktivləşdirilməsinə və **Syslog**-un işlədiyinə əmin olmaq.
      * Hər bir istifadəçinin öz hesabına sahib olmasını təmin etmək.
      * **Güclü şifrələrin** istifadəsini tətbiq etmək.
      * Şifrələnmə müddətini təyin etmək və əvvəlki şifrələrin istifadəsini məhdudlaşdırmaq.
      * Giriş uğursuzluqlarından sonra istifadəçi hesablarını kilidləmək.
      * İstənməyən **SUID/SGID** ikililərini deaktiv etmək.
      * **Snort**, **chkrootkit**, **rkhunter**, **Lynis** kimi tətbiq və xidmətlərdən istifadə etmək.

Təhlükəsizlik bir məhsul deyil, bir **prosesdir** və sistemlərin daha yaxşı qorunması üçün daima xüsusi addımlar atılmalıdır.

-----

## TCP Wrappers

**TCP wrapper**, Linux sistemlərində sistem administratoruna hansı xidmətlərə sistemə giriş icazəsi verildiyini idarə etməyə imkan verən bir təhlükəsizlik mexanizmidir.

### İşləmə Mexanizmi

TCP wrapper, girişi tələb edən istifadəçinin **hostname** və ya **IP ünvanına** əsaslanaraq müəyyən xidmətlərə girişi məhdudlaşdırır. Bir müştəri bir xidmətə qoşulmağa cəhd etdikdə, sistem əvvəlcə müştərinin IP ünvanını müəyyən etmək üçün TCP wrapper konfiqurasiya fayllarındakı qaydaları yoxlayır.

TCP wrapper aşağıdakı konfiqurasiya fayllarından istifadə edir:

1.  **`/etc/hosts.allow`**: Hansı **xidmətlərin** və **hostların** sistemə girişinə **icazə verildiyini** müəyyən edir.
2.  **`/etc/hosts.deny`**: Hansı **xidmətlərin** və **hostların** sistemə girişinə **icazə verilmədiyini** müəyyən edir.

### Qayda Sıralanması

Qaydalarda sıralanma vacibdir: **tələb olunan xidmətə və hosta uyğun gələn ilk qayda tətbiq edilir.** Sistem əvvəlcə `/etc/hosts.allow` faylını, sonra isə `/etc/hosts.deny` faylını yoxlayır.

| Fayl | Məqsəd | Nümunə |
| :--- | :--- | :--- |
| **`/etc/hosts.allow`** | Girişə **İCAZƏ** verilir. | `sshd : 10.129.14.0/24` (SSH-a yerli şəbəkədən icazə) |
| **`/etc/hosts.deny`** | Giriş **QADAĞAN** edilir. | `ALL : .inlanefreight.com` (Bütün xidmətlərə bu domendən qadağa) |

### Məhdudiyyətlər

TCP wrappers bir **firewall-un əvəzedicisi deyil**, çünki onlar yalnız **xidmətlərə** (servislərə) girişi idarə edə bilirlər, **portlara** yox. Bu, şəbəkə səviyyəsində deyil, tətbiq səviyyəsində işləyən bir nəzarət mexanizmidir.
