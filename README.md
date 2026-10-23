<img width="1920" height="927" alt="tmdview web" src="https://github.com/user-attachments/assets/59bf2d98-fd80-4f68-9bd3-46d1048fb18a" />

# tmdview web

A browser tmd viewer: Renders one playable character from SK:EV on screen Built on three.js r149
(shipped next to the page as `three.min.js`, MIT licence in `three.LICENSE`).

**Run it:** open `tmdview.html` (double-click; no server needed) and press **Open GameData folder…**. Pick the
game's `GameData` folder, or the game folder that contains it. Every file is read inside the browser; nothing is
uploaded. Chrome / Edge use the folder picker and open files only when needed. Other browsers list the folder's files
through the file dialog (they may call it an "upload"; nothing leaves the computer). Keep `three.min.js` beside the
page. Without it the page falls back to the jsDelivr CDN.

**What it shows:**
* Characters `pl00`–`pl34`, dressed from `Binary/Param/Player/cosinfo.bin`:
  * everyday (Uniform) and shinobi (Ninja) costume;
  * that costume's hair and panties, bra textures, accessories (`accoffset.bin`);
  * proportions from `plbodyscl.bin`;
  * bust variant (small for pl08, and pl18 everyday);
  * face / hair layer tags (NNJ / UNI: e.g. pl26's mask only in her ninja costume);
  * pl16's hair weapon (her back hair).
* Motions from `Motion/Adv/Adv_Motion.cat` and `Motion/Room/Dress_Motion.cat`. Only those that drive the body
  skeleton are listed; switching cross-fades over 10 frames.
* Physics, as in tmdview:
  * yure chains: costume, skirt (`category_list.bin` cate), hair, accessories;
  * collision shapes;
  * bust from `BustInfo.bin`;
  * baked keys of the dressing-room poses follow the animation.
* Face: the character's `Motion/Adv/plNN_facemot.cat`:
  * brow, eye and mouth poses: the 30-frame blocks of `adv_plNN_f_brow` / `_eye` / `_mouth`, 14 each for pl00;
  * mouth opening inside the block, or Talk (lip flap);
  * the overlay meshes 2–6: cheeks, blush, tears, `><` and `= =` eyes.
* Outfit layers:
  * NTRL intact, BRK0–BRK3 clothes damage, BRKS transformation (the costume animation restarts);
  * the CSBB break shader (pieces burn away along the break map, glowing edge).
* Touch: right-drag (or Alt + left-drag) grabs a skirt / hair particle or a bust side and pulls it.
* Shading: GLSL port of the character shaders:
  * CSB0 / SKC0 / SKF0 toon ramp, sheen and rim;
  * HRH0 hair ring;
  * CSW0 tights over the panties;
  * the outline hull.

**Keys:**
* left drag: orbit
* Shift / middle drag: pan
* wheel: zoom
* Space: play / pause
* PgUp / PgDn (or ← / →): previous / next motion
* C: costume
* L: next outfit layer
* F / N: next brow + eye pose / next mouth
* T: talk
* P: physics
* R: camera

**Source:** `src/*.js` are ES modules, bundled with esbuild into the one-file page by `node build.mjs`, which writes
`tmdview.html` from `template.html`.

| file | content |
|---|---|
| `gamedata.js` | folder access (`showDirectoryPicker` handle or `webkitdirectory` file list), case-insensitive paths |
| `cat.js`, `dds.js` | `.cat` catalogues, DDS → RGBA |
| `tmd2.js` | `.tmd2` models: vertices, draw commands, materials, skeleton, layer tags, material channels; texture pooling |
| `tmo.js` | `.tmo2` motions (Key4 / 8 / 16), baked-physics key windows |
| `physics.js` | yure / collision / bust physics and the grab (port of `viewer/Physics.cpp`) |
| `character.js` | cosinfo outfit, costumes, parts, accessories, posing (port of `viewer/Actor.cpp`) |
| `render.js` | three.js meshes, skin-matrix texture, character shaders (port of `viewer/GameShaders.h`) |
| `main.js` | UI, camera, input, frame loop; `window.tmdview` exposes the state for the console |
