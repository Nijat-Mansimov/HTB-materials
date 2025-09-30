# Ehtiyat Nüsxələmə və Bərpa (Backup and Restore)

Linux sistemləri, məlumatların ehtiyat nüsxələnməsi və bərpası üçün həm səmərəli, həm də təhlükəsiz olmaq üçün nəzərdə tutulmuş bir sıra güclü alətlər təqdim edir. Bu alətlər məlumatlarımızın yalnız itki və ya korrupsiyadan qorunmasını deyil, həm də bizə lazım olduqda asanlıqla əlçatan olmasını təmin etməyə kömək edir.

Ubuntu sistemində məlumatların ehtiyat nüsxəsini çıxararkən, bir neçə seçimimiz var, o cümlədən:

  * **Rsync**
  * **Deja Dup**
  * **Duplicity**

**Rsync** həm yerli, həm də uzaq bir yerə sürətli və təhlükəsiz ehtiyat nüsxələməyə imkan verən açıq mənbəli bir alətdir. Onun əsas üstünlüklərindən biri, yalnız dəyişdirilmiş fayl hissələrini köçürməsi, bu da böyük miqdarda məlumatla işləyərkən onu yüksək dərəcədə səmərəli edir. Rsync, xüsusilə serverlər arasında faylları sinxronizasiya etmək və ya internet üzərindən **inkremental ehtiyat nüsxələri** yaratmaq kimi şəbəkə köçürmələri üçün faydalıdır.

**Duplicity** Rsync üzərində qurulmuş, lakin ehtiyat nüsxələrini qorumaq üçün **şifrələmə xüsusiyyətləri** əlavə edən başqa bir güclü alətdir. O, ehtiyat nüsxə nüsxələrinizi şifrələməyə imkan verir, hətta uzaq serverlərdə, FTP saytlarında və ya Amazon S3 kimi bulud xidmətlərində saxlanılsa belə, həssas məlumatların təhlükəsiz qalmasını təmin edir. Duplicity, Rsync-in səmərəli məlumat köçürmə qabiliyyətlərini qoruyarkən əlavə bir təhlükəsizlik qatı təmin edir.

Məlumatlarınızı bir evdə saxlanan dəyərli xəzinələr kimi düşünün. Rsync, Duplicity və Deja Dup kimi Linux-dakı ehtiyat nüsxələmə alətləri müxtəlif növ **seyflər** kimi fəaliyyət göstərir. **Rsync**, yalnız yeni və ya dəyişdirilmiş olanları daşıyan, uzaq bir zirzəmiyə yeniləmələri göndərmək üçün ideal bir yol olan sürətli hərəkət edən bir nəqliyyat kimidir. **Duplicity**, yalnız xəzinəni saxlamayan, həm də onu mürəkkəb bir kodla kilidləyən, başqa heç kimin daxil ola bilməyəcəyini təmin edən yüksək təhlükəsizlikli bir seyfdir. **Deja Dup** isə eyni səviyyədə qoruma təklif edərkən, hər kəsin işlədə biləcəyi sadə, əlçatan bir seyfdir. Ehtiyat nüsxələrinizi şifrələmək, seyfinizə əlavə bir kilid əlavə edir, hətta kimsə onu tapsa belə, içəri girə bilməyəcəyini təmin edir.

Daha sadə, daha istifadəçi dostu bir seçimi üstün tutan istifadəçilər üçün **Deja Dup** ehtiyat nüsxələmə prosesini asanlaşdıran **qrafik interfeys** təklif edir. Pərdə arxasında, o da Rsync istifadə edir və Duplicity kimi, şifrələnmiş ehtiyat nüsxələrini dəstəkləyir. Deja Dup, əmr sətrinə girmədən ehtiyat nüsxələmə və bərpa seçimlərinə sürətli, asan giriş istəyən istifadəçilər üçün idealdır.

Ehtiyat nüsxələrinizin təhlükəsizliyini təmin etmək, onları yaratmaq qədər vacibdir. Ehtiyat nüsxə məlumatlarınızı **şifrələmək**, onları icazəsiz girişdən qorumağa kömək edir, həssas məlumatların qorunub saxlanılmasına əminlik verir. Ubuntu sistemlərində, ehtiyat nüsxələrinizə əlavə bir qoruma qatı əlavə etmək üçün **GnuPG**, **eCryptfs** və ya **LUKS** kimi əlavə şifrələmə alətlərindən istifadə edə bilərsiniz.

Məlumatların ehtiyat nüsxələnməsi və bərpası, Ubuntu ilə işləyən hər kəs üçün vacib bir praktikadır. **Rsync**, **Duplicity** və **Deja Dup** kimi alətləri istifadə edərək, məlumatlarınızın təhlükəsiz saxlanmasını və asanlıqla bərpa edilməsini təmin edə bilərsiniz, bu da gözlənilməz bir məlumat itkisi vəziyyətində məlumatlarınızın tez və etibarlı şəkildə bərpa edilə biləcəyinə inam verir.

