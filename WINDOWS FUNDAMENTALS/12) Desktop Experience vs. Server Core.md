## Desktop Experience və Server Core

**Windows Server Core** ilk dəfə **Windows Server 2008** ilə minimalistik Server mühiti kimi buraxıldı və yalnız əsas Server funksionallığını ehtiva edirdi. Nəticədə, Server Core **Desktop Experience (GUI)** həmkarından daha aşağı idarəetmə tələblərinə, daha kiçik hücum səthinə və daha az disk sahəsi və yaddaş istifadəsinə malikdir. Server Core-da bütün konfiqurasiya və texniki xidmət tapşırıqları **əmr sətri**, **PowerShell** vasitəsilə və ya MMC və ya **Remote Server Administration Tools (RSAT)** ilə uzaqdan idarəetmə yolu ilə yerinə yetirilir.

Server Core GUI-nin olmaması ilə daha kiçik bir izə (footprint) sahib olmağı hədəfləsə də, **Registry Editor**, **Notepad**, **System Information**, **Windows Installer**, **Task Manager** və **PowerShell** kimi bəzi qrafik proqramlar hələ də dəstəklənir. O, həmçinin **Active Directory Explorer**, **Process Explorer**, **Process Monitor** və **TCPView** kimi bəzi **Sysinternals** dəsti alətlərini də dəstəkləyir.

Windows Server 2019-dan etibarən, **Server Core** və ya **Desktop Experience** quraşdırma zamanı seçilməlidir və heç biri geri qaytarıla bilməz (yəni, Server Core-u Desktop Experience-ə çevirmək mümkün deyil). Quraşdırıldıqdan sonra, Server Core üçün ilkin quraşdırma **Sconfig** vasitəsilə edilə bilər ki, bu da bir **mətn əsaslı interfeysdir** (əslində WScript tərəfindən icra edilən VBScript-dir). **Sconfig** şəbəkə konfiqurasiyası, Windows yeniləmələrinin yoxlanılması/quraşdırılması, hesab idarəetməsi, uzaqdan idarəetmənin konfiqurasiyası, Windows-un aktivləşdirilməsi və daha çox kimi müxtəlif ümumi əmrləri yerinə yetirmək üçün istifadə olunur.

<img width="1220" height="560" alt="image" src="https://github.com/user-attachments/assets/c3d4f781-19ac-4188-8d9a-d25c6eb96302" />

Əlbəttə, mətni Azərbaycan dilinə tərcümə edirəm:

---

Müəyyən server tətbiqləri **Server Core** üzərində işləyə bilməz, bunlara **Microsoft Server Virtual Machine Manager 2019 (SCVMM)**, **System Center Data Protection Manager 2019**, **SharePoint Server 2019**, **Project Server 2019** daxildir.

Xülasə, **Server Core** **daha yüngül** və **daha az resurs tələb edəndir**, lakin **daha çətin öyrənmə əyrisi** var və idarə edilməsi **daha çətin** ola bilər. Onun həmçinin müəyyən GUI proqramlarından istifadə edərək idarəetmə tapşırıqlarını yerinə yetirmək kimi bəzi məhdudiyyətləri var.

İstənilən mühitdə, bir server quraşdırılması üçün **Desktop Experience** və ya **Server Core** istifadə edilməsi qərarı həm **biznes ehtiyacları** və serverin **nəzərdə tutulan istifadəsi**, həm də ona xidmət edən administratorların **bacarıq səviyyəsi** tərəfindən verilməlidir. Aşağıdakı cədvəl **Server Core** və **Desktop Experience** üzərində mövcud olan bəzi tətbiqləri göstərir. Bu, ümumi tətbiqlərin siyahısıdır və tam siyahı deyil.

| Tətbiq | Server Core | Desktop Experience |
| :--- | :--- | :--- |
| **Command Prompt (Əmr Sətri)** | Mövcuddur | Mövcuddur |
| **Windows PowerShell / Microsoft .NET** | Mövcuddur | Mövcuddur |
| **Regedit (Reyestr Redaktoru)** | Mövcuddur | Mövcuddur |
| **Diskmgmt.msc** | Mövcud Deyil | Mövcuddur |
| **Server Manager** | Mövcud Deyil | Mövcuddur |
| **Mmc.exe** | Mövcud Deyil | Mövcuddur |
| **Eventvwr** | Mövcud Deyil | Mövcuddur |
| **Services.msc** | Mövcud Deyil | Mövcuddur |
| **Control Panel** | Mövcud Deyil | Mövcuddur |
| **Windows Explorer** | Mövcud Deyil | Mövcuddur |
| **Taskmgr (Tapşırıq İdarəedicisi)** | Mövcuddur | Mövcuddur |
| **Internet Explorer və ya Edge** | Mövcud Deyil | Mövcuddur |
| **Remote Desktop Services (Uzaq Masaüstü Xidmətləri)** | Mövcuddur | Mövcuddur |




