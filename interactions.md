# Lumina Interaction Framework VR (LIFVR) Documentation

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
