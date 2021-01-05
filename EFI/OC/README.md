# Configuring OpenCore for your X1 Yoga 3rd


About Experimental USB-C Support: 
The USB-C support is implemented through injecting 'SSDT-USBC.aml'. It works as emulating  the `JHL6540 Thunderbolt 3 USB-C controller` as a `ExpressCard USB eXtensible Host Controller`, which makes it possible to completely poweroff the controller  after using the USB-C port, saving batterylife and prevent the controller from preventing the CPU goes into deeper C-State. 

However, this implementation comes with a few trade-offs: <br>

Firstly, The USB-C device can only be immediately recognized after the first cold boot up. <br>

Secondly, If you plug it off and reconnect, It will NOT be recognized unless you let the computer goes into sleep. The controller will automatically restart and recognize the device you replug. after that, your computer will lit up automatically. (usually take less than 1 minute). <br>

Last but not least, I found sometimes the charging status went abnormal if you swap the charging USB-C port: The battery will not be recognized as "Plugged in, Charging" or "Plugged in, Not Charging", but "Draining". However, the battery percentage will remain the same forever, as the laptop is actually be powered by the USB-C charger. 

The context of the SSDT is listed as below: 

```
DefinitionBlock ("", "SSDT", 2, "hack", "TYPC", 0x00000000)
{
    External (_SB_.PCI0.RP09, DeviceObj)
    External (_SB_.PCI0.RP09.PXSX, DeviceObj)

    Scope (\_SB.PCI0.RP09)
    {
        Name (RTBT, One)
    }

    Scope (\_SB.PCI0.RP09.PXSX)
    {
        Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
        {
            Return (One)
        }

        Method (_STA, 0, NotSerialized)  // _STA: Status
        {
            Return (0x0F)
        }

        Device (TBL3)
        {
            Name (_ADR, 0x00020000)  // _ADR: Address
            Method (_STA, 0, NotSerialized)  // _STA: Status
            {
                Return (0x0F)
            }

            Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
            {
                Return (Zero)
            }

            Device (TBTU)
            {
                Name (_ADR, Zero)  // _ADR: Address
                Name (_PRW, Package (0x02)  // _PRW: Power Resources for Wake
                {
                    0x6D, 
                    Zero
                })
                Device (RHUB)
                {
                    Name (_ADR, Zero)  // _ADR: Address
                    Method (GPLD, 2, Serialized)
                    {
                        Name (PCKG, Package (0x01)
                        {
                            Buffer (0x10){}
                        })
                        CreateField (DerefOf (PCKG [Zero]), Zero, 0x07, REV)
                        REV = One
                        CreateField (DerefOf (PCKG [Zero]), 0x40, One, VISI)
                        VISI = Arg0
                        CreateField (DerefOf (PCKG [Zero]), 0x57, 0x08, GPOS)
                        GPOS = Arg1
                        Return (PCKG) /* \_SB_.PCI0.RP09.PXSX.TBL3.TBTU.RHUB.GPLD.PCKG */
                    }

                    Method (GUPC, 1, Serialized)
                    {
                        Name (PCKG, Package (0x04)
                        {
                            Zero, 
                            0xFF, 
                            Zero, 
                            Zero
                        })
                        PCKG [Zero] = Arg0
                        Return (PCKG) /* \_SB_.PCI0.RP09.PXSX.TBL3.TBTU.RHUB.GUPC.PCKG */
                    }

                    Method (TPLD, 2, Serialized)
                    {
                        Name (PCKG, Package (0x01)
                        {
                            Buffer (0x10){}
                        })
                        CreateField (DerefOf (PCKG [Zero]), Zero, 0x07, REV)
                        REV = One
                        CreateField (DerefOf (PCKG [Zero]), 0x40, One, VISI)
                        VISI = Arg0
                        CreateField (DerefOf (PCKG [Zero]), 0x57, 0x08, GPOS)
                        GPOS = Arg1
                        CreateField (DerefOf (PCKG [Zero]), 0x4A, 0x04, SHAP)
                        SHAP = One
                        CreateField (DerefOf (PCKG [Zero]), 0x20, 0x10, WID)
                        WID = 0x08
                        CreateField (DerefOf (PCKG [Zero]), 0x30, 0x10, HGT)
                        HGT = 0x03
                        Return (PCKG) /* \_SB_.PCI0.RP09.PXSX.TBL3.TBTU.RHUB.TPLD.PCKG */
                    }

                    Method (TUPC, 1, Serialized)
                    {
                        Name (PCKG, Package (0x04)
                        {
                            One, 
                            Zero, 
                            Zero, 
                            Zero
                        })
                        PCKG [One] = Arg0
                        Return (PCKG) /* \_SB_.PCI0.RP09.PXSX.TBL3.TBTU.RHUB.TUPC.PCKG */
                    }

                    Device (UB21)
                    {
                        Name (_ADR, One)  // _ADR: Address
                        Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
                        {
                            Return (TUPC (0x09))
                        }

                        Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
                        {
                            Return (TPLD (One, One))
                        }
                    }

                    Device (UB22)
                    {
                        Name (_ADR, 0x02)  // _ADR: Address
                        Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
                        {
                            Return (GUPC (Zero))
                        }

                        Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
                        {
                            Return (GPLD (Zero, Zero))
                        }
                    }

                    Device (UB31)
                    {
                        Name (_ADR, 0x03)  // _ADR: Address
                        Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
                        {
                            Return (TUPC (0x09))
                        }

                        Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
                        {
                            Return (TPLD (One, One))
                        }
                    }

                    Device (UB32)
                    {
                        Name (_ADR, 0x04)  // _ADR: Address
                        Method (_UPC, 0, NotSerialized)  // _UPC: USB Port Capabilities
                        {
                            Return (GUPC (Zero))
                        }

                        Method (_PLD, 0, NotSerialized)  // _PLD: Physical Location of Device
                        {
                            Return (GPLD (Zero, Zero))
                        }
                    }
                }
            }
        }
    }
}

```

<br>


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
