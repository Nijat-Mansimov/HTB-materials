## JavaScript

**JavaScript** dünyada **ən çox istifadə olunan** dillərdən biridir. Əsasən **veb inkişafı** və **mobil inkişaf** üçün istifadə olunur. JavaScript adətən bir tətbiqin **ön interfeysində** (*front end*) **brauzer daxilində icra edilmək** üçün istifadə olunur. Lakin, **NodeJS** kimi bütün veb tətbiqlərini inkişaf etdirmək üçün istifadə edilən **arxa fon JavaScript tətbiqləri** də mövcuddur.

**HTML** və **CSS** əsasən bir veb səhifənin **necə göründüyünə** cavabdeh olduğu halda, **JavaScript** adətən ön interfeys veb səhifəsinin tələb etdiyi **istənilən funksionallığı** idarə etmək üçün istifadə olunur. JavaScript olmadan, bir veb səhifə əsasən **statik** olacaq və çox **funksionallığa** və ya **interaktiv elementlərə** sahib olmayacaq.

### Nümunə

Səhifənin mənbə kodu daxilində, JavaScript kodu **`<script>`** teqi ilə aşağıdakı kimi yüklənir:

```html
<script type="text/javascript">..JavaScript kodu..</script>
```

Bir veb səhifə həmçinin `src` və skriptin linki ilə **uzaq JavaScript kodunu** da yükləyə bilər, aşağıdakı kimi:

```html
<script src="./script.js"></script>
```

Veb səhifə daxilində JavaScript-in əsas istifadəsinə bir nümunə aşağıdakıdır:

```javascript
document.getElementById("button1").innerHTML = "Dəyişdirilmiş Mətn!";
```

Yuxarıdakı nümunə **`button1`** HTML elementinin **məzmununu dəyişir**. Bundan sonra, bir veb səhifədə JavaScript-in daha **qabaqcıl istifadələri** mövcuddur. Aşağıdakı, yuxarıdakı JavaScript kodunun bir düyməyə klikləməklə əlaqələndirildikdə nə edəcəyini göstərir:

Orijinal Mətn

HTML kimi, JavaScript ilə eksperiment etmək üçün onlayn olaraq mövcud bir çox sayt var. Bir nümunə **JSFiddle**-dır ki, JavaScript, CSS və HTML-i sınaqdan keçirmək və kod parçalarını saxlamaq üçün istifadə edilə bilər. JavaScript **qabaqcıl bir dildir** və onun sintaksisi HTML və ya CSS qədər sadə deyil.

-----

## İstifadəsi

Əksər ümumi veb tətbiqləri veb səhifədə tələb olunan **bütün funksionallığı idarə etmək** üçün JavaScript-ə **çox güvənir**, məsələn, veb səhifə görünüşünü **real-time yeniləmək**, məzmunu **dinamik olaraq real-time yeniləmək**, **istifadəçi girişini qəbul etmək və emal etmək** və bir çox digər potensial funksionallıqlar.

JavaScript həmçinin **mürəkkəb prosesləri avtomatlaşdırmaq** və **arxa fon komponentləri ilə qarşılıqlı əlaqədə olmaq** üçün **HTTP sorğuları** yerinə yetirmək, **Ajax** kimi texnologiyalar vasitəsilə **məlumat göndərmək və almaq** üçün istifadə olunur.

Avtomatlaşdırmaya əlavə olaraq, JavaScript əvvəllər qeyd edildiyi kimi, **tək CSS ilə mümkün olmayacaq** qabaqcıl animasiyaları idarə etmək üçün tez-tez **CSS ilə birlikdə** istifadə olunur. **Çoxlu qabaqcıl və vizual cəlbedici animasiyalardan** istifadə edən **interaktiv və dinamik** bir veb səhifəyə daxil olduğumuz zaman, biz brauzerimizdə işləyən **aktiv JavaScript kodunun** nəticəsini görürük.

Bütün müasir veb brauzerlər, səhifəni yeniləmək üçün **arxa fon vebserverinə güvənmədən** **klient tərəfində** JavaScript kodunu icra edə bilən **JavaScript mühərrikləri** (*engines*) ilə təchiz olunmuşdur. Bu, JavaScript-dən istifadə etməyi **çoxlu prosesi sürətlə həyata keçirmək** üçün çox **sürətli bir yol** edir.

-----

## Çərçivələr (*Frameworks*)

Veb tətbiqləri daha qabaqcıl olduqca, **bütün bir veb tətbiqini sıfırdan inkişaf etdirmək** üçün təmiz JavaScript-dən istifadə etmək **səmərəsiz** ola bilər. Buna görə də, veb tətbiqinin inkişaf təcrübəsini yaxşılaşdırmaq üçün bir çox **JavaScript çərçivəsi** təqdim edilmişdir.

Bu platformalar **istifadəçi girişi və istifadəçi qeydiyyatı** kimi qabaqcıl funksionallıqları yenidən yaratmağı **çox sadə edən kitabxanalar** təqdim edir və mövcud texnologiyalara əsaslanan **yeni texnologiyalar** tətbiq edirlər, məsələn, **statik HTML kodu** əvəzinə **dinamik şəkildə dəyişən HTML kodu** istifadə etmək.

Bu platformalar ya proqramlaşdırma dili olaraq **JavaScript-dən istifadə edir**, ya da kodunu **JavaScript koduna kompilyasiya edən** JavaScript tətbiqini istifadə edir.

Ən çox yayılmış ön interfeys JavaScript çərçivələrindən bəziləri bunlardır:

  * **Angular**
  * **React**
  * **Vue**
  * **jQuery**

-----

JavaScript-in veb tətbiqlərinə dinamiklik və funksionallıq qatmaq üçün nə qədər əsaslı olduğunu və çərçivələrin inkişafı necə asanlaşdırdığını anladınızmı? Bu, HTML, CSS və JavaScript-dən ibarət olan ön interfeys üçlüyünün (*front end trinity*) tamamlanması deməkdir\! 🤔

