# 🎬 Directing the World

### Fast Autoregressive Video Generation with Compositional Human-Camera Control

> A unified autoregressive world model that **decouples** human-motion control and
> camera-trajectory control, then **integrates** them into a single generative
> model — enabling stable long-horizon generation, precise SMPL-driven human
> dynamics, and coherent camera-based world exploration.

<p align="center">
  <em>Haoyuan Wang*&nbsp;&nbsp;·&nbsp;&nbsp;Yabo Chen*&nbsp;&nbsp;·&nbsp;&nbsp;Haibin Huang&nbsp;&nbsp;·&nbsp;&nbsp;Chi Zhang&nbsp;&nbsp;·&nbsp;&nbsp;Xuelong Li (corresponding)</em>
  <br><sub>Institute of Artificial Intelligence, China Telecom (TeleAI) · IEEE Transactions on Multimedia (TMM), 2026</sub>
  <br><sub>* Equal contribution</sub>
</p>

---

This repository hosts the **project page** (`index.html`) for the paper, deployed
via GitHub Pages. It is a single, dependency-free HTML/CSS/JS file with a
cinematic "film director" theme — clapperboard loader, viewfinder cursor,
film-strip scroll scrubber, and an interactive **Director's Console** that lets
you drive a simulated explorable world in real time.

