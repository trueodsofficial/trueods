# TRUEODS 更新记录 / Changelog

[产品主页](../README.zh-CN.md) / [Home](../README.md) · [快速上手 / Quick Start](QUICKSTART.md) · [版本选择 / Editions](EDITIONS.md) · [技术支持 / Support](SUPPORT.md)

**跳转 / Jump to:** [中文](#中文) · [English](#english)

## 中文

### 未发布 — Fab 首发准备中

**截至 2026-09-26，尚未宣布正式公开发行版**。本页记录文档更新；开发构建、内部测试与页面准备不代表插件已在 Fab 上架。正式发布时将在这里列出实际版本号、发布日期、支持范围和获取链接。

#### 2026-09-26：支持引擎、DLAA 与售后群说明

- 支持的引擎更新为 **Unreal Engine 5.7 与 5.8**（仅 Windows 64 位）。Fab 为每个引擎版本提供单独的插件包，在启动器中选择你的引擎版本即可。
- 补充 DLAA 说明（见[常见问题](FAQ.md)）：可选的 DLAA 需要与引擎版本对应、并经 TrueODS 验证的 NVIDIA DLSS 插件，目前为 UE 5.7 上的 DLSS 8.4.x。UE 5.8 暂无经验证的 DLSS 插件，DLAA 在面板中显示为灰色，TrueODS 使用 TSR。其余功能都不需要 DLSS。
- 注明公布的渲染耗时均在 Unreal Engine 5.7 上、开启 DLAA 测得。
- 常见问题补充一项已知限制：UE 5.8 项目的 Film > Method 设为 Standard ACES 时，PNG / JPG / TIFF 审片图与视口不一致；EXR 母版不受影响。
- 售后群：邮件核实购买后，我们回复 Telegram 入群邀请链接。
- 插件包根目录附有第三方软件声明 `THIRD_PARTY_NOTICES.txt`。

#### 2026-09-25：公开文档整理

- 更新[快速上手](QUICKSTART.md)：从 4K 单帧、短序列到 8K 双眼测试，补充输出尺寸、EXR 压缩与续渲检查。
- 新增[版本与工作流](EDITIONS.md)：明确 Standard 与 Pro 的共有画面能力，以及 Pro 的引擎级时序锁定、多机协同渲染两项独立能力。
- 更新[常见问题](FAQ.md)：说明两版均可续渲，区分空间接缝、跨段时间接续，以及模拟和需要多帧才能稳定的光照与效果各自的检查需求。
- 补充性能、显存与预热的评估方法，明确时间接续与模拟状态的适用边界。
- 对齐中英文说明，并区分 TRUEODS 产品版本与 Fab 价格档。
- Pro：**TrueODS Pro** 菜单中的入口改名为 **Multi-Machine Rendering**，打开的面板标签页为 **TrueODS Multi-Machine**。
- 更新续渲说明（两版均适用）：续渲保留已完整写出的帧并补渲缺失帧，同时把第一个缺失帧之前的若干帧重新渲染，与已写出的原帧平滑叠化，使中断处画面不跳变；这些帧需要额外的渲染时间，具体帧数会在续渲提示中写明。
- 截图已按当前界面更新。

#### 发布范围说明

支持的引擎为 **Unreal Engine 5.7 与 5.8**，主要工作流为 **Windows 64 位编辑器、Movie Render Queue、DX12 / SM6、Deferred**。完整的交付内容、验证覆盖和已知限制将在正式版本发布时确认。

文档中的功能介绍不等于所有硬件、场景、第三方插件或实验选项均已完成验证。未发布的实验功能不列为正式交付内容。正式发行前的技术问题可通过[支持说明](SUPPORT.md)联系。

## English

### Unreleased — Preparing for the Fab launch

**As of 2026-09-26, no public release has been announced.** This page records documentation updates. Development builds, internal tests, and page preparation do not mean the plugin is available on Fab. An actual release entry will include its version, release date, support scope, and official download or purchase link.

#### 2026-09-26: Supported engines, DLAA and support group

- Supported engines are now **Unreal Engine 5.7 and 5.8** (Windows 64-bit only). Fab provides a separate package for each engine version; pick your engine version in the launcher.
- Added DLAA notes (see the [FAQ](FAQ.md#english)): the optional DLAA needs NVIDIA's DLSS plugin built for the same engine version and verified by TrueODS, which today is DLSS 8.4.x on UE 5.7. No DLSS plugin is verified for UE 5.8 yet, so on 5.8 DLAA is greyed out in the panel and TrueODS uses TSR. Everything else works without DLSS.
- Stated that the published render times were measured on Unreal Engine 5.7 with DLAA.
- Added a known limitation to the FAQ: in a UE 5.8 project whose Film > Method is Standard ACES, the PNG / JPG / TIFF viewing image does not match the viewport; the EXR master is unaffected.
- Support group: after we verify your purchase by email, we reply with a Telegram group invitation link.
- The plugin package includes third-party software notices in `THIRD_PARTY_NOTICES.txt` at its root.

#### 2026-09-25: Public documentation refresh

- Updated the [Quick Start](QUICKSTART.md#english): 4K single-frame and short-sequence checks before 8K stereo tests, including output dimensions, EXR compression, and resume checks.
- Added [Editions and Workflow](EDITIONS.md#english): shared image capabilities and the two distinct Pro capabilities, Engine-Level Temporal Lock and Multi-Machine Rendering.
- Updated the [FAQ](FAQ.md#english): both editions can resume; spatial seams, time continuity across segments, and checks for simulations and for lighting or effects that need several frames to settle are separate topics.
- Clarified how to evaluate performance, memory, and warm-up, and explained the distinction between time continuity and simulation state.
- Aligned the Chinese and English explanations and separated TRUEODS product editions from Fab price tiers.
- Pro: the entry in the **TrueODS Pro** menu is renamed **Multi-Machine Rendering**, and the panel it opens has the tab **TrueODS Multi-Machine**.
- Updated the resume notes (both editions): a resume keeps the frames that were written completely and renders the missing ones. It also renders a number of frames before the first missing frame again and blends them smoothly into the frames already written, so the picture does not jump where the render stopped. These frames take extra render time; the resume notice states how many.
- Updated the screenshots to match the current interface.

#### Release scope

Supported engines are **Unreal Engine 5.7 and 5.8**; the main workflow is **the Windows 64-bit editor, Movie Render Queue, DX12 / SM6, and Deferred**. The complete delivered content, validation coverage, and known limitations will be confirmed with the release.

Feature descriptions do not mean every hardware configuration, scene, third-party plugin, or experimental option has been validated. Unreleased experiments are not listed as delivered features. For technical questions before release, see [Support](SUPPORT.md#english).
