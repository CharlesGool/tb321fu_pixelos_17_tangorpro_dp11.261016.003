---
name: project-changelog
description: 变更日志
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# 变更日志

## 多语言

**简体中文** | [English](en/CHANGELOG.md) | [Español](es/CHANGELOG.md)

## 文档

- 项目概览: [README](../README.md)

- 设计思路: [DESIGN](DESIGN.md)

- 项目状态: [LOG](LOG.md)
- 历史记录: [HISTORY](HISTORY.md)
- 变更日志: [CHANGELOG](CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](THIRD_PARTY_NOTICES.md)

## 变更日志

### user-v1.2(V3)- 2026-11-06

- 说明: 本仓库第一个公开的正式版. user-v1.0 的 Release 没有传完, 因缺可用的 WebView 作废; user-v1.1(V2)未公开发布, 它的改动并入本版.
- 修复: 系统里没有可用的 WebView, 谷歌账号登录等界面闪退. 新增必选模块 `webview-full`, 内置完整的 Android System WebView 156.0.8078.47(user-v1.1 起).
- 修复: 保数据刷新版后系统不重新扫描系统应用, 沿用旧版留下的记录(user-v1.1 保数据刷入后 WebView 仍加载不了原生库). 每一版的 `ro.system_ext.build.fingerprint` 末段带本版标记(`identity` V1.4), `ro.build.fingerprint` 不变.
- 变更: 模块数 34 → 35(31 个必选加 4 个可选).

### user-v1.0(V1)- 2026-11-06

- 新增: 本仓库的首个 Release, 提供 Android 17 QPR3 Beta 3(`DP11.261016.003`)底包的完整 9008 刷机包分卷, `SHA256SUMS.txt` 和模块列表.
- 说明: Android 17 底包线从 v1.0 起独立编号, 补丁集对齐 Android 15 线: 全部必选模块加 4 个可选模块, 共 34 个.
- 变更: 人脸解锁, 流畅显示, 6GHz 修复和 Tango 按 Android 17 重新适配.
- 变更: 相机改为内置联想原厂相机, 修复它在 Android 17 上拍照前"内存已满"的误判(快门没反应).
- 变更: Google 应用只删 8 个(计算器, 日历, 通讯录, 我的 Pixel, 信息, Kids Space, Play 图书, YouTube Kids), 保留数字健康等其余应用.
- 新增: 开机动画转成本机竖屏面板的尺寸, 修复比例错乱.
- 新增: 桌面模式不限同时打开的应用数(原来最多 4 个).
- 新增: 电池管理并入必选模块(寿命查看, 长期插电保护, 充电上限, 旁路充电).
- 修复: 开机"设备内部出现了问题"(版本不一致)弹窗.
- 修复: 设置里的"版本号"一直显示 `Ported.Using.MysticGSI.Tool`.
