# Naviqasiya

Naviqasiya siçanla işləmək kimi, standart Windows istifadəçisi üçün vacibdir. Bununla biz sistemi boyunca hərəkət edirik, ehtiyac duyduğumuz və istədiyimiz qovluqlarda və fayllarla işləyirik. Buna görə də müəyyən qovluq və ya fayl haqqında məlumat çap etmək üçün müxtəlif əmrlər və alətlərdən istifadə edirik və çıxışı ehtiyacımıza uyğun optimallaşdırmaq üçün qabaqcıl seçimlərdən yarana bilərik.

Yeni bir şeyi öyrənməyin ən yaxşı yollarından biri onunla təcrübə aparmaqdır. Burada Linux boyunca naviqasiya, fayl və qovluqların yaradılması, köçürülməsi, redaktə edilməsi və silinməsi, əməliyyat sistemində onların tapılması, müxtəlif yönləndirmə növləri və fayl təsvirçilərinin (file descriptors) nə olduğu barədə bölmələri əhatə edəcəyik. Həmçinin shell ilə işimizi daha asan və rahat edəcək qısa yolları tapacağıq. Tövsiyə olunur ki, bu təcrübələri lokal VM-də yoxlayasan və sistem gözlənilmədən zədələnərsə istifadə etmək üçün VM üçün snapshot yaratmağı unutma.

İndi naviqasiyaya başlayaq. Sistemi boyunca hərəkət etməzdən əvvəl hansı qovluqda olduğumuzu tapmalıyıq. Bunun üçün `pwd` əmrindən istifadə edirik.

```bash
cry0l1t3@htb[~]$ pwd

/home/cry0l1t3
```

Direktoriyanın məzmununu sadalamaq üçün yalnız `ls` əmri kifayətdir. Bu əmrin cari qovluqdakı məzmunu göstərmək üçün çoxsaylı əlavə seçimləri mövcuddur.

```bash
cry0l1t3@htb[~]$ ls

Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
```

Əlavə seçimsiz `ls` yalnız qovluq və fayl adlarını göstərir. Daha çox məlumat görmək üçün `-l` seçimini əlavə edə bilərik:

```bash
cry0l1t3@htb[~]$ ls -l

total 32
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 Desktop
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Documents
drwxr-xr-x 3 cry0l1t3 htbacademy 4096 Nov 15 03:26 Downloads
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Music
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Pictures
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Public
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Templates
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Videos
```

Birinci sətrdə cari direktoriyadakı fayl və qovluqların istifadə etdiyi blokların (1024 baytlıq) ümumi miqdarı göstərilir. Bu misalda 32 blok × 1024 bayt/blok = 32,768 bayt (və ya 32 KB) diskin istifadə olunduğunu bildirir. Sonrakı sütunlar aşağıdakı kimi strukturlanıb:

| Sütun | Məzmun         | İzah                                                                          |
| ----- | -------------- | ----------------------------------------------------------------------------- |
| 1     | `drwxr-xr-x`   | Tip və icazələr                                                               |
| 2     | `2`            | Fayl/qovluğun sərt bağlantı (hard link) sayı                                  |
| 3     | `cry0l1t3`     | Fayl/qovluğun sahibi                                                          |
| 4     | `htbacademy`   | Fayl/qovluğun qrup sahibi                                                     |
| 5     | `4096`         | Faylın ölçüsü və ya qovluq məlumatını saxlamaq üçün istifadə olunan blok sayı |
| 6     | `Nov 13 17:37` | Tarix və vaxt                                                                 |
| 7     | `Desktop`      | Qovluq və ya faylın adı                                                       |

Qovluqdakı bütün məzmunu görməyə bilərik—adının əvvəlində nöqtə (`.`) ilə başlayan gizli fayllar ola bilər (məsələn, `.bashrc`, `.bash_history`). Bütün faylları göstərmək üçün `ls -la` istifadə etməliyik:

```bash
cry0l1t3@htb[~]$ ls -la

total 403188
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 .bash_history
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 .bashrc
...SNIP...
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 Desktop
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Documents
drwxr-xr-x 3 cry0l1t3 htbacademy 4096 Nov 15 03:26 Downloads
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Music
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Pictures
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Public
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Templates
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Videos
```

Bir direktoriyanın məzmununu göstərmək üçün həmişə ona keçməyə ehtiyac yoxdur; `ls` ilə yol təyin edərək istədiyimiz yeri yoxlaya bilərik:

```bash
cry0l1t3@htb[~]$ ls -l /var/

total 52
drwxr-xr-x  2 root root     4096 Mai 15 18:54 backups
drwxr-xr-x 18 root root     4096 Nov 15 16:55 cache
drwxrwsrwt  2 root whoopsie 4096 Jul 25  2018 crash
drwxr-xr-x 66 root root     4096 Mai 15 03:08 lib
drwxrwsr-x  2 root staff    4096 Nov 24  2018 local
<SNIP>
```

Direktoriyalara keçmək üçün `cd` əmri istifadə olunur. Məsələn, `/dev/shm` qovluğuna keçək. Tam yolu verərək birbaşa ora tullanmaq mümkündür:

```bash
cry0l1t3@htb[~]$ cd /dev/shm

cry0l1t3@htb[/dev/shm]$
```

Əvvəlki qovluğa tez qayıtmaq üçün `cd -` istifadə edə bilərik:

```bash
cry0l1t3@htb[/dev/shm]$ cd -

cry0l1t3@htb[~]$
```

Shell avtomatik tamamlama (autocomplete) funksiyasını təklif edir. Məsələn, `cd /dev/s` yazıb `[TAB]`-a iki dəfə basdıqda `/dev/` içində `s` hərfi ilə başlayan bütün girişlər göstəriləcək:

```bash
cry0l1t3@htb[~]$ cd /dev/s [TAB 2x]

shm/ snd/
```

Əgər `sh` üçün unikal uyğunluq varsa, tamamlama avtomatik olaraq tamamlayacaq və biz `cd /dev/shm` ilə həmin qovluğa keçə bilərik. Qovluğun məzmununu göstərsək:

```bash
cry0l1t3@htb[/dev/shm]$ ls -la /dev/shm

total 0
drwxrwxrwt  2 root root   40 Mai 15 18:31 .
drwxr-xr-x 17 root root 4000 Mai 14 20:45 ..
```

Bir nöqtə (`.`) cari qovluğu, iki nöqtə (`..`) isə valideyn qovluğu göstərir. Valideyn qovluğa keçmək üçün:

```bash
cry0l1t3@htb[/dev/shm]$ cd ..

cry0l1t3@htb[/dev]$
```

Terminalı təmizləmək üçün `clear` əmri istifadə olunur. Məsələn, `/dev/shm`-ə qayıdıb terminalı təmizləyə bilərik:

```bash
cry0l1t3@htb[/dev]$ cd shm && clear
```

Terminalı təmizləməyin başqa yolu isə `Ctrl + L` qısa yoludur. Əvvəl istifadə edilmiş əmrləri görmək üçün ox düymələrindən (↑ və ↓) istifadə edə bilərik. Keçmiş əmrlər arasında axtarış etmək üçün isə `Ctrl + R`-dən istifadə edib axtarmaq istədiyimiz mətni yaza bilərik.
