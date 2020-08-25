# macOS on ThinkPad X1 Yoga 3rd Gen, Model 20LE

[![macOS](https://img.shields.io/badge/macOS-Catalina-yellow.svg)](https://www.apple.com/macos/catalina/)
[![version](https://img.shields.io/badge/10.15.6-yellow)](https://support.apple.com/en-us/HT210642)
[![BIOS](https://img.shields.io/badge/BIOS-1.30-blue)](https://github.com/996icu/996.ICU/blob/master/LICENSE)
[![MODEL](https://img.shields.io/badge/Model-20LE-blue)](https://www.lenovo.com/gb/en/laptops/thinkpad/thinkpad-x1/ThinkPad-X1-Yoga-3rd-Gen/p/22TP2TXX13Y)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.6.0-green)](https://github.com/acidanthera/OpenCorePkg)
[![LICENSE](https://img.shields.io/badge/license-BSD-green)](https://github.com/996icu/996.ICU/blob/master/LICENSE)

<img align="right" src="https://i.imgur.com/I3yUS4Q.png" alt="machine-logo" width="300">

#### READ THE ENTIRE README.MD BEFORE YOU START.

#### I am not responsible for any damages you may cause.

### Should you find an error, or improve anything, be it in the config itself or in the my documentation, please consider opening an issue or a pull request to contribute.

`I am a full time student, who has limited knowledge about hackintosh. any help or improvement towards the existing problem of this hackintosh patch is appreciated.  `

<br>

> ## Recent

### 2020-8-24
* Added Experimental USB-C "Hotplug" Support (I'll explain it in /EFI-OC 0.6.0/README.md)

### 2020-8-23
* Corrected .plist file structure problem, now the `EFI` file can be used as Installation Boot file. 
* Added `DW1560 Wireless Card Support`


### 2020-8-22

* With the help of @Jamesxxx1997 , we successfully completed the adaptation of the patch to Non-BIOS Modded Machines, and introduced a new way to enable TB3 support. <br>However, this new method still need you to turn off Thunderbolt 3 BIOS Assist Mode, which cut the battery life at about 50%. <br>We are continuing investigating the ways to enable USB-C support without the cost of battery life. <br> Besides, the notification center gesture problem has successfully solved by using the `DEBUG` version of `VoodooRMI`.

* <s>Requested by @Jamesxxx1997, I'm now diving in to fix touchscreen support.</s> 

* TouchScreen successfully driven. Now both fingertouch and pentouch can work flawlessly. 

* Adjust ``ForceTouchMinPressure`` in the configuration file of ``VoodooRMI.kext`` to ``5`` to enable ForceTouch Support

### 2020-8-20

* Solved CPU C-Storm problem by removing  ``IOEletricity.kext`` and  ``SSDT-TB3.aml``
and turn "Thunderbolt BIOS Assist Mode" in BIOS from ```DISABLE``` to ```ENABLE```.  
* Uploaded detailed BIOS Modding Settings and the OC folder for USB installation


### 2020-8-19

* Initial Post

<br>

> ## SUMMARY:

**`x1y gen3-hackintosh is currently stable and fully functioning except for USB-C and Thunderbolt 3 Support.`**


| Fully functional | Non-functional | Semi-functional. Additional pulls needed and welcomed. |
| ---------------- | -------------- | ------------------------------------------------------ |
| Native Power Management✅ *Need BIOS modding              | Hibernation❌              | Thunderbolt 3 hotplug (Must disable TB3 BIOS assist mode, No TB3 Device for testing)⚠                                                      |
| Wi-Fi, Bluetooth, All Apple Continuity Functions including Sidecar, iCloud Suite(Generate your own SMBIOS information)✅ *Network Card Replacement (DW1820A) Needed               | Fingerprint Reader and WWAN Card❌ (Disable them in BIOS)             |                                                       |
| USB-A 3.0/2.0 Ports, WebCam and Complete Audio Functions, Sleep, Ethernet, iGPU, MicroSD Card Reader✅               |             |    Thunderbolt and full USB-C Support   ⚠*(TB3 Hotpatch Can be enabled at the cost of Battery life, Turn them on at your own risk; USB-C partial support is present)         |
| Full TouchScreen Support, Full TrackPoint and TrackPad Support, Up to 5 finger gestures, Ultra Smooth Experience  ✅*Using Voodoo RMI                |             |                                                      |
| BIOS Mod, unlocking ```Advanced``` Menu. ✅               | Unable to patch WWAN Whitelist❌             |                                                       |
| HIDPI (1680*945) using One-key HIDPI, HDMI Output & Hotplug✅               |              | 4K UHD via HDMI Port in theory, no device to test⚠                                                      |


<br>

> ## Note: 

(2020-8-20)<br>
By completely removing Thunderbolt 3 support, I was able to achieve less than 0.9W CPU Package idle power consumption and 8W overall power consumption of the system (7h+ Battery Life). 

Now the goal turns to achieve full USB-C support, while keeping Thunderbolt 3 from preventing deeper CPU C-States. 

<br>

<s>
(2020-8-19)
Currently the biggest problem of this post that I cannot solve by myself is the abnormally high power consumption of this machine. 

The Power Consumption Problem remain to be solved. Under my current Hardware & Software settings, the typical idle power consumption with Wi-Fi and Bluetooth ON is around 8.5-9 Watt, results in merely 5-6hrs Battery life, While in Windows 10 1903, The figure is usually around 5-6W. 

Under this circumstance, The CPU Package Power Consumption is around 2W when idle and 3.3-3.6w with Wechat in the background and Safari Opening 4 tabs + playing Online Video, almost exactly the same as the initial circumstance described in [THIS POST](https://github.com/tylernguyen/x1c6-hackintosh/issues/28). 

Although X1Y3 shares almost the same hardware spec with X1C6, turning on "Thunderbolt 3 BIOS Assist Mode" has barely no effect on CPU Package consumption, and turning it off cause completely NO USB-C functionality in macOS. The 2W average CPU Packge power consumption is the best case I can achieve via BIOS Modding (Undervolt, Disabling CFG Lock, set FCLK Frequency to 400Mhz, Enabling PCIe ASPM, etc. ). 

Besides, I have already applied USB Mapping and using NVMeFix.kext to enable ASPM of the NVMe SSD. The temperature of the ssd under macOS is identical to Windows 10. Without appropriate knowledge and experience, I'm unable to continue discovering which hardware is consumpting extra power or preventing the processer from entering deeper C-States. 
</s>


<br>

>## NOTICE:
1. If you encountered any problem while booting, you may want to add `-v` in `config.plist`-`NVRAM`-`Add`--`7C436110-AB2A-4BBB-A880-FE41995C9F82`--`boot-args` to figure out what is going on. This variable is present in every config files I provided, you can manually delete it after everything is all set. 
2. For Private reasons, I erased my SMBIOS Serials in the post. Please generate your SMBIOS using [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) to enjoy iCloud Suite and Apple Continuity Functions. 
3. The network card I prefer and used in this post is `DW1820A`, which is far cheaper than `DW1830`, and working flawlessly in X1 Yoga 3rd. If you are using a different network card such as `DW1560`, please choose the right  `config.plist` to power your card correctly. 
4. For best performance and bettery life, you may want to do BIOS Modding to unlock the Advanced menu of the BIOS. Detailed BIOS modding instructions and Modded BIOS Configs can be found at [HERE](https://github.com/M82589933/ThinkPad-X1-Yoga-3rd-Hackintosh/blob/master/docs/BIOS-Settings.md). 
5. The reason why I prefer using `BIOS Ver1.30` is that for me it is the only BIOS version that can drive touchscreen after S3 sleep. (There is a hardware designing flaw in X1 Yoga 3rd that the WACOM Touchscreen will disapper from the Device Manager in Windows 10 after recovering from S3 Sleep). You may apply BIOS Modding to any BIOS version, as it is not dependent on BIOS versions. 
6. HIDPI （1680*945）can be enabled through [One-Key HIDPI](https://github.com/xzhih/one-key-hidpi/)


<br>

> ## NEEDED:

* A macOS machine would be VERY useful: to create install drives, and for when your ThinkPad cannot boot. Though it is not completely necessary.  
* A Flash Drive with no less than 16G storage.   
* XCode or [ProperTree](https://github.com/corpnewt/ProperTree) for editing plist files.
* [MaciASL](https://github.com/acidanthera/MaciASL), for patching ACPI tables.  
* [MountEFI](https://github.com/corpnewt/MountEFI) to quickly mount EFI partitions.  
* [IOJones](https://github.com/acidanthera/IOJones), for diagnosis.  
* [Hackintool](https://www.insanelymac.com/forum/topic/335018-hackintool-v286/), for diagnostic ONLY, Hackintool should not be used for patching, it is outdated.

<br>


>## PROCEDURES

1. Download `.dmg` installation file of macOS 10.15.6 Catalina. 

2. Use [Balena Etcher](https://www.balena.io/etcher/) to flash the `.dmg` file into your USB disk. 

3. Mount the EFI partition of the USB disk, replace the entire `EFI` Folder with `EFI-Install`. 

4. Reboot and install macOS 10.15.6 Catalina. 

5. Put `/EFI-Opencore/OC` to `"Your SSD's EFI Partition"/EFI`, done. 

<br>

> ## SPECIFICATIONS

My ThinkPad X1 Yoga 3rd Gen configurations:

| Processor Number                                                                                                                   | # of Cores | # of Threads | Base Frequency | Max Turbo Frequency | Cache | Memory Types | Graphics      |
| :--------------------------------------------------------------------------------------------------------------------------------- | :--------- | :----------- | :------------- | :------------------ | :---- | :----------- | :------------ |
| [i7-8650U](https://ark.intel.com/content/www/us/en/ark/products/124968/intel-core-i7-8650u-processor-8m-cache-up-to-4-20-ghz.html) | 4          | 8            | 1.9 GHz        | 4.2 GHz             | 8 MB  | LPDDR3-2133  | Intel UHD 620 |

**Peripherals:**

```
Two USB 3.1 Gen 1 (Left USB Always On)
Two USB 3.1 Type-C Gen 2 / Thunderbolt 3 (Max 5120x2880 @60Hz)
HDMI 1.4b (Max 4096x2160 @30Hz)
Ethernet via ThinkPad Ethernet Extension Cable Gen 2: I219-LM Ethernet (vPro)
WWAN: Fibocom L850-GL (Intel XMM7360 LTE-A WWAN Modem)
TrackPoint: Synaptics PS/2
TrackPad: Synaptics PS/2
SSD: WD Black SN720 NVMe SSD 
```

**Display:**  
`14.0" (355mm) WQHD (2560x1440) AUO B140QAN02.0 500nit HDR`  
**Audio:**  
`ALC285 Audio Codec`  
**Thunderbolt:**  
`Intel JHL6540 (Alpine Ridge 4C) Thunderbolt 3 Bridge`


Additional used resources: 

- [dortania's Hackintosh guides](https://github.com/dortania)
- [dortania/ Getting Started with ACPI](https://dortania.github.io/Getting-Started-With-ACPI/)
- [dortania/ vanilla laptop guide](https://dortania.github.io/vanilla-laptop-guide/)
- [dortania/ opencore `laptop` guide](https://dortania.github.io/oc-laptop-guide/)
- [dortania/ opencore `desktop` guide](https://dortania.github.io/OpenCore-Desktop-Guide/)
- [dortania/ opencore `multiboot`](https://github.com/dortania/OpenCore-Multiboot)
- [dortania/ `USB map` guide](https://github.com/dortania/USB-Map-Guide)
- Daliansky's [Hackintool tutorial](https://translate.google.com/translate?js=n&sl=auto&tl=en&u=https://blog.daliansky.net/Intel-FB-Patcher-tutorial-and-insertion-pose.html).
- [WhateverGreen Intel HD Manual](https://github.com/acidanthera/WhateverGreen/blob/master/Manual/FAQ.IntelHD.en.md)
- [OpenCore Sanity Checker](opencore.slowgeek.com)
- [OpenCore Driver for ThinkLED Blinking patch](http://ocbook.tlhub.cn)
- [Fix iGPU Crashing after Updating to Catalina 10.15.6](https://blog.csdn.net/qq_42700192/article/details/104862057)
- [Adapting DW1820A Wireless Card](https://blog.daliansky.net/DW1820A_BCM94350ZAE-driver-inserts-the-correct-posture.html)


<br>

> ## OTHER

- [Jamesxxx1997/thinkpad-x1-yoga-2018-hackintosh](https://github.com/Jamesxxx1997/thinkpad-x1-yoga-2018-hackintosh)

- [StevenBPDRP/ThinkPad-X1yoga-Hackintosh](https://github.com/StevenBPDRP/ThinkPad-X1yoga-Hackintosh)

Create a pull request if you like to be added, final decision at my discreation.

<br>

> ## CONTACT:

Luyi1720839132@Gmail.com

<br>

> ## Credits and Thank You:
- [VoodooRMI Team](https://github.com/VoodooSMBus/VoodooRMI) for providing outstanding touchpad driver for ThinkPad series
- [@Jamesxxx1997](https://github.com/Jamesxxx1997) for friendly discussion and the testing the config for non-BIOS Modding Machines
- [@Colton-Ko](https://github.com/Colton-Ko/macOS-ThinkPad-X1C6) for the great features template and OpenCore Configuration Reference.<br>
- [@tylernguyen](https://github.com/tylernguyen/x1c6-hackintosh) for Power Consumption Improvement reference and BIOS Modding Reference.<br> 
- [@daliansky](https://github.com/daliansky) for all the hotpatches.<br>  
- [@corpnewt](https://github.com/corpnewt) for GibMacOS, EFIMount, and USBMap.  
- [@Sniki](https://github.com/Sniki) and [@goodwin](https://github.com/goodwin) for ALCPlugFix.  
- [@xzhih](https://github.com/xzhih) for one-key-hidpi. 
paranoidbashthot and \x for the BIOS mod to unlocked Intel Advance Menu.
- [@FlasHRender](https://github.com/FlasHRender) for CPU Friend Preference file from - [here](https://github.com/FlasHRender/Zenbook-S-UX391UA-hackintosh/tree/master/EFI/OC/Kexts/CPUFriendDataProvider.kext/Contents)

The greatest thank you and appreciation to [@Acidanthera](https://github.com/acidanthera), without whom's work, none of this would be possible.
