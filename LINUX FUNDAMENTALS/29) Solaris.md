## Solaris Əməliyyat Sistemi və Linux Distributivləri ilə Fərqləri

**Solaris**, 1990-cı illərdə Sun Microsystems (daha sonra Oracle Corporation tərəfindən alınmış) tərəfindən hazırlanmış **Unix əsaslı** bir əməliyyat sistemidir. O, **möhkəmliyi, miqyaslana bilməsi** və yüksək səviyyəli aparat və proqram təminatına dəstəyi ilə tanınır. Solaris bank, maliyyə və dövlət sektorları kimi **missiya-kritik tətbiqlər** üçün geniş istifadə olunur.

### Solaris-in Əsas Xüsusiyyətləri

* **Mülkiyyətli (Proprietary):** Oracle Corporation-a məxsusdur və əksər Linux distributivləri kimi **açıq mənbəli deyil**.
* **Hypervisor:** **Oracle VM Server for SPARC** daxili hypervisor-a malikdir.
* **Fayl Sistemi (ZFS):** Linux distributivləri tərəfindən də istifadə olunsa da, Solaris-də əsas fayl sistemlərindən biridir (məlumat sıxılması, snapshot-lar və yüksək miqyaslanma).
* **Xidmət İdarəetməsi:** Sistem xidmətləri üçün daha yaxşı etibarlılıq və əlçatanlıq təmin edən **Service Management Facility (SMF)** istifadə edir.
* **Paket İdarəetməsi:** **Image Packaging System (IPS)** paket menecerindən istifadə edir.
* **Təhlükəsizlik:** Bütün Linux distributivlərində olmayan **Rola Əsaslanan Giriş Nəzarəti (RBAC)** və məcburi giriş nəzarətləri kimi qabaqcıl təhlükəsizlik xüsusiyyətləri təmin edir.

***

## Linux Distributivləri ilə Əsas Fərqlər

Solaris və Linux arasındakı əsas fərqlər aşağıdakı kateqoriyalara bölünür:

| Kateqoriya | Solaris | Linux Distributivləri (Ubuntu) |
| :--- | :--- | :--- |
| **Mənbə Kodu** | Mülkiyyətli, qapalı mənbə. | Əsasən açıq mənbə. |
| **Sistem Məlumatı** | **`showrev -a`** (patch səviyyəsi, aparat təchizatçısı kimi ətraflı məlumat). | **`uname -a`** (əsas kernel və OS məlumatı). |
| **Paket İdarəetməsi** | **`pkgadd`** (Solaris Package Manager - SPM). | **`apt-get`** (Advanced Packaging Tool - APT). |
| **İmtiyaz İdarəetməsi**| Əsasən **RBAC** (Solaris 11-dən bəri `sudo` dəstəyi var). | Əsasən **`sudo`** (superuser do). |
| **İcazələr (find)** | **`find / -perm -4000`** (İcazə dəyərindən əvvəl **`-`** istifadə olunur). | **`find / -perm 4000`** |
| **Proses Xəritələşdirməsi** | **`pfiles \`pgrep httpd\``** (Prosesin açdığı faylları siyahılayır). | **`lsof -c apache2`** (Prosesin açdığı faylları/soketləri siyahılayır). |
| **Sistem Çağırışları** | **`truss`** (Uşaq proseslərinin siqnallarını və sistem çağırışlarını izləyə bilər). | **`strace`** (Yalnız əsas prosesin sistem çağırışlarını izləyə bilər). |

***

### Fərqli Əmrlərə Nümunələr

#### Paket Quraşdırılması

* **Ubuntu:** `sudo apt-get install apache2`
* **Solaris:** `$ pkgadd -d SUNWapchr`

#### İcazə Axtarışı (SUID)

* **Ubuntu:** `$ find / -perm 4000`
* **Solaris:** `$ find / -perm -4000` (Solaris-də fərqli icazə sistemi səbəbindən **`-`** istifadə olunur).

#### NFS Paylaşımı

Solaris-də NFS serveri **`share`** əmri ilə konfiqurasiya edilir.
* **Paylaşım:** `$ share -F nfs -o rw /export/home`
* **Montaj (Mount):** `mount -F nfs 10.129.15.122:/nfs_share /mnt/local` (Linux-dakı montaj əmri ilə oxşardır, lakin **`-F nfs`** tələb olunur).
* **Konfiqurasiya faylı:** `/etc/dfs/dfstab`

#### Proses İzlənməsi

* **Linux (`lsof`):** `$ sudo lsof -c apache2`
* **Solaris (`pfiles`):** `$ pfiles \`pgrep httpd\``

#### Sistem Çağırışı İzlənməsi

* **Linux (`strace`):** `$ sudo strace -p \`pgrep apache2\``
* **Solaris (`truss`):** `$ truss ls`
