# Lumina Interaction Framework VR (LIFVR) Documentation

Welcome to the LIFVR documentation. Here, you'll find basic information about functions and classes, along with tips, tricks, and recommendations for working with the LIFVR Plugin. The C++ API documentation will be available soon.

<span style="color: #FF6666 ;">Note: In all LIFVR classes and components, you can find the relevant variables for customization and configuration under the category Settings. Simply click on the actor or component you wish to configure and search for 'settings' in the details panel.</span>

## 0. Introduction

LIFVR is a UE5 Plugin for VR Game developers with the intend of a physics based character and interactions in VR (similar to Boneworks/Bonelabs style or the VRGK Plugin of UE4). The core of this framework is written in C++ and most of the functions and classes are accessible through blueprints. **This means you don't need any C++ knowledge to use this plugin.** There are also Blueprint examples with comments to help you and show some applications (see for example: **BP_SimpleGun** or **BP_ExampleDestructible**). If you want a really fast starting point look into the part **1. First Steps** of this documentation.

The Plugin contains the following classes and components:

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

**Note**: At the moment there is no full body avatar, but that as well as teleportation and a stabbing system will be the next features to be implemented in the framework.

The most important classes and components will be explained in more detail in the main part of this documentation.

## 1. First Steps

1. Enable this plugin in your Unreal Engine project under Plugins -> LuminaInteractionFrameworkVR (You will have to restart the engine after this).
2. Paste the predefined default project settings from the discord server to replace you're project settings. This will ensure you have the physical materials setup coorectly in your project and also start with default VR settings.


**How to create your VR character?**

It's recommended to create child classes from the basic Blueprint classes of LIFVR (like BP_HexaCharacter or BP_PhysicsHand_PullGrab_Right /_Left). You can also create your game classes directly from the C++ classes, but than you would need to setup the BP Childs and the settings yourself. In the basic BP classes everything is set up to work smooth.

**Which hand class should I choose?**

It's recommended to use one of the hand classes:
- **BP_VRPhysicsHand_PullGrab_Right/_Left** or BP_VRPhysicsHand_Right/Left (without pull grab)

- If you want to create a completely own version of the hand or don't need physical hands, you can use the BP_VRHand_right/_left class as parent.

- (Experimental: The class BP_VRPhysicsControlHand is still experimental and not finished / stable yet!)

## 2. LIFVR Character

### 2.1 Character hierarchy and structure
The base class for the Hexa Character is the **<span style="color: #ADD8E6;">VRControllerCharacter</span>**. This class handles all inputs (enhanced input actions, mapping context).

The Hexa Character (BP_HexaCharacter) is designed to be able to interact with the VR world completely based on physics. It's highly inspired by Boneworks/Bonelabs and the VRGK (Virtual Reality Game Kit) character. This means the character is driven by a Hexa Physics Rig (BP_HexaPhysRig) which contains the basic collision objects which are joined by physic constraints. The collision solver component enables to track any collisions of different body parts in the Hexa Physics Rig which than drive logic in the HexaCharacter. 

The character class is like the manager of everything to do with it like spawning and calling the functions in the hexa physics rig, LuminaVRMovementComponent or the specified VR hands.

#### 2.1.1 Character Data (Data assets)
Main characteristics like the body proportions of the HexaPhysics Rig and the Hexa Character are defined by a **CharacterProportionsDA**. The dataasset is assigned in the variable ```BP_HexaCharacter -> CharacterProportions```. The **DefaultCharacterProportionsDA** is set up to fit the UE5 Mannequins proportions. 
You can customize the character further by creating (duplicating the default data asset or creating a child data asset from the class CharacterProportionsDA) your own data asset and assigning it in the HexaCharacter to the varaible **CharacterProportions**. In the data asset you can eather specify the exact local position of the pelvis or using the LegToBodyRatio. The **Default Character Height** is the total height of the character in standing position. 

In the calibrate method the hmd will automaticly adjust the height to fit the virtual character height as defined in the data asset. It is called once at begin play after the player puts on the headset. The calibration can also be adjusted in the **MeinMenu** under **Character** (hold the B button for a few seconds to open the menu). The reference point for the hmd is floor level.

**Note:** If you have issues that the HexaPhysicsRig can't initialize correctly check if your guardian is set up fine and the floor level is tracked correctly.

