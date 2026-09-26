# TRUEODS 常见问题 / FAQ

[产品主页](../README.zh-CN.md) / [Home](../README.md) · [快速上手 / Quick Start](QUICKSTART.md) · [版本选择 / Editions](EDITIONS.md) · [技术支持 / Support](SUPPORT.md)

**跳转 / Jump to:** [中文](#中文) · [English](#english)

## 中文

### 现在可以在哪里购买？

TRUEODS 正在准备 Fab 首发。正式商品链接会在[产品主页](../README.zh-CN.md)公布；本仓库提供产品介绍、使用文档与支持信息，不是插件下载包。当前没有已宣布的正式发行版，见[更新记录](CHANGELOG.md)。

### ODS 是什么？

**ODS = Omnidirectional Stereo（全向立体）**，用于生成具有正确双眼视差的 360° 立体全景。使用支持对应立体布局的播放器与 VR 头显，才能正确观看。它支持转头环视；自由移动观看位置的 6DoF 体积视频是另一种交付形式。

### 8K 是每眼 8192 × 8192 吗？

对 **360 等距柱状、上下双眼**，每眼宽度为 `W`、高度为 `W/2`，双眼整图为 `W × W`。

| 每眼输出宽度 | 每眼图像 | 上下双眼整图 |
|---|---|---|
| 4096 | 4096 × 2048 | 4096 × 4096 |
| 8192 | 8192 × 4096 | 8192 × 8192 |

对于 **180（VR180）**，每眼图像为 `W × W`，上下排列为 `W × 2W`，左右排列为 `2W × W`。设置位置：**True ODS Panoramic** 设置中的 **Format**、**Stereo Layout** 与 **Resolution > Output Width Per Eye**。交付时请同时说明每眼尺寸和双眼整图尺寸。

当 **Resolution Preset** 选的是固定预设（不是 **Custom (manual)**）时，把 **Format** 切到 **180** 会保持相同的每度像素数，宽度随之减半：默认的 **8192 / eye** 在 180 下为每眼 **4096 × 4096**。180 需要每眼 **8192 × 8192** 时，请选择 **16384 / eye**，或先切到 180，再在 **Output Width Per Eye** 中手动输入 **8192**；手动输入后，预设会显示为 **Custom (manual)**。

**Supersample** 调整渲染质量和成本，不改变最终文件尺寸；最终尺寸仍由 **Output Width Per Eye** 决定。

### Standard 能续渲吗？为什么还需要 Pro？

**能。Standard 和 Pro 都有续渲功能**。单机时，在 MRQ 中打开同一任务的 **True ODS Panoramic** 设置，最上方（Setup 区）的 **Resume Render** 按钮只在输出目录中有未完成的渲染时出现，按钮上方的提示会显示已写出的帧数和第一个缺失帧。该按钮会重新渲染整个队列，请让队列里只留这一个任务。Pro 多机任务使用 **TrueODS Multi-Machine** 面板第 4 步的 **Resume Render Job**。

续渲会保留已写出且记录完整的帧，补渲缺失帧；同时把第一个缺失帧之前的若干帧重新渲染，并与磁盘上的原帧逐帧平滑混合后写回，使中断处不跳变。单机续渲的重渲帧数最多等于 **Warm Up Frames**（留 0 时为 32；需开启 **Warm Up Before First Frame**，默认开启，关闭该项或使用 Path Tracing 时不重渲、不混合）；Pro 多机任务在默认设置下等于面板第 4 步显示的 **Handle frames**（默认 32）。续渲提示会写出第一个缺失帧之前需要重渲的帧数与原因（若缺失帧夹在已写出的帧中间，缺帧之后的若干帧也会同样重渲并混合，不计入该帧数），请为这些帧预留额外的渲染时间。混合前的原帧保存在输出目录的 `_resume_originals` 文件夹中。改变内容或设置后，请使用新的输出目录。

Pro 的重点是两项不同能力：

- **引擎级时序锁定 / Engine-Level Temporal Lock**：从引擎层保持时间驱动效果跨分段、续渲与机器的时间接续。把同一镜头分给多台机器后，各段仍接在同一条时间线上；单纯给机器设置不同帧范围并没有完成这项工作。
- **多机协同渲染 / Multi-Machine Rendering**：通过 **TrueODS Pro > Multi-Machine Rendering** 自动校验配置、分配帧段、检查收帧完整性，减少逐台手工整理任务。用户仍需部署工程，并在各台机器上启动或续渲。

两版画质相关的共有能力相同。选择依据与完整步骤见[版本与工作流](EDITIONS.md)。

### 有时序锁定，所有水、火、粒子都能在接点完全一样吗？

不保证。时序锁定解决的是**引擎内时间接续**。未烘焙的模拟、随机或外部驱动的效果，以及需要多帧才能稳定的光照和特效，仍可能需要缓存、相应的可重复设置和预热。序列含 Time Dilation 轨道时，只有在其第一个关键帧之前开始的分段和续渲才保证时间接续；多机任务请在 **TrueODS Multi-Machine** 面板 **Advanced** 区的 **Frames no part may start at** 中填入该关键帧的帧号（默认留空）。

开始多机任务前，应使用相同的工程内容、引擎与插件版本，最好使用同一级别的显卡（同一系列、相同显存），运行配置检查，并测试实际接点。预热量按场景实测确定。配置检查通过不等于所有模拟状态一致，也不替代接点画面检查。

### “无缝体积雾”与 Pro 的时序锁定有什么不同？

**无缝体积雾是两版共有的画面能力**，关注一张全景图不同方向之间的体积雾衔接，让外景大气和内景光束在环视中连续。

**Pro 时序锁定关注不同时间段之间的接续**，例如前半段和后半段分别由两台机器渲染。空间接缝与跨段时间接续是不同问题；体积效果仍应在自己的场景中检查。

### EXR、half 与有损压缩该怎么选？

**Also Write EXR (HDR master)** 输出线性 HDR 母版，使用 **16 位半精度浮点（half）EXR**，供后期调色与合成。它不同于 16 位整数 TIFF。线性母版适用于默认的 Deferred 渲染器，并需保持 **HDR Tone Chain (recommended)** 开启、**Look > Bake Film Look Into EXR Master** 关闭（均为默认值）；若使用 Path Tracing 渲染，请先用短范围测试确认 EXR 是否满足调色母版的要求。

在 **EXR Compression** 中：**DWAB / DWAA 为有损压缩；ZIP / PIZ 为无损压缩**。需要无损交付或精确像素比较时选 ZIP/PIZ。无损压缩保存的是已编码的 half 数据，不会把它变成 32 位浮点。数据类型和压缩概念可参阅 [OpenEXR 官方技术说明](https://openexr.com/en/latest/TechnicalIntroduction.html)。

EXR 与 PNG 外观不同，通常需要先检查读取软件的线性输入解释和显示变换。不要重复套用伽马或曝光；反馈问题时附上色彩管理设置和输出设置。

### 支持什么引擎、平台和输出模式？

支持 **Unreal Engine 5.7 与 5.8**，仅限 **Windows 64 位编辑器**，配合 **Movie Render Queue、DX12 / SM6、Deferred** 使用。Fab 为每个引擎版本提供单独的插件包，在启动器中选择你的引擎版本安装即可。

360 与 180（VR180）等距柱状双眼输出、上下或左右布局是文档介绍的工作流。**True ODS Panoramic** 设置中还提供 **Path Tracing** 渲染器（**Rendering > Renderer**）、非立体输出（关闭 **Stereo**）和立方面输出（**Format > Projection: Cubemap Faces**）；这些选项尚未经过与 Deferred 双眼流程同等的验证，正式使用前请先做短范围测试。Depth / Normals / Masks 不在本页承诺的功能范围内。Linux、macOS、打包游戏也不是当前目标流程。

使用第三方材质、毛发、水面、粒子或其他特殊效果时，先做短测试。真实视差、遮挡与视角相关反射本来就可能使左右眼不同；两眼完全相同不是立体正确的判断标准。

### DLAA 需要什么？在 UE 5.8 上能用吗？

**DLAA 是可选的抗锯齿方式**，在 **True ODS Panoramic > Anti-Aliasing > Anti-Aliasing Method** 中选择。它需要 NVIDIA RTX 显卡，以及与引擎版本对应、并经 TrueODS 验证的 NVIDIA DLSS 插件：目前为 UE 5.7 上的 DLSS 8.4.x。UE 5.8 暂无经验证的 DLSS 插件，因此在 5.8 上 DLAA 显示为灰色，TrueODS 使用 TSR，即默认的 **Standard (recommended)**。其余功能都不需要 DLSS。该选项下方的 **DLAA Status** 一行会说明本机的情况。

### 为什么 UE 5.8 项目的 PNG 审片图和视口颜色不一样？

这是当前的已知限制。UE 5.8 在 **Post Process > Film** 中新增了 **Method**（Filmic / Standard ACES）。TrueODS 的 PNG / JPG / TIFF 审片图始终使用引擎默认的 Filmic 色调曲线，所以项目设为 Standard ACES 时，审片图与视口不一致。**EXR 母版不受影响**。需要审片图与视口一致时请使用 Filmic；调色请以 EXR 母版为准。

### 8K 一帧要多久？必须有 32 GB 显存吗？

没有适用于全部场景的固定秒数或显存门槛。分辨率、采样、超采样、场景、特效、硬件与后台负载都会影响速度和资源需求。

先按[快速上手](QUICKSTART.md)渲染单帧，再测试 8K 的普通帧与最重帧。**VRAM Mode** 可调整内存与耗时的取舍；它不保证整机内存或速度的固定比例。公布的测试数据需要结合其硬件、场景类别、设置和测量范围阅读；公布的渲染耗时均在 Unreal Engine 5.7 上、开启 DLAA 测得。

### TRUEODS Standard / Pro 与 Fab Personal / Professional 是一回事吗？

不是。**TRUEODS Standard / Pro 是插件的产品版本**；**Fab Personal / Professional 是 Fab 价格档**。选择 Pro 产品版本与是否需要 Professional 价格档是两个独立问题。

购买时请依照正式商品页、适用资格与 [Fab 完整许可条款](https://www.fab.com/eula)选择；本 FAQ 不增加或替代平台许可。技术问题请见[支持说明](SUPPORT.md)，订单及退款请使用 [Fab 官方购买帮助](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab)。

## English

### Where can I buy TRUEODS?

TRUEODS is preparing for its Fab launch. The [product home](../README.md) will link to the official listings. This repository contains product information, documentation, and support resources, not the plugin download package. No public release has been announced; see the [changelog](CHANGELOG.md#english).

### What does ODS mean?

**ODS stands for Omnidirectional Stereo**: 360° stereo panoramas with correct binocular parallax. Use a compatible stereo player and VR headset with the matching layout. This supports looking around; freely changing the viewing position in 6DoF volumetric video is a different delivery format.

### Does 8K mean 8192 × 8192 for each eye?

For **360 equirectangular top/bottom stereo**, each eye is `W × W/2`, and the combined image is `W × W`.

| Output width per eye | Per-eye image | Combined top/bottom image |
|---|---|---|
| 4096 | 4096 × 2048 | 4096 × 4096 |
| 8192 | 8192 × 4096 | 8192 × 8192 |

For **180 (VR180)**, each eye is `W × W`: the combined layout is `W × 2W` top/bottom or `2W × W` side by side. Set these with **Format**, **Stereo Layout**, and **Resolution > Output Width Per Eye** in the **True ODS Panoramic** settings. State both per-eye and combined dimensions when delivering files.

When **Resolution Preset** is set to a fixed preset (not **Custom (manual)**), switching **Format** to **180** keeps the same pixels per degree, so the width is halved: the default **8192 / eye** gives **4096 × 4096** per eye at 180. For **8192 × 8192** per eye at 180, choose **16384 / eye**, or switch to 180 first and then type **8192** in **Output Width Per Eye**; the preset then shows **Custom (manual)**.

**Supersample** changes rendering quality and cost, not final file dimensions. **Output Width Per Eye** still determines the output dimensions.

### Can Standard resume? Why would I need Pro?

**Both Standard and Pro can resume.** On a single machine, open the same job's **True ODS Panoramic** settings in MRQ: the **Resume Render** button at the top (Setup section) appears only when the output folder holds an unfinished render, and the notice above it shows how many frames were written and the first missing frame. The button renders the whole queue again, so keep only this job in the queue. A Pro multi-machine job resumes with **Resume Render Job** in step 4 of the **TrueODS Multi-Machine** panel.

A resume keeps every frame already written with a complete record and renders the missing frames. It also renders the frames just before the first missing frame again and blends them, frame by frame, into the frames on disk, so the render does not jump where it stopped. On a single machine that is up to as many frames as **Warm Up Frames** (32 when left at 0; it needs **Warm Up Before First Frame**, on by default, and does not happen with Path Tracing); for a Pro multi-machine job with default settings it is the **Handle frames** shown in step 4 of the panel (32 by default). The resume notice states the number of frames before the first missing frame and the reason (if missing frames lie between frames already written, the frames after the gap are also rendered again and blended, and are not counted there); allow extra render time for these frames. The frames as they were before blending are kept in the `_resume_originals` folder in the output folder. Use a new output folder after changing content or settings.

Pro adds two distinct capabilities:

- **Engine-Level Temporal Lock** maintains the time continuity of time-driven effects across segments, resumes, and machines at the engine level. Parts of one shot stay on the same timeline. Assigning different frame ranges alone does not provide this capability.
- **Multi-Machine Rendering**, available through **TrueODS Pro > Multi-Machine Rendering**, provides automatic configuration checks, frame-range allocation, and output completeness checks. It reduces repetitive task preparation on each machine. Users deploy the project and start or resume work on each machine.

The two editions share the same core image capabilities. See [Editions and Workflow](EDITIONS.md#english) for a comparison and steps.

### Does Temporal Lock make every water, fire, or particle simulation identical at a join?

Not guaranteed. Temporal Lock addresses **engine-time continuity**. Unbaked simulations, random or externally driven effects, and lighting or effects that need several frames to settle may still require caches, repeatable settings, and warm-up. If the sequence has a Time Dilation track, time continuity is guaranteed only for segments and resumes that start before its first key; for a multi-machine job, enter that key's frame number in **Frames no part may start at** under **Advanced** in the **TrueODS Multi-Machine** panel (empty by default).

Use matching project content, engine, and plugin versions on the machines, preferably with the same GPU family and memory, run the configuration check, and test actual joins. Determine warm-up from scene tests. A configuration check does not certify that every simulation state matches or replace visual inspection of joins.

### How does seamless volumetric fog differ from Pro Temporal Lock?

**Seamless volumetric fog is shared by both editions.** It concerns fog continuity between viewing directions within a panorama, including outdoor atmosphere and indoor light shafts.

**Pro Temporal Lock concerns continuity between time segments**, such as the first and second halves of a shot rendered on separate machines. Spatial seams and time continuity are different problems. Test volumetric effects in your own scene.

### How should I choose EXR, half, and compression?

**Also Write EXR (HDR master)** writes a linear HDR master as **16-bit half-float EXR** for grading and compositing. Half-float is different from 16-bit integer TIFF. The linear master applies to the default Deferred renderer with **HDR Tone Chain (recommended)** left on and **Look > Bake Film Look Into EXR Master** left off (both are the defaults). If you render with Path Tracing, check the EXR on a short range before you treat it as a grading master.

Under **EXR Compression**, **DWAB / DWAA are lossy; ZIP / PIZ are lossless**. Select ZIP/PIZ for lossless delivery or exact pixel comparisons. Lossless compression preserves the encoded half values; it does not turn them into 32-bit float. See the [official OpenEXR technical introduction](https://openexr.com/en/latest/TechnicalIntroduction.html).

If an EXR looks different from a PNG, first check the viewer's linear input interpretation and display transform. Do not apply gamma or exposure twice. Include colour-management and output settings when reporting a problem.

### Which engine, platforms, and output modes are covered?

Supported engines are **Unreal Engine 5.7 and 5.8**, on the **Windows 64-bit editor** only, with **Movie Render Queue, DX12 / SM6, and Deferred**. Fab provides a separate package for each engine version; pick your engine version in the launcher when you install.

The documented workflow covers 360 and 180 (VR180) equirectangular stereo, top/bottom or side by side. The **True ODS Panoramic** settings also offer the **Path Tracing** renderer (**Rendering > Renderer**), mono output (**Stereo** turned off), and cube-face output (**Format > Projection: Cubemap Faces**). These have not been through the same validation as the Deferred stereo workflow; test them on a short range before production. Depth, normal, and mask passes are not promised here. Linux, macOS, and packaged games are not current workflow targets.

Test third-party materials, hair, water, particles, and other special effects on a short sequence first. Real parallax, occlusion, and view-dependent reflections can legitimately differ between eyes; identical images are not the test for correct stereo.

### What does DLAA need? Does it work on UE 5.8?

**DLAA is an optional anti-aliasing method**, selected under **True ODS Panoramic > Anti-Aliasing > Anti-Aliasing Method**. It needs an NVIDIA RTX GPU and NVIDIA's DLSS plugin built for the same engine version and verified by TrueODS: today that is DLSS 8.4.x on UE 5.7. No DLSS plugin is verified for UE 5.8 yet, so on 5.8 DLAA is greyed out and TrueODS uses TSR, the default **Standard (recommended)** method. Everything else works without DLSS. The **DLAA Status** line under the setting says what TrueODS found on your machine.

### Why does the PNG from a UE 5.8 project look different from the viewport?

This is a known limitation. UE 5.8 adds **Method** (Filmic / Standard ACES) under **Post Process > Film**. The TrueODS PNG / JPG / TIFF viewing image always uses the engine's default Filmic tone curve, so in a project set to Standard ACES the viewing image does not match the viewport. **The EXR master is unaffected.** Use Filmic when the viewing image must match the viewport, and grade from the EXR master.

### How long does an 8K frame take? Is 32 GB of VRAM required?

There is no universal time per frame or VRAM threshold. Resolution, sampling, supersampling, scene, effects, hardware, and background load all affect cost.

Follow the [Quick Start](QUICKSTART.md#english), then test ordinary and demanding 8K frames. **VRAM Mode** offers memory/time trade-offs, not fixed ratios for total memory or speed. Read benchmark figures together with their hardware, scene category, settings, and measurement scope. The published render times were measured on Unreal Engine 5.7 with DLAA.

### Are TRUEODS Standard / Pro the same as Fab Personal / Professional?

No. **TRUEODS Standard / Pro are product editions of the plugin. Fab Personal / Professional are Fab price tiers.** Choosing Pro and determining which Fab price tier you need are separate decisions.

Use the released listing, eligibility criteria, and [full Fab licence terms](https://www.fab.com/eula) when purchasing. This FAQ does not add to or replace the platform licence. See [Support](SUPPORT.md#english) for technical help and [Fab's official purchasing help](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab) for orders and refunds.
