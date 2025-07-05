
# SplitCAD

> A GUI-first, Git-native CAD system — like KiCad, but for 3D mechanical design

## ❌ What’s Broken in Today’s CAD

### 🔄 Git can’t track CAD files

- Files like `.sldprt` or `.f3d` are opaque binary blobs
- Even a tiny change shows up as a meaningless binary diff
- You can't review, merge, or collaborate cleanly
- PDM systems are heavy, locked-down, and hostile to Git workflows

### 🧩 Parametric logic is fragile, limited, and hard to manage

- Basic parameters and expressions exist — but anything beyond simple math quickly becomes fragile
- Referencing values across parts or assemblies (e.g. one part’s tab width = another’s slot) is possible only through brittle measurement-driven links, external references, or global variable hacks
- Although advanced CAD systems like NX and Creo provide mechanisms such as WAVE linking for robust cross-part references, these implementations depend heavily on opaque GUI-driven workflows and layered state contexts, making them hard to track, audit, or manage through version control
- Assembly structure changes often break rebuilds with no clear cause
- There’s no support for `if`, `for`, `function`, or structured reuse across parts
- Any complex constraint or dependency logic must be recreated manually via GUI operations — with no way to see or diff what’s going on

## ✅ A Better Paradigm: CAD like Code

SplitCAD is built on two core ideas:

### 1. GUI-first, logic-tracked

- Model your part in a clean, visual interface
- Every feature (sketch, extrude, pattern, etc) maps to an explicit source entry
- No hidden state - everything is tracked and reproducible

### 2. Source/Result separation — like code and build artifacts

```
models/enclosure.yaml     # source (Git-tracked)
output/enclosure.cache    # intermediate geometry (not Git-tracked)
output/enclosure.step     # result (built from source)
```

- Git tracks what matters: modeling intent
- CI can rebuild geometry from clean source
- Exported formats like `.step` are final outputs, not modeling state
- Intermediate files (like `.cache.cad`) store geometry to speed up GUI preview or CI testing, similar to `.o` files in software builds
- Build speed is not critical: geometry generation can be slower as long as results are cached, just like compilation in code projects
- GitHub diffs show `length: 10 -> 12`, not hex dumps

## 🖱️ How it Works

No code required — this is not OpenSCAD.  
You model visually, and the system writes a structured, trackable source file behind the scenes.

- GUI-first UX, no coding skills needed
- Git-native text format (e.g. YAML/JSON)
- Deterministic builds produce `.step`, `.stl`, `.png`, etc

## 🎯 Goals

* ✅ GUI-first modeling, no code required
* ✅ Parametric features tracked as clean logic
* ✅ Git-native text-based source files
* ✅ CI/CD friendly (build + validate on push)

## 🛠️ Comparison to KiCad

||KiCad|GitNativeCAD|
|-|-|-|
|Use mode|GUI-first|GUI-first|
|Files|`.kicad_pcb`, `.sch` (text)|`.yaml` / `.json` (text)|
|Output|Gerber, netlist|STEP, STL, preview|
|Versioning|Git-friendly|Git-friendly|
|Logic|Explicit net, footprint|Explicit parametric tree|
|CI support|Yes (e.g. KiBot)|Planned CI + preview builds|

## 🤝 Who is this for?

* Engineers tired of binary CAD files in Git
* Small teams without expensive PDM systems
* Hardware projects that want to work like software
* Developers who want PRs and semantic diffs, not blobs

## 📌 Status

This is a **proposal** for a better CAD workflow paradigm.

I do not plan to build this myself — but I hope it can inspire discussion or implementation by others. Feel free to fork, share, or reuse the ideas.
