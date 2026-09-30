# TRUEODS 版本与工作流 / Editions and Workflow

[产品主页](../README.zh-CN.md) / [Home](../README.md) · [快速上手 / Quick Start](QUICKSTART.md) · [常见问题 / FAQ](FAQ.md) · [技术支持 / Support](SUPPORT.md)

**跳转 / Jump to:** [中文](#中文) · [English](#english)

## 中文

**TrueODS 基础版适合独立制作与单机完成镜头；TrueODS Distributed（分布式渲染版）适合把同一镜头交给多台机器并行渲染的制作团队**。两版拥有相同的核心画面能力，分布式渲染版增加**引擎级时序锁定**（跨段画面的时间接续）与**多机协同渲染**（多机任务组织）。

TRUEODS 正在准备 Fab 首发。本页介绍版本定位与工作流，正式功能及兼容范围以发布时商品页和交付包为准。

### 四项共有画面能力

| 卖点 | 带来的结果 | 设置入口 |
|---|---|---|
| **正确立体视差** | 360° 环视时保留立体深度，面向 VR 观看 | MRQ > True ODS Panoramic > Stereo |
| **快速 8K 双眼渲染** | 将高分辨率立体内容用于实际制作，缩短迭代等待；耗时依场景与硬件变化 | Resolution / Supersample / Samples Per Pane / VRAM Mode |
| **无缝体积雾** | 外景大气和内景光束在全景各方向之间连续衔接 | 同一 True ODS Panoramic 渲染流程；雾与光照在场景内设置 |
| **线性 HDR 母版** | 输出供后期调色与合成的 EXR，保留高动态范围 | Output > Also Write EXR (HDR master) / EXR Compression |

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

基础版可以续渲，也可以由用户安排不同帧范围；这些操作本身不会提供分布式渲染版的跨机时序接续与任务协同。单机续渲使用该 MRQ 任务 **True ODS Panoramic** 设置 **Output** 区输出文件夹下方的 **Resume Render** 按钮（输出目录中有未完成的渲染时才出现），详见[快速上手](QUICKSTART.md)。

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

先完成[快速上手](QUICKSTART.md)的单机检查，再打开 **TrueODS Distributed > Multi-Machine Rendering**。在主机上创建任务，由你保存工程并复制到其他参与渲染的机器，最后在主机上收帧；在每台参与渲染的机器上，由你执行配置检查，再启动或续渲该机分到的帧段。

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

## English

**The base TrueODS edition suits independent production and shots completed on one machine. TrueODS Distributed suits teams rendering parts of the same shot in parallel across several machines.** Both share the core image capabilities; TrueODS Distributed adds **Engine-Level Temporal Lock** (scene-time continuity across segments) and **Multi-Machine Rendering** (task organisation across machines).

TRUEODS is preparing for its Fab launch. This page explains edition positioning and workflow. The released listings and packages will define final features and compatibility.

### Four shared image capabilities

| Feature | Result | Where to configure |
|---|---|---|
| **Correct stereo parallax** | Stereo depth while looking around a 360° panorama for VR | MRQ > True ODS Panoramic > Stereo |
| **Fast 8K stereo rendering** | High-resolution stereo for production with shorter iteration waits; time depends on scene and hardware | Resolution / Supersample / Samples Per Pane / VRAM Mode |
| **Seamless volumetric fog** | Continuous outdoor atmosphere and indoor light shafts between panorama directions | The same True ODS Panoramic workflow; configure fog and lighting in the scene |
| **Linear HDR masters** | EXR output retaining high dynamic range for grading and compositing | Output > Also Write EXR (HDR master) / EXR Compression |

For 360 top/bottom stereo, 8K is **8192 × 4096 per eye and 8192 × 8192 combined**. Test materials and effects on a short sequence in your scene first.

### Choose an edition

| Capability / workflow | Base edition | TrueODS Distributed |
|---|---|---|
| The four shared image capabilities | Included | Included |
| Single-machine rendering and resume after interruption | Included | Included |
| User-organised shots or frame ranges | Supported | Supported |
| **Engine-Level Temporal Lock** across segments and machines | Not included | Included |
| **Multi-Machine Rendering**: automatic configuration checks, frame-range allocation, and output completeness checks | Not included | Included |
| Typical use | Independent creators, smaller projects, single-machine shots | Production teams, long shots, several machines you manage yourself |

The base edition can resume and users can arrange frame ranges themselves. Those operations alone do not provide TrueODS Distributed's cross-machine time continuity and coordinated task workflow. For a single-machine resume, use the **Resume Render** button below the output folder in the **Output** section of **True ODS Panoramic** (it appears only when the output folder holds an unfinished render); see [Quick Start](QUICKSTART.md#english).

### TrueODS Distributed: Engine-Level Temporal Lock

**The frame numbers line up. The scene's motion should, too.**

For example, the first and second halves of a shot are rendered separately: on two machines, or on one machine in two runs that each start at their own first frame. The frame numbers must line up, and time-driven effects must reach the same point in time. TrueODS Distributed maintains time continuity at the engine level across segments, addressing time offsets that simply assigning frame ranges may leave unresolved.

This capability handles **continuity in scene time**; it does not guarantee that every simulation matches exactly. Unbaked simulations, random or externally driven effects, and lighting or effects that need several frames to settle still need appropriate caches, repeatable settings, warm-up, and join checks.

If the sequence has a Time Dilation track, time continuity is guaranteed only for segments that start before its first key. For a multi-machine job, enter that key's frame number in **Frames no part may start at** under **Advanced** in the **TrueODS Multi-Machine** panel (empty by default).

Temporal Lock is on by default in TrueODS Distributed and is used for segments and multi-machine jobs; you do not need to open a separate tool. It corresponds to **Consistent Motion Timing** in the job's **True ODS Panoramic** settings in MRQ. That option appears once **Rendering On Several Machines** is switched on; it is on by default, so keep it on.

### TrueODS Distributed: Multi-Machine Rendering

**Automatic configuration checks, frame-range allocation, and output completeness checks.**

This capability handles **task organisation**. Create a job in the **TrueODS Multi-Machine** panel, and the plugin allocates frame ranges by machine. Run configuration checks and start or resume the job on each machine, then bring the output together for the plugin to check frame completeness. Together with Temporal Lock, it supports rendering one shot in segments across several machines you manage yourself.

The panel checks each machine's configuration, allocates frame ranges, and checks the collected output for completeness, listing any missing frames. You copy the project to every machine, install the same engine version and plugin build on each, handle third-party resource licensing, set each machine's output location and storage, start or resume rendering on each machine, and copy the other machines' output to the host before collecting; the plugin does not connect to render-queue or farm management software. Passing a configuration check confirms the checked settings meet requirements; final images still need review.

### TrueODS Distributed multi-machine workflow

Complete the single-machine [Quick Start](QUICKSTART.md#english), then open **TrueODS Distributed > Multi-Machine Rendering**. You create the job on the host, save the project and copy it to the other rendering machines yourself, and collect the frames on the host at the end. On every rendering machine, you run the configuration check, then start or resume rendering that machine's assigned frames.

**Find TrueODS Distributed in the editor's top menu bar, to the right of Help.** Click it and choose **Multi-Machine Rendering** to open the **TrueODS Multi-Machine** panel tab. This entry is available in TrueODS Distributed only; in the base edition the top-level menu is named **TrueODS** and has no multi-machine entry.

[![Multi-Machine Rendering entry: TrueODS Distributed to the right of Help, select Multi-Machine Rendering, and the TrueODS Multi-Machine panel opens](../media/guide/distributed-menu-entry.png)](../media/guide/distributed-menu-entry.png)

These are cropped captures of the actual interface; click to view the originals. They show where to find the controls; the example ranges and values are not recommendations. Layout may vary with the installed version.

#### 1. Create the job · Host

Prepare the level, sequence, and MRQ settings. In the job's **True ODS Panoramic** settings, switch on **Rendering On Several Machines**; **Consistent Motion Timing** then appears, already on, so keep it on. In the panel, press **Re-read Queue**, enter a **Job name** (the sequence name is added in front automatically; leave it empty to use the sequence name and date), choose **Whole sequence / Frames** and **Machines**, fill in **Split (optional)** if needed, confirm **Multi-machine stability** (it has a default), then press **Create Job**. A multi-machine job writes one panorama per frame, so **Format > Projection** must be Equirectangular; with Cubemap Faces, **Create Job** rejects the job and explains why. Render cube faces on one machine.

[![TrueODS Distributed job creation: Re-read Queue, job name, frame range, machine count, multi-machine stability option and Create Job button](../media/guide/distributed-create-job.png)](../media/guide/distributed-create-job.png)

**What to look for:** Press **Re-read Queue** first, and create the job once the panel has read the job in the queue. **Machines** is the number of rendering machines. Leave **Split (optional)** empty for equal shares, or enter one number per machine, separated by commas (for example, 2,1 gives machine 1 twice as many frames as machine 2). Under **Multi-machine stability**, the panel states what each option does to the picture and how much render time it adds; read it before creating the job. To change the job's settings later, press **Recreate Job** to rebuild the job of the same name, then copy the project to every machine again as in step 2 and repeat the step 3 check on each. **Open Job Folder** opens the job's folder.

#### 2. Save and copy · Host

Press **Save All Now** in the panel (or **File > Save All**), then copy the whole project folder yourself, including its `TrueODS\Jobs` job folder, to every other rendering machine. Every machine needs the same engine version and the same plugin build. After copying, do not save the level or the sequence on any machine, or the step 3 check will fail. Each machine's output folder is set in step 4.

#### 3. Check configuration · Every machine

Select the job in the **Job** dropdown to the left of **Rescan**, then press **Check This Machine**. If the job is not listed, press **Rescan** first. Resolve reported mismatches before rendering.

[![TrueODS Distributed configuration check: Rescan and Check This Machine buttons](../media/guide/distributed-check-machine.png)](../media/guide/distributed-check-machine.png)

#### 4. Start or resume · Every machine

Select the local machine number. On a machine with less video memory than the host, first select a lower setting under **VRAM mode on this machine** (the picture is the same, rendering is slower). Confirm the assigned range and output location, then press **Start Rendering On This Machine**. Machines of the same class (same GPU family and amount of video memory) give the best results. By default the editor then closes by itself **without saving** unsaved changes, so this machine renders exactly the files checked in step 3 and copied to the other machines; **Resume Render Job** does the same. Two checkboxes under **Output folder on this machine** control this. If the scene needs changes after step 2, rebuild the job with **Recreate Job** instead of ticking the save option.

If a render stops, a **Resume Render Job** button appears in this step: finished parts are skipped and the stopped part continues where it stopped. A resume keeps the frames already written with complete records and renders the missing ones; it also renders the frames just before the first missing frame again and blends them, frame by frame, into the frames already on disk, so the render does not jump where it stopped. If missing frames lie between frames already written, the frames after the gap are also rendered again and blended, and the resume note does not count them. With default settings that is as many frames as the **Handle frames** shown in this step (32 by default); the resume note in this step gives the exact number. These frames take extra render time. The frames as they were before blending are kept in a `_resume_originals` folder next to the output frames.

Progress is shown in the **Watch Console** window: completed frames, the estimated finish time, and GPU usage. It opens automatically when you press **Start Rendering On This Machine**; if you close it, reopen it with **Open Watch Console** in the panel or from the menu **TrueODS Distributed > Watch Console**. The editor panel itself does not show live progress.

#### 5. Collect and review · Host

Copy the other machines' output to the host as the panel directs, then press **Collect Frames**. It gathers every machine's frames into one place, lists missing frames, and compares the frames rendered on both sides of each join. Once all frames have been collected, it applies a crossfade at each join for EXR output. If frames are missing, it skips the crossfades; fill in the missing frames, then press **Collect Frames** again. The frames replaced by blending are kept in the `dissolve_originals` folder inside the collected folder. After reviewing the missing-frame and collection reports, still play across the joins yourself to check timing, simulations, fog, and lighting.

After changing the scene, sequence, or key settings, rebuild the corresponding job with **Recreate Job**, copy the project to every machine again as in step 2, and repeat the step 3 check on each. Preserve old output and use a separate directory for new results. Determine caches and warm-up through tests on the actual shot.

### Product editions and Fab price tiers

**The base TrueODS edition and TrueODS Distributed are product editions. Fab Personal / Professional are Fab price tiers.** Select them separately. Eligibility and rights are governed by the released listing and [full Fab terms](https://www.fab.com/eula).
