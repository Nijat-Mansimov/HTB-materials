# Firewall-un Qurulması

Firewall-ların əsas məqsədi daxili və xarici şəbəkələr və ya müxtəlif şəbəkə zonaları kimi fərqli şəbəkə seqmentləri arasında **şəbəkə trafikini idarə etmək və monitorinq etmək** üçün təhlükəsizlik mexanizmi təmin etməkdir. Firewall-lar kompüter şəbəkələrini icazəsiz girişdən, zərərli trafikdən və digər təhlükəsizlik təhdidlərindən qorumaqda mühüm rol oynayır. Serverlərdə və digər şəbəkə cihazlarında istifadə olunan populyar əməliyyat sistemi olan Linux, şəbəkə trafikini idarə etmək üçün **daxili firewall imkanları** təmin edir. Başqa sözlə, onlar icazəsiz girişi önləmək və təhlükəsizlik təhdidlərini azaltmaq üçün əvvəlcədən müəyyən edilmiş qaydalar, protokollar, portlar və digər meyarlar əsasında gələn və gedən trafiki filtrləyə bilərlər.

Linux firewall-larının tarixindən bir nümunə, əvvəlki `ipchains` və `ipfwadm` alətlərini əvəz edən **`iptables`** alətinin inkişafıdır. `iptables` utility ilk dəfə 2000-ci ildə Linux 2.4 kernelində təqdim edildi və şəbəkə trafikini filtrləmək üçün çevik və səmərəli bir mexanizm təmin etdi. `iptables` Linux sistemləri üçün **de facto standart firewall həlli** oldu.

Linux-da firewall funksionallığı adətən kernelin ayrılmaz hissəsi olan **Netfilter framework** vasitəsilə həyata keçirilir. Netfilter sistemdən keçən şəbəkə trafikini ələ keçirmək və dəyişdirmək üçün istifadə edilə bilən bir sıra **hook-lar** təqdim edir. `iptables` utility isə Linux sistemlərində firewall qaydalarını konfiqurasiya etmək üçün geniş istifadə olunur.

## Iptables

`iptables` utility mənbə və təyinat IP ünvanları, port nömrələri, protokollar və daha çox kimi müxtəlif meyarlar əsasında şəbəkə trafikini filtrləmək üçün çevik qaydalar dəsti təqdim edir.

`iptables` ilə yanaşı, **`nftables`**, **`ufw`** (Uncomplicated Firewall) və **`firewalld`** kimi digər həllər də mövcuddur. `nftables` `iptables` üzərində daha müasir sintaksis və təkmilləşdirilmiş performans təmin edir. `UFW` firewall qaydalarının konfiqurasiyası üçün sadə və istifadəçi dostu interfeys təqdim edir. `FirewallD` isə mürəkkəb firewall konfiqurasiyalarını idarə etmək üçün dinamik və çevik həll təmin edir.

`iptables`-in əsas komponentləri bunlardır:

| Komponent | Təsvir |
| :--- | :--- |
| **Cədvəllər (Tables)** | Firewall qaydalarını təşkil etmək və kateqoriyalara ayırmaq üçün istifadə olunur. |
| **Zəncirlər (Chains)** | Xüsusi bir şəbəkə trafik növünə tətbiq olunan bir sıra firewall qaydalarını qruplaşdırmaq üçün istifadə olunur. |
| **Qaydalar (Rules)** | Şəbəkə trafikini filtrləmək üçün meyarları və meyarlara uyğun gələn paketlər üçün görüləcək tədbirləri müəyyən edir. |
| **Uyğunluqlar (Matches)** | Mənbə və ya təyinat IP ünvanları, portlar, protokollar və daha çox kimi xüsusi meyarlara uyğun gəlmək üçün istifadə olunur. |
| **Hədəflər (Targets)** | Xüsusi bir qaydaya uyğun gələn paketlər üçün görüləcək tədbiri müəyyən edir (məsələn, paketi qəbul etmək, atmaq və ya rədd etmək). |

### Cədvəllər (Tables)

`iptables`-də cədvəllər, işlənməli olduqları trafik növünə əsasən firewall qaydalarını kateqoriyalara ayırmaq üçün istifadə olunur:

| Cədvəlin Adı | Təsvir | Daxili Zəncirlər |
| :--- | :--- | :--- |
| **`filter`** | IP ünvanları, portlar və protokollar əsasında **şəbəkə trafikini filtrləmək** üçün istifadə olunur. | `INPUT`, `OUTPUT`, `FORWARD` |
| **`nat`** | Şəbəkə paketlərinin **mənbə və ya təyinat IP ünvanlarını dəyişdirmək** (NAT) üçün istifadə olunur. | `PREROUTING`, `POSTROUTING` |
| **`mangle`** | Şəbəkə paketlərinin **başlıq sahələrini dəyişdirmək** üçün istifadə olunur. | `PREROUTING`, `OUTPUT`, `INPUT`, `FORWARD`, `POSTROUTING` |

