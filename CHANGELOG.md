# Changelog

This changelog summarizes, in plain language, what changes from version to version of **Little GLSL Editor**. It is intended for people who use the software — for the technical details of each feature, see `ROADMAP.md`.

## Version 0.1.15 — August 18, 2026

### New

- **The currently displayed pass can now be exported as a standalone HLSL or Metal (MSL) shader file**, via `File → Export Compiled Shader To → HLSL (.hlsl)` / `→ Metal (.metal)`, next to the existing golfed-code export. This is meant for pasting into a game engine or a native app targeting DirectX/Unity/Unreal (HLSL) or Metal (MSL) — **not** for continuing to edit the shader in this software: the exported file is a one-time, faithful translation of the shader as compiled at the moment of export, and this software has no way to read HLSL or MSL back in, so it can't be pasted back into the editor afterward. This limitation is shown up front, before the save dialog even opens. Custom `iChannel`/uniform bindings translate to `Texture2D`/`SamplerState` (HLSL) or `texture2d<float>`/`sampler` (MSL) using generic binding conventions that may need manual adjustment for the target engine. This export always translates whichever GLSL/WGSL source is currently compiled, golfed or not (your choice) — golfing is a text-level GLSL minifier and is never applied again as part of the HLSL/MSL translation. There is also no guarantee that recompiling the exported file in a third-party engine renders pixel-for-pixel identically to this software's own preview: the translation targets functional correctness, not a bit-exact match — worth checking case by case in your target engine.

## Version 0.1.14 — August 18, 2026

### New

- **WGSL fragment shaders are now accepted as a third input style, alongside Shadertoy and plain GLSL, with no manual conversion.** Pasting or typing a WGSL fragment entry point (`@fragment fn ... -> @location(0) vec4<f32> { ... }`) now compiles and renders directly, just like the GLSL/Shadertoy styles introduced in 0.1.13:
  - the built-in values (`iTime`, `iResolution`, `iMouse`, `iChannel0-3`, ...) are exposed through a `globals` struct, made available only for the fields the code actually uses;
  - custom `var<uniform>` declarations are accepted as well (currently given a default value of 0, same limitation as for plain GLSL — not wired to the sliders panel yet);
  - the footer indicator gains a third icon (🟪 *WGSL*) alongside 🌈 *Shadertoy* and 📄 *GLSL*, with the same live-updating tooltip explaining what triggered the detection.

## Version 0.1.13 — August 17, 2026

### New

- **GLSL and Shadertoy shaders are now both accepted automatically, with no manual conversion.** Until now, every pass had to follow the Shadertoy convention (a `mainImage` function). Pasting or typing a "raw" GLSL fragment shader — with its own `void main()`, its own `#version` directive, its own `uniform` declarations, or the older `gl_FragColor`/`gl_FragData` output style — now compiles directly, without editing it by hand first. The software detects which style is being used and adapts on the fly:
  - the shader's own `#version` is kept if present, otherwise one is added automatically;
  - Shadertoy's built-in values (`iTime`, `iResolution`, `iMouse`, `iChannel0-3`, ...) are made available only for the ones the code actually uses;
  - the older `gl_FragColor`/`gl_FragData[0]` output style is translated automatically into the modern form;
  - the shader's own custom `uniform` declarations are now accepted as well (currently given a default value of 0 — not wired to the sliders panel yet);
  - compile-error messages point to the correct line in the editor in both styles, exactly as before.
  - The `Common` tab and Buffer A-D passes keep working exactly as before, independently of which style each pass uses.
- **A small indicator now appears in the footer** (🌈 *Shadertoy* / 📄 *GLSL*) showing which style was detected for the pass currently being edited, updating live shortly after you stop typing or as soon as you switch tabs. Hovering over it explains, in plain language, which part of the code triggered that detection (e.g. "detected via `mainImage()`").

## Version 0.1.12 — August 16, 2026

### New

