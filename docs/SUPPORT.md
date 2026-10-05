# TRUEODS Support

**English** · [简体中文](SUPPORT.zh-CN.md)
<a id="english"></a>

[Home](../README.md) · [Quick Start](QUICKSTART.md) · [Editions](EDITIONS.md) · [FAQ](FAQ.md)

Official profile: [TRUEODS](https://github.com/trueodsofficial)  
Support email: [trueodssupport@gmail.com](mailto:trueodssupport@gmail.com)

Support group: [How to join](COMMUNITY.md)

### Contact and scope

For technical support, email [trueodssupport@gmail.com](mailto:trueodssupport@gmail.com) with the subject **[TrueODS Support] Short description**. Chinese and English reports are welcome. Send private materials by email; group membership is not required for email support.

To join the Telegram support group, email your **Fab order number, purchased product name and proof of purchase** to the same address with the subject **[TrueODS Telegram] Join request**. Once verified, we will reply with a group invitation link. See [How to join](COMMUNITY.md) for the steps and group guidelines.

Use the package for your UE version and reproduce the issue over a short frame range, saving the output to a new folder. Supported engines are Unreal Engine 5.7 and 5.8, with the Win64 Editor and MRQ. Linux, macOS, packaged games and command-line renders you set up yourself (for example, MRQ launched directly by render-queue management software) are not current support targets; the plugin's own **Render With Editor Closed** and the TrueODS Distributed **Start Rendering On This Machine** are supported. Test Path Tracing, third-party integrations and unusual workflows before production; compilation alone does not establish rendering compatibility.

Reports are assessed by reproducibility, impact and the information available. This page does not promise round-the-clock coverage, a fixed response time or compatibility fixes for every third-party plugin.

### Before reporting

1. Check that only one TrueODS copy is installed, record the loaded version, and do not mix in DLLs from an older build.
2. Record the first failing frame and keep the log. Reproduce at 4K on one frame or a short range in a new empty folder; do not overwrite the only evidence.
3. For exposure problems, state whether **Use Scene Exposure** was used, the camera/PPV exposure mode, and the chosen value.
4. For continuity problems, state the start frame, restarts, resume status and whether simulations are baked. A cold start on a later frame is not equivalent to continuous rendering.
5. For performance or memory issues, record Output Width Per Eye, Supersample, Samples Per Pane, VRAM Mode, the scene event on the slowest frame and peak VRAM, not just an average time per frame.

### Email template

Copy and fill in. Write "unknown" for anything you do not know yet; you do not need to reveal sensitive information to fill in the template.

```text
Title:
TrueODS Version / VersionName:
Product edition (TrueODS base edition / TrueODS Distributed):
Exact UE version (e.g. 5.7.4); Launcher or source build:
Windows version:
GPU / VRAM / driver:
System RAM:

Renderer:
Format / Projection / Stereo Layout:
Output Width Per Eye / Supersample:
Samples Per Pane / Anti-Aliasing Method / VRAM Mode:
Image Format / Also Write EXR (HDR master) on or off / EXR Compression:
Exposure mode / Use Scene Exposure used:
Frame range / whole sequence or custom range:
Fresh / resume / continuation after restart / Multi-Machine Rendering:
Baked simulations:
Relevant third-party plugins and versions, if any:

Steps to reproduce:
1.
2.
3.
Expected result:
Actual result:
First failing frame / frequency:
One redacted error line, if available:
Private materials available: log / frame / minimal project
Preferred reply language:
```

### Privacy and attachments

**Do not post full logs, render diagnostics, receipts, client images or projects in public issues, the support group or public file shares.** Even an error line can contain private paths, so check before sharing. Start with the description and settings; send only the relevant, redacted materials privately if needed. Ask how to send large files before uploading a project.

Remove credentials, tokens, personal paths and confidential client or asset names. Order verification needs only the necessary details; never send an Epic password, verification code, payment-card number or full payment details. Share only assets and minimal reproduction projects you are permitted to share, and do not break a client NDA or a third-party asset licence to get help. This address uses a third-party email provider, so send only material you are willing to have handled through it; if you need a confidentiality agreement or a special transfer method, say so before sending materials.

### Reinstalling and refunds

Save your work and close the editor before reinstalling. Remove only the identified plugin copy. **Do not delete Binaries from a binary-only package**: reinstall the complete package instead. For a source build, clean only this plugin's generated Binaries and Intermediate folders, and only when a rebuild is needed. Preserve project Content, Config and Saved. Shared engine-cache deletion is not a routine first step.

Technical issues are handled at the support address above. For Fab account, order and refund procedures, use [Fab's official help](https://dev.epicgames.com/documentation/en-us/fab/purchasing-and-downloading-assets-in-fab). Contacting us does not change the platform's refund policy or your statutory rights.
