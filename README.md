# LUT Maker - a GPU-Accelerated LUT Generator

Run this app at this address:
[https://o-l-l-i.github.io/lut-maker/](https://o-l-l-i.github.io/lut-maker/)

- **Author:** Olli Sorjonen
- **Github:** [https://github.com/o-l-l-i/](https://github.com/o-l-l-i/)
- **X:** [https://x.com/Olmirad](https://x.com/Olmirad)
- **Version:** Alpha (very early development)

---

## What is this?

LUT Maker is a **GPU-accelerated** Lookup Table (LUT) generator designed to create `.cube` and **Unity-compatible PNG LUTs** for color grading and creative image adjustments. It’s a personal project made to provide a lightweight tool for artists and developers, so you don't need to have access to high-cost professional software if you want to create a LUT for testing a look.

The .cube LUTs also work with **Comfy Essentials Image Apply LUT node**, so you can quickly create LUTs to tweak and grade your AI art.

***The whole LUT process runs in your browser, your images are not uploaded anywhere and stay on your computer.***

![LUT Maker preview image](/images/app_image.png)

---

## TL;DR Basic Usage

- Load your source image (or use the default image.)
- Adjust controls to tweak color curves, exposure, contrast, and more.
- Preview changes in real-time.
- Export LUTs for `.cube` format or **Unity PNG**.
- Import to your application and use.

---

## Purpose, Use and Features

- Generate 3D LUTs quickly with GPU acceleration.
- You can load in your own images to preview the LUT effect.
- Support for industry-standard `.cube` LUT format.
  - Resolutions **17, 32, 64**.
- Unity support (PNG LUT texture)
  - Currently only resolution 32.
  - Special support and detailed guidance for Unity integration down below.
- Dark and light modes for UI.
- Intended as an **Alpha version** — expect rough edges and incomplete features.
- Not production-ready: **use carefully** and always verify results in your target environment.
- Suitable for artistic experimentation and early-stage workflows.
- Users can **save and load presets**, allowing quick reuse of specific LUT configurations or color grading setups.
- Reset functionality to restore default settings (both global and local for specific adjustments.)

---

## Adjustment Modes

(well, obviously you see these in the UI, but some comments:)
  - ACES (Academy Color Encoding System) tone mapping
  - Exposure
  - Brightness
  - Contrast
  - HSV (Hue, Saturation, Value)
  - Vibrancy
  - Cross Processing (my interpretation)
  - Levels (Shadows, Midtones, Highlights)
  - Channel Mixer (similar to Photoshop)
  - RGB curves (currently R,G,B channels)
  - Lift, Gamma Gain (my artistic and possibly flawed implementation)

---

## Histogram

  - Luminance/value mode
  - RGB
    - Separate RGB channels (can be toggled on/off)
  - Smoothing
  - Linear and Log display modes
  - Value inspector, hover over the histogram to see
  - Statistics that could be useful (min, max, mean, median, mode, standard deviation, dynamic range)

---

## Unity LUT Usage Notes

If you are importing LUTs into Unity, please note the following to ensure correct results/to be able to use the LUTs:

- **LUT Size:** Must be 32x32x32. The UI disables export if this is not selected.

LUT textures look like this:
![LUT texture](/images/unity_lut_bubblegum_dream.png)

**In Unity:**
- **Texture Type:** Set to **Default**.
- **sRGB:** Disable sRGB on the texture.
- **Gamma:** Consider ignoring PNG gamma in settings, it should not affect anything right now, but it's recommended to disable it just in case.
- **Compression:**
  - Disable compression.
  - Set format to **RGB 24-bit** or **RGBA 32-bit**.
  - Or any RGB format that retains all three color channels.
- **Important:** Compression or incorrect format will distort color grading results drastically.

![Texture settings in Unity](/images/texture_settings.png)
![Volume component](/images/unity_component.png)

### LUT Applied in Unity

![LUT applied in Unity](/images/unity_lut_applied.png)

---

## ComfyUI

At least two custom nodes exist which support usage of LUTs, sadly the ComfyUI Essentials is no longer developed, so it might not be guaranteed to work well in near future.

![ComfyUI LUT nodes](/images/comfyui_lut.png)

- **ComfyUI_essentials:** [https://github.com/cubiq/ComfyUI_essentials](https://github.com/cubiq/ComfyUI_essentials/)
- **ComfyUI_LayerStyle:** [https://github.com/chflame163/ComfyUI_LayerStyle](https://github.com/chflame163/ComfyUI_LayerStyle/)

---

### LUT Applied in ComfyUI

Here's test of one of the presets (bubblegum dream) applied to an image in ComfyUI with ComfyUI Essentials:
Disable gamma correction to get matching colors:

![ComfyUI LUT applied](/images/comfyui_lut_applied.png)

For comparison, same image in LUT Maker:

![Same image in LUT Maker](/images/comfyui_lut_applied_in_lut_maker.png)

---

## Known Issues & Limitations

- Curve math/evaluation is not super precise, might cause the a slight deviation in colors even with default curves.
- Some color math might be imprecise or simplified; expect minor deviations.
- The order of processing needs further evaluation and testing, it might be far from optimal and degrade the color accuracy.
- Clunky UI on small screens; layout tuning ongoing.
- Unity export locked to 32x LUT size currently.
- No undo/redo functionality, but there's per-adjustment reset which mitigates this quite a lot.
- This is a work-in-progress, so expect bugs and UI quirks.
- LUTs tested in Photoshop and Unity with good fidelity, but your mileage may vary.

---

## Tools & Technology

This project uses a modern frontend stack with the following notable tools and libraries:

- **React 19** — for building the user interface in a component-based, declarative style.
- **Vite** — a fast development build tool that provides lightning-fast HMR (Hot Module Replacement) and optimized builds.
- **Three.js** (via `@react-three/fiber` and `@react-three/drei`) — used for WebGL-powered 3D rendering and GPU-accelerated image processing within the app.
- **Tailwind CSS** (with plugins like typography and animations) for styling and responsive design.
- **Radix UI** — dialogs and sliders, for building interactive UI elements.
- **React Markdown** — used to display this README.

---

### Rendering & Performance

- Image processing operations (e.g., LUT generation, color adjustments for image preview) are accelerated via WebGL rendering powered by Three.js.
- Histogram rendering and other UI elements are currently CPU-bound and not GPU-accelerated, which may impact performance.

---

### 🔒 Privacy & Data Handling

All image and LUT processing is performed **entirely in your browser**, using local WebGL acceleration.

To the best of my knowledge:
- **No image data or user input is sent anywhere** — not to my server, and not to any third parties.
- The app **does not track**, log, or store your data.
- You can use it fully offline.

This tool does include some third-party open source dependencies, and while I actively avoid libraries that request telemetry or external connections, I can't guarantee with 100% certainty that none of them include such behavior internally.
If any telemetry-related settings or prompts appear, I will disable them promptly and update the app accordingly.

Privacy and user data safety are important to me, and I aim to keep this tool self-contained and transparent.

---

## License & Usage Terms

- **Not licensed for copying, redistribution, etc — in other words — don't steal it!**
- But feel free to use and try it out as you like, the results are free to use however you want.
- Do **not** repurpose or rebrand this work without explicit written permission. This is not freeware.
- No warranties or guarantees are implied; use at your own risk.

---

## A Personal Note

This tool was crafted during my own time and out of personal enthusiasm — not a commercial product or professional-grade software.
If it’s not as polished as "real" industry tools, that's expected!
Your feedback is appreciated but please understand the scope and limitations of this alpha project.

---

## Contributions & Support

Contributions and suggestions are welcome but may not be prioritized.

---
