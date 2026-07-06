# CONVEYOR WORKS — externalized Three.js

The old single-file HTML was **740 KB**. 80% of that was the minified Three.js r128
library inlined on one line. Pulling it out drops the working file to **151 KB**.

## Files
- `three.r128.min.js` — Three.js r128 (MIT), extracted verbatim. Push this to your repo.
- `conveyor-works-v3-18.html` — the game, now 151 KB. Loads Three.js by URL instead of inlining it.

## Setup (one time)
1. Put `three.r128.min.js` in a repo, e.g. `Graphic37/conveyor-works` on the `main` branch.
2. In the HTML, the tag near the top is:
   ```html
   <script src="https://raw.githubusercontent.com/Graphic37/conveyor-works/main/three.r128.min.js"></script>
   ```
   Change the `Graphic37/conveyor-works/main` part to wherever you actually pushed it.

That's it. From now on you only ever edit / re-upload the 151 KB HTML — the 589 KB library
stays parked in the repo.

## Notes
- It's a classic UMD script (not a module), so loading it from `raw.githubusercontent.com`
  works even though that host serves `text/plain`. Same pattern you already use for assets.
- Verified: game boots headless with `THREE.REVISION === 128`, canvas renders, no errors.
- For a **shipped Steam/Electron build** where you don't want a runtime network dependency,
  drop `three.r128.min.js` next to the HTML and use a relative path instead:
  `<script src="./three.r128.min.js"></script>`. Or a CDN:
  `https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js`.
