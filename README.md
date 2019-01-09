# My Hackintosh experience

...while installing macOS Mojave (10.14) on Dell Inspiron 5567.

---

### Contents

* **Part 1** : ```Denial, Anger, Bargain.```
  * [Build Specifications](#build-specifications)
  * [Introduction](#introduction)
  * [The Install Media](#the-install-media)
  * [The Install Process](#the-install-process)
* **Part 2** : ```Depression, Acceptance.```
  * [The Good](#the-good)
  * [The Bad](#the-bad)
  * [The Ugly](#the-ugly)
  * [Lessons Learned](#lessons-learned)

---

### Part 1 : ```Denial, Anger, Bargain.```

### Build Specifications

Device Make & Model   | Dell Inspiron 5567
---------------------:|:--------------------------------
Motherboard           | Dell ```0C6XG5```
CPU                   | Intel ```i5-7200U (Kaby Lake)```
Graphics (Integrated) | Intel ```HD Graphics 620```
Graphics (Dedicated)  | ATI ```Radeon 445M```
Wireless Card         | Intel ```Dual Band Wireless-AC 3165```
Audio Card            | Realtek ```ALC3246 (ALC256)```

---

### Introduction

Like the name suggests, Hackintosh is indeed full of hacks and requires **a lot** of patience.

##### But why macOS/Hackintosh?

Wanted to try it out for the longest time. I have worked with every version on Windows since XP and have played around myriad of Linux distros for almost a decade. The only thing I felt was having an OSX install on my laptop. And after all these years, I finally felt like doing it.

##### Why macOS Mojave? Wasn't High Sierra more stable than Mojave?

`"New is always better." - Barney Stinson` I wasn't going to settle for anything that is not the latest in the market.

##### Where did I start?

Google > "hackintosh" > First result was from [hackintosh.com](https://hackintosh.com). Browsed the site for few minutes and it later redirected me to [tonymacx86's](https://tonymacx86.com) forum. And the rest is history.

Many sites, blogs, YouTube videos helped along the way, but reading [RehabMan](https://github.com/RehabMan)'s guides (word-to-word) on tonymacx86's forums had every answer.

##### How did I go about it?

There are few main steps for installing Hackintosh on a laptop/desktop -

1. Create the macOS install media.
2. Install the Clover bootloader (`Clover:macOS :: Grub2:Linux`) on EFI partition of the USB.
3. Configure the BIOS per the requirements of macOS.
4. Boot into Clover bootloader.
5. Verify options, kexts (**K**ernel **Ext**ensions), drivers and boot into macOS.
6. Clean the entire HDD and install macOS.
7. Boot into the new install.
8. Identify broken and missing pieces of the OS and try fixing them.

---

### The Install Media

The installation media a.k.a bootable USB of macOS can be created only using macOS. It's very frustrating but Apple cannot be blamed. Apple anyway does not mean to distribute macOS without a real Macintosh. But even then there is a way to create macOS install USB using PC. Virtual Machine. The hackintosh community strongly suggests against using VMs for creating the install media because it often fails. I had no other option except using VM. I went ahead with it. I was skeptical about it too, but it did work.

First, I downloaded the VM image of macOS Mojave. Booted it using VMware. There is a whole different process just for booting macOS in VMware/VirtualBox.

After booting the VM, I created the installation USB. Then mounted the EFI partition on the USB and pasted a generic, latest Clover bootloader on it. Didn't mind looking into the `config.plist` file or any other details.

Obviously, I was wrong. The `config.plist` file is the **most** crucial part of the entire process. It has hundreds of attributes and parameters that can be configured or modified in a thousand ways. And there are only handful configurations that are right for your device. One change and the installer goes into a Kernel Panic resulting in a crash. I spent about 3 days playing around with the ```.plist``` file, obviously because I was an idiot. If I had carefully read [RehabMan](https://github.com/RehabMan)'s posts on forums, I would have booted the installer within mere minutes. Turn's out RehabMan has created a set of [```config.plist``` files](https://github.com/RehabMan/OS-X-Clover-Laptop-Config) according to specific Graphics cards. I just had to look for [mine](https://github.com/RehabMan/OS-X-Clover-Laptop-Config/blob/master/config_HD615_620_630_640_650.plist), rename it, and put it in ```/EFI/CLOVER/``` path of the bootable USB. I didn't feel any shame in using it directly because it is indeed a difficult task to build a config file from scratch.

P.S. The whole process sucked 24GB of my bandwidth. The VM was 6GB and the Mojave image was another 6GB. Only thing is that I did it twice.

---

### The Install Process

After booting into the setup, I wiped the entire HDD that had Windows and Debian installed on it. It is a tiresome process to dual boot Windows + macOS because:

1. The macOS/OSX needs the EFI partition to be 200MB, as compared to the default size which is 100MB. This might be a precautionary thing by Apple.
2. The macOS install partition/system partition `Mac HD` should be the next to EFI partition on HDD.

Instead of going through the trouble of moving and freeing partitions, I found it best to wipe the entire HDD clean for the macOS install.

A partition `Mac HD` needs to be created with `Mac OS Extended` (not `APFS`) journaling (like `ext4` on Linux or `NTFS/exFAT` on Windows). The rest process is simple Agreeing to T&C and setting up user account installing Mojave.
Takes several reboots and few retries (yes, the entire process) to finish the installation and it boots to the Welcome page where it asks for user password. That was it.

***It was a great feeling.***

---

### Part 2 : ```Depression, Acceptance.```

After selecting the newly introduced Dark theme option in macOS, the dark Desert Mojave wallpaper soothed my eyes. But the battle was far from over. I knew there were going to be some problems. After running the standard checks on the hardware - `Audio`, `Graphics`, `WLAN`, `Power`, `Trackpad` and `Keyboard` - it was clear that none of them worked perfectly. Fortunately, some didn't need workarounds and hacks at all (`The Good`), few needed some hacks to make them work (`The Bad`) and the others never worked (`The Ugly`).

---

### The Good

* **Graphics** : This was a strange “issue”. Those who complained about not having graphics acceleration said that they only had some `4MB` or `7MB` of graphics memory. Also they said that their system didn’t detect any graphics card (on their `System Information`). Now why was this strange to me was because I had neither of the problems. My `System Information` showed me `Intel HD Graphics 620` as  graphics with a `1536MB` of video memory. Which is perfectly fine. And I thought that I wasn’t getting the required acceleration because - get this - there was not magnification effect on the dock that macOS typically has. Turned out Mojave had it disabled by default for unknown reasons and I just had to enable it from System Preferences. *Stupid me*.
* **Keyboard** : The layout (obviously) does not match the MacBook’s keyboard. The `Cmd` key did’t work and the `Option` key (`Alt`) behaved very strangely. After some forum digging and YouTube, turned out there was an easy fix for that: The `Cmd` and `Option` modifier keys needed to be altered/remapped to fix their behaviour. And it is easy. `System Preferences > Keyboard > General tab > Modifier Keys`. Do the required changes and close. Kinda cool that Apple, famous for their user controlling behaviour, natively supports a little of keyboard remapping in macOS \*gets amused in Windows\*.

---

### The Bad

* **Trackpad** : The trackpad refused to detect whatsoever. But I was never worried about it and was almost sure it would work after a fix. *Spoiler Alert: It was because of the power issue.*
* **Battery Indicator** : The problem with battery is because of this: macOS failed to recognise the computer as a laptop although the `SMBIOS` in Clover’s `.plist` makes it look like a `MacBook Pro 14,2`. From what I have read, battery indicator and other battery related fixes needs the `DSDT` to be patched manually since recent releases of macOS and the old kexts didn’t work. `DSDT patching` includes writing and editing machine code. Changing register values and bringing them down to `8-bits` each. When I disassembled and opened the `DSDT` file, I saw that the registers were already `8-bit`. **NO MANUAL PATCHING** was required. So I quickly used the old kext patch and the battery indicator appeared. Now the OS stared recognising the hardware as a laptop. This fixed another issue I had. The trackpad. Now that OS understood that the computer was a laptop. It quickly showed me the trackpad settings and gesture settings (which by the way, still don’t work).
* **Audio** : This probably was the fix that required real working and testing because all other fixes were simple even though I thought they were difficult to fix and might need a lot of work to fix them. In audio’s case, it required many things to be taken care of and needed lots of testing. `AppleALC kext` was the main player during the audio hunt. The install procedure was simple. You just had to look up if your `codec` (a.k.a the Audio card) is supported by `AppleALC` and install the kext under `/Library/Extensions` path if the answer was yes. And then lookup the `layout-id` of the codec and configure the Clover `config.plist` accordingly. My codec was supported (`ALC3246/ALC256`). I installed the kext and changed the `layout-id` (mine was `13`) as instructed. Audio didn’t work. I tested with various `layout-ids`. Got the same result every time. Then according to a post, I used `Intel FrameBuffer patcher` to generate a patch for the audio. I opened it and generated a patch. Then manually edited the Clover’s `config.plist` and added the following to it:
  1. `ACPI` > `Enabled: FixHPET, FixIPIC, FixRTC, FixTMR.`
  2. `Devices` > `Audio` > `Inject` > `13`
  3. `Devices` > `Properties` tab > + under `devices` > Add `PciRoot(0x0)/Pci(0x1f,0x3)` > In Right pane > Add `layout-id` Properties Key > `0D000000` Properties Value.

* **Audio (contd.)** : Also yet another kext, `CodecCommander` needed a custom edit to support `ALC256` codec. I don’t remember what I edited there but I have a backup, just in case. Even after doing so there was no audio. After some log hunting into the system, I found out that `AppleALC` had failed to load. The main culprit. In order to fix that I read a lot about `AppleALC kext`. Found that `AppleALC` inherits some functions from `Lilu kext`, which was already available in Clover’s kexts (`/CLOVER/kexts/other`). I tried to put both `AppleALC` and `Lilu` kexts on `EFI` partition in `Clover directory` but it sill didn’t load. So I moved both extensions to `/Library/Extensions` directory and rebooted. The audio worked on the next boot.

---

### The Ugly

* **WLAN** : Oh the horror. Now this was a real issue. No kext patch or DSDT patch for the WiFi card I have in the laptop (`Intel AC3165 Dual Band`). There was no way to fix it because a group of kext enthusiasts had tried fixing it in past. `"3 years ago"`, said the GitHub repo’s last commit. There was no option, everyone on various forums roared. It was because Apple doesn’t include **any** Intel related network cards and so there was no official Apple’s patch to refer to and make a supporting driver out of it. The only solution was to replace the WLAN card with a macOS compatible card. The easier solution was to get a USB WiFi adapter. But many people on the forum were skeptic about it because of the drivers. They don't always work. But I gave it a try anyway. Bought a `TP-Link WN823N` adapter and installed it. To my relief, it worked flawlessly.

---

### Lessons Learned

* I should definatly start reading the documentation more carefully and thoroughly. Doing so would have saved around three days duing the `Hackintosh Project`.
* Booting and fixing Hackintosh was even more thrilling experience than installing and setting up Arch Linux.
* Hackintosh is very similar to Android ROMs. Each and every device has it's specific `Kernels`, `Frameworks`, `Device Trees`, `Vendors`. In Hackintosh, every build is very specific. This is in contrast to Windows and Linux where drivers can easily be found and installed (Windows) and where the kernel has all the necessary and popular drivers (Linux). macOS kernels are modular. They exclusively use extensions called `kexts` to kind of "patch" the kernel on the fly. Interesting.