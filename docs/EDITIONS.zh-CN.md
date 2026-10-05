# TRUEODS 版本与工作流

[English](EDITIONS.md) · **简体中文**

[产品主页](../README.zh-CN.md) · [快速上手](QUICKSTART.zh-CN.md) · [常见问题](FAQ.zh-CN.md) · [技术支持](SUPPORT.zh-CN.md)

**TrueODS 基础版适合独立制作与单机完成镜头；TrueODS Distributed（分布式渲染版）适合把同一镜头交给多台机器并行渲染的制作团队**。两版拥有相同的核心画面能力，分布式渲染版增加**引擎级时序锁定**（跨段画面的时间接续）与**多机协同渲染**（多机任务组织）。

TRUEODS 正在准备 Fab 首发。本页介绍版本定位与工作流，正式功能及兼容范围以发布时商品页和交付包为准。

### 四项共有画面能力

| 卖点 | 带来的结果 | 设置入口 |
|---|---|---|
| **正确立体视差** | 360° 环视时保留立体深度，面向 VR 观看 | MRQ > True ODS Panoramic > Stereo |
| **快速 8K 双眼渲染** | 将高分辨率立体内容用于实际制作，缩短迭代等待；耗时依场景与硬件变化 | Resolution / Supersample / Samples Per Pane / VRAM Mode |
| **无缝体积渲染** | 体积效果在全景各观看方向之间连续衔接 | 同一 True ODS Panoramic 渲染流程；体积效果在场景内设置 |
| **线性 HDR 母版** | 输出供后期调色与合成的 EXR，保留高动态范围 | Output > Also Write EXR (HDR master) / EXR Compression |

两版均支持**高度雾与体积雾、网格体积材质、Local Fog Volume（局部雾体积）、Volumetric Cloud（体积云），以及 VDB / Heterogeneous Volumes（异质体积）**。

也支持自定义雾效果，以及用于蒸汽、雨、浮尘和烟的粒子贴片。

360 上下双眼的 8K 输出为 **8192 × 4096 每眼、8192 × 8192 整图**。各类材质与特效应先在自己的场景中做短测试。

### 两个版本怎么选？

| 能力 / 使用方式 | 基础版 | 分布式渲染版 |
|---|---|---|
| 上述四项画面能力 | 包含 | 包含 |
| 单机渲染与中断后续渲 | 包含 | 包含 |
| 由用户按镜头或帧范围安排任务 | 支持 | 支持 |
| **引擎级时序锁定**：分段与多机的时间接续 | 不包含 | 包含 |
| **多机协同渲染**：自动校验配置、分配帧段、检查收帧完整性 | 不包含 | 包含 |
| 典型用途 | 独立创作者、小型项目、单机完成镜头 | 制作团队、长镜头、多台自行管理的机器 |

基础版可以续渲，也可以由用户安排不同帧范围；这些操作本身不会提供分布式渲染版的跨机时序接续与任务协同。单机续渲使用该 MRQ 任务 **True ODS Panoramic** 设置 **Output** 区输出文件夹下方的 **Resume Render** 按钮（输出目录中有未完成的渲染时才出现），详见[快速上手](QUICKSTART.zh-CN.md)。

### 分布式渲染版：引擎级时序锁定

**帧号接得上，场景里的动态效果也要接得上。**

例如，一个镜头的前半段和后半段分开渲染：可能交给两台机器，也可能在同一台机器上分两次、各从自己的起始帧开始渲染。除了接上帧号，还需要让时间驱动的效果接上同一时刻。分布式渲染版从引擎层保持跨段的时间接续，解决只划分帧范围仍可能出现的时间错位问题。

这项功能负责**画面时间接续**，但不保证所有模拟完全一致。未烘焙的模拟、随机或外部驱动效果，以及需要多帧才能稳定的光照与效果，仍需对应缓存、可重复设置、预热与接点检查。

