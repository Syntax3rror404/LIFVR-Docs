# Lumina Interaction Framework VR (LIFVR) Documentation

## 6. Destructibles

Basicly there are two types of destructibles: 

| Type                                 | Recommended Component/Usage                   | Description                                                          | Example Blueprints                     |
|--------------------------------------|-----------------------------------------------|----------------------------------------------------------------------|----------------------------------------|
| **Destructibles which are grabable** | Destructible Grab Component                   | Similar behavior as physics actors (bodies)                          | BP_ExampleDestructible, BP_WineBottle  |
| **Other destructibles**              | Normal Geometry Collections                   | More static objects with limited interactions, like walls, shooting targets. | BP_ShootingTarget, BP_ShootingTarget2 |

### 6.1 Destructible Grab Component

Setting up a destructible actor to grab is quite simple. Create a new actor or make a child of the example BP : `BP_ExampleDestructible`. 

- The root component should be the static mesh, which needs to have `simulate physics` enabled and setup as any grabable actor: grab tag, collision settings, hand posing method etc. (see: [grabbing](interactions.md)).

- You need to create a geometry collection (GC) of the static mesh and add the geometry collection component to this actor.

- Add the **Destructible Grab Component** component and write the name of the static mesh component in the variable: `Physics Mesh Name` (under Settings).

Further settings of the component:

| Variable             | Type  | Description                                                                                      |
|----------------------|-------|--------------------------------------------------------------------------------------------------|
| Break Sound          | USoundBase | Sound that will automatically be played if the destructible breaks                               |
| Destroy After Break  | bool  | Enable/Disable to remove the broken particles after some time                                    |
| Broken Lifetime      | float | Define the time in seconds after which the particles should be removed if Destroy After Break is enabled |
| Allow Destruction    | bool  | You can prohibit destruction by disabling this and allowing it in game                           |
| Enable Clustering    | bool  | Use clustering in the Geometry Collection (GC)                                                   |
| Override Materials   | bool  | Enable this if you want to automatically use the materials of the static mesh for the GC         |

**Radial impulse**

You can additionally add a radial impulse on the breaking impact if you want a more explosive effect.

**No Simulating Hit Actor**

This section is if the actor should be able to brake by hits with components (actors) which don't have simulate physics enabled. An example for this is the **BP_SimpleBullet**. It uses the projectile movement, without the component itself simulating physics. The component checks if the hit component/actor has the manual break tag and uses the defined settings: `Manual Impulse Multiplier` and `Manual Collision Strength` to execute the break logic.

### 6.2 Other Destructibles

For the other destructibles see **BP_ShootingTarget** as example and how to use chaos destruction and create geometry collections in UE5: [UE5 Documentation Chaos Destruction](https://dev.epicgames.com/documentation/en-us/unreal-engine/chaos-destruction-in-unreal-engine).