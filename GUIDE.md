# Technical Guides & Troubleshooting 📖

Step-by-step instructions for bootloader unlocking, rooting, partition backups, sideloading official OTA updates, and device customization specifically tailored for the **Nothing Phone (2a) (Pacman)**.

---

## General Use & Troubleshooting

### OTA Sideloading (Manual System Updates)

:::note
* Bootloader unlocking is **not mandatory** to sideload official incremental or full OTA updates. Skip the partition restoration steps unless you are currently rooted.
* Sideloading official updates is completely safe as long as they are downloaded directly from the official Nothing Archive. Do not use third-party sources. All firmware is sourced directly from Nothing's official OEM servers.
* The built-in Nothing OS offline updater only accepts OEM-signed update packages. The updater verifies the cryptographic hash of the firmware before installation and will fail immediately if an incorrect or mismatched OTA zip is used.
* Because of these checks, it is impossible to brick your device by sideloading an official OTA zip on a locked or unlocked bootloader.
:::

#### A. Restoring Stock Partitions (For Rooted Users Only)
*If your bootloader is locked, skip directly to Section B.*

1. **Check your current Nothing OS version:**
   * Go to `Settings > About phone > Tap the device banner` and note down your exact current Build Number.
2. **Fetch stock images for your current firmware build:**
   * Download the `stock-boot.img` or `stock-init_boot.img` matching your exact current active build from your repository's Releases page.
3. **Flash stock partitions in Bootloader Mode:**
   * Reboot your phone to Fastboot mode:
     ```bash
     adb reboot bootloader
     ```
   * Flash the stock unmodified images to restore partition integrity (only modified partitions need to be flashed):
     * **For APatch / FolkPatch users:**
       ```bash
       fastboot flash boot stock-boot.img
       ```
     * **For KernelSU / KernelSU-Next / SukiSU-Ultra users:**
       ```bash
       fastboot flash init_boot stock-init_boot.img
       ```
4. **Reboot to System:**
   * Reboot your device normally. You will temporarily lose root access, which is required to allow the built-in updater to write blocks without security conflicts:
     ```bash
     fastboot reboot
     ```

---

#### B. Sideloading the Update (Offline Dialer Method)
1. **Download the Update Package:**
   * Download the official incremental or full OTA `.zip` firmware package corresponding to your target Nothing OS version.
2. **Create the OTA Directory:**
   * Open any file manager on your phone.
   * Create a folder named exactly `ota` in the root of your internal storage. The exact directory path must be:
     ```text
     /sdcard/ota/
     ```
   * Move the downloaded `<firmware_update>.zip` file into this `/sdcard/ota/` folder.
3. **Launch the Built-in Offline Updater:**
   * Open the stock **Phone Dialer app** on your Nothing Phone (2a) and dial:
     ```text
     *#*#682#*#*
     ```
   * This will launch the offline system updater interface (labeled as *NothingOfflineOtaUpdate* or *NOTHING BETA OTA UPDATE*).

:::tip
If the updater utility shows an unknown error or fails to scan, try using the **"Browse"** option inside the app to manually locate and select the OTA zip file instead of copying it to the `ota` folder.
:::

4. **Apply the Update:**
   * The utility will verify the zip file integrity. Tap **Directly Apply OTA** (or **Update**) to begin installation.
   * Wait for the verification and installation progress to reach 100%. The phone will automatically reboot into the newly updated Nothing OS version.
5. **Re-Root the Updated OS:**
   * Once your phone successfully boots into the newly updated Nothing OS version, download the newly generated patched images from your GitHub Release page matching the new version.
   * Flash them using the rooting steps in the Advanced section below to restore root.

---

### Battery Health Check (Hardware Calibration Diagnostics)
You can view hidden battery cycle counts and maximum factory capacity calibration metrics directly within Nothing OS using Shizuku and Root Activity Launcher.

