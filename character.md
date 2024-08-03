# Lumina Interaction Framework VR (LIFVR) Documentation

## 2. LIFVR Character

### 2.1 Character Hierarchy and Structure
The base class for the Hexa Character is the **<span style="color: #ADD8E6;">VRControllerCharacter</span>**. This class handles all inputs, including enhanced input actions and mapping contexts.

The Hexa Character (BP_HexaCharacter) is fully physics-based, enabling interactive VR world experiences. It is heavily inspired by Boneworks/Bonelabs and the VRGK (Virtual Reality Game Kit) character. The character is powered by a Hexa Physics Rig (BP_HexaPhysRig), which includes basic collision objects connected by physic constraints. A collision solver component tracks collisions of different body parts within the Hexa Physics Rig, driving logic in the Hexa Character.

The character class acts as the manager for all related classes, such as spawning and invoking the Hexa Physics Rig, LuminaVRMovementComponent, or the specified VR physics hands.

#### 2.1.1 Character Data (Data Assets)
Key characteristics like the body proportions of the Hexa Physics Rig and the Hexa Character are defined by a **CharacterProportionsDA**. This data asset is assigned to the variable `BP_HexaCharacter -> CharacterProportions`. The **DefaultCharacterProportionsDA** is configured to match the proportions of the UE5 Mannequins. You can further customize the character by creating (duplicating the default data asset or creating a child data asset from the class CharacterProportionsDA) your own data asset and assigning it to the variable **CharacterProportions** in the BP_HexaCharacter. In this data asset, you can specify the exact local position of the pelvis or use the LegToBodyRatio. The **Default Character Height** represents the total height of the character in a standing position.

During calibration, the HMD automatically adjusts the height to match the virtual character height as defined in the data asset. This method is called once at the beginplay or after the player puts on the headset. Calibration can also be adjusted in the **MainMenu** under **Character** (hold the B button for a few seconds to open the menu). The reference point for the HMD is floor level.

> **_NOTE:_** If you encounter issues where the HexaPhysicsRig does not initialize correctly, check that your guardian is set up properly and the floor level is tracked accurately.

Currently, other character features like strength, jump strength, speed, etc., are not automatically adjusted by these proportions. In the future, there will be the possibility to enable automatic physics-based calculations for these features.

> **_NOTE:_** Be aware that changes made in the proportions may also change the behavior of the character because everything is physics based.

The second character data asset is the **CrouchConfigDA** in this data asset it's possible to control the positions (height and backward leaning) of the character for the different crouch levels. For each crouch level, you can define where the pelvis should be. Be aware that changes made here may also impact and change the behavior like the jump height, for example, if physical jumping is used. For a customized version, it's recommended to create a child or duplicate of the **DefaultCrouchConfigDA** so that you always have the default one as a backup.

### 2.2 Locomotion: LuminaVRMovementComponent
The locomotion of the character is managed by the **LuminaVRMovementComponent**. This is a completely new component within LIFVR, not a derivative of Epic's movement component.

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

You can access them in the character by dragging in the LuminaVRMovementComponent reference and using the function `LuminaVRMovementComponent->GetCurrentMovementState()`.

<img src="./images/MovementState.png" style="width: 50%;">

In the details panel of the **LuminaVRMovementComponent** within the BP_HexaCharacter under the category **Settings**, you can configure essentially everything about the Hexa Physics Character, such as walking, sprinting, swimming speed, jump strength, crouch speed, and many more. 

<img src="./images/LuminaVRMovementCompSettings.png" style="width: 50%;">

Below is a table providing an overview of some of the variables in the settings of the LuminaVRMovementComponent:

There are two main locomotion modes:

    1.) Logical
    2.) Physical

The primary features are implemented for the **Physical locomotion mode**, which utilizes the HexaPhysicsRig for the HexaCharacter. Therefore, if you want a fully physics-based character and in any children of the HexaCharacter, you need to set the locomotion mode to `Physical`.

The Logical locomotion mode exists for those who prefer to use mainly the VRHands (NOT VRPhysicsHands) and a more logic-driven character. It is recommended to create a child of the ControllerCharacter and enhance or override functions in the LuminaVRMovementComponent to add logic-based locomotion. This mode has only implemented logical movement (see method `MoveLogical(FInputActionValue& Value)`) and turning.

For logical jumping, implement an event or override the method:
- in C++: `DoLogicalJumping()`
- in Blueprint: `LogicalJumping()`

#### Moving 

The character is moving through a locomotion sphere in the Hexa Physics Rig. The input values of the thumbstick are transformed to a torque for the locomotion sphere. The magnitude for the torque is scaled by the speed values, which you can set in the settings of the LuminaVRMovementComponent -> Walking.

<p>
<img src="./images/LocoSphere.png" style="width: 20%;">
<img src="./images/WalkingSetting.png" style="width: 65%;">
</p>

If the character crouches the movement speed is reduced automaticly. 

| Variable               | Description                                           |
|------------------------|-------------------------------------------------------|
| **Walking Speed**      | The speed at which the character walks if standing. |
| **Allow Sprinting**    | A boolean that enables or disables the ability to sprint. |
| **Sprinting Speed**    | The speed at which the character sprints. |
| **Use Auto Sprinting Speed** | A boolean setting that, when enabled, allows the system to automatically adjust sprinting speed based on other factors or conditions. |
| **Crouching Speed Scale**    | A speed factor that is multiplied by the movement speed when auto speed adjustments are made based on the percentage of crouching. |

For logical locomotion mode with logical movement you can set directly the speed while crouching ```LogicalCrouchingSpeed (float)``` and enable automatic sprinting speed changes ```UseLogicalAutoSprintingSpeed (boolean)```. These settings can be found under LuminaVRMovementComponent -> Settings -> Locomotion -> Walking-> Logical.


#### Turning 
#### Climbing
#### Crouching / TipToe
#### Swimming
#### Jumping
#### Slopes

### 2.3 Default Controls and How to Change Input Mapping (Enhanced Inputs)
In the picture below, you can see the default input mapping of the Hexa character. [Insert Picture or Provide Link]

### 2.4 Motion Sickness Prevention
The Hexa Character includes a Vignette actor component which can be used to prevent motion sickness. It can be enabled or disabled in the main menu or within the **LuminaVRMovementComponent** (Settings).

Teleportation will be added in the next update.

### 2.5 Slow Motion Mode
The character features an integrated slow motion mode, similar to what is seen in Bonelabs/Boneworks. By pressing the right thumbstick, this mode can be activated. Quick successive presses can further slow down the time. After a threshold time (variable: `[specify variable]`), which needs to be maintained between the thumbstick presses, another press will deactivate the slow motion. 

> **_NOTE:_** Be aware that the slow motion mode can also affect physical calculations.

You can retrieve the current slow motion time dilation for any actor placed in the world by using the node `GetActorTimeDilation`. For example, see how this is implemented for the shooting sound in **BP_SimpleGun**.


