# Complete Guide: Installing Ubuntu Touch on Lenovo Tab M10 HD (TB-X306F)

## Table of Contents
1. [Introduction and Requirements](#introduction-and-requirements)
2. [Device Preparation](#device-preparation)
3. [Installing Drivers and Tools](#installing-drivers-and-tools)
4. [Bootloader Unlocking](#bootloader-unlocking)
5. [Flashing Android 11 Stock Firmware](#flashing-android-11-stock-firmware)
6. [Installing Ubuntu Touch](#installing-ubuntu-touch)
7. [Troubleshooting Common Issues](#troubleshooting-common-issues)
8. [Alternatives If the Process Fails](#alternatives-if-the-process-fails)
9. [References and Additional Resources](#references-and-additional-resources)

## Introduction and Requirements

This document provides a comprehensive guide for installing Ubuntu Touch on a Lenovo Tab M10 HD (model TB-X306F). This device uses a MediaTek chipset, which adds complexity to the process compared to other devices.

### Requirements:

- **Device**: Lenovo Tab M10 HD (TB-X306F) with battery charged to at least 50%
- **Computer**: Windows 10/11 (preferably)
- **USB Connection**: Good quality USB cable that supports data transfer
- **Backup**: Make a backup of your important data before starting
- **Time**: The complete process may take between 1-3 hours
- **Risks**: This process can "brick" (make unusable) your device if something goes wrong

**IMPORTANT NOTE**: This process will erase all data from your device. Make sure to back up any important information.

## Device Preparation

### 1. Activate Developer Options:

1. Go to **Settings > About device**
2. Tap 7 times on **Build number** until the message "You are now a developer" appears
3. Return to **Settings** and you will now see a new option called **Developer options**

### 2. Configure Developer Options:

1. Go to **Settings > Developer options**
2. Enable **OEM unlocking**
3. Enable **USB debugging**
4. Disable **Verify apps over USB**
5. On some devices, it's also necessary to enable **Fastboot mode**

### 3. Verify Compatibility:

1. Make sure your device is actually the TB-X306F model
2. In **Settings > About device**, verify that you have a MediaTek processor

## Installing Drivers and Tools

### 1. MediaTek and Lenovo USB Drivers:

MediaTek and Lenovo drivers are essential for your PC to communicate correctly with the tablet in different modes (normal, fastboot, recovery).

1. Download MediaTek USB drivers from [this link](https://www.mediatek.com/products/smartphones/usb-drivers) or [Driver Easy](https://www.drivereasy.com/knowledge/download-and-install-mediatek-usb-drivers/)
2. Download specific Lenovo drivers for the TB-X306F model from [Lenovo's official website](https://pcsupport.lenovo.com)
3. Install both drivers on your PC
4. Restart your PC after installation

### 2. Android Platform Tools (ADB and Fastboot):

1. Download the official version of Android Platform Tools from [here](https://developer.android.com/tools/releases/platform-tools)
2. Extract the contents to an easy-to-remember location (e.g., C:\android-sdk)
3. Add the path to the environment variables:
   - Search for "environment variables" in Windows
   - Edit the PATH variable
   - Add the path where you extracted the tools (e.g., C:\android-sdk\platform-tools)
4. Open a new Command Prompt (cmd) window as administrator
5. Verify the installation with:
   ```
   adb version
   fastboot --version
   ```

### 3. SP Flash Tool and MTK Auth Bypass Tool (Specific Tools for MediaTek):

1. Download SP Flash Tool from [spflashtool.org](https://spflashtool.org/) or [here](https://www.spflashtool.com/download)
2. Extract the contents to a dedicated folder
3. Look for the version compatible with Windows (generally v5.1916 or later)
4. Download MTK Auth Bypass Tool - this tool may be needed if you have problems with Secure Boot on your MediaTek device
5. Note that some versions of SP Flash Tool work better with certain devices - if you encounter problems, try a different version (such as v5.1628 or v5.2128)

### 4. UBports Installer:

1. Download UBports Installer from [devices.ubuntu-touch.io/installer](https://devices.ubuntu-touch.io/installer/)
2. Install the application following the installer's instructions

## Bootloader Unlocking

The bootloader is the first thing that runs when you turn on the device. It must be unlocked to install alternative operating systems.

### 1. Prepare the PC and the device:

1. Connect the tablet to the PC via USB cable
2. Make sure USB debugging is enabled
3. When the "Allow USB debugging?" message appears, check "Always allow from this computer" and tap "Allow"
4. Open a Command Prompt window as administrator

### 2. Verify ADB connection:

1. Run:
   ```
   adb devices
   ```
2. You should see your device listed with the word "device" next to it (not "unauthorized")

### 3. Restart in Bootloader mode:

1. Run:
   ```
   adb reboot bootloader
   ```
2. The tablet will restart and show a screen with "FASTBOOT mode..." or similar

### 4. Verify connection in Fastboot mode:

1. Run:
   ```
   fastboot devices
   ```
2. You should see your device listed

**NOTE**: If the device doesn't appear in fastboot devices, but appears in the Windows Device Manager (possibly as "Android" with a yellow warning), you need to install the correct driver:
   
1. Open Device Manager
2. Locate the device (it will probably appear as "Android" with a warning)
3. Right-click and select "Update driver"
4. Select "Browse my computer for drivers"
5. Select "Let me pick from a list of available drivers on my computer"
6. Select "Android Bootloader Interface" or a similar driver

### 5. Unlock the Bootloader:

1. Run one of these commands (different devices use different commands):
   ```
   fastboot oem unlock
   ```
   Or:
   ```
   fastboot flashing unlock
   ```

2. On the tablet screen, use the volume buttons to navigate and the power button to confirm the unlock
3. The device will restart and erase all data during this process

**NOTE**: On some Lenovo devices, you need to obtain an unlock code from Lenovo's official website. If the previous commands don't work, visit Lenovo's support site for information specific to your model.

**IMPORTANT**: Your TB-X306F model has Secure Boot enabled, which may block the standard unlocking process. If normal fastboot commands don't work, you may need to use MTK Auth Bypass Tool to overcome this protection. In extreme cases, you can also try to manually enter bootloader mode by holding down the power + volume down buttons while the tablet is off.

## Flashing Android 11 Stock Firmware

According to UBports documentation, you need to have Android 11 stock before installing Ubuntu Touch.

### 1. Obtain the firmware:

1. Download the Android 11 Stock firmware for TB-X306F. You can find it at:
   - [UBports community repository](https://gitlab.com/ubports/porting/community-ports/android11/lenovo-tab-m10-hd/lenovo-m10-hd)
   - [Lenovo's official website](https://pcsupport.lenovo.com/downloads/DS548918)

2. Extract the contents to a dedicated folder

### 2. Flash the firmware:

1. Open SP Flash Tool (run flash_tool.exe as administrator)
2. In the SP Flash Tool interface:
   - In "Download-Agent", select the DA.bin file that comes with the tool
   - Click on "Scatter-loading" and select the scatter.txt or MT6762_Android_scatter.txt file from the firmware you downloaded
   - Mark the necessary partitions (normally, preloader, system, boot, recovery)
   - Select "Download Only" in the dropdown menu

3. Turn off the tablet completely
4. Click the "Download" button in SP Flash Tool
5. Connect the tablet to the PC while it's off
6. SP Flash Tool should automatically detect the device and begin flashing
7. Wait for the process to finish (a green circle with "Download OK" will appear when complete)
8. The tablet will restart automatically

**NOTE**: If SP Flash Tool doesn't detect the device, try different USB cables, USB ports, or reinstall the MediaTek drivers. Also make sure to use the correct version of SP Flash Tool, as some versions work better with certain devices.

## Installing Ubuntu Touch

Once the device has Android 11 stock and the bootloader unlocked, we can install Ubuntu Touch.

### 1. Prepare the device:

1. Make sure the device has at least 50% battery
2. Reactivate Developer options and USB Debugging in Android 11
3. Connect the device to the PC

### 2. Use UBports Installer:

1. Open UBports Installer
2. The installer should automatically detect your device as "Lenovo Tab M10 HD (TB-X306F)"
3. Follow the on-screen instructions to install Ubuntu Touch
4. The installer will guide you through the different steps, which may include:
   - Restarting in fastboot mode
   - Installing a custom recovery
   - Installing the Ubuntu Touch system
   - Initial setup

5. Do not disconnect the device during the entire process

### 3. Initial Ubuntu Touch setup:

1. Once the installation is complete, the tablet will restart into Ubuntu Touch
2. Follow the initial setup wizard to configure Wi-Fi, language, etc.

## Troubleshooting Common Issues

### Issue 1: ADB doesn't detect the device

**Solution**:
- Verify that USB debugging is enabled
- Accept the allow USB debugging message on the tablet
- Try different USB cables
- Reinstall the drivers
- Restart both the tablet and the PC

### Issue 2: Fastboot doesn't detect the device

**Solution**:
- Check in Device Manager if it appears with a yellow warning
- Manually install the correct drivers as explained earlier
- Try the `adb reboot bootloader` command from normal mode
- Try holding down the volume and power buttons to manually enter fastboot

### Issue 3: SP Flash Tool doesn't detect the device

**Solution**:
- Make sure the tablet is completely off before connecting it
- Use a different USB port (preferably USB 2.0)
- Reinstall the MediaTek drivers
- Try a different version of SP Flash Tool
- Check the specific guide at [this link](https://gist.github.com/thekosmix/1a7fc2308e9c9ea78bb4ad24bc0cda1a)

### Issue 4: Error during flashing

**Solution**:
- Make sure you're using the correct firmware for your specific model
- Verify that all partitions are correctly selected
- Try flashing only the essential partitions
- Check XDA forums for solutions specific to your error

### Issue 5: UBports Installer gets stuck

**Solution**:
- Verify internet connection
- Try running the installer as administrator
- Verify that the bootloader is really unlocked
- Consider manually installing a custom recovery before using UBports Installer
- Check the [UBports community](https://forums.ubports.com/) for specific assistance

### Issue 6: Bootloop after installing Ubuntu Touch

**Solution**:
- Enter recovery mode (normally by holding Power + Volume up)
- Perform a factory reset from recovery
- If that doesn't work, reinstall Android 11 with SP Flash Tool
- Make sure the Ubuntu Touch version is compatible with your specific model
- Try flashing a custom recovery before reinstalling Ubuntu Touch

## Alternatives If the Process Fails

If after several attempts you can't install Ubuntu Touch, consider these alternatives:

### 1. Optimize existing Android:

1. Perform a factory reset
2. Uninstall non-essential applications (bloatware)
3. Use a lightweight launcher like Nova Launcher
4. Disable animations and visual effects

### 2. Python programming environment on Android:

1. Install Termux from the Play Store
2. Inside Termux, run:
   ```
   pkg update
   pkg upgrade
   pkg install python
   ```
3. Complement with Pydroid 3 for a more complete IDE

### 3. LineageOS or other alternative ROMs:

Look for a LineageOS ROM compatible with your device, which is usually lighter than the stock ROM.

## References and Additional Resources

- [Official repository for Lenovo Tab M10 HD port](https://gitlab.com/ubports/porting/community-ports/android11/lenovo-tab-m10-hd/lenovo-m10-hd)
- [SP Flash Tool guide](https://spflashtool.org/)
- [XDA Forums for TB-X306F](https://xdaforums.com/t/lenovo-tb-x306f-how-to-root-and-flash-recovery-help.4236557/)
- [SP Flash Tool troubleshooting](https://gist.github.com/thekosmix/1a7fc2308e9c9ea78bb4ad24bc0cda1a)
- [Lenovo bootloader unlock guide](https://unlocktechy.com/lenovo-tab-m10-hd-gen-2-unlock-bootloader)
- [MediaTek USB Drivers](https://www.drivereasy.com/knowledge/download-and-install-mediatek-usb-drivers/)
- [UBports Community](https://forums.ubports.com/)
- [Official Ubuntu Touch guide](https://ubuntu-touch.io/)
- [XDA Developers - General Forum](https://xdaforums.com/)

### Real-Time Support Channels

For real-time help during the process, consider joining these groups:
- [UBports Telegram Group](https://t.me/ubports)
- [UBports Discord](https://discord.gg/3PFsCmZ)

---

**FINAL NOTE**: This process involves risks and your device could become unusable if something goes wrong. Back up all your important data before starting. If you don't have experience with these procedures, consider seeking help in specialized forums or communities such as XDA Developers or UBports.
