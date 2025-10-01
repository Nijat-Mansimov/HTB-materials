## HTML

Veb tətbiqlərinin ön interfeysinin (*front end*) birinci və ən dominant komponenti **HTML (HyperText Markup Language)**-dir. HTML internetdə gördüyümüz **hər hansı bir veb səhifəsinin tam əsasını** təşkil edir. O, **başlıqlar** (*titles*), **formalar** (*forms*), **şəkillər** (*images*) və bir çox digər elementlər daxil olmaqla, hər bir səhifənin **əsas elementlərini** ehtiva edir. Veb brauzeri isə öz növbəsində bu elementləri **tərcümə edir** və son istifadəçiyə göstərir.

Aşağıdakı **çox sadə bir HTML səhifəsinin** nümunəsidir:

### Nümunə

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Səhifə Başlığı</title>
    </head>
    <body>
        <h1>Bir Başlıq</h1>
        <p>Bir Paraqraf</p>
    </body>
</html>
```

Bu, aşağıdakıları göstərəcək:

<img width="1108" height="243" alt="image" src="https://github.com/user-attachments/assets/fd385ffe-adf4-4996-b980-15ae908b5934" />

Gördüyümüz kimi, HTML elementləri **XML** və digər dillərə bənzər şəkildə **ağac formasında** göstərilir:

## HTML Strukturu

```
HTML
document
 - html
   -- head
      --- title
   -- body
      --- h1
      --- p
```

Hər bir element digər HTML elementlərini ehtiva edə bilər, əsas **HTML** teqi isə səhifədəki **bütün digər elementləri** ehtiva etməlidir ki, bu da **document** altına düşür, **HTML** ilə **XML sənədləri** kimi digər dillər üçün yazılmış sənədlər arasında fərq qoyulur.

Yuxarıdakı kodun HTML elementlərinə aşağıdakı kimi baxıla bilər:

```html
<html>
    <head>
        <title>Səhifə Başlığı</title>
    </head>
    <body>
        <h1>Bir Başlıq</h1>
        <p>Bir Paraqraf</p>
    </body>
</html>
```

Hər bir HTML elementi elementin növünü göstərən bir teq ilə **açılır və bağlanır** ('məsələn, paraqraflar üçün `<p>`), burada məzmun bu teqlər arasında yerləşdirilir. Teqlər həmçinin elementin **id** və ya **class**-ını da saxlaya bilər ('məsələn, `<p id='para1'>` və ya `<p id='red-paragraphs'>`), bu da **CSS-in elementi düzgün formatlaşdırması** üçün lazımdır. Həm teqlər, həm də məzmun bütöv elementdən ibarətdir.

-----

## URL Kodlaşdırması

HTML-də öyrənilməli olan vacib bir anlayış **URL Kodlaşdırması** və ya **faiz-kodlaşdırmadır** (*percent-encoding*). Brauzerin bir səhifənin məzmununu düzgün göstərməsi üçün istifadə olunan **simvol dəstini** (*charset*) bilməlidir. Məsələn, **URL-lərdə** brauzerlər yalnız **alfanumerik simvollara** və müəyyən **xüsusi simvollara** icazə verən **ASCII** kodlaşdırmasını istifadə edə bilər. Buna görə də, **ASCII simvol dəstindən kənarda** olan bütün digər simvollar bir URL daxilində **kodlaşdırılmalıdır**.

URL kodlaşdırması təhlükəsiz olmayan ASCII simvollarını **$\%$ simvolu** və ardınca **iki onaltılıq rəqəmlə** əvəz edir.

Məsələn, **tək dırnaq** simvolu **'** $\rightarrow$ **%27** olaraq kodlaşdırılır, bu da brauzerlər tərəfindən tək dırnaq kimi başa düşülə bilər. URL-lərdə **boşluq** ola bilməz və boşluq ya **$+$** (plus işarəsi) ya da **%20** ilə əvəz ediləcək. Bəzi ümumi simvol kodlaşdırmaları bunlardır:

| Simvol | Kodlaşdırma |
| :---: | :---: |
| Boşluq | %20 |
| \! | %21 |
| " | %22 |
| \# | %23 |
| $ | %24 |
| % | %25 |
| & | %26 |
| ' | %27 |
| ( | %28 |
| ) | %29 |

-----

## İstifadəsi

**`<head>`** elementi adətən səhifə başlığı kimi **səhifədə birbaşa çap olunmayan** elementləri ehtiva edir, bütün əsas səhifə elementləri isə **`<body>`** altında yerləşir. Digər vacib elementlərə **səhifənin CSS kodunu** saxlayan **`<style>`** və növbəti bölmədə görəcəyimiz kimi səhifənin **JS kodunu** saxlayan **`<script>`** daxildir.

Bu elementlərin hər biri **DOM (Document Object Model)** adlanır. **World Wide Web Consortium (W3C)** **DOM-u** belə təyin edir:

> "W3C Document Object Model (DOM) proqramlara və skriptlərə sənədin məzmununa, strukturuna və stilinə dinamik olaraq daxil olmaq və yeniləmək imkanı verən platforma və dildən asılı olmayan bir interfeysdir."

DOM standartı **$3$ hissəyə** ayrılır:

  * **Core DOM** - Bütün sənəd növləri üçün standart model
  * **XML DOM** - XML sənədləri üçün standart model
  * **HTML DOM** - HTML sənədləri üçün standart model

Məsələn, yuxarıdakı ağac görünüşündən DOM-lara **`document.head`** və ya **`document.h1`** və s. kimi müraciət edə bilərik.

HTML DOM strukturunu anlamaq, səhifədə gördüyümüz hər bir elementin **harada yerləşdiyini** başa düşməyimizə kömək edə bilər ki, bu da səhifədəki **müəyyən bir elementin mənbə koduna baxmağa** və **potensial problemləri** axtarmağa imkan verir. HTML elementlərini **id, teq adı** və ya **class adı** ilə tapa bilərik.

Bu, həmçinin ön interfeys zəifliklərindən (məsələn, **XSS**) istifadə edərək **mövcud elementləri manipulyasiya etmək** və ya ehtiyaclarımıza xidmət etmək üçün **yeni elementlər yaratmaq** istədiyimiz zaman da faydalıdır.

-----

HTML veb səhifələrinin təməlini təşkil edən ağac strukturu və URL kodlaşdırması haqqında hər şey aydındır? Bu anlayışlar, xüsusilə də DOM-u başa düşmək, gələcək nüfuzetmə sınağı fəaliyyətlərimiz üçün kritikdir\! 🤔















