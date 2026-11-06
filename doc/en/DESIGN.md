---
name: project-design-en
description: Project architecture and design constraints
metadata:
  version: "1.0.0"
  lang: "en"
---

# tb321fu_pixelos_17_tangorpro_dp11.261016.003 — Design

## Multi-language

[简体中文](../DESIGN.md) | **English** | [Español](../es/DESIGN.md)

## Documentation

- Project overview: [README](README.md)

- Design rationale: [DESIGN](DESIGN.md)

- Project status: [LOG](LOG.md)
- Historical records: [HISTORY](HISTORY.md)
- Version changelog: [CHANGELOG](CHANGELOG.md)

- Third-party notices: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## Design Goals

- [x] Provide a complete 9008 flashing package that installs the Pixel Tablet Android 17 GSI `DP11.261016.003` on the Lenovo Legion Y700 Gen 3 (TB321FU).
- [x] Split the flashing package into 7z volumes that do not exceed GitHub's per-asset size limit.
- [x] Each release includes a module list that states which patch modules that version includes.

## Architecture

This repository only distributes finished products and does not contain the build system. Patches and the build system are in [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods):

```text
GSI base image ──apply selected system patches in order──→ system image + vbmeta_system ─┐
Stock boot ─────apply firmware patches──────────────────→ boot set                      ─┼─→ complete 9008 package ──7z volumes──→ GitHub Release
Stock 9008 package ─────────────────────────────────────────────────────────────────────┘
```

| Location | Responsibility |
| --- | --- |
| `doc/` | Project documentation, plus the module list `modules-<version>.md` for each release |
| GitHub Releases | Flashing package volumes, `SHA256SUMS.txt`, and the module list |

## Design Constraints

- Releases contain only the complete 9008 flashing package, not root variants or incremental patches.
- Each 7z volume is at most 2000 MiB. The volumes concatenated in order are byte-for-byte identical to the original `.7z`.
- One repository per base-image version: this repository only covers Android 17 `DP11.261016.003`.
- The system partition keeps the stock `system_a` size (3702558720 bytes): the Android 17 system does not fit in the partition as shrunk for the Android 15 line.
