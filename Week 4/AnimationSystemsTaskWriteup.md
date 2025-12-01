# Animation Systems Task


## 1. Animation Blueprint with Locomotion System
The core of the character's movement is handled by the `ABP_Player` Animation Blueprint. I set up a State Machine to manage the different movement states. This ensures smooth transitions between standing still, moving, and jumping.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/LocomotionGIF.gif"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/LocomotionGIF.gif" alt="Character Locomotion" width="100%"></a>


*Figure 1: The BS_Locomotion blend space showing the Locomotion State Machine.*

### State Machine Structure
The State Machine is composed of several key states.

| State Name | Description |
| :--- | :--- |
| **Locomotion** | This is the primary state for movement on the ground. It uses a Blend Space to blend between idle, walking, and running animations based on the character's speed. |
| **Jump_Start** | This state is entered when the character begins a jump. |
| **Jump_Loop** | A looping state that plays while the character is in the air. |
| **Jump_End** | The landing animation that plays when the character hits the ground. |

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/StateMachine.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/StateMachine.png" alt="Figure 2: State Machine Graph" width="100%"></a>

*Figure 2: The State Machine graph showing transitions between Locomotion and Jump states.*

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/Locomotion.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/Locomotion.png" alt="Locomotion Blend Space" width="100%"></a>

*Figure 3: The Locomotion Blend Space graph showing transitions between idle, walking, and running states.*

### Transitions and Variables
Transitions between these states are driven by variables updated in the Event Graph.

| Variable | Type | Description |
| :--- | :--- | :--- |
| **IsFalling** | Boolean | Determines if the character is in the air. When true, the system transitions from `Locomotion` to `Jump_Start`. |
| **GroundSpeed** | Float | Drives the Blend Space in the `Locomotion` state. This allows the character to smoothly accelerate from idle to run. |

The Event Graph updates these variables every frame by checking the character's movement component. This ensures the animation always matches the physics.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/EventGraphLilDude.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/EventGraphLilDude.png" alt="Event Graph Logic" width="100%"></a>

*Figure 4: The Event Graph logic updating IsFalling and GroundSpeed variables.*

### Guiding Questions
**How do you prevent animation popping during state transitions?**

To prevent popping, I utilized cross-fade durations in the state transitions. Instead of instantly switching from one animation to another, the engine blends them over a short period (e.g., 0.2 seconds). This creates a smooth visual progression. Additionally, using a Blend Space for locomotion ensures that changes in speed result in a continuous shift between idle, walk, and run animations rather than abrupt jumps.


## 2. Combat Combo System Using Animation Montages
For the combat system, I implemented a combo mechanic that allows the player to chain attacks together. This was done using Animation Montages and Blueprint logic in `AC_Combat`.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/ComboShowcase.gif"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/ComboShowcase.gif" alt="Combo Montage" width="100%"></a>

*Figure 5: GIF showcasing the combo system in action.*

### Combo Logic
The combat logic is handled within the `AC_Combat` component. When the player presses the attack button (Left or right Mouse Button), the system checks if an attack is already in progress. If not, or if the input falls within a specific "combo window," the `ComboCount` integer is incremented.


<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/HandCombatAttacks.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/HandCombatAttacks.png" alt="Figure 6: Combat Logic Blueprint" width="100%"></a>
*Figure 6: The AC_Combat Blueprint logic handling hand attacks and combo counting.*

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/KickCombatAttacks.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/KickCombatAttacks.png" alt="Figure 7: Combat Logic Blueprint" width="100%"></a>
*Figure 7: The AC_Combat Blueprint logic handling kick attacks and combo counting.*

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/AttackRotationAndEvents.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/AttackRotationAndEvents.png" alt="Figure 8: Combat Logic Blueprint" width="100%"></a>
*Figure 8: The AC_Combat Blueprint logic handling attack rotation and events.*

### Animation Montages and Notifies
The Attack Montages contain the animation data for these attacks. I used Animation Notifies to control the flow and gameplay effects.

| Notify Name | Description |
| :--- | :--- |
| **ANS_Attacking** | A custom notify state that marks the frame range where the character is actively dealing damage. |
| **ANS_HitTrace** | Triggers the actual hit detection logic during the swing. |
| **TimedNiagaraEffect** | Spawns visual effects, like lightning on the hands, to add impact to the punches. |

The montage is divided into sections. The Blueprint logic handles jumping between these sections to create a fluid combo chain. If the player stops pressing the attack button, the combo resets.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/AnimMontageTimeline.gif"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/AnimMontageTimeline.gif" alt="Figure 9: Animation Montage Timeline" width="100%"></a>

*Figure 9: The H2H_PunchCombo01_Montage GIF showing sections and notifies.*

### Guiding Questions
**How do you make the combo feel responsive while maintaining animation quality?**
Responsiveness is achieved by allowing input during the `ComboWindow` notify. This buffer means the player doesn't have to hit the button on the exact frame the next attack starts. However, to maintain animation quality, the character must finish the current "active" part of the swing (the attack frames) before transitioning. This balance ensures the animations have weight and impact but still feel under the player's control.

**How do you prevent button mashing while rewarding timing?**
The `ComboWindow` notify is placed specifically during the recovery phase of an attack. Inputs are only accepted during this window. If a player mashes the button too early (during the active swing), the input is ignored. This encourages the player to learn the rhythm of the animation and press the button at the right moment to continue the chain.