Daxili cədvəllərə əlavə olaraq, `iptables` xüsusi paket emal variantlarını konfiqurasiya etmək üçün istifadə olunan **`raw`** cədvəlini də təmin edir.

### Zəncirlər (Chains)

Zəncirlər şəbəkə trafikinin necə filtrlənməli və ya dəyişdirilməli olduğunu müəyyən edən qaydaları təşkil edir. İki növ zəncir var: **Daxili zəncirlər** və **İstifadəçi tərəfindən müəyyən edilmiş zəncirlər**.

**Daxili zəncirlər** cədvəl yaradıldıqda avtomatik olaraq yaradılır:

  * **`filter`** cədvəli: **`INPUT`** (gələn trafik), **`OUTPUT`** (gedən trafik) və **`FORWARD`** (başqa interfeyslərə yönləndirilən trafik).
  * **`nat`** cədvəli: **`PREROUTING`** (routing cədvəli emal etməzdən əvvəl gələn paketlərin təyinat IP-ni dəyişdirmək) və **`POSTROUTING`** (routing cədvəli emal etdikdən sonra gedən paketlərin mənbə IP-ni dəyişdirmək).

**İstifadəçi tərəfindən müəyyən edilmiş zəncirlər** (User-defined chains) xüsusi meyarlara əsasən firewall qaydalarını qruplaşdıraraq qayda idarəetməsini sadələşdirə bilər və əsas üç cədvəlin hər hansı birinə əlavə oluna bilər.

### Qaydalar (Rules) və Hədəflər (Targets)

`iptables` qaydaları şəbəkə trafikini filtrləmək üçün meyarları və meyarlara uyğun gələn paketlər üçün görüləcək tədbirləri müəyyən etmək üçün istifadə olunur. Qaydalar **`-A`** seçimi ilə zəncir adına əlavə edilir.

Hər bir qayda bir sıra meyarlardan (uyğunluqlar) və uyğun gələn paketlər üçün görüləcək tədbiri müəyyən edən bir **hədəfdən (target)** ibarətdir.

| Hədəfin Adı | Təsvir |
| :--- | :--- |
| **`ACCEPT`** | Paketin firewall-dan keçməsinə və təyinatına davam etməsinə icazə verir. |
| **`DROP`** | Paketi atır, onun firewall-dan keçməsini effektiv şəkildə bloklayır. |
| **`REJECT`** | Paketi atır və mənbə ünvanına paketin bloklandığını bildirən **xəta mesajı** göndərir. |
| **`LOG`** | Paket məlumatlarını sistem log-a yazır. |
| **`SNAT`** | Paketin **mənbə IP ünvanını dəyişdirir** (adətən NAT üçün istifadə olunur). |
| **`DNAT`** | Paketin **təyinat IP ünvanını dəyişdirir** (adətən port yönləndirmə üçün istifadə olunur). |
| **`MASQUERADE`** | `SNAT`-a bənzəyir, lakin mənbə IP ünvanı sabit olmadıqda istifadə olunur (məsələn, dinamik IP ssenarilərində). |

**Qayda Nümunəsi:** Port 22 (SSH) üzərindən gələn TCP trafikini qəbul edən bir giriş əlavə etmək:

```bash
nijatmansimov@htb[/htb]$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### Uyğunluqlar (Matches)

**Uyğunluqlar**, bir firewall qaydasının müəyyən bir paketə və ya əlaqəyə tətbiq edilib-edilməyəcəyini müəyyən edən meyarları göstərmək üçün istifadə olunur.

| Uyğunluğun Adı | Təsvir |
| :--- | :--- |
| **`-p`** və ya **`--protocol`** | Uyğunlaşdırılacaq protokolu göstərir (məsələn, `tcp`, `udp`, `icmp`). |
| **`--dport`** | Uyğunlaşdırılacaq **təyinat portunu** göstərir. |
| **`--sport`** | Uyğunlaşdırılacaq **mənbə portunu** göstərir. |
| **`-s`** və ya **`--source`** | Uyğunlaşdırılacaq **mənbə IP ünvanını** göstərir. |
| **`-d`** və ya **`--destination`** | Uyğunlaşdırılacaq **təyinat IP ünvanını** göstərir. |
| **`-m state`** | Əlaqənin vəziyyətinə uyğun gəlir (məsələn, `NEW`, `ESTABLISHED`, `RELATED`). |
| **`-m multiport`** | Bir neçə porta və ya port diapazonuna uyğun gəlir. |
| **`-m iprange`** | IP ünvanlarının bir diapazonuna uyğun gəlir. |

Uyğunluqlar ümumiyyətlə `iptables`-də **`-m`** seçimi ilə göstərilir. Məsələn, **`-m tcp`** TCP paketlərinə uyğun gəlmək və əlavə TCP-yə xas seçimləri daxil etmək üçün istifadə olunur.

**Uyğunluq Nümunəsi:** `filter` cədvəlindəki `INPUT` zəncirinə gələn TCP trafikini port 80 üzərindən qəbul edən bir qayda əlavə etmək:

```bash
nijatmansimov@htb[/htb]$ sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
```