Currently the other character features like strength, jump strength, speed etc. are not automaticly adjusted by these proportion setup. In the future there will be the possibility to enable automatic physics based calculations for these features. 

!!! Be aware that changes made in the proportions may also change the behavior of the character because everything is physics based !!!


The second character data asset is the **CrouchConfigDA** in this data asset it's possible to control the positions (height and backward leaning) of the character for the different crouch levels. For each crouch level you can define where the pelvis should be. Be aware that changes made here may also impact and change the behavior like the jump height for example if physical jumping is used. For a customized version it's recommended to create a child or duplicate of the **DefaultCrouchConfigDA** so that you have always the default one as backup. 


### 2.2 Locomotion: LuminaVRMovementComponent
The locomotion of the character is written in the **LuminaVRMovementComponent**. This is a completely new component of LIFVR (not a child of epics movement component). 

The Hexa Character has the following **movement states**:

```cpp
UENUM(BlueprintType)
enum class EVRMovementState : uint8
{
    Standing = 0,
    Walking,
    Sprinting,
    Jumping,
    Crouching,
    Climbing,
    Swimming,
    Tiptoeing
};
```

You can access them in the character by dragging in the LuminaVRMovementComponent reference and the function ```LuminaVRMovementComponent->GetMovementState()```.

In the details panel of the **LuminaVRMovementComponent** in the BP_HexaCharacter under the category **Settings** you can configure basicly everything in the Hexa Physics Character like walking, sprinting, swimming speed, jump strength, crouch speed and many more ... . In the following table you can see a overview of the variables for the different movement states.

![Logo](./path/to/logo.png)

It exists to main locomotion modes: 

    1.) Logical
    2.) Physical

The main features are implemented for the **Physical locomotion mode**. This one uses the HexaPhysicsRig for the HexaCharacter. So if you want the fully physics based character and in any children of the HexaCharacter you need to set the locomotion mode to ```Physical```!

The Logical locomotion mode exists if anyone wants to use mainly the VRHands (NOT VRPhysicsHands) and a more logical driven character. Than it is recommended to create a child of the ControllerCharacter and enhance / override functions in the LuminaVRMovementComponent to add logical based locomotions. In this locomotion mode is only already implemented: Logical movement (see method MoveLogical(FInputActionValue& Value)) and Turning (snap or smooth turn). 

For Logical jumping implement event or override method    
    - in C++ : ```DoLogicalJumping()```    
    - in blueprint :  ```LogicalJumping()```


#### Moving 
#### Turning 
#### Climbing
#### Crouching / TipToe
#### Swimming
#### Jumping
#### Slopes

### 2.3 Default controls and how to change input mapping (enhanced inputs)
In the picture you can see the default input mapping of the hexa character ...

### 2.4 Motion Sickness Prevention
The Hexa Character has a Vigniette actor component which can be used to prevent motion sickness. It can be enabled and disabled in the main menu or in the LuminaVRMovementComponent (Settings).

Teleportation will be added in the next update.

### 2.5 Slow Motion Mode
The character has an integrated slow motion mode. It's similar as in bonelabs/boneworks. By pressing the right thumbstick it can be enabled. With fast presses after each other one can slow down the time even more. After a threshhold time (variable: ...) which needs to be between the thumbstick presses another press will disable the slow motion. Note: Be aware that the slow motion mode can also effect physical calculations/impacts. 

You can get the current slow motion time dilation in any actor placed in the world by the node ... (getactortimedilation), See example: shooting sound in **BP_SimpleGun**.

## 3. VR Physics Hands
How the hands work: pd switch, strength switch, collisions, loose grabbing, index/middle grab, surface animations, hand animations and interfaces, animation types: basic poser, grab circle with modes ....
- switching hand poses: function explenation 

How to change the VR hands from the default quinn hands ... .

How to change the default hand animations...

## 4. Interactions with objects
### 4.0 Collision Solver Component
This component handles more complex collisions between actors, like "soft/normal" collisions (will fire every custom tick (defined in variable: ) as long the collision is occurung / the actor touches another object) or hard collision (fires once if a threshold of the collision strength is hit) and also registeres the end of the collision (fires once). (Note: The end of the collision is not completely stable and sometimes chaos collision ends are missed to track. Hope this gets better with the developement of chaos, but also work in progress to improve this).

