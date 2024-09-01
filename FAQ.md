# Lumina Interaction Framework VR (LIFVR) Documentation

## FAQ

> [!NOTE] 
> If you have any questions that you would like to see answered here, please let us know!

### 1. I can't find the plugin content in the content browser. 
----
Make sure you have enabled the plugin in your project. After enabling it a restart of the engine is required. Then open the **settings** of the content browser (in the content browser window in the right upper corner). There you need to check ```content -> Show Plugin Content```. Now You should see ```Plugins/LIFVR Content``` in the folder structure on the left side of the content browser.

Here you can find the entire BP available content, as well as the base BP classes to start creating your project logic.

### 2. I can't find the C++ classes of the framework.
----
The C++ classes can be found in the content browser under ```Plugins/LIFVR C++ Classes```. Analougously to 1.) make sure the plugin is enabled in the plugins section of your project and ```Show Plugin Content``` is enabled in the content browser settings. Furthermore you need to enable ```Show C++ Classes``` in the content browser settings.

Here you can find the basic classes of LIFVR. For Blueprint only users it's recomended to create child classes from the BP basic classes of the framework, instead of the C++ classes. This is the most easiest way to use and start scripting you're own logic. (So for example instead of creating a BP child class from the C++ class HexaCharacter use the BP_HexaCharacter in ```Plugins/LIFVR Content/Blueprints/Core/Characters/BP_HexaCharacter```).


### 3. The collision events of the collision solver component don't fire or 
### the character has to much impact on collisions of an grabbed actor with **GrabHandlerComponent** ?
----
If you have one of the upper issue check that in the grabable actor at least one primitive component has simulate physics enabled, has collision (query and physics) enabled and `Simulation Generates Hit Events` is checked. Further the collision preset physics actor should be used (object type: physics body).

If the collisions are still not tracked and the primitive component is a skeletal mesh, make sure that in the physics asset of this component also `Simulation Generates Hit Events` is enabled.

You can verify if the collisions are tracked correctly by clicking on the collision solver component/ grab handler component and adding in the details panel under `Events` the `OnActorColliding` and the `OnActorHardColliding` events into the event graph. (Add a print string node after each of it and connect them to the events.) The `OnActorColliding` event should be triggered if the primitve component has any collision. The `OnActorHardColliding` fires if the collision impact is higher than the `HardCollisionThreshold (default: 400)`. You can find the threshold variable under `Settings -> Collisions` of the **Collision Solver / GrabHandler Component**. 


### 4. I have a grab handler component and interaction points. Some variables are in both components like the Grabbing sound, so which variables should I use?
----
In general the interaction point variables will override the grab handler variables, so they are always prioritized. This gives the possibility to have for example different grabbing sounds for different interaction points in one actor. If no interaction point is found to grab the varaibles setup in the grab handler are used.