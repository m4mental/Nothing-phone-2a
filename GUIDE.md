# Nothing Phone (2a) (Pacman) - Rooting & OTA Flashing Guide 📖

This comprehensive guide will walk you through the step-by-step process of rooting your Nothing Phone (2a) using various root managers, and how to safely install official OTA updates without losing data or bootlooping.

---

## ⚠️ Prerequisites
Before you begin, ensure you have prepared the following:
1. **Unlocked Bootloader:** Your phone's bootloader must be unlocked. (Warning: Unlocking the bootloader will wipe all data on your device. Please back up your files first.)
2. **PC/Laptop & USB Cable:** Required to run fastboot commands.
3. **Android Platform Tools (ADB & Fastboot):** Install the official Google ADB/Fastboot drivers on your PC.
4. **USB Debugging:** Enable USB Debugging in the Developer Options on your phone.

---

## 🛠️ Flashing & Rooting Instructions

### Method 1: APatch or FolkPatch (Modifying boot.img)
APatch and FolkPatch modify the **`boot.img`** partition.

1. Power off your phone. Press and hold the `Power` and `Volume Down` buttons together to boot into **Fastboot Mode**.
2. Connect your phone to your PC using a reliable USB cable.
3. Open a Command Prompt (CMD) or Terminal window in your PC's platform-tools folder and verify connection:
   ```bash
   fastboot devices
