# 赛马 3D 模型公开库（Horse Racing 3D Assets）

一个可直接被任意电脑上的智能体、三维开发者读取和使用的赛马 3D 模型仓库。

全部资源离线可用，不依赖外网 CDN（内容分发网络）。模型均为 glb（二进制 glTF，一种通用三维模型格式）。

## 目录结构

```
horseracing-3d-assets/
├── assets/                      模型文件
│   ├── draft_horse.glb          重型挽马，2.87 MB，42 骨骼、1 段动画、带贴图
│   ├── horse_wonderunit.glb     木纹马，0.31 MB，40 骨骼、0 段动画、带贴图
│   └── horse_google_modelviewer.glb  低模演示马，0.17 MB，1 段动画、无骨骼
├── vendor/                      离线三维库（three.js 生态）
│   ├── three.module.js          三维渲染引擎 three.js
│   ├── GLTFLoader.js            模型加载器（读取 glb/gltf）
│   └── OrbitControls.js         鼠标视角控制器（旋转、缩放、平移）
├── viewer.html                  浏览器预览页
├── NOTICE-models.md             每个模型的作者、授权协议与来源
├── LICENSE                      代码部分许可（MIT）
└── .gitignore
```

## 模型规格

| 文件（file） | 体积（size） | 骨骼（skeleton） | 动画（animation） | 贴图（texture） | 适合场景 |
|---|---|---|---|---|---|
| draft_horse.glb | 2.87 MB | 42 | 1 | 有 | 写实渲染、近距离展示 |
| horse_wonderunit.glb | 0.31 MB | 40 | 0 | 有 | 低带宽、移动端、静态造型 |
| horse_google_modelviewer.glb | 0.17 MB | 0 | 1 | 无 | 极简占位、性能测试、低配设备 |

## 在浏览器里看效果

glb 用 file://（本地文件协议）直接双击打不开，浏览器会拦截本地模块与模型请求，必须起一个本地服务器：

```
python -m http.server 8080
```

然后打开 `http://localhost:8080/viewer.html`。没有 Python 就用 `npx serve`（Node 的静态服务器）。

预览页支持三个模型一键切换、左键旋转、滚轮缩放、右键平移，也可以把任意本地 .glb 文件直接拖进网页查看。

## 给智能体的接入说明

项目根目录下，模型路径是相对路径 `assets/*.glb`，加载库在 `vendor/`。

最小可用代码（Three.js 三维引擎 + GLTFLoader 模型加载器）：

```js
import * as THREE from './vendor/three.module.js';
import { GLTFLoader } from './vendor/GLTFLoader.js';

const loader = new GLTFLoader();
loader.load('./assets/draft_horse.glb', (gltf) => {
  scene.add(gltf.scene);
  // gltf.animations 是动画数组（骨骼动作），可直接交给 AnimationMixer（动画混合器）播放
});
```

注意事项：

1. 模型单位不统一，加载后需按包围盒（Box3，外接长方体）自动缩放，参考 `viewer.html` 里的 `fitToGround` 函数。
2. `draft_horse.glb` 与 `horse_wonderunit.glb` 带骨骼但没有绑定到具体动作的播放时长，只有 `draft_horse.glb` 自带 1 段动作。
3. `horse_google_modelviewer.glb` 是整块静态网格，没有骨骼，不能做骨骼动画替换。
4. 页面必须通过 HTTP 服务访问，file:// 下 ES 模块（模块化脚本）与模型加载都会被浏览器拦截。

## 未包含的模型

本库只收录授权链条清晰的模型。以下文件存在于开发环境，但来源无法核实，未纳入公开库，请勿从其他渠道传播：

- `horse_jockey_rig.glb`（线上 3D 赛马实际使用的主模型，马+骑手，2.95 MB，21 网格，1 段动画）
- `horse_realistic.glb`（写实赛马，1.06 MB，50 骨骼，26 段动画）
- `horse.glb`、`horserider.glb`（来源不明）

## 许可

- 代码（`viewer.html`、`vendor/` 集成方式）：MIT，见 `LICENSE`。
- 模型：各自独立授权，见 `NOTICE-models.md`。使用或再分发前请保留作者署名与协议链接。