it gives access to the following events:

| Event Category          | Event Name                   | Description                                                                      | Outputs                                                                                     |
|-------------------------|------------------------------|----------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------|
| **Actor Collisions**    | **On Actor Colliding**       | Continuously fires as long as the actor collides (soft/normal).                  | - Other Actor<br>- Current Collision Strength                                                |
|                         | **On Actor Hard Colliding**  | Fires once each time the collision strength exceeds the `HardCollisionThreshold`.| - Other Actor<br>- Current Collision Strength                                                |
|                         | **On Collision Ended**       | Fires once when the actor is no longer colliding.                                | None                                                                                        |
| **Component Collisions**| **On Component Colliding**   | Fires if a specified component in `ComponentsNameArray` is colliding, when `UseComponentsHit = True`. | - Other Actor<br>- Colliding Component<br>- Current Collision Strength                      |
|                         | **On Component Hard Colliding** | Fires if a specified component in `ComponentsNameArray` is hard colliding (above hard collision threshold), when `UseComponentsHit = True`. | - Other Actor<br>- Colliding Component<br>- Current Collision Strength                      |
|                         | **On Component Collision Ended** | Fires if a specified component in `ComponentsNameArray` is no longer colliding, when `UseComponentsHit = True`. | - Colliding Component                                                                       |

**Output description**

- **Other Actor**: The other actor involved in the collision. This output identifies which actor is colliding with the subject actor or component.
- **Colliding Component**: The component of this actor that triggers the collision event. This is particularly useful for understanding which part of the actor is involved in the collision.
- **Current Collision Strength**: Indicates the intensity of the collision at the moment of the event. Higher values represent stronger impacts.
- **Colliding Component (On Component Collision Ended)**: Specifies which component of the actor has stopped colliding, providing clarity on which part of the interaction has concluded.

![CollisionSolverEvents](./images/CollisionSolver_Events.png)
**Settings**

![CollisionSolverSettings](./images/CollisionSolverComp_Settings.png)

**Component Hits**

In complex actors you can also define which components should fire the collision events, so that not all primitive components with collision enabled are tracked by this component. 

This is for example used to track only some body part collisions of the HexaPhysicsRig for the HexaCharacter (otherwise the locomotion sphere would always fire the collision event).

In the details panel of clicked on the Collision Solver Component in the actor -> Settings -> ComponentsHit:

- ```UseComponentsHit = True```

- In the ```ComponentsNameArray``` you can write all the names of the components of this actor for which it should track the collisions.


### 4.1 Grabbing
Grab tag, how to setup/ create grabbable actors

### 4.1.1 Grab Handler Component
grab handler component (more precise control and strength switch, feeling weights)
This component is a children of the collision solver component, so all events and methods as described in subsection **4.0 Collision Solver Component** can be accessed from this component. It is recommended to use the collision solver component for all actors, so if you have a grab able actor only use this Grab Handler Component instead of the Collision Solver Component.

The Grab Handler Component enables precise control of the grabbing of actors. Because it also handles the strength of the VRPhysicsHands dynamically to feel right in all situations and there impact on the physics character this component should be added to all grab able actors. It also let you access the following events:

    - 

### 4.1.2 Interaction component
How to use it, needed components: interaction solver component, aligner components, aligner poses, dev tools: visualising flip/mirror preview, using gizmos, two hand stuff, physical settings, how to use button inputs,..

### 4.2 Pull Grab

## 5. Collision profiles, physical materials, impact effect component

## 6. Destructibles
### 6.1 Destructible Component
for grabable actors (how to setup/ use it). example BP_WineBottle, Cups
### 6.2 other Destructibles
direct GC collections -> example targets

## 7. Gun and projectile basics
Explenations because these are a bit mor complex to understand/use.

## 8. Physics areas
No gravity area, vector fields like jump pad, explosions of the grenade etc...

## 8. Tools and miscellenous
short explanations for tools like grenades, simple guns, projectiles, locker etc...

## 9. Drawing
**NOTE**: For drawing to work it's important to enable uv drawing in the project settings under .....


## 10. Physical inputs
how to use them ... 

## 11. Menu, Game mode, Settings, Save game

# FAQ

# Further ressources