- **An audio file (.mp3/.wav/.ogg/.flac) can now be used as a source for an `iChannel`**, exactly as on shadertoy.com: a new option "Audio (file)..." appears in the dropdown menu for each slot, between "Video (file)..." and "Webcam" (drag-and-dropping an audio file directly onto a slot also works). The chosen file plays in a loop and is **audible** during preview — unlike a video, which remains muted — since that is the whole point of a sound-reactive shader. The shader can then sample, just like on Shadertoy, the frequency spectrum (`texture(iChannelX, vec2(u, 0.25)).x`) and the waveform (`texture(iChannelX, vec2(u, 0.75)).x`) of the playing track.
  ⚠️ **Note for this version**: exact spectrum scaling (matching volume level to amplitude intensity) is tuned visually rather than guaranteed pixel-perfect to shadertoy.com due to the lack of an officially published formula; live microphone input is not provided (audio files only); and there is no UI volume slider yet (playback follows system volume).

## Version 0.1.11 — August 16, 2026

### Fixed

- **Small shader constants could be reset to zero as soon as their slider was touched.** A ray marching epsilon, an anti-aliasing bias, or any other very small number (`0.0001`, `1e-5`, etc.) kept its true value in code as long as its slider wasn't touched — but upon the first movement, no matter how small, it could silently get rewritten as `0.0` without any error message (the default displayed precision did not adapt to the number's order of magnitude). The default number of decimal places now adjusts to the original literal's value, keeping behavior unchanged for standard values.
- **Moving a slider via mouse wheel or keyboard could leave orphan `-` signs accumulating in the code.** The protection meant to prevent resynchronization in the middle of a slider adjustment was only triggered during standard mouse dragging; the wheel and keyboard arrows, which produce the exact same type of rapid edit bursts, slipped through. This is now handled consistently regardless of the method used to move the slider.
- **The color picker (swatch next to a `vec3`/`vec4`) could display a different color than the one actually used by the shader, without ever correcting itself.** A bright color selected for a constant originally intended to be dark could exceed the range set for the associated sliders; the display was then silently capped, but the written code still contained the uncapped value — a permanent disconnect between what was displayed and what was computed. The color written to the code now always matches the displayed color.
- **`vec3(-1.0, 0.5, 0.2)` and other vectors with negative components were never recognized as a single color/X-Y group.** Nothing was lost (each component remained editable as a separate slider), but grouping into a single control — the signature feature of vector sliders — never triggered whenever a component was negative, which is very common for a direction or offset. This has been fixed: these vectors are now grouped just like any other.
- **A corrupted or manually edited project file could crash the app or display garbled data in the sliders panel.** A `NaN`/`Infinity` bound slipped into a project's `.json` file passed through existing checks and could push non-finite values into UI sliders. Such inputs are now simply ignored upon opening without affecting the rest of the project.
- **Switching passes (Image/Buffer A-D) while actively dragging a slider could, in rare cases, rewrite the wrong spot in the code.** The risk only concerned a very narrow window (switching tabs without releasing a mouse drag, usually combining keyboard + mouse, or on a touchscreen), but is now explicitly eliminated.

### New

- **`vec4(r, g, b, a)` constants (color + transparency) now group into a single control**, featuring the same color picker as `vec3`, instead of remaining as 4 separate numerical sliders.
- **Right-clicking a component of a vector slider (`vec2`/`vec3`/`vec4`)**: it is now possible to adjust its bounds and decimal precision, exactly as was already possible on a simple numerical slider.
- The "Reset category", "Randomize category", and "Clear keyframes" buttons now display a tooltip clarifying that they apply to all sliders in the category, including those currently hidden by the search field.

## Version 0.1.10 — August 16, 2026

### New

- **New set of major European and Asian language files.**
- Norwegian, Swedish, German, Spanish, Italian, Portuguese, Japanese, Chinese (Simplified), Korean, Hindi

## Version 0.1.9 — August 16, 2026

### Fixed

