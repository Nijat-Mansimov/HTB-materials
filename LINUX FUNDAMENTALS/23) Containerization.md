# Kontainerləşdirmə (Containerization)

**Kontainerləşdirmə**, tətbiqləri təcrid olunmuş mühitlərdə, adətən **kontainerlər** adlanan yerlərdə paketləmə və işlətmə prosesidir. Bu kontainerlər, tətbiqlərin quraşdırıldığı yerdən asılı olmayaraq, eyni şəkildə işləməsini təmin edərək, tətbiqlərin işləməsi üçün **yüngül**, **ardıcıl mühitlər** təmin edir. **Docker**, **Docker Compose** və **Linux Kontainerləri (LXC)** kimi texnologiyalar kontainerləşdirməni, əsasən Linux əsaslı sistemlərdə mümkün edir. Kontainerlər, host sistemin nüvəsini bölüşmələri ilə **virtual maşınlardan** fərqlənir, bu da onları daha yüngül və səmərəli edir. Bu texnologiyalarla istifadəçilər təhlükəsizliyi, daşınması və miqyaslanması yaxşılaşdırılmış tətbiqləri sürətlə yarada, quraşdıra və idarə edə bilərlər.

Kontainerlər yüksək dərəcədə konfiqurasiya edilə biləndir, istifadəçilərə onları xüsusi ehtiyaclarına uyğunlaşdırmağa imkan verir və onların yüngül təbiəti eyni host sistemdə eyni vaxtda çoxlu kontainerləri işlətməyi asanlaşdırır. Bu xüsusiyyət, tətbiqlərin **miqyaslanması** və **mürəkkəb mikroxidmət arxitekturalarının** idarə edilməsi üçün xüsusilə əlverişlidir.

Təsəvvür edin ki, böyük bir konsert təşkil edirsiniz və hər bir qrupa özəl səhnə quruluşu lazımdır. Hər qrup üçün tamamilə yeni bir səhnə qurmaq (virtual maşın istifadə etmək kimi) əvəzinə, qrupa lazım olan hər şeyi - işıqları, alətləri, dinamikləri və s. ehtiva edən **portativ, özü-özlüyündə olan "səhnə podları"** yaradırsınız. Bu podlar yüngüldür, yenidən istifadə edilə bilər və məkandan məkana asanlıqla köçürülə bilər. Əsas odur ki, podların hamısı eyni əsas səhnədə (host sistem) qüsursuz işləyir, lakin hər biri o qədər təcrid olunub ki, heç bir qrupun quruluşu digərlərinə mane olmur.

Eyni şəkildə, kontainerlər bir tətbiqi bütün tələb olunan alətləri və parametrləri ilə paketləyir, fərqli sistemlərdə münaqişəsiz ardıcıl işləməsinə imkan verir, bütün bunlar eyni "əsas səhnəni" (əsas sistemin resurslarını) paylaşarkən.

**Təhlükəsizlik** kontainerləşdirmənin kritik bir aspektidir. Kontainerlər tətbiqləri host sistemindən və bir-birindən təcrid edir, hosta və ya digər kontainerlərə təsir edə biləcək zərərli fəaliyyətlərin riskini azaldan bir maneə təmin edir. Bu təcrid, düzgün konfiqurasiya və sərtləşdirmə üsulları ilə birlikdə əlavə bir təhlükəsizlik qatı əlavə edir. Lakin, qeyd etmək vacibdir ki, kontainerlər ənənəvi virtual maşınlarla eyni səviyyədə təcrid təklif etmir. Düzgün idarə olunmazsa, **imtiyaz yüksəlişi** və ya **kontainerdən qaçış** kimi zəifliklər host sistemə və ya digər kontainerlərə icazəsiz giriş əldə etmək üçün istifadə edilə bilər.

