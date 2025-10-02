## Şellin Anatomiyası

Hər əməliyyat sisteminin bir **şelli** (shell) var və onunla qarşılıqlı əlaqə qurmaq üçün biz **terminal emulyatoru** (terminal emulator) kimi tanınan bir tətbiqdən istifadə etməliyik. Ən çox rast gəlinən terminal emulyatorlarından bəziləri bunlardır:

| Terminal Emulyatoru | Əməliyyat Sistemi |
| :--- | :--- |
| **Windows Terminal** | Windows |
| **cmder** | Windows |
| **PuTTY** | Windows |
| **kitty** | Windows, Linux və MacOS |
| **Alacritty** | Windows, Linux və MacOS |
| **xterm** | Linux |
| **GNOME Terminal** | Linux |
| **MATE Terminal** | Linux |
| **Konsole** | Linux |
| **Terminal** | MacOS |
| **iTerm2** | MacOS |

Bu siyahı mövcud olan bütün terminal emulyatorlarını əhatə etmir, lakin bəzi diqqətəlayiq olanları özündə birləşdirir. Həmçinin, bu alətlərin bir çoxu açıq mənbəli (open-source) olduğu üçün, onları tərtibatçıların orijinal niyyətlərindən fərqli yollarla müxtəlif əməliyyat sistemlərində quraşdıra bilərik. Lakin bu, bu modulun əhatə dairəsindən kənar bir mövzudur.

İş üçün düzgün terminal emulyatorunu seçmək, əsasən, fərdi və stilistik bir seçimdir və seçdiyimiz Əməliyyat Sistemi ilə tanış olduqca inkişaf edən iş axınlarımızdan (workflows) asılıdır. Buna görə də, bir seçimi digərinə üstün tutduğunuza görə kimsənin özünüzü pis hiss etdirməsinə imkan verməyin. Hədəf sistemlərdə qarşılıqlı əlaqə qurduğumuz terminal emulyatoru, əslində, sistemdə yerli olaraq mövcud olandan asılı olacaq.

### Əmr Dili Tərcüməçiləri (Command Language Interpreters)

İnsan dilini tərcümə edən bir tərcüməçinin danışıq və ya işarə dilini real vaxtda tərcümə etdiyi kimi, **əmr dili tərcüməçisi (command language interpreter)** də istifadəçi tərəfindən verilən təlimatları şərh etmək (interpretasiya etmək) və emal üçün əməliyyat sisteminə tapşırıqları vermək üçün işləyən bir proqramdır. Beləliklə, əmr sətri interfeyslərini müzakirə edərkən bilirik ki, bu, əməliyyat sistemi, terminal emulyatoru tətbiqi və əmr dili tərcüməçisinin bir birləşməsidir.

Çox fərqli əmr dili tərcüməçiləri istifadə edilə bilər, onların bəziləri **MITRE ATT&CK Matrisinin İcra (Execution) texnikalarında** müəyyən edildiyi kimi **şell skript dilləri** və ya **Əmr və Skript Tərcüməçiləri** adlanır. Bu anlayışları başa düşmək üçün proqram təminatı tərtibatçısı olmağımıza ehtiyac yoxdur, lakin nə qədər çox bilsək, zəif sistemləri istismar etməyə və şell sessiyası əldə etməyə çalışarkən bir o qədər uğurlu ola bilərik.

Hər hansı bir sistemdə istifadə edilən əmr dili tərcüməçisini anlamaq bizə hansı əmrləri və skriptləri istifadə etməli olduğumuz barədə də bir fikir verəcək. Gəlin, bu anlayışlardan bəziləri ilə praktikada tanış olaq.

### Terminal Emulyatorları və Şelllər ilə Praktiki Tanışlıq

Gəlin, şellin anatomiyasını daha dərindən araşdırmaq üçün **Parrot OS Pwnbox**-umuzdan istifadə edək. Ekranın yuxarı hissəsindəki **yaşıl** kvadrat işarəsinə klikləyin ki, **MATE** terminal emulyatorunu açsın, sonra təsadüfi bir şey yazın və Enter düyməsini basın.

## Terminal Nümunəsi

<img width="1888" height="1312" alt="image" src="https://github.com/user-attachments/assets/0bbe5026-6e64-4fc7-93c9-4d5d472d4be3" />

İkonu seçən kimi MATE terminal emulator tətbiqi açıldı və bu tətbiq əvvəlcədən komanda dili tərcüməçisi (interpreter) istifadə edəcək şəkildə konfiqurasiya edilmişdi. Bu halda, istifadə olunan dil tərcüməçisini `$` işarəsinə baxaraq anlamaq mümkündür. Bu `$` işarəsi Bash, Ksh, POSIX və digər bir çox shell dillərində istifadə olunur və istifadəçinin komandaları və digər girişləri yaza biləcəyi shell promptunun başlanğıcını göstərir.

Biz təsadüfi bir mətn yazıb `Enter` düyməsini basanda, komanda dili tərcüməçimiz müəyyən edildi. Bu, Bash-in yazdığımız komandayı tanımadığını bizə bildirir. Beləliklə, komand dili tərcüməçilərinin tanıdığı öz komandaları ola biləcəyini görürük.

Dil tərcüməçisini müəyyən etməyin başqa bir yolu isə maşında işləyən prosesləri yoxlamaqdır. Linux-da bunu aşağıdakı komanda ilə edə bilərik:

**`ps` ilə Shell-in təsdiqi**
**Shell-in anatomiyası**

```
nijatmansimov@htb[/htb]$ ps

    PID TTY          TIME CMD
   4232 pts/1    00:00:00 bash
  11435 pts/1    00:00:00 ps
```

Həmçinin, istifadə olunan shell dilini mühit dəyişənlərini (`environment variables`) `env` komandası ilə yoxlayaraq tapa bilərik:

**`env` ilə Shell-in təsdiqi**
**Shell-in anatomiyası**

```
nijatmansimov@htb[/htb]$ env

SHELL=/bin/bash
```

İndi Pwnbox-un ekranının yuxarısında yerləşən mavi kvadrat ikonu seçək.

<img width="2864" height="1454" alt="image" src="https://github.com/user-attachments/assets/648e93b5-a368-436d-aca3-71755a9b00d5" />

Bu ikonu seçmək MATE terminal tətbiqini açır, amma bu dəfə fərqli bir komanda dili tərcüməçisi (interpreter) istifadə edilir. Onları yan‑yana qoyub müqayisə edin.

Nə fərqlər aşkar edə bilərik?
Eyni sistemdə niyə birindən digəri üzərində istifadə edərdik?
Kəşf edə biləcəyimiz saysız‑hesabsız fərqlər və özəlləşdirmələr var. Hər iki terminalda bildiyiniz bəzi komandaları sınayın və çıxışlardakı fərqləri və hansı komandaların tanındığını diqqətdə saxlayın. Əsas dərslərdən biri budur ki, terminal emulatoru müəyyən bir dilə bağlı deyil. Əslində, shell dili dəyişdirilə və sistem inzibatçısının, inkişafçının və ya pentesterin şəxsi üstünlüklərinə, iş axınına və texniki tələblərinə uyğunlaşdırıla bilər.

İndi anlayışımızı sınamaq üçün bəzi çətin suallar var. Bütün cavabları Pwnbox istifadə edərək tapa bilərsiniz.










