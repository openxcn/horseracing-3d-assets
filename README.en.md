# Horse Racing 3D Assets

A small, offline-ready collection of horse 3D models (glb / binary glTF) plus a browser viewer, intended for developers and AI agents that need ready-to-load assets on any machine.

No CDN（内容分发网络） dependency: the three.js（三维引擎）files are vendored locally.

## Layout

```
horseracing-3d-assets/
├── assets/
│   ├── draft_horse.glb               2.87 MB · 42 bones · 1 clip · textured
│   ├── horse_wonderunit.glb          0.31 MB · 40 bones · 0 clip · textured
│   └── horse_google_modelviewer.glb  0.17 MB · no bones · 1 clip
├── vendor/                           three.module.js, GLTFLoader.js, OrbitControls.js
├── viewer.html                       preview page
├── NOTICE-models.md                  attribution（署名）and licenses（许可协议）
└── LICENSE                           MIT (code only)
```

## Preview

glb files cannot be opened via `file://` because browsers block local module and model requests. Run a local HTTP server:

```
python -m http.server 8080
```

Open `http://localhost:8080/viewer.html`. Drag any local `.glb` onto the page to inspect it.

## Agent usage

```js
import * as THREE from './vendor/three.module.js';
import { GLTFLoader } from './vendor/GLTFLoader.js';

const loader = new GLTFLoader();
loader.load('./assets/draft_horse.glb', (gltf) => {
  scene.add(gltf.scene);   // gltf.animations holds AnimationClips（动画片段）
});
```

Models have inconsistent units. Normalize with a bounding box（包围盒）：scale to a target height, then offset so `min.y === 0`. See `fitToGround` in `viewer.html`.

## Licensing

- Code: MIT.
- Models: third-party, each under its own license — see `NOTICE-models.md`. Keep the attribution when redistributing.
- Models whose provenance could not be verified (`horse_jockey_rig.glb`, `horse_realistic.glb`, `horse.glb`, `horserider.glb`) are intentionally excluded.
