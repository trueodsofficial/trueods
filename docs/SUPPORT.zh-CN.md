# TRUEODS 技术支持

[English](SUPPORT.md) · **简体中文**

[产品主页](../README.zh-CN.md) · [快速上手](QUICKSTART.zh-CN.md) · [版本选择](EDITIONS.zh-CN.md) · [常见问题](FAQ.zh-CN.md)

官方主页：[TRUEODS](https://github.com/trueodsofficial)  
支持邮箱：[trueodssupport@gmail.com](mailto:trueodssupport@gmail.com)

售后群：[Telegram 入群方式](COMMUNITY.zh-CN.md)

### 如何获得帮助

技术支持请发邮件到 [trueodssupport@gmail.com](mailto:trueodssupport@gmail.com)，主题使用 **[TrueODS Support] 简短问题描述**。支持中文和英文反馈；私密材料请通过邮箱发送，不入群也可获得邮件支持。

如需加入 Telegram 售后群，请将 **Fab 订单号、所购产品名和购买凭证**发到同一邮箱，主题使用 **[TrueODS Telegram] 入群申请**。核实后，我们会回复邮件发送入群邀请链接。详细步骤与群内注意事项见[入群说明](COMMUNITY.zh-CN.md)。

请先用与你的 UE 版本对应的插件包，并在一个新的输出文件夹里复现短范围问题。支持的引擎为 Unreal Engine 5.7 与 5.8，配合 Win64 编辑器与 MRQ；Linux、macOS、打包游戏，以及你自行搭建的命令行渲染（例如由渲染队列管理软件直接启动 MRQ），不是当前支持目标；插件自带的 **Render With Editor Closed** 与 TrueODS Distributed（分布式渲染版）的 **Start Rendering On This Machine** 属于支持范围。Path Tracing、第三方插件及特殊工作流请先用短测试确认，不能由编译成功推定画面兼容。

我们会根据可复现性、影响范围和资料完整性处理问题；本页没有承诺 24 小时值守、固定响应时限或为所有第三方插件提供兼容修复。

### 先做这几项检查

1. 确认只安装了一份 TrueODS，记录实际加载版本，不要混用旧 DLL。
2. 记录首次失败帧并保留日志。尝试 4K 单帧或短连续范围，输出到新空目录；不要覆盖唯一的证据。
3. 曝光问题：记录是否点击 **Use Scene Exposure**、相机/PPV 的曝光方式，以及实际选用的曝光值。
4. 帧间或分段问题：记录起始帧、是否重启进程、是否续渲已有输出、模拟是否烘焙。直接从后续帧冷启动，不等同于连续渲染。
5. 性能/内存问题：记录 Output Width Per Eye、Supersample、Samples Per Pane、VRAM Mode、最慢帧的场景事件和峰值显存；不要只报一个平均秒数。

### 邮件反馈模板

复制后填写；暂时不知道的项写“不知道”，无需为填表暴露敏感信息。

```text
问题标题：
TrueODS Version / VersionName：
产品版本（TrueODS 基础版 / TrueODS Distributed 分布式渲染版）：
UE 完整版本（例如 5.7.4），Launcher / 源码引擎：
Windows 版本：
GPU 型号 / 显存 / 驱动版本：
系统内存：

Renderer：
Format / Projection / Stereo Layout：
Output Width Per Eye / Supersample：
Samples Per Pane / Anti-Aliasing Method / VRAM Mode：
Image Format / 是否开启 Also Write EXR (HDR master) / EXR Compression：
曝光方式 / 是否使用 Use Scene Exposure：
测试帧范围 / 整段还是指定范围：
首次运行 / 续渲 / 重启后接续 / 多机协同渲染：
相关模拟是否烘焙：
相关第三方插件及版本（如有）：

重现步骤：
1.
2.
3.
预期结果：
实际结果：
首次失败帧 / 发生频率：
一行已脱敏的关键错误（如有）：
可提供的私密附件：日志 / 成帧 / 最小复现场景
你希望使用的回复语言：
```

### 隐私和附件

- **不要在公开 issue、售后群或公开网盘中发送完整 Unreal 日志、渲染诊断附件、订单、客户素材或工程**。一行报错也可能带路径，分享前应先检查。
- 优先发送问题描述与设置。确需日志/图像时，私下邮件发送脱敏的相关范围；大文件先询问收件方式，不要把完整工程作为第一封邮件附件。
- 删除密码、令牌、私人目录、客户名及不愿透露的资产名。订单核对只需必要信息，不要发送 Epic 密码、验证码、支付卡号或完整付款信息。
- 只分享你有权提供的素材和最小复现工程。不要因求助而违反客户 NDA 或第三方资产许可。
- 联系邮箱是第三方邮件服务；只发送你允许通过该渠道处理的资料。需要保密协议或特殊传输方式时，请在发材料前说明。

### 重装与退款问题

重装前先保存工作、关闭编辑器，只移除明确的那份插件。**无 Source 的二进制包不要单独删除 Binaries**；从原交付包重新安装。源码构建仅在需要重建时清理该插件生成的 Binaries/Intermediate。不要删除工程的 Content、Config 或 Saved；共享引擎缓存清理不是常规第一步。

技术问题由上述邮箱受理；Fab 订单、账户和退款流程以 [Fab 官方帮助](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab)为准。联系我们不会更改平台退款政策或你的法定权利。
