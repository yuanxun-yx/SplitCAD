
# SplitCAD

> A GUI-first, Git-native CAD system — like KiCad, but for 3D mechanical design

## ❌ What’s Broken in Today’s CAD

### 🔄 Git can’t track CAD files

* Files like `.sldprt` or `.f3d` are opaque binary blobs
* Even a tiny change shows up as a meaningless binary diff
* You can't review, merge, or collaborate cleanly
* PDM systems are heavy, locked-down, and hostile to Git workflows

### 🧩 Parametric logic is fragile, limited, and hard to manage

* Cross-part references rely on absolute paths — breaking easily when assemblies are moved or renamed
* There’s no support for modularity — expressions can’t be scoped across files, reused as functions, or composed into structured logic
* Failures from circular dependencies or broken links are hard to trace, with no clear logs or dependency graph
* All logic must be rebuilt through the GUI, with no way to automate or replicate via scripts


## ✅ A Better Paradigm: CAD like Code

SplitCAD is built on two core ideas:

### 1. GUI-first, logic-tracked

* Model your part in a clean, visual interface
* Every feature (sketch, extrude, pattern, etc) maps to an explicit source entry
* No hidden state - everything is tracked and reproducible

### 2. Source/Result separation — like code and build artifacts

| Concept        | Software Project      | SplitCAD                      |
| -------------- | --------------------- | ----------------------------- |
| Source         | `.cpp`, `.h`          | `.yaml`, `.json`              |
| Build Artifact | `.o`, `.obj`          | `.cache.cad`                  |
| Final Output   | `.exe`, `.wasm`, `.a` | `.step`, `.stl`, `.png`       |
| Compiler       | `gcc`, `clang`        | kernel (e.g. Open Cascade)    |
| CI/CD          | GitHub Actions, etc.  | Model checks + preview builds |


* Git tracks what matters: modeling intent
* CI can rebuild geometry from clean source
* Exported formats like `.step` are final outputs, not modeling state
* Intermediate files (like `.cache.cad`) store geometry to speed up GUI preview or CI teasting, similar to `.o` files in software builds
* Build speed is not critical: geometry generation can be slower as long as results are cached, just like compilation in code projects
* GitHub diffs show `length: 10 -> 12`, not hex dumps
* **AI-friendly**: With human-readable source files instead of opaque binaries, it's possible to train LLMs to analyze, repair, or generate CAD logic — similar to AI-assisted programming.

These ideas are not hypothetical. Tools like [KiCad](https://kicad.org/) have already proven the value of GUI-first, Git-native workflows in hardware design. KiCad stores structured text files (`.kicad_pcb`, `.sch`), enables readable diffs and reviewable PRs, and integrates cleanly with CI pipelines (e.g. [KiBot](https://github.com/INTI-CMNB/KiBot)). SplitCAD extends this proven paradigm to the domain of 3D mechanical CAD.

## 🧠 Lessons from Compiler History

Like compilers before GCC or LLVM, CAD tools have long been opaque, closed, and expensive. Once compilers became open and standardized, the ecosystem flourished — from static analysis to AI tooling.

**CAD may be next.**
Open geometry kernels like Open Cascade could fill the role LLVM played for compilers — providing a foundation for analysis, AI tooling, and new user paradigms.

## 🤝 Who is this for?

* Engineers tired of binary CAD files in Git
* Small teams without expensive PDM systems
* Hardware projects that want to work like software
* Developers who want PRs and semantic diffs, not blobs

## 📌 Status

This is a **proposal** for a better CAD workflow paradigm.

I do not plan to build this myself — but I hope it can inspire discussion or implementation by others. Feel free to fork, share, or reuse the ideas for as long as your project is either **open source** or **non-commercial**.
