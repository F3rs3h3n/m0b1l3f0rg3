# Setup NetHunter on Nexus5

How to setup NetHunter for Nexus5.

Nexus5 can use Nexmon for the some wifi tools.  
I would try to use it next Wifi penetration test engagement.

> Note: Note: Some apps may not work properly. However, Aircrack-ng seems to work fine. I have only tested listing Wi-Fi access points and did not attempt any attacks.

<br>

# The following content is currently under construction.


## Setup Flow

### 1. Enable Develper Mode

On Nexus5
1. Settings -> About 
2. Tapping on Build number field 7 times

### 2. Unlocking

#### a. Download ADB and fastboot (depends on OS/distro)

Just unzip on Linux

- ADB: https://developer.android.com/tools/releases/platform-tools
- Reference: https://www.xda-developers.com/install-adb-windows-macos-linux/#how-to-set-up-adb-on-your-computer


#### b. Enable USB debugging in the developer options and connect computer
Reference: https://source.android.com/docs/setup/test/running

After enable USB debugging mode, check connection.

```sh
lsusb
```

![alt text](01_images/lsusb.png)


And check connection between adb and android

```sh
adb devices
```
![alt text](01_images/adb_devices.png)


If you need full backup without system data as an apk 
```sh
adb backup -apk -noshared -all -nosystem -f full_backup.ab 
```

#### c. Put phone into fastboot by holding down vol down + power
Reference: https://source.android.com/docs/setup/build/running?hl=ja

<img src="01_images/fastboot_mode.JPEG" width="300">


#### d. Unlocking OEM (Bootloader)

```sh
fastboot oem unlock
```

![alt text](01_images/oem_unlock.png)
    

### 4. Installing TWRP for recovery

#### About: TWRP

Reference: https://twrp.me/lg/lgnexus5.html

TWRP (Team Win Recovery Project) is a custom recovery that allows you to install custom ROMs and modifications on your Android device.

#### Install TWRP

When Android device is fastboot mode and OEM was unlocked. An img can be flashed to device by following Command.

```
fastboot flash recovery twrp-3.7.0_9-0-hammerheadcaf.img
```

![alt text](01_images/flash_TWRP.png)


### 5. Lunch TWRP entering recovery mode

After flashing TWRP, Enter Recovery Mode

fastboot mode -> Recovery 

### 6. Wipe and format strage for the mount

I was not able to be mounted /system directory. Following steps are good for it fix. 

Reference: https://droidwin.com/failed-to-mount-data-invalid-argument-in-twrp-fix/

#### STEP 1: Repair Data Partition via TWRP

a. Wipe > Advanced Wipe > Repair or Change File System

b. Repair File System > Swipte to Repair.

#### STEP 2: Change the File System

a. Wipe > Advanced Wipe  
b. Sllect "Data" and tap Repair File system  
c. Chage file System and choose exFAT  
d. After finished it Change file system to ext4  
e. Wipe > Format Data > enter YES  

After wiping and re-format ext4fs. Internal Strage of Nexus5 will apear.

![alt text](01_images/Internal_Strage.png)

### 7. Install Android 6 from Factory Image

We wiped OS image, so need re-install OS from factory image.

#### a. reboot to bootloader

Flash script has some fastboot commands. so need to enter bootloader mode.

#### b. Run the script to install Android 6 OS

Factory Image: https://developers.google.com/android/images#hammerhead

Unzip downloaded zip > run flash-all.sh


### 7. Install Magisk

Install Magisk to install into Android 6 OS.

#### a. Downlaod and install Magisk apk by browser of Android

Need to allow to install downloaded apk.

Reference: https://github.com/topjohnwu/Magisk/

> Note: Magisk.apk can be Magisk Manager and Magisk

#### b. Download Magisk apk

1. Change filename Magisk-v27.0.apk -> Magisk-v27.0.zip
2. `adb push Magisk-v27.0.zip /sdcard` and Install Magisk from TWRP


### 8. Build NetHunter Image

Offsec is releasing NetHunter OS for nought (Android 7).  
Nexus5 accepts only Android 6 (mashmallow).

I would not compile lingearOS for it, so I tried to build NetHunter image for mashmallow using official compile kit.

Reference: https://www.kali.org/docs/nethunter/building-nethunter/

```sh
./bootstrap.sh

python3 build.py -d hammerhead --marshmallow -fs nano
```

> Note: NetHunter.apk's hash was not match. so I had over written it. It's risky, I have to notice it maintainer. 


### 9. Install NetHunter

from TWRP.

Install > select Nethunter image


<br>

## Recovery

If you crash OS, You can restart from the factory image.

Factory Image: https://developers.google.com/android/images#hammerhead

Unzip > run flash-all.sh



## Refered contents

- https://gist.github.com/jmingov/4fd6d02ae75cdedd62144623725ef6e6
- https://pwnieexpres.com/blogs/news/a-simple-guide-to-installing-nethunter-on-a-nexus-5x
- https://www.getdroidtips.com/install-official-twrp-recovery-for-nexus-5/

