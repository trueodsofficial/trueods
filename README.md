![TRUEODS — crystal double-loop identity](media/brand/trueods-v24.png)

# TRUEODS
### 360° stereoscopic panoramas with correct parallax in every direction

**English** · [简体中文](README.zh-CN.md)

High-resolution **360° ODS and VR180 rendering for Unreal Engine**. Create stereo image sequences in Movie Render Queue, whether you work on a single workstation or coordinate rendering across multiple machines.

[Sample downloads](#sample-downloads) · [Quick start](docs/QUICKSTART.md#english) · [Editions](docs/EDITIONS.md#english) · [FAQ](docs/FAQ.md#english) · [Support](docs/SUPPORT.md#english)

> **Preparing for our Fab launch.** Purchase links will appear here when the listings are live. Supported: **Unreal Engine 5.7 and 5.8 · Windows 64-bit · Movie Render Queue**.

## Four capabilities. Both editions.

| Capability | What it gives you |
| :--- | :--- |
| **Correct stereo parallax with Lumen** | Achieving correct parallax in every direction can be difficult when rendering 360° stereo panoramas with Lumen in Unreal Engine. TRUEODS preserves stereo depth and scale wherever you look. Render 360° ODS or VR180, with top/bottom or side-by-side stereo layouts. |
| **8K stereo in minutes** | A single GPU renders both eyes of an 8K 360° panorama in minutes, making high-resolution output practical for everyday production. |
| **Seamless volumetric rendering** | Volumetric effects stay continuous across the full 360° panorama, indoors and outdoors, without stitching seams or brightness jumps where view directions meet. |
| **Linear HDR masters** | 16-bit half-float EXR sequences for grading and compositing, alongside PNG, JPG or 16-bit TIFF review output. |

Supported volume types include **height fog and volumetric fog, mesh-based volume materials, Local Fog Volumes, Volumetric Clouds, and VDB / Heterogeneous Volumes**.

Custom fog effects and particle cards for steam, rain, airborne dust and smoke are also supported.

**8K 360° stereo in these examples:** **8192 × 4096 per eye**, or **8192 × 8192** in a combined top/bottom frame. Render times depend on the scene, settings and GPU; use a short test render from your own project to estimate the time needed for the full sequence.

<sub>The benchmark figures on this page were measured with plugin Version 67 (v12).</sub>

## Sample downloads

**Find each scene's original image downloads directly below its preview.** All images are **8192 × 8192, 360° Top/Bottom stereo** — **8192 × 4096 per eye**. Choose **16-bit PNG originals** or smaller, same-resolution **8-bit JPGs at quality 100 (4:4:4)**. JPGs retain the original colour profile; JPEG remains a lossy format.

<sub>Ghosting at the top and bottom poles comes from Pole Mono Merge, not a rendering artifact.</sub>

**[Download the coffee-shop video (MP4 · 341.0 MB)](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/coffee_sample.mp4)** — 8192 × 8192 Top/Bottom stereo, 30 fps, about 20 seconds; HEVC/H.265 video.

Use a 360° player that supports 8K HEVC and manually select **360° equirectangular / Top/Bottom stereo**. The video does not contain embedded VR projection or stereo metadata.

**[Download all five JPGs (ZIP · 154.13 MB)](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/TrueODS-8K-JPG-quality100.zip)**

**Additional exterior original (checkpoint01):** [JPG · 36.2 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/checkpoint01_00108619.jpg) · [PNG original · 215.5 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/checkpoint01_00108619.png)

## See the result

The previews alternate between the left and right eye. They show stereo parallax on a flat screen; use a compatible headset and player to view the final stereo panorama.

### Correct stereo parallax in every direction

![Interior — alternating left and right eye](media/showcase/interior-stereo.webp)

[JPG · 26.5 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/coffee01_00108409.jpg) · [PNG original · 295.8 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/coffee01_00108409.png)

<sub>Ghosting at the top and bottom poles comes from Pole Mono Merge, not a rendering artifact.</sub>

<!-- TRUEODS-PERF -->
Render time per 8K stereo frame (both eyes): **RTX 5090 ~48 s · RTX 4090 ~59 s** · measured in Unreal Engine 5.7
<!-- /TRUEODS-PERF -->

Near and distant objects shift by different amounts. The sense of depth is preserved as you look around.

### 8K stereo output in minutes

![Interior — reduced preview of an 8K stereo render, alternating left and right eye](media/showcase/interior-8k.webp)

[JPG · 38.6 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/labdark01_00108668.jpg) · [PNG original · 332.6 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/labdark01_00108668.png)

<sub>Ghosting at the top and bottom poles comes from Pole Mono Merge, not a rendering artifact.</sub>

<!-- TRUEODS-PERF -->
Render time per 8K stereo frame (both eyes): **RTX 5090 ~44 s · RTX 4090 ~56 s** · measured in Unreal Engine 5.7
<!-- /TRUEODS-PERF -->

Detailed lighting, surface textures and nearby objects in a high-resolution interior. This is a downscaled preview of the stereo render.

<!-- TRUEODS-PERF -->
**How the times were measured:** Unreal Engine 5.7 with the default TSR anti-aliasing, 8192 × 4096 per eye, 150% supersampling, Paced memory mode, the same plugin version and settings on both machines (RTX 5090 + i9-13900KF, RTX 4090 + i7-13700KF). Each time is the average of the 10 intervals between 11 consecutively saved frames, including stitching and saving the output, excluding project startup and warm-up. The timed frames come from the same scenes as the previews but are not the frames shown. Actual times vary with scene, settings and hardware.

**Heavy-scene reference:** a confidential project whose images cannot be shown; a heavier scene rendered on Unreal Engine 5.7, settings as above. Both machines rendered the same 6 consecutive frames with the same settings (average of the 5 intervals between them): **RTX 5090 ~234 s · RTX 4090 ~304 s** per 8K stereo frame.
<!-- /TRUEODS-PERF -->

### Seamless volumetric rendering — interior and exterior fog examples

| Interior | Exterior |
| :---: | :---: |
| ![Interior volumetric fog](media/showcase/interior-fog.webp)<br>[JPG · 25.5 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/coffee02_00108360.jpg) · [PNG original · 293.5 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/coffee02_00108360.png)<br><sub>Ghosting at the top and bottom poles comes from Pole Mono Merge, not a rendering artifact.</sub> | ![Exterior volumetric fog](media/showcase/exterior-fog.webp)<br>[JPG · 27.2 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/CP01_00108668.jpg) · [PNG original · 299.6 MB](https://github.com/trueodsofficial/trueods/releases/download/samples-20260928/CP01_00108668.png)<br><sub>Ghosting at the top and bottom poles comes from Pole Mono Merge, not a rendering artifact.</sub> |
| Continuous volumetric fog and lighting throughout the interior, with no seams across the full 360° view. | Dense outdoor fog across the full panorama, with no breaks or brightness jumps where view directions meet. |


## Choose your workflow

| | **TrueODS** (base edition) | **TrueODS Distributed** |
| :--- | :--- | :--- |
| Best fit | Independent creators and single-workstation production | Teams splitting long shots across multiple machines |
| All four rendering capabilities above | Included | Included |
| Resume an unfinished render | Included | Included |
| **Engine-Level Temporal Lock** | — | Consistent scene timing across rendered sections |
| **Multi-Machine Rendering** | — | Automatic configuration checks, frame-range allocation and output completeness checks |

Both editions can resume unfinished jobs. TrueODS Distributed adds engine-level temporal continuity across separately rendered sections and machines.

### TrueODS Distributed · Engine-Level Temporal Lock
**The frame numbers line up. The scene's motion should, too.**

Starting at the correct frame is only part of joining a shot. Time-driven scene effects must also stay in sync across segment boundaries. TrueODS Distributed maintains that timing at the engine level when sections are rendered separately or on several machines.

### TrueODS Distributed · Multi-Machine Rendering
**Automatic configuration checks, frame-range allocation, and output completeness checks.**

The plugin checks each machine's configuration, allocates frame ranges, and checks output completeness during collection. Deploy the project to each machine, then use the panel on each machine to start or resume its assigned work. This workflow is designed for multiple machines you manage yourself, such as studio workstations and render nodes.

[Compare editions and follow the TrueODS Distributed workflow →](docs/EDITIONS.md#english)

Unbaked simulations, random or externally driven effects, and lighting or effects that need several frames to settle may still require caching, warm-up and checks at segment boundaries; see the [workflow notes](docs/EDITIONS.md#english). The base TrueODS edition and TrueODS Distributed are product editions; Fab's Personal and Professional price tiers are a separate distinction.

## Start rendering

1. Install the package matching your supported Unreal Engine version.
2. Add **True ODS Panoramic** to your Movie Render Queue job.
3. Set the resolution, output folder, and frame range in the plugin panel, then start rendering.

[Open the complete quick start →](docs/QUICKSTART.md#english)

## Documentation & support

| Need | Go to |
| :--- | :--- |
| Installation and first render | [Quick start](docs/QUICKSTART.md#english) |
| Edition comparison and multi-machine workflow | [Editions](docs/EDITIONS.md#english) |
| Output sizes, HDR, compatibility and resuming | [FAQ](docs/FAQ.md#english) |
| Technical help | [Support guide](docs/SUPPORT.md#english) · [trueodssupport@gmail.com](mailto:trueodssupport@gmail.com) |
| Support group | [Request to join the Telegram support group](docs/COMMUNITY.md#english) · Email proof of purchase to receive an invitation link |
| Availability and updates | [Release status](docs/CHANGELOG.md#english) |

This repository contains public product documentation and examples. Plugin packages are distributed separately. Demonstration scenes, characters and other third-party assets are not included with the plugin. Third-party software notices for the plugin (engine libraries such as OpenEXR and Imath, and code derived from ACES and the engine's tone curve) are in `THIRD_PARTY_NOTICES.txt` at the root of the plugin package.

<details>
<summary>Environment credits</summary>

Thanks to **Leartes Studios** for [Coffee Shop Environment](https://www.fab.com/listings/a0c7819e-a61d-4a19-8d3b-f0f5e584e6e0), and **Tirgames Assets** for [Sci-Fi Creatures Research Lab](https://www.fab.com/listings/c2494db5-55d2-4e35-9c57-540d0f2cf455).

</details>
