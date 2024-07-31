# Lumina Interaction Framework VR (LIFVR) Documentation

## 1. Getting Started

1. Enable this plugin in your Unreal Engine project under Plugins -> LuminaInteractionFrameworkVR (You will have to restart the engine after this).
2. Paste the predefined default project settings from the discord server to replace you're project settings. This will ensure you have the physical materials setup coorectly in your project and also start with default VR settings.


**How to create your VR character?**

It's recommended to create child classes from the basic Blueprint classes of LIFVR (like BP_HexaCharacter or BP_PhysicsHand_PullGrab_Right /_Left). You can also create your game classes directly from the C++ classes, but than you would need to setup the BP Childs and the settings yourself. In the basic BP classes everything is set up to work smooth.

**Which hand class should I choose?**

It's recommended to use one of the hand classes:
- **BP_VRPhysicsHand_PullGrab_Right/_Left** or BP_VRPhysicsHand_Right/Left (without pull grab)

- If you want to create a completely own version of the hand or don't need physical hands, you can use the BP_VRHand_right/_left class as parent.

- (Experimental: The class BP_VRPhysicsControlHand is still experimental and not finished / stable yet!)

