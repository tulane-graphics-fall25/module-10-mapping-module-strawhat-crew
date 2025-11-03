##Day/night transition
Mechanism: A point light “sun” rotates around the Y‑axis. In the fragment shader:
Day color = textureEarth * max(dot(N, L), 0.0) (Lambert diffuse).
Night lights = textureNight * pow(1.0 - max(dot(N, L), 0.0), 3.0) (fades in smoothly at night, zero at midday).
Clouds: textureCloud is added to the base and clamped to [0,1]; subtle drift via animate_time and a Perlin noise offset.
Timing
Full cycle (day → night → day): 25 seconds.
Half cycle (midday → midnight or midnight → midday): ~12.5 seconds.
Controls
ESC: Quit (window stays open until you press ESC).
SPACE: Toggle wireframe on/off.
Mouse drag (LMB): Rotate (trackball).
Shift + LMB drag: Zoom (scale).
Alt + LMB drag: Pan.