**How do you communicate combo windows to the player visually?**
Visual cues like the `TimedNiagaraEffect` (lightning on hands) help indicate the active phase of the attack. When the effect ends or the character begins to retract their arm, it signals the recovery phase, visually suggesting to the player that it is safe to input the next command.

## 3. Animation Layering & Slot System
To allow the character to attack while moving, I utilized animation layering. This ensures that the upper body can play combat animations while the lower body continues to run or walk.

### Layered Blend Per Bone
In the Anim Graph of `ABP_Player`, I added a `Layered Blend Per Bone` node. This node takes two animation poses.

| Pose Type | Source |
| :--- | :--- |
| **Base Pose** | The output from the Locomotion State Machine. |
| **Blend Pose** | The output from the `UpperBody` slot. |


<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/LayeredBlendPerBone.png"><img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/AnimationSystem/LayeredBlendPerBone.png" alt="Figure 5: Anim Graph with Layering" width="100%"></a>
*Figure 5: The Anim Graph showing the Layered Blend Per Bone node and Slot setup.*

### Slot System
The `H2H_PunchCombo01_Montage` is set to use the `UpperBody` slot. When the montage plays, its animation data is fed into the `Layered Blend Per Bone` node through this slot. This setup allows for a seamless mix of actions. It makes the gameplay feel much more dynamic and responsive.

### Guiding Questions
**What bone split creates the most natural upper/lower body separation?**
I found that splitting the hierarchy at `spine_01` created the most natural result. This bone is low enough to include the entire torso and arms in the upper body action, but high enough to leave the hips and legs controlled by the locomotion system.

**How do you handle the transition between full-body and layered animations?**
The `Layered Blend Per Bone` node handles this smoothly using blend weights. When the `UpperBody` slot is empty, the weight of the blend pose is effectively zero, so the full-body locomotion animation plays. When a montage plays in that slot, the weight increases, blending the upper body animation in.

**What blend weights feel most natural for your character?**
For this combat system, a blend weight of 1.0 (full override) for the upper body felt most natural during attacks. Since the attacks are forceful punches, we want the upper body to fully commit to the animation. Lower blend weights would result in a "weak" look where the idle/run sway interferes with the punch trajectory.



## References

- Animation Blueprints in Unreal Engine | Unreal Engine 5.5 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-blueprints-in-unreal-engine (Accessed 01/12/2025).

- Animation Montage in Unreal Engine | Unreal Engine 5.5 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-montage-in-unreal-engine (Accessed 01/12/2025).

- Animation Notifies in Unreal Engine | Unreal Engine 5.5 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/animation-notifies-in-unreal-engine (Accessed 01/12/2025).

- Blend Spaces in Unreal Engine | Unreal Engine 5.5 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/blend-spaces-in-unreal-engine (Accessed 01/12/2025).

- State Machines in Unreal Engine | Unreal Engine 5.5 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/state-machines-in-unreal-engine (Accessed 01/12/2025).

- Using Layered Animations in Unreal Engine | Unreal Engine 5.5 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/using-layered-animations-in-unreal-engine (Accessed 01/12/2025).

- Unreal Engine 5 - Blendspaces Explained (2024) Directed by Matt Courtois - Technically Animation. At: https://www.youtube.com/watch?v=s_E4lE80HBc (Accessed  01/12/2025).

- Layered Blend Per Bone | Adv. Anim Application [UE4/UE5] (2021) Directed by PrismaticaDev. At: https://www.youtube.com/watch?v=ja-C9VcNyKw (Accessed  01/12/2025).

- How To Make An Attack Combo System In Unreal Engine 5 (2024) Directed by Unreal University. At: https://www.youtube.com/watch?v=Q5xk5PYlQ1k (Accessed  01/12/2025).

## Declared Assets

- "Stylized_Tree_Pack" from Fab,
https://www.fab.com/listings/b39d5f78-9494-4a20-b029-24babc8405c5

- "Quarry_Rocks" from Fab,
https://www.fab.com/listings/6524e193-c690-4191-8b9a-33b62dd846f8

- "Wild_Grass_vlkhcbxia" from Fab,
https://www.fab.com/listings/50d9a417-73ed-4132-9421-6be3d4f7432e

- "Spring_Lungwort_flowering_plants" from Fab,
https://www.fab.com/listings/bc57eee9-a709-4553-a1ae-a488afb2a8a4

- `Week4/Animations` from Mixamo,
https://www.mixamo.com/

- "Rocky Sand" from Fab,
https://www.fab.com/listings/a913d25a-5488-4293-9049-8c942a4f8332

- "Fresh Windswept Snow" from Fab,
https://www.fab.com/listings/6ac5c478-8f2c-47cc-88e1-ac36aac010c8

- "Uncut Grass" from Fab,
https://www.fab.com/listings/1b93f569-aa1d-4f06-8452-03216dfbc1a1

- "Rock Cliff" from Fab,
https://www.fab.com/listings/22762313-071f-4017-a721-b29c5a2f1a87

- Blood Splatter Textures, 
https://drive.google.com/file/d/1NqWHqKRGV6WOAyvhrHaXgIxYYKH2bigd/view

- SubUV Flipbook Fire,
https://bit.ly/2uWwpwZ