Asus A455LA-WX667D Hackintosh
===================


**Summary**

## Languages:

- **English**
- **Bahasa Indonesia**


-------
**Specs :**

 - Processor : Intel Core i3 5005U 2.0 Ghz
 - IGPU : Intel HD Graphics 5500
 - RAM : 4GB DDR3 1600MHz 
 - Built-in Audio : Connexant CX20751_2
 - Wifi : **Replaced** with AR9285
 - Ethernet : Realtek RTL8168GU/8111GU
 - Bootloader : CLoverEFI
 

**Working :**

 - Native Power Management
 - Intel HD Graphics 5500
 - Ethernet
 - USB ports
 - Battery status
 - Keyboard
 - TrackPad with Gestures 
 - Internal Audio & Microphone
 - Wifi AR9285
 - Brightness
 - Backlight Control
 - VGA Ports
 - HDMI Ports
 - Shutdown / Reboot
 - Sleep
 - iMessage
 - Webcam


----------

**Setup guide**
-----------

**BIOS Configuration**

Bios Config | Setting 
:---:| :---:
Security -> Secure Boot | Disabled
Intel Virtualization    | Disabled
VT-d | Disabled
Graphics Configuration -> DVMT Pre-Allocation | 64M
USB Configuration -> XHCI Pre-Boot Mode | Smart Auto / Enabled
SATA Mode | AHCI
Boot -> Launch CSM | Enabled

-------------
**Create USB Installer**
-------------

1. Download macOS from the Mac App Store. It downloads to your Applications folder as a single ”Install” file, such as Install macOS High Sierra.

2. When the installer opens, quit it without continuing installation.

3. Open Terminal, which is in the Utilities folder of your Applications folder.

4. Type one of the following commands in Terminal. These all assume that the installer is in your Applications folder, and the name of the volume that you're using for the bootable installer is `MyVolume`. If your volume is named differently, replace `MyVolume` with the name of your volume.

**High Sierra :**

    sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

**Sierra :**

    sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Sierra.app

**El Capitan :**

    sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app

**Yosemite :**

    sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ Yosemite.app
    
5. **DO NOT** Close Terminal until it finish copying all files on the USB.


-------------


**Installing CLover Bootloader on the USB Installer**
-------------