Təkmilləşdirilmiş təhlükəsizlik və resurs səmərəliliyinə əlavə olaraq, kontainerlər tətbiqlərin quraşdırılmasını, idarə edilməsini və miqyaslanmasını asanlaşdırır. Kontainerlər tətbiqin ehtiyac duyduğu hər şeyi (məsələn, kitabxanalar, asılılıqlar) özündə ehtiva etdiyi üçün, inkişaf, test və istehsal mühitlərində **ardıcıllığı** təmin edirlər. Bu **daşınma qabiliyyəti** tətbiqlərin fərqli mühitlərdə etibarlı işləməsini təmin edir.

Lakin, onların üstünlüklərinə baxmayaraq, kontainerlərin təhlükəsizlik risklərinə qarşı immunitetə malik olmadığını qəbul etmək vacibdir. Kontainerlərin təmin etdiyi təcridi yüksəltmək və ya ondan qaçmaq üçün istifadə edə biləcəyimiz üsullar var.

-----

## Docker

**Docker** tətbiqlərin **kontainerlər** adlanan özü-özlüyündə olan vahidlər kimi quraşdırılmasını avtomatlaşdırmaq üçün açıq mənbəli bir platformadır. O, elastiklik və daşınma qabiliyyəti təmin etmək üçün qatlı fayl sistemindən və resurs təcrid xüsusiyyətlərindən istifadə edir. Əlavə olaraq, o, kontainerləşdirmə prosesini sürətləndirməyə kömək edən tətbiqlərin yaradılması, quraşdırılması və idarə edilməsi üçün güclü bir alətlər dəsti təqdim edir.

Docker kontainerlərini möhürlənmiş bir **nahar qutusu** kimi təsəvvür edin. İçindəki yeməyi yeyə bilərsiniz (tətbiqləri işlədə bilərsiniz), lakin qutunu bağladıqdan sonra (kontaineri dayandırdıqdan sonra) hər şey sıfırlanır. Yenilənmiş məzmunu (dəyişdirilmiş konfiqurasiyaları) olan yeni bir nahar qutusu (yeni kontainer) etmək üçün, orijinalına əsaslanan yeni bir resept (Dockerfile) yaradırsınız. Bir restoranda çoxsaylı nahar qutularını (istehsal) təqdim edərkən, bütün sifarişləri rəvan idarə etmək üçün mətbəx sistemi (Kubernetes/Docker Compose) istifadə edərsiniz.

### Docker Mühərrikinin Quraşdırılması

Docker-in quraşdırılması nisbətən sadədir. Onu bir Ubuntu hostunda quraşdırmaq üçün aşağıdakı skripti istifadə edə bilərik:

```bash
#!/bin/bash
# Preparation
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Add user htb-student to the Docker group
sudo usermod -aG docker htb-student
echo '[!] You need to log out and log back in for the group changes to take effect.'

# Test Docker installation
docker run hello-world
```

Kontainer işlətmək üçün **Docker mühərriki** və xüsusi **Docker imicləri** lazımdır. Bunlar hazır imiclərin deposu olan **Docker Hub**-dan əldə edilə bilər və ya istifadəçi tərəfindən yaradıla bilər. Docker Hub, proqram təminatı depoları üçün bulud əsaslı bir qeydiyyat yeri və ya Docker imicləri üçün bir kitabxanadır. O, **ictimai** və **şəxsi** sahələrə bölünür. İctimai sahə istifadəçilərə icma ilə imicləri yükləməyə və paylaşmağa imkan verir. O, həmçinin Docker inkişaf komandasından və qurulmuş açıq mənbəli layihələrdən **rəsmi imicləri** ehtiva edir. Qeydiyyatın şəxsi sahəsinə yüklənmiş imiclər ictimaiyyət üçün əlçatan deyil. Onlar bir şirkət daxilində və ya komandalar və tanışlar ilə paylaşıla bilər.

