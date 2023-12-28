# The ultimate guide for Xiaomi phones (MT6781)
So, you want to mod your Xiaomi phone but you don't know what tf are you doing? You are in the right place then. Note that this guide is focused on all Xiaomi phones using the Mediatek MT6781 SoC (known as Helio G96), tips here may work if your device has another MTK SoC, but have in mind that this is focused on this specific SoC.

### Phones fully supported on this guide
| Model | Codename |
| ----- | ----- |
| Redmi Note 11S 4G | `fleur` `miel` |
| POCO M4 Pro 4G | `fleur_p` `miel_p` |
| Redmi Note 11 Pro 4G | `viva` `vida` |
| Redmi Note 12S | `sea` `ocean` |

## Index
1. Definitions
   - [Bootloader](#bootloader)
     - [Unlock bootloader](#unlock-bootloader)
   - [Fastboot](#fastboot)
     - [FastbootD](#fastbootd)
   - [BROM](#brom)
   - [Recovery mode](#recovery-mode)
   - [ROM](#rom)
   - [OTA](#ota)
   - [IMEI](#imei)
   - [Root](#root)
2. [Backup IMEI](#backup-imei)
3. [Unlocking bootloader](#unlocking-bootloader)
4. Root
5. Flashing a custom ROM
6. Flashing the stock ROM
7. Using SP Flash Tool
8. Mi Flash

# Definitions
## Bootloader
The bootloader is the part of the system that is responsible of correctly booting the OS. It has some configuration and data that is very important, including the lock state.
### Unlock bootloader
The bootloader contains a protection to prevent you from modifying system partitions and other stuff. If you are wishing to install a custom ROM or root your phone, you have to unlock the bootloader.
## Fastboot
This is a boot mode that exposes the bootloader to a command-line named Fastboot.
### FastbootD
This is the same as Fastboot, but with dynamic-partitions support.
## BROM
This is a boot mode that exposes the whole device hardware to a command-line.
## Recovery mode
This is a boot mode containing a system recovery utility. Here, you can factory reset your device, flash things and more.
## ROM
This is referring to the system itself.
## OTA
Over-the-air update.
## IMEI
This is a number used by your mobile operator to identify your device. If your device is dual-SIM, it will have two IMEI numbers. Losing this number will result in various things like not being able to use VoLTE, or 4G/LTE at all.
## Root
This gives you system-wide admin rights and it's used to modify parts of the system that a non-root user wouldn't be able to modify.

# Backup IMEI
Your IMEI number it's very important, without it you won't be able to correctly connect to any mobile data provider/make calls/send SMS and all that. When modifying your device, you have the risk of losing your IMEI, and there's no fix for it... unless you backup it!.
Now, with a backup, I don't mean writing it on a piece of paper, you see, your IMEI is stored in a system partition called `NVRAM`. You have to make a copy of that partition, along with other important partitions like `NVDATA` and `NVCFG`.
## How do I backup my IMEI?
It's simple. You have to root your device for this, head to Root to find out how you do that. If your device is rooted, follow this steps:
1. Download Termux from F-Droid (not Play Store)
2. Install it, open it and execute this commands:

- `mkdir -p /sdcard/imeibackup`
- `for part in nvcfg nvdata persist protect1 protect2; do dd if=/dev/block/by-name/$part of=/sdcard/imeibackup/${part}.img; done`
- `for part in seccfg nvram ; do dd if=/dev/block/by-name/$part of=/sdcard/imeibackup/${part}.bin; done`

In the second command, Magisk will ask you for root permissions, you have to allow Termux this root permissions.
Now, yes, they are huge, but that's needed. You can obtain a pre-made script in the releases page, if you will use that, the command will be:

- `su`
- `sh /path/to/the/script`

Your backup will be placed in a folder called _imeibackup_ inside your internal storage. Move that folder to a safe place, like an SD Card, or your PC.

# Unlocking bootloader
You'll need to do this if you are going to modify your device. There are two situations you can be before unlocking your bootloader:
1. You have the latest MIUI/HyperOS version
2. You have an old MIUI version that is not the latest

If you are in the first situation, you're fucked. Some users have reported that they're unable to unlock their bootloader with newer MIUI/HyperOS versions. You have to downgrade to a version that's known to work, to do that, you have to use SP Flash Tool. Go to SP Flash Tool to find out more.

However, if your are in the second situation, you're good to go. You'll have to use `mtkclient` for this.
## Preparing your environment
Most users use Windows 10/11 so I'll focus on those. You require some software in your PC:
- Python
- mtkclient
- UsbDk
- MTK Serial Port driver (download this in releases)

With all that said, you have to:

1. Install Python
2. Grab the mtkclient repository source [here](https://github.com/bkerler/mtkclient)
3. Install UsbDk, download it from here
4. Install the MTK Serial Port driver, download it from the releases page.
5. Reboot your PC

Now that you've done that, you are good to go.