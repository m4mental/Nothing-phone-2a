# Nothing Phone (2a) (Pacman) - Rooting & OTA Flashing Guide 📖

Step-by-step instructions for bootloader unlocking, rooting, partition backups, sideloading official OTA updates, and device customization specifically tailored for the **Nothing Phone (2a) (Pacman)** across both **Stable** and **Open Beta** channels.

---

## ⚠️ Prerequisites

Before you begin, ensure you have prepared the following:

1. **Unlocked Bootloader:** Your phone's bootloader must be unlocked. (Warning: Unlocking the bootloader will wipe all data on your device. Please back up your files first.)
2. **PC/Laptop & USB Cable:** Required to run fastboot commands.
3. **Android Platform Tools (ADB & Fastboot):** Install the official Google ADB/Fastboot drivers on your PC.
4. **USB Debugging:** Enable USB Debugging in Developer Options on your phone.

---

## 🛠️ Flashing & Rooting Instructions

### 🟢 Method 1: KernelSU, KernelSU-Next, or SukiSU-Ultra (Modifying init_boot.img)

KernelSU-based variants use GKI LKM ramdisk injection and strictly patch the **`init_boot.img`** partition.

1. **Reboot to Fastboot:**
   * Power off your phone. Press and hold `Power` and `Volume Down` buttons together to boot into **Fastboot Mode**, or run:

     ```bash
     adb reboot bootloader
     ```

2. **Verify Fastboot Connection:**

   ```bash
   fastboot devices
   ```

3. **Flash the Patched Init_Boot Image:**

#### A. If You Are on the STABLE Channel (Nothing OS Stable)

* Download the images from the **Stable** releases section.
* Flash explicitly to both slot A and slot B:
  * **For KernelSU (Official):**

    ```bash
    fastboot flash init_boot_a KernelSU-init_boot.img
    fastboot flash init_boot_b KernelSU-init_boot.img
    ```

  * **For KernelSU-Next (Normal APK):**

    ```bash
    fastboot flash init_boot_a KernelSU-Next-Normal-init_boot.img
    fastboot flash init_boot_b KernelSU-Next-Normal-init_boot.img
    ```

  * **For KernelSU-Next (Spoofed APK / SUSFS):**

    ```bash
    fastboot flash init_boot_a KernelSU-Next-Spoof-init_boot.img
    fastboot flash init_boot_b KernelSU-Next-Spoof-init_boot.img
    ```

  * **For SukiSU-Ultra:**

    ```bash
    fastboot flash init_boot_a SukiSU-Ultra-init_boot.img
    fastboot flash init_boot_b SukiSU-Ultra-init_boot.img
    ```

#### B. If You Are on the OPEN BETA Channel (Nothing OS Beta)

* Download the images from the **Open Beta** releases section (prefixed with `Beta`).
* Flash explicitly to both slot A and slot B:
  * **For KernelSU (Official Beta):**

    ```bash
    fastboot flash init_boot_a KernelSU-Beta-init_boot.img
    fastboot flash init_boot_b KernelSU-Beta-init_boot.img
    ```

  * **For KernelSU-Next (Normal Beta APK):**

    ```bash
    fastboot flash init_boot_a KernelSU-Next-Normal-Beta-init_boot.img
    fastboot flash init_boot_b KernelSU-Next-Normal-Beta-init_boot.img
    ```

  * **For KernelSU-Next (Spoofed Beta APK / SUSFS):**

    ```bash
    fastboot flash init_boot_a KernelSU-Next-Spoof-Beta-init_boot.img
    fastboot flash init_boot_b KernelSU-Next-Spoof-Beta-init_boot.img
    ```

  * **For SukiSU-Ultra (Beta):**

    ```bash
    fastboot flash init_boot_a SukiSU-Ultra-Beta-init_boot.img
    fastboot flash init_boot_b SukiSU-Ultra-Beta-init_boot.img
    ```

1. **Reboot to System:**

   ```bash
   fastboot reboot
   ```

5. **Install Matching Manager APK:**
   * Install the matching manager APK from the Releases download links. Your device is now fully rooted!

---

## 🔄 How to Install Official OTA Updates Safely (Anti-Bootloop Method)

If your device is rooted, do not install system updates directly without restoring stock images.

### Step 1: Restore Stock Images

1. Download unmodified `stock-init_boot.img` (or `stock-beta-init_boot.img` if on Beta) from the corresponding Release page.
2. Reboot phone to Fastboot mode:

   ```bash
   adb reboot bootloader
   ```

3. Flash stock init_boot:

   ```bash
   fastboot flash init_boot stock-init_boot.img
   fastboot reboot
   ```

4. Your phone will boot with full stock integrity (root temporarily unhooked).

### Step 2: Install Official System Update

1. Install the OTA update normally via `Settings > System > System update`.
2. Let the phone reboot normally into the new OS build.

### Step 3: Re-Root

1. Open this repository's Releases page.
2. Download newly generated patched images matching your new OS version (Stable or Beta).
3. Flash via Fastboot as described above to restore root safely!

---

## 🛡️ Critical Hardware Partition Backups (Via Termux)

After unlocking your bootloader, it is highly recommended to back up your device's unique hardware partitions (IMEI, network calibration, fingerprint) before flashing any system modifications:

1. Open Termux app on phone and get root shell:

   ```bash
   su
   ```

2. Run backup commands:

   ```bash
   mkdir -p /sdcard/partitions_backup/
   cd /sdcard/partitions_backup
   dd if=/dev/block/by-name/nvram of=/sdcard/partitions_backup/nvram.img
   dd if=/dev/block/by-name/nvdata of=/sdcard/partitions_backup/nvdata.img
   dd if=/dev/block/by-name/nvcfg of=/sdcard/partitions_backup/nvcfg.img
   dd if=/dev/block/by-name/persist of=/sdcard/partitions_backup/persist.img
   ```

3. Move the `/sdcard/partitions_backup/` folder to a secure PC or drive. Never share these files as they contain your private IMEI numbers.
