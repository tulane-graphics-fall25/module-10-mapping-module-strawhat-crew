## Module 11 – Earth (OpenGL)

### What is this?
An interactive, real‑time Earth rendering built with modern OpenGL. It shows a textured globe (NASA Blue Marble), animated cloud cover, and a smooth day/night cycle with city lights visible on the night side.

### Key features
- **Procedural sphere mesh**: positions, normals, and UVs generated in code.
- **NASA Blue Marble day texture** and **Black Marble night lights**.
- **Clouds** blended on top and clamped to avoid over‑brightening; subtle drift via Perlin noise.
- **Day/Night shading** using Lambert diffuse; the “sun” (point light) rotates to produce a ~25 s full cycle.
- **Trackball controls** for rotate/pan/zoom and a wireframe toggle.

### Requirements
- Windows 10/11
- Visual Studio 2022 Build Tools (or VS 2022)
- CMake 3.10+
- GPU/driver with OpenGL 3.2 support

### Build (CLI)
```powershell
# From repo root
cmake -S earth -B earth/build -DCMAKE_BUILD_TYPE=Release
cmake --build earth/build --config Release -j 4
```

### Run
```powershell
earth/build/Release/earth.exe
```

### Controls
- ESC: quit
- SPACE: toggle wireframe
- Mouse drag: rotate
- Shift + drag: zoom
- Alt + drag: pan

### How day/night works
- The fragment shader computes `lambert = max(dot(N, L), 0.0)`.
- Day color = `textureEarth * lambert`.
- Night lights fade in with `pow(1.0 - lambert, 3.0)` so there are no lights at midday and a smooth ramp at night.
- Clouds are added on top and the result is clamped to `[0, 1]`. A small Perlin‑based UV offset and `animate_time` produce gentle drift.
- The light (sun) rotates around the Y‑axis; the full rotation takes ~25 seconds (set in `earth/source/earth.cpp` inside `animate()` via `sun_cycle_seconds`).

### Project layout (relevant parts)
- `earth/` – app sources, shaders, and assets
  - `source/earth.cpp` – OpenGL setup, textures, animation loop
  - `source/common/ObjMesh.cpp` – sphere generation (positions, normals, UVs)
  - `shaders/` – GLSL vertex/fragment shaders
  - `images/` – Blue Marble, Black Marble, cloud and noise maps

### Troubleshooting
- Window closes immediately: run from a terminal to see shader errors; ensure GPU supports OpenGL 3.2.
- App builds but shows black: press SPACE (wireframe) to confirm geometry; verify textures exist under `earth/images/`.
- Cycle too fast/slow: change `sun_cycle_seconds` in `animate()`.

### Credits
- Blue Marble and Black Marble imagery © NASA.
- GLFW and GLAD included for windowing and OpenGL loading.


