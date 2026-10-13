# Spatial Forming: Technical Pre-Review

Version 0.2 · 12 October 2026 · Based on [CONCEPT.md](CONCEPT.md)

This is a critical pre-review, not an implementation plan. Every statement is labelled.

## Labels

| Label | Meaning |
|---|---|
| **[R]** | Original requirement of the initiator (from CONCEPT.md, unchanged) |
| **[P]** | Technical proposal (not a requirement) |
| **[K]** | Known technology. With a source link: checked against that source on 12.10.2026. Without a link: general state of the art, not re-checked |
| **[A]** | Unverified assumption |
| **[F]** | Possible future development, not part of a first prototype |
| **[Q]** | Open research question, only an experiment can answer it |

---

## Summary

1. **Many of the described functions already exist individually** [K]: camera hand tracking, hand-based modelling in VR, signed-distance-field ("digital clay") modelling, layers and history, procedural surfaces, per-region materials, language-model control of Blender.
2. **The genuinely open part** is the layer between hand, meaning and operation, the learning profile on top of it, and an open library of movements *and* functions. Whether this exact combination exists has not been checked [Q].
3. **Hardest problems:** capture precision, missing haptics, mode switching without confusion, traceability of a learning system, and editing early steps after the fact (topological naming problem).
4. **Recommendation:** measure sensor precision first (Experiment 0). Then a prototype with four or five basic functions. Defer learning, community library and AI modules, but **log confirmed operations from day one** so that learning data exists later.

---

## Prior art