[![Clover Setting](http://img.ziggi.org/Q7mMDAVg.png "Clover Setting")](http://img.ziggi.org/Q7mMDAVg.png "Clover Setting")

1. Download Clover Bootloader from this link : [CloverEFI Bootloader](https://sourceforge.net/projects/cloverefiboot/ "CloverEFI")

2. Open `Clover_v*.pkg` in the Tools directory of this repository or from the link above and install it on your USB, customize and select 

3. Open `/Volumes/EFI/Clover/` , Copy and Replace the entire `EFI/CLOVER` Folder in the ESP of your USB Drive with `EFI/CLOVER` folder in this repository.

4. Open `EFI/CLOVER/config.plist` in the ESP of your USB Drive. Run Clover Configurator included in the `Tools/` folder in the repository. Use Macbook Air 6,2 / Macbook Pro 12,1. Generate Serial Number, BoardSerialNumber, SmUUID in SMBIOS tab.

5. Also copy Clover Bootloader installer .pkg file  & other files you need to your install macOS USB  for Post-installation.

6. Boot the USB Installer !

----------------

**Post-install**
----------------

- Install Clover EFI Bootloader to your **HDD** and select Clover UEFI.

- Copy Clover files from your USB to yout HDD. *like you did with your USB Installer before*
--------------

**Summary of problems and fixes**
--------------

| Features  |   Fixes      |
| :------------: | :------------: |
|     Intel HD 5500 QE/Ci           |   Inject Intel  ig-platform **0x16260006**   & SMBIOS MBP12.1        |   
|      Audio         |        AppleALC.kext & Lilu.kext + LayoutID 28        |  
| Wifi AR9285 | WifiInjector.kext &  Rehabman wifi_AR9285-RP02-PXSX patch
| Brightness |  ACPIBacklight.kext Apply DSDT patches for iGPU, brightness fix PNLF Broadwell (**10.12.4** below only) *for **10.12.4+** above use AppleBacklight method https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/
|Brightness Control Key |  Replace Method Q0E & Q0F in your DSDT with code i attached below   | 
| Touchpad & Keyboard  | ApplePS2SmartTouchpad.kext 
| Battery Status | ACPIBatteryManager.kext & patch DSDT 

----------------

**DSDT Patches**
---------------

Patches are from Rehabman Laptop Patches repo Source :  http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master

- ADBG Error (Only use it if u get ADBG Error)
- fix _WAK  Arg0 v2
- fix HPET
- SMBUS fix
- IRQ fix
- RTC fix
- OS Check Fix ( Windows 7)
- Rename GFX0 to IGPU
- Brightness fix
- AR9285 Wifi RP02-PXSX
- And for the Brightness key replace method Q0E & Q0F with this code :

```c
Method (_Q0E, 0, NotSerialized)  
{
    If (ATKP)
    {
        \_SB.ATKD.IANE (0x20)
    }
}
Method (_Q0F, 0, NotSerialized)
{
    If (ATKP)
    {
        \_SB.ATKD.IANE (0x10)
    }
}
```
--------------------

-----------------------
# TUTORIAL BAHASA INDONESIA

### Buat USB Installer
1. Download macOS dari Mac App Store. Download ke Aplikasi Anda, seperti Install macos High Sierra.

2. Saat installer terbuka, berhenti tanpa instalasi lagi.

3. Buka Terminal, yang ada di folder Utilitas folder Applications anda.

4. Ketik salah satu dari perintah berikut di Terminal. Pastikan bahwa installer ada di folder Applications Anda, dan nama volume yang Anda gunakan untuk installer bootable adalah MyVolume. Jika volume Anda diberi nama berbeda, ganti MyVolume dengan nama volume Anda. 

**High Sierra :**

    sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume

**Sierra :**

    sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ macOS\ Sierra.app

**El Capitan :**

    sudo /Applications/Install\ OS\ X\ El\ Capitan.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ El\ Capitan.app

**Yosemite :**

    sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume --applicationpath /Applications/Install\ OS\ X\ Yosemite.app
    
5.'**JANGAN** Tutup Terminal sampai selesai menyalin semua file di USB.


----------------
Memasang Bootloader Clover pada USB Installer
----------------
[![Clover EFI](http://img.ziggi.org/Q7mMDAVg.png "Clover EFI")](http://img.ziggi.org/Q7mMDAVg.png "Clover EFI")

1. Download Clover Bootloader dari link ini: [CloverEFI Bootloader](https://sourceforge.net/projects/cloverefiboot/ "CloverEFI")

2. Buka Clover_v * .pkg di direktori Tools dari repositori ini atau dari link di atas dan menginstalnya pada USB anda, sesuaikan dan pilih

3. Open / Volumes / EFI / Clover /, Copy dan Ganti Seluruh Folder EFI / CLOVER di ESP dari USB Drive Anda dengan folder EFI / CLOVER dalam repositori ini.

4. Buka EFI / CLOVER / config.plist di ESP USB Drive Anda. Jalankan Clover Configurator yang disertakan dalam Tools / folder di repositori. Pilih Macbook Air 6,2 / Macbook Pro 12,1. Buat Serial Number, BoardSerialNumber, SmUUID di tab SMBIOS.

5. Juga copy file installer Clover Bootloader .pkg & file lainnya yang Anda perlukan untuk menginstal macos USB untuk Post-installation.

6. Boot USB Installer!
----------------

**Post-install**
----------------
- Install Clover EFI Bootloader ke ** HDD ** Anda dan pilih Clover UEFI.

- Salin file Clover dari USB ke HDD Anda. * seperti yang Anda lakukan dengan USB Installer Anda sebelumnya *

--------------------
Ringkasan masalah dan perbaikan
------------------

| Features  |   Fixes      |
| :------------: | :------------: |
|     Intel HD 5500 QE/Ci           |   Inject Intel  ig-platform **0x16260006**   & SMBIOS MBP12.1        |   
|      Audio         |        AppleALC.kext & Lilu.kext + LayoutID 28        |  
| Wifi AR9285 | WifiInjector.kext &  Rehabman wifi_AR9285-RP02-PXSX patch
| Brightness |  ACPIBacklight.kext Apply DSDT patches for iGPU, brightness fix PNLF Broadwell (**10.12.4** below only) *for **10.12.4+** above use AppleBacklight method https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/
|Brightness Control Key |  Rubah Method Q0E & Q0F di DSDT dengan kode dibawah   | 
| Touchpad & Keyboard  | ApplePS2SmartTouchpad.kext 
| Battery Status | ACPIBatteryManager.kext & patch DSDT 
------------


**DSDT Patches**
---------------

Patches are from Rehabman Laptop Patches repo Source :  http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master

- ADBG Error (Only use it if u get ADBG Error)
- fix _WAK  Arg0 v2
- fix HPET
- SMBUS fix
- IRQ fix
- RTC fix
- OS Check Fix ( Windows 7)
- Rename GFX0 to IGPU
- Brightness fix
- AR9285 Wifi RP02-PXSX
- Brightness key ganti kode yang ada di method Q0E & Q0F dengan kode :

```c
Method (_Q0E, 0, NotSerialized)  
{
    If (ATKP)
    {
        \_SB.ATKD.IANE (0x20)
    }
}
Method (_Q0F, 0, NotSerialized)
{
    If (ATKP)
    {
        \_SB.ATKD.IANE (0x10)
    }
}
```


-------------
**Other resources**
-------------

- https://www.tonymacx86.com/threads/guide-laptop-backlight-control-using-applebacklightinjector-kext.218222/
- https://www.tonymacx86.com/threads/guide-patching-laptop-dsdt-ssdts.152573/
- http://raw.github.com/RehabMan/Laptop-DSDT-Patch/master
- Google is your best friend.
- Hackintosh Indonesia




------------


