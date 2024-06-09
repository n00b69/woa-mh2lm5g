<img align="right" src="https://github.com/n00b69/woa-mh2lm5g/blob/main/mh2lm5g.png" width="350" alt="Windows 11 running on mh2lm5g">

# Running Windows on the LG V50S

## Partitioning your device

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Qfil](https://github.com/n00b69/woa-mh2lm5g/releases/tag/Qfil)

- [QFILHelper](https://github.com/Beliathal/QFILHelper) Optional, for easier partition backups

- [Parted script](https://github.com/n00b69/woa-mh2lm5g/releases/download/Files/parted)

- [Engineering ABL](https://github.com/n00b69/woa-mh2lm5g/releases/download/Files/engabl_ab.bin) Only needed if you don't have fastboot
  
- Any custom recovery

### Notes
> [!WARNING]  
>  
> Do not run all commands at once, execute them in order!
>
> YOU CAN BREAK YOUR DEVICE WITH THE COMMANDS BELOW IF YOU DO THEM WRONG!!!
>
> DO NOT REBOOT YOUR PHONE! If you think you made a mistake, ask for help in the [Telegram chat](https://t.me/lgedevices).

### Opening CMD as an admin
> Download **platform-tools** and extract the folder somewhere, then open CMD as an **administrator**.
>
> It is recommended to keep this window open and use it throughout the entire guide.
> 
> Replace `path\to\platform-tools` with the actual path to the platform-tools folder, for example **C:\platform-tools**.
```cmd
cd path\to\platform-tools
```

### Backing up partitions
> If you don't do this and mess something up, you're on your own

#### Setting up Qfil
- Open **Qfil**.
- In "Select Build Type", select **flat build**.
- In "Select programmer", select the downloaded firehose.
- In "Configuration", make sure the "Device Type" is set to **UFS**.

#### Boot to EDL
- Open **Device Manager** on your PC
- With the phone turned off, hold **volume down** + **power**.
- After the LG logo appears, while still holding **volume down** + **power**, start rapidly pressing the **volume up** button.
- Keep doing this until you hear a USB connection sound on your PC, or when **Qualcomm HS-USB QDLoader 9008** appears in the **Ports (COM & LPT)** category of Device Manager.
- If the device is called **QUSB_BULK_CID** or has a ⚠️ yellow warning triangle / question mark, and is located in any other category (for example **Other devices**), you need to install EDL drivers first.
- To install EDL drivers, extract the contents of **QUD.zip** somewhere, right click on **QUSB_BULK_CID**, click on **Update driver** and **Browse my computer for drivers**, then find and select the **QUD** folder.

#### Making sure Qfil works
- In **Qfil**, make sure the correct port is selected. If it says `No Port Available`, select the **Qualcomm HS-USB QDLoader 9008** port.
- At the top, select "Tools" > "Partition manager", and click **Ok**.
- If you see a **Download Fail:Sahara Fail** error, make sure your cable stays connected and reboot to EDL again by holding **volume down** + **power**, then start rapidly pressing the **volume up** button once the LG logo appears.
- Once you're back in EDL, try opening the Partition manager again.
- If it still fails, try to repeat the last step a few times. You can also try rebooting your phone and PC.

#### Backing up your partitions
- In the Partition manager, right click on **laf_a** > **Manage Partition Data** and press **Read Data**.
- Do the same thing for **boot_a**, **abl_a**, **aop_a**, **xbl_a**, **fsg**, **fsc**, **modemst1**, **modemst2** and **modem_a**
- If you want to be on the safe side, you can also use [QFILHelper](https://github.com/Beliathal/QFILHelper) to additionally back up every partition. In this guide we only back up the most critical partitions.

> [!Important]
> Navigate to `C:\Users\YOURNAME\AppData\Roaming\Qualcomm\QFIL\COMPORT_#\` and rename the backed up partitions one by one as you back them up. Qfil does not name the backups, and if you don't rename them, it'll be impossible to figure out which files are which. You can restore them later with the **Load Image** function.

### Flashing engineering ABL
> If fastboot works on your phone, you can skip this step
- In **Qfil**, select Tools > Partition manager, and click **Ok**.
- Right click on **abl_a** > **Manage Partition Data** and press **Load Image**.
- Select and flash the **engabl_ab.bin** file.
- Do the same thing for **abl_b**.

#### Reboot your phone
> Hold **volume down** + **power** until it shows the LG logo, then release the buttons.

### Boot into any custom recovery
> Such as Lineage recovery, OFOX, or TWRP, which should be accessible by holding the **volume up** + **power** buttons, or with the Reboot to recovery button in Magisk

#### Unmount all partitions
Go to mount in your recovery and unmount all partitions

### Preparing for partitioning
> Download the parted file and move it in the platform-tools folder, then run
```cmd
adb push parted /cache/ && adb shell "chmod 755 /cache/parted" && adb shell /cache/parted /dev/block/sda
```

#### Printing the current partition table
> Parted will print the list of partitions, **userdata** should be the last partition in the list.
```cmd
print
```

#### Removing userdata
> Replace **$** with the number of the **userdata** partition, which should be **30**
> 
> If you have a **grow** partition, remove it as well
```cmd
rm $
```

#### Recreating userdata
> Replace **17.7GB** with the former start value of **userdata** which we just deleted
>
> Replace **60GB** with the end value you want **userdata** to have. In this example Android will have 60GB-17.7GB= **42.3GB** of usable space.
```cmd
mkpart userdata ext4 17.7GB 60GB
```

#### Creating ESP partition
> Replace **60GB** with the end value of **userdata**
>
> Replace **60.3GB** with the value you used before, adding **0.3GB** to it
```cmd
mkpart esp fat32 60GB 60.3GB
```

#### Creating Windows partition
> Replace **60.3GB** with the end value of **esp**
>
> Replace **254GB** with the end value of your disk, use `p free` to find it
```cmd
mkpart win ntfs 60.3GB 254GB
```

#### Making ESP usable
> Use `print` to see all partitions. Replace "$" with your ESP partition number, which should be 31
```cmd
set $ msftdata on
```

#### Exit parted
```cmd
quit
```

### Format all data
Go to the Wipe menu in your recovery and wipe all data. If this doesn't work, simply reboot your phone.

#### Check if Android still starts
> Once it is booted, it should tell you decryption was unsuccesful and it will ask you to erase all data.
- Press this button to erase all data, let the phone boot back up, then reboot back to fastboot mode.
Just reboot the phone and see if Android still boots.

## [Next step: Installing Windows](2-install.md)












