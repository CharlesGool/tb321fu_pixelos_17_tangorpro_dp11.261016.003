---
name: project-third-party-notices
description: 第三方署名与合规声明
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# 第三方声明

## 多语言

**简体中文** | [English](en/THIRD_PARTY_NOTICES.md) | [Español](es/THIRD_PARTY_NOTICES.md)

## 文档

- 项目概览: [README](../README.md)

- 设计思路: [DESIGN](DESIGN.md)

- 项目状态: [LOG](LOG.md)
- 历史记录: [HISTORY](HISTORY.md)
- 变更日志: [CHANGELOG](CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## 第三方声明

### 组件清单

| 组件名称 | 版本或哈希 | 上游地址 | 许可证类型 | 使用方式 | 版权归属 | 发布合规义务 | 核验日期 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Google Pixel Tablet 官方镜像(经 MysticGSI 工具转为 GSI) | `DP11.261016.003`, 增量 16708287 | 未核验 | 未核验 | 底包, 修改后重新打包 | Google | 未核验 | 未核验 |
| 人脸解锁(移植自 PixelOS 的 ParanoidSense, 含旷视 Megvii SDK) | 未核验 | https://github.com/CharlesGool/paranoidsense-gsi-port | 未核验 | 编译为 FaceUnlock 系统应用放入系统镜像 | PixelOS / 旷视(Megvii) | 未核验 | 未核验 |
| 联想 ZUI 原厂固件和 9008 工具 | ZUI 17.0.12.044 | 未核验 | 未核验 | 随 9008 包分发其中的 vendor, boot 等固件和刷机工具 | 联想 | 未核验 | 未核验 |
| 联想相机(ZuiCamera) | 17.0.2.250609115111 | 未核验 | 未核验 | 取自原厂 ZUI 固件, 打补丁后重签, 装为 `system/priv-app` | 联想 | 未核验 | 未核验 |
| OhMyKeymint(认证引擎 fork) | 基线提交 bafe5ba | https://github.com/ITxiao6666/OhMyKeymint | 未核验(上游含 AGPL-3.0 文本和 qwq233 的附加协议) | 编译为 `keymint` 和 `inject`, 放入系统镜像 | James Clef / qwq233 | 未核验 | 未核验 |
| Tango(32 位应用二进制翻译) | v2.1.0 | 未核验 | 未核验 | 翻译器等放入系统镜像 | 未核验 | 未核验 | 未核验 |

### 许可证文本

未核验的组件暂无许可证文本. 补齐前, 本文件不构成对这些组件的授权.

### 源代码提供

认证引擎的源码在 [herald-core](https://github.com/CharlesGool/herald-core) 仓库的 `engine/`. 其余组件未核验.

### 合规审查

未做. OhMyKeymint 受 Copyleft 约束, 尚无分发与分离分析记录.