1. **Prerequisites:**
   * Install [Shizuku](https://github.com/RikkaApps/Shizuku) and [Root Activity Launcher](https://github.com/LSPosed/RootActivityLauncher) on your phone.
   * Configure and start the Shizuku service on your device.
   * Grant Shizuku permission when opening Root Activity Launcher.
2. **Accessing the Hidden Activity:**
   * Open Root Activity Launcher and search for `Settings`.
   * Expand the Settings list, scroll down, and launch the Battery Information sub-activity listed exactly as:
     ```text
     com.android.settings/com.nothing.settings.NtSettings$BatteryInformationActivity
     ```
3. **Read Diagnostics:**
   * You will land on the hidden **Battery Information** page, displaying:
     * **Maximum Capacity:** The current calibrated battery health percentage.
     * **Cycle Count:** The total number of complete charge-discharge cycles.
     * **Production Date:** The physical manufacturing date of your battery.
     * **First Use Date:** The exact date the battery was first initialized on boot.

---

### Hidden Dialer Codes
Type these codes into the stock Phone Dialer app to access diagnostics, hidden menus, and testing interfaces:

* `*#*#4636#*#*` - **Testing Menu:** View detailed Wi-Fi, cellular network signal parameters, and usage statistics.
* `*#*#0#*#*` - **Nothing CIT (Factory Test):** Comprehensive hardware testing interface (screen pixels, cameras, Glyph LED diagnostics, vibration motor calibration, and speaker tests).
* `*#*#682#*#*` - **Offline OTA Updater:** Utility for manually sideloading official update packages.

---

## Advanced Guides

### Prerequisites & Tools

#### 1. USB Drivers
* **Windows Users:** Install the official [Google USB Driver](https://developer.android.com/studio/run/win-usb) to allow your PC to recognize and write to the device in ADB and Fastboot interfaces.

#### 2. SDK Platform Tools (ADB & Fastboot)
* Always use the latest official [Google SDK Platform-Tools](https://developer.android.com/studio/releases/platform-tools). Do not use outdated third-party minimal fastboot installers, as they cannot correctly process modern dynamic partition maps or GKI headers on Android 13+.

---

### Unlocking the Bootloader
Unlocking the bootloader is a mandatory prerequisite before you can flash any patched root partition images or custom recovery partitions.

:::warning
* Unlocking the bootloader will trigger a complete factory reset. All personal files, apps, and accounts on the device's internal storage will be erased. Back up your essential data before proceeding.
* Unlocking the bootloader voids the official OEM warranty. However, you can restore your warranty status by flashing untouched stock firmware and relocking the bootloader.
* You will temporarily lose Device Integrity certification. Some banking and secure apps may detect this and refuse to run unless patched using Play Integrity fixes.
:::

1. **Enable Developer Options:**
   * Go to `Settings > About phone > Tap Software info > Tap Build number 7 times` until a toast notification appears confirming you are a developer.
2. **Enable OEM Unlocking & USB Debugging:**
   * Go to `Settings > System > Developer options`.
   * Turn ON **OEM Unlocking** and **USB Debugging**.
3. **Establish PC Connection:**
   * Connect your phone to your PC using a reliable USB cable.
   * Open a command prompt or terminal in your PC's platform-tools directory.
   * Verify the ADB connection:
     ```bash
     adb devices
     ```
     *(If prompted on your phone, authorize USB Debugging permission).*
4. **Reboot to Bootloader:**
   * Execute the following command to reboot your device into Fastboot Mode:
     ```bash
     adb reboot bootloader
     ```
   * Verify the fastboot connection:
     ```bash
     fastboot devices
     ```
     *(If your device serial number is not displayed, re-install your PC's USB drivers).*
5. **Execute Unlock Command:**
   * Run the bootloader unlock command:
     ```bash
     fastboot flashing unlock
     ```
6. **Confirm on Device:**
   * Use the `Volume` keys on your phone to select the **"Unlock the bootloader"** option, then press the `Power` button to confirm.
   * The device will wipe all data, reboot, and display an unlocked bootloader warning logo during boot.

---

### Rooting Guides

#### Method 1: APatch or FolkPatch (Modifying boot.img)
APatch and FolkPatch utilize the KernelPatch framework, which directly modifies the Linux kernel inside the main `boot` partition block.

1. **Reboot to Fastboot:**
   * Boot your phone into **Fastboot Mode** (Power + Volume Down during startup, or via `adb reboot bootloader`).
2. **Flash the Patched Boot Image:**
   * Execute the command corresponding to your chosen manager:
     * **For APatch:**
       ```bash
       fastboot flash boot APatch-boot.img
       ```
     * **For FolkPatch:**
       ```bash
       fastboot flash boot FolkPatch-boot.img
       ```
3. **Reboot to System:**
   * Reboot your phone back into Nothing OS:
     ```bash
     fastboot reboot
     ```
4. **Install Manager APK:**
   * Download and install the respective stable manager APK from your repository's Releases page.
   * **APatch Setup:** Open the app and enter the defined SuperKey password: `root@123`.
   * **FolkPatch Setup:** Open the app; root privileges are automatically managed using the default `su` interface.

---

#### Method 2: KernelSU, KernelSU-Next, or SukiSU-Ultra (Modifying init_boot.img)
KernelSU-based variants use GKI LKM ramdisk injection and strictly patch the **`init_boot.img`** partition.

1. **Reboot to Fastboot:**
   * Reboot your phone into **Fastboot Mode**.
2. **Flash the Patched Init_Boot Image:**
   * Execute the command corresponding to your preferred manager package:
     * **For KernelSU (Official):**
       ```bash
       fastboot flash init_boot KernelSU-init_boot.img
       ```
     * **For KernelSU-Next (Normal APK):**
       ```bash
       fastboot flash init_boot KernelSU-Next-Normal-init_boot.img
       ```
     * **For KernelSU-Next (Spoof APK):**
       ```bash
       fastboot flash init_boot KernelSU-Next-Spoof-init_boot.img
       ```
     * **For SukiSU-Ultra:**
       ```bash
       fastboot flash init_boot SukiSU-Ultra-init_boot.img
       ```
3. **Reboot to System:**
   * Reboot your phone back to Nothing OS:
     ```bash
     fastboot reboot
     ```
4. **Install Manager APK:**
   * Install the matching stable manager APK downloaded from your GitHub Release page. Your device is now successfully rooted.

---

### Backing Up Essential Partitions
After unlocking your bootloader, it is highly recommended to back up your device's unique hardware-level partitions (containing your IMEI, network settings, and fingerprint sensor calibration data) before flashing any system modifications.

:::danger
Never share your partition backups with anyone else! They contain device-specific identifiers like your unique IMEI numbers. If these partitions are corrupted or lost, your device will lose cellular connectivity and hardware calibration.
:::

#### Partitions Backup Procedure (Via Termux)
1. **Prerequisites:**
   * Ensure your device is currently rooted with any manager (APatch, FolkPatch, KernelSU, or SukiSU).
   * Install the [Termux](https://f-droid.org/en/packages/com.termux/) app (preferably via F-Droid).
2. **Execute Backup Commands:**
   * Open Termux and request root access by executing:
     ```bash
     su
     ```
   * Copy and paste the following commands in one go to backup MediaTek unique partition blocks:
     ```bash
     mkdir -p /sdcard/partitions_backup/
     cd /sdcard/partitions_backup
     dd if=/dev/block/by-name/nvram of=/sdcard/partitions_backup/nvram.img
     dd if=/dev/block/by-name/nvdata of=/sdcard/partitions_backup/nvdata.img
     dd if=/dev/block/by-name/nvcfg of=/sdcard/partitions_backup/nvcfg.img
     dd if=/dev/block/by-name/persist of=/sdcard/partitions_backup/persist.img
     ```
3. **Secure Your Backup:**
   * Open your phone's file manager, navigate to the newly created `partitions_backup` folder in your internal storage, and transfer it to a secure external storage device (such as a PC or Cloud storage) for safekeeping.

---

### Relocking the Bootloader
To restore your phone to full stock factory settings, pass Play Integrity verification natively, and lock the system partition block.

:::warning
Relocking the bootloader requires 100% untouched stock partitions. Do not execute this command if you have a modified kernel, patched boot/init_boot image, or custom ROM installed, as it will result in a hardbrick.
:::

1. **Flash Stock Partition Backup Images:**
   * Ensure both unmodified images are flashed first in fastboot mode:
     ```bash
     fastboot flash boot stock-boot.img
     fastboot flash init_boot stock-init_boot.img
     ```
2. **Execute Relock Command:**
   * While in bootloader mode, run:
     ```bash
     fastboot flashing lock
     ```
3. **Confirm on Device:**
   * Use the `Volume` buttons to select the **"Lock the bootloader"** option, and press the `Power` button to confirm. The device will reset and boot back to pristine stock Nothing OS.