-----

## Rsync ilə İşləmək

Ubuntu-da Rsync-i quraşdırmaq üçün **`apt`** paket menecerini istifadə edə bilərik:

### Rsync Quraşdırmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt install rsync -y
```

Bu, Rsync-in ən son versiyasını sistemdə quraşdıracaq. Quraşdırma tamamlandıqdan sonra, məlumatların ehtiyat nüsxəsini çıxarmaq və bərpa etmək üçün aləti istifadə etməyə başlaya bilərik. Rsync istifadə edərək bütün bir qovluğun ehtiyat nüsxəsini çıxarmaq üçün aşağıdakı əmri istifadə edə bilərik:

### Rsync - Yerli Qovluğun Ehtiyat Nüsxəsini Uzaq Serverə Çıxarmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ rsync -av /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

Bu əmr bütün qovluğu (`/path/to/mydirectory`) bir uzaq hosta (`backup_server`) `/path/to/backup/directory` qovluğuna kopyalayacaq. **Arxiv** (`-a`) seçimi icazələr, zaman möhürləri və s. kimi orijinal fayl atributlarını qorumaq üçün istifadə olunur və **ətraflı** (`-v`) seçimindən istifadə etmək `rsync` əməliyyatının gedişatı haqqında ətraflı çıxış verir.

Ehtiyat nüsxələmə prosesini fərdiləşdirmək üçün sıxılma və inkremental ehtiyat nüsxələmə kimi əlavə seçimlər də əlavə edə bilərik. Bunu aşağıdakı kimi edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ rsync -avz --backup --backup-dir=/path/to/backup/folder --delete /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

Bununla biz `mydirectory` qovluğunun ehtiyat nüsxəsini uzaq `backup_server`-ə çıxarırıq, orijinal fayl atributlarını, zaman möhürlərini və icazələri qoruyuruq və daha sürətli köçürmələr üçün **sıxılmanı** (`-z`) aktivləşdiririk. **`--backup`** seçimi `/path/to/backup/folder` qovluğunda **inkremental ehtiyat nüsxələri** yaradır və **`--delete`** seçimi artıq mənbə qovluğunda mövcud olmayan faylları uzaq hostdan silir.

Əgər qovluğumuzu ehtiyat nüsxə serverimizdən yerli qovluğumuza **bərpa etmək** istəyiriksə, aşağıdakı əmri istifadə edə bilərik:

### Rsync - Ehtiyat Nüsxəsini Bərpa Etmək

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ rsync -av user@remote_host:/path/to/backup/directory /path/to/mydirectory
```

-----

## Şifrələnmiş Rsync (Encrypted Rsync)

Yerli hostumuzla ehtiyat nüsxə serverimiz arasındakı **`rsync`** fayl köçürməsinin təhlükəsizliyini təmin etmək üçün **SSH** və digər təhlükəsizlik tədbirlərinin istifadəsini birləşdirə bilərik. SSH istifadə edərək, məlumatlarımız köçürülərkən onu şifrələyə bilərik, bu da icazəsiz şəxslərin ona daxil olmasını çox çətinləşdirir. Əlavə olaraq, köçürmə zamanı məlumatlarımızın təhlükəsiz və etibarlı saxlanmasını təmin etmək üçün **firewall-lar** və digər təhlükəsizlik protokollarını da istifadə edə bilərik. Bu addımları atmaqla, məlumatlarımızın qorunduğuna və fayl köçürməmizin təhlükəsiz olduğuna əmin ola bilərik. Buna görə də, `rsync`-ə aşağıdakı kimi **SSH** istifadə etməsini söyləyirik:

### Ehtiyat Nüsxəmizin Təhlükəsiz Köçürülməsi

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

Yerli hostumuzla ehtiyat nüsxə serverimiz arasındakı məlumat köçürməsi **şifrələnmiş SSH əlaqəsi** üzərindən baş verir, bu da köçürülən məlumatlar üçün **məxfiliyi və bütövlüyü** qorunmasını təmin edir. Bu şifrələmə prosesi, məlumatların icazəsiz daxil ola və dəyişdirə biləcək hər hansı bir potensial zərərli aktyorlardan qorunmasını təmin edir. Şifrələmə açarının özü də hərtərəfli təhlükəsizlik protokolları ilə qorunur, bu da icazəsiz şəxslərin məlumatlara giriş əldə etməsini daha da çətinləşdirir. Əlavə olaraq, şifrələnmiş əlaqə təhlükəsizliyin pozulmasına yönəlmiş hər hansı bir cəhdə qarşı yüksək müqavimətli olmaq üçün nəzərdə tutulmuşdur, bu da köçürülən məlumatların qorunmasına inamımızı artırır.

-----

## Avtomatik Sinxronizasiya (Auto-Synchronization)

