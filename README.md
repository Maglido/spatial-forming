# Spatial Forming

**An open concept for shaping 3D objects with your hands, in the air, the way you would shape clay.**

> Status: **early concept, no code yet.** This repository is a request for comments.
> Concept initiated by Antonio Krämer, 12 October 2026.

---

## The idea in one paragraph

Most 3D software asks people to learn its language: menus, modes, modifiers, vertices. Spatial Forming asks the opposite question: can software learn the natural way a person shapes things with their hands, and translate it into 3D geometry? Hands are tracked by cameras, depth sensors or optional sensor gloves. Movements are interpreted as modelling operations and executed in an existing 3D engine such as Blender. Over time, the system learns from confirmed movements and corrections, so the software adapts to the person and not only the other way round.

## What this is not

- **Not a claim of novelty.** Shaping 3D forms by hand in space has been explored since at least *Surface Drawing* (Schkolne, CHI 2001), and tools such as Gravity Sketch or Adobe Substance 3D Modeler exist today. See [Prior art](docs/TECHNICAL_REVIEW.md#prior-art).
- **Not a product announcement.** There is no software, no company and no release date.
- **Not a new 3D engine.** The plan is to build on existing open tools.

What may be open is the **combination**: a hardware-independent pipeline, a personal and local learning layer, a personal meaning lexicon for natural-language form terms, and an open, community-extendable library of movements, functions, surfaces and workflows. Whether this combination already exists has **not** been verified.

## Core principles

1. **The software adapts to the person.** Not only the person to the software.
2. **The user decides when something happens.** Explicit activation (foot switch, gesture, button). Accidental movements never change a form.
3. **Nothing is destroyed.** Every change is recorded as an operation. History, layers and alternative branches stay available.
4. **Assistants suggest, they never overwrite.** Suggestions live in separate branches until the user accepts them.
5. **Exact things are computed exactly.** Classical algorithms and simulation tools are preferred over AI for precise calculations.
6. **Personal data stays personal.** Learning profiles are stored locally, separated from public library content.
7. **Works offline.** The core must be useful without internet access and without the community library.

## Proposed architecture (draft)

```
Sensor adapters        webcam | depth camera | glove | XR headset
      │                standardised hand data (e.g. OpenXR 26-joint model)
      ▼
Conditioning           filtering, calibration, working volume      ◄── personal profile
      ▼
Activation             foot switch / gesture / button: on - off
      ▼
Interpretation         mode + context + profile + lexicon          ◄── personal profile
      │                → proposed operation                        ◄── public library
      ▼  confirmation
Operations log         declarative, versioned, branchable          ◄── assistants: proposal branches only
      ▼
Engine adapter         Blender (replaceable)
```

**Key rule:** a model must be fully reproducible from the operations log alone, without raw movement data and without the personal profile.

Details: [Technical review](docs/TECHNICAL_REVIEW.md)

## The first question to answer

Before anything is built, one measurement decides most of the architecture:

> **Experiment 0:** How precise is camera-based hand tracking at a desk, in millimetres, along each axis, including depth?

If that is good enough for coarse shaping, the next step is **Experiment 1**: a single operation ("hold pinch to draw a soft strand of volume") compared against mouse sculpting in Blender on time, shape deviation and perceived workload.

See [Experiments](docs/TECHNICAL_REVIEW.md#smallest-experiments).

## Documents

| File | Content |
|---|---|
| [docs/CONCEPT.md](docs/CONCEPT.md) | The original concept and requirements, unchanged in intent |
| [docs/TECHNICAL_REVIEW.md](docs/TECHNICAL_REVIEW.md) | Critical technical pre-review: existing tools, limits, contradictions, architecture, experiments, open questions |
| [CONTRIBUTING.md](CONTRIBUTING.md) | How to give feedback or contribute |

## How you can help

- Point out **prior art** we missed, especially anything that already combines these parts.
- Share **measurements** of hand-tracking precision on real desks.
- Challenge the **architecture**. Critical objections are explicitly welcome.

Open an issue or start a discussion.

## License

Concept and documentation: **[Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)**. You may use, adapt and share this work, including commercially, as long as you credit the author.

Any future code will be released under an open-source licence compatible with the engines it connects to.

## Author

Concept: **Antonio Krämer**
Technical review and documentation prepared with AI assistance (Claude, Anthropic).
