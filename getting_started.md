# Lumina Interaction Framework VR (LIFVR) Documentation

## 1. Getting Started

1. Enable this plugin in your Unreal Engine project under Plugins -> LuminaInteractionFrameworkVR (You will have to restart the engine after this).
2. Paste the predefined default project settings from the discord server to replace you're project settings. This will ensure you have the physical materials setup coorectly in your project and also start with common VR settings.


**How to create your VR character?**

It's recommended to create child classes from the basic blueprint classes of LIFVR (like BP_HexaCharacter or BP_PhysicsHand_PullGrab_Right /_Left). You can also create your game classes directly from the C++ classes, but than you would need to setup the BP children and the settings yourself. In the basic BP classes everything is set up to work out of the box.

**Which hand class should I choose?**

It's recommended to use one of the hand classes:
- **BP_VRPhysicsHand_PullGrab_Right/_Left** or BP_VRPhysicsHand_Right/Left (without pull grab)

- If you want to create a completely own version of the hand or don't need physical hands, you can use the BP_VRHand_right/_left class as parent.

- (Experimental: The class BP_VRPhysicsControlHand is still experimental and not finished / stable yet!)

Choose the hand classes for the right (R) and left hand (L) in the drop down menu in you're character class in the variables `Hand Class R/L`. You can find it in the character details panel under `Settings -> Hands`. Let `bUSeAutoSetup = True` (checked) so they are automaticly setup.

**Setting up the core**

LIFVR has it's own game mode: **BP_LuminaGameMode**. This should be the default game mode in you project. Otherwise you can set it in the world settings.

In **BP_LuminaGameMode** are already the standard classes setup:

- Default Pawn Class: BP_HexaCharacter <- Change this to you're child character class.
- Player Controller Class: LuminaPlayerController

The game instance to use is: **BP_LuminaGameInstance**. In it are the Settings object as well as the TagConfig object loaded and saved.

