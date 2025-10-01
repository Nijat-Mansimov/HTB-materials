**Well‑Known URIs**
.well-known standartı, RFC 8615 ilə müəyyən edilmişdir və veb saytın kök domenində standartlaşdırılmış kataloq rolunu oynayır. Bu təyin olunmuş yer, adətən veb serverdə `/.well-known/` yolu vasitəsilə əlçatan olur və xidmətlər, protokollar və təhlükəsizlik mexanizmləri ilə bağlı konfiqurasiya faylları və məlumatları mərkəzləşdirir.

Belə məlumatlar üçün ardıcıl bir yer yaradaraq, .well-known veb brauzerlər, tətbiqlər və təhlükəsizlik alətləri daxil olmaqla müxtəlif maraqlı tərəflərin kəşf və əldə etmə prosesini sadələşdirir. Bu sadələşdirilmiş yanaşma müştərilərə müəyyən konfiqurasiya fayllarını avtomatik tapıb götürməyə imkan verir — məsələn, bir veb saytın təhlükəsizlik siyasətinə daxil olmaq üçün müştəri `https://example.com/.well-known/security.txt` sorğusunu yerinə yetirə bilər.

İnternetə Təyin Olunan Rəqəmlər Təşkilatı (IANA) .well-known URI‑lərinin reyestrini saxlayır; hər bir giriş müxtəlif spesifikasiyalar və standartlar tərəfindən müəyyən edilmiş konkret məqsədə xidmət edir. Aşağıda IANA reyestrində qeyd olunmuş bəzi əhəmiyyətli nümunələri vurğulayan cədvəl verilmişdir:

| URI Sonluğu                    |                                                                                                                Təsviri |                  Status | İstinad                                                                                                                                                                            |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------: | ----------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `security.txt`                 |                  Təhlükəsizlik tədqiqatçılarına zəifliklər barədə məlumat vermək üçün əlaqə məlumatlarını ehtiva edir. |                   Daimi | RFC 9116                                                                                                                                                                           |
| `/.well-known/change-password` |                                   İstifadəçiləri parol dəyişmə səhifəsinə yönəltmək üçün standart bir URL təqdim edir. | Provisional (müvəqqəti) | [https://w3c.github.io/webappsec-change-password-url/#the-change-password-well-known-uri](https://w3c.github.io/webappsec-change-password-url/#the-change-password-well-known-uri) |
| `openid-configuration`         |      OpenID Connect üçün konfiqurasiya detalları müəyyənləşdirir (OAuth 2.0 üzərində qurulmuş identifikasiya qatıdır). |                   Daimi | [http://openid.net/specs/openid-connect-discovery-1_0.html](http://openid.net/specs/openid-connect-discovery-1_0.html)                                                             |
| `assetlinks.json`              |              Domenlə əlaqəli rəqəmsal aktivlərin (məsələn, tətbiqlərin) mülkiyyətinin doğrulanmasında istifadə olunur. |                   Daimi | [https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md](https://github.com/google/digitalassetlinks/blob/master/well-known/specification.md)         |
| `mta-sts.txt`                  | Poçt üçün SMTP MTA Strict Transport Security (MTA‑STS) siyasətini müəyyən edir və e‑poçt təhlükəsizliyini gücləndirir. |                   Daimi | RFC 8461                                                                                                                                                                           |

Bu yalnız IANA‑da qeydiyyatdan keçmiş bir çox .well-known URI‑lərindən kiçik bir nümunəsidir. Reyestrdəki hər bir giriş implementasiya üçün konkret təlimatlar və tələblər təqdim edir ki, .well-known mexanizminin müxtəlif tətbiqlər üçün standartlaşdırılmış şəkildə istifadə olunması təmin edilsin.

**Veb kəşfiyyatı (Web Recon) və .well-known**
Veb kəşfiyyatında .well-known URI‑ləri son nöqtələri və konfiqurasiya detalları kəşf etmək üçün qiymətli ola bilər — bu da penetrasiya testləri zamanı daha dərindən yoxlanıla bilər. Xüsusilə faydalı URI‑lərdən biri `openid-configuration` dir.

`openid-configuration` URI‑si OpenID Connect Discovery protokolunun bir hissəsidir — OpenID Connect isə OAuth 2.0 protokolunun üzərinə qoyulmuş identifikasiya qatıdır. Bir müştəri tətbiqi OpenID Connect‑dən istifadə etmək istədikdə, OpenID Connect təminatçısının konfiqurasiyasını `https://example.com/.well-known/openid-configuration` endpointinə müraciət edərək əldə edə bilər. Bu endpoint server haqqında metadata qaytaran JSON sənədini verir; nümunə olaraq belə bir JSON verilə bilər:

```json
{
  "issuer": "https://example.com",
  "authorization_endpoint": "https://example.com/oauth2/authorize",
  "token_endpoint": "https://example.com/oauth2/token",
  "userinfo_endpoint": "https://example.com/oauth2/userinfo",
  "jwks_uri": "https://example.com/oauth2/jwks",
  "response_types_supported": ["code", "token", "id_token"],
  "subject_types_supported": ["public"],
  "id_token_signing_alg_values_supported": ["RS256"],
  "scopes_supported": ["openid", "profile", "email"]
}
```

`openid-configuration` endpointindən əldə olunan məlumat bir neçə əlavə araşdırma imkanları təqdim edir:

* **Endpoint kəşfi:**

  * **Authorization Endpoint:** istifadəçi yetkiləndirmə sorğuları üçün URL‑in müəyyən edilməsi.
  * **Token Endpoint:** tokenlərin verildiyi URL‑in tapılması.
  * **Userinfo Endpoint:** istifadəçi məlumatlarını verən endpoint‑in yeri.
  * **JWKS URI:** `jwks_uri` serverin istifadə etdiyi kriptoqrafik açarları əks etdirən JSON Web Key Set (JWKS)‑i göstərir.

* **Dəstəklənən scope və response tiplərinin müəyyənləşdirilməsi:** hansı scope və cavab tiplərinin (response types) dəstəkləndiyini bilmək OpenID Connect implementasiyasının funksionallığını və məhdudiyyətlərini xəritələndirməyə kömək edir.

* **Algoritm detalları:** dəstəklənən imzalama alqoritmləri barədə məlumat təhlükəsizlik tədbirlərini anlamaqda faydalı ola bilər.

IANA reyestrini araşdırmaq və müxtəlif `.well-known` URI‑ləri ilə təcrübə aparmaq veb kəşfiyyatı üçün çox faydalıdır. `openid-configuration` nümunəsində göstərildiyi kimi, bu standartlaşdırılmış URI‑lər kritik metadata və konfiqurasiya detalları üçün strukturlaşdırılmış giriş təmin edir və təhlükəsizlik mütəxəssislərinə bir veb saytın təhlükəsizlik mənzərəsini ətraflı şəkildə xəritələndirməyə imkan yaradır.
