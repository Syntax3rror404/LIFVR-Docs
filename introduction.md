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

> **_NOTE:_** If you encounter issues where the HexaPhysicsRig does not initialize correctly, check that your guardian is set up properly and the floor level is tracked accurately. At the moment, there is no full body avatar, but that, along with teleportation and a stabbing system, will be the next features to be implemented in the framework.

The most important classes and components will be explained in more detail in the main part of this documentation.


### Lumina Tags
LIFVR uses tags for a lot of logic to make it user-friendly. For example to enable grabbing an actor you need to set "grab" into the actor tag.
The default tags of the framework are:


| Tag Name         | Tag Value | Description | Category      |
|------------------|-----------|-------------|---------------|
| GrabTag          |     grab      |  Enable grabbing an actor by adding this tag in the actor tag.    | User |
| PullTag          |     pull      |  Enable pull grabbing an actor by adding this tag in the actor tag.    | User |
| ClimbTag         |     climb      |   Enable additional features for climbing for which it's important to be 100% accurate. For example if you want to use bAlwaysClimbCrouch          | User |
| InteractionTag   |     interaction      |      This is an internal tag used to detect if an actor has an interaction point and solver.       | Internal |
| AutoCrouchTag    |     autocrouch      |     Enable Auto Crouch mechanics for a component of type physics body. Add this tag eather to the component itself or to the actor (actor tag)       | User |
| RotatingFloorTag |     rotate      |     Enable that the character rotates automaticly if standing on rotating floor (e.g. platforms). Add this tag to the rotating component of the actor or the whole actor if every floor component is rotating in it.      | User |
| PhysicsInputTag  |     input      |     This is an internal tag to detect if an actor has an physics input component.        | Internal |

**User** means it's a tag you have to set manually in the actor or component to enable the action.

**Internal** means that's an internal tag used to efficiently drive logic and you don't need to set it up. This is automaticly handled by the framework.

The tag name values are stored in a data asset called **DA_TagsConfig**. It can be found in the content folder: Plugins/LIFVR/LIFVR Content/Blueprints/Core.
If you want to change the names for the tags, change the text value on the right side of the map in this data asset and the framework automaticly uses the new tag values.

> **_Important:_** Do not rename or move this data asset to another location. The redirections are not automaticly updated in C++, so the framework can't find this asset anymore if you do so.