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
