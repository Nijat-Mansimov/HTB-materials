### Kömək Almaq – Sənədləşmə

#### Ümumi Məlumat

Linux-un strukturu, müxtəlif paylamaları və shell-in məqsədi haqqında əsas biliklər əldə etdikdən sonra bu bilikləri praktikada tətbiq etmək vaxtıdır. İndi əmrli şəkildə terminalda işləməyi və tanımadığımız əmrlərlə qarşılaşdıqda necə kömək axtarmağı öyrənəcəyik.

Həmişə yadda saxlamalıyıq ki, yaddaşımızda olmayan və ya əvvəllər görmədiyimiz alətlərlə qarşılaşacağıq. Buna görə də həmin alətlərlə tanış olmaq üçün necə özümüzə kömək edə biləcəyimizi bilmək vacibdir. İlk iki üsul `man` səhifələri və `--help` funksiyalarından istifadə etməkdir. İstədiyimiz aləti əvvəlcə tanımaq həmişə yaxşı bir fikirdir. `man` səhifələrində ətraflı izahlarla birlikdə geniş təlimatlar tapacağıq.

---

#### İlk əmr (nümunə)

```
nijatmansimov@htb[/htb]$ ls

cacert.der  Documents  Music     Public     Videos
Desktop     Downloads  Pictures  Templates
```

`ls` əmri Linux və Unix sistemlərində cari qovluqdakı və ya göstərilən qovluqdakı fayl və kataloqları siyahıya almaq üçün istifadə olunur. `ls`-in əlavə seçim və xüsusiyyətləri var ki, çıxışı süzmək və formatlamaq üçün faydalıdır. Hər hansı bir alətin hansı seçimləri təklif etdiyini öyrənmək üçün bir neçə üsul var. Onlardan biri `man` əmrindən istifadə edib həmin alətin manual səhifəsini göstərməkdir.

**Sintaksis:**

```
nijatmansimov@htb[/htb]$ man <tool>
```

**Nümunə:**

```
nijatmansimov@htb[/htb]$ man ls
```

`man ls` nəticəsi (qısa hissə):

```
LS(1)                            User Commands                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci‐
       fied.

       Mandatory  arguments  to  long  options are mandatory for short options
       too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..
```

(Manual səhifəsində daha çox detallar və nümunələr göstərilir.)

---

#### Qısa kömək (–-help və -h)

Bəzən tam manualı oxumaq yerinə, alətin qısa köməyinə tez baxmaq kifayət edir. Bir çox alətlər üçün bu aşağıdakı şəkildə edilir:

**Sintaksis:**

```
nijatmansimov@htb[/htb]$ <tool> --help
```

**Nümunə:**

```
nijatmansimov@htb[/htb]$ ls --help

Usage: ls [OPTION]... [FILE]...
List information about the FILEs (the current directory by default).
Sort entries alphabetically if none of -cftuvSUX nor --sort is specified.

Mandatory arguments to long options are mandatory for short options too.
  -a, --all                  do not ignore entries starting with .
  -A, --almost-all           do not list implied . and ..
      --author               with -l, print the author of each file
  -b, --escape               print C-style escapes for nongraphic characters
      --block-size=SIZE      with -l, scale sizes by SIZE when printing them;
                             e.g., '--block-size=M'; see SIZE format below

  -B, --ignore-backups       do not list implied entries ending with ~
  -c                         with -lt: sort by, and show, ctime (time of last
                             modification of file status information);
                             with -l: show ctime and sort by name;
                             otherwise: sort by ctime, newest first

  -C                         list entries by columns
<SNIP>
```

Bəzi alətlər `--help` əvəzinə `-h` qısaltmasından istifadə edirlər. Məsələn:

**Sintaksis:**

```
nijatmansimov@htb[/htb]$ <tool> -h
```

**Nümunə:**

```
nijatmansimov@htb[/htb]$ curl -h

Usage: curl [options...] <url>
     --abstract-unix-socket <path> Connect via abstract Unix domain socket
     --anyauth       Pick any authentication method
 -a, --append        Append to target file when uploading
     --basic         Use HTTP Basic Authentication
     --cacert <file> CA certificate to verify peer against
     --capath <dir>  CA directory to verify peer against
 -E, --cert <certificate[:password]> Client certificate file and password
<SNIP>
```

---

#### `apropos` aləti

`apropos` aləti manual səhifələrinin qısa izah hissələrini axtarır. Bu, müəyyən açar sözə uyğun hansı nəzərdən keçirilməsi lazım olan alətlər olduğunu tapmaq üçün faydalıdır.

**Sintaksis:**

```
nijatmansimov@htb[/htb]$ apropos <keyword>
```

**Nümunə:**

```
nijatmansimov@htb[/htb]$ apropos sudo

sudo (8)             - execute a command as another user
sudo.conf (5)        - configuration for sudo front end
sudo_plugin (8)      - Sudo Plugin API
sudo_root (8)        - How to run administrative commands
sudoedit (8)         - execute a command as another user
sudoers (5)          - default sudo security policy plugin
sudoreplay (8)       - replay sudo session logs
visudo (8)           - edit the sudoers file
```

---

#### Əlavə resurslar

Uzaqdan uzun və çətin əmrləri başa düşərkən faydalı ola biləcək bir sayt:
`https://explainshell.com/` — burada uzun komanda sətrləri hissə-hissə izah olunur.

---

#### Nəticə

İrəlidə çox sayda yeni əmr və alətlə tanış olacağıq. Lakin indi siz tanımadığınız əmrlər üçün necə kömək axtarmağı bilirsiniz — `man`, `--help`/`-h`, `apropos` və onlayn alətlər kimi mənbələrdən istifadə edərək. Həmişə təcrübə etmək, eksperiment aparmaq və alətlərlə oynamaq üçün vaxt ayırmağı tövsiyə edirik — bu vaxt faydalı sərmayə olacaq.
