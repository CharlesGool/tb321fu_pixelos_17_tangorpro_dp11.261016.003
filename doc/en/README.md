# tb321fu_pixelos_17_tangorpro_dp11.261016.003

[简体中文](../../README.md) | **English** | [Español](../es/README.md)

9008 flashing package for the Pixel Tablet Android 17 GSI (`DP11.261016.003`) on the Lenovo Legion Y700 Gen 3 (TB321FU).

[![Release](https://img.shields.io/github/v/release/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003?logo=github)](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/latest)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Introduction

The Pixel Tablet Android 17 GSI 9008 flashing package for the Lenovo Legion Y700 Gen 3 (TB321FU). The base image is Google's official Android 17 QPR3 Beta 3 (`DP11.261016.003`) image, which is a Beta system. The current release is **user-v2.0** (internal build V23); what changed is under [What's New](#whats-new-user-v20) below, and known issues are in [LOG](LOG.md#limitations).

> Releases currently provide the user-v1.2 full package plus step-by-step incremental patches for every later version. For a fresh install, flash the user-v1.2 full package first and then upgrade step by step; see [Install](#install) and [Upgrade](#upgrade).

### Key Points Up Front

1. This is a 9008 flashing package and can be flashed without unlocking the bootloader. It is signed, so vbmeta verification does not need to be disabled. The package includes the complete stock Lenovo ZUI 17.0.12.044 9008 firmware, so you do not need to flash the official base package first.
2. All features below are implemented rootless.
3. The base image is a Google Beta, so the system itself may still have Beta-stage issues.
4. This package can be built automatically. With a Linux PC, you can swap the GSI and base version and build it yourself (you need to provide the base image and stock firmware); see [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods).

### What's New (user-v2.0)

Compared with user-v1.9, the module count grows from 38 to 48. The item-by-item record is in [CHANGELOG](CHANGELOG.md).

Added:

- **Flashlight strength levels**: the brightness slider that appears when you long-press the flashlight tile now really works, with four distinct levels. The stock camera HAL fixes both LEDs at 75 mA each and even turns off the second LED when you change the level while it is on; the flash current is now rewritten per level. Turning on level 1 or 2 from off no longer flashes bright first, and it works before the first unlock after a reboot.
- **Dual-motor vibration**: a system-side vibrator HAL drives both Awinic motors, with primitives, compositions and synced vibration; haptics in third-party apps work.
- **Three built-in Settings apps**, no pairing needed, and advanced privileges stay out of reach of normal users:
  - Battery management: Settings → Battery → Battery management.
  - Device spoofing and key attestation: Settings → About tablet → below Model.
  - Hide navigation bar: Settings → System → Navigation mode → bottom of the gear page.
- **Predictive back**: the three apps above and the Lenovo camera support the Android 13+ back gesture animation.
- **Cover close turns the screen off, opening turns it on** (folio case); edges follow rotation when held in landscape.
- **Quick Settings "Auto brightness" tile**; long-press opens Display & touch.
- **Wi-Fi-only tablet handling**: dual-SIM and telephony features removed.
- **Audio/video in battery usage**: audio and video power use is no longer 0.

Fixed:

- **keystore2 crash**: after herald restarts (crash or kill), keystore2 no longer segfaults with it and the keystore no longer locks up, so Wi-Fi sharing and other operations that need authentication no longer crash. The cause was the startup script overwriting, in place, the inject payload that keystore2 had mapped; it is now replaced atomically.
- **qesdk crash loop**: `lseek` added to the seccomp policy of Qualcomm's qesdk service, so it no longer crashes every 5 seconds.
- **init errors at boot**: the SELinux types of two QTI debug scripts used by the stock vendor rc are added, so init no longer hits SIGABRT twice at boot.
- **dmesg flooding**: SELinux denial logs from herald are silenced and no longer push out other kernel logs.

### Modules

Each module is described in [modules-user-v2.0](../modules-user-v2.0.md). This release has 48 modules: 44 required modules plus 4 optional modules. All features of this release are listed by category below.

### Identity and Attestation

- The device model is restored to TB321FU (`ro.product.*`), and system version fields keep the original Android 17 values instead of falsely reporting stock Android 15.
- Rootless key attestation engine: can import a keybox and can provision RKP keys locally (based on the features of the Oh My Keymint module). The "Device spoofing and key attestation" app is under Settings → About tablet → below Model.
- Global device spoofing (optional module `global-pif`): spoof as Pixel Tablet and other models in one step. When enabled, the "Build number" in Settings also shows the spoofed model's value, and abnormal system mounts can be detected. Leave it off if you do not want it.
- `herald-core`: rootless command and plugin entry (`heraldctl`); frontend apps use it to control system features.
- `herald-apps`: a dedicated channel for built-in apps on the system partition, no pairing needed; each app can only run the commands on its own allowlist, and it works before unlock too.

### Camera

- Built-in stock Lenovo camera (ZuiCamera), taken from the stock ZUI firmware: main, front and macro cameras all work. Fixes its crashes, permission prompts and false "storage full" check on GSI (on Android 17 the original check always reported full, so the shutter did nothing). Supports predictive back.
- Macro camera fix: camera 2 (macro) is available to third-party camera apps.

### Face Unlock

- Face unlock, ported from PixelOS's ParanoidSense (Megvii SDK), uses the front camera and works for lock-screen unlock and non-payment app scenarios. It is WEAK (Class 2) biometrics and cannot authorize payments or unlock StrongBox credentials. Framework hooks were re-adapted to Android 17's `services.jar`.

### Display

- Boot animation adaptation: the Pixel Tablet boot animation is made for landscape dimensions and looks distorted on this device's portrait panel, so it is converted to this device's size. The image is upright when the tablet is held in landscape.
- Auto-brightness fix: restores the adaptive brightness switch in Settings, uses Lenovo's stock brightness curve and response speed, and restores the stock manual minimum level. Quick Settings has an "Auto brightness" tile; long-press opens Display & touch.
- Rotation fix: Pixel Tablet and this device have different default orientations; landscape-only apps (such as Bilibili full screen) are no longer upside down.
- Closing the folio case turns the screen off and opening turns it on; edges follow rotation when held in landscape.
- Double-tap to wake: Settings → Display → Other display controls → Tap to wake; double-tap the screen while it is off to wake it.
- Adaptive refresh-rate fix: restores stock idle/touch timers and content detection, lowering the rate when static, raising it on touch, and matching video frame rates.
- Smooth display levels: in Settings, "Smooth Display" on = no cap (panel max 165Hz), off = 120Hz.
- Free rotation: auto-rotate works at 90/180/270 degrees, including upside-down portrait.
- Unlimited apps in desktop mode: Android 17 desktop mode allows at most 4 apps at once by default; the limit is removed.

### Vibration and Flashlight

- Dual-motor vibrator HAL: both Awinic motors are recognized, with primitives, compositions and synced vibration; haptics in third-party apps work.
- Flashlight strength levels: long-press the flashlight tile to adjust brightness; levels 1 to 4 really take effect, and turning on level 1 or 2 from off no longer flashes bright first.

### Battery

- Battery management app (Settings → Battery → Battery management) and the `heraldctl battery` command: view cycle counts and health of both cells, long-term plugged-in protection, charge limit, bypass charging (with the charger plugged in, the charger powers the device directly and the battery neither charges nor discharges), live status.
- Audio and video in battery usage: audio and video power use is no longer 0.

### Network and Bluetooth

- Handled as a Wi-Fi-only tablet: dual-SIM and telephony features removed.
- 6GHz Wi-Fi band and country code fixes, with the Wi-Fi APEX repacked.
- Hotspot UI fix: Pixel Tablet hides the Wi-Fi hotspot entry by default; the Settings entry and Quick Settings tile are restored.
- Bluetooth audio: A2DP and LE Audio (LC3) use Qualcomm ADSP hardware offload, with the stock vendor audio policy; "LE audio" can be turned on in the earbud details; fixes the roughly 60-second voice downlink delay under LE Audio. The SCO call patch is kept.

### App Compatibility

- Ported Tango, supporting 32-bit APKs so 32-bit apps can run (based on OnePlus 12 Android 15 ColorOS).
- Built-in complete Android System WebView (156.0.8078.47), available right after boot; the Pixel image's original "stub package + first-boot extraction" could not extract on this device, which made Google account sign-in and similar screens crash.
- Video freeze fix: scrolling videos in X and similar apps no longer leaves the picture frozen while the sound keeps playing.

### System Trimming

- Removes Pixel-specific apps and data, trims Pixel hardware components this device lacks, and cleans GSI (Mystic) leftovers.
- Only 8 Google apps are removed: Calculator, Calendar, Contacts, My Pixel, Messages, Kids Space, Play Books, YouTube Kids. Other Google apps (including Digital Wellbeing) are kept.
- Removes Now Playing and its acoustic models, so it no longer holds the audio.

### Trace Removal and Cleanup

- Debugging disabled (the original image had adb root/debugging enabled by default).
- GSI identity traces hidden (against GSI detection).
- Removes the boot-time "There's an internal problem with your device" dialog.
- The "Build number" in Settings shows this package's version.
- Unifies the kernel builder string, cleans stale precompiled files, repacks boot and re-signs it for this device's AVB.

### Interface

- The navigation bar can be hidden (in both tablet and phone modes): the switch at the bottom of Settings → System → Navigation mode → gear page, or `heraldctl navbar hide/show/status`. It only applies with gesture navigation and is shown again automatically when you switch to three-button navigation. About 3 pixels of space remain at the bottom after hiding it (so the back gesture keeps working).

### Stability Fixes

- SELinux rule fixes: Enforcing from boot.
- DeviceConnectionService domain fix, so it no longer crashes every 10 seconds.
- keystore2 no longer crashes after herald restarts, so Wi-Fi sharing and similar no longer crash.
- Qualcomm's qesdk service no longer crashes every 5 seconds.
- init no longer hits SIGABRT at boot; herald SELinux denials no longer flood dmesg.

Patches and the build system are in [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods). Each base version has its own repository; this repository only covers Android 17 `DP11.261016.003`. For Android 15, see [tb321fu_pixelos_15_tangorpro_bp1a.250505.005](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_bp1a.250505.005) and [tb321fu_pixelos_15_tangorpro_ap3a.241005.015](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015).

## Requirements

- Lenovo Legion Y700 Gen 3 TB321FU, able to enter 9008 (EDL) mode.
- Windows PC, USB data cable, 7-Zip.
- Upgrading with incremental patches needs the Windows `xdelta3.exe` (download it yourself; it is not included in the patches).
- Keep-data flashing only applies when the device already runs this project's Android 17 system; otherwise use data-wipe flashing.

## Install

### Quick Install

The only full package so far is user-v1.2. Download all volumes (`.7z.001`, `.7z.002`, `.7z.003`) from the [user-v1.2 Release](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/tag/user-v1.2) into the same directory, extract `.7z.001` with 7-Zip, put the device into 9008 mode, and run `flash.bat` in the extracted directory (wipes data). The package includes the complete stock Lenovo 9008 firmware, so you do not need to flash the official base package first. Then follow [Upgrade](#upgrade) step by step to user-v2.0.

### Normal Install

```bash
sha256sum -c SHA256SUMS.txt
7z x pixelos_tangorpro_dp11.261016.003_user-v1.2_optional.7z.001
```

For keep-data flashing, run `flash_keep_data.bat`. Both scripts are in the root of the extracted directory; after running one, enter the 9008 port number (such as `COM18`). If the device does not boot automatically after flashing, hold the power and volume-down buttons for more than 8 seconds.

## Usage

| File | Contents |
| --- | --- |
| `…user-v1.2_optional.7z.001`, `.002`, `.003` | The 7z volumes of the complete 9008 flashing package, up to 2000 MiB each; only in the user-v1.2 Release |
| `…user-vX_patch_from_vY.zip` | Incremental patch (one per Release since user-v1.3) from the previous version to this one: an xdelta of the system image, `vbmeta_system.img`, text files and `升级说明.txt` (upgrade notes) |
| `SHA256SUMS.txt` | Checksums for this Release's assets |
| `module-list.md` | Module list for this release, same as [modules-user-v2.0](../modules-user-v2.0.md) |

### Using Face Unlock

Settings → Security & privacy → Device unlock → Face unlock → enter PIN → complete enrollment. After that, look at the front camera on the lock screen. It is WEAK biometrics and must **not** be used to protect payment, banking, or similar apps.

### Battery Management

Open Settings → Battery → Battery management to view and change the settings in the app. You can also use the `heraldctl battery` command (requires a herald-paired terminal or frontend app):

```bash
heraldctl battery info              # cycle counts, health, and dates for both cells
heraldctl battery protect on 75 80  # long-term plugged-in protection: stop at 80%, recharge at 75%; protect off disables it
heraldctl battery limit 80          # charge limit 80%; limit off disables it
heraldctl battery bypass on         # bypass charging; only active when the reply includes verified=yes; bypass off disables it
```

### Device Spoofing and Key Attestation

Open it from Settings → About tablet → below Model, and manage device spoofing and key attestation in the app. The command-line equivalent is `heraldctl spoof tablet` / `heraldctl spoof on <model>`.

### Hide Navigation Bar

Turn on "Hide navigation bar" at the bottom of Settings → System → Navigation mode → gear page. It needs gesture navigation; it is shown again automatically when you switch to three-button navigation and hidden again when you switch back to gestures. The setting persists across reboots.

### Flashlight Levels

Pull down Quick Settings, long-press the flashlight tile to show the brightness slider, and drag it to level 1 to 4.

### Version Naming

The public version is `user-vMAJOR.MINOR`; the internal build number `V<number>` is written into the image, and user-v2.0 = V23. Each base line numbers itself from its own v1.0. With global spoofing off, Settings → About tablet → Build number shows `pixelos_tangorpro_dp11.261016.003_user-v2.0_optional`. Each release adds its own marker to the last field of `ro.system_ext.build.fingerprint` (`.72bef0d4` for user-v2.0), so that after keep-data flashing a new release the system rescans system apps as an upgrade.

## Upgrade

Each Release's incremental patch only upgrades from the previous version, so upgrade step by step:

| Current version | Patches to apply, in order |
| --- | --- |
| user-v1.2 | v1.3 → v1.4 → v1.5 → v1.9 → v2.0 |
| user-v1.3 | v1.4 → v1.5 → v1.9 → v2.0 |
| user-v1.4 | v1.5 → v1.9 → v2.0 |
| user-v1.5 | v1.9 → v2.0 |
| user-v1.9 | v2.0 |

Each step (details in `升级说明.txt` in the patch):

1. Check that the sha256 of `image\super_4.img` in your current flashing package directory matches the base value in the notes.
2. Restore the new `super_4.img` with `xdelta3 -d`, then check the result's sha256; if it does not match, delete it and do not flash it.
3. Overwrite the files of the same name in the flashing package directory with the new `super_4.img`, the patch's `vbmeta_system.img`, and the text files.
4. You can restore all the way to user-v2.0 and flash once; flash with `flash_keep_data.bat` (keeps data). The first boot is slower than usual.

When switching from the stock system, the Android 15 line or another system, first run `flash.bat` (wipes data) with the user-v1.2 full package, then upgrade as in the table above. Verification: after boot, the build number in Settings should contain `user-v2.0`.

## Uninstall

There is no separate quick uninstall. For a full uninstall, re-flash the complete Lenovo stock ZUI 17.0.12 9008 package. All data will be erased.

## Acknowledgements

- Google's official Pixel Tablet Android 17 QPR3 Beta 3 (`DP11.261016.003`) image, converted to a GSI with the MysticGSI tool and used as the base image.
- Face unlock is ported from PixelOS's ParanoidSense (Megvii SDK).
- The attestation engine is forked from [ITxiao6666/OhMyKeymint](https://github.com/ITxiao6666/OhMyKeymint) (original author James Clef / qwq233).
- Lenovo's stock ZUI firmware, the Lenovo camera, and flashing tools.

For attribution and compliance information, see [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md).

## License

No license has been chosen for this repository, and no license is granted. For the license status of each third-party component, see [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md).
