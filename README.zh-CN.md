![TRUEODS — 水晶双环主视觉](media/brand/trueods-v24.png)

# TRUEODS
### 360° 立体全景，每个方向都有纵深。

[English](README.md) · **简体中文**

为 Unreal Engine 制作高分辨率 **360° ODS 与 VR180 立体内容**。通过 Movie Render Queue 输出双眼图像序列，覆盖单机制作与多机协同渲染。

[快速上手](docs/QUICKSTART.md) · [Standard 与 Pro](docs/EDITIONS.md) · [常见问题](docs/FAQ.md) · [技术支持](docs/SUPPORT.md)

> **正在准备 Fab 上线**。商品公开后，这里会提供购买入口。支持 **Unreal Engine 5.7 与 5.8 · Windows 64 位 · Movie Render Queue**。

## 四项渲染能力，两版共有

| 能力 | 解决什么问题 |
| :--- | :--- |
| **Lumen 下的正确双眼视差** | 以往在 UE 中用 Lumen 渲染 360° 立体全景，很难让每个方向的双眼视差都正确。TRUEODS 解决了这一点：环顾四周时，物体的纵深与尺度都真实可信。支持 360° ODS、VR180 及上下、左右双眼排列。 |
| **分钟级 8K 双眼渲染** | 单张显卡即可在分钟级完成一帧 8K 双眼 360° 立体画面，让高分辨率输出进入日常制作流程。 |
| **360° 无缝体积雾** | 室内外的体积雾与体积光在整个 360° 全景中连续呈现，看不到拼接痕迹，也没有方向交界处的明暗跳变、条纹等渲染瑕疵。 |
| **线性 HDR 母版** | 输出 16 位半浮点 EXR 序列，用于调色与合成，同时可生成 PNG、JPG 或 16 位 TIFF 审片图。 |

**这里的 8K 双眼**：360° 全景每眼为 **8192 × 4096**，上下双眼整图为 **8192 × 8192**。耗时随场景、设置与显卡变化，可用自己工程中的短序列估算整条镜头。

## 看实际效果

动图在左眼与右眼之间切换，用平面屏幕展示双眼视差。最终立体全景需要在兼容的播放器与头显中观看。

### 环顾四周，都有正确的左右视差

![内景 — 左右眼交替展示双眼视差](media/showcase/interior-stereo.webp)

<!-- TRUEODS-PERF -->
单帧 8K 双眼渲染时间：**RTX 5090 约 47 秒 · RTX 4090 约 56 秒**（Unreal Engine 5.7、开启 DLAA 实测）
<!-- /TRUEODS-PERF -->

近处与远处的物体呈现不同幅度的位移，立体纵深延续到观看位置的四周。

### 分钟级输出 8K 双眼画面

![内景 — 缩小的 8K 双眼渲染预览，左右眼交替](media/showcase/interior-8k.webp)

<!-- TRUEODS-PERF -->
单帧 8K 双眼渲染时间：**RTX 5090 约 42 秒 · RTX 4090 约 54 秒**（Unreal Engine 5.7、开启 DLAA 实测）
<!-- /TRUEODS-PERF -->

高分辨率内景中的灯光、表面细节与近处物体。此处展示的是双眼输出的缩小预览。

### 360° 无缝体积效果，覆盖内景与外景

<!-- TRUEODS-PERF -->

| 内景 | 外景 |
| :---: | :---: |
| ![内景体积雾](media/showcase/interior-fog.webp) | ![外景体积雾](media/showcase/exterior-fog.webp) |
| 单帧 8K 双眼：**RTX 5090 约 47 秒 · RTX 4090 约 56 秒**（与第一个示例同一场景，此画面未单独计时；Unreal Engine 5.7、开启 DLAA 实测） | 单帧 8K 双眼：**RTX 5090 与 RTX 4090 持平，均约 81–82 秒**（Unreal Engine 5.7、开启 DLAA 实测） |
| 室内光线与雾气的连续氛围。 | 全景方向中的浓雾与光照。 |

<!-- /TRUEODS-PERF -->

