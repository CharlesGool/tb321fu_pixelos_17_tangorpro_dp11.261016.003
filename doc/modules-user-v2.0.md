---
name: modules-user-v2-0
description: user-v2.0 的补丁模块列表
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# user-v2.0 模块列表

本版(内部版本 V23)由 48 个补丁模块构建: 44 个 `required` 模块, 加 4 个 `optional` 模块. 原始清单见刷机包根目录的 `MODULES.txt` 和 `SELECTION.txt`.

构建参数: `VERSION=V23 VERSION_LABEL=user-v2.0 PROFILE=user-v1.4 CAMERA=zui`(预设名 `user-v1.4` 沿用 Android 15 线的"标准选集"叫法, 不是本包的发布号).
设备: 联想拯救者 Y700 三代 TB321FU, 底包 Pixel Tablet(tangorpro)Android 17 QPR3 Beta 3 `DP11.261016.003` GSI(Google 官方镜像经 MysticGSI 工具转成), vendor 和 boot 用联想原厂 ZUI 17.0.12.044.

相对 user-v1.9 新增: 双马达振动 HAL, 纯 Wi-Fi 处理, 耗电统计补音视频, qesdk seccomp, 合盖灭屏/边缘旋转, 快捷设置自动亮度长按, 手电筒档位, 内置功能 App 通道和三个带界面的 App(电池/机型伪装/导航栏), 以及这三个 App 和联想相机的预测性返回; keystore2 在 herald 重启后不再崩溃. 逐条见 [CHANGELOG](CHANGELOG.md).

## 身份与隐藏

| 模块 | 作用 |
|---|---|
| 机型还原 `identity` V1.4 | `ro.product.*` 改成 TB321FU, 指纹保留 Android 17 的版本段只换机型段; 各分区 build.prop 的 `ro.build.display.id` 都写本包版本号; `ro.system_ext.build.fingerprint` 末段加本版标记, 保数据刷新版时系统按升级重新扫描系统应用 |
| 关闭调试 `debug-off` | 关掉调试相关开关 |
| 隐藏 GSI 痕迹 `gsi-hide` | `partition.*.verified` 改 1, 去掉虚拟机属性标签 |
| 全局指纹伪装 `global-pif` (可选) | 全局指纹伪装; 开启后"版本号"也显示伪装机型的值 |
| 密钥认证 `attestation-fix` V1.4 | keymint + inject: 签出 deviceLocked / verifiedBootState=green 的证书链, 附指纹伪装(一键 `heraldctl spoof tablet` / `spoof on <机型>`), keybox, 本机 RKP, 安全补丁同步命令. 带"机型伪装与密钥认证"App(设置 → 关于平板电脑 → 型号下面). 本版修 herald 重启后 keystore2 崩溃: 载荷二进制改原子替换, 不再原地覆写被 mmap 的 inject(避免截断丢掉已重定位的私有页) |
| herald 核心 `herald-core` V1.4 | 免 root 的控制后端: broker + `heraldctl` + 插件接口. 只提供命令通道, 具体能力由别的补丁接入 |
| 内置功能 App 通道 `herald-apps` V1.2 | 给装在系统分区的内置功能 App 开专用文件通道, 不用配对; 每个 App 只能跑 `apps/<包名>.allow` 白名单里的命令, 用户接触不到高级权限. 同时服务 CE 和 DE 存储(解锁前可用) |

## 精简

| 模块 | 作用 |
|---|---|
| 精简旧版 VNDK `old-vndk` | 只留 vendor 要用的 VNDK v34 apex, 删 v31 到 v33 |
| 精简 Pixel 专属应用和数据 `pixel-leftovers` | 删 Play 自动安装配置, "正在播放"歌曲库, Pixel 相机后台服务, 儿童模式 |
| 精简 Pixel 硬件组件 `pixel-hardware` | 删本机没有对应硬件的组件: 底座, 投屏, 零售演示, Tensor 服务, 侧边指纹, UWB, 强光/UV, 基带, NFC 应用 |
| 精简 Google 应用 `google-apps-min` | 只删 8 个: 计算器, 日历, 通讯录, 我的 Pixel, 信息, Kids Space, Play 图书, YouTube Kids; 数字健康等其余应用保留 |
| 去除 Now Playing `now-playing` V1.1 | 删环境音乐识别应用及其 sysconfig, 并删 ASI 的 4 个声学模型, "文曲知音"彻底不自启, 音频不再被它占用 |
| 精简 GSI 残留 `gsi-traces` | 删 Mystic 的 vendor overlay 和嵌套的 system 残留目录 |

