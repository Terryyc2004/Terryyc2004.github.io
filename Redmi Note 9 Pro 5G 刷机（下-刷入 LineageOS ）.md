# Redmi Note 9 Pro 5G 刷机（下-刷入 LineageOS ）

内容更新：2026.2.23
我刷第三方 ROM - LineageOS 是主要跟着 ROM 对应指导走的，可以说相当顺利。记录下一些别的注意点。
[toc]

---

## 根据指引刷入 LineageOS

我的设备 [Redmi Note 9 Pro 5G 的 LineageOS 指导](https://wiki.lineageos.org/devices/gauguin/install/variant3/)十分清晰，大概步骤概括一下：

1. 下镜像、解 BL 、格式化
2. 升系统到要求的底包版本
3. 进 bootloader 刷 recovery
4. 进 recovery 再格式化
5. 在 recovery 里用更新系统，通过 adb 侧载包
6. 启动前装插件/谷歌框架等（可选）
7. 重启，进系统，结束

## 捣鼓过程中遇到的注意点

1. 问题：我已经刷好 LineageOS ，recovery 也是 LineageOS 对应的，但我想用 TWRP ，我进 fastboot 重刷入 TWRP 到 recovery 后，我重启进入不了 TWRP ， TWRP 在加载页面结束之后就返回启动系统了，系统目前还是正常的（不是被 OS 系统恢复了recovery,也不是我 TWRP 与设备不兼容）。

   > 结果：
   >
   > - 查询一通后 AI 告诉我是因为 _LineageOS_ 修改了的设备树，而 _TWRP_ 是基于原厂设备树构建的，两者不兼容。
   > - 尝试 `fastboot boot <你的twrp文件名>.img` 临时启动也是同样问题，判断 _TWRP_ 与 _LineageOS_ 有冲突。
   > - 去找 ROM 提供的相关文件也没找到定制后对应的 _TWRP_ 镜像。
   > - 最终**放弃** _TWRP_ ，用了和 LineageOS 对应的 recovery 。

2. 问题：用不了 TWRP 的话怎么装 Magisk ？
   > 结果：
   >
   > - LineageOS 给了单独的 boot.img ，下好它。
   > - 直接安装 Magisk 软件后，**_主页_** -> **Magisk** 边的 **_安装_** -> **_选择并修补一个文件_**，把 boot.img 修补了并拷出来。
   > - 重启进 fastboot 用电脑把修补好的 boot.img 用 `fastboot flash boot <修补后的 boot >.img` 覆盖刷了一遍。
   > - 重启后装好的 Magisk 就能用了。
3. 问题：类原生没有谷歌套件（看别人说也没必要装），大部分安卓软件又都只能谷歌商店下咋办？
   > 结果：
   >
   > - 用[F-Droid](https://f-droid.org/zh_Hans/packages/org.fdroid.fdroid/)（“F-Droid 是 Android 平台自由软件应用的可安装目录。F-Droid 客户端应用使得浏览、安装和跟踪设备上的应用更新变得轻而易举。”一个自由软件分发平台，下载需要科学上网，用时可以自行添加存储卡/用镜像）[AuroraStore](https://auroraoss.com/files/AuroraStore/Release)（“找到你喜欢的所有应用 Aurora 商店允许你从官方 Google Play 商店搜索和下载应用。”一个替代版的谷歌商店，用时要科学上网）
4. 问题：想更方便的管理权限？
   > 结果：
   >
   > - 可以试试 [Shizuku](https://github.com/RikkaApps/Shizuku/releases) 来为中介，接收应用请求，发送到系统服务器，并返回结果。
5. 问题：找不到好模块/不知道去哪能方便下模块？
   > 结果：
   >
   > - 可以试试 [LSPosed](https://github.com/LSPosed/LSPosed) （针对应用的模块框架）
   > - 此外还有 [sence](https://omarea.com) 工具箱（内部包括一个 Magisk 模块仓库；免费开源的[旧版 sence](https://github.com/helloklf/vtools/releases) ）
   > - 这两个配合 Magisk 和 root 都能提供更多模块仓库、控制和功能
6. 问题：LSPosed 长期不维护，不支持 Android 新版本？
   > 结果：
   >
   > - Magisk 设置里开 **_Zygisk_**
   > - 去新的[复兴项目](https://github.com/JingMatrix/LSPosed)下模块并安装
7. 问题：好多墓碑还要单买好贵
   > 结果：
   >
   > - 单买了个 Sence ，全丢在里边的应用偏见里，然后从那里面添加个快捷方式到桌面（使用时自动切换冻结->解冻）
   > - 配合调节里的针对性设置使用，感觉基本满足需求
