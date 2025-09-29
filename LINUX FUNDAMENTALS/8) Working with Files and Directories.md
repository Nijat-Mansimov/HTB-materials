# Fayllar və Qovluqlarla İşləmək

Linux-da fayllarla işləmək və Windows-da işləmək arasındakı əsas fərq, həmin fayllara necə daxil olmağımız və onları necə idarə etməyimizdədir. Windows-da adətən Explorer kimi qrafik alətlərdən istifadə edərək faylları tapır, açır və redaktə edirik. Linux-da isə terminal güclü alternativ təqdim edir; fayllara birbaşa əmrlərlə daxil olmaq və onları redaktə etmək mümkündür. Bu üsul təkcə daha sürətli deyil, həm də daha səmərəlidir, çünki faylları interaktiv şəkildə redaktə etmək üçün vim və ya nano kimi redaktorlar tələb olunmur.

Terminalın səmərəliliyi bir neçə əmrlə fayllara daxil olma qabiliyyətindən irəli gəlir və faylları seçici şəkildə dəyişdirmək üçün müntəzəm ifadələrdən (regex) istifadə etməyə imkan verir. Həmçinin, bir neçə əmri eyni anda işə sala, çıxışı fayllara yönləndirə və toplu redaktə işlərini avtomatlaşdıra bilərik. Bu üsul qrafik interfeys vasitəsi ilə işləməyə nisbətən çox vaxt qazandırır və iş prosesini optimallaşdırır.

İndi fayllar və qovluqlarla işləməyi öyrənəcəyik ki, əməliyyat sistemimizdəki məzmunu effektiv idarə edə bilək.

## Yaratmaq, Köçürmək və Kopyalamaq

Əvvəlcə faylları yaratmaq, adını dəyişdirmək, köçürmək, kopyalamaq və silmək kimi əsas əməliyyatları öyrənək. Bu əmrləri işə salmazdan əvvəl hədəfə SSH ilə qoşulmalıyıq (bölmənin sonunda verilən qoşulma təlimatlarına baxın). Gəlin yeni fayl və ya qovluq yaratmaqla başlayaq.

### Sintaksis - touch

```bash
nijatmansimov@htb[/htb]$ touch <ad>
```

### Sintaksis - mkdir

```bash
nijatmansimov@htb[/htb]$ mkdir <ad>
```

Nümunə: `info.txt` adlı fayl və `Storage` adlı qovluq yaradaq.

#### Boş Fayl Yaratmaq

```bash
nijatmansimov@htb[/htb]$ touch info.txt
```

#### Qovluq Yaratmaq

```bash
nijatmansimov@htb[/htb]$ mkdir Storage
```

Əgər bir qovluğun içində bir neçə qovluq yaratmaq lazımdırsa, `mkdir` əmrinə `-p` (parents) seçimini əlavə etmək kifayətdir:

```bash
nijatmansimov@htb[/htb]$ mkdir -p Storage/local/user/documents
```

Parent qovluqları yaratdıqdan sonra `tree` aləti ilə bütün strukturu görə bilərik:

```bash
nijatmansimov@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            └── documents

4 directories, 1 file
```

Faylları müəyyən qovluqlarda birbaşa yaratmaq üçün tam yolu göstərə bilərik. Cari qovluqdan başlamaq üçün `.` istifadə etmək rahatdır.

```bash
nijatmansimov@htb[/htb]$ touch ./Storage/local/user/userinfo.txt
```

```bash
nijatmansimov@htb[/htb]$ tree .

.
├── info.txt
└── Storage
    └── local
        └── user
            ├── documents
            └── userinfo.txt

4 directories, 2 files
```

### Faylları Köçürmək və Adını Dəyişdirmək

`mv` əmri faylları və qovluqları həm köçürmək, həm də adını dəyişdirmək üçün istifadə olunur.

```bash
nijatmansimov@htb[/htb]$ mv <fayl/qovluq> <yeni ad/fayl/qovluq>
```

Faylın adını `info.txt`-dən `information.txt`-ə dəyişdirək və sonra `Storage` qovluğuna köçürək:

```bash
nijatmansimov@htb[/htb]$ mv info.txt information.txt
```

Cari qovluqda `readme.txt` faylını yaradaq və sonra `information.txt` və `readme.txt` fayllarını `Storage/` qovluğuna köçürək:

```bash
nijatmansimov@htb[/htb]$ touch readme.txt
```

```bash
nijatmansimov@htb[/htb]$ mv information.txt readme.txt Storage/
```

```bash
nijatmansimov@htb[/htb]$ tree .

.
└── Storage
    ├── information.txt
    ├── local
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 3 files
```

`readme.txt` faylını `local/` qovluğuna kopyalayaq:

```bash
nijatmansimov@htb[/htb]$ cp Storage/readme.txt Storage/local/
```

```bash
nijatmansimov@htb[/htb]$ tree .

.
└── Storage
    ├── information.txt
    ├── local
    │   ├── readme.txt
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directories, 4 files
```

Linux-da fayllarla işləmək üçün digər güclü üsullar da var, məsələn, yönləndirmələr və mətn redaktorlarından istifadə. Yönləndirmə əmrlər və fayllar arasında giriş və çıxış axınını idarə etməyə imkan verir, faylları yaratmaq və dəyişdirmək daha sürətli və səmərəli olur. Daha interaktiv redaktə üçün vim və nano kimi məşhur redaktorlardan istifadə edə bilərik.

Bu üsulları sonrakı bölmələrdə daha ətraflı müzakirə edəcəyik. Təcrübə keçdikcə faylları yaratmaq, redaktə etmək və idarə etməkdə daha çox çeviklik qazanacaqsınız.

## İstəyə Bağlı Tapşırıq

Artıq öyrəndiyimiz alətlərdən istifadə edərək faylları və qovluqları necə silməyi öyrənin. Online tədqiqat öyrənmə prosesinin dəyərli hissəsidir—bu, fırıldaq deyil. İndi test olunmur, biliklərinizi artırırsınız. Online axtarış fərqli yanaşmaları və alternativ üsulları göstərərək, məsələlərin həlli üçün daha geniş anlayış qazandırır.