序列含 Time Dilation 轨道时，只有在其第一个关键帧之前开始的分段才保证时间接续。多机任务请在 **TrueODS Multi-Machine** 面板 **Advanced** 区的 **Frames no part may start at** 填入该关键帧的帧号（默认留空）。

时序锁定在分布式渲染版中默认开启，用于分段与多机渲染，无需单独入口。它对应 MRQ 任务 **True ODS Panoramic** 设置中的 **Consistent Motion Timing**；该选项在打开 **Rendering On Several Machines** 后显示，默认开启，请保持开启。

### 分布式渲染版：多机协同渲染

**自动校验配置、分配帧段、检查收帧完整性。**

这项功能负责**任务组织**。用户在 **TrueODS Multi-Machine** 面板中创建任务后，插件按机器分配帧段；用户在各台机器上运行配置检查并启动或续渲，最后汇总输出，由插件检查收帧完整性。它与时序锁定配合，用于在多台自行管理的机器上分段渲染同一镜头。

面板负责检查各机器的配置、分配帧段，并在收帧时检查输出完整性、列出缺失帧。把工程复制到每台机器、在各机器安装相同版本的引擎与同一插件构建、第三方资源授权、设置各机器的输出位置与存储、在每台机器上启动或续渲，以及收帧前把其他机器的输出复制到主机，都由用户完成；插件不对接渲染队列或农场管理软件。配置检查通过表示检查范围内的配置符合要求，最终画面仍要验收。

### 分布式渲染版多机流程

先完成[快速上手](QUICKSTART.zh-CN.md)的单机检查，再打开 **TrueODS Distributed > Multi-Machine Rendering**。在主机上创建任务，由你保存工程并复制到其他参与渲染的机器，最后在主机上收帧；在每台参与渲染的机器上，由你执行配置检查，再启动或续渲该机分到的帧段。

**入口在编辑器顶栏的 Help 右侧**。点击 **TrueODS Distributed**，再选 **Multi-Machine Rendering**，打开的面板标签页为 **TrueODS Multi-Machine**。此入口仅在分布式渲染版提供；基础版的顶层菜单名为 **TrueODS**，其中没有多机入口。

[![多机协同渲染入口：顶栏 Help 右侧的 TrueODS Distributed，选择 Multi-Machine Rendering，打开 TrueODS Multi-Machine 面板](../media/guide/distributed-menu-entry.png)](../media/guide/distributed-menu-entry.png)

以下为真实界面的局部截图，点击可查看原图。截图用于定位操作，图中帧范围与数值是示例；具体布局以安装版本为准。

#### 1. 创建任务 · 主机

准备关卡、序列和 MRQ 配置，并在该任务的 **True ODS Panoramic** 设置中打开 **Rendering On Several Machines**；随后显示的 **Consistent Motion Timing** 默认开启，请保持开启。在面板中点击 **Re-read Queue** 读取队列，填写 **Job name**（序列名会自动加在前面；留空则使用序列名加日期），选择 **Whole sequence / Frames** 和 **Machines**，按需填写 **Split (optional)**，确认 **Multi-machine stability**（有默认值），最后点击 **Create Job**。多机任务每帧输出一张全景图，**Format > Projection** 需为 Equirectangular；选了 Cubemap Faces 时 **Create Job** 会拒绝并说明原因，立方体面请在单机渲染。

[![分布式渲染版创建任务：读取队列、任务名、帧范围、机器数、多机稳定性选项和 Create Job 按钮](../media/guide/distributed-create-job.png)](../media/guide/distributed-create-job.png)

**看这里**：先点击 **Re-read Queue**，面板读到队列中的任务后再创建。**Machines** 是参与渲染的机器数量；**Split (optional)** 留空时均分，也可为每台机器各填一个数字，用英文逗号分隔（例如 2,1 表示 1 号机分到的帧数是 2 号机的两倍）。面板会在 **Multi-machine stability** 下方写明每个选项对画面的影响和增加的耗时，创建前先看这段说明。之后要修改任务设置，点击 **Recreate Job** 重建同名任务，再按第 2 步把工程重新复制到每台机器，并在各机器重做第 3 步检查。**Open Job Folder** 打开该任务的文件夹。

