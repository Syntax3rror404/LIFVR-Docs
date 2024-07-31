# Lumina Interaction Framework VR (LIFVR) Documentation

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

