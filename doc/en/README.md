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

The Pixel Tablet Android 17 GSI 9008 flashing package for the Lenovo Legion Y700 Gen 3 (TB321FU). The base image is Google's official Android 17 QPR3 Beta 3 (`DP11.261016.003`) image, which is a Beta system. The current release is user-v1.2 (internal build V3). Known issues are in [LOG](LOG.md#limitations).

### Key Points Up Front

1. This is a 9008 flashing package that can be flashed without unlocking the bootloader (BL). It is signed, and disabling vbmeta verification is not required. The package includes the complete Lenovo stock ZUI 17.0.12.044 9008 firmware, so you do not need to flash an official base image beforehand.
2. All features below are implemented rootless.
3. The base image is a Google Beta build, so the system itself may still have issues from the Beta stage.
4. This package can be built automatically. With a Linux PC, you can replace the GSI and base-image versions and build it yourself (the base image and stock firmware must be supplied by you); see [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods).

The function of each module is described in [modules-user-v1.2](../modules-user-v1.2.md). This release includes 35 modules: 31 required modules plus 4 optional modules.

### Identity and Attestation

- Restores the device identity to TB321FU (`ro.product.*`). The system version fields keep their original Android 17 values and do not claim to be the stock Android 15.
- Rootless key attestation engine: keybox import is supported, and RKP keys can be issued locally (refer to the functionality of the Oh My Keymint module).
- Global device spoofing (optional module `global-pif`): one-tap spoofing as a Pixel Tablet and other models. When enabled, the "Version number" in Settings also shows the spoofed model's value, and this is detected as an abnormal system mount. Leave it off if you do not want this.
- `herald-core`: root-free command and plugin entry point (`heraldctl`); the front-end app uses it to control system features.

### Camera

- Built-in Lenovo stock camera (ZuiCamera), taken from the stock ZUI firmware: the main, front and macro cameras all work. Fixed its crash on the GSI, its permission popups, and the false "memory full" detection (on Android 17 the original check always reported full, so the shutter did nothing).
- Macro camera fix: macro camera 2 is available to third-party camera apps.

### Face Unlock

- Face unlock, ported from PixelOS's ParanoidSense (Megvii SDK). It uses the front camera and can be used for lock screen unlock and for apps and other non-payment scenarios. It is a WEAK (Class 2) biometric and cannot authorize payments or open StrongBox credentials. The framework hooks have been re-adapted to Android 17's `services.jar`.

### Display

- Boot animation adaptation: the Pixel Tablet boot animation was made for landscape dimensions, so its proportions were distorted on this device's portrait panel. It has been converted to this device's dimensions. When the tablet is held landscape, the picture is upright.
- Auto brightness fix: restores the adaptive brightness toggle in Settings, uses Lenovo's stock brightness curve and response speed, and restores the lowest manual level to stock.
- Screen orientation fix: the Pixel Tablet system and this device have different default orientations, so apps that only support landscape (such as Bilibili fullscreen) no longer appear upside down.
- Adaptive refresh rate fix: restores the stock idle and touch timers and content detection. It drops the rate when the screen is static, raises it on touch, and follows the frame rate for video.
- Smooth display level: in Settings, "Smooth display" on = no cap (panel up to 165Hz), off = 120Hz.
- Free rotation: auto-rotates at 90, 180 and 270 degrees, including portrait upside down.
- Desktop mode with no app limit: Android 17 desktop mode allows up to 4 apps open at once by default. The limit has been removed.

### Battery

- `heraldctl battery`: view the cycle count and health of both battery cells, long-term plug-in protection, a charge limit, and bypass charging (while a charger is plugged in, the charger powers the device directly and the battery neither charges nor discharges).

### Network and Bluetooth

- 6GHz Wi-Fi band and country code fix; Wi-Fi APEX repackaged.
- Hotspot UI fix: the Pixel Tablet hides the WLAN hotspot entry by default. The Settings entry and the Quick Settings tile have been restored.
- Bluetooth audio fix: disables A2DP hardware offload and LE Audio and uses vendor software encoding instead, fixing headphones with no sound and callers who cannot be heard.

### App Compatibility

- Ported Tango, which supports 32-bit APKs so that 32-bit apps can run (refer to OnePlus 12 Android 15 ColorOS).
- Ships the full Android System WebView (156.0.8078.47), usable from first boot. The Pixel image's original "stub plus decompress on first boot" setup fails to decompress on this device, which made Google account sign-in and similar screens crash.

### System Trimming

