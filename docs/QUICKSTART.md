# TRUEODS Quick Start

**English** · [简体中文](QUICKSTART.zh-CN.md)
<a id="english"></a>

[Home](../README.md) · [Editions](EDITIONS.md) · [FAQ](FAQ.md) · [Support](SUPPORT.md)

This page follows the order you actually work in. The main path is:

**1. Install → 2. Add the MRQ settings → 3. Understand and set the parameters → 4. Render**

- The framing preview, the scene check (section 5) and test renders are optional; do them before you start the render in section 4.
- Section 6 covers resuming a stopped render.
- Section 7 covers the differences between the base TrueODS edition and TrueODS Distributed and between UE 5.7 and 5.8.

**On this page**

1. [Install](#en-install)
2. [Add the MRQ settings](#en-mrq)
3. [Understand and set the parameters](#en-parameters)
4. [Render](#en-render)
5. [Optional tools: framing preview and scene check](#en-optional)
6. [Resume a stopped render (both editions)](#en-resume)
7. [Edition and engine differences](#en-differences)

**Parameter sections:** [Setup](#en-setup) · [Format](#en-format) · [Stereo](#en-stereo) · [Resolution](#en-resolution) · [Output](#en-output) · [Look](#en-look) · [Anti-Aliasing](#en-anti-aliasing) · [Rendering](#en-rendering) · [Render Passes / Simulation / Performance / Diagnostics](#en-other-settings)

**About the screenshots:** every image below was captured from the plugin's real interface. Click an image to view it full-size.

- Sections 1–6 show UE 5.7, base edition; section 7 shows UE 5.8, TrueODS Distributed; all on Windows.
- Screenshots are cropped and private local paths are obscured. Parameter values are unchanged.
- The values are the defaults of a newly added **True ODS Panoramic** setting, which are the recommended settings.

Exceptions (the captions repeat them):

- **Manual EV100** and **Exposure Reference** show the exposure read from the captured scene. It differs from scene to scene.
- The yellow triangle at the end of the True ODS Panoramic row in the settings list is an MRQ notice caused by the capture project's own ray-tracing settings (see the caption in section 2).

Scope: Unreal Engine 5.7 and 5.8, the Windows 64-bit editor, DX12 / SM6, and Movie Render Queue (MRQ). The base edition and TrueODS Distributed share this workflow. Section 7 lists what TrueODS Distributed adds.

<a id="en-install"></a>

### 1. Install

1. Save the project and close Unreal Editor, then install the TRUEODS package that matches your UE version. Fab provides a separate package for 5.7 and for 5.8.
    - For a manual install, put the whole `TrueODS` folder in the project's `Plugins` folder.
    - Keep one installation only: not one copy in the project and another in the engine.

2. Open the project, search for **TrueODS** under **Edit > Plugins**, enable it and restart when asked.
    - The base edition is listed as **TrueODS Panoramic**; TrueODS Distributed is listed as **TrueODS Panoramic Distributed**.
    - The plugins it needs (Movie Render Pipeline, Niagara, Level Sequence Editor) are enabled with it.

3. After the restart, a **TrueODS** menu (base edition) or **TrueODS Distributed** menu appears to the right of **Help** in the menu bar. That means the plugin loaded.

    [![Menu bar: the TrueODS menu appears to the right of Help](../media/guide/qs-menu-trueods.png)](../media/guide/qs-menu-trueods.png)

    *UE 5.7 menu bar, base edition. TrueODS Distributed shows a TrueODS Distributed menu here.*

<a id="en-mrq"></a>

### 2. Add the MRQ settings

1. Open **Window > Cinematics > Movie Render Queue**, click **+ Render** and pick the Level Sequence. A job appears in the queue.

    [![Movie Render Queue: one job, Settings column shows Unsaved Config](../media/guide/qs-mrq-queue.png)](../media/guide/qs-mrq-queue.png)

    *The MRQ queue. Click Unsaved Config in the Settings column to open the job settings. Render (Local), bottom right, starts the render (section 4). The job in the capture already has True ODS Panoramic added and was renamed, so the Output column shows the output folder set by the plugin (a local path, obscured in the capture). A brand-new job shows MRQ's default output directory here.*

2. Click the link in the job's **Settings** column (**Unsaved Config** for a new job) to open the job settings window. **This window edits a temporary copy of the job:**
    - when you have finished the settings in section 3, click **Accept** at its bottom right to apply them to the job (section 4);
    - **Cancel**, or closing the window, discards every change;
    - while this window is open, MRQ's Render (Local) is greyed out.

3. Click **+ Setting** at the top left of the window and choose **True ODS Panoramic** in the **Rendering** group. It is the same plugin as TrueODS Panoramic in the plugin list. Here the name has spaces and is followed by a short summary in brackets.
4. **Keep only True ODS Panoramic and Output in the job settings.**
    - A new MRQ job comes with **Deferred Rendering** and **JPG Sequence**. Select each and press Delete.
        - True ODS Panoramic does all of the panorama rendering and output.
        - The plugin neither checks nor removes other render or output entries. If they stay, MRQ may also render and write ordinary images, which wastes render time and VRAM.

    - If you added MRQ's own **Anti-aliasing** entry (a separate item in the settings list, not the Anti-Aliasing section of the plugin panel), you can delete it too.
        - When the plugin needs one, it adds it by itself at the start of the render, for simulation warm-up (see **Simulation Warmup Frames**).
        - Panorama anti-aliasing is set in the plugin panel's Anti-Aliasing section. Deleting MRQ's entry does not turn it off.

5. **Keep MRQ's own Output setting, but leave it as it is.** It is a separate entry in the settings list; do not delete it. The plugin fills in its key fields:
    - **Output Directory** is set to the plugin panel's output folder, or to `<project folder>/TrueODS/Renders` when that field is empty.
    - **Use Custom Playback Range / Custom Start Frame / Custom End Frame** are set to the frames to render. The start frame is earlier than your first frame (-64 in the picture below). Those extra frames are reserved for the plugin's warm-up (see "three different warm-ups" at the start of section 3) and are never written as images.
    - **Output Resolution** is changed to 256×256 by the plugin when a render starts. This is not the panorama size; the plugin panel sets that, so leave it.
    - **File Name Format** is not used for the panorama files. Panorama file names are set in the Output section of the plugin panel.

    If you change the directory or the range here, the plugin sets them back at once. Leave the other fields at their MRQ defaults as well:

    - **Use Custom Frame Rate / Output Frame Rate**, **Output Frame Step** and **Handle Frame Count** are MRQ's own frame controls. The plugin does not read them, but changing them can make the frames MRQ renders differ from the plugin panel's range.
    - **Frame Number Offset**, under Output's **Advanced**, is added to the panorama frame numbers. Keep it at 0.

    [![Job settings: only True ODS Panoramic and Output are left; on the right, MRQ's own Output setting as filled in by the plugin](../media/guide/qs-mrq-own-output-setting.png)](../media/guide/qs-mrq-own-output-setting.png)

    *Left: the settings list holds only True ODS Panoramic and Output. Right: what MRQ's own Output setting shows when it is selected.*

    - *The plugin wrote Output Directory, Custom Start Frame (-64) and Custom End Frame; the ↺ marks show they are no longer MRQ's defaults. Output Resolution is only rewritten when a render starts, so it still shows 1920×1080 here.*
    - *The yellow triangle after True ODS Panoramic in the list is an MRQ validation notice.*
        - *It appears only when a project has ray tracing on but ray-traced shadows off, as the captured project does, and it says that ray-traced shadows are off.*
        - *In that situation it also reminds you to press Use Scene Exposure if the exposure has not been read yet.*
        - *It may not appear in your project, and it does not block the render.*

6. **Don't mix up the two things called "Output":**
    - **MRQ's own Output setting** is the separate entry above. Keep it, but leave it alone.
    - **The Output section of the plugin panel** is one group of True ODS Panoramic's parameters. Set the output folder, frame range, file names and image format there (section 3).

<a id="en-parameters"></a>

### 3. Understand and set the parameters

Select **True ODS Panoramic** in the settings list. All of the plugin's parameters appear on the right; this guide calls that area the "plugin panel". **The defaults are the recommended settings.** On a first job you usually only need to check two things:

- the output folder and frame range in the **Output** section;
- the exposure in the **Look** section.

**Checking the exposure:** when the plugin panel first opens, the plugin reads the exposure of the current viewport once. The value it read is shown in **Exposure Reference** in the Look section.

- If the viewport brightness is what you want, leave it.
- To set the exposure from another viewing direction, point the viewport in that direction and press **Use Scene Exposure** in Look.
- If it reads "No viewport exposure yet…", nothing was read yet; see the Look section below.

**The panel has three different "warm-ups":**

1. **Simulation warm-up** (Simulation Warmup Frames) lets particles and weather run before output, without writing images.
2. **Frame warm-up** (Warm Up Before First Frame / Warm Up Frames) renders frames and throws them away, so fog, volumetric light and indirect light are already settled in the first written frame.
3. **Warm-up views** (Warm-up Pages / Warm View Resolution Cap) only affect VRAM use, not the picture.

The 64 extra frames in MRQ's Output are reserved for the second and third kinds.

The parameters below follow the plugin panel from top to bottom. Each entry says what it controls, its default, when to change it, and whether changing it affects image quality, render speed or video memory (VRAM).

- A greyed-out row has no effect with the current settings.
- For rows that appear only under certain conditions, those conditions are noted below.
- Hidden and experimental properties that are not in the panel are left out.

[![All True ODS Panoramic parameters at their defaults](../media/guide/qs-panel-overview.png)](../media/guide/qs-panel-overview.png)

*The whole plugin panel at its defaults, from Setup to Diagnostics.*

- *The Advanced groups of Rendering and Performance are expanded in this capture; the Anti-Aliasing Advanced group and Diagnostics are collapsed.*
- *Manual EV100 and Exposure Reference show the exposure read from the captured scene.*
- *The yellow triangle is explained in the caption in section 2.*
- *The images below show each section.*

<a id="en-setup"></a>

#### Setup

[![Setup, Format, Stereo and Resolution sections (defaults)](../media/guide/qs-panel-1-setup-format-stereo-resolution.png)](../media/guide/qs-panel-1-setup-format-stereo-resolution.png)

*Setup, Format, Stereo and Resolution, at their defaults.*

- **Restore Recommended Settings** (button): puts the plugin's settings back to the recommended values.
    - **Not** restored: the output folder, file name format, frame-number digits, Render Whole Sequence and the frame range, the Look LUT, and the exposure already read from the scene. TrueODS Distributed also keeps Require This Checksum.
    - Press it when settings got mixed up or you reuse an old preset. Undo (Ctrl+Z) reverts it.

<a id="en-format"></a>

#### Format

- **Format** (default **360 (full sphere, 2:1)**): a full 360° sphere, or the front hemisphere only, **180 (front hemisphere)**.
    - With 180 and a resolution preset, each eye is square; the 8192 preset gives 4096×4096 per eye. When you choose 180, also set Stereo Layout (Stereo section) to Side by Side.
    - With 180 the plugin renders only the half of the view that the front hemisphere needs, so a frame takes about half as long as 360, and the output image is smaller.

- **Projection** (default **Equirectangular**):
    - **Equirectangular**: the one-image-per-eye layout VR headsets and 360 players expect.
    - **Cubemap Faces**: writes the faces of a cube as separate images instead, 6 for 360 and 5 for 180, mono or stereo.
        - Cube faces are named and written by their own rules, not by the Output section's file-name and image-format settings. Try a short range first to check they fit your pipeline.
        - With Cubemap Faces, HDR Tone Chain below is hidden.

- **Match Viewport Color** (default on): the panorama uses the scene's own tone mapping and post-process colour, matching the viewport. Off switches to the old fixed filmic curve. It affects colour; you rarely need to turn it off.
- **HDR Tone Chain (recommended)** (default on; shown only when Format is 360 or 180, Projection is Equirectangular and Renderer is Deferred):
    - On: the whole panorama is assembled in HDR and tone-mapped once. The EXR is a true linear master; grade it as a Linear input. The PNG / JPG / TIFF carry the finished look.
    - Off: the old 8-bit processing, and the EXR is no longer scene-linear.
    - Keep it on. With it on, VRAM use may be slightly higher.

<a id="en-stereo"></a>

#### Stereo

- **Stereo** (default on): two-eye stereo. Off renders a mono panorama; half as many eyes are rendered, so it takes roughly half the time.
- **Stereo Layout** (default **Top / Bottom**; shown only with Stereo on): only changes how the two eyes are packed. Quality and speed are unaffected.
    - **For 360, Top / Bottom is recommended:** at 8192 the image is 8192×8192 (8192×4096 per eye).
    - **For 180, Side by Side is recommended:** at 8192 the image is 8192×4096 (4096×4096 per eye). The default is Top / Bottom, so switch to Side by Side yourself when you choose 180.

    Use a different layout if your player or post-production pipeline requires it.

- **IPD (cm)** (default 6.5; shown only with Stereo on): the distance between the eyes, which sets how strong the depth is. Larger values give more depth and make close objects less comfortable to view. No effect on speed.
- **Pole Merge Angle** (default 60; shown only with Stereo on): the latitude from which the two eyes blend towards mono, fully merged straight up and straight down. Full stereo is uncomfortable in a headset when you look up or down. 90 turns the blend off. Affects viewing comfort only.
- **Far Eye Merge (px, experimental)** (default 0 = off; shown only with Stereo on): experimental; leave it at 0.
    - The number is a distance threshold measured in output pixels. Distant content whose left/right offset is at most this many pixels uses the same image for both eyes, so the far background matches more closely between the eyes.
    - Rough guide at 8192 per eye and an IPD of 6.5 cm (horizontal direction): 1 ≈ beyond 85 m, 2 ≈ beyond 42 m, 4 ≈ beyond 21 m, 8 ≈ beyond 11 m.
    - The cost: in the merged distant areas, water, glass and reflections get the wrong stereo depth, and each frame takes more render time and VRAM.
    - It only affects Equirectangular output.

<a id="en-resolution"></a>

#### Resolution

- **Resolution Preset** (default **8192 / eye (Recommended, ~22.8 px/deg)**): width per eye. Choose 4096, 6144, 8192, 12288 or 16384, or type your own. Higher is sharper; render time grows roughly with the pixel count, and VRAM use goes up.
- **Output Width Per Eye** (default 8192): the actual width per eye.
    - A preset fills it in. Typing a value switches the preset to **Custom (manual)**, which is expected.
    - For 360 the height is half the width; for 180 it equals the width.

- **Supersample** (default 150%): renders at a higher internal resolution and scales down. Higher gives cleaner edges but is slower and uses more VRAM.
- **VRAM Mode** (default **Paced (recommended)**): how hard the plugin works to keep the render inside your video memory. **The picture is identical at every setting.** Textures are always fully loaded; only temporary render memory and speed change.
    - When to change it: a render fails for lack of video memory, or heavier frames suddenly take several times longer (the GPU has started borrowing system memory). Move down one step and render again.
    - **Tiled 2x2 / 3x3 / 4x4** cut peak temporary VRAM to about 1/4, 1/9 or 1/16. Rendering gets much slower; the setting's tooltip estimates about 4, 9 or 16 times as long.
    - How to choose (the tooltip's guide):
        - Paced for 24 GB+ cards at 8K, or 12–16 GB cards at 4K;
        - Tiled 2x2 for 12–16 GB cards at 8K;
        - 3x3 to 4x4 for 8–12 GB cards.

    - **Normal** does no memory management; use it only when your GPU has plenty of VRAM to spare.

- **Warm-up Pages** (default 0 = automatic: 4 with Normal, Paced or Tiled 2x2; 9 with Tiled 3x3; 16 with Tiled 4x4): lowers the VRAM peak at the start of the render, at almost no time cost.
    - Higher values lower that peak further, and the start frame in MRQ's Output moves earlier; this is expected.
    - If Paced still runs out of memory but Tiled is too slow, try a higher value here, for example 8 (maximum 16). Paced already uses 4 automatically, so entering 4 changes nothing.

- **Apply Recommended Render Settings** (default on): for the length of the render, the engine settings a panorama needs are applied, then put back afterwards, so the project itself is not changed. They are:
    - motion blur off;
    - dark edges where objects meet removed (this is not lens vignetting);
    - shadow sharpness and ray-traced shadow samples tuned;
    - texture loading boosted.

    Turn it off only if one of these settings causes problems in your scene. Six manual rows then appear:

    - **Disable Motion Blur**
    - **Remove Dark Halos**: removes the dark edges and halos where objects meet each other, walls or the floor; it is not lens vignetting. Indirect light loses a little detail.
    - **Sun Shadow Sharpness**
    - **Lamp Shadow Sharpness**
    - **RT Shadow Samples Per Pixel**
    - **Boost Texture Streaming**

    Note that with it off, the plugin still writes the shadow sharpness and ray-traced shadow sample values from these rows; it does not fall back to the project's own settings. A shadow sharpness of 0 is written as 0 (the tooltip's "0 leaves the project's setting alone" is not what happens). Sharper shadows and more samples cost time and VRAM.

- **Warm View Resolution Cap** (default 256): lowers peak VRAM at high output resolutions. 0 = no cap, highest VRAM use. The picture does not change either way; keep the default.

<a id="en-output"></a>

#### Output (the Output section of the plugin panel)

[![Output and Look sections (defaults)](../media/guide/qs-panel-2-output-look.png)](../media/guide/qs-panel-2-output-look.png)

*Output and Look, at their defaults. Manual EV100 and Exposure Reference show the exposure read from the captured scene; they are not defaults.*

- **Panorama Output Folder** (default empty): where finished frames are written.
    - Empty writes to `<project folder>/TrueODS/Renders`. The tooltip says Saved/TrueODS, but the files go to the folder above.
    - Use a new, empty folder for each job; resuming works from this folder's records (section 6).
    - When this folder holds a render that did not finish, a resume notice and a **Resume Render** button appear under it; see section 6.

- **Render Whole Sequence** (default on): renders the whole Level Sequence. To render part of it, turn this off and fill in the two rows below. The plugin arranges the warm-up frames the render needs before the first frame; MRQ's own Output needs no change.
- **First Frame / Last Frame** (default 0 / 0): the first and last frames written, in sequence frame numbers, both included. Greyed out while Render Whole Sequence is on.
- **File Name Format** (default `TrueODS.{frame_number}.equirect`): the name of each frame, without extension. MRQ placeholders such as `{sequence_name}`, `{map_name}` and `{date}` work. If the name has no frame number, one is added at the end.
- **Frame Number Digits** (default 1 = no zero padding: 0, 1, 2 …): the minimum number of digits of the frame number in each file name; shorter numbers get leading zeros. Some editing tools only recognise an image sequence when every frame number has the same number of digits; in that case set 4 or more, enough for your longest sequence.
- **Image Format** (default **PNG (8-bit)**): the viewable image written per frame. The panorama does not use MRQ's own output formats; choose the format here only.
    - **PNG**: lossless, 8-bit.
    - **JPG**: small files, quality 95.
    - **TIFF**: 16-bit, uncompressed, large files.
    - **None (EXR master only)**: no viewable image, EXR only.

- **Also Write EXR (HDR master)** (default on): also writes a linear, ungraded HDR master `.exr`. Grade from this file; the PNG / JPG / TIFF beside it is only a version with the preset look applied.
- **EXR Compression** (default **DWAB**; shown only when EXR is written): DWAB gives small files with no visible loss at normal grading exposures, but it is lossy. Choose ZIP or PIZ for lossless files, several times larger. None is larger still.
- **Render With Editor Closed** (default on): when the render starts, the editor closes, a background process renders, and a progress window opens.
    - It saves memory. On a large scene at high resolution, the editor's own memory usage is what most often causes the machine to run out of memory.
    - The picture is identical to an in-editor render.
    - The editor closes, so check every setting before you start. Turn this off to render inside the editor.

- **Save Modified Content First** (default on; shown only when Render With Editor Closed is on): whether modified levels and assets are saved before the editor closes.
    - On: they are **saved without asking**. Off: the editor closes without saving and those edits are lost.
    - **If saving fails, the render does not start.** The editor stays open and the Output Log shows `[TrueODS][EDITOR_CLOSED] saving modified content FAILED`. Save by hand and fix whatever blocked the save, or turn this off if the changes can be discarded, then start again.

<a id="en-look"></a>

#### Look

- **Bake Film Look Into EXR Master** (default off): off keeps the full highlight range in the EXR master for grading. On bakes the film curve into it; bright areas flatten and cannot be recovered in grading. Turn it on only for a scene that looks wrong without it.
- **Exposure** (default **Manual EV100**): where the panorama's brightness comes from.
    - **Auto**: uses the shot camera's own exposure, but also meters the scene and switches to the metered value when the two differ by more than Auto Exposure Tolerance. The log says which one it used.
    - **Match Viewport**: locks to the exposure you see in the editor viewport now.
    - **Camera / Scene**: always the camera's or post-process volume's own exposure.
    - **Neutral Auto Meter**: always meters again and ignores the scene's exposure settings.
    - **Manual EV100**: uses the fixed value below.

    The drop-down marks Auto as "recommended", but in this version the default is Manual EV100, filled in from the scene (next two rows).

- **Exposure Reference** (read-only): the viewport exposure read since the panel was opened, for example "EV100 -1.01 = viewport now (+1 = one stop darker)". After you reopen the panel this row can be empty. That does not mean the exposure was lost; the value in Manual EV100 still applies.
- **Scene Exposure → Use Scene Exposure** (button): reads the exposure of the current perspective viewport into Manual EV100, with no render needed.
    - Before pressing it, point the viewport in the direction you want to use as the exposure reference.
    - Afterwards, check Exposure Reference. "No viewport exposure yet…" means nothing was read yet: click in the viewport so it renders a frame, then press again.
    - The first time the panel opens, if Exposure is Manual EV100 and nothing has been read yet, the plugin reads the viewport once by itself. That is why Manual EV100 in the capture shows the captured scene's value.

- **Auto Exposure Tolerance (stops)** (default 1.0): only used when Exposure is Auto; greyed out otherwise. It sets how many stops the camera's exposure may differ from the metered value before Auto switches to the metered value. Raise it if Auto overrides a deliberately dark or bright look.
- **Manual EV100** (shown only when Exposure is Manual EV100): the fixed exposure. Lower is brighter; +1 is one stop darker.
    - It is the plugin's own scale and does not add the scene's Exposure Compensation, so do not copy the number from a camera's post-process settings.
    - Normally read it with the button above rather than typing it.

- **Exact Exposure** (default on): makes the finished frames land exactly on the exposure set above. It costs a moment once at the start of the job and nothing per frame. Keep it on.
- **Output Look LUT (.lut1d.txt)** (default empty = the LUT bundled with the plugin): applies to PNG / JPG / TIFF only and **never** to the EXR. Set a path to use your own look.
- **Global Tone Creative Exposure (EV)** (default 0; shown while HDR Tone Chain is ticked): adjusts the overall brightness of the PNG / JPG / TIFF only, in stops. **+1 is one stop brighter**, the opposite direction to Manual EV100.
    - The EXR master's pixels are not changed; the value is only recorded in the EXR header.
    - With Cubemap Faces or Path Tracing, the HDR Tone Chain row is hidden but this row stays visible. It then has no effect.

<a id="en-anti-aliasing"></a>

#### Anti-Aliasing

[![Anti-Aliasing and Rendering sections (defaults)](../media/guide/qs-panel-3-antialiasing-rendering.png)](../media/guide/qs-panel-3-antialiasing-rendering.png)

*Anti-Aliasing and Rendering, at their defaults.*

- *The Anti-Aliasing Advanced group is collapsed and holds only Near Clip Distance (cm). The Advanced group of Rendering is expanded in this capture.*

- **Anti-Aliasing Method** (default **TSR (recommended)**; shown only with the Deferred renderer):
    - **TSR**: steady highlights and indirect light, matching the viewport.
    - **TAA**: a little noisier, but may handle fast motion better in some cases.
    - **Off**: fastest, but highlights and indirect light shimmer from frame to frame.

- **Full Quality On Every Frame** (default on; Deferred only): gives every frame the full quality of the first. It costs extra time when Samples Per Pane is 2 or more. Off makes later frames faster but can show a square glow near volumetric-fog lights.
- **Advanced > Near Clip Distance (cm)** (default 0): how close to the camera something can be and still be drawn. 0 keeps almost everything (it is treated as 1 cm) and is right for almost every shot. Raise it only to leave out something touching the lens, such as a shoulder or a prop.

<a id="en-rendering"></a>

#### Rendering

- **Analyze Scene & Sequence (Pre-flight)** (button): the scene check in section 5. Run from this button, it does **not** check the sequence. To check the sequence too, use the menu entry of the same name.
- **Renderer** (default **Deferred (recommended)**):
    - **Deferred** is fast and right for almost every job.
    - **Path Tracing** gives reference-quality light and reflections, but takes hours per frame instead of minutes. Path Tracing must already be enabled in the project, or the job stops.

    Both renderers produce correct stereo. With Path Tracing, these rows are hidden:

    - Samples Per Pane and Thin Detail Stability;
    - Warm Up Before First Frame, with its Warm Up Frames / Warm Up Sparse Stride / Warm Up Full Frames rows;
    - Anti-Aliasing Method, Full Quality On Every Frame and HDR Tone Chain.

    Warm Up From Sequence Start and Sequence Efficiency stay. Two new rows appear:

    - **Path Tracing Samples Per Pane** (default 64): more samples, less grain; time grows roughly in proportion.
    - **Path Tracing Denoiser** (default Project): whether to denoise.

- **Samples Per Pane** (default 2; Deferred only): the sample count.
    - 2 keeps hair, wires, railings and foliage clean, and Thin Detail Stability needs it.
    - 1 is faster and looks the same on scenes without thin detail.
    - Render time increases with the sample count, but less than proportionally.

- **Warm Up Before First Frame** (default on; Deferred only): renders and throws away up to Warm Up Frames frames first, so the first written frame looks like the frames after it. The difference shows most in fog, volumetric lighting and indirect light. Resume blending also depends on it (section 6).
    - The warm-up uses the sequence frames before the start of the range; it can only use as many as exist.
    - With the default Render Whole Sequence, the range starts at the sequence's first frame, so nothing comes before it. No warm-up time is spent, but the first written frame can differ slightly from the rest.
    - If that frame matters, extend the sequence's playback range earlier in Sequencer (by up to Warm Up Frames frames) so there are enough frames before the range.

- **Sequence Efficiency** (default on): faster when rendering a run of frames; the picture is unchanged. Makes no difference for a single frame.
- **Warm Up Frames** (default 0 = 32 frames; shown only when Warm Up Before First Frame is on and the renderer is Deferred): the maximum number of frames rendered and thrown away before the first written one. Each costs one frame of render time.
- **Warm Up Sparse Stride** (default 0 = off; shown under the same condition as Warm Up Frames): makes the frame warm-up cheaper; higher values save more time, and it needs 2 or more to take effect. Written frames are not affected.
- **Warm Up Full Frames** (default 4; shown under that condition when Warm Up Sparse Stride is above 0): while Warm Up Sparse Stride is in effect, how many of the last warm-up frames are still rendered at full quality.
- **Warm Up From Sequence Start** (default on): plays the sequence from its first frame up to the render range, so effects that build up over time (smoke filling a room, growing foam) look as they would in one continuous render. It costs about half a second per sequence frame before the range, and nothing when the range starts at the sequence's first frame.
- **Texture Sync Per Pane** (default on): stops an occasional soft, low-resolution patch of texture from being baked into a frame. The problem comes and goes, so one clean test render does not show that turning this off is safe. It costs very little; keep it on.
- **Render Report** (default on): writes `trueods_advisory.md` (plus a `.jsonl`) into `_metadata` in the output folder. It only records; it changes nothing in the scene and costs no render time. It lists:
    - the number of cloth and hair components in the scene;
    - simulations whose result can differ from render to render (under the heading "Cross-machine consistency", which also appears for single-machine renders);
    - for each frame, how many texture loads the renderer had to wait for and the viewing directions where late-loading textures may appear soft.

    Low-VRAM warnings are not in this report; they appear only in the log and the progress notification.

- **Advanced > Thin Detail Stability** (default on; shown only with Deferred and Samples Per Pane above 1): removes shimmer and crawling from thin geometry.
    - The trade: objects very close to the camera can get a faint double image and slightly softer surface detail.
    - Which is better depends on the shot; if unsure, render one frame each way and compare.
    - No extra render time.

<a id="en-other-settings"></a>

#### Render Passes, Simulation, Performance, Diagnostics

[![Render Passes, Simulation, Performance and Diagnostics sections (defaults)](../media/guide/qs-panel-4-passes-simulation-performance-diagnostics.png)](../media/guide/qs-panel-4-passes-simulation-performance-diagnostics.png)

*Render Passes, Simulation, Performance and Diagnostics, at their defaults. Performance's Advanced group is expanded in this capture; Diagnostics is collapsed.*

- **Render Passes**: shows only the text "In development"; this feature is not available yet and has nothing to set.
- **Simulation Warmup Frames** (default 90): frames simulated before the first written frame.
    - Particles, weather and falling debris start empty in an offline render and need simulation time to look as they do on screen.
    - It runs once per job, so a continuous frame range stays continuous.
    - For static scenes, 0 is fastest.

- **Performance > Advanced:**
    - **Panes Per Batch** (default 4): 4 is fastest; lower it only if your GPU driver becomes unstable. The picture is the same at any value.
    - **Assemble In Background** (default on): assembles the finished panorama while the next frame renders, saving time with an identical result.
    - **Texture Memory Scale** (default 0 = automatic): raise it by hand only if surfaces stay soft or visibly swap detail during a render; it costs VRAM.

- **Diagnostics > Advanced:** **Dump GPU Memory Per Frame** and **Log Texture Memory Each Frame** are off by default. They are for troubleshooting only and make logs very large; keep them off.

<a id="en-render"></a>

### 4. Render

0. **(Optional) Before you click Render:**
    - If you want the framing preview or the scene check, do it now; see section 5.
    - For a complex scene, you can turn off **Render Whole Sequence** and render a few frames first to check exposure, seams, thin detail and stereo depth. If VRAM is a concern, also watch peak VRAM and time per frame.
        - With the defaults, a test render also closes the editor, and the MRQ queue is empty when you reopen the project.
        - So save a preset first (next step), or turn off Render With Editor Closed for the test.

    - None of this is required.

1. **Save a preset first (recommended):** click the preset button at the top of the job settings window (it reads **Unsaved Config** for a new job) and choose **Save As Preset** to save it in the project. The queue is empty when you reopen the project, and the preset brings the settings back. (Resuming does not need it: the plugin keeps the settings with the frames; see section 6.)
2. Click **Accept** at the bottom right of the job settings window. The window closes and the settings are applied to the job.
3. Click **Render (Local)** at the bottom right of the MRQ window. With the defaults, this happens in order:
    1. every modified level and asset is **saved without asking** (Save Modified Content First);
    2. the editor closes and a background process renders;
    3. a progress window opens.

    The editor does not reopen by itself when the render ends; reopen the project when you need it.

4. When the render finishes, the output folder holds:
    - one PNG and one EXR per frame, named `TrueODS.0.equirect.png` / `TrueODS.0.equirect.exr` and so on by default. At the 360 top/bottom 8192 setting, each image is 8192×8192 (8192×4096 per eye);
    - a `_metadata` folder with the progress record, the per-frame records and a copy of the job's settings (used for resuming), and `trueods_advisory.md` from Render Report;
    - `TrueODS_render_report.txt`: a short summary (frames, times, the exposure used) written when a background render reaches its last frame. In build 72, the plugin opens this text file with the Windows default application for .txt files, usually Notepad.

    View the images in a stereo-360 player or VR headset set to the matching layout (for example top/bottom).

<a id="en-optional"></a>

### 5. Optional tools before rendering: framing preview and scene check

#### Equirect Preview (framing preview)

**What it does:** shows a live panorama of everything around the camera inside the editor, including behind, above and below it, so you can check the framing before rendering.

**How to open it:** menu bar **TrueODS** (base edition) or **TrueODS Distributed** **> Preview > Equirect Preview**. It opens a separate window called **TrueODS Preview**. You can also type `TrueODS.OpenPreview` in the console. Both editions and both engine versions have it.

[![Equirect Preview window (defaults)](../media/guide/qs-equirect-preview.png)](../media/guide/qs-equirect-preview.png)

*The Equirect Preview window at its defaults, following the level viewport and showing the full 360°.*

**What it is not:**

- It is a mono, low-resolution framing preview, not a sample of the final render.
- It does not read any True ODS Panoramic setting (format, stereo, resolution, exposure, Look LUT), so detail and brightness differ from the final render.
- It adds nothing to your scene and does not modify the level.

**Options in the window** (left to right):

- **Source** (default **Auto (Viewport Camera)**): follows the level viewport. When the viewport is locked to Sequencer's camera cuts, the preview follows the shot camera and changes as you scrub. The drop-down lists every camera in the level; pick one to pin the preview to it (shown as "Pinned: <camera name>").
- **Area** (default **All Directions (360° / 2:1)**): shows the full 360°. **Front Half Only (180° / Square 1:1)** shows only the hemisphere the camera faces, to check what a 180 frame contains. It affects the preview only, not the render output.
- **Keep Horizon Level** (default on): keeps the picture level and follows only the camera's left/right turn, as the final render does. Off follows the camera's tilt and roll, which the final render does not.
- **Display mode** (default **Workbench**):
    - **Workbench**: surface colours only, no lighting, animation updates live, fastest. Fog, clouds and translucent surfaces such as glass and water are not shown.
    - **Viewport Unlit**: the unlit look, animation updates live. Fog and clouds are not shown.
    - **Fast (No Dynamic Shadows)**: lit, but without shadows, reflections, volumetric fog, clouds and similar effects.
    - **Quality**: full lighting, closest to the final picture, and the slowest.

    Fast and Quality keep showing the last image during playback or camera movement, then refresh when playback or movement stops.

- **Pause Viewport** (default on): pauses the level viewports' continuous redraw while the preview is open, to spare the GPU. The viewport still redraws while you work in it, and everything returns to normal when the preview closes.
- **Resolution** (default **256 / face**; 256, 512 or 1024): higher is sharper and uses more of the GPU.
- **Extra EV** (default 0): brightens or darkens the preview on top of the scene's own exposure; it affects the preview only. Not available in Workbench and Viewport Unlit.

**Notes:**

- While it is open, the preview keeps using some GPU resources; close the window when you do not need it.
- With the default Render With Editor Closed, the editor and the preview close when the render starts. If you turned that off to render inside the editor, close the preview first.
- In Quality mode, fog and lighting are only approximate, and blocky light/dark edges can appear. This is a limitation of the preview; the rendered frames are what count.
- These options return to their defaults when the editor restarts.

#### Analyze Scene & Sequence (Pre-flight) (scene check)

**What it does:** a **static settings scan** of the open level and of the queue job's Level Sequence, looking for known conditions that affect panorama renders.

- It renders nothing and compares no pixels.
- It usually takes a few seconds. A large level with many lights and sequence bindings can take a minute or more, and the editor does not respond meanwhile.
- Only loaded sub-levels are checked; unloaded parts are not.

**Where to run it:**

- **Recommended:** the menu bar, **TrueODS** (base edition) or **TrueODS Distributed** **> Analysis > Analyze Scene & Sequence (Pre-flight)**.
    - It uses the first enabled TrueODS job in the queue and checks that job's sequence too. So click Accept in the job settings window first, so that the job really contains True ODS Panoramic.
    - With no such job, it checks only the open level.

- The plugin panel's Rendering section has a button with the same name. MRQ's settings window edits a temporary copy of the job, so from there the sequence is **not** checked, and the report begins with `Sequence: none`.

Before running it:

- open the job's level;
- open the sequence in Sequencer with the playhead inside the render range, so the effects and objects the sequence spawns exist in the scene and get checked.

**What it checks:** the report lists findings in five groups; groups with nothing to report are left out.

1. **Un-baked simulation**: cloth, hair, Niagara effects, physics, destruction, legacy Cascade particles and similar. These can come out differently on every render, so a resumed or repeated render may not match the same frame. The report suggests baking a cache or fixing the random seed.
2. **Things that will not render under default settings, or are handled for you:**
    - **Volumetric fog reminder:** shown whenever the level has volumetric fog and at least one light. It does not check whether emissive materials exist. It means: emissive materials do not produce light shafts in volumetric fog, so if you need a shaft, add a real light with Volumetric Scattering Intensity above 0. The heading says how many lights in the level can produce shafts.
    - **Per-vertex fog on large translucent surfaces:** with the default settings, these are switched to per-pixel fog during the render, and your assets are not changed. Materials driven by a Dynamic Material Instance cannot be corrected, though. The report lists them separately so you can tick Compute Fog Per Pixel on their root material.
    - **Fog volumes:** a fog volume within 1–2 m of the camera is only approximate; when the camera passes through a hard-edged fog volume, its boundary shows. Soften the volume's edges, or keep the camera outside it.
    - **Translucent materials on Nanite meshes:** these draw nothing.

3. **VRAM / RAM risk**:
    - When the check runs, more than half of the first NVIDIA GPU's memory is already in use. This counts the whole system, including the editor's own share, and is read with nvidia-smi (NVIDIA only).
    - Less than 16 GB of RAM is free.

4. **High-cost frames**: frame ranges of the sequence with heavy effects, which can be several times slower and use more VRAM.
5. **Materials that read the camera**: materials that use the screen position or the camera direction give different results in different directions of the panorama.

**Where the report is:**

- **Notification:** when run from the menu on a queue job, or from the panel button, an editor notification shows for about 8 seconds, for example "Pre-flight: 3 un-baked / 3 not-rendering / 0 memory / 0 cost / 1 camera-dependent (0.1s)". The numbers are the groups of findings in the five categories above, followed by the scan time. With no TrueODS job in the queue, the menu entry shows no notification and just opens the report.
- **Full report:** in `<project folder>/TrueODS/Reports/SceneAnalysis/scene_analysis_<date>_<time>.md`, plus a `.json` with the same name for tools. The `.md` opens in your default app.
- **Log:** a `[TrueODS][PREFLIGHT]` line in the Output Log.

[![Notification after pre-flight: number of finding groups per category](../media/guide/qs-preflight-toast.png)](../media/guide/qs-preflight-toast.png)

*The notification after pre-flight (result for the captured scene).*

**Does it change the scene?** No. It only reads settings. Apart from writing the two report files, it changes no level, asset or project setting, and it fixes nothing automatically.

**What to do with the findings:** each item is a candidate to look at, not a confirmed defect. For each one, decide:

- fix it, for example:
    - bake a cache;
    - pair a light shaft with a real light;
    - make the material opaque or masked;
    - have the material read world directions;
    - tick Compute Fog Per Pixel on the root materials the report lists as dynamic instances;

- or accept it if it does not matter for your shot.

Run the check again after changes. **A clean report does not guarantee a correct render.** It does not check exposure, ray-tracing settings or the plugin panel's settings; the rendered frames are what count. A few sentences in the base edition's report mention "other render machines"; ignore them when rendering on one machine.

<a id="en-resume"></a>

### 6. Resuming a stopped render (both editions)

If a render stops part-way (the process was closed, the power went out, a frame failed), the plugin can carry on instead of starting over. Every time a render starts, the plugin keeps a copy of the job's settings in the output folder's `_metadata`, and resuming puts them back.

1. **Open the settings of any job.** After you reopen the project the MRQ queue is usually empty; add a job. Any level and sequence will do: Resume Render switches the job back to the level, sequence and settings of that render. Keep only this job in the queue: Resume Render renders the whole queue, as Render (Local) does, and with several jobs in the queue it may not be able to tell which one to replace.
2. **Point the output folder at that render.** In the **Output** section of True ODS Panoramic, set **Panorama Output Folder** to that render's output folder (leave it empty if it was empty). When the folder holds a render that did not finish, a yellow notice appears under it: how many frames are done, the frame it stopped before, and whether its settings were kept with the frames.

    [![Resume notice in the Output section](../media/guide/qs-resume-output.png)](../media/guide/qs-resume-output.png)

    *The resume notice and choices in the Output section when the output folder holds a render that stopped before frame 2 (UE 5.7, base edition). The button stays grey until you choose a resume option.*

3. **Choose how the stop point is joined** (you choose every time; there is no default):
    - **Render again and blend in the last X frames before frame N:** renders the X frames before frame N again and writes each as a blend with the frame already there, the new render's share rising frame by frame (25%, 50%, 75% for X = 3). From frame N on, every frame is new.
        - A larger X joins more smoothly. X can be at most the number of finished frames directly before frame N.
        - Cost: X extra frames. If finished frames follow the missing ones, the frames where they start are blended back the same way, about 2X frames in all. The notice shows the extra frames and an estimate from that render's time per frame.
        - The frames as they were before blending are kept in `_resume_originals` in the output folder.
        - Frames written only as 16-bit TIFF, without the EXR master, cannot be blended; this choice is then unavailable.

    - **Continue at frame N without rendering earlier frames again (warm-up only):** renders no extra frames and starts at frame N.
        - Risk: frame N starts without the frames before it, so lighting, fog and reflections can change visibly from frame N−1 to frame N. If finished frames follow the missing ones, the same can happen where they start.

    - **Render every frame again (the scene changed):** shown only when the level, one of its sublevels or the sequence was saved after the frames were rendered; the notice names the file. Renders the whole range again and replaces the frames already done; the notice gives an estimate from that render's time per frame.
        - Choose it when the scene did change; otherwise the frames already done would not match the new ones. A save without changes can be ignored: choose one of the two choices above as usual.

    - With Path Tracing there is no choice; the render continues at the first missing frame.

4. **Click Resume Render.**
    - The job's settings are replaced with the ones kept with that render (level, sequence, True ODS Panoramic and MRQ's own settings), the settings window closes, and the queue starts the same way as **Render (Local)**. With Render With Editor Closed on, the editor closes as usual.
    - Finished frames whose records match are skipped; missing frames are rendered.
    - Other enabled jobs in the queue render too, as with Render (Local).
    - When the button is grey, the reason is shown next to it, for example no way to join chosen yet, a background render still running on this machine, or the render wrote to the folder less than two minutes ago (it may still be running).

**Notes:**

- Before resuming, do not change the sequence or the scene. The plugin checks each frame's settings record; frames whose record does not match are rendered again. For the level, its sublevels and the sequence it only checks whether they were saved after the frames were rendered (the third choice above); other assets such as materials and textures are not checked, and neither are unsaved changes.
- Do not delete frames already written, or the `_metadata` folder inside the output folder (it holds the progress record, the per-frame records and the copy of the job's settings).
- Output from older plugin versions has no copy of the settings: the notice says "settings were not kept", and Resume Render uses the settings in the settings window. They must then match that render exactly (for example, by importing the preset saved for that render); frames that do not match are rendered again.

**TrueODS Distributed multi-machine jobs:** after a stop, use **Resume Render Job** in step 4 of the **TrueODS Multi-Machine** window. See [Editions and Workflow](EDITIONS.md) for the entry point and steps.

<a id="en-differences"></a>

### 7. What actually differs: base edition / TrueODS Distributed and UE 5.7 / 5.8

**The base edition and TrueODS Distributed**

- **Plugin name and menu:** the base edition is **TrueODS Panoramic** in the plugin list and shows a **TrueODS** menu. TrueODS Distributed is **TrueODS Panoramic Distributed** and shows a **TrueODS Distributed** menu.
- **The TrueODS Distributed menu adds a Multi-Machine Rendering group:**
    - **Multi-Machine Rendering**: opens a separate multi-machine window whose tab is titled **TrueODS Multi-Machine**. This is not the Multi-Machine Rendering section of the plugin panel described below;
    - **Watch Console**: the progress window;
    - **Latest Advisory Report**: the latest render's report;
    - **Latest Run Folder**: the latest render's folder.

    See [Editions and Workflow](EDITIONS.md) for the multi-machine workflow.

- **Plugin panel:** TrueODS Distributed adds a **Multi-Machine Rendering** section between Rendering and Render Passes.
    - Always shown: **This Job's Checksum** (a code for the current settings) with a **Copy** button, and **Rendering On Several Machines** (default off).
    - Shown only after you turn that switch on: **Settings Checksum** (default on), **Require This Checksum**, **Consistent Motion Timing** (default on).
    - Keep the switch off for single-machine renders.

    [![TrueODS Distributed menu bar: TrueODS Distributed to the right of Help](../media/guide/qs-menu-trueods-distributed.png)](../media/guide/qs-menu-trueods-distributed.png)

    [![TrueODS Distributed plugin panel: the Multi-Machine Rendering section between Rendering and Render Passes (defaults)](../media/guide/qs-distributed-multimachine-section.png)](../media/guide/qs-distributed-multimachine-section.png)

    *UE 5.8, TrueODS Distributed: the menu bar, and the Multi-Machine Rendering section of the plugin panel at its defaults. The checksum is computed from the plugin and MRQ settings, the engine version, the enabled plugins and the project file, so yours will differ from the one shown; when all of these match, the checksum matches too.*

- **Timing lock:** TrueODS Distributed's **Consistent Motion Timing** is on by default and also applies to single-machine renders. Its switch is only shown after Rendering On Several Machines is turned on.
    - It keeps water, fire, flicker and other time-driven effects at the same point in time across segments and machines.
    - It costs no render time.
    - The base edition does not have it.

- Every other parameter, the framing preview, the pre-flight check and resuming are the same in both editions.

**UE 5.7 and UE 5.8**

- **Same source:** both engine versions are built from the same plugin source, and the plugin panel and the workflow are the same. Fab provides a separate package for each engine version. Apart from TrueODS Distributed's Multi-Machine Rendering section, the UE 5.7 and UE 5.8 plugin panels have the same parameters in the same order.
- **Film > Method = Standard ACES:** this option exists only in UE 5.8, and the plugin's HDR tone processing does not read it.
    - If a 5.8 project uses Standard ACES, PNG / JPG / TIFF colours can differ from the viewport.
    - The linear EXR master is unaffected. Work from the EXR and convert it in your grading application.

### Need help?

If a render fails, keep the logs (`<project folder>/Saved/Logs`) and the output folder's `_metadata`, and contact [trueodssupport@gmail.com](mailto:trueodssupport@gmail.com) using the [support template](SUPPORT.md).
