---
name: project-changelog-en
description: Changelog
metadata:
  version: "1.0.0"
  lang: "en"
---

# Changelog

## Multi-language

[简体中文](../CHANGELOG.md) | **English** | [Español](../es/CHANGELOG.md)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Changelog

### user-v2.0(V23)- 2026-11-10

- Added: dual-motor vibrator HAL (`vibrator-hal`, system-side IVibratorManager, both Awinic motors recognised, third-party haptics work).
- Added: flashlight strength levels (`torch-levels`, the quick-settings brightness slider's 1-4 levels now take effect; turning on level 1/2 from off no longer flashes bright first; LED triggers self-heal; works before first unlock after a reboot).
- Added: a channel for built-in feature apps (`herald-apps`) and three Settings apps — battery, device spoofing & key attestation (About), and hide-navbar (navigation-mode page); these three and the Lenovo camera all support predictive back.
- Added: Wi-Fi-only handling (`wifi-only`), audio/video power profile (`power-profile`), qesdk seccomp lseek fix (`qesdk-seccomp-fix`), cover-close sleep / edge rotation (`cover-rotation-fix`), long-press auto-brightness tile (`qs-auto-brightness`), and no more init child SIGABRT at boot (`qti-testscripts-fix`).
- Fixed: after a herald restart (crash/kill) keystore2 would segfault and keystore would lock (Wi-Fi share etc. crashed). Root cause: the startup script overwrote the inject payload in place while keystore2 had it mmap'd; `attestation-fix` V1.4 switches to an atomic replace (temp file + rename for a new inode).
- Changed: module count 38 → 48 (44 required + 4 optional); `battery` V1.2, `attestation-fix` V1.4, `navbar-hide` V1.2, `zuicamera` V1.5, `herald-apps` V1.2.
- Note: this Release ships an incremental patch from user-v1.9; the full package will be added later. The internal variant `user-v2.0-cust_bebug` additionally carries custom fonts/emoji, ush and one-tap spoofing, and is not distributed publicly.

### user-v1.9(V10)- 2026-11-09

- Added: double-tap to wake (`dt2w` V1.1, Settings → Display → tap to wake; `heraldctl dt2w`).
- Added: video-freeze fix (`video-freeze-fix`, videos in X etc. no longer "sound continues, picture frozen").
- Changed: Bluetooth (`bluetooth-fix` V2.0) moved to Qualcomm ADSP hardware offload — A2DP and LE Audio (LC3), vendor uses the stock audio policy, "LE audio" appears in headset details, and the ~60 s voice downlink delay under LE Audio is fixed; the SCO call patch is kept.
- Changed: module count 36 → 38 (34 required + 4 optional).
- Note: this Release ships an incremental patch from user-v1.5; the full package will be added later.

### user-v1.5(V6)- 2026-11-07

- Fixed: Now Playing still auto-started. v1.3 removed the Now Playing app, but Android System Intelligence (ASI) kept re-starting ambient music recognition from the four acoustic models at `product/etc/firmware/music_detector.*`, holding a recording session open and occasionally making audio choppy again. This version bumps `now-playing` to V1.1 and also removes those four models (descriptor / sound_model / sound_model_2 / sound_model_tflite); with no models to load, ASI cannot start it, so Now Playing never auto-starts again.
- Note: Same module set as user-v1.4 (36 modules); otherwise unchanged. This Release provides the incremental patch from user-v1.4 first; the full package will be added later.

### user-v1.4(V5)- 2026-11-07

- Added: One-tap spoofing (`attestation-fix` V1.2). `heraldctl spoof tablet` spoofs as a Pixel Tablet in a single command, `spoof on <product>` enables any built-in model in one step, and `spoof off` disables it — no more apply→get→set steps. Tested on-device: `spoof tablet` passes Play certification.
- Note: Same module set as user-v1.3 (36 modules); otherwise unchanged. This Release provides the incremental patch from user-v1.3 first; the full package will be added later.

### user-v1.3(V4)- 2026-11-07

- Fixed: Choppy audio. Now Playing (ambient music recognition) kept a recording session open and competed with normal playback for the audio path. New required module `now-playing` removes the Now Playing app (`NowPlayingPrebuilt`) and its sysconfig; audio is back to normal after removal. It is a standalone app, not part of Android System Intelligence, so removing it does not affect Live Caption and similar features.
- Changed: Module count 35 → 36 (32 required plus 4 optional).
- Note: Otherwise identical to user-v1.2. This Release provides the incremental patch from user-v1.2 first; the full package will be added later.

### user-v1.2(V3)- 2026-11-06

- Note: The first public release of this repository. The user-v1.0 Release was never fully uploaded and was withdrawn for lacking a working WebView; user-v1.1 (V2) was not published and its changes are included here.
- Fixed: No working WebView in the system, so Google account sign-in and similar screens crashed. New required module `webview-full` ships the full Android System WebView 156.0.8078.47 (since user-v1.1).
- Fixed: After a keep-data flash of a new release the system did not rescan its system apps and reused records from the previous release (after a keep-data flash of user-v1.1, WebView still could not load its native library). Each release now appends a release marker to `ro.system_ext.build.fingerprint` (`identity` V1.4); `ro.build.fingerprint` is unchanged.
- Changed: Module count 34 → 35 (31 required plus 4 optional).

### user-v1.0(V1)- 2026-11-06

- Added: The first release of this repository, providing the complete 9008 flashing package volumes for the Android 17 QPR3 Beta 3 (`DP11.261016.003`) base image, `SHA256SUMS.txt`, and the module list.
- Note: The Android 17 base-image line is numbered independently from v1.0. Its patch set is aligned with the Android 15 line: all required modules plus 4 optional modules, 34 in total.
- Changed: Face unlock, smooth display, the 6GHz fix, and Tango have been re-adapted for Android 17.
- Changed: The camera is now the built-in Lenovo stock camera (ZuiCamera). Fixed its false "memory full" detection on Android 17, which made the shutter unresponsive before taking photos.
- Changed: Only 8 Google apps are removed (Calculator, Calendar, Contacts, Pixel Tips, Messages, Kids Space, Play Books, YouTube Kids). Other apps, including Digital Wellbeing, are kept.
- Added: The boot animation was converted to this device's portrait panel size, fixing the distorted proportions.
- Added: Desktop mode no longer limits the number of open apps (previously 4).
- Added: Battery management was merged into the required modules (lifespan view, long-term plug-in protection, charge limit, bypass charging).
- Fixed: The boot popup "Internal problem with the device" (version mismatch).
- Fixed: The "Version number" in Settings always showed `Ported.Using.MysticGSI.Tool`.
