# The ultimate guide for Xiaomi phones (MT6781)
So, you want to mod your Xiaomi phone but you don't know what tf are you doing? You are in the right place then. Note that this guide is focused on all Xiaomi phones using the Mediatek MT6781 SoC (known as Helio G96), tips here may work if your device has another MTK SoC, but have in mind that this is focused on this specific SoC.

### Phones fully supported on this guide
| Model | Codename |
| ----- | ----- |
| Redmi Note 11S 4G | `fleur` `miel` |
| POCO M4 Pro 4G | `fleur_p` `miel_p` |
| Redmi Note 11 Pro 4G | `viva` `vida` |
| Redmi Note 12S | `sea` `ocean` |

> [!WARNING]
> Read everything of the section you are interested of, and when I say everything, I mean EVERYTHING. That will prevent you from having dumb issues.

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
### How do I get Fastboot?
Good question, here's how.

1. Download the `platform-tools` package from here.
2. Unzip it in its own folder.
3. Download the Google USB drivers here.
4. Power off your phone, wait 5 seconds and then press the `Power` key and `Volume -` key together until you see some orange letters saying FASTBOOT.
5. Connect your phone to your computer, open the device manager.

> [!NOTE]
> To open the device manager in Windows 10/11, press `Windows` + `X` on your keyboard. A menu should open above your Windows logo on the taskbar, select device manager.

6. You should see a device called `Android` that has a ⚠️ on the side. Select it and right-click it, then select `Update drivers`.
7. Choose
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

> [!WARNING]
> As I said earlier, read everything. It's specially important to read everything from here because this process can kill your device if you make a bad mistake. Please, READ EVERYTHING.

Most users use Windows 10/11 so I'll focus on those. You require some software in your PC:
- Python
- mtkclient
- UsbDk
- MTK Serial Port driver (download this in releases)

With all that said, you have to:

1. Install Python
2. Grab the mtkclient repository source [here](https://github.com/bkerler/mtkclient)
3. Unzip the `mtkclient` source you just downloaded in its own folder.
4. Install UsbDk, download it from here
5. Install the MTK Serial Port driver, download it from the releases page.
6. Reboot your PC

Now that you've done that, you are good to go.

## Unlocking

Go to the `mtkclient` folder in where you unzipped the source. Open a CMD window inside that folder.

> [!NOTE]
> In Windows, to open a CMD window inside the folder, type `cmd` on the address bar of File Explorer and hit enter. That will open a CMD window inside the folder you are currently in.

In the CMD, type this command:

- `python mtk da seccfg unlock`

You should see some text telling you to connect your device in BROM mode. It's completely normal that the message repeats itself. While that happens, grab your phone and turn it off, wait 5 to 10 seconds and connect the USB cable to your computer, not to your phone.

Now, get ready to do a fast movement. You have to hold all the buttons of your phone, and immediately connect it to the PC while still holding all the buttons. This has to be done very fast, otherwise your device will reboot and you'll have to start over.

If you did it correctly, you should see how a bunch of stuff gets printed in the CMD window ok your computer, and eventually, a message will appear saying that the bootloader unlocked successfully. At this moment, you can safely disconnect your phone and then turn it on.

> [!CAUTION]
> DO NOT disconnect your phone if the command is still running. Please wait for the command to complete, then disconnect your phone. If you disconnect your phone and the command is still running, you will pretty much fuck your phone.

If it says something like `Bootloader already unlocked`, look for help in the MT6781 telegram group here.

> [!NOTE]
> I am 100% sure that your device shows a message in small letter when booting, then it turns off. This is COMPLETELY NORMAL and harmless. To get rid of it, boot to Fastboot by pressing the `Power` key and the `Volume -` key when the device is off. You should see some orange letters that say FASTBOOT. Plug your phone to your PC, open a terminal and use this command: `fastboot oem cdms`. If the command throws an error, refer to the [Fastboot](#fastboot) section on this guide.

# Root
Now that you've unlocked your bootloader, you can root your phone. There are several ways to root your phone, but the most popular is using Magisk. 