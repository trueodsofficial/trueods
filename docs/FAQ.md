# TRUEODS FAQ

**English** · [简体中文](FAQ.zh-CN.md)
<a id="english"></a>

[Home](../README.md) · [Quick Start](QUICKSTART.md) · [Editions](EDITIONS.md) · [Support](SUPPORT.md)

### Where can I buy TRUEODS?

TRUEODS is preparing for its Fab launch. The [product home](../README.md) will link to the official listings. This repository contains product information, documentation, and support resources, not the plugin download package. No public release has been announced; see the [changelog](CHANGELOG.md).

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

Each time, choose to re-render and blend X frames before the gap, or continue at the missing frame with warm-up only. Blending adds render time to smooth the join; direct continuation avoids re-rendering earlier frames but lighting, fog, or reflections may jump. Frames written only as 16-bit TIFF, without the EXR master, cannot be blended, so the first choice is unavailable. If the level, a sublevel or the sequence was saved after the frames were rendered, the notice says so and adds a third choice, **Render every frame again**: choose it when the scene did change; a save without changes can be ignored. Path Tracing continues directly. The button starts the whole queue, so keep only this job if it is the only one you want to resume. See [Quick Start](QUICKSTART.md).

TrueODS Distributed multi-machine jobs still use **Resume Render Job** in step 4 of **TrueODS Multi-Machine**. The default overlap is controlled by **Handle frames** in that step; see the [multi-machine workflow](EDITIONS.md).

TrueODS Distributed adds two distinct capabilities:

- **Engine-Level Temporal Lock** maintains timing continuity for time-driven effects across segments and machines at the engine level. Parts of one shot stay on the same timeline. Assigning different frame ranges alone does not provide this capability.
- **Multi-Machine Rendering**, available through **TrueODS Distributed > Multi-Machine Rendering**, provides automatic configuration checks, frame-range allocation, and output completeness checks. It reduces repetitive task preparation on each machine. Users deploy the project and start or resume work on each machine.

The two editions share the same core image capabilities. See [Editions and Workflow](EDITIONS.md) for a comparison and steps.

### Does Temporal Lock make every water, fire, or particle simulation identical at a join?

Not guaranteed. Temporal Lock addresses **engine-time continuity**. Unbaked simulations, random or externally driven effects, and lighting or effects that need several frames to settle may still require caches, repeatable settings, and warm-up. If the sequence has a Time Dilation track, time continuity is guaranteed only for segments that start before its first key; for a multi-machine job, enter that key's frame number in **Frames no part may start at** under **Advanced** in the **TrueODS Multi-Machine** panel (empty by default).

Use matching project content, engine versions, and plugin versions on all machines, preferably with GPUs from the same family and with the same amount of video memory. Run the configuration check and test the actual joins. Determine warm-up from scene tests. A configuration check does not certify that every simulation state matches or replace visual inspection of joins.

### How does seamless volumetric rendering differ from Temporal Lock in TrueODS Distributed?

**Seamless volumetric rendering is shared by both editions.** It concerns the continuity of volumetric effects between viewing directions within a panorama.

Supported volume types include **height fog and volumetric fog, mesh-based volume materials, Local Fog Volumes, Volumetric Clouds, and VDB / Heterogeneous Volumes**.

Custom fog effects and particle cards for steam, rain, airborne dust and smoke are also supported.

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

Follow the [Quick Start](QUICKSTART.md), then test ordinary and demanding 8K frames. **VRAM Mode** offers memory/time trade-offs, not fixed ratios for total memory or speed. Read benchmark figures together with their hardware, scene category, settings, and measurement scope. The published render times were measured on Unreal Engine 5.7 with the default TSR anti-aliasing.

Published benchmark figures were measured with plugin **Version 67 (v12)**.

### Are the base TrueODS edition and TrueODS Distributed the same as Fab Personal / Professional?

No. **The base TrueODS edition and TrueODS Distributed are product editions of the plugin. Fab Personal / Professional are Fab price tiers.** Choosing TrueODS Distributed and determining which Fab price tier you need are separate decisions.

Use the released listing, eligibility criteria, and [full Fab licence terms](https://www.fab.com/eula) when purchasing. This FAQ does not add to or replace the platform licence. See [Support](SUPPORT.md) for technical help and [Fab's official purchasing help](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab) for orders and refunds.
