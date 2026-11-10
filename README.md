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

联想拯救者 Y700 三代(TB321FU)的 Pixel Tablet Android 17 GSI 9008 刷机包. 底包是 Google 官方 Android 17 QPR3 Beta 3(`DP11.261016.003`)镜像, 是 Beta 版系统. 当前正式版 **user-v2.0**(内部版本 V23), 本版改了什么见下方 [本版更新](#本版更新user-v20), 已知问题见 [LOG](doc/LOG.md#限制).

> Releases 目前提供 user-v1.2 完整包和之后各版的逐级增量补丁, 全新安装先刷 user-v1.2 完整包再逐级升级, 见 [安装](#安装) 和 [升级](#升级).

### 重点, 写在前面

1. 本刷机包是 9008 刷机包, 可在不解锁 BL 的情况下刷入. 已签名, 不需要关 vbmeta 验证. 包里带完整的联想原厂 ZUI 17.0.12.044 9008 固件, 不需要事先刷官方底包.
2. 下列所有功能都是 rootless 实现.
3. 底包是 Google 的 Beta 版, 系统本身可能还有 Beta 阶段的问题.
4. 本刷机包可自动化构建. 有 Linux PC 的话, 可以自己更换 GSI 和底包版本并构建(底包和原厂固件需自备), 见 [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods).

### 本版更新(user-v2.0)

相对 user-v1.9, 模块从 38 个增加到 48 个. 逐条记录见 [CHANGELOG](doc/CHANGELOG.md).

新增:

- **手电筒档位**: 下拉面板长按手电筒出现的亮度滑块真正生效, 1~4 档亮度不同. 原厂相机 HAL 两颗灯固定各 75 mA, 亮着改档还会关掉第二颗灯; 现在按档位重写闪光灯电流. 从关闭直接打开 1/2 档不会先闪一下强光; 重启后首次解锁前也生效.
- **双马达振动**: 系统侧振动 HAL 认两颗 Awinic 马达, 支持 primitives, 组合和同步振动, 第三方 App 的触感反馈正常.
- **三个内置设置 App**, 不用配对, 普通用户碰不到高级权限:
  - 电池管理: 设置 → 电池 → 电池管理.
  - 机型伪装与密钥认证: 设置 → 关于平板电脑 → 型号下面.
  - 隐藏导航栏: 设置 → 系统 → 导航模式 → 齿轮页底部.
- **预测性返回**: 上面三个 App 和联想相机都支持 Android 13+ 的返回手势动画.
- **合盖灭屏, 开盖亮屏**(皮套), 横屏握持时边缘跟随旋转.
- **快捷设置"自动亮度"磁贴**, 长按进 显示和触控.
- **纯 Wi-Fi 平板处理**: 去掉双卡和电话相关特性.
- **耗电统计补音视频**: 电池用量里音频和视频耗电不再是 0.

修复:

- **keystore2 崩溃**: herald 重启(崩溃或被杀)后 keystore2 不再跟着段错误, keystore 不再被锁, 分享 WLAN 等需要认证的操作不再闪退. 原因是启动脚本原地覆写了正被 keystore2 映射的 inject 载荷, 改成原子替换.
- **qesdk 崩溃循环**: 给高通 qesdk 服务的 seccomp 策略补 `lseek`, 不再每 5 秒崩一次.
- **开机 init 报错**: 补原厂 vendor rc 用到的两个 QTI 调试脚本 SELinux 类型, 开机不再 SIGABRT 两次.
- **dmesg 被刷屏**: herald 相关的 SELinux 拒绝日志静默, 不再冲掉其他内核日志.

### 模块

每个模块的作用见 [modules-user-v2.0](doc/modules-user-v2.0.md). 本版共 48 个模块: 44 个必选模块加 4 个可选模块. 下面按类别列出本版的全部功能.

### 身份和认证

- 机型还原成 TB321FU(`ro.product.*`), 系统版本字段保留 Android 17 原值, 不谎报成原厂的 Android 15.
- rootless 密钥认证引擎: 可导入 keybox, 可本地下发 RKP 密钥(参考 Oh My Keymint 模块的功能). 设置 → 关于平板电脑 → 型号下面有"机型伪装与密钥认证"App.
- 全局机型伪装(可选模块 `global-pif`): 一键伪装成 Pixel Tablet 等机型. 开启后设置里的"版本号"也会显示伪装机型的值, 会被检测到系统异常挂载. 不想要就不开启.
- `herald-core`: 免 root 的命令和插件入口(`heraldctl`), 前端 App 通过它控制系统功能.
- `herald-apps`: 给系统分区里的内置 App 开专用通道, 不用配对; 每个 App 只能执行自己白名单里的命令, 解锁前也可用.

### 相机

- 内置联想原厂相机(ZuiCamera), 取自原厂 ZUI 固件: 主摄, 前摄, 微距都能用. 修了它在 GSI 上的闪退, 权限弹窗和"内存已满"误判(Android 17 上原判断恒为满, 快门没反应). 支持预测性返回.
- 微距相机修复, 2 号微距相机对第三方相机可用.

### 人脸解锁

- 人脸解锁, 移植自 PixelOS 的 ParanoidSense(旷视 Megvii SDK), 走前置摄像头, 可用于锁屏解锁和 App 等非支付场景. 它是 WEAK(Class 2)生物识别, 不能授权支付或打开 StrongBox 凭据. 框架钩子已按 Android 17 的 `services.jar` 重新适配.

### 屏幕和显示

- 开机动画适配: Pixel Tablet 的开机动画按横屏尺寸做的, 在本机竖屏面板上比例错乱, 已转成本机尺寸. 平板横过来时画面正立.
- 自动亮度修复: 补回设置里的自适应亮度开关, 换上联想原厂的亮度曲线和反应速度, 手动最低档还原原厂. 快捷设置有"自动亮度"磁贴, 长按进 显示和触控.
- 屏幕方向修复: Pixel Tablet 系统和本机默认方向不同, 只能横屏的应用(如 B 站全屏)不再倒过来.
- 合盖灭屏, 开盖亮屏(皮套); 横屏握持时边缘跟随旋转.
- 双击唤醒: 设置 → 显示 → 其他显示控制 → 点按唤醒, 灭屏时双击亮屏.
- 自适应刷新率修复: 补回原厂的空闲/触摸定时器和内容检测, 静止降档, 触摸升档, 视频按帧率.
- 流畅显示档位: 设置里"流畅显示"开 = 不设上限(面板最高 165Hz), 关 = 120Hz.
- 自由旋转: 90/180/270 度都能自动旋转, 包括竖屏倒过来.
- 桌面模式不限应用数: Android 17 桌面模式默认最多同时开 4 个应用, 已去掉上限.

### 振动和手电筒

- 双马达振动 HAL: 两颗 Awinic 马达都认, 支持 primitives, 组合和同步振动, 第三方 App 触感正常.
- 手电筒档位: 下拉面板长按手电筒调亮度, 1~4 档真正生效, 从关闭打开 1/2 档不先闪强光.

### 电池

- 电池管理 App(设置 → 电池 → 电池管理)和 `heraldctl battery` 命令: 查看两颗电芯的循环次数和健康度, 长期插电保护, 充电上限, 旁路充电(插着充电器时由充电器直接供电, 电池不充不放), 实时状态.
- 耗电统计补音视频: 电池用量里音频和视频耗电不再是 0.

### 网络和蓝牙

- 按纯 Wi-Fi 平板处理: 去掉双卡和电话相关特性.
- 6GHz Wi-Fi 频段和国家码修复, Wi-Fi APEX 重新打包.
- 热点界面修复: Pixel Tablet 默认隐藏 WLAN 热点入口, 已补回设置入口和快捷设置磁贴.
- 蓝牙音频: A2DP 和 LE Audio(LC3)走高通 ADSP 硬件卸载, vendor 用原厂音频策略; 耳机详情里可开"LE 音频"; 修了 LE Audio 下语音下行约 60 秒延迟. 保留 SCO 通话补丁.

### 应用兼容

- 移植 Tango, 支持 32 位 APK, 可以玩 32 位应用(参考一加 12 Android 15 ColorOS).
- 内置完整的 Android System WebView(156.0.8078.47), 开机即可用; Pixel 镜像原来的"占位包 + 首次开机解压"在本机上解压不出来, 谷歌账号登录等界面会闪退.
- 视频冻结修复: X 等 App 刷视频不再"声音继续, 画面定格".

### 系统精简

- 删 Pixel 专属应用和数据, 精简本机没有的 Pixel 硬件组件, 清理 GSI(Mystic)残留.
- Google 应用只删 8 个: 计算器, 日历, 通讯录, 我的 Pixel, 信息, Kids Space, Play 图书, YouTube Kids. 其他 Google 应用(含数字健康)都保留.
- 删"正在播放"(Now Playing)及其声学模型, 音频不再被它占用.

### 去痕迹和收尾

- 关闭调试(原版 adb root/调试默认开着).
- 隐藏 GSI 身份痕迹(防 GSI 检测).
- 去掉开机"设备内部出现了问题"弹窗.
- 设置里"版本号"显示本包的版本.
- 统一内核编译者字符串, 清理过期预编译, boot 重新打包并针对本机 AVB 重签.

### 界面

- 可隐藏导航栏(平板/手机模式均支持): 设置 → 系统 → 导航模式 → 齿轮页底部的开关, 或 `heraldctl navbar hide/show/status`. 只在手势导航下生效, 切到三按钮导航时自动恢复显示. 隐藏后底部留约 3 像素占位(为保证返回手势正常).

### 稳定性修复

- SELinux 规则修复: 开机即 Enforcing.
- 设备连接服务(DeviceConnectionService)域修复, 不再每 10 秒崩一次.
- herald 重启后 keystore2 不再崩溃, 分享 WLAN 等不再闪退.
- 高通 qesdk 服务不再每 5 秒崩一次.
- 开机 init 子进程不再 SIGABRT; herald 的 SELinux 拒绝日志不再刷屏 dmesg.

补丁和构建系统在 [TB321FU-GSI-Mods](https://github.com/CharlesGool/TB321FU-GSI-Mods). 每个底包版本一个仓库, 本仓库只对应 Android 17 `DP11.261016.003`; Android 15 的见 [tb321fu_pixelos_15_tangorpro_bp1a.250505.005](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_bp1a.250505.005) 和 [tb321fu_pixelos_15_tangorpro_ap3a.241005.015](https://github.com/CharlesGool/tb321fu_pixelos_15_tangorpro_ap3a.241005.015).

## 要求

- 联想拯救者 Y700 三代 TB321FU, 能进 9008(EDL)模式.
- Windows 电脑, USB 数据线, 7-Zip.
- 用增量补丁升级时需要 Windows 版 `xdelta3.exe`(自行下载, 补丁里不带).
- 保数据刷只适用于设备上原来就是本项目 Android 17 系统的情况; 其他情况用清数据刷.

## 安装

### 快速安装

完整包目前只有 user-v1.2. 从 [user-v1.2 Release](https://github.com/CharlesGool/tb321fu_pixelos_17_tangorpro_dp11.261016.003/releases/tag/user-v1.2) 下载全部分卷(`.7z.001`, `.7z.002`, `.7z.003`)放在同一目录, 用 7-Zip 解压 `.7z.001`, 设备进 9008 后在解压目录运行 `flash.bat`(清数据). 刷机包带完整的联想原厂 9008 固件, 不需要事先刷官方底包. 刷好后按 [升级](#升级) 逐级升到 user-v2.0.

### 常规安装

```bash
sha256sum -c SHA256SUMS.txt
7z x pixelos_tangorpro_dp11.261016.003_user-v1.2_optional.7z.001
```

保数据刷运行 `flash_keep_data.bat`. 两个脚本都在解压目录根部, 运行后输入 9008 端口号(如 `COM18`). 刷完没有自动开机时, 长按电源键和音量下 8 秒以上.

## 指南

| 文件 | 内容 |
| --- | --- |
| `…user-v1.2_optional.7z.001`, `.002`, `.003` | 完整 9008 刷机包的 7z 分卷, 每卷最多 2000 MiB, 只在 user-v1.2 Release 里 |
| `…user-vX_patch_from_vY.zip` | 增量补丁(user-v1.3 起每个 Release 一个), 从上一版升到本版: system 镜像的 xdelta, `vbmeta_system.img`, 说明文本和 `升级说明.txt` |
| `SHA256SUMS.txt` | 本 Release 资产的校验值 |
| `module-list.md` | 本版模块列表, 同 [modules-user-v2.0](doc/modules-user-v2.0.md) |

### 使用人脸解锁

设置 → 安全与隐私 → 设备解锁 → 人脸解锁 → 输入 PIN → 走完录入, 之后锁屏对准前置摄像头即可. 它是 WEAK 生物识别, **禁止**用来保护支付, 银行这类应用.

### 电池管理

设置 → 电池 → 电池管理, 打开 App 即可查看和设置. 也可以用 `heraldctl battery` 命令(需要 herald 配对的终端或前端 App):

```bash
heraldctl battery info              # 两颗电芯的循环次数, 健康度, 日期
heraldctl battery protect on 75 80  # 长期插电保护: 充到 80% 停, 掉到 75% 再充; protect off 关闭
heraldctl battery limit 80          # 充电上限 80%; limit off 关闭
heraldctl battery bypass on         # 旁路充电, 回包 verified=yes 才算生效; bypass off 关闭
```

### 机型伪装与密钥认证

设置 → 关于平板电脑 → 型号下面进入. 在 App 里管理机型伪装和密钥认证. 命令行等价于 `heraldctl spoof tablet` / `heraldctl spoof on <机型>`.

### 隐藏导航栏

设置 → 系统 → 导航模式 → 齿轮页底部打开"隐藏导航栏". 需要手势导航; 切到三按钮导航时自动恢复显示, 切回手势导航再隐藏. 重启后保持.

### 手电筒档位

下拉快捷设置, 长按手电筒磁贴出现亮度滑块, 拖到 1~4 档.

### 版本标识

对外版本 `user-vMAJOR.MINOR`, 内部版本号 `V<数字>` 写在镜像里, user-v2.0 = V23. 每条底包线从自己的 v1.0 起编号. 全局伪装关闭时, 设置 → 关于平板电脑 → 版本号显示 `pixelos_tangorpro_dp11.261016.003_user-v2.0_optional`. 每一版的 `ro.system_ext.build.fingerprint` 末段带本版标记(user-v2.0 是 `.72bef0d4`), 保数据刷新版时系统会按升级重新扫描系统应用.

## 升级

各版 Release 的增量补丁都只从上一版升, 所以要逐级升:

| 当前版本 | 依次用的补丁 |
| --- | --- |
| user-v1.2 | v1.3 → v1.4 → v1.5 → v1.9 → v2.0 |
| user-v1.3 | v1.4 → v1.5 → v1.9 → v2.0 |
| user-v1.4 | v1.5 → v1.9 → v2.0 |
| user-v1.5 | v1.9 → v2.0 |
| user-v1.9 | v2.0 |

每一级的做法(详见补丁里的 `升级说明.txt`):

1. 核对当前刷机包目录里 `image\super_4.img` 的 sha256 和说明里的基准一致.
2. 用 `xdelta3 -d` 还原出新的 `super_4.img`, 再核对结果的 sha256, 对不上就删掉, 别刷.
3. 用新 `super_4.img` 和补丁里的 `vbmeta_system.img`, 说明文本覆盖刷机包目录里的同名文件.
4. 可以一路还原到 user-v2.0 再刷一次; 刷机用 `flash_keep_data.bat`(保数据). 首次开机比平时慢.

从原厂系统, Android 15 线或其他系统换过来, 先用 user-v1.2 完整包运行 `flash.bat`(清数据), 再按上表升级. 验证: 开机后设置里的版本号应含 `user-v2.0`.

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
