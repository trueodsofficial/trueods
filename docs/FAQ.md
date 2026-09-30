# TRUEODS 常见问题 / FAQ

[产品主页](../README.zh-CN.md) / [Home](../README.md) · [快速上手 / Quick Start](QUICKSTART.md) · [版本选择 / Editions](EDITIONS.md) · [技术支持 / Support](SUPPORT.md)

**跳转 / Jump to:** [中文](#中文) · [English](#english)

## 中文

### 现在可以在哪里购买？

TRUEODS 正在准备 Fab 首发。正式商品链接会在[产品主页](../README.zh-CN.md)公布；本仓库提供产品介绍、使用文档与支持信息，不是插件下载包。当前没有已宣布的正式发行版，见[更新记录](CHANGELOG.md)。

### ODS 是什么？

**ODS = Omnidirectional Stereo（全向立体）**，用于生成具有正确立体视差的 360° 立体全景。使用支持对应立体布局的播放器与 VR 头显，才能正确观看。它支持转头环视；自由移动观看位置的 6DoF 体积视频是另一种交付形式。

### 8K 是每眼 8192 × 8192 吗？

对 **360 等距柱状、上下双眼**，每眼宽度为 `W`、高度为 `W/2`，双眼整图为 `W × W`。

| 每眼输出宽度 | 每眼图像 | 上下双眼整图 |
|---|---|---|
| 4096 | 4096 × 2048 | 4096 × 4096 |
| 8192 | 8192 × 4096 | 8192 × 8192 |

对于 **180（VR180）**，每眼图像为 `W × W`，上下排列为 `W × 2W`，左右排列为 `2W × W`。设置位置：**True ODS Panoramic** 设置中的 **Format**、**Stereo Layout** 与 **Resolution > Output Width Per Eye**。交付时请同时说明每眼尺寸和双眼整图尺寸。

当 **Resolution Preset** 选的是固定预设（不是 **Custom (manual)**）时，把 **Format** 切到 **180** 会保持相同的每度像素数，宽度随之减半：默认的 **8192 / eye** 在 180 下为每眼 **4096 × 4096**。180 需要每眼 **8192 × 8192** 时，请选择 **16384 / eye**，或先切到 180，再在 **Output Width Per Eye** 中手动输入 **8192**；手动输入后，预设会显示为 **Custom (manual)**。

**Supersample** 调整渲染质量和成本，不改变最终文件尺寸；最终尺寸仍由 **Output Width Per Eye** 决定。

### TrueODS 基础版能续渲吗？为什么还需要 TrueODS Distributed（分布式渲染版）？

**能。基础版和分布式渲染版都有续渲功能**。单机续渲入口位于 **True ODS Panoramic > Output**，选择原输出文件夹后，续渲提示和 **Resume Render** 按钮会出现在文件夹下方。新版会恢复原任务保存的关卡、序列与渲染设置；旧版本输出若没有保存任务设置，则使用当前窗口设置，需要自行核对。保留图像、逐帧记录及 `_metadata` 文件夹。

每次可以选择“重渲并混合中断前的 X 帧”或“直接从缺失帧继续，仅预热”。前者增加渲染时间以平滑接点；后者不重渲之前的帧，但光照、雾或反射可能跳变。只写了 16 位 TIFF、没有 EXR 母版的帧不能混合，前一种不可选。关卡、子关卡或序列在这些帧渲完之后存过盘时，提示会写明，并多出第三种“全部重渲”（**Render every frame again**）：场景确实改过时选它，只是存了盘、没改内容可以不理会。Path Tracing 直接从首个缺失帧继续。按钮会启动整个队列，因此只想续渲单个任务时应让队列只保留该任务。完整说明见[快速上手](QUICKSTART.md)。

分布式渲染版的多机任务仍使用 **TrueODS Multi-Machine** 面板第 4 步的 **Resume Render Job**；默认接点重渲范围由该步骤的 **Handle frames** 控制，详见[多机工作流](EDITIONS.md)。

分布式渲染版的重点是两项不同能力：

- **引擎级时序锁定 / Engine-Level Temporal Lock**：从引擎层保持时间驱动效果跨分段与机器的时间接续。把同一镜头分给多台机器后，各段仍接在同一条时间线上；单纯给机器设置不同帧范围并没有完成这项工作。
- **多机协同渲染 / Multi-Machine Rendering**：通过 **TrueODS Distributed > Multi-Machine Rendering** 自动校验配置、分配帧段、检查收帧完整性，减少逐台手工整理任务。用户仍需部署工程，并在各台机器上启动或续渲。

两版画质相关的共有能力相同。选择依据与完整步骤见[版本与工作流](EDITIONS.md)。

### 有时序锁定，所有水、火、粒子都能在接点完全一样吗？

不保证。时序锁定解决的是**引擎内时间接续**。未烘焙的模拟、随机或外部驱动的效果，以及需要多帧才能稳定的光照和特效，仍可能需要缓存、相应的可重复设置和预热。序列含 Time Dilation 轨道时，只有在其第一个关键帧之前开始的分段才保证时间接续；多机任务请在 **TrueODS Multi-Machine** 面板 **Advanced** 区的 **Frames no part may start at** 中填入该关键帧的帧号（默认留空）。

开始多机任务前，应使用相同的工程内容、引擎与插件版本，最好使用同一级别的显卡（同一系列、相同显存），运行配置检查，并测试实际接点。预热量按场景实测确定。配置检查通过不等于所有模拟状态一致，也不替代接点画面检查。

### “无缝体积雾”与分布式渲染版的时序锁定有什么不同？

**无缝体积雾是两版共有的画面能力**，关注一张全景图不同方向之间的体积雾衔接，让外景大气和内景光束在环视中连续。

**分布式渲染版的时序锁定关注不同时间段之间的接续**，例如前半段和后半段分别由两台机器渲染。空间接缝与跨段时间接续是不同问题；体积效果仍应在自己的场景中检查。

### EXR、half 与有损压缩该怎么选？

**Also Write EXR (HDR master)** 输出线性 HDR 母版，使用 **16 位半精度浮点（half）EXR**，供后期调色与合成。它不同于 16 位整数 TIFF。线性母版适用于默认的 Deferred 渲染器，并需保持 **HDR Tone Chain (recommended)** 开启、**Look > Bake Film Look Into EXR Master** 关闭（均为默认值）；若使用 Path Tracing 渲染，请先用短范围测试确认 EXR 是否满足调色母版的要求。

在 **EXR Compression** 中：**DWAB / DWAA 为有损压缩；ZIP / PIZ 为无损压缩**。需要无损交付或精确像素比较时选 ZIP/PIZ。无损压缩保存的是已编码的 half 数据，不会把它变成 32 位浮点。数据类型和压缩概念可参阅 [OpenEXR 官方技术说明](https://openexr.com/en/latest/TechnicalIntroduction.html)。

EXR 与 PNG 外观不同，通常需要先检查读取软件的线性输入解释和显示变换。不要重复套用伽马或曝光；反馈问题时附上色彩管理设置和输出设置。

### 支持什么引擎、平台和输出模式？

支持 **Unreal Engine 5.7 与 5.8**，仅限 **Windows 64 位编辑器**，配合 **Movie Render Queue、DX12 / SM6、Deferred** 使用。Fab 为每个引擎版本提供单独的插件包，在启动器中选择你的引擎版本安装即可。

360 与 180（VR180）等距柱状双眼输出、上下或左右布局是文档介绍的工作流。**True ODS Panoramic** 设置中还提供 **Path Tracing** 渲染器（**Rendering > Renderer**）、非立体输出（关闭 **Stereo**）和立方面输出（**Format > Projection: Cubemap Faces**）；这些选项尚未经过与 Deferred 双眼流程同等的验证，正式使用前请先做短范围测试。Depth / Normals / Masks 不在本页承诺的功能范围内。Linux、macOS、打包游戏也不是当前目标流程。

使用第三方材质、毛发、水面、粒子或其他特殊效果时，先做短测试。真实视差、遮挡与视角相关反射本来就可能使左右眼不同；两眼完全相同不是立体正确的判断标准。

### 为什么 UE 5.8 项目的 PNG 审片图和视口颜色不一样？

这是当前的已知限制。UE 5.8 在 **Post Process > Film** 中新增了 **Method**（Filmic / Standard ACES）。TrueODS 的 PNG / JPG / TIFF 审片图始终使用引擎默认的 Filmic 色调曲线，所以项目设为 Standard ACES 时，审片图与视口不一致。**EXR 母版不受影响**。需要审片图与视口一致时请使用 Filmic；调色请以 EXR 母版为准。

### 8K 一帧要多久？必须有 32 GB 显存吗？

没有适用于全部场景的固定秒数或显存门槛。分辨率、采样、超采样、场景、特效、硬件与后台负载都会影响速度和资源需求。

先按[快速上手](QUICKSTART.md)渲染单帧，再测试 8K 的普通帧与最重帧。**VRAM Mode** 可调整内存与耗时的取舍；它不保证整机内存或速度的固定比例。公布的测试数据需要结合其硬件、场景类别、设置和测量范围阅读；公布的渲染耗时均在 Unreal Engine 5.7 上、使用默认的 TSR 抗锯齿测得。

### TrueODS 基础版 / 分布式渲染版与 Fab Personal / Professional 是一回事吗？

不是。**TrueODS 基础版 / 分布式渲染版是插件的产品版本**；**Fab Personal / Professional 是 Fab 价格档**。选择分布式渲染版这一产品版本与是否需要 Professional 价格档是两个独立问题。

购买时请依照正式商品页、适用资格与 [Fab 完整许可条款](https://www.fab.com/eula)选择；本 FAQ 不增加或替代平台许可。技术问题请见[支持说明](SUPPORT.md)，订单及退款请使用 [Fab 官方购买帮助](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab)。

## English

### Where can I buy TRUEODS?

TRUEODS is preparing for its Fab launch. The [product home](../README.md) will link to the official listings. This repository contains product information, documentation, and support resources, not the plugin download package. No public release has been announced; see the [changelog](CHANGELOG.md#english).

### What does ODS mean?

**ODS stands for Omnidirectional Stereo**: 360° stereoscopic panoramas with correct parallax. Use a compatible stereo player and VR headset with the matching layout. This supports looking around; freely changing the viewing position in 6DoF volumetric video is a different delivery format.

### Does 8K mean 8192 × 8192 for each eye?

For **360 equirectangular top/bottom stereo**, each eye is `W × W/2`, and the combined image is `W × W`.

| Output width per eye | Per-eye image | Combined top/bottom image |
|---|---|---|
| 4096 | 4096 × 2048 | 4096 × 4096 |
| 8192 | 8192 × 4096 | 8192 × 8192 |

For **180 (VR180)**, each eye is `W × W`: the combined layout is `W × 2W` top/bottom or `2W × W` side by side. Set these with **Format**, **Stereo Layout**, and **Resolution > Output Width Per Eye** in the **True ODS Panoramic** settings. State both per-eye and combined dimensions when delivering files.

When **Resolution Preset** is set to a fixed preset (not **Custom (manual)**), switching **Format** to **180** keeps the same pixels per degree, so the width is halved: the default **8192 / eye** gives **4096 × 4096** per eye at 180. For **8192 × 8192** per eye at 180, choose **16384 / eye**, or switch to 180 first and then type **8192** in **Output Width Per Eye**; the preset then shows **Custom (manual)**.

**Supersample** changes rendering quality and cost, not final file dimensions. **Output Width Per Eye** still determines the output dimensions.

### Can the base TrueODS edition resume? Why would I need TrueODS Distributed?

**Both editions can resume.** For a single machine, choose the original output folder under **True ODS Panoramic > Output**. The notice and **Resume Render** button appear below that folder. The new workflow restores the saved level, sequence, and render settings; older output without saved job settings uses the current window's settings, which you must check. Keep the images, per-frame records, and `_metadata` folder.

Each time, choose to re-render and blend X frames before the gap, or continue at the missing frame with warm-up only. Blending adds render time to smooth the join; direct continuation avoids re-rendering earlier frames but lighting, fog, or reflections may jump. Frames written only as 16-bit TIFF, without the EXR master, cannot be blended, so the first choice is unavailable. If the level, a sublevel or the sequence was saved after the frames were rendered, the notice says so and adds a third choice, **Render every frame again**: choose it when the scene did change; a save without changes can be ignored. Path Tracing continues directly. The button starts the whole queue, so keep only this job if it is the only one you want to resume. See [Quick Start](QUICKSTART.md#english).

TrueODS Distributed multi-machine jobs still use **Resume Render Job** in step 4 of **TrueODS Multi-Machine**. The default overlap is controlled by **Handle frames** in that step; see the [multi-machine workflow](EDITIONS.md#english).

TrueODS Distributed adds two distinct capabilities:

- **Engine-Level Temporal Lock** maintains timing continuity for time-driven effects across segments and machines at the engine level. Parts of one shot stay on the same timeline. Assigning different frame ranges alone does not provide this capability.
- **Multi-Machine Rendering**, available through **TrueODS Distributed > Multi-Machine Rendering**, provides automatic configuration checks, frame-range allocation, and output completeness checks. It reduces repetitive task preparation on each machine. Users deploy the project and start or resume work on each machine.

The two editions share the same core image capabilities. See [Editions and Workflow](EDITIONS.md#english) for a comparison and steps.

### Does Temporal Lock make every water, fire, or particle simulation identical at a join?

Not guaranteed. Temporal Lock addresses **engine-time continuity**. Unbaked simulations, random or externally driven effects, and lighting or effects that need several frames to settle may still require caches, repeatable settings, and warm-up. If the sequence has a Time Dilation track, time continuity is guaranteed only for segments that start before its first key; for a multi-machine job, enter that key's frame number in **Frames no part may start at** under **Advanced** in the **TrueODS Multi-Machine** panel (empty by default).

Use matching project content, engine versions, and plugin versions on all machines, preferably with GPUs from the same family and with the same amount of video memory. Run the configuration check and test the actual joins. Determine warm-up from scene tests. A configuration check does not certify that every simulation state matches or replace visual inspection of joins.

### How does seamless volumetric fog differ from Temporal Lock in TrueODS Distributed?

**Seamless volumetric fog is shared by both editions.** It concerns fog continuity between viewing directions within a panorama, including outdoor atmosphere and indoor light shafts.

**Temporal Lock in TrueODS Distributed concerns continuity between time segments**, such as the first and second halves of a shot rendered on separate machines. Spatial seams and time continuity are different problems. Test volumetric effects in your own scene.

### How should I choose EXR, half, and compression?

**Also Write EXR (HDR master)** writes a linear HDR master as **16-bit half-float EXR** for grading and compositing. Half-float is different from 16-bit integer TIFF. The linear master applies to the default Deferred renderer with **HDR Tone Chain (recommended)** left on and **Look > Bake Film Look Into EXR Master** left off (both are the defaults). If you render with Path Tracing, check the EXR on a short range before you treat it as a grading master.

Under **EXR Compression**, **DWAB / DWAA are lossy; ZIP / PIZ are lossless**. Select ZIP/PIZ for lossless delivery or exact pixel comparisons. Lossless compression preserves the encoded half values; it does not turn them into 32-bit float. See the [official OpenEXR technical introduction](https://openexr.com/en/latest/TechnicalIntroduction.html).

If an EXR looks different from a PNG, first check the viewer's linear input interpretation and display transform. Do not apply gamma or exposure twice. Include colour-management and output settings when reporting a problem.

### Which engine, platforms, and output modes are covered?

Supported engines are **Unreal Engine 5.7 and 5.8**, on the **Windows 64-bit editor** only, with **Movie Render Queue, DX12 / SM6, and Deferred**. Fab provides a separate package for each engine version; pick your engine version in the launcher when you install.

The documented workflow covers 360 and 180 (VR180) equirectangular stereo, top/bottom or side by side. The **True ODS Panoramic** settings also offer the **Path Tracing** renderer (**Rendering > Renderer**), mono output (**Stereo** turned off), and cube-face output (**Format > Projection: Cubemap Faces**). These have not been through the same validation as the Deferred stereo workflow; test them on a short range before production. Depth, normal, and mask passes are not promised here. Linux, macOS, and packaged games are not current workflow targets.

Test third-party materials, hair, water, particles, and other special effects on a short sequence first. Real parallax, occlusion, and view-dependent reflections can legitimately differ between eyes; identical images are not the test for correct stereo.

### Why does the PNG from a UE 5.8 project look different from the viewport?

This is a known limitation. UE 5.8 adds **Method** (Filmic / Standard ACES) under **Post Process > Film**. The TrueODS PNG / JPG / TIFF viewing image always uses the engine's default Filmic tone curve, so in a project set to Standard ACES the viewing image does not match the viewport. **The EXR master is unaffected.** Use Filmic when the viewing image must match the viewport, and grade from the EXR master.

### How long does an 8K frame take? Is 32 GB of VRAM required?

There is no universal time per frame or VRAM threshold. Resolution, sampling, supersampling, scene, effects, hardware, and background load all affect cost.

Follow the [Quick Start](QUICKSTART.md#english), then test ordinary and demanding 8K frames. **VRAM Mode** offers memory/time trade-offs, not fixed ratios for total memory or speed. Read benchmark figures together with their hardware, scene category, settings, and measurement scope. The published render times were measured on Unreal Engine 5.7 with the default TSR anti-aliasing.

### Are the base TrueODS edition and TrueODS Distributed the same as Fab Personal / Professional?

No. **The base TrueODS edition and TrueODS Distributed are product editions of the plugin. Fab Personal / Professional are Fab price tiers.** Choosing TrueODS Distributed and determining which Fab price tier you need are separate decisions.

Use the released listing, eligibility criteria, and [full Fab licence terms](https://www.fab.com/eula) when purchasing. This FAQ does not add to or replace the platform licence. See [Support](SUPPORT.md#english) for technical help and [Fab's official purchasing help](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab) for orders and refunds.