Docker imicinin yaradılması, Docker mühərrikinin kontaineri yaratmaq üçün ehtiyac duyduğu bütün təlimatları ehtiva edən **`Dockerfile`** yaradılması ilə həyata keçirilir. Biz Docker kontainerlərini hədəf sistemlərimizə xüsusi faylları köçürərkən "fayl hostinq" serverimiz kimi istifadə edə bilərik. Buna görə də, **Apache** və **SSH** serveri işləyən **Ubuntu 22.04** əsasında bir `Dockerfile` yaratmalıyıq. Bununla biz faylları docker imicinə köçürmək üçün **`scp`** istifadə edə bilərik və Apache bizə faylları host etməyə və lazım olan faylları yükləmək üçün hədəf sistemdə **`curl`**, **`wget`** və digərləri kimi alətləri istifadə etməyə imkan verir. Belə bir `Dockerfile` aşağıdakı kimi görünə bilər:

### Dockerfile

```bash
# Use the latest Ubuntu 22.04 LTS as the base image
FROM ubuntu:22.04

# Update the package repository and install the required packages
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Create a new user called "docker-user"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Give the docker-user user full access to the Apache and SSH services
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Expose the required ports
EXPOSE 22 80

# Start the SSH and Apache services
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

`Dockerfile`-ımızı təyin etdikdən sonra, onu bir **imicə** çevirməliyik. **`build`** əmri ilə `Dockerfile` olan qovluğu götürürük, `Dockerfile`-dakı addımları icra edirik və imici yerli Docker Mühərrikimizdə saxlayırıq. Addımlardan biri səhv ucbatından uğursuz olarsa, kontainerin yaradılması dayandırılacaq. **`-t`** seçimi ilə kontainerimizə bir etiket veririk ki, sonradan onu müəyyən etmək və onunla işləmək daha asan olsun.

### Docker Qurmaq (Build)

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ docker build -t FS_docker .
```

Docker imici yaradıldıqdan sonra, Docker mühərriki vasitəsilə icra edilə bilər, bu da onu kontainer işlətmək üçün çox səmərəli və asan bir yol edir. Bu, imiclərə əsaslanan virtual maşın konsepsiyasına bənzəyir. Yenə də, bu imiclər **yalnız oxumaq üçün olan şablonlardır** və işləmə müddəti üçün lazım olan fayl sistemini və bütün parametrləri təmin edir. Bir kontainer bir imicin **işləyən prosesi** sayıla bilər. Bir kontainer bir sistemdə başladılmalı olduqda, əvvəlcə müvafiq imic ilə bir paket, yerli olaraq mövcud deyilsə, yüklənir. Kontaineri **`docker run`** əmri ilə başlata bilərik:

### Docker İşlətmək - Sintaksis

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ docker run -p <host port>:<docker port> -d <docker container name>
```

### Docker İşlətmək (Run)

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ docker run -p 8022:22 -p 8080:80 -d FS_docker
```

Bu vəziyyətdə, **`FS_docker`** imicindən yeni bir kontainer başladırıq və host portları 8022 və 8080-i müvafiq olaraq kontainer portları 22 və 80-ə **eşləyirik**. Kontainer arxa planda işləyir, bu da bizə göstərilən host portlarından istifadə edərək kontainer daxilindəki SSH və HTTP xidmətlərinə daxil olmağa imkan verir.

### Docker İdarəetməsi (Management)

Docker kontainerlərini idarə edərkən, Docker bizə kontainerləri asanlıqla yaratmağa, quraşdırmağa və idarə etməyə imkan verən hərtərəfli bir alətlər dəsti təqdim edir. Bu güclü alətlərlə biz kontainerləri siyahılaya, başlada və dayandıra və tətbiqlərin qüsursuz icrasını təmin edərək onları effektiv şəkildə idarə edə bilərik. Ən çox istifadə olunan Docker idarəetmə əmrlərindən bəziləri:

| Əmr | Təsvir |
| :--- | :--- |
| `docker ps` | Bütün işləyən kontainerləri siyahılayır. |
| `docker stop` | İşləyən bir kontaineri dayandırır. |
| `docker start` | Dayandırılmış bir kontaineri başladır. |
| `docker restart` | İşləyən bir kontaineri yenidən başladır. |
| `docker rm` | Bir kontaineri silir. |
| `docker rmi` | Bir Docker imicini silir. |
| `docker logs` | Bir kontainerin loglarını göstərir. |

