<img align="right" src="https://github.com/n00b69/woa-mh2lm5g/blob/main/mh2lm5g.png" width="350" alt="Windows 11 running on mh2lm5g">

# Running Windows on the LG V50S

## Reinstalling Windows

### Prerequisites
- [ADB & Fastboot](https://developer.android.com/studio/releases/platform-tools)

- [Modded TWRP](https://github.com/n00b69/woa-mh2lm5g/releases/download/Files/Modded-twrp-g8x.img)

### Reboot into fastboot mode
> If you don't have access to fastboot, use the instructions in the [partitioning guide](1-partition.md) to flash the engineering ABL.
- With the device turned off, hold the **volume down** button, then plug the cable in.

#### Boot into the modded TWRP
> Replace `path\to\modded-twrp-g8x.img` with the actual path of the provided TWRP image
>
> After booting into TWRP, leave the device on the main screen. You can press the power button to turn the display off, if you want
```cmd
fastboot boot path\to\modded-twrp-g8x.img
```

### Formatting Windows and ESP partitions
```cmd
adb shell mkfs.ntfs -f /dev/block/by-name/win -L WINMH2LM5G
```

```cmd
adb shell mkfs.fat -F32 -s1 /dev/block/by-name/esp -n ESPMH2LM5G
```

## [Next step: Reinstalling Windows](3-install.md)
