| Work | What it is | Label |
|---|---|---|
| Surface Drawing (Schkolne, CHI 2001) | Creating organic 3D shapes with the hand and tangible tools | [K] [source](https://authors.library.caltech.edu/records/12r6y-23d65) |
| MIT inFORM (Tangible Media Group, 2013) | Physical shape display made of motorised pins, a real-world version of the pin-board mental model | [K] [source](https://designawards.core77.com/2014/recipients/inform-dynamic-shape-display/) |
| Gravity Sketch | Free VR modelling application | [K] [source](https://mixed-news.com/en/gravity-sketch-update-apple-vision-pro-version/), controller-free hand-tracking support [A] |
| Adobe Substance 3D Modeler (formerly Oculus Medium) | VR clay modelling | [K] [source](https://www.adobe.com/products/substance3d-modeler), internal representation as distance field [A] |
| Open Brush (formerly Tilt Brush) | Open-source spatial painting | [K], licence Apache 2.0 [A] |
| $1 / $P recognisers (Wobbrock et al.) | Gesture recognition from a few personal examples | [K] |
| FreeCAD | Parametric history and its limits (topological naming) | [K] [source](https://hackster.io/news/freecad-hits-1-0-after-two-decades-finally-sees-an-end-to-the-toponaming-problem-7e7ac2df3e63) |

**Conclusion:** hand-based spatial modelling is not new. Any claim of novelty would require a systematic search (Google Patents, DEPATISnet, ACM Digital Library, arXiv).

---

## 1. What is possible with existing technology

| § | Requirement [R] | Possible today? | Notes |
|---|---|---|---|
| 2 | Spatial measuring field (pin-board model) | **Yes, digitally** | [P] The digital counterpart is a volume or signed distance field: a grid of millions of points, each storing inside/outside/distance to surface. [K] OpenVDB, libfive |
| 2 | Sensor arrangement at the monitor | **Yes** | [K] Webcam + MediaPipe; stereo and depth cameras (Luxonis, supported by Monado) |
| 3 | Capture hands, fingers, sequences | **Yes, with precision limits** | [K] MediaPipe: 21 landmarks per hand. OpenXR: 26 joints per hand. Precision at a desk: [Q] |
| 3 | Geometric calibration | **Yes** | [P] Measure finger lengths, range of motion, resting tremor once. Data protection: see 3.3 |
| 4 | Controlled capture | **Yes** | [K] USB foot switches usually register as keyboards. Other variants are software logic |
| 5A | Grab, move, rotate, scale primitives | **Yes** | [K] Standard in VR modellers; controllable in Blender via Python |
| 5B | Free forming (add/remove volume) | **Yes** | [K] VR clay tools, SDF libraries. In Blender, a prototype via metaballs is conceivable; speed [A] |
| 6 | Mode-dependent meaning of a movement | **Yes, but risky** | [K] Standard in 3D software. Known problem: mode errors (users forget the active mode) |
| 7 | Layers, history, branches | **Partly** | [K] Blender collections, modifiers, Geometry Nodes, undo. A persistent, branchable history over free forming is not built in [A] |
| 7 | Selective correction of an early step | **Limited** | [K] Topological naming problem: changing an early step breaks later references. FreeCAD 1.0 mitigates it, "not completely solved" |
| 8 | Parametric surfaces | **Yes** | [K] Geometry Nodes, displacement, procedural textures. Nature patterns must be built as templates |
| 9 | Material and finish per region | **Yes** | [K] Blender material slots per face; geometry/material separation is a core principle there |
| 10 | Natural-language form terms | **Partly** | [K] BlenderMCP connects language models to Blender. Reliable translation of terms like "as if cast in one piece" into geometry: [Q] |
| 10 | Personal meaning lexicon | **Yes (simple)** | [P] A table "term → confirmed operation with parameters". Technically trivial; the content is the real work |
| 11 | Learning profile | **Partly** | [K] Few-shot gesture recognition ($P) and custom gesture training ([MediaPipe](https://ai.google.dev/mediapipe/solutions/customization/gesture_recognizer)) exist. Learning preferences and intentions: [Q] |
| 12 | Open community library | **Infrastructure yes, content no** | [K] Blender has an extensions platform with packages and versions. A library of movements and functions does not exist |
| 13 | Combinable sequences | **Yes, as operations** | [K] Nested node groups in Geometry Nodes. Replayed as raw hand movement, a sequence does not fit other geometry (see 3.2) |
| 14 | Context-aware suggestions | **Technically yes, value open** | [A] Image/text-to-3D models exist. Whether suggestions help or disturb while shaping: [Q] |
| 15 | Expert modules | **Yes, as classical tools** | [K] FEM simulation (FreeCAD FEM, CalculiX), Blender 3D-Print Toolbox |
| 16 | Hardware-independent chain | **Yes** | [P] Feasible if the movement format is defined early (section 4) |

---

## 2. Suitable open-source components

| Layer | Component | Use | Note |
|---|---|---|---|
| Capture | **MediaPipe Hand Landmarker** | Webcam hand tracking, 21 landmarks, world landmarks in metres | [K] [docs](https://github.com/google-ai-edge/mediapipe/blob/master/docs/solutions/hands.md). z is relative to the wrist only |
| Capture | **MediaPipe Gesture Recognizer** | Train custom gestures | [K] |
| Capture | **Monado / Mercury** | Open OpenXR runtime with optical hand tracking | [K] [source](https://monado.freedesktop.org/handtracking), "still in development" |
| Capture | **OpenXR hand tracking** | Open standard, 26 joints | [K] [spec](https://registry.khronos.org/OpenXR/specs/1.1/man/html/XR_HAND_JOINT_COUNT_EXT.html). [P] candidate for the internal standard format |
| Capture | **LucidGloves** | Open DIY finger-tracking glove | [K] [repo](https://github.com/LucidVR/lucidgloves), precision [A] |
| Filtering | **One Euro Filter** | Jitter reduction with low latency | [K] Casiez et al., CHI 2012 |
| Recognition | **$P / $1 recognisers** | Gestures from few personal examples | [K] |
| Engine | **Blender** (Python API, Geometry Nodes, Asset Browser, Extensions) | Display, materials, surfaces, export | [K] GPL. Add-ons using bpy must be GPL-compatible according to the Blender Foundation's position [A] |
| Bridge | **BlendArMocap** | Shows MediaPipe → Blender works in real time (for animation) | [K] [source](https://www.blendernation.com/2022/06/08/blendarmocap-free-motion-capture-add-on-release-version/), reference only |
| Free form | **libfive** (MPL / GPL), **OpenVDB** | Function-based and volume modelling | [K] [libfive](https://libfive.com/) |
| Language | **BlenderMCP** | Language model controls Blender | [K]. Security risk if free Python is executed [A] |
| Materials | **Poly Haven, ambientCG** (CC0), **MaterialX** | Free materials, open format | [K] |
| Not usable in the EU | **Hunyuan3D** | Image-to-3D | [K] Licence territory excludes the EU, UK and South Korea ([licence](https://huggingface.co/tencent/Hunyuan3D-2/blob/main/LICENSE)) |
| Research only | **MANO**-based models (HaMeR, WiLoR) | Full hand mesh | [K] "for research purposes" ([MANO](https://mano.is.tue.mpg.de/)) |

---

## 3. Limits, unclear assumptions, contradictions

### 3.1 Technical limits

| Limit | Why | Proposal [P] |
|---|---|---|
| **Precision** | A single camera estimates depth; jitter and finger occlusion add error [K] | Measure (Experiment 0). Hands for coarse form, numbers or snapping for exact dimensions |
| **No haptics** | The hand feels no resistance, exactly what clay provides [K] | Strong visual and audio feedback; haptic glove only as [F] |
| **Fatigue ("gorilla arm")** | Holding arms in the air tires within minutes [K] | Working volume above the desk, elbows supported |
| **Real time in Blender** | Mesh edits via Python every frame are slow for large meshes [A] | Preview with metaballs or own distance field; mesh at the end |
| **Selective correction** | Topological naming problem [K] | Make early steps correctable as *parameters*, not geometry. State limits openly |

### 3.2 Tensions inside the requirements

1. **"Learns me" [R] vs. "traceable, never overrides" [R].** A learning system may interpret the same movement differently next month.
   [P] The log always stores the **executed operation**, never only the movement. Interpretation is versioned. Every model stays reproducible however the profile changes.
2. **"No technical vocabulary" [R] vs. "meaning depends on mode" [R].** Modes are exactly what makes classical 3D software hard.
   [P] The active mode is always visible and only active while a foot switch or gesture is held (clutch principle), instead of staying on.
3. **"Reusable sequences" [R] vs. "same movement, different geometry" [R].** The concept names this itself.
   [P] Store sequences as **operations relative to the object** (e.g. "10 % of length, along the surface"), not as raw hand coordinates.
4. **"Proactive suggestions" [R] vs. "never change without approval" [R].**
   [P] Assistants write **only to a separate proposal branch**. Accepting is always a user action.
5. **"Works offline" [R] vs. "AI modules and community library" [R].
   [P] The core is complete without both. Modules and library are extensions.

### 3.3 Unclear assumptions

| Assumption | Issue |
|---|---|
| Calibration is not biometric [R] | Hand geometry is a known biometric trait (historically used in access control) [K]. Even without identification intent the data is personal. Classification under the GDPR needs legal review [Q] |
| "Millions of points" | Clear as a mental model; required resolution (mm? 0.1 mm?) not yet defined |
| "Intuitive" | Not testable without metrics. Proposal: time, deviation from target shape, perceived workload (NASA-TLX) |
| Community contributes functions | Executable code from strangers is a security risk. [P] Functions as declarative descriptions (data), not code |
| Target precision and use | Art object, 3D print or manufacturing-grade CAD? Decides sensor and geometry kernel. **Most important open decision** |

---

## 4. Architecture with clean separation [P]

```
 ┌──────────────────────────────────────────────────────────┐
 │ 1 SENSOR ADAPTERS  webcam | depth cam | glove | XR       │
 └──────────────┬───────────────────────────────────────────┘
                ▼  standard movement format (OpenXR joints, timestamps)
 ┌──────────────────────────────────────────────────────────┐
 │ 2 CONDITIONING     filter · calibration · working volume │◄── [P] calibration
 └──────────────┬───────────────────────────────────────────┘
                ▼
 ┌──────────────────────────────────────────────────────────┐
 │ 3 ACTIVATION       foot switch / gesture / button        │
 └──────────────┬───────────────────────────────────────────┘
                ▼  active segments only
 ┌──────────────────────────────────────────────────────────┐
 │ 4 INTERPRETATION   mode + context + profile + lexicon    │◄── [P] profile, lexicon
 │                    → proposed operation                  │◄── [L] library
 └──────────────┬───────────────────────────────────────────┘
                ▼  confirmation (immediate or implicit)
 ┌──────────────────────────────────────────────────────────┐
 │ 5 OPERATIONS       declarative, versioned schema         │
 │                    → OPERATIONS LOG (branches, layers)   │◄── [S] assistants: proposal branches only
 └──────────────┬───────────────────────────────────────────┘
                ▼
 ┌──────────────────────────────────────────────────────────┐
 │ 6 ENGINE ADAPTER   Blender (replaceable)                 │
 └──────────────────────────────────────────────────────────┘

 [P] personal, local, private   [L] public library, read-only, signed packages
 [S] assistant and expert modules: read the log, write proposals only
```

### Separate data stores

| Store | Content | Lifetime | Rule |
|---|---|---|---|
| **Raw movement** | Joint positions, time, device | Volatile | May be deleted without losing any model |
| **Operations log** | What was executed, with parameters | Permanent | A model must be reproducible **from this alone** |
| **Personal profile** | Calibration, gesture templates, meaning lexicon | Permanent, versioned | Local, exportable, deletable; never writes to the log directly |
| **Public library** | Forms, surfaces, materials, sequences | Versioned packages | Separate from the profile; nothing personal flows in without explicit consent |

---

## 5. Basic functions for a first prototype [P]

| # | Function | Tests which assumption |
|---|---|---|
| 1 | **Activation by foot switch** (pressed = hand acts) | Controlled capture without false triggers (§4) |
| 2 | **Add volume** (strand or lump from the hand) | Free forming like clay (§5B, core question) |
| 3 | **Remove volume** (same movement, other mode) | Mode-dependent meaning (§6) |
| 4 | **Grab and move/rotate** a primitive | Object-based modelling (§5A) |
| 5 | **Log with undo and one branch** | Non-destructive foundation (§7) |

At no extra cost: every confirmed operation is stored with its movement segment. That becomes the learning data for §11 later.

---

## Smallest experiments

### Experiment 0: sensor measurement (first)

- **Setup:** webcam at the monitor, MediaPipe, a ruler or grid on the desk.
- **Measure:** jitter and deviation of the index fingertip in millimetres, separately for left/right, up/down and depth.
- **Decision:** good enough for coarse shaping? If not, stereo or depth camera first, nothing else.

### Experiment 1: core idea

**Testable claim [Q]:** an organic form can be created by hand in space faster and with less workload than with a mouse.

- **Tool:** functions 1 and 2 only (foot switch + add volume).
- **Task:** recreate a simple organic target shape, e.g. a stem with a cap.
- **Comparison:** the same task with mouse sculpting in Blender.
- **Metrics:** time, shape deviation (Hausdorff distance), NASA-TLX.
- **Participants:** initiator plus at least three others. The result indicates a direction, not proof.

---

## 6. Deliberately deferred

| Deferred | Reason |
|---|---|
| Community library and infrastructure | Nothing to share before the core works |
| Learning profile (the learning itself) | Needs data first; only logging runs from day one |
| Natural language and meaning lexicon | A research question of its own |
| Creative suggestions, AI modules | Goal and value undefined; licensing partly problematic |
| Expert modules (physics, manufacturing) | Only needed with a concrete manufacturing target |
| Glove, portable capture space | Only if Experiment 0 shows the camera is not enough |
| Surface and material library | Blender already has this; connect later |
| Composite and automated sequences | Requires a stable operations schema |

---

## 7. Open design decisions

1. **What is the form for?** Art object, 3D print or manufacturing template?
2. **What precision is "good enough"** for a first step: centimetres or millimetres?
3. **One or two hands** in the first prototype?
4. **Screen or headset** for viewing?
5. **Who is the first user?** The initiator alone, or others from the start?

## 8. Open research questions

1. What precision does camera hand tracking achieve at a desk? [Q]
2. After how many confirmed examples does a personal profile measurably improve recognition, and when does it drift? [Q]
3. Does visual feedback sufficiently replace missing haptics? [Q]
4. How expensive is replaying a long operations log for free-form shapes? [Q]
5. Where is the boundary between free gesture and symbolic command that feels natural? [Q]
6. Does the full combination already exist (patents, papers)? [Q]