Qeyd etmək vacibdir ki, Docker əmrləri əlavə funksionallıq əlavə etmək üçün müxtəlif seçimlərlə birləşdirilə bilər. Məsələn, hansı portları açıqlayacağınızı göstərə, məlumatları saxlamaq üçün **həcmləri qoşa** və ya kontainerlərinizi konfiqurasiya etmək üçün **ətraf mühit dəyişənlərini** təyin edə bilərsiniz. Bu elastiklik sizə xüsusi ehtiyacları və tələbləri ödəmək üçün Docker kontainerlərinizi fərdiləşdirməyə imkan verir.

Docker imicləri ilə işləyərkən, bir imicə əsaslanan işləyən kontainerdə edilən hər hansı bir dəyişikliyin imicə avtomatik olaraq saxlanılmadığını anlamaq vacibdir. Bu dəyişiklikləri qorumaq üçün onları ehtiva edən yeni bir imic yaratmalısınız. Bu, **`FROM`** ifadəsi ilə başlayan (əsas imici göstərən) və sonra dəyişiklikləri tətbiq etmək üçün lazım olan əmrləri ehtiva edən yeni bir `Dockerfile` yazmaqla edilir. `Dockerfile` hazır olduqdan sonra, yeni imici qurmaq və onu müəyyən etmək üçün unikal bir etiket təyin etmək üçün **`docker build`** əmrini istifadə edə bilərsiniz. Bu proses orijinal imicin dəyişməz qalmasını təmin edir, yeni imic isə yeniləmələri əks etdirir.

Həmçinin yadda saxlamaq vacibdir ki, Docker kontainerləri dizaynla **statussuzdur** (stateless), yəni işləyən bir kontainer daxilində edilən hər hansı bir dəyişiklik (məsələn, faylların dəyişdirilməsi) kontainer dayandırıldıqdan və ya silindikdən sonra itirilir. Bu səbəbdən, məlumatları kontainerin xaricində **saxlamaq** və ya tətbiq vəziyyətini saxlamaq üçün **həcmlərdən** (**volumes**) istifadə etmək ən yaxşı praktikadır.

İstehsal mühitlərində kontainerləri miqyasda idarə etmək daha mürəkkəbləşir. **Docker Compose** və ya **Kubernetes** kimi alətlər kontainerləri **orkestrasiya etməyə** kömək edir, bu da sizə çoxsaylı kontainerləri səmərəli şəkildə idarə etməyə, miqyaslamağa və əlaqələndirməyə imkan verir.

-----

## Linux Kontainerləri (LXC)

**Linux Kontainerləri (LXC)** birdən çox təcrid olunmuş Linux sisteminin (**kontainerlər** adlanır) tək bir hostda işləməsinə imkan verən yüngül bir virtuallaşdırma texnologiyasıdır. LXC, hər bir kontainerin müstəqil işləməsini təmin etmək üçün **nəzarət qrupları** (`cgroups`) və **ad sahələri** (`namespaces`) kimi əsas resurs təcrid xüsusiyyətlərini istifadə edir. Hər bir instansiya üçün tam ƏS tələb edən ənənəvi virtual maşınlardan fərqli olaraq, kontainerlər hostun nüvəsini bölüşür, bu da LXC-ni resurs istifadəsi baxımından daha səmərəli edir.

LXC, kontainerləri idarə etmək və konfiqurasiya etmək üçün hərtərəfli bir alətlər dəsti və API-lər təqdim edir, bu da onu Linux sistemlərində kontainerləşdirmə üçün populyar bir seçim edir. Lakin, LXC və Docker hər ikisi kontainerləşdirmə texnologiyaları olsa da, fərqli məqsədlərə xidmət edir və unikal xüsusiyyətlərə malikdir.

