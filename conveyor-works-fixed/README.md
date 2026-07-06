# CONVEYOR WORKS — externalized Three.js

The old single-file HTML was **740 KB**. 80% of that was the minified Three.js r128
library inlined on one line. Pulling it out drops the working file to **151 KB**.

## Files
- `three.r128.min.js` — Three.js r128 (MIT), extracted verbatim. Lives here in the repo.
- `conveyor-works-v3-18.html` — the game, 151 KB. Loads Three.js by URL instead of inlining it.

## How it loads
The HTML pulls Three.js from **jsDelivr's GitHub CDN**, pointed at this repo:

    <script src="https://cdn.jsdelivr.net/gh/Graphic37/conveyor-repo@main/conveyor-works-slim/three.r128.min.js"></script>

jsDelivr just mirrors the file already in this repo — no separate upload. If you move or
rename the file, update the path after `@main/` to match.

## Why jsDelivr and not a raw.githubusercontent.com link
A raw.githubusercontent.com URL returns the file, but GitHub serves it as text/plain with
nosniff, so modern Chrome/Electron REFUSE to execute it as a <script> (ERR_BLOCKED_BY_ORB).
jsDelivr serves the identical file as application/javascript with CORS, so it runs.
(raw URLs are still fine for GLB/texture assets loaded via fetch()/loader.parse() — that
path isn't affected. This only bit the <script src>.)

## Workflow
From now on you only ever edit / re-upload the 151 KB HTML. The 589 KB library stays
parked here and is never touched — adding machines (miners, etc.) never changes it.

## Shipped build alternative (optional)
To avoid a runtime CDN dependency in the packaged Electron build, drop three.r128.min.js
next to the HTML and use a relative path — served locally it gets the right MIME type:

    <script src="./three.r128.min.js"></script>

That doesn't help workspace size during dev, but it's the most robust option for the ship.