#### 2. 保存并复制 · 主机

点击面板中的 **Save All Now**（或 **File > Save All**），然后由你把整个工程文件夹（含其中的 `TrueODS\Jobs` 任务文件夹）复制到其他每台参与渲染的机器。每台机器需使用相同的引擎版本和同一插件构建。复制后不要在任何机器上再保存关卡或序列，否则第 3 步检查不会通过。各机器的输出目录在第 4 步设置。

#### 3. 检查配置 · 每台机器

在 **Rescan** 左侧的 **Job** 下拉框选择任务，点击 **Check This Machine**。列表里没有该任务时先点 **Rescan**。按检查报告修正不匹配项，通过后再进行渲染。

[![分布式渲染版配置检查：Rescan 和 Check This Machine 按钮](../media/guide/distributed-check-machine.png)](../media/guide/distributed-check-machine.png)

#### 4. 启动或继续 · 每台机器

选择本机编号；显存少于主机的机器，先在 **VRAM mode on this machine** 中选更低一档（画面相同，渲染更慢）。确认分配帧段和输出位置后，点击 **Start Rendering On This Machine**。同级别的机器（同一显卡系列、相同显存）效果最好。点击后，编辑器默认会自动关闭，并且**不保存**未保存的修改，本机渲染的就是第 3 步检查过、已复制到各机器的文件；**Resume Render Job** 也是如此。这由 **Output folder on this machine** 下方的两个勾选框控制。第 2 步之后如需修改场景，请用 **Recreate Job** 重建任务，而不是勾选先保存。

渲染中断时，此步骤会出现 **Resume Render Job** 按钮：已完成的部分会跳过，中断的部分从停止处继续。续渲会保留已写出且记录完整的帧、补渲缺失帧，同时把第一个缺失帧之前的若干帧重新渲染，并与磁盘上的原帧逐帧平滑混合写回，使中断处不跳变。若缺失帧夹在已写出的帧中间，缺帧之后的若干帧也会同样重渲并混合，不计入续渲提示中的帧数。默认设置下，重渲帧数等于本步骤显示的 **Handle frames**（默认 32 帧）；此步骤的续渲提示会写出具体帧数。这些帧需要额外的渲染时间。混合前的原帧保存在输出帧旁的 `_resume_originals` 文件夹。

进度显示在 **Watch Console** 窗口中：已完成帧数、预计结束时间和 GPU 使用情况。点击 **Start Rendering On This Machine** 后会自动打开；关闭后可在面板中点击 **Open Watch Console**，或通过菜单 **TrueODS Distributed > Watch Console** 重新打开。编辑器面板本身不显示实时进度。

#### 5. 汇总与验收 · 主机

依面板提示把其他机器的输出复制到主机，点击 **Collect Frames**：它把各机器的帧汇集到一起并列出缺失帧，还会比对每个接点两侧都渲染过的帧；收齐全部帧后，EXR 输出会在接点处做淡入淡出过渡（有缺帧时不做，补齐后再点一次 **Collect Frames**），被替换的原帧保存在收帧目录内的 `dissolve_originals` 文件夹。查看缺帧及收帧报告后，仍要亲自连续播放交界处，检查时间接续、模拟、雾与光照。

修改场景、序列或关键设置后，用 **Recreate Job** 重建对应任务，再按第 2 步把工程重新复制到各台机器，并在各机器重做第 3 步检查；保留旧输出，新的结果使用独立目录。预热和缓存按实际镜头测试确定。

### 产品版本与 Fab 价格档

**TrueODS 基础版 / 分布式渲染版是产品版本；Fab Personal / Professional 是 Fab 价格档**。二者分别选择。购买资格及权利范围以正式商品页与 [Fab 完整条款](https://www.fab.com/eula)为准。
