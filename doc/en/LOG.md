---
name: project-log-en
description: Project bugs, limitations, decisions, and handoff
metadata:
  version: "1.0.0"
  lang: "en"
---

# Log

## Multi-language

[简体中文](../LOG.md) | **English** | [Español](../es/LOG.md)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Records

- [History](HISTORY.md)
- [Changelog](CHANGELOG.md)

## Bugs

No confirmed bugs.

## Limitations

- The base image is Google's Android 17 QPR3 Beta 3, not a stable release system.
- The system image ships with a backup keybox seed. It stops working once it expires or is revoked, after which key attestation requires the user to import a valid keybox. The validity of the seed in this release has not been verified.
- Keep-data flashing only applies to a device already running this project's Android 17 system.
- With global spoofing enabled, the "Version number" in Settings shows the spoofed model's value. To confirm which version is flashed, check the `ro.tb321fu.mod_version` property.
- The Lenovo camera's watermark does not work: it requires the camera HAL to recognize the Lenovo camera, which this release does not do.
- The Lenovo camera needs to read the real device model. If the model is changed by means other than global spoofing, it may not work.
- LE Audio is disabled; dual-mode headphones use classic Bluetooth.
- Face unlock is a WEAK biometric: it can unlock and do weak verification, but it cannot authorize payments or open StrongBox credentials.
- The boot animation is upright when the tablet is held landscape; when held in portrait, the picture is sideways.
- About 3 pixels of space remain at the bottom after hiding the navigation bar.
- With no app limit in desktop mode, memory pressure is high when many windows are open at once, and background apps may be reclaimed by the system.

## Decisions

| Date | Decision | Status | Reason |
| --- | --- | --- | --- |
| 2026-11-06 | One repository per base-image version | Accepted | Consistent with the Android 15 line |
| 2026-11-06 | Releases contain only the complete 9008 flashing package, not root variants or incremental patches | Accepted | Consistent with the Android 15 line |
| 2026-11-06 | 7z volumes are 2000 MiB each | Accepted | Below GitHub's 2 GiB per-asset limit |
| 2026-11-06 | Release numbers are per base-image line; the first release of this repository is user-v1.0 | Accepted | Each base-image line is numbered from v1.0 |
| 2026-11-06 | The base image is `DP11.261016.003`, not `CP3A.260905.009` | Accepted | The CP3A base image does not boot on this device; the stock DP11 base image boots normally |
| 2026-11-06 | The built-in camera is the Lenovo stock camera, no longer the Pixel Camera | Accepted | The Pixel Camera is unreliable on Android 17 |
| 2026-11-06 | Third-party licenses not yet verified when the repository is made public | Accepted | Follows the approach of the Android 15 line repository; verification is listed as a remaining step |

## Handoff

The current handoff is maintained in the [Simplified Chinese LOG.md](../LOG.md#交接).
