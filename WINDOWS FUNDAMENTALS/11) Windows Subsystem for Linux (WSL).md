## Windows Subsystem for Linux (WSL)

**WSL (Windows Subsystem for Linux)** Linux ikilik fayllarının **Windows 10** və **Windows Server 2019** üzərində yerli olaraq işləməsinə imkan verən bir xüsusiyyətdir. O, əvvəlcə **Bash**, **Ruby** və `sed`, `awk`, `grep` və s. kimi yerli Linux əmr sətri alətlərini birbaşa öz Windows iş stansiyalarında işlətməli olan tərtibatçılar üçün nəzərdə tutulmuşdu. WSL-in 2019-cu ilin may ayında buraxılmış ikinci versiyası, **Hyper-V** xüsusiyyətlərinin bir alt dəstindən istifadə edərək **əsl Linux nüvəsini** tətbiq etdi.

WSL Administrator kimi **PowerShell** əmrini `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux` işlətməklə quraşdırıla bilər. Bu xüsusiyyət aktivləşdirildikdən sonra, ya **Microsoft Store**-dan bir Linux distrosu yükləyib quraşdıra bilərik, ya da seçdiyimiz Linux distrosunu əl ilə yükləyib əmr sətrindən açıb quraşdıra bilərik.

WSL **Bash.exe** adlı bir tətbiq quraşdırır ki, bu da bir **Bash qabığı (shell)** başlatmaq üçün sadəcə Windows konsoluna `bash` yazmaqla işlədilə bilər. Bu qabıqdan, standart Linux qovluq strukturu daxil olmaqla, bir Linux hostunun tam görünüşünə və hissiyyatına sahib oluruq.

```powershell
PS C:\htb> ls /

bin dev home lib lLib64 media opt root sbin srv tmp var
boot etc init 1lib32 Libx32 mnt proc run Snap sys usr
```

Host əməliyyat sistemindəki **C$ həcminə** və digər həcmlərə **mnt** qovluğu vasitəsilə daxil ola bilərik, bu da WSL hostu ilə Windows host ƏS arasında keçidi **qüsursuz** edir. Bu bash qabığında olduqdan sonra, istənilən Linux əsaslı əməliyyat sistemi ilə qarşılıqlı əlaqədə olduğumuz kimi WSL ilə qarşılıqlı əlaqədə ola bilərik: əmrləri işlətmək, yeniləmələri/paketləri quraşdırmaq və s.

```powershell
PS C:\htb> uname -a
Linux WS01 4.4.0-18362-Microsoft #476-Microsoft Frit Nov 01 16:53:00PST 2019 x86_64 x86 _64 x86_64 GNU/Linux
```