## 启动与 SELinux

| 模块 | 作用 |
|---|---|
| SELinux 修复 `selinux/fix` | 开机即 Enforcing: early-init 就 `setenforce 1`, 内核命令行加 `enforcing=1 selinux=1` |
| 设备连接服务域修复 `dcservice-seapp-fix` | 删指向 `dcservice_app` 的 `seapp_contexts` 规则, 否则设备连接服务每 10 秒崩一次 |
| QTI 调试脚本类型 `qti-testscripts-fix` V1.0 | 补原厂 vendor rc 用到的两个 QTI 调试脚本 SELinux 类型(不给权限), 开机 init 子进程不再 SIGABRT 两次 |
| herald 域静默 `herald-quiet` V1.1 | herald 域 dontaudit, dmesg 不再被 herald 的 avc 冲掉; 另外静默 init 起 herald 域服务时的 noatsecure/rlimitinh/siginh |
| 去开机内部问题弹窗 `build-consistency-dialog-fix` V1.1 | 去掉开机"设备内部出现了问题"弹窗. Android 17 上改 `services.jar` 里弹窗的调用方, 不动 `framework.jar` |
| 清过期预编译 `stale-boot-image` | 清掉因改 jar 而过期的预编译产物 |
| 内核编译者字符串 `kernel-builder-string` | 修改内核编译者字符串 |
| boot 打包重签 `boot-assemble` | 汇总改动, 重签 boot |

## 硬件修复

| 模块 | 作用 |
|---|---|
| 自动亮度修复 `auto-brightness-fix` | 补回设置里的自适应亮度开关, 换上联想原厂的自动亮度曲线和反应速度, 手动最低档还原原厂 |
| 自适应刷新率修复 `adaptive-refresh-fix` | 补回原厂 SurfaceFlinger 的空闲/触摸定时器和内容检测(静止降档, 触摸升档, 视频按帧率) |
| 屏幕方向修复 `rotation-fix` | 只能横屏的应用画面不再倒过来; 去掉 Pixel Tablet 底座的方向设置 |
| 合盖灭屏/边缘旋转 `cover-rotation-fix` V1.1 | 皮套合盖灭屏、开盖亮屏; 横屏握持时边缘跟随旋转 |
| 开机动画适配 `bootanim-portrait` | 把 Pixel Tablet 横屏尺寸的开机动画转成本机面板的 1600x2560, 平板横拿时画面正立 |
| 振动 HAL `vibrator-hal` V1.2 | 系统侧 `IVibratorManager/default`, 两颗 Awinic 马达都认, 支持 primitives / 组合 / 同步振动; 第三方 App 触感正常(每颗马达只有 1 个 FF 槽位, 换效果前先释放) |
| 耗电统计修复 `power-profile` V1.0 | power_profile 补 `audio` / `video` 键名, 电池用量里音视频耗电不再是 0 |
| 纯 Wi-Fi `wifi-only` V1.0 | 按纯 Wi-Fi 平板处理(去双卡、telephony 特性、voice/sms/data capable) |
| qesdk seccomp `qesdk-seccomp-fix` V1.0 | 给高通 qesdk 服务的 seccomp 策略补 lseek, 停掉每 5 秒一次的崩溃循环 |
| 手电筒档位 `torch-levels` V1.4 | 下拉面板手电筒亮度滑块真正生效(1~4 档). vendor 相机 HAL 固定两颗各 75 mA、亮着改档还关掉第二颗灯; 常驻小 App 监听 TorchCallback 写最新状态文件, herald 用 inotify 读到后按档位重写 PMIC 闪光灯节点; 关灯 park 使从关闭打开 1/2 档不先闪, LED trigger 自愈, 重启解锁前也生效 |
| 快捷设置自动亮度 `qs-auto-brightness` V1.1 | 快捷设置"自动亮度"开关, 长按进 显示和触控 |
| 热点界面修复 `hotspot-ui-fix` | 补回设置里的 WLAN 热点入口和快捷设置磁贴 |
| 6GHz 修复 `6ghz-fix` V1.1 | Wi-Fi 国家码默认 DE, 驱动才允许 6GHz 热点; 选 6GHz 时把热点固定在 6GHz 频段. 已按 Android 17 的 `service-wifi.jar` 重新对齐 |
| Wi-Fi 打包 `wifi-apex-assemble` | 把各 Wi-Fi 补丁登记的 smali 补丁一次性应用到 `service-wifi.jar`, 回编, 重签 APEX |
| 蓝牙音频修复 `bluetooth-fix` V2.0 | A2DP 和 LE Audio(LC3)走高通 ADSP 硬件卸载, vendor 用原厂音频策略; 耳机详情里可开"LE 音频"; 修 LE Audio 下语音下行约 60 秒延迟. SCO 通话补丁保留 |
| 视频冻结修复 `video-freeze-fix` V1.1 | X 等 app 刷视频不再"声音继续、画面定格" |
| 微距相机修复 `macro-camera-fix` | 补回 `ro.vendor.qti.va_aosp.support=1`, 相机 HAL 才报出 2 号微距相机 |

