# Faylları və Qovluqları Tapmaq

Ehtiyac duyduğumuz fayl və qovluqları tapa bilmək çox vacibdir. Linux sisteminə daxil olduqdan sonra konfiqurasiya fayllarını, istifadəçilər və ya administrator tərəfindən yaradılmış skriptləri və digər fayl və qovluqları tapmaq vacib olacaq. Hər qovluğu əl ilə gəzib son dəyişiklik tarixini yoxlamağa ehtiyac yoxdur. Bunun üçün işimizi asanlaşdıracaq bir neçə alət mövcuddur.

## Which

Ən çox istifadə olunan alətlərdən biri `which`-dir. Bu alət icra edilməli olan faylın və ya linkin yolunu göstərir. Bu sayədə cURL, netcat, wget, python, gcc kimi proqramların sistemdə olub olmadığını müəyyən edə bilərik. Məsələn, Python-un yerini tapmaq üçün:

```bash
nijatmansimov@htb[/htb]$ which python
/usr/bin/python
```

Əgər axtardığımız proqram mövcud deyilsə, heç bir nəticə göstərilməyəcək.

## Find

Başqa faydalı alət isə `find`-dir. Bu alət fayl və qovluqları tapmaqla yanaşı nəticələri filtrələmək imkanı da verir. Məsələn, faylın ölçüsünə və ya tarixə görə filtr tətbiq edə bilərsiniz. Həmçinin yalnız fayl və ya yalnız qovluq axtara bilərsiniz.

### Sintaksis - find

```bash
nijatmansimov@htb[/htb]$ find <yer> <seçimlər>
```

Məsələn, bir neçə seçim ilə belə bir əmrdən istifadə edə bilərik:

```bash
nijatmansimov@htb[/htb]$ find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null
```

Nəticə nümunəsi:

```
-rw-r--r-- 1 root root 136392 Apr 25 20:29 /usr/src/linux-headers-5.5.0-1parrot1-amd64/include/config/auto.conf
-rw-r--r-- 1 root root 82290 Apr 25 20:29 /usr/src/linux-headers-5.5.0-1parrot1-amd64/include/config/tristate.conf
...
```

### İstifadə olunan seçimlər

| Seçim               | Təsviri                                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------------------- |
| -type f             | Axtarılan obyektin növünü göstərir. Burada `f` fayl deməkdir.                                           |
| -name *.conf        | Axtardığımız faylın adını göstərir. `*` simvolu bütün `.conf` uzantılı faylları əhatə edir.             |
| -user root          | Faylın sahibi `root` olan faylları göstərir.                                                            |
| -size +20k          | Fayllar 20 KiB-dən böyük olmalıdır.                                                                     |
| -newermt 2020-03-03 | Yalnız göstərilən tarixdən yeni fayllar göstərilir.                                                     |
| -exec ls -al {} ;   | Hər nəticəyə əmri tətbiq edir (`{}` hər fayl üçün əvəz olunur). `\;` nöqtəli vergülü shell-dən qoruyur. |
| 2>/dev/null         | STDERR çıxışını "null" cihazına yönləndirir, səhvlər terminalda görünməyəcək.                           |

## Locate

Bütün sistemi `find` ilə axtarmaq çox vaxt aparır. `locate` komandası daha sürətli alternativ təqdim edir. `locate` mövcud fayl və qovluqlar haqqında məlumatları saxlayan lokal verilənlər bazası ilə işləyir. Bu bazanı aşağıdakı əmrlə yeniləyə bilərik:

```bash
nijatmansimov@htb[/htb]$ sudo updatedb
```

Sonra `.conf` uzantılı bütün faylları tapmaq üçün:

```bash
nijatmansimov@htb[/htb]$ locate *.conf
/etc/GeoIP.conf
/etc/NetworkManager/NetworkManager.conf
/etc/UPower/UPower.conf
/etc/adduser.conf
...
```

Lakin `locate` `find` qədər çox filtr seçimlərinə malik deyil. Hər zaman hansı komandadan istifadə etməyimiz lazım olduğunu məqsədimizə görə müəyyən etmək lazımdır.
