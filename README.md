# VK Protect (2017)

Legacy Android hack for **Micromax Canvas Pace 4G Q415**.  
On a wrong pattern unlock attempt, the phone takes a selfie with the front camera and sends it to your VK (VKontakte) inbox.  

Originally written in 2017 as an experiment.  
⚠️ This is **legacy code**, not maintained and not secure.  
Archived here for historical/educational purposes only.  

---

## Features
- Hooks into `SystemUI.apk` (patched version provided in Releases).
- Captures photo using Termux + Termux:API (`termux-camera-photo`).
- Sends photo to VK inbox via PHP + curl.
- Works only on **CyanogenMod/LineageOS 13** (tested on Micromax Canvas Pace 4G Q415).

---

## Requirements
- Micromax Canvas Pace 4G Q415  
- CyanogenMod/LineageOS 13  
- Root access  
- [Termux](https://play.google.com/store/apps/details?id=com.termux) + [Termux:API](https://play.google.com/store/apps/details?id=com.termux.api)  
- PHP installed in Termux  
- Patched `SystemUI.apk` (included in Releases)  
- VK API token and your VK user ID  

---

## Installation

### 1. Root
Make sure your device is rooted.  

### 2. Install Termux and Termux:API
From Google Play.  

### 3. Setup Termux
```bash
pkg install termux-api
pkg install php
```
When asked, type `y` to confirm.  

Check that `termux-camera-photo` works:
```bash
termux-camera-photo -c 0 /sdcard/t.png
```
It should request permissions for camera and storage. Grant them.  

Then copy the binary to system:
```bash
cp /data/data/com.termux/files/usr/bin/termux-camera-photo /system/bin/
chmod 755 /system/bin/termux-camera-photo
```

### 4. Replace SystemUI
Replace `/system/priv-app/SystemUI.apk` with the patched version.  
Set the same permissions as the original.  

### 5. Place additional files
Create directory:  
```
/data/data/com.android.systemui/files/
```
Put the 4 additional files from the archive there.  
Set permissions to `777` (all checkboxes).  

### 6. VK setup
1. Log into VK from a browser.  
2. Follow [this link](https://vk.com/dev/implicit_flow_user) (or similar) to get a token.  
3. Copy the `access_token` between `access_token=` and `&` into `vk_token.txt`.  
4. Copy your `user_id` into `vk_id.txt`.  

### 7. Reboot
After reboot, when someone enters the wrong pattern, the script will:  
- Take a photo with the front camera.  
- Send it to your VK inbox.  

---

## How it works
- `SystemUI` triggers `protect` script on wrong unlock attempt.  
- `protect` script:
  - captures photo with `termux-camera-photo`,  
  - waits 15 seconds,  
  - runs `vkup.php` with the photo timestamp.  
- `vkup.php` uploads the photo to VK via VK API.  

---

## Disclaimer
- This project is from **2017**, written as an experiment.  
- It may cause bootloops, crashes, or bricks.  
- It is not secure and not production-ready.  
- VK token must be reissued if you change your password.  
- Archived here for educational/historical purposes only.  

---

## License
MIT License (you can change to any license you prefer).
