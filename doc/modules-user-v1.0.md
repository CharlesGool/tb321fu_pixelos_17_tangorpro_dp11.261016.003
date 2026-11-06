---
name: modules-user-v1-0
description: user-v1.0 的补丁模块列表
metadata:
  version: "1.0.0"
  lang: "zh-CN"
---

# user-v1.0 模块列表

本版(内部版本 V1)由 34 个补丁模块构建: 30 个 `required` 模块, 加 4 个 `optional` 模块. 原始清单见刷机包根目录的 `MODULES.txt` 和 `SELECTION.txt`.

构建参数: `VERSION=V1 VERSION_LABEL=user-v1.0 PROFILE=user-v1.4 CAMERA=zui`(预设名 `user-v1.4` 沿用 Android 15 线的"标准选集"叫法, 不是本包的发布号).
设备: 联想拯救者 Y700 三代 TB321FU, 底包 Pixel Tablet(tangorpro)Android 17 QPR3 Beta 3 `DP11.261016.003` GSI(Google 官方镜像经 MysticGSI 工具转成), vendor 和 boot 用联想原厂 ZUI 17.0.12.044.

## 身份与隐藏

| 模块 | 作用 |
|---|---|
| 机型还原 `identity` | `ro.product.*` 改成 TB321FU, 指纹保留 Android 17 的版本段只换机型段; 各分区 build.prop 的 `ro.build.display.id` 都写本包版本号 |
| 关闭调试 `debug-off` | 关掉调试相关开关 |
| 隐藏 GSI 痕迹 `gsi-hide` | `partition.*.verified` 改 1, 去掉虚拟机属性标签 |
| 全局指纹伪装 `global-pif` (可选) | 全局指纹伪装; 开启后"版本号"也显示伪装机型的值 |
| 密钥认证 `attestation-fix` | keymint + inject: 签出 deviceLocked / verifiedBootState=green 的证书链, 附指纹伪装, keybox, 安全补丁同步命令 |
| herald 核心 `herald-core` | 免 root 的控制后端: broker + `heraldctl` + 插件接口. 只提供命令通道, 配对和接入方式, 具体能力由别的补丁接入. 本版为 V1.4 |

## 精简

| 模块 | 作用 |
|---|---|
| 精简旧版 VNDK `old-vndk` | 只留 vendor 要用的 VNDK v34 apex, 删 v31 到 v33 |
| 精简 Pixel 专属应用和数据 `pixel-leftovers` | 删 Play 自动安装配置, "正在播放"歌曲库, Pixel 相机后台服务, 儿童模式 |
| 精简 Pixel 硬件组件 `pixel-hardware` | 删本机没有对应硬件的组件: 底座, 投屏, 零售演示, Tensor 服务, 侧边指纹, UWB, 强光/UV, 基带, NFC 应用 |
| 精简 Google 应用 `google-apps-min` | 只删 8 个: 计算器, 日历, 通讯录, 我的 Pixel, 信息, Kids Space, Play 图书, YouTube Kids; 数字健康等其余应用保留 |
| 精简 GSI 残留 `gsi-traces` | 删 Mystic 的 vendor overlay 和嵌套的 system 残留目录 |

## 启动与 SELinux

| 模块 | 作用 |
|---|---|
| SELinux 修复 `selinux/fix` | 开机即 Enforcing: early-init 就 `setenforce 1`, 内核命令行加 `enforcing=1 selinux=1` |
| 设备连接服务域修复 `dcservice-seapp-fix` | 删 `seapp_contexts` 里指向 `dcservice_app` 的规则, 否则设备连接服务每 10 秒崩一次 |
| 去开机内部问题弹窗 `build-consistency-dialog-fix` | 去掉开机"设备内部出现了问题"弹窗. Android 17 上改 `services.jar` 里弹窗的调用方, 不动 `framework.jar` |
| 清过期预编译 `stale-boot-image` | 清掉因改 jar 而过期的预编译产物 |
| 内核编译者字符串 `kernel-builder-string` | 修改内核编译者字符串 |
| boot 打包重签 `boot-assemble` | 汇总改动, 重签 boot |

