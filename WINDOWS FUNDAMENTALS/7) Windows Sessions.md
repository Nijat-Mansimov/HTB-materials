## Windows Sessiyaları

### İnteraktiv (Interactive)

İnteraktiv və ya yerli giriş (logon) sessiyası, bir istifadəçinin öz etimadnamələrini daxil edərək yerli və ya domen sistemində autentifikasiya etməsi ilə başladılır. İnteraktiv giriş birbaşa sistemə daxil olmaqla, əmr sətri vasitəsilə `runas` əmrindən istifadə edərək ikinci bir giriş sessiyası tələb etməklə və ya **Uzaq Masaüstü (Remote Desktop)** qoşulması vasitəsilə başladılabilər.

### Qeyri-interaktiv (Non-interactive)

Windows-dakı qeyri-interaktiv hesablar standart istifadəçi hesablarından fərqlənir, çünki onlar giriş etimadnamələri tələb etmirlər. **3 növ qeyri-interaktiv hesab** var: **Local System Account** (Yerli Sistem Hesabı), **Local Service Account** (Yerli Xidmət Hesabı) və **Network Service Account** (Şəbəkə Xidməti Hesabı). Qeyri-interaktiv hesablar adətən **Windows əməliyyat sistemi tərəfindən istifadəçi müdaxiləsi olmadan xidmətləri və tətbiqləri avtomatik başlamaq** üçün istifadə olunur. Bu hesablarla əlaqəli heç bir şifrə yoxdur və adətən sistem yükləndikdə xidmətləri başlamaq və ya planlanmış tapşırıqları işlətmək üçün istifadə olunur.

Bu üç hesab növü arasında fərqlər var:

| Hesab | Təsviri |
| :--- | :--- |
| **Local System Account** (Yerli Sistem Hesabı) | Həmçinin **NT AUTHORITY\SYSTEM** hesabı kimi tanınır, bu, Windows sistemlərində **ən güclü hesabdır**. Windows xidmətlərini başlamaq kimi müxtəlif ƏS ilə əlaqəli tapşırıqlar üçün istifadə olunur. Bu hesab yerli administratorlar qrupundakı hesablardan **daha güclüdür**. |
| **Local Service Account** (Yerli Xidmət Hesabı) | **NT AUTHORITY\LocalService** hesabı kimi tanınır, bu, SYSTEM hesabının daha az imtiyazlı versiyasıdır və yerli istifadəçi hesabına bənzər imtiyazlara malikdir. Məhdud funksionallıq verilir və bəzi xidmətləri başlada bilər. |
| **Network Service Account** (Şəbəkə Xidməti Hesabı) | Bu, **NT AUTHORITY\NetworkService** hesabı kimi tanınır və standart bir domen istifadəçi hesabına bənzəyir. Yerli maşında Local Service Account-a bənzər imtiyazlara malikdir. Müəyyən şəbəkə xidmətləri üçün **autentifikasiya edilmiş sessiyalar qura bilər**. |
