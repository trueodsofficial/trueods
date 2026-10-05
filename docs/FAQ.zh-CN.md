# TRUEODS 常见问题

[English](FAQ.md) · **简体中文**

[产品主页](../README.zh-CN.md) · [快速上手](QUICKSTART.zh-CN.md) · [版本选择](EDITIONS.zh-CN.md) · [技术支持](SUPPORT.zh-CN.md)

### 现在可以在哪里购买？

TRUEODS 正在准备 Fab 首发。正式商品链接会在[产品主页](../README.zh-CN.md)公布；本仓库提供产品介绍、使用文档与支持信息，不是插件下载包。当前没有已宣布的正式发行版，见[更新记录](CHANGELOG.zh-CN.md)。

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

每次可以选择“重渲并混合中断前的 X 帧”或“直接从缺失帧继续，仅预热”。前者增加渲染时间以平滑接点；后者不重渲之前的帧，但光照、雾或反射可能跳变。只写了 16 位 TIFF、没有 EXR 母版的帧不能混合，前一种不可选。关卡、子关卡或序列在这些帧渲完之后存过盘时，提示会写明，并多出第三种“全部重渲”（**Render every frame again**）：场景确实改过时选它，只是存了盘、没改内容可以不理会。Path Tracing 直接从首个缺失帧继续。按钮会启动整个队列，因此只想续渲单个任务时应让队列只保留该任务。完整说明见[快速上手](QUICKSTART.zh-CN.md)。

分布式渲染版的多机任务仍使用 **TrueODS Multi-Machine** 面板第 4 步的 **Resume Render Job**；默认接点重渲范围由该步骤的 **Handle frames** 控制，详见[多机工作流](EDITIONS.zh-CN.md)。

分布式渲染版的重点是两项不同能力：

- **引擎级时序锁定 / Engine-Level Temporal Lock**：从引擎层保持时间驱动效果跨分段与机器的时间接续。把同一镜头分给多台机器后，各段仍接在同一条时间线上；单纯给机器设置不同帧范围并没有完成这项工作。
- **多机协同渲染 / Multi-Machine Rendering**：通过 **TrueODS Distributed > Multi-Machine Rendering** 自动校验配置、分配帧段、检查收帧完整性，减少逐台手工整理任务。用户仍需部署工程，并在各台机器上启动或续渲。

两版画质相关的共有能力相同。选择依据与完整步骤见[版本与工作流](EDITIONS.zh-CN.md)。

### 有时序锁定，所有水、火、粒子都能在接点完全一样吗？

不保证。时序锁定解决的是**引擎内时间接续**。未烘焙的模拟、随机或外部驱动的效果，以及需要多帧才能稳定的光照和特效，仍可能需要缓存、相应的可重复设置和预热。序列含 Time Dilation 轨道时，只有在其第一个关键帧之前开始的分段才保证时间接续；多机任务请在 **TrueODS Multi-Machine** 面板 **Advanced** 区的 **Frames no part may start at** 中填入该关键帧的帧号（默认留空）。

开始多机任务前，应使用相同的工程内容、引擎与插件版本，最好使用同一级别的显卡（同一系列、相同显存），运行配置检查，并测试实际接点。预热量按场景实测确定。配置检查通过不等于所有模拟状态一致，也不替代接点画面检查。

### “无缝体积渲染”与分布式渲染版的时序锁定有什么不同？

**无缝体积渲染是两版共有的画面能力**，关注一张全景图不同观看方向之间的体积效果衔接。

支持的体积类型包括**高度雾与体积雾、网格体积材质、Local Fog Volume（局部雾体积）、Volumetric Cloud（体积云），以及 VDB / Heterogeneous Volumes（异质体积）**。

也支持自定义雾效果，以及用于蒸汽、雨、浮尘和烟的粒子贴片。

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

先按[快速上手](QUICKSTART.zh-CN.md)渲染单帧，再测试 8K 的普通帧与最重帧。**VRAM Mode** 可调整内存与耗时的取舍；它不保证整机内存或速度的固定比例。公布的测试数据需要结合其硬件、场景类别、设置和测量范围阅读；公布的渲染耗时均在 Unreal Engine 5.7 上、使用默认的 TSR 抗锯齿测得。

已公布的性能数据使用插件 **Version 67（v12）** 实测。

### TrueODS 基础版 / 分布式渲染版与 Fab Personal / Professional 是一回事吗？

不是。**TrueODS 基础版 / 分布式渲染版是插件的产品版本**；**Fab Personal / Professional 是 Fab 价格档**。选择分布式渲染版这一产品版本与是否需要 Professional 价格档是两个独立问题。

购买时请依照正式商品页、适用资格与 [Fab 完整许可条款](https://www.fab.com/eula)选择；本 FAQ 不增加或替代平台许可。技术问题请见[支持说明](SUPPORT.zh-CN.md)，订单及退款请使用 [Fab 官方购买帮助](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab)。