## 硬件修复

| 模块 | 作用 |
|---|---|
| 自动亮度修复 `auto-brightness-fix` | 补回设置里的自适应亮度开关, 换上联想原厂的自动亮度曲线和反应速度, 手动最低档还原原厂 |
| 自适应刷新率修复 `adaptive-refresh-fix` | 补回原厂 SurfaceFlinger 的空闲/触摸定时器和内容检测(静止降档, 触摸升档, 视频按帧率) |
| 屏幕方向修复 `rotation-fix` | 只能横屏的应用画面不再倒过来; 去掉 Pixel Tablet 底座的方向设置 |
| 开机动画适配 `bootanim-portrait` | 把 Pixel Tablet 横屏尺寸(2560x1600)的开机动画转成本机面板的 1600x2560, 平板横拿时画面正立 |
| 热点界面修复 `hotspot-ui-fix` | 补回设置里的 WLAN 热点入口和快捷设置磁贴 |
| 6GHz 修复 `6ghz-fix` | Wi-Fi 国家码默认 DE, 驱动才允许 6GHz 热点; 设置里选 6GHz 时把热点固定在 6GHz 频段. 已按 Android 17 的 `service-wifi.jar` 重新对齐 |
| Wi-Fi 打包 `wifi-apex-assemble` | 把各 Wi-Fi 补丁登记的 smali 补丁一次性应用到 `service-wifi.jar`, 回编, 重签 APEX |
| 蓝牙音频修复 `bluetooth-fix` | 关 A2DP 硬件 offload 和 LE Audio, 改走 vendor 软件编码; 修耳机没声音和通话听不到 |
| 微距相机修复 `macro-camera-fix` | 补回 `ro.vendor.qti.va_aosp.support=1`, 相机 HAL 才报出 2 号微距相机 |

## 功能

| 模块 | 作用 |
|---|---|
| 联想相机 `zuicamera` | 从原厂 ZUI 固件取联想相机装进系统, 并修补它在 GSI 上的问题: 辅助镜头清单, 相机编号核对, 读真实机型, 屏蔽不支持的功能, 不再要求读写存储权限, 拍照前"内存已满"判断改按真实已用量(Android 17 上原判断恒为满, 快门没反应) |
| 电池管理 `battery` | `heraldctl battery`: 电芯寿命查看, 长期插电保护, 充电上限, 旁路充电 |
| 32 位应用支持 `tango` | Tango 二进制翻译: 翻译器, sepolicy, 内核模块, 32 位 vendor 库, 32 位 zygote 和开机保险; Android 17 上去掉了已不存在的 `boringssl_self_test64` 依赖 |
| 人脸解锁 `faceunlock` | 生物识别放行钩子 + FaceUnlock App 和权限 + 设置里的人脸入口; 框架钩子已按 Android 17 的 `services.jar` 重新适配 |
| 自由旋转 `free-rotation` (可选) | 四个方向都能自动旋转, 包括竖屏倒过来; 不受"显示大小"影响 |
| 流畅显示档位 `smooth-display` (可选) | "流畅显示"开 = 不设上限(最高 165Hz), 关 = 120Hz |
| 桌面模式不限应用数 `desktop-unlimited` (可选) | Android 17 桌面模式默认最多同时开 4 个应用, 改成不限(`persist.wm.debug.desktop_max_task_limit=0`) |
| 隐藏导航栏 `navbar-hide` V1.1 | 隐藏手势提示线和任务栏占位, 带 `heraldctl navbar` 开关(`hide` / `show` / `status`). 启动器任务栏的收起高度取 1dp, 底部会留约 3 像素占位 |

## 没有打的模块

本版预设只含上面的模块. 默认不打的高级功能(应用隐藏, 字体替换, 应用备份等)不在本版里.