## 功能

| 模块 | 作用 |
|---|---|
| 联想相机 `zuicamera` V1.5 | 从原厂 ZUI 固件取联想相机装进系统, 并修补它在 GSI 上的问题: 辅助镜头清单, 相机编号核对, 读真实机型, 屏蔽不支持的功能, 不再要求读写存储权限, 拍照前"内存已满"判断改按真实已用量(Android 17 上原判断恒为满, 快门没反应). 本版开启预测性返回 |
| 电池管理 `battery` V1.2 | `heraldctl battery` 和设置 → 电池里的"电池管理"App: 电芯寿命查看, 长期插电保护, 充电上限, 旁路充电, 实时状态. 开启预测性返回 |
| 双击唤醒 `dt2w` V1.1 | 设置 → 显示 → 其他显示控制 → 点按唤醒; 静态 RRO 让开关可见, herald 插件把开关同步到触摸节点; `heraldctl dt2w on/off/status` |
| 隐藏导航栏 `navbar-hide` V1.2 | 隐藏手势提示线和任务栏占位; 入口在 设置 → 系统 → 导航模式 → 三按钮/手势导航的齿轮页底部(Settings 静态 RRO 加的开关); `heraldctl navbar hide/show/status`. 开启预测性返回 |
| 完整 WebView `webview-full` | 用完整的 Android System WebView 换掉 Pixel 镜像里"占位包 + 首次开机解压"的组合, 开机即可用 |
| 32 位应用支持 `tango` | Tango 二进制翻译: 翻译器, sepolicy, 内核模块, 32 位 vendor 库, 32 位 zygote 和开机保险 |
| 人脸解锁 `faceunlock` | 生物识别放行钩子 + FaceUnlock App 和权限 + 设置里的人脸入口; 框架钩子按 Android 17 的 `services.jar` 适配 |
| 自由旋转 `free-rotation` (可选) | 四个方向都能自动旋转, 包括竖屏倒过来; 不受"显示大小"影响 |
| 流畅显示档位 `smooth-display` (可选) | "流畅显示"开 = 不设上限(最高 165Hz), 关 = 120Hz |
| 桌面模式不限应用数 `desktop-unlimited` (可选) | Android 17 桌面模式默认最多同时开 4 个应用, 改成不限 |

> 电池、机型伪装与密钥认证、隐藏导航栏这三个带界面的内置 App, 以及联想相机, 本版都开启了 Android 13+ 的预测性返回(返回手势/动画)。

## 没有打的模块

本版预设只含上面的模块. 默认不打的高级功能(应用隐藏, 字体替换, 应用备份, ush 等)不在本版里; 自用变种 `user-v2.0-cust_bebug` 另含字体/emoji、ush、一键伪装, 不公开分发.
