# Blockrift_VR

A small, browser-based voxel sandbox in the style of Minecraft — built with
[Three.js](https://threejs.org) and [WebXR](https://immersiveweb.dev/), playable
in VR on the Meta Quest 3 as well as on desktop for testing without a headset.

Everything lives in a single file: `blockrift-vr.html`. No build process,
no installation required.

## Features

- Procedurally generated terrain (grass, dirt, stone) with scattered trees
- Break and place blocks, 6 block types via a hotbar
- VR controls for Meta Quest 3 controllers
- Desktop fallback with mouse & keyboard for testing without a headset
- Automatic "Enter VR" button whenever the browser supports WebXR

## Hosting & Launching

WebXR and ES modules don't work reliably over `file://`. The file needs to be
served over **http(s)**. Options:

**Test locally (desktop browser):**
```bash
# inside the folder containing blockrift-vr.html
python3 -m http.server 8000
# then open in your browser: http://localhost:8000/blockrift-vr.html
```

**Test on the Quest 3:**
WebXR generally requires an **HTTPS** connection (except on `localhost`).
The easiest path:
- Upload the file to a free static host, e.g.
  [Netlify Drop](https://app.netlify.com/drop), [Vercel](https://vercel.com),
  [GitHub Pages](https://pages.github.com), or [Glitch](https://glitch.com)
- Open the resulting HTTPS URL in the **Meta Browser** on the Quest 3
- Tap "PLAY GAME", then press "ENTER VR" in the bottom right

## Controls

### VR (Meta Quest 3)
| Input | Action |
|---|---|
| Left thumbstick | Move (relative to headset direction) |
| Right thumbstick (up/down) | Fly |
| Trigger | Break block |
| Grip / squeeze | Place block |

### Desktop (for testing without a headset)
| Input | Action |
|---|---|
| Click on canvas | Lock mouse (pointer lock) |
| W A S D | Move |
| Mouse | Look around |
| Spacebar | Jump |
| Left click | Break block |
| Right click | Place block |
| 1–6 | Select block type in the hotbar |
| ESC | Release mouse |

## Technical overview

- **Three.js** (one `InstancedMesh` per block type) renders the world
  efficiently, even with several thousand blocks.
- Block detection via raycasting against the instanced meshes; when placing
  a block, the hit face's normal determines the neighboring block position.
- Ground collision via a downward raycast (no fixed height grid, so it still
  works correctly after breaking/placing blocks).
- Terrain is generated with a simple sine-based pseudo-noise function (no
  external noise library needed).

## Known limitations

This is intentionally a compact MVP, not a full Minecraft clone:

- No saving/loading of the world (every restart regenerates the same world;
  changes are lost on reload)
- No horizontal collision (you can walk through walls)
- Blocks are flat-colored, no textures
- No inventory or crafting system
- World is limited to a fixed area (no infinite chunk streaming)

## Possible extensions

- World save state (e.g. export/import as JSON)
- Textures instead of flat colors
- Chunk-based streaming for larger worlds
- Inventory & crafting
- Multiplayer via WebSockets

