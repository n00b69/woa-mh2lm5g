<img align="right" src="https://github.com/n00b69/woa-mh2lm5g/blob/main/mh2lm5g.png" width="350" alt="Windows 11 running on mh2lm5g">

# Running Windows on the LG V50S

## Reinstalling Windows

### Prerequisites
- [Windows on ARM image](https://worproject.com/esd)
  
- [Drivers](https://github.com/n00b69/woa-mh2lm5g/releases/tag/Drivers)

- [Mass storage image](https://github.com/n00b69/woa-mh2lm5g/releases/download/Files/massstorage.img)

### Reboot to fastboot mode
> If you don't have access to fastboot, use the instructions in the [partitioning guide](1-partition.md) to flash the engineering ABL.
- With the device turned off, hold the **volume down** button, then plug the cable in.
- If the phone in device manager is called **Android** and has a ⚠️ yellow warning triangle, you need to install fastboot drivers before you can continue.
- To install fastboot drivers, extract the contents of **QUD.zip** somewhere, right click on **Android**, click on **Update driver** and **Browse my computer for drivers**, then find and select the **QUD** folder.

#### Boot to the mass storage mode image
> Replace `path\to\massstorage.img` with the actual path of the image
>
> If popups show up telling you to format the disks, ignore or close them
```cmd
fastboot boot path\to\massstorage.img
```

> [!Note]
> After 1-2 minutes **WINMH2LM5G** should automatically appear in Windows Explorer. If it does, skip to the "Formatting Windows" step, else continue with the "Diskpart" steps.

### Diskpart
```cmd
diskpart
```

#### Finding your phone
> This will list all connected disks

> Look for your phone (which should be the last disk which will be 236GB). If you do not see it, wait a few seconds and run the command again. Repeat this until you see the disk.
```cmd
lis dis
```

#### Selecting your phone
> Replace $ with the actual number of your phone
```cmd
sel dis $
```

#### Listing your phone's partitions
> This will list your device's partitions
```cmd
lis par
```

#### Selecting the Windows partition
> Replace $ with the partition number of Windows (should be 32)
```cmd
sel par $
```

#### Add letter to Windows
```cmd
assign letter x
```

#### Exit diskpart
```cmd
exit
```

#### Formatting Windows
> Go to Windows Explorer > This PC and select **WINMH2LM5G**. Right click and format as NTFS.

### Installing Windows
> Replace `path\to\install.esd` with the actual path of install.esd (it may also be named install.wim or 22631.2861.XXXXXXX.esd)

```cmd
dism /apply-image /ImageFile:path\to\install.esd /index:6 /ApplyDir:X:\
```

> If you get `Error 87`, check the index of your image with `dism /get-imageinfo /ImageFile:path\to\install.esd`, then replace `index:6` with the actual index number of **Windows 11 Pro** in your image

### Copying your boot.img into Windows
> If you have a copy of your rooted boot image, copy it into Windows now. If not, make a copy using WOA Helper later.
- Drag and drop the **root.img** from the **platform-tools** folder into the **WINMH2LM5G** disk in Windows Explorer, then rename it to **boot.img**

### Installing Drivers
- Unpack the driver archive, then open the `OfflineUpdater.cmd` file (if an error shows up, run `OfflineUpdaterFix.cmd` instead)

> If it asks you to enter a letter, enter the drive letter of **WINMH2LM5G** (which should be **X**), then press enter

### Boot into Windows
Reboot your phone. If you end up in Android instead of Windows, flash the UEFI again using WOA Helper.

#### Setting up Windows
> Your device will now set up Windows. This will take some time. It will eventually reboot, and after that the initial setup (oobe) should launch.

> [!Tip]
> If you wish to skip the Microsoft Account login, press the **I don't have internet** button in the WiFi page, then when prompted, press the **Continue with limited setup** button.

## Finished!

































