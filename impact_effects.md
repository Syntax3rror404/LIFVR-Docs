# Lumina Interaction Framework VR (LIFVR) Documentation

## 5. Collision profiles, physical materials, impact effect component

### 5.1 Impact Effect Component

With the **Impact Effect Component** you can create different effects like sound effects (SFX), visual effects (VFX) (Niagara Effects) and decals on impact hits.

Thhis component uses the collision strength of the impact to define if a VFX or SFx should be played. 

| Variable                    | Type  | Description                                                                                       |
|-----------------------------|-------|---------------------------------------------------------------------------------------------------|
| Complete Ignore Velocity    | float | Effects will only be played if the collision strength exceeds this threshold.                     |
| Use Only Delay Between Hits | bool  | This is an alternative mode. Example usage in **BP_MovableCubeBase**.                             |
| Debug Mode                  | bool  | Enable debug messages for this component. The current collision strength will be displayed.       |



Sound effects
----

To play an impact sound you simply need to define the sound effect which should be played in the `Collision SFX array` ( `+` icon to add a new element in the array). The sound will be played if the collision strength exceeds the complete ignore velocity threshold. The volume of the sound scales by the collision strength. You can adjust this with the `Volume Multiplier`. In LIFVR default `physical materials` are setup. To use these you can simply copy and paste the `DefaultEngine.ini` in you're project:
1. Navigate to your project folder.
2. Navigate into the folder `Config`: replace the `DefaultEngine.ini` with the one you can find here: [LIFVR Important Code](https://discord.com/channels/1197557737987518534/1197571941482111130).

> [!IMPORTANT]
> You need to order the effects in the array in the same order of the physical surfaces in the project settings. So e.g. if you want to set a sound effect for wood and wood is the fifth element in the project settings -> physical surface you need to set the effect as fifth element of the array (index = 4).

This will automaticly set the prepared physical surfaces (materials). Alternatively you can set them manually in the project settings: `Physics -> Physical Surface`. More information about the `physical materials`: see [here].

You can define a different sound for each physical surface by adding them in the `Collision SFX array`. You can rename and change the physical surfaces (materials) in your project as you like, the only important thing here is to set the effects into the right index of the array. They must match the order of the physical surfaces in your project. The default order can be viewed in the variable `Physical Material Names`.

<img src="./images/PhysMatNames.png" style="width: 65%;">

> [!TIP]
> If you want to use one effect for all physical surfaces or without using these at all, you can simply set only the first array element (index = 0). It will use this effect for all physical surfaces.

> [!NOTE]
> Currently the component can only check the physical surfaces if you defined them in the materials of the primitive components/meshes. I'm working on an update in which you can also set up the physical materials by using the physical material overrides.

**Sound Effect Settings:** 
- Collision SFX : Array of sound effects : array containing the sounds to play ordered like the physical surfaces in the project settings. Default: index 0.



#### Sound Effects for the  Hexa Character
---


Visual effects
----

Decals
----

