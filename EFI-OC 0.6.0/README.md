<font color="#FF0000"> WARNING: Currently the config file cannot be used as regula (INSTALLATION) START UP file. If you use my folder, you will encounter a strange problem:
   
```
OCS: No Schema for Vault at 4 index!
OC: Configuration requires vault but no vault provided! 
```

Clearly I have already implemented "Vault: Optional" in config.plist. However, it pops up everytime you try to boot directly from USB Disk or HDD instead of boot options. 

I will take time to reconstruct `config.plist` using OC 0.6.0 official `sample.plist` some day, hope it can solve this problem. 

</font>

# Configuring OpenCore for your X1 Yoga 3rd


##  Finding the correct config.plist

As you can see, there are four different `config.plist` in this folder: <br>
* For users who `CANNOT PERFORM BIOS MODDING` and `DO NOT USE DW1820A`: 
Copy  `Config-modded bios no, dw1820a no.plist` to \EFI\OC\ folder, rename it as `config.plist`. <br>

* For users who `CANNOT PERFORM BIOS MODDING` and `USE DW1820A`: 
Copy  `Config-modded bios no, dw1820a yes.plist` to \EFI\OC\ folder, rename it as `config.plist`. <br>

* For users who `HAVE DONE BIOS MODDING` and `DO NOT USE DW1820A`: 
Copy  `Config-bios modded, dw1820a no.plist` to \EFI\OC\ folder, rename it as `config.plist`. <br>

* For users who `HAVE DONE BIOS MODDING` and `USE DW1820A`: 
Copy  `Config-bios modded, dw1820a yes` to \EFI\OC\ folder, rename it as `config.plist`. <br>


* For users who need to clean install macOS 10.15.x Catalina: 
If you have successfully done BIOS Modding and adjusted the settings as recommended:
use `Config-bios modded, dw1820a no.plist`. 

If you cannot perform BIOS Modding: 
use  `Config-modded bios no, dw1820a no.plist`. 

<br>

## Generate your own SMBIOS Information

For privacy reasons, I intentially wiped all SMBIOS information in the configuration files. 
You need to generate your unique SMBIOS info by yourself (recommend to use GenSMBIOS), and inject them into your `Config.plist`. Remember: DO NOT SHARE THEM TO ANYONE. 

<br>

## Customization

* FileVault compatibility:
    * Misc -> Boot
        * `PollAppleHotKeys` set to `YES`(While not needed can be helpful)
    * Misc -> Security
        * `AuthRestart` set to `YES`(Enables Authenticated restart for FileVault 2 so password is not required on reboot. Can be considered a security risk so optional)
    * NVRAM -> Add -> 4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14
        * `UIScale` set to `02` for high resolution small displays
    * UEFI -> Input
        * `KeySupport` set to `YES`(Only when using OpenCore's builtin input, users of OpenUsbKbDxe should avoid)
    * UEFI -> Output
        * `ProvideConsoleGop` to `YES`
    * UEFI -> ProtocolOverrides
        * `FirmwareVolume` set to `YES`
        * `AppleSmcIo` set to `YES`(this replaces VirtualSMC.efi)
    * UEFI -> Quirks
        * `RequestBootVarRouting` set to `YES`
* Personalization:
    * `ShowPicker` is `NO`. Use `Esc` during boot to show picker when needed.
