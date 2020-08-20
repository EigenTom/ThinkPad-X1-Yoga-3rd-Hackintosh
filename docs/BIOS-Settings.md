# Hardware and BIOS:
 

> ## BIOS Settings:

At the minimum, these BIOS settings must be made to install and run macOS without any problems:

| Main Menu | Sub 1       | Sub 2                                         | Sub 3                                                              |
| --------- | ----------- | --------------------------------------------- | ------------------------------------------------------------------ |
|           |             | >> Power                                      | Sleep State `Linux`                                                |
|           | >> Security | >> Security Chip                              | Security Chip `DISABLED`                                           |
|           |             | >> Fingerprint                                | Predesktop Authentication `DISABLED`                               |
|           |             | >> Secure Boot Configuration                  | Secure Boot `DISABLED`                                             |
|           |             |                                               | Press `Clear All Secure Boot Keys`                                 |
|           |             | >> Intel SGX                                  | Intel SGX Control `DISABLED`                                       |
|           | >> Config   | >> Network                                    | Wake on Lan `DISABLED`                                             |
|           |             |                                               | Wake on Lan from Dock `DISABLED`                                   |
|           |             |                                               | UEFI IPv4 Network Stack `DISABLED`                                 |
|           |             |                                               | UEFI IPv6 Network Stack `DISABLED`                                 |
|           | >> Startup  | UEFI/Legacy Boot `UEFI Only`                  |                                                                    |
|           |             | CSM Support `No` (per OpenCore Documentation) |                                                                    |

* You could also disable hardware devices you do not need to save power:

| Main Menu | Sub 1       | Sub 2                                         | Sub 3                                                              |
| --------- | ----------- | --------------------------------------------- | ------------------------------------------------------------------ |
|           | >> Security | >> I/O Port Access                            | Wireless WAN `DISABLED`                                            |
|           |             |                                               | Fingerprint Reader `DISABLED`                                      |
|           |             |                                               | Memory Card Slot `DISABLED`                                        |
|           | >> Config   | >> USB                                        | Always On USB `DISABLED`                                           |

* If you do not use Thunderbolt 3 hotplug in macOS (don't mind shutting down the machine to connect TB3 devices), this will drastically lower power consumption:

| Main Menu | Sub 1       | Sub 2                                         | Sub 3                                                              |
| --------- | ----------- | --------------------------------------------- | ------------------------------------------------------------------ |
|           | >> Config   | >> Thunderbolt (TM) 3                         | Thunderbolt BIOS Assist Mode `Enabled`                             |
|           |             |                                               | Thunderbolt(TM) Device `Enabled`                                   |

* If you do do want to use Thunderbolt 3 hotplug in macOS (at the expense of idle power consumption):

| Main Menu | Sub 1       | Sub 2                                         | Sub 3                                                              |
| --------- | ----------- | --------------------------------------------- | ------------------------------------------------------------------ |
|           | >> Config   | >> Thunderbolt (TM) 3                         | Thunderbolt BIOS Assist Mode `Disabled`                            |
|           |             |                                               | Thunderbolt(TM) Device `Enabled`                                   |


> ## Modding your BIOS:
### A modded BIOS will allow for more optimizations to be made for macOS and will overall make your hackintosh better. The default `config.plist` for this repository is meant to accommodate a modded BIOS with appropriate settings. <br>Look up for [HERE](https://github.com/tylernguyen/x1c6-hackintosh/blob/master/docs/1_README-HARDWAREandBIOS.md) for detailed BIOS Modding Instructions. 

### Note: For X1 Yoga 3rd Gen, The BIOS Chip is located next to the WWAN Card on the right: 

![BIOS CHIP LOCATION](https://s1.ax1x.com/2020/08/20/dGArTI.md.jpg)

> ## Modded BIOS Settings:
The following are further optimization settings that can be figured once your BIOS is modded.

> * These settings are universally recommended optimizations for your hackintosh:

| Main Menu    | Sub 1                  | Sub 2                              | Sub 3                             | Sub 4                    |
|--------------|------------------------|------------------------------------|-----------------------------------|--------------------------|
| Advanced Tab | >> Intel Advanced Menu | >> System Agent (SA) Configuration | >> Graphics Configuration         | DVMT Pre-Allocated `64M` |
|              |                        | >> Power & Performance             | >> CPU - Power Management Control | >> CPU Lock Configuration (Last item, scroll up/down until you see it) CFG Lock `Disabled`|
|              |                        

* I also recommend undervolting your machine regarless of your usage, the following are stable settings for my x1c6 with `i7-8650U`, verified by stress testing with `Prime95` and `Heaven Benchmark`, your may be worse or better, please do your own testing. In addition, I suggest you repaste your machine with an aftermarket thermal paste for lower temps and a better undervolt.

| Main Menu    | Sub 1                  | Sub 2                              | Sub 3                                                                  | Sub 4                     |
|--------------|------------------------|------------------------------------|------------------------------------------------------------------------|---------------------------|
| Advanced Tab | >> Intel Advanced Menu | >> OverClocking Performance Menu   | OverClocking Feature| `Enabled`                                         |                           |
|              |                        |                                    | >> Processor                                                           | Voltage Offset `105`      |
|              |                        |                                    |                                                                        | Offset Prefix `-`         |
|              |                        |                                    | >> GT                                                                  | GT Voltage Offset `85`    |
|              |                        |                                    |                                                                        | Offset Prefix `-`         |
|              |                        |                                    |                                                                        | GTU Voltage Offset `85`   |
|              |                        |                                    |                                                                        | Offset Prefix `-`         |
|              |                        |                                    | >> Uncore                                                              | Uncore Voltage Offset `80`|
|              |                        |                                    |                                                                        | Offset Prefix `-`         |
|              |                        | >>CPU Configuration    |FCLK Frequency for Early Power On | `400 Mhz`                            |                                                                     |                           |
| | >> Power & Performance | >> CPU - Power Management Control  | Boot performance mode| `Turbo Performance`                              |                          |
|              |                        |                                    | >> Config TDP Configurations                                           | `Up`                     |
|              |  |CPU |>>CPU Lock-CFG Lock|`OFF`                            |                                                                        |                          |
 