Docker, kontainerləşdirmə ideyası üzərində qurulur və istifadə asanlığı və daşınma qabiliyyəti əlavə edir, bu da onu DevOps dünyasında yüksək dərəcədə populyar edib. Docker, tətbiqləri bütün asılılıqları ilə birgə portativ bir **"imic"** daxilində paketləməyi vurğulayır, bu da onların fərqli mühitlərdə asanlıqla quraşdırılmasına imkan verir. Lakin, ikisi arasında aşağıdakı kateqoriyalara əsaslanan bəzi fərqlər var:

| Kateqoriya | Təsvir |
| :--- | :--- |
| **Yanaşma** | LXC, yüngül virtual maşınlar kimi davranan təcrid olunmuş Linux mühitləri yaratmağa yönəlmiş daha ənənəvi, **sistem səviyyəli** kontainerləşdirmə aləti kimi görülür. Docker isə **tətbiq yönümlüdür**, yəni tək tətbiqlərin və ya mikroxidmətlərin paketlənməsi və quraşdırılması üçün optimallaşdırılmışdır. |
| **İmicin Qurulması** | Docker, bir tətbiqi işlətmək üçün lazım olan hər şeyi (kod, kitabxanalar, konfiqurasiyalar) ehtiva edən standartlaşdırılmış bir imic formatından (Docker imicləri) istifadə edir. LXC, oxşar funksionallığa malik olsa da, adətən mühitlərin qurulması və idarə edilməsi üçün daha çox əl ilə quraşdırma tələb edir. |
| **Daşınma Qabiliyyəti** | Docker daşınma qabiliyyətində üstündür. Onun kontainer imicləri Docker Hub və ya digər qeydiyyat yerləri vasitəsilə fərqli sistemlərdə asanlıqla paylaşıla bilər. LXC mühitləri bu mənada daha az daşınabiləndir, çünki host sistemin konfiqurasiyası ilə daha sıx inteqrasiya olunmuşdur. |
| **İstifadə Asanlığı** | Docker sadəlik nəzərə alınmaqla hazırlanmışdır, istifadəçi dostu bir CLI və geniş icma dəstəyi təklif edir. LXC, güclü olsa da, Linux sistem idarəetməsi haqqında daha dərindən bilik tələb edə bilər, bu da yeni başlayanlar üçün daha az sadədir. |
| **Təhlükəsizlik** | Docker kontainerləri, **AppArmor** və **SELinux** kimi əlavə təcrid qatları, eləcə də onun **yalnız oxumaq üçün olan fayl sistemi** xüsusiyyəti sayəsində, adətən qutudan çıxarıldığı kimi daha təhlükəsizdir. LXC kontainerləri, təhlükəsiz olsa da, Docker-in defolt olaraq təklif etdiyi təcrid səviyyəsinə uyğunlaşmaq üçün əlavə konfiqurasiyalar tələb edə bilər. |

LXC-də imiclər, bir kök fayl sistemi yaradaraq və lazımi paketləri və konfiqurasiyaları quraşdıraraq əl ilə qurulur. Bu kontainerlər host sisteminə bağlıdır, asanlıqla daşınabilməz və konfiqurasiya etmək və idarə etmək üçün daha çox texniki təcrübə tələb edə bilər.

Digər tərəfdən, Docker, LXC üzərində qurulan və kontainerləşdirmə üçün daha istifadəçi dostu bir interfeys təmin edən **tətbiq yönümlü bir platformadır**. Onun imicləri, əsas imici və imici qurmaq üçün tələb olunan addımları göstərən bir `Dockerfile` istifadə edərək qurulur. Bu imiclər daşınabilən olmaq üçün nəzərdə tutulmuşdur ki, asanlıqla bir mühitdən digərinə köçürülə bilsinlər.

Bir Linux paylamasında LXC-ni quraşdırmaq üçün paylamanın paket menecerini istifadə edə bilərik. Məsələn, Ubuntu-da LXC-ni aşağıdakı əmrlə quraşdırmaq üçün `apt` paket menecerini istifadə edə bilərik:

