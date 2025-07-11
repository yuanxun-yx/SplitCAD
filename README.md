
# SplitCAD

**Can we build mechanical 3D models like compiling a codebase?**

## ❌ What’s Broken in Today’s CAD

### 🔒 Proprietary, binary formats

* Most mainstream tools (SolidWorks, NX, Creo, Fusion 360, CATIA, Inventor, etc) store models in opaque, undocumented binary blobs — unreadable to humans and incompatible with Git
* Even small changes produce noisy diffs, making version control, collaboration, and peer review difficult or impossible
* PDM systems are heavyweight, locked-in, and fundamentally hostile to the Git-based workflows used in modern software and hardware development

### 🧱 Text-based ≠ Git-friendly

* Some systems (e.g. OpenSCAD, FreeCAD’s zipped .FCStd, or DXF) offer technically readable formats
* But they mix modeling intent (sketches, constraints, feature parameters) with geometry results (BREP, mesh, solver data, caches, etc.)
* Without strict source/result separation, even text formats remain noisy and unreliable in Git — diffs are polluted, changes misleading, and true modeling logic gets buried

### 🧩 Parametric logic is fragile and opaque

* Systems like NX or Creo provide powerful parametric tools — but compared to code, they still lack modularity, reuse, and transparent dependency management
* Cross-part references often rely on absolute paths, measurement links, or external references that break easily
* There’s no way to define scoped variables, reusable functions, or modular logic blocks across parts
* Logic is encoded via GUI interactions, with no textual diff, audit trail, or reuse
* Failures from circular dependencies or bad rebuild order are hard to diagnose — there’s no dependency graph or logs

## ✅ A Better Paradigm: CAD like Code

SplitCAD is built on two core ideas:

### 🔨 Source/Result separation — like code and build artifacts

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

### 🖱️ GUI-first, logic-tracked

* Tools like CadQuery already adopt a source/result separation model — but they are code-first, not GUI-first, which makes them less accessible to most mechanical engineers
* Visual modeling is inherently spatial and interactive; requiring users to write code for every action breaks this intuitive workflow
* In contrast, our approach is GUI-first: users work visually, while every feature (sketch, extrude, pattern, etc) maps to an explicit source entry
* No hidden state — everything is tracked and reproducible

These ideas are not hypothetical. Tools like [KiCad](https://kicad.org/) have already proven the value of GUI-first, Git-native workflows in hardware design. KiCad stores structured text files (`.kicad_pcb`, `.sch`), enables readable diffs and reviewable PRs, and integrates cleanly with CI pipelines (e.g. [KiBot](https://github.com/INTI-CMNB/KiBot)). SplitCAD extends this proven paradigm to the domain of 3D mechanical CAD.

## 🧠 Lessons from Compiler History

Like compilers before GCC or LLVM, CAD tools have long been opaque, closed, and expensive. Once compilers became open and standardized, the ecosystem flourished — from static analysis to AI tooling.

**CAD may be next.**
Open geometry kernels like Open Cascade could fill the role LLVM played for compilers — providing a foundation for analysis, AI tooling, and new user paradigms.

## 📌 Status

This is a **proposal** for a better CAD workflow paradigm.

I do not plan to build this myself — but I hope it can inspire discussion or implementation by others. Feel free to fork, share, or reuse the ideas for as long as your project is either **open source** or **non-commercial**.