- Removes Pixel-exclusive apps and data, trims Pixel hardware components that this device does not have, and cleans up GSI (Mystic) leftovers.
- Only 8 Google apps are removed: Calculator, Calendar, Contacts, Pixel Tips, Messages, Kids Space, Play Books and YouTube Kids. All other Google apps (including Digital Wellbeing) are kept.

### Trace Removal and Cleanup

- Disables debugging (stock adb root and debugging are on by default).
- Hides GSI identity traces (against GSI detection).
- Removes the "Internal problem with the device" popup at boot.
- The "Version number" in Settings shows this package's version.
- Unifies the kernel build-user string, cleans up expired prebuilts, and repackages boot, re-signing it for this device's AVB.

### Interface

- The navigation bar can be hidden (supported in both tablet and phone modes). About 3 pixels of space remain at the bottom after hiding it.

### Stability Fixes

- SELinux rule fix: Enforcing on boot.
- DeviceConnectionService domain fix: no longer crashes every 10 seconds.

Patches and the build system are in [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods). Each base-image version has its own repository; this repository only covers Android 17 `DP11.261016.003`. For Android 15, see [tb321fu_pixelos_15_tangorpro_bp1a.250505.005](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_bp1a.250505.005) and [tb321fu_pixelos_15_tangorpro_ap3a.241005.015](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015).

## Requirements

- Lenovo Legion Y700 Gen 3 TB321FU, able to enter 9008 (EDL) mode.
- A Windows PC, a USB data cable, and 7-Zip.
- Keep-data flashing only applies to a device already running this project's Android 17 system. In all other cases, use the wipe-data flash.

## Install

### Quick Install

Download all volumes (`.7z.001`, `.7z.002`, `.7z.003`) from [Releases](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/latest) into the same directory. Extract `.7z.001` with 7-Zip. Once the device is in 9008 mode, run `flash.bat` (wipe data) in the extracted directory. The flashing package includes the complete Lenovo stock 9008 firmware, so there is no need to flash an official base image beforehand.

### Normal Install

```bash
sha256sum -c SHA256SUMS.txt
7z x pixelos_tangorpro_dp11.261016.003_user-v1.2_optional.7z.001
```

For a keep-data flash, run `flash_keep_data.bat`. Both scripts are in the root of the extracted directory. After running them, enter the 9008 port number (e.g. `COM18`). If the device does not power on automatically after flashing, hold the power button and volume down for more than 8 seconds.

## Usage

| File | Contents |
| --- | --- |
| `…user-v1.2_optional.7z.001`, `.002`, `.003` | The 7z volumes of the complete 9008 flashing package, up to 2000 MiB each |
| `SHA256SUMS.txt` | Checksums for the volumes; the sha256 of the concatenated complete package is in the notes of the corresponding Release |
| `module-list.md` | Module list for this release, same as [modules-user-v1.2](../modules-user-v1.2.md) |

### Using Face Unlock

Settings → Security & privacy → Device unlock → Face unlock → enter PIN → complete enrollment. Afterward, point the front camera at your face on the lock screen to unlock. It is a WEAK biometric, and **do not** use it to protect payment or banking apps.

### Battery Management

Use the `heraldctl battery` command (requires a terminal or front-end app paired through herald):

```bash
heraldctl battery info              # cycle count, health and date of both cells
heraldctl battery protect on 75 80  # long-term plug-in protection: stop charging at 80%, resume at 75%; protect off disables it
heraldctl battery limit 80          # charge limit 80%; limit off disables it
heraldctl battery bypass on         # bypass charging; only active once the reply shows verified=yes; bypass off disables it
```

### Version Naming

The public version is `user-vMAJOR.MINOR`, and the internal build number `V<number>` is written into the image; user-v1.2 = V3. Each base-image line is numbered from its own v1.0. With global spoofing off, Settings → About tablet → Version number shows `pixelos_tangorpro_dp11.261016.003_user-v1.2_optional`. Each release appends a release marker to the last field of `ro.system_ext.build.fingerprint` (`.7c5abca8` for user-v1.2), so a keep-data flash of a new release makes the system rescan its system apps as an upgrade.

## Upgrade

This is the first public release of this repository (the user-v1.0 Release was never fully uploaded and was withdrawn for lacking a working WebView; user-v1.1 was not published), so there is no public earlier version to upgrade from. When coming from the stock system, the Android 15 line or any other system, run `flash.bat` (wipe data) from the full package. Keep-data flashing (`flash_keep_data.bat`) only applies to a device already running this project's Android 17 system. Verification: after boot, the version number in Settings should contain `user-v1.2`.

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