<!-- TRUEODS-PERF -->
**计时方法：** Unreal Engine 5.7、开启 DLAA，每眼 8192 × 4096、150% 超采样、Paced 显存模式，两台机器使用同一插件版本和同一套设置（RTX 5090 + i9-13900KF，RTX 4090 + i7-13700KF）。每个时间取连续写出 11 帧之间 10 个帧间隔的平均，含拼接与输出保存，不含工程启动与预热。测试帧与预览来自同一场景，但不是同一帧。外景场景的时间主要花在 CPU 一侧的渲染准备上，因此两卡持平。实际耗时随场景、设置及硬件变化。

**重场景参考：** 一个因保密不能展示画面的项目，场景更重，在 Unreal Engine 5.7 上使用 TSR（不是 DLAA），其余设置同上。两台机器用同一套设置渲染同一批连续 6 帧（取其间 5 个帧间隔的平均）：单帧 8K 双眼 **RTX 5090 约 234 秒 · RTX 4090 约 304 秒**。
<!-- /TRUEODS-PERF -->

## 按制作方式选择版本

| | **Standard 标准版** | **Pro 专业版** |
| :--- | :--- | :--- |
| 适合谁 | 独立创作者、单机制作 | 需要多台机器并行完成长镜头的团队 |
| 上述四项渲染能力 | 包含 | 包含 |
| 中断后继续渲染 | 包含 | 包含 |
| **引擎级时序锁定** | — | 跨段保持场景时间连贯 |
| **多机协同渲染** | — | 自动校验配置、分配帧段、检查收帧完整性 |

Standard 可以继续未完成的任务；跨段与续渲的引擎级时序锁定由 Pro 提供。

### Pro · 引擎级时序锁定
**帧号接得上，场景里的动态效果也要接得上。**

一条镜头分开渲染，除了帧号正确，时间驱动的场景效果也要在相同的运动时刻接续。Pro 从引擎层保持这份时间连贯性，让分机分段与中断续渲有一致的时间基础。

### Pro · 多机协同渲染
**自动校验配置、分配帧段、检查收帧完整性。**

插件校验各机器配置、分配帧段，并在收帧时检查输出完整性。你将工程部署到各台机器后，在各台机器的面板上启动或续渲各自的任务。适用于多台自行管理的机器，例如工作室工作站与渲染节点。

[查看版本比较与 Pro 操作流程 →](docs/EDITIONS.md)

未烘焙的模拟、随机或外部驱动的效果，以及需要多帧才能稳定的光照与效果，仍可能需要缓存、预热与接点检查，详见[工作流说明](docs/EDITIONS.md)。Standard / Pro 是产品版本，与 Fab 的 Personal / Professional 价格档分开。

## 从一帧开始

1. 安装与受支持 Unreal Engine 版本匹配的插件包。
2. 在 Movie Render Queue 任务中添加 **True ODS Panoramic**。
3. 先测试一帧 4K 双眼画面，再测试短序列，最后安排目标分辨率的正式渲染。

[打开完整快速上手指南 →](docs/QUICKSTART.md)

## 文档与支持

| 需要什么 | 入口 |
| :--- | :--- |
| 安装与第一次出图 | [快速上手](docs/QUICKSTART.md) |
| 版本选择与多机制作 | [Standard 与 Pro](docs/EDITIONS.md) |
| 输出尺寸、HDR、兼容性与续渲 | [常见问题](docs/FAQ.md) |
| 技术帮助 | [支持说明](docs/SUPPORT.md) · [trueodssupport@gmail.com](mailto:trueodssupport@gmail.com) |
| 售后群 | [申请加入 Telegram 售后群](docs/COMMUNITY.md) · 邮件发送订单凭证，回复获取邀请链接 |
| 上线状态与更新 | [发行状态](docs/CHANGELOG.md) |

此仓库用于公开产品说明与示例，插件包另行分发。演示中的场景、角色及其他第三方资产不包含在插件内。插件涉及的第三方软件声明（OpenEXR、Imath 等引擎库，源自 ACES 与引擎色调曲线的代码，以及可选的 NVIDIA DLSS 插件）见插件包根目录的 `THIRD_PARTY_NOTICES.txt`。

<details>
<summary>环境资产致谢</summary>

感谢 **Leartes Studios** 的 [Coffee Shop Environment](https://www.fab.com/listings/a0c7819e-a61d-4a19-8d3b-f0f5e584e6e0)，以及 **Tirgames Assets** 的 [Sci-Fi Creatures Research Lab](https://www.fab.com/listings/c2494db5-55d2-4e35-9c57-540d0f2cf455)。

</details>
