# Spatial Forming: Concept

Concept by Antonio Krämer · Concept state: 12 October 2026 · Phase: requirements and technical pre-review

This document describes the **original vision and requirements**. It is intentionally kept close to the initiator's intent. Technical proposals, known technologies and assumptions are kept separate in [TECHNICAL_REVIEW.md](TECHNICAL_REVIEW.md).

---

## 1. Background and motivation

The initiator is an independent artist and designer who wants to develop complex spatial forms without having to master every technical detail of classical 3D software.

The starting question: **Can three-dimensional objects be shaped with bare hands as intuitively as physical modelling clay?**

Central vision:

> The software should increasingly adapt to the natural way people work and express themselves, not only the other way round.

A commercial company is not the goal. A meaningful contribution to an open, community-developed tool would already be a success.

## 2. Original mental model: a spatial measuring field

The initial picture was a defined working volume filled with millions of tiny measuring or forming points. An analogy is a pin-art board, where pushed pins reproduce a three-dimensional contour.

Transferred to space: hand movements are detected and turned into digital forms.

**Important:** the point field is a *mental model*. Millions of physical lasers or moving points are explicitly **not** a technical requirement. Existing approaches should be examined instead, for example:

- multi-camera hand tracking
- depth sensors
- optical and other spatial measuring methods
- optional sensor gloves
- combinations of several sensor types

A compact sensor arrangement above or on the monitor is a possible first variant. A portable, larger capture space was discussed as a brainstorming idea, not as a requirement.

## 3. Hand tracking and personal calibration

The system should capture hand positions, finger poses, movement directions and spatial movement sequences.

Individual calibration should account for the geometry of a person's hands and their movement habits.

Biometric identification is **not** required and must not be confused with purely geometric calibration.

## 4. Controlled capture

The user must be able to decide exactly when a modelling operation starts and ends. Possible activation methods:

- foot switch
- menu or button
- countdown
- defined gesture
- activated movement mode

Unintended hand movements must not trigger shape changes.

## 5. Transferring the mouse principle to the hands

Familiar functions of classical 3D software should become accessible through direct hand interaction, for example:

- select primitives
- grab and move objects
- change size
- stretch or compress
- rotate
- press in
- create local deformations
- change colour and material

Two directions of work:

- **A. Object-based modelling:** existing primitives such as cube, sphere or cylinder are manipulated.
- **B. Free forming:** volumes and surfaces are built up, removed or changed without a fixed base shape.

Both must be combinable.

## 6. Selectable editing modes

The meaning of a hand movement is defined by the active mode and the editing context. Planned functions:

- add volume
- remove volume
- move
- scale
- pull apart
- compress
- rotate
- smooth
- structure surfaces
- freeze the current form state

The same hand movement may trigger different operations depending on the mode.

## 7. Non-destructive editing and versioning

The initiator describes a system of layers, drawers and earlier editing points. This results in requirements for:

- editing history
- separate form regions
- restorable intermediate states
- alternative development branches
- selective correction
- traceable changes

Earlier versions must be preserved. The concrete implementation, and the limits of changing operations after the fact, must be examined.

## 8. Parametric surface library

The software should offer structured surfaces, for example: smooth, rough, wavy, spiky, angular, scaled, ribbed, porous, crystalline.

Nature-inspired patterns could come from hedgehogs, sea urchins, reptiles, bark or coral.

Patterns should have adjustable parameters such as height, width, count, density, spacing, orientation and regularity. Users should be able to save their own templates, apply them locally and reuse them later.

## 9. Material and finish system

Independent of geometric surface structure, optical material design should be possible: matte, satin, glossy, metallic, transparent.

Materials can be assigned to a whole object or to specific regions.

The separation between actual geometry and purely optical material representation is technically essential.

## 10. Natural-language control

The initiator does not want to learn a complete technical vocabulary.

Expressions such as "more bulbous", "soft transition" or "as if cast in one piece" should be interpreted from context and translated into geometric operations.

Ambiguous expressions must be clarified through questions, previews and confirmation. Individually confirmed meanings can be stored in a **personal meaning lexicon**.

## 11. Learning movement and working profile

A central vision is gradually getting to know an individual's way of working. The initiator's analogy: two people getting to know each other and, over time, anticipating each other's behaviour better.

The system should recognise recurring relationships between:

- movement geometry
- hand poses
- chosen editing modes
- resulting forms
- corrections
- confirmed user intentions
- personal preferences

This can lead to better function suggestions or interpretations. It is a development goal, **not** a promise of near-perfect prediction.

Personal movement profiles should be stored locally where possible and kept separate from public library data.

## 12. Open movement and function library

The vision includes a worldwide, community-extendable library. Users and developers could voluntarily contribute:

- hand poses
- movement patterns
- editable function definitions
- forms
- surfaces
- materials
- templates
- combined modelling workflows

Content should be available by category and installable individually or as packages.

The core software must also work meaningfully without internet access and without the community library.

Contributions require suitable formats, versioning, licences, security reviews and compatibility rules. No such project-specific library exists yet.

## 13. Combinable and automatable movement sequences

Stored movements and modelling operations should be reusable. Several operations can be combined into a composite sequence, and these combinations can themselves become parts of new combinations.

Conceivable:

- sequential execution
- time-coordinated execution
- parameter changes
- repetitions
- semi-automatic sequences
- manual intervention during execution

The implementation must account for the fact that real hand movements do not automatically produce the same result on different object geometries.

## 14. Context-aware creative support

When a user pauses, the system could optionally offer suggestions. A pause must **not** automatically be interpreted as a creative block.

Possible assistance modes:

- only on explicit request
- subtle offers after adjustable inactivity
- proactive suggestions

Suggestions could come from community libraries, parametric variants or AI-generated drafts. **The original object must never be changed without approval.**

## 15. Specialised AI and expert modules

Specialised systems could be integrated for specific tasks, for example geometry, design, biology, material science, physics or manufacturing planning.

Selection can be automatic or manual.

Not all modules need to be AI systems. For exact calculations, suitable classical algorithms and simulation tools are preferred.

A central coordination layer should merge results **without overriding design decisions on its own**.

## 16. Hardware-independent architecture

The system should support different capture technologies in the long term. Proposed principle:

```
Sensor hardware → standardised movement data → interpretation layer → modelling operations → 3D engine
```

An existing engine such as Blender should be examined as a starting point. Building a complete 3D engine is neither intended nor justified.

## 17. Development approach

The project is first examined conceptually. In the long term, an open developer community is conceivable, in which independent people contribute individual modules.

---

## Documentation rules

To keep the original vision intact, all project documents distinguish:

1. **Original requirement** of the initiator
2. **Technical proposal**
3. **Known technology**
4. **Unverified assumption**
5. **Possible future development**

Known functions must not be presented as new inventions. Unproven technical properties must not be presented as established. Critical objections are explicitly welcome.