### LXC Quraşdırmaq

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo apt-get install lxc lxc-utils -y
```

LXC quraşdırıldıqdan sonra, Linux hostunda kontainerləri yaratmağa və idarə etməyə başlaya bilərik. Qeyd etmək lazımdır ki, LXC kontainerləşdirmə üçün lazım olan xüsusiyyətləri dəstəkləmək üçün Linux nüvəsini tələb edir. Əksər müasir Linux nüvələrində kontainerləşdirmə üçün daxili dəstək var, lakin bəzi köhnə nüvələr LXC üçün dəstəyi aktivləşdirmək üçün əlavə konfiqurasiya və ya yamaq tələb edə bilər.

### LXC Kontaineri Yaratmaq

Yeni bir LXC kontaineri yaratmaq üçün, **`lxc-create`** əmrini, ardınca kontainerin adını və istifadə ediləcək şablonu istifadə edə bilərik. Məsələn, **`linuxcontainer`** adlı yeni bir Ubuntu kontaineri yaratmaq üçün aşağıdakı əmri istifadə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo lxc-create -n linuxcontainer -t ubuntu
```

### LXC Kontainerlərini İdarə Etmək

LXC kontainerləri ilə işləyərkən, onları idarə etməklə əlaqəli bir neçə vəzifə var. Bu vəzifələrə yeni kontainerlərin yaradılması, onların parametrlərinin konfiqurasiyası, lazım olduqda onların başlanması və dayandırılması, eləcə də onların performansının monitorinqi daxildir. Xoşbəxtlikdən, bu vəzifələrə kömək edə biləcək bir çox **əmrlər sətri alətləri** və **konfiqurasiya faylları** mövcuddur. Bu alətlər kontainerlərimizi tez və asanlıqla idarə etməyə, onların xüsusi ehtiyaclarımız və tələblərimiz üçün optimallaşdırılmasını təmin etməyə imkan verir.

| Əmr | Təsvir |
| :--- | :--- |
| `lxc-ls` | Bütün mövcud kontainerləri siyahılayır. |
| `lxc-stop -n <container>` | İşləyən bir kontaineri dayandırır. |
| `lxc-start -n <container>` | Dayandırılmış bir kontaineri başladır. |
| `lxc-restart -n <container>` | İşləyən bir kontaineri yenidən başladır. |
| `lxc-config -n <container name> -s storage` | Kontainer yaddaşını idarə edir. |
| `lxc-config -n <container name> -s network` | Kontainer şəbəkə parametrlərini idarə edir. |
| `lxc-config -n <container name> -s security` | Kontainer təhlükəsizlik parametrlərini idarə edir. |
| `lxc-attach -n <container>` | Bir kontainerə qoşulur. |
| `lxc-attach -n <container> -f /path/to/share` | Bir kontainerə qoşulur və xüsusi bir qovluğu və ya faylı paylaşır. |

Penetrasiya testçiləri olaraq, biz tez-tez yerli maşınlarımızda təkrarlanması çətin olan mürəkkəb asılılıqlara və ya konfiqurasiyalara malik proqram təminatını və ya sistemləri test etməli olduğumuz vəziyyətlərlə qarşılaşırıq. Məhz bu zaman Linux kontainerləri olduqca faydalı olur. Bir Linux kontaineri, kod, kitabxanalar və konfiqurasiya faylları kimi xüsusi bir proqram təminatını işlətmək üçün lazım olan hər şeyi ehtiva edən **yüngül, müstəqil bir paketdir**. Kontainerlər, host sistemin konfiqurasiyasından asılı olmayaraq, proqram təminatının istənilən Linux maşınında ardıcıl işləməsinə imkan verən **təcrid olunmuş bir mühit** təklif edir.

