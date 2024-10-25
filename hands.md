# Lumina Interaction Framework VR (LIFVR) Documentation

All questions about configuring grabable and interactible objects have a look here: [Grabbing and Interactions with objects](/interactions.md).
There is explained how to setup grabable objects, fixed grab poses and animation types, loose grabbing, index/middle grab, surface animations and grab circle with modes and much more.
- [Collision Solver Component](/interactions.md/#40-collision-solver-component)
- [Grabbing](/interactions.md/#41-grabbing)
    - [Grab Handler Component](/interactions.md/#411-grab-handler-component)
    - [Interaction Component](/interactions.md/#412-interaction-component)
- [Pull Grab](/interactions.md/#42-pull-grab)

## 3. VR Physics Hands

---

### _Table of Contents_
---
>:raised_hand: [Overview of VR Physics Hands](#30-Overview-of-VR-Physics-Hands)

>:gloves: [Custom Hand Mesh](#31-How-to-change-the-hand-skeletal-mesh-to-a-custom-hand-mesh?)

>:raised_hand: [Custom Hand Animations](#31-How-to-change-the-default-hand-animations?)

---


In this section we will give an overview how the physics hand work in LIFVR and how to change the hand mesh and default animations to custom ones.

### 3.0 Overview of VR Physics Hands

#### **Hand Classes Hierarchy**
---

The base parent class of all hands ios the VR_Hands class (BP_VR_Hands). This hand class does physical grabbing but has no physical connection to the character. **This class is not intended to be used with the BP_HexaCharacter!**.

How the hands work: pd switch, strength switch, collisions,
- switching hand poses: function explenation 

### 3.1 How to change the hand skeletal mesh to a custom hand mesh?
How to change the VR hands from the default quinn hands ... .

### 3.2 How to change the default hand animations?
How to change the default hand animations...
