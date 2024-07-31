# Lumina Interaction Framework VR (LIFVR) Documentation

## 0. Introduction

LIFVR is a UE5 plugin designed for VR game developers, focusing on physics-based characters and interactions similar to those seen in games like Boneworks and Bonelabs, or the VRGK plugin from UE4. The core of this framework is developed in C++, but most functions and classes are accessible through Blueprints. **This means you don't need any C++ knowledge to use this plugin.** Additionally, there are commented Blueprint examples available to assist you and demonstrate various applications (for instance, `BP_SimpleGun` or `BP_ExampleDestructible`). For a quick start, refer to the section  [**1. Getting Started**](/getting_started.md) in this documentation.      

The plugin includes the following classes and components:

1. **Core**:
    *   Physics character: BP_HexaCharacter + BP_HexaPhysicsRig
    *   Physics hands:   BP_PhysicsHand (children: )
    *   Game instance + Game mode: <span style="color: #ADD8E6;">LuminaGameInstance</span> + BP_LuminaGameMode
    *   Save game object and settings object: LuminaSaveGame + LuminaSettings
    *   Character View Vignette: BP_Vignette (C++ base: <span style="color: #ADD8E6;">ViewVignette</span>)

2. **Components**:
    *   <span style="color: #ADD8E6;">Collision Solver Component</span> -> <span style="color: #ADD8E6;">Grab Handler Component</span>
    *   Destructible Component
    *   Impact Effect Component
    *   LuminaVRMovement Component
    *   PhysAttachComponent
    *   PullGrabComponent
    
    **Interaction Components**:
    *   Interaction Base Component (Base scene component for all interactionpoints)
    *   Interaction Solver Component (Actor component to manage interactionpoints)
    *   Aligner Components: BP_LeftAligner, BP_RightAligner (used to defineexact position for grabbing an interaction point and optionally ifaligner animations should be used)
    *   Grab Blocking Area

    **Physical Inputs (Button Components)**:
    *   Button Component (Base class for all other Button components)
    *   Lever Component 
    *   PushButton Component
    *   Slider Component

3. **Interaction Objects**:

    **Physical Inputs**:
    *   Base Button   ->   BP_Button (BP only example) or BP_LuminaButton (child) 
    *   Base Knob   ->  BP_Knob
    *   Base Lever  ->  BP_Lever
    *   Base Slider ->  BP_Slider
    *   Base Valve Wheel    ->  Valve_Wheel    

    **Tools**:
    *   Gun examples:   BP_SimpleGun, BP_Baloon_Gun + BP_Baloon, BP_FlyGun, BP_Thruster, Gun -> BP_Gun
    *   Projectiles:    BP_Projectile, BP_OutProjectile
    *   Grenades:   BP_Grenade, BP_RecycleGrenade
    *   Other tools:    BP_Crowbar, BP_SledgeHammer, BP_Dagger, BP_Katana, BP_Bucket, BP_Flashlight, BP_Hammer, BP_Can

    **Miscellaneous**:
    *   BP_Door (C++ base: Door)
    *   BP_Drawer (C++ base: Drawer)
    *   BP_Locker   (C++ base: Locker)
    *   BP_ShootingTarget (C++ base: ShootingTarget)
    *   BP_Elevator (C++ base: Elevator)
    *   BP_GrabPole, BP_SwingPlatform, BP_SlidePole, BP_Rope
    *   BP_Ladder, BP_StaticLadder
    *   Destructible Shooting targets: 
    *   Destructible actors: BP_Cup, BP_ExampleDestructible, BP_WineBottle
    *   
    
4. **Physic Areas**:
    *   PhysicsArea (C++ basic Chaos Field)
    *   No Gravity Area: BP_NoGravityArea
    *   Swimming Area:  BP_SwimmingArea
    *   Vector field area: BP_VectorField -> BP_JumpPad

5. **Menu**:
    
    *   BP_Menu (C++ base: Menu) with Widgets/UI: BP_MainMenu (C++ base: MainMenu) and BP_SwitchButton (C++ base: SwitchButton)

5. **Others**:
    *   LuminaVRFunctionlibrary (a function library with useful functions you cann access in blueprints directly)

**Note**: At the moment, there is no full body avatar, but that, along with teleportation and a stabbing system, will be the next features to be implemented in the framework.

The most important classes and components will be explained in more detail in the main part of this documentation.