Kontainerlər, xüsusi test ehtiyaclarımıza uyğunlaşdırılmış **təcrid olunmuş mühitləri** sürətlə yaratmağa və işlətməyə imkan verdiyi üçün xüsusilə faydalıdır. Məsələn, müəyyən bir verilənlər bazası və ya veb server versiyasından asılı olan bir veb tətbiqini sınamalıyıqsa, bizə lazım olan dəqiq versiyaları və konfiqurasiyaları olan bir kontainer yarada bilərik. Bu, maşınımızda bu komponentləri əl ilə qurmaq çətinliyini aradan qaldırır, bu da vaxt aparan və səhvlərə meylli ola bilər.

Biz həmçinin onları **nəzarət edilən bir mühitdə** zərərsiz istismar vasitələrini və ya zərərli proqramları sınaqdan keçirmək üçün istifadə edə bilərik, burada həssas bir sistemi və ya şəbəkəni simulyasiya edən bir kontainer yaradırıq və sonra maşınlarımıza və ya şəbəkələrimizə zərər vermə riskini almadan istismar vasitələrini təhlükəsiz şəkildə sınamaq üçün bu kontainerdən istifadə edirik. Lakin, kontainer daxilində icazəsiz girişin və ya zərərli fəaliyyətlərin qarşısını almaq üçün **LXC kontainer təhlükəsizliyini konfiqurasiya etmək** vacibdir. Bu, bir neçə təhlükəsizlik tədbirinin tətbiqi ilə əldə edilə bilər, məsələn:

  * Kontainerə girişi məhdudlaşdırmaq
  * Resursları məhdudlaşdırmaq
  * Kontaineri hostdan təcrid etmək
  * Məcburi giriş nəzarətini tətbiq etmək
  * Kontaineri güncəl saxlamaq

LXC kontainerlərinə **SSH** və ya **konsol** kimi müxtəlif üsullarla daxil olmaq olar. Lazımsız xidmətləri aradan qaldırmaq, təhlükəsiz protokollardan istifadə etmək və güclü autentifikasiya mexanizmlərini tətbiq etməklə kontainerə girişi məhdudlaşdırmaq tövsiyə olunur. Məsələn, `openssh-server` paketini silməklə və ya SSH-i yalnız etibarlı IP ünvanlarından girişi təmin etmək üçün konfiqurasiya etməklə kontainerə SSH girişini aradan qaldıra bilərik. Bu kontainerlər həmçinin host sistemi ilə eyni nüvəni paylaşır, yəni sistemdə mövcud olan bütün resurslara daxil ola bilərlər. Kontainerlərin həddindən artıq resurs istehlak etməsinin qarşısını almaq üçün **resurs limitlərini** və ya **kvotaları** istifadə edə bilərik. Məsələn, bir kontainerin istifadə edə biləcəyi CPU, yaddaş və ya disk sahəsinin miqdarını məhdudlaşdırmaq üçün **`cgroups`** istifadə edə bilərik.

### LXC Təhlükəsizliyini Təmin Etmək

