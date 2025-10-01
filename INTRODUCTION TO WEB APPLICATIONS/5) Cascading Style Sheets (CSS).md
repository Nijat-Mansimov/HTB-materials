## CSS (Cascading Style Sheets)

**CSS (Cascading Style Sheets)** **HTML elementlərini formatlaşdırmaq və stilini təyin etmək** üçün HTML ilə birlikdə istifadə edilən **stil cədvəli dilidir** (*stylesheet language*). HTML kimi, CSS-in də bir neçə versiyası var və hər sonrakı versiya HTML elementlərini formatlaşdırmaq üçün istifadə edilə bilən **yeni imkanlar toplusunu** təqdim edir. Brauzerlər də bu yeni xüsusiyyətləri dəstəkləmək üçün onunla birlikdə yenilənir.

### Nümunə

Əsas səviyyədə, CSS hər bir **class-ın** və ya **HTML elementinin tipinin** (yəni, `body` və ya `h1`) stilini müəyyən etmək üçün istifadə olunur, belə ki, həmin səhifədəki **istənilən element** CSS faylında təyin olunduğu kimi təmsil olunsun. Buna **şrift ailəsi, şrift ölçüsü, fon rəngi, mətn rəngi və düzülüşü** və daha çox şey daxil ola bilər.

```css
body {
  background-color: black;
}
h1 {
  color: white;
  text-align: center;
}
p {
  font-family: helvetica;
  font-size: 10px;
}
```

Əvvəllər qeyd edildiyi kimi, müəyyən HTML elementləri üçün **unikal ID-lər** və ya **class adları** təyin etməyimizin səbəbi budur ki, daha sonra lazım olduqda onlara **CSS** və ya **JavaScript** daxilində müraciət edə bilək.

-----

## Sintaksis

CSS hər bir **HTML elementinin** və ya **class-ın** stilini **buruq mötərizələr** (`{}`) arasında müəyyən edir, burada xüsusiyyətlər öz dəyərləri ilə təyin olunur (yəni, `element { xüsusiyyət : dəyər; }`).

Hər bir HTML elementinin CSS vasitəsilə təyin edilə bilən **bir çox xüsusiyyəti** var, məsələn, **`height`** (hündürlük), **`position`** (mövqe), **`border`** (kənar xətt), **`margin`** (kənar boşluq), **`padding`** (daxili boşluq), **`color`** (rəng), **`text-align`** (mətn düzülüşü), **`font-size`** (şrift ölçüsü) və yüzlərlə başqa xüsusiyyət. Bütün bunlar birləşdirilərək **vizual cəlbedici veb səhifələr** dizayn etmək üçün istifadə edilə bilər.

CSS, elementləri hərəkət etdirməkdən tutmuş **təkmil 3D animasiyalara** qədər geniş istifadə sahələri üçün **qabaqcıl animasiyalar** üçün istifadə edilə bilər. Animasiyalar üçün bir çox CSS xüsusiyyəti mövcuddur, məsələn, **`@keyframes`**, **`animation`**, **`animation-duration`**, **`animation-direction`** və bir çox başqaları.

-----

## İstifadəsi

CSS tez-tez **sürətli hesablamalar** aparmaq, müəyyən HTML elementlərinin **stil xüsusiyyətlərini dinamik olaraq tənzimləmək** və ya **klaviş basmaları** və ya **siçan kursorunun yeri** əsasında **qabaqcıl animasiyalara** nail olmaq üçün **JavaScript** ilə birlikdə istifadə olunur.

Aşağıdakı nümunə HTML və JavaScript ilə istifadə edildikdə CSS-in bu cür imkanlarını gözəl şəkildə nümayiş etdirir ("Parallax Depth Cards - by Andy Merskin on **CodePen**"):

CSS-in veb tətbiqinin görünüşü və hissiyatı üçün nə qədər vacib olduğunu və HTML elementlərini necə formalaşdırdığını anlamaq aydındır? CSS elementlərinin təhlükəsizliyə təsirinin olub-olmaması ilə maraqlanırsınızmı? 🤔

https://codepen.io/andymerskin/pen/XNMWvQ

Bu, HTML və CSS-in veb inkişafının **ən əsas təməl daşları** arasında olmasına baxmayaraq, düzgün istifadə edildikdə **vizual olaraq heyrətamiz veb səhifələr** qurmaq üçün istifadə edilə biləcəyini göstərir ki, bu da veb tətbiqləri ilə qarşılıqlı əlaqəni **daha asan və istifadəçi üçün daha rahat təcrübə** edə bilər.

Bundan əlavə, CSS **XML** kimi digər dillərin stillərini tətbiq etmək üçün və ya **SVG** elementləri daxilində istifadə edilə bilər və həmçinin **bütün mobil tətbiqi İstifadəçi İnterfeyslərini (UI) dizayn etmək** üçün müasir mobil inkişaf platformalarında da istifadə oluna bilər.

---

## Çərçivələr (*Frameworks*)

Çoxları CSS-in inkişafının **çətin** olduğunu düşünə bilər. Digərləri isə hər veb səhifədə **bütün HTML elementlərinin stilini və dizaynını əl ilə təyin etməyin səmərəsiz** olduğunu iddia edə bilər. Buna görə də, **çoxlu CSS çərçivələri** təqdim edilmişdir ki, bunlar **gözəl HTML elementləri yaratmağı daha sürətli və asan etmək** üçün **CSS stil cədvəlləri və dizaynlarının toplusunu** ehtiva edir.

Bundan başqa, bu çərçivələr **veb tətbiqi istifadəsi üçün optimallaşdırılıb**. Onlar **JavaScript ilə istifadə** edilmək və bir veb tətbiqi daxilində **geniş istifadə** üçün nəzərdə tutulmuşdur və adətən müasir veb tətbiqlərində tələb olunan elementləri ehtiva edir. Ən çox yayılmış CSS çərçivələrindən bəziləri bunlardır:

* **Bootstrap**
* **SASS**
* **Foundation**
* **Bulma**
* **Pure**

---

CSS çərçivələrinin inkişaf prosesini necə sürətləndirdiyini və veb tətbiqlərinin dizaynını necə yaxşılaşdırdığını başa düşdükmü? Gəlin veb inkişafının son əsas komponentinə – JavaScript-ə keçək! 🤔











