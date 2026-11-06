---
name: project-design
description: 项目架构与设计约束
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# tb321fu_pixelos_17_tangorpro_dp11.261016.003 — 设计

## 多语言

**简体中文** | [English](en/DESIGN.md) | [Español](es/DESIGN.md)

## 文档

- 项目概览: [README](../README.md)

- 设计思路: [DESIGN](DESIGN.md)

- 项目状态: [LOG](LOG.md)
- 历史记录: [HISTORY](HISTORY.md)
- 变更日志: [CHANGELOG](CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## 设计目标

- [x] 提供联想拯救者 Y700 三代(TB321FU)刷 Pixel Tablet Android 17 GSI `DP11.261016.003` 的完整 9008 刷机包.
- [x] 刷机包拆成不超过 GitHub 单个资产上限的 7z 分卷.
- [x] 每个 Release 附模块列表, 写明该版本打了哪些补丁模块.

## 架构

本仓库只负责分发成品, 不含构建系统. 补丁和构建系统在 [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods):

```text
GSI 底包 ──依次 apply 选中的 system 补丁──→ system 镜像 + vbmeta_system ─┐
原厂 boot ─apply 固件补丁────────────────→ boot 集                    ─┼─→ 完整 9008 包 ──7z 分卷──→ GitHub Release
原厂 9008 包 ────────────────────────────────────────────────────────┘
```

| 位置 | 职责 |
| --- | --- |
| `doc/` | 项目文档, 以及每个正式版的模块列表 `modules-<版本>.md` |
| GitHub Releases | 刷机包分卷, `SHA256SUMS.txt` 和模块列表 |

## 设计约束

- Release 只放完整 9008 刷机包, 不放 root 变体和增量补丁.
- 7z 分卷每卷最多 2000 MiB, 各卷按顺序拼接后与原 `.7z` 逐字节一致.
- 一个底包版本一个仓库: 本仓库只对应 Android 17 `DP11.261016.003`.
- system 分区保持原厂 `system_a` 的大小(3702558720 字节): Android 17 的 system 放不进 Android 15 线缩小后的分区.