Gəlin kontainer üçün resursları məhdudlaşdıraq. LXC üçün **`cgroups`** konfiqurasiya etmək və bir kontainerin CPU və yaddaşını məhdudlaşdırmaq üçün, **`/usr/share/lxc/config/<container name>.conf`** qovluğunda kontainerimizin adı ilə yeni bir konfiqurasiya faylı yarada bilərik. Məsələn, **`linuxcontainer`** adlı bir kontainer üçün konfiqurasiya faylı yaratmaq üçün aşağıdakı əmri istifadə edə bilərik:

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo vim /usr/share/lxc/config/linuxcontainer.conf
```

Bu konfiqurasiya faylında, kontainerin istifadə edə biləcəyi CPU və yaddaşı məhdudlaşdırmaq üçün aşağıdakı sətirləri əlavə edə bilərik.

```txt
lxc.cgroup.cpu.shares = 512
lxc.cgroup.memory.limit_in_bytes = 512M
```

Kontainerin resurs bölgüsünə nəzarət edən əsas parametrlərdən biri **`lxc.cgroup.cpu.shares`** parametridir. Bu parametr bir kontainerin sistemdəki digər kontainerlərə nisbətən istifadə edə biləcəyi **CPU vaxtını** müəyyənləşdirir. Defolt olaraq, bu dəyər **1024** olaraq təyin edilmişdir, yəni kontainer CPU vaxtının öz ədalətli hissəsinə qədər istifadə edə bilər. Lakin, bu dəyəri, məsələn, **512** olaraq təyin etsək, kontainer sistemdə mövcud olan CPU vaxtının yalnız yarısını istifadə edə bilər. Bu, resursları idarə etmək və bütün kontainerlərin CPU vaxtına lazımi girişi təmin etmək üçün faydalı bir yol ola bilər.

Kontainerin resurs bölgüsünə nəzarət etməkdə əsas parametrlərdən biri **`lxc.cgroup.memory.limit_in_bytes`** parametridir. Bu parametr bir kontainerin istifadə edə biləcəyi **maksimum yaddaş miqdarını** təyin etməyə imkan verir. Qeyd etmək vacibdir ki, bu dəyər bayt, kilobayt (K), meqabayt (M), giqabayt (G) və ya terabayt (T) daxil olmaqla müxtəlif vahidlərdə göstərilə bilər, bu da kontainer resurs limitlərini təyin edərkən yüksək dərəcədə qranulyarlıq təmin edir. Bu iki sətri əlavə etdikdən sonra, faylı saxlaya və bağlaya bilərik:

```bash
[Esc]
:
wq
```

Bu dəyişiklikləri tətbiq etmək üçün LXC xidmətini yenidən başlatmalıyıq.

Müntəzəm İfadələr

```bash
nijatmansimov@htb[/htb]$ sudo systemctl restart lxc.service
```

LXC, prosesləri, şəbəkələri və fayl sistemlərini host sistemindən təcrid olunmuş bir mühit təmin etmək üçün **`namespaces`** istifadə edir. Ad sahələri, sistem resurslarının mücərrədliyini təmin etməklə təcrid olunmuş mühitlərin yaradılmasına imkan verən Linux nüvəsinin bir xüsusiyyətidir.

Ad sahələri, kontainerin prosesləri, şəbəkə interfeysləri, marşrutlaşdırma cədvəlləri və firewall qaydaları üçün yüksək dərəcədə təcrid təmin etdiyi üçün kontainerləşdirmənin kritik bir aspektidir. Hər bir kontainerə host sistemin proses ID-lərindən təcrid olunmuş unikal bir proses ID (pid) nömrəsi sahəsi ayrılır. Bu, kontainerin proseslərinin host sistemin proseslərinə mane ola bilməməsini təmin edir, sistemin sabitliyini və etibarlılığını artırır. Əlavə olaraq, hər bir kontainerin host sistemin şəbəkə interfeyslərindən tamamilə ayrı olan öz şəbəkə interfeysləri (`net`), marşrutlaşdırma cədvəlləri və firewall qaydaları var. Kontainer daxilindəki hər hansı bir şəbəkə ilə əlaqəli fəaliyyət host sistemin şəbəkəsindən ayrılır, əlavə bir şəbəkə təhlükəsizliyi qatı təmin edir.

Bundan əlavə, kontainerlər host sistemin kök fayl sistemindən tamamilə fərqli olan öz kök fayl sistemi (`mnt`) ilə gəlir. İkisi arasındakı bu ayrılma, kontainerin fayl sistemində edilən hər hansı bir dəyişikliyin və ya dəyişikliyin host sistemin fayl sisteminə təsir etməməsini təmin edir. Lakin, unutmaq vacibdir ki, ad sahələri yüksək səviyyədə təcrid təmin etsə də, **tam təhlükəsizlik** təmin etmir. Buna görə də, kontaineri və host sistemi potensial təhlükəsizlik pozuntularından daha da qorumaq üçün hər zaman əlavə təhlükəsizlik tədbirləri tətbiq etmək məsləhətdir.