- **A shader could appear washed out, almost white, even though it looked vibrant and saturated on shadertoy.com.** Cause: when a shader computed the alpha channel of `fragColor` itself (instead of leaving it at 1, which most shaders do without thinking about it), the app passed this value through as-is to the display — which treated it as true transparency and let the window's light background bleed through the image. Shadertoy, however, always ignores this channel for the final displayed image (its viewer is opaque by design): the rendering remains full and saturated regardless of what the shader writes into `fragColor.a`. The Image pass (the one actually displayed) is now forced opaque in the same way, aligning rendering with shadertoy.com. Buffer A-D passes are **not** affected: several shader techniques use them to store data in their alpha channel from one pass to another, so this value remains intact for them.

## Version 0.1.8 — August 16, 2026

### New

- **Language selector in Preferences.** *File → Preferences…* now offers a dropdown menu to choose the interface language, listing languages actually present on disk (no "half-installed" languages in the list). The choice is saved and remembered for the next launch. **The change does not apply on the fly**: as already displayed menus, buttons, and messages do not re-translate themselves automatically, a prompt reminds you upon confirming — simply restart the app to see the new language everywhere.

## Version 0.1.7 — August 16, 2026

### New

- **First building blocks for localization.** The application can now load a translation file (`lngs/fr.json`, `lngs/en.json`) at startup, with automatic fallback to French if a translation is incomplete. **Nothing is visible in the interface yet**: menus, buttons, and messages remain hardcoded in French for now while they are progressively wired into this system. No language selector in Preferences yet either — that will be the final piece of the puzzle once texts are migrated.

### Fixed

- **Video export: the encoding window remained stuck at 100%.** Once encoding finished, the *Export Video (MP4)* dialog could stay displayed indefinitely without ever closing, forcing a hard quit of the software. Cause: the error output stream of the encoder tool was never read during export, which could cause it to hang silently right before finishing, even after hitting 100%. It is now systematically cleared in the background, allowing export to complete and close normally.

## Version 0.1.6 — August 15, 2026

### New

- **Video export: `.mp4` assembly finally working.** The limitation announced in the previous version is lifted: *File → Export Video (MP4)…* now directly outputs a ready-to-share `.mp4` file without manual steps or external tools.
  - A **cancellable progress bar** separately displays render progress ("Rendering: 120/300 frames") then encoding progress. Cancelling at any time cleanly aborts the operation and leaves no temporary files on disk.
  - Encoding uses a constant quality setting (CRF) rather than a fixed bitrate, ensuring visually consistent results regardless of the exported shader.
  - No separate installation required: the encoding tool is bundled directly with the application.
  - A new command line option (`--export-mp4`) allows video export without opening the interface, useful for batch processing multiple shaders.

### Changed

- **The golfer produces even shorter code**, with three new optimizations always active (alongside comment removal and number shortening, without new checkboxes):
  - an assignment using the same variable on both sides (`x = x + value;`) is rewritten in its shorter compound form (`x += value;`);
  - a vector constructor where all arguments are identical (`vec3(v, v, v)`) is reduced to the equivalent short form (`vec3(v)`);
  - the `in` keyword before a function parameter (`in vec2 fragCoord`), optional in GLSL, is removed automatically (`vec2 fragCoord`).

  As always, every optimization is verified to ensure it never alters the shader's rendered output.

- The app version number (**0.1.6**) now appears in the main window title and in the "About" dialog.

## Version 0.1.5 — August 15, 2026

### New

- **Video export (first step).** A new menu *File → Export Video (MP4)…* opens a settings dialog to select:
  - export **duration**, in seconds or directly in frame count;
  - **frame rate** (24, 30, or 60 frames per second, or a custom value);
  - output **resolution**, independent of current preview size;
  - **compression level**, with three simple presets (Quality, Balanced, Minimal Size) or precise controls for advanced users.

  An estimated final file size is displayed live as you tweak settings.

  ⚠️ **Note for this version**: automatic assembly into a true `.mp4` file is not available yet. The app currently produces a sequence of numbered images (one per frame of animation) and indicates where to find them once capture is done; they must still be assembled manually with an external tool. Direct `.mp4` export will arrive in a future update.

- **Updated "About" window** (*Help → About*): now displays the full name of the software, version number, copyright information, and a contact address.

### Changed

- The app version number (**0.1.5**) now appears in the main window title and in the "About" dialog.