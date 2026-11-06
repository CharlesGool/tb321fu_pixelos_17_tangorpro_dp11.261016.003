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
