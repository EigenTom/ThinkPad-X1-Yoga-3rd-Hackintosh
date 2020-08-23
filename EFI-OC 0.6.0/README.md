# Configuring OpenCore for your X1 Yoga 3rd


##  Finding the correct config.plist

```
if (you cannot perform BIOS Modding) : 
    goto "/EFI-No BIOSMod" 
elif：
    goto "/EFI-Yes BIOSMod"
else:
    Go buy one ThinkPad X1 Yoga 3rd !
    
if (using DW1820A): 
    use "config.plist with '1820A' in its filename"
elif (using DW1560):
    use "config.plist with '1560' in its filename"
else: 
    use "config.plist with 'NoCard' in its filename"
```        

### Note: 
1. Every 'config.plist' COME WITH `boot-args--'-v'` and `showpicker = True`. It makes it easier for you to deal with potential booting problems, and make the file capable for acting as Installation Boot Files. You may want to manually change or delete these arguments after everything is settled down. 
2.  For privacy reasons, SMBIOS information has been wiped out, you need to generate your own SMBIOS and inject them into the file before you startup or doing clean install. 


<br>


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
