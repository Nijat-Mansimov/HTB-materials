responder for linux

```shell
sudo responder -I ens224
```

responder for windows

```powershell
Import-Module .\Inveigh.ps1
```
```powershell
(Get-Command Invoke-Inveigh).Parameters
```
```powershell
Invoke-Inveigh Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```

responder as an application for windows

```powershell
.\Inveigh.exe
```
```powershell
GET NTLMV2UNIQUE
```


prevention

<img width="1399" height="561" alt="image" src="https://github.com/user-attachments/assets/0dcb29ff-fb3f-4b23-bbaa-c1d831ef9bc9" />
Computer Configuration --> Administrative Templates --> Network --> DNS Client keçərək və "Turn OFF Multicast Name Resolution." funksiyasını aktiv etməklə Qrup Siyasətində LLMNR-ni söndürə bilərik.

<img width="993" height="524" alt="image" src="https://github.com/user-attachments/assets/4390f9a3-95ee-4b30-9243-2d52d361f5e0" />
NBT-NS Qrup Siyasəti vasitəsilə deaktiv edilə bilməz, lakin hər bir hostda yerli olaraq deaktiv edilməlidir. Bunu İdarəetmə Paneli altından Şəbəkə və Paylaşım Mərkəzini açaraq, Adapter parametrlərini dəyişdirin üzərinə klikləməklə, onun xassələrinə baxmaq üçün adapterin üzərinə sağ klikləməklə, İnternet Protokolu 4 Versiyasını (TCP/IPv4) seçməklə və Xüsusiyyətlər düyməsini klikləməklə, Qabaqcıl üzərinə klikləməklə və WINS sekmesini seçməklə və nəhayət, TCP/IP üzərindən NetBIOS-u Disable seçərək edə bilərik.

```powershell
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
NBT-NS-ni birbaşa GPO vasitəsilə söndürmək mümkün olmasa da, biz PowerShell skriptini Computer Configuration --> Windows Settings --> Script (Startup/Shutdown) --> powershell command kimi bir şeylə işə sala bilərik:
Yerli Qrup Siyasəti Redaktorunda Başlanğıc üzərinə iki dəfə klik etməli, PowerShell Skriptləri sekmesini seçməli və əvvəlcə Windows PowerShell skriptlərini işə salmaq üçün "Bu GPO üçün skriptləri aşağıdakı ardıcıllıqla işlədin" seçimini etməli, sonra Əlavə et üzərinə klikləyib skripti seçməliyik. Bu dəyişikliklərin baş verməsi üçün ya hədəf sistemi yenidən başlatmalı, ya da şəbəkə adapterini yenidən başlatmalı olacağıq.

<img width="1170" height="521" alt="image" src="https://github.com/user-attachments/assets/7a50a9b1-47ae-45cf-859d-c4316e9f8b25" />

Bunu bir domendəki bütün hostlara çatdırmaq üçün biz Domen Nəzarətçisində Qrup Siyasətinin İdarə edilməsindən istifadə edərək GPO yarada və skripti skriptlər qovluğunda SYSVOL paylaşımında yerləşdirə və sonra onu UNC yolu ilə çağıra bilərik:

\\inlanefreight.local\SYSVOL\INLANEFREIGHT.LOCAL\skriptləri

GPO xüsusi OU-lara tətbiq edildikdən və həmin hostlar yenidən işə salındıqdan sonra, skript SYSVOL paylaşımında hələ də mövcud olduğu və şəbəkə üzərindən host tərəfindən əlçatan olması şərti ilə skript növbəti yenidən yükləmə zamanı işləyəcək və NBT-NS-ni deaktiv edəcək.

<img width="1559" height="580" alt="image" src="https://github.com/user-attachments/assets/824252f9-6914-407b-9c27-5eb412d9e51b" />

Digər yumşaldıcı tədbirlərə LLMNR/NetBIOS trafikini bloklamaq üçün şəbəkə trafikinin filtrlənməsi və NTLM relay hücumlarının qarşısını almaq üçün SMB İmzalanmasının aktivləşdirilməsi daxildir. Şəbəkəyə müdaxilənin aşkarlanması və qarşısının alınması sistemləri də bu fəaliyyəti azaltmaq üçün istifadə edilə bilər, şəbəkə seqmentasiyası isə düzgün işləməsi üçün LLMNR və ya NetBIOS-un aktivləşdirilməsini tələb edən hostları təcrid etmək üçün istifadə edilə bilər.