🌐 **Live site:** [https://whydahuzi.github.io/Directing-the-World.github.io/](https://whydahuzi.github.io/Directing-the-World.github.io/)

---

## ✨ Highlights

Given an input image, an **SMPL human-motion sequence**, and a prescribed
**camera trajectory**, *Directing the World* generates stable **15–20s**
long-horizon world-model videos with consistent human motion *and* coherent
camera-based world exploration.

- **🧠 MMPL autoregressive backbone** — Plan-then-Populate generation that
  predicts sparse planning frames (incl. a terminal anchor) to constrain each
  block, connecting consecutive segments into temporally consistent long videos
  with stable world memory.
- **🤸 Human motion control** — SMPL sequences injected as structured 3D
  guidance via a **t-guided Dynamic Projection** that adapts motion conditions to
  the timestep-dependent denoising latent — coarse-to-fine control that also
  supports simultaneous multi-person motion.
- **🎥 Causal-aligned camera control** — a camera pathway that decouples global
  Plücker-ray trajectory encoding from block-local feature injection, preserving
  long-range trajectory understanding while staying temporally aligned with
  block-wise autoregressive generation.
- **⚡ Fast–Slow Memory training** — a differential learning-rate strategy:
  self/cross-attention layers stay *slow* to preserve the long-video prior,
  while new control modules adapt *fast*, stabilizing controllable post-training
  and reducing signal interference.
- **🗂️ 20M → 50K data pipeline** — built from 20M public iStock videos with
  synchronized video / text / SMPL / camera-trajectory annotations, filtered and
  aligned in a unified latent space, then split into a **~20K motion-centric**
  subset (Stage I) and a **~30K camera-centric** subset (Stage II).

---

## 📊 State-of-the-Art Results (excerpt)

Evaluated on **APRIL-AIGC/UltraVideo-Long** under motion-only, camera-only, and
joint motion-camera settings. Full table in the paper (Table I).

| Method | Refined | Motion | Camera | Overall Score | Consist | Quality | Motion Err. | ATE | RPE | RRE |
|---|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| FunCamera | — | — | ✓ | 3.401 | 86.59 | 59.77 | 0.213 | 0.601 | 0.101 | 0.970 |
| FunMotion | — | ✓ | — | 3.445 | 91.76 | 62.52 | 0.184 | — | — | — |
| WanMove | — | ✓ | ✓ | 3.401 | 89.67 | **64.23** | 0.194 | 0.669 | 0.077 | 0.685 |
| Uni3C | ✓ | ✓ | ✓ | 3.445 | 88.65 | 56.07 | **0.151** | 0.482 | **0.063** | 0.722 |
| **Ours** | ✓ | ✓ | ✓ | **3.868** | **90.25** | 62.98 | 0.161 | 0.532 | 0.070 | **0.349** |

> *Directing the World achieves the best Overall Score under joint human-motion
> and camera control, with strong temporal stability and the best orientation
> accuracy (RRE).*

---

## 🗺️ Project page sections

| Nav | Section | What's there |
|---|---|---|
| **Hero** | top | Title, authors, venue badge, teaser video, action buttons |
| **Abstract** | `#abstract` | Paper overview — why decouple then integrate |
| **Console** | `#deck` | Interactive Director's Console — sliders, gauges, toggles, live explorable world viewfinder |
| **Method** | `#method` | Two-tab pipeline figure (method + data), 4 component cards |
| **Agents** | `#control` | Multi-agent world control — paired SMPL-script → generated-world galleries |
| **Results** | `#results` | SOTA comparison table + parallax monitor wall of generated videos |
| **Data** | `#dataset` | Stage I (motion-centric ~20K) & Stage II (camera-centric ~30K) subsets |
| **Citation** | `#bibtex` | BibTeX with copy-to-clipboard |

---

## 📁 Repository layout

```
.
├── index.html                  # the entire project page (HTML + inline CSS/JS)
├── README.md                   # this file
└── assets/
    ├── teaser.mp4              # hero teaser video
    ├── teaser-poster.jpg       # teaser poster frame
    ├── pipeline.png            # method framework figure (Fig. 2)
    ├── data_pipeline.png       # dataset construction figure (Fig. 5)
    ├── pair_{a,b,c,d}.mp4      # generated-world clips (multi-agent section)
    ├── pair_{a,b,c,d}_smpl.mp4 # matching rendered SMPL motion scripts
    ├── res_*.mp4               # long result videos (monitor wall)
    ├── smpl_*.mp4              # SMPL conditions for the results
    ├── scenes/
    │   └── scene_*.jpg         # poster thumbnails for each result video
    └── direct_the_world_data_pipeline.pdf
```

> **Note on assets:** the media in `assets/` are placeholders/sample renders for
> the project page. Replace them with the official released videos/figures when
> the camera-ready materials are available.

---

## 🚀 Local preview

No build step. Just open the page, or serve the directory so the videos load
with the right MIME types:

```bash
# Python 3
python -m http.server 8000
# then visit http://localhost:8000

# or Node
npx serve .
```

To open the file directly (no server), some browsers restrict autoplaying media —
serving the directory is recommended.

---

## 🌍 Deploy (GitHub Pages)

This repo is a **GitHub Pages user/project site**: pushing to the default branch
(`main`) publishes the page automatically.

```bash
git add index.html assets/ README.md
git commit -m "Update project page"
git push origin main
```

The site goes live at the URL above within a minute or two. Make sure the
repository **Settings → Pages** source is set to the `main` branch / root.

---

## 📚 Citation

If you find this work useful, please cite:

```bibtex
@article{wang2026directing,
  title   = {Directing the World: Fast Autoregressive Video Generation with
             Compositional Human-Camera Control},
  author  = {Wang, Haoyuan and Chen, Yabo and Huang, Haibin and Zhang, Chi and Li, Xuelong},
  journal = {IEEE Transactions on Multimedia (TMM)},
  volume  = {XX},
  number  = {XX},
  year    = {2026}
}
```

---

## 🙏 Acknowledgements

This project page theme (clapperboard loader, viewfinder cursor, film-strip
scrubber, Director's Console) was designed to echo the paper's "directing the
world" metaphor. Fonts: Inter, Space Grotesk, JetBrains Mono (Google Fonts).
The framework builds upon [MMPL](https://arxiv.org/abs/2508.03334) and draws
inspiration from [Uni3C](https://arxiv.org/abs/2504.14899).

## License

Project page code is released for demonstration purposes. The paper, dataset,
and model weights are subject to their respective licenses — see the paper for
details.