**Rsync** istifadə edərək **avtomatik sinxronizasiyanı** aktivləşdirmək üçün, sinxronizasiya prosesini avtomatlaşdırmaq üçün **`cron`** və **`rsync`**-in birləşməsindən istifadə edə bilərsiniz. `cron` işinin müntəzəm intervallarla işləməsi üçün planlaşdırılması, iki sistemin məzmununun sinxronizasiyada saxlanılmasını təmin edir. Bu, xüsusilə məlumatlarını birdən çox maşında sinxronizasiya etməli olan təşkilatlar üçün faydalı ola bilər. Bundan əlavə, `rsync` ilə avtomatik sinxronizasiyanın qurulması, əl ilə sinxronizasiyaya ehtiyacı aradan qaldırdığı üçün vaxt və səyə qənaət etmək üçün əla bir yol ola bilər. O, həmçinin sistemlərdə saxlanılan faylların və məlumatların **güncəl və ardıcıl** saxlanmasını təmin etməyə kömək edir, bu da səhvləri azaltmağa və səmərəliliyi artırmağa kömək edir.

Buna görə də, yerli qovluğumuzu uzaq qovluqla sinxronizasiya etmək üçün `rsync` əmrini işə salacaq **`RSYNC_Backup.sh`** adlı yeni bir skript yaradırıq. Lakin, `rsync` əlaqəsi üçün SSH-i yerinə yetirmək üçün bir skript istifadə etdiyimiz üçün, **açar əsaslı autentifikasiyanı** konfiqurasiya etməliyik. Bu, SSH ilə qoşularkən parolumuzu daxil etmək ehtiyacını aradan qaldırmaq üçündür.

Əvvəlcə, istifadəçimiz üçün bir açar cütü yaradırıq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ssh-keygen -t rsa -b 2048
```

Yeri (defolt olaraq `~/.ssh/id_rsa`) göstərmək və isteğe bağlı olaraq bir parol (parol olmaması üçün boş buraxın) təqdim etmək üçün təlimatlara əməl edin. Sonra, **ictimai açarımızı** uzaq serverə kopyalamalıyıq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ ssh-copy-id user@backup_server
```

İndi, `rsync` ehtiyat nüsxəsini avtomatlaşdırmaq üçün skriptimizi yarada bilərik.

**RSYNC\_Backup.sh**

```bash
#!/bin/bash
rsync -avz -e ssh /path/to/mydirectory user@backup_server:/path/to/backup/directory
```

Sonra, skriptin düzgün icra edilə biləcəyini təmin etmək üçün lazımi **icazələri** verməliyik. Əlavə olaraq, skriptin düzgün istifadəçiyə aid olduğundan əmin olmaq da vacibdir, çünki bu, yalnız düzgün istifadəçinin skriptə daxil olmasını və skriptin hər hansı başqa bir istifadəçi tərəfindən dəyişdirilməməsini təmin edəcək.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ chmod +x RSYNC_Backup.sh
```

Bundan sonra, `cron`-a hər saatın 0-cı dəqiqəsində skripti işə salmağı söyləyən bir **`crontab`** yarada bilərik.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ crontab -e
```

Ehtiyaclarımıza uyğun olaraq vaxtı tənzimləyə bilərik. Bunu etmək üçün, `crontab` aşağıdakı məzmuna ehtiyac duyur:

### Avtomatik Sinxronizasiya - Crontab

Müntəzəm İfadələr

```bash
0 * * * * /path/to/RSYNC_Backup.sh
```

Bu quraşdırma ilə **`cron`** skripti istənilən intervalda icra etməkdən, `rsync` əmrinin işləməsini və yerli qovluğun məzmununun uzaq hostla sinxronizasiya edilməsini təmin edəcək.

Sizi **Pwnbox** istifadə edərək rsync-i sınamağa təşviq edirik. Faylları uzaq bir serverlə sinxronizasiya etmək əvəzinə, sınağı asanlaşdıran Pwnbox-u həm mənbə, həm də təyinat yeri kimi istifadə edə bilərsiniz. Bunu etmək üçün Pwnbox-da iki qovluq yaradın:

1.  **`to_backup`** (orijinal məlumatlarınızın saxlandığı yer) və
2.  **`synced_backup`** (sinxronizasiya edilmiş məlumatların kopyalanacağı yer)

Daha sonra **`rsync`** istifadə edərək məlumatları **`to_backup`** qovluğundan **`synced_backup`** qovluğuna köçürəcəksiniz. Bu prosesi avtomatlaşdırmaq üçün, davamlı sinxronizasiyanı təmin etmək üçün hər dəqiqə işləyən bir **`cron`** işi qurun. Unutmayın, bunu yerli olaraq sınaqdan keçirdiyimiz üçün, "uzaq" host üçün ünvan kimi **loopback IP ünvanı 127.0.0.1** istifadə edə bilərik.
