# Little GLSL Editor

A lightweight shader editor for Windows, designed for writing, tweaking, and testing real-time GPU-generated visual effects — the kind of effects found on [Shadertoy](https://www.shadertoy.com/). Fully compatible with Shadertoy code out of the box: no special syntax to learn.

**[⬇ Download Latest Version](../../releases/latest)**

![Editor Preview](docs/screenshot1.png)

![Editor Preview](docs/screenshot2.png)

## Features

- **Live Preview**: The rendering updates with every keystroke — no "compile" button to click.
- **Automatic Sliders**: Every numerical value in your code (colors, sizes, speeds, etc.) automatically becomes a draggable slider in a side panel, grouped by category, complete with a "reset" button and a "randomize" button to rapidly explore variants.
- **Multi-pass Rendering** (Image + Buffers A through D) to build multi-stage effects that feed into each other, exactly like Shadertoy.
- **Interactive Inputs**: Mouse (position, click) and keyboard can drive your effect in real time.
- **Audio & Textures**:
  - Load your own audio files (`.mp3`, `.wav`, `.ogg`, `.flac`) into an `iChannel` to drive sound-reactive shaders with frequency spectrum and waveform sampling.
  - Load your own images, use auto-generated textures (checkerboard, noise), or 360° environment images (cubemaps) for reflections.
- **Drag & Drop**: Drag an image or audio file directly onto a texture slot.
- **Image Export**: Save the currently displayed frame as a PNG image.
- **Projects**: Save your work to a project file and reopen it later, with a quick-access recent files list.
- **Performance Indicator**: A small graph displays the rendering frame time.
- **Preferences**: Customizable editor font size, minimap visibility, and preview update frequency.
- **Code Minification ("Golfing")**: A one-click tool to shrink your shader code down to the smallest possible byte count — ideal for code golfing competitions.

## Installation

1. Click on **[⬇ Download Latest Version](../../releases/latest)** above (or go to the **Releases** tab of this repository).
2. Download the installer (`LittleGLSLEditor-Setup-x.x.x.exe`) from the **Assets** section of the release.
3. Run the installer and follow the on-screen instructions.
4. Once installed, launch **Little GLSL Editor** from the Start menu.

> Requires 64-bit Windows and a compatible graphics card.

## Getting Started

1. Open the application: an example shader is already loaded and running.
2. Edit the code in the left panel — the preview on the right updates automatically.
3. Check out the sliders panel: every number detected in your code appears as a slider you can tweak without editing text directly.
4. Use the **Image / Buffer A / B / C / D / Common** tabs above the editor to build multi-pass effects.
5. Assign images, procedural textures, audio files, mouse input, or keyboard input to an `iChannel` slot using the panel to the right of the preview.
6. Once you are happy with the result, use the **File** menu to save your project, export a PNG screenshot, or export minified ("golfed") code.

### Useful Shortcuts

| Action | Shortcut |
|---|---|
| Undo | Ctrl+Z |
| Redo | Ctrl+Y |

## Need Help?

Have a question, encountered a bug, or have a feature request? Feel free to open an [issue](../../issues) on this repository.

## License

Little GLSL Editor is **free**, but its source code is not provided.  
See [LICENSE.md](LICENSE.md) for details on allowed and restricted usage.
