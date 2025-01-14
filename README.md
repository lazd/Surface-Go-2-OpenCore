![OpenCore logo](https://github.com/acidanthera/OpenCorePkg/raw/master/Docs/Logos/OpenCore_with_text_Small.png)

# Surface-Go-2-OpenCore
macOS on the Core m3-8100Y Microsoft Surface Go 2 thanks to [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg).

## Disclaimer
This repository is neither a howto nor an installation manual. Using these files requires at least basic knowledge of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg), ACPI, UEFI and the art of hackintoshing in general. I recommend reading the excellent [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide), as well as all its linked resources.

Although this repository is a fork of [kingo132's Surface Go 2 hackintosh repository](https://github.com/kingo132/surface-go2-hackintosh), my OpenCore EFI folder was built from scratch by following [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide) with the latest releases of [Acidanthera's OpenCore bootloader](https://github.com/acidanthera/OpenCorePkg) and every required kexts. Nevertheless, [kingo132's repository](https://github.com/kingo132/surface-go2-hackintosh) provided an extremely valuable and helpful resource for building my OpenCore EFI folder and hacking the tablet's UEFI BIOS.

## Recommendations
I recommend completely erasing the device's SSD by creating a new GPT partition table before attempting to install macOS, as it makes the installation process much easier. You may use any Linux live ISO with a partitioning tool such as `GParted` or `KPartition` to erase the SSD.

For macOS to be able to boot on the Surface Go 2, the `Secure Boot` option needs to be _**disabled**_ in the BIOS. The boot screen will then display a large red bar with a lock icon at the top of the display when Secure Boot is disabled. This is normal.

Please be aware that all `PlatformInfo` and `SMBIOS` information was removed from the OpenCore `config.plist` files. Users will therefore need to generate their own `PlatformInfo` with [CorpNewt's GenSMBIOS tool](https://github.com/corpnewt/GenSMBIOS) before attempting to boot a Surface Go 2 with this repository's EFI folder.

This repository features an EFI folder with two distinct `config.plist` files. One is meant to be used without the UEFI BIOS hacks below (`config_easy.plist`), the other one (`config_hard.plist`) will only boot after the UEFI hacks have been applied. Simply rename the config file you plan to use to `config.plist` and delete the other config file.

The `kexts` required to enable the trackpad and the touchscreen are special versions of `VoodooI2C.kext` and `VoodooI2CHID.kext` [patched and improved by lazd](https://github.com/jlempen/Surface-Go-2-OpenCore/issues/1#issuecomment-1705597716). Updating those `kexts` with the official ones from [the VoodooI2C repo](https://github.com/VoodooI2C) will most certainly break trackpad and touchscreen functionality! They were renamed to `VoodooI2C-SurfaceTouch.kext` and `VoodooI2CHID-SurfaceTouch.kext` so as not to be auto-updated by tools such as [OCAT](https://github.com/ic005k/OCAuxiliaryTools).
The `AirportItlwm.kext` from the [OpenIntelWireless repo](https://github.com/OpenIntelWireless/itlwm) is required to enable the Wifi chip and was renamed to `AirportItlwm-Sonoma.kext` for the same reason.

## Software Specifications
| Software         | Version                            |
| ---------------- | ---------------------------------- |
| Target OS        | Apple macOS 14.2.1 Sonoma |
| OpenCore         | [MOD-OC v0.9.7](https://github.com/wjz304/OpenCore_NO_ACPI_Build/releases/download/0.9.7_9802d1c/OpenCore-Mod-0.9.7-RELEASE.zip) |
| SMBIOS           | MacBookAir8,1 |
| SSD format       | APFS file system, GPT partition table |

## Computer Specifications
| Device           | Hardware                           |
| ---------------- | ---------------------------------- |
| CPU              | Intel Core m3-8100Y (1.1 - 3.4 GHz, Amber Lake-Y) |
| iGPU             | Intel UHD Graphics 615 |
| Audio            | Realtek ALC298 |
| RAM              | 2x4 GB LPDDR3 1867 MHz |
| Wifi + Bluetooth | Wifi6 AX200, Bluetooth 5.0 |
| Storage          | Kioxia/Toshiba 128GB SSD |
| USB Type-C 3.1 Gen 1 | Supports Power Delivery and DisplayPort |
| MicroSD Card reader | Realtek PCI-E Card Reader |
| Front & Rear camera | Intel(R) AVStream Camera 2500, ISP Interface, 8 MPix/5 MPix |
| IR camera | Intel(R) AVStream Camera 2500, ISP Interface |
| Keyboard / Trackpad | Microsoft Type Cover |
| Display | 10.50 inch 3:2, 1920 x 1280 pixel 220 PPI, 10-Point Capacitive |
| Touchscreen | `ELAN9038`, `\_SB.PCI0.I2C1.TPL1` |
| Screen | BOE CQ NV105WAM-N31, BOE088B |
| NFC | NXP NFC Client Device, NXP3001 |
| Battery | 26.81Wh 7.66v 3500mAh |
| LTE (if available) | Surface Mobile Broadband, USB device, Qualcomm Snapdragon X16 LTE |
| GPS (if available) | |
| Accelerometers, gyroscopes, ambient light sensors | |

## What works
- [x] CPU power management (`SSDT-PLUG.aml`)
- [x] CPU SpeedStep (`CPUFriend.kext`, `CPUFriendDataProvider.kext`)
- [x] iGPU with full acceleration (`SSDT-PNLF.aml`, `WhateverGreen.kext`, `AAPL,ig-platform-id 00001B59`)
- [x] SSD drive (`NVMeFix.kext`)
- [x] USB-C port (`SSDT-EC-USBX-LAPTOP.aml`, `USBMap.kext`)
- [x] WLAN (`AirportItlwm.kext`)
- [x] Bluetooth (`IntelBluetoothFirmware.kext`, `IntelBTPatcher.kext`, `BlueToolFixup.kext`)
- [x] Internal speakers, microphone and Combojack (`AppleALC.kext`, `alcid=3`)
- [x] Power, volume up and volume down buttons (`VoodooPS2.kext`)
- [x] Surface Type Cover keyboard with working brightness, volume and mute keys (`VoodooPS2.kext`)
- [x] Surface Type Cover trackpad with native multi-touch gestures (`SSDT-XOSI.aml`, `VoodooI2C`, `VoodooI2CHID`)
- [x] Touchscreen (`SSDT-XOSI.aml`, `VoodooI2C.kext`, `VoodooI2CHID.kext`)
- [x] Surface Pen (`SSDT-XOSI.aml`, `VoodooI2C.kext`, `VoodooI2CHID.kext`)
- [x] MicroSD Card reader (`RealtekCardReader.kext`, `RealtekCardReaderFriend.kext`)
- [x] Battery percentage and cycle count (`VirtualSMC.kext`, `SMCBatteryManager.kext`)
- [x] USB Type-C to HDMI
- [x] USB Type-C to USB3 & USB2
- [x] USB Type-C Power Delivery

## What needs some more work
- [ ] Sleep/wake - the device wakes up from sleep displaying the Surface logo on the internal display
- [ ] Type Cover - the caps lock led does not light up when enabled: [issue fired](https://github.com/VoodooI2C/VoodooI2CHID/pull/52#issuecomment-841795827)
- [ ] Accelerometers, gyroscope
- [ ] Ambient light sensor

## What will probably never work
- [ ] Front & rear cameras
- [ ] IR camera (Windows Hello)
- [ ] NFC
- [ ] LTE

## Turning off BD PROCHOT
On this device, BD PROCHOT will activate at temperatures as low as 60°C ~ 70°C. When it kicks in, the CPU will throttle down to 0.4Ghz, making the device more or less unusable. That's why BD PROCHOT needs to be disabled in order to increase the performance of the machine.

You may use [arter97's DisablePROCHOT UEFI extension](https://github.com/arter97/DisablePROCHOT) which is already included in the EFI/OC/Drivers folder to disable BD PROCHOT. However, if the CPU continues to be fully loaded with BD PROCHOT disabled, the temperature may increase up to 90°C. Beyond 90°C, the device will become unstable and either crash or power off. To avoid this, the temperature needs to be kept under control.

The DisablePROCHOT.efi driver is included but disabled in both config.plist files, as enabling it leads to the CPU overheating and the computer shutting down during install. You may enable the driver once macOS is installed on the device.

## Undervolting to reduce heat and improve performance
There are two undervolting tools available for macOS:
* [Volta](https://volta.garymathews.com)
* [VoltageShift](https://github.com/sicreative/VoltageShift)

The `VoltageShift.kext` is already included and enabled in this repository's `Kexts` folder. To be able to launch the `voltageshift` command line tool from anywhere, copy [VoltageShift from the Tools folder](https://github.com/jlempen/Surface-Go-2-OpenCore/blob/main/Tools/VoltageShift-EFI.zip) to your `/usr/local/bin` folder by entering `sudo cp voltageshift /usr/local/bin/` in the macOS terminal after navigating to the folder where you downloaded and unzipped the `voltageshift` executable file.

Please refer to the instructions found in the [VoltageShift repository](https://github.com/sicreative/VoltageShift), as well as to the excellent howto found in [zearp's repository](https://github.com/zearp/Nucintosh#undervolting).

## UEFI BIOS hacks
The UEFI BIOS UI of the Surface Go 2 shows only a few trivial settings. In order to take advantage of better CPU power management and graphics acceleration, there are a few other settings that need to be changed in the UEFI BIOS. The best way to achieve this is to use the [RU.efi tool](http://ruexe.blogspot.com) or the new [setup_var.efi tool](https://github.com/datasone/setup_var.efi). The [Dortania OpenCore Post-Install guide](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html#turning-off-cfg-lock-manually) has detailed instructions on how to use the `RU.efi` tool.

Hint: the `RU.efi` tool is included in this EFI folder and may be launched from the OpenCore picker in the Auxiliary tools section.

_**Please be aware that these hacks may potentially brick your computer! Proceed carefully and only if you know what you are doing!**_

### UEFI BIOS variables which need to be modified
| VarName | VarOffset | VarStore | From | To |
| ---------------- | -- | -- | --------- | --------- |
| CFG Lock | 0x3C | 0x3 (CpuSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| VT-d | 0xE3 | 0x2 (SaSetup) | 0x1 (Enabled) | 0x0 (Disabled) |
| DVMT Pre-Allocated | 0xDF | 0x2 (SaSetup) | 0x1 (32M) | 0x2 (64M) |
| DVMT Total Gfx Mem | 0xE0 | 0x2 (SaSetup) | 0x2 (256M) | 0x3 (MAX) |

## Enabling native HiDPI display settings in macOS
On the installed macOS system, the default display resolution is too small for a small device such as the Surface Go 2. To enable the native HiDPI settings in the Display Preferences of macOS, download and run the [one-key-hidpi](https://github.com/jlempen/one-key-hidpi) script and select the option `(4) 1920x1280 Display`. This fork of [xzhih's one-key-hidpi tool](https://github.com/xzhih/one-key-hidpi) was modified to add the 1920x1280 resolution needed for the Surface Go 2.

## Disabling sleep
As there is still no solution to the sleep issue on the Surface Go 2, it is recommended to disable Sleep altogether for now:
```
sudo pmset -a sleep 0
sudo pmset -a hibernatemode 0
sudo pmset -a disablesleep 1
```

There have been reports that the Surface Go 2 is able to successfully enter and wake from Hibernation instead of Sleep [in this discussion](https://github.com/jlempen/Surface-Go-2-OpenCore/issues/1#issuecomment-1500697563), but I haven't been able to make it work yet. The [SSDT-GPRW.aml and the GPRW to XPRW patch](https://dortania.github.io/OpenCore-Post-Install/usb/misc/instant-wake.html) are enabled in this EFI folder for those who want to troubleshoot the sleep and/or hibernate issues.

## Related issues
* https://github.com/VoodooI2C/VoodooI2CHID/pull/48
* https://github.com/VoodooI2C/VoodooI2C/issues/455
* https://www.reddit.com/r/hackintosh/comments/mwrzxg/intel_igpu_hdmiinternal_screen_problem/
* https://www.reddit.com/r/hackintosh/comments/mrfmmk/surface_go2_hackintosh/
* https://www.reddit.com/r/hackintosh/comments/nm6nh7/surface_go_2_has_anyone_ever_attempted_this/
* https://github.com/linux-surface/linux-surface/wiki/Surface-Go-2

## Useful links
* https://www.anandtech.com/show/6355/intels-haswell-architecture/3

## Related repositories
* https://github.com/kingo132/surface-go2-hackintosh
* https://github.com/stryses/Mi-Laptop-Air12.5-8100Y-Hackintosh
* https://github.com/anonymous5l/macpanda
* https://github.com/jibsaramnim/gpd-pocket2-hackintosh
