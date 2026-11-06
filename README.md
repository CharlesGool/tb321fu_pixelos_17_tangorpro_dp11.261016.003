# tb321fu_pixelos_17_tangorpro_dp11.261016.003

**简体中文** | [English](doc/en/README.md) | [Español](doc/es/README.md)

联想拯救者 Y700 三代(TB321FU)刷 Pixel Tablet Android 17 GSI(`DP11.261016.003`)的 9008 刷机包.

[![Release](https://img.shields.io/github/v/release/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003?logo=github)](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/latest)

## 文档

- 项目概览: [README](README.md)

- 设计思路: [DESIGN](doc/DESIGN.md)

- 项目状态: [LOG](doc/LOG.md)
- 历史记录: [HISTORY](doc/HISTORY.md)
- 变更日志: [CHANGELOG](doc/CHANGELOG.md)

- 第三方声明: [THIRD_PARTY_NOTICES](doc/THIRD_PARTY_NOTICES.md)

## 简介

联想拯救者 Y700 三代(TB321FU)的 Pixel Tablet Android 17 GSI 9008 刷机包. 底包是 Google 官方 Android 17 QPR3 Beta 3(`DP11.261016.003`)镜像, 是 Beta 版系统. 当前正式版 user-v1.0(内部版本 V1). 已知问题见 [LOG](doc/LOG.md#限制).

### 重点, 写在前面

1. 本刷机包是 9008 刷机包, 可在不解锁 BL 的情况下刷入. 已签名, 不需要关 vbmeta 验证. 包里带完整的联想原厂 ZUI 17.0.12.044 9008 固件, 不需要事先刷官方底包.
2. 下列所有功能都是 rootless 实现.
3. 底包是 Google 的 Beta 版, 系统本身可能还有 Beta 阶段的问题.
4. 本刷机包可自动化构建. 有 Linux PC 的话, 可以自己更换 GSI 和底包版本并构建(底包和原厂固件需自备), 见 [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods).

每个模块的作用见 [modules-user-v1.0](doc/modules-user-v1.0.md). 本版共 34 个模块: 30 个必选模块加 4 个可选模块.

### 身份和认证

- 机型还原成 TB321FU(`ro.product.*`), 系统版本字段保留 Android 17 原值, 不谎报成原厂的 Android 15.
- rootless 密钥认证引擎: 可导入 keybox, 可本地下发 RKP 密钥(参考 Oh My Keymint 模块的功能).
- 全局机型伪装(可选模块 `global-pif`): 一键伪装成 Pixel Tablet 等机型. 开启后设置里的"版本号"也会显示伪装机型的值, 会被检测到系统异常挂载. 不想要就不开启.
- `herald-core`: 免 root 的命令和插件入口(`heraldctl`), 前端 App 通过它控制系统功能.

### 相机

- 内置联想原厂相机(ZuiCamera), 取自原厂 ZUI 固件: 主摄, 前摄, 微距都能用. 修了它在 GSI 上的闪退, 权限弹窗和"内存已满"误判(Android 17 上原判断恒为满, 快门没反应).
- 微距相机修复, 2 号微距相机对第三方相机可用.

### 人脸解锁

- 人脸解锁, 移植自 PixelOS 的 ParanoidSense(旷视 Megvii SDK), 走前置摄像头, 可用于锁屏解锁和 App 等非支付场景. 它是 WEAK(Class 2)生物识别, 不能授权支付或打开 StrongBox 凭据. 框架钩子已按 Android 17 的 `services.jar` 重新适配.

### 屏幕和显示

- 开机动画适配: Pixel Tablet 的开机动画按横屏尺寸做的, 在本机竖屏面板上比例错乱, 已转成本机尺寸. 平板横过来时画面正立.
- 自动亮度修复: 补回设置里的自适应亮度开关, 换上联想原厂的亮度曲线和反应速度, 手动最低档还原原厂.
- 屏幕方向修复: Pixel Tablet 系统和本机默认方向不同, 只能横屏的应用(如 B 站全屏)不再倒过来.
- 自适应刷新率修复: 补回原厂的空闲/触摸定时器和内容检测, 静止降档, 触摸升档, 视频按帧率.
- 流畅显示档位: 设置里"流畅显示"开 = 不设上限(面板最高 165Hz), 关 = 120Hz.
- 自由旋转: 90/180/270 度都能自动旋转, 包括竖屏倒过来.
- 桌面模式不限应用数: Android 17 桌面模式默认最多同时开 4 个应用, 已去掉上限.

### 电池

- `heraldctl battery`: 查看两颗电芯的循环次数和健康度, 长期插电保护, 充电上限, 旁路充电(插着充电器时由充电器直接供电, 电池不充不放).

### 网络和蓝牙

- 6GHz Wi-Fi 频段和国家码修复, Wi-Fi APEX 重新打包.
- 热点界面修复: Pixel Tablet 默认隐藏 WLAN 热点入口, 已补回设置入口和快捷设置磁贴.
- 蓝牙音频修复: 关 A2DP 硬件 offload 和 LE Audio, 改走 vendor 软件编码, 修耳机没声音和通话听不到.

### 应用兼容

- 移植 Tango, 支持 32 位 APK, 可以玩 32 位应用(参考一加 12 Android 15 ColorOS).

### 系统精简

- 删 Pixel 专属应用和数据, 精简本机没有的 Pixel 硬件组件, 清理 GSI(Mystic)残留.
- Google 应用只删 8 个: 计算器, 日历, 通讯录, 我的 Pixel, 信息, Kids Space, Play 图书, YouTube Kids. 其他 Google 应用(含数字健康)都保留.

### 去痕迹和收尾

- 关闭调试(原版 adb root/调试默认开着).
- 隐藏 GSI 身份痕迹(防 GSI 检测).
- 去掉开机"设备内部出现了问题"弹窗.
- 设置里"版本号"显示本包的版本.
- 统一内核编译者字符串, 清理过期预编译, boot 重新打包并针对本机 AVB 重签.

### 界面

- 可隐藏导航栏(平板/手机模式均支持), 隐藏后底部会留约 3 像素占位.

### 稳定性修复

- SELinux 规则修复: 开机即 Enforcing.
- 设备连接服务(DeviceConnectionService)域修复, 不再每 10 秒崩一次.

补丁和构建系统在 [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods). 每个底包版本一个仓库, 本仓库只对应 Android 17 `DP11.261016.003`; Android 15 的见 [tb321fu_pixelos_15_tangorpro_bp1a.250505.005](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_bp1a.250505.005) 和 [tb321fu_pixelos_15_tangorpro_ap3a.241005.015](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015).

## 要求

- 联想拯救者 Y700 三代 TB321FU, 能进 9008(EDL)模式.
- Windows 电脑, USB 数据线, 7-Zip.
- 保数据刷只适用于设备上原来就是本项目 Android 17 系统的情况; 其他情况用清数据刷.

## 安装

### 快速安装

从 [Releases](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/latest) 下载全部分卷(`.7z.001`, `.7z.002`, `.7z.003`)放在同一目录, 用 7-Zip 解压 `.7z.001`, 设备进 9008 后在解压目录运行 `flash.bat`(清数据). 刷机包带完整的联想原厂 9008 固件, 不需要事先刷官方底包.

### 常规安装

```bash
sha256sum -c SHA256SUMS.txt
7z x pixelos_tangorpro_dp11.261016.003_user-v1.0_optional.7z.001
```

保数据刷运行 `flash_keep_data.bat`. 两个脚本都在解压目录根部, 运行后输入 9008 端口号(如 `COM18`). 刷完没有自动开机时, 长按电源键和音量下 8 秒以上.

## 指南

| 文件 | 内容 |
| --- | --- |
| `…user-v1.0_optional.7z.001`, `.002`, `.003` | 完整 9008 刷机包的 7z 分卷, 每卷最多 2000 MiB |
| `SHA256SUMS.txt` | 分卷的校验值; 拼接后整包的 sha256 写在对应 Release 的说明里 |
| `module-list.md` | 本版模块列表, 同 [modules-user-v1.0](doc/modules-user-v1.0.md) |

### 使用人脸解锁

设置 → 安全与隐私 → 设备解锁 → 人脸解锁 → 输入 PIN → 走完录入, 之后锁屏对准前置摄像头即可. 它是 WEAK 生物识别, **禁止**用来保护支付, 银行这类应用.

### 电池管理

用 `heraldctl battery` 命令(需要 herald 配对的终端或前端 App):

```bash
heraldctl battery info              # 两颗电芯的循环次数, 健康度, 日期
heraldctl battery protect on 75 80  # 长期插电保护: 充到 80% 停, 掉到 75% 再充; protect off 关闭
heraldctl battery limit 80          # 充电上限 80%; limit off 关闭
heraldctl battery bypass on         # 旁路充电, 回包 verified=yes 才算生效; bypass off 关闭
```

### 版本标识

对外版本 `user-vMAJOR.MINOR`, 内部版本号 `V<数字>` 写在镜像里, user-v1.0 = V1. 每条底包线从自己的 v1.0 起编号. 全局伪装关闭时, 设置 → 关于平板电脑 → 版本号显示 `pixelos_tangorpro_dp11.261016.003_user-v1.0_optional`.

## 升级

本版是本仓库的第一个正式版, 没有旧版本可以升级. 从原厂系统, Android 15 线或其他系统换过来, 请用完整包运行 `flash.bat`(清数据). 保数据刷(`flash_keep_data.bat`)只适用于设备上原来就是本项目 Android 17 系统的情况. 验证: 开机后设置里的版本号应含 `user-v1.0`.

## 卸载

快速卸载没有单独的办法. 完全卸载: 用联想原厂 ZUI 17.0.12 的 9008 包整套重刷, 数据会被清除.

## 致谢

- Google 的 Pixel Tablet 官方 Android 17 QPR3 Beta 3(`DP11.261016.003`)镜像, 经 MysticGSI 工具转成 GSI 作为底包.
- 人脸解锁移植自 PixelOS 的 ParanoidSense(旷视 Megvii SDK).
- 认证引擎 fork 自 [ITxiao6666/OhMyKeymint](https://github.com/ITxiao6666/OhMyKeymint)(原作者 James Clef / qwq233).
- 联想 ZUI 原厂固件, 联想相机和刷机工具.

署名和合规信息见 [THIRD_PARTY_NOTICES](doc/THIRD_PARTY_NOTICES.md).

## 许可证

本仓库尚未选定许可证, 未授予任何许可. 第三方组件各自的许可证状态见 [THIRD_PARTY_NOTICES](doc/THIRD_PARTY_NOTICES.md).
