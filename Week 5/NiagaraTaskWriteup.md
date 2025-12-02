# Week 5 Task – Niagara VFX Systems

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/NiagaraOverview.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/NiagaraOverview.gif" alt="Niagara VFX Showcase" width="100%" />
</a>
*Figure 1: Overview of the Niagara VFX Showcase.*

## 1. Fire Effect
### Description
A stylized fire effect that uses mesh particles to create a volumetric look. The fire moves upward with a natural, turbulent motion, simulating heat rising and flickering flames.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireShowcase.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireShowcase.gif" alt="Fire Effect Showcase" width="100%" />
</a>
*Figure 2: The Fire effect in action.*

### Technical Details
I used a **Mesh Renderer** for this effect to give the flames volume and a stylized appearance. The motion is driven by **Add Velocity** (configured as a Cone for upward spread) and **Curl Noise Force** to add turbulence and natural variation to the movement.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireSystem.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireSystem.png" alt="NS_Fire_System" width="100%" />
</a>
*Figure 3: Fire Niagara System .*



| Parameter | Description |
| :--- | :--- |
| **User.ParticleScale** | Controls the overall size of the fire particles. |
| **User.ParticleOrientation** | Controls the initial rotation/orientation of the particles. |

### Curves
I utilized curves to control **Scale Mesh Size** and **Scale Color** over the particle's life (Normalized Age). This ensures the fire grows/shrinks appropriately and fades out naturally at the top, shifting colors as it cools.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireEmitterCurves.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireEmitterCurves.png" alt="NS_Fire_System Curves" width="100%" />
</a>
*Figure 4: Fire Niagara System curves for scaling color and alpha over time.*

## 2. Fireball Projectile
### Description
The Fireball is a projectile actor (`BP_Projectile`) spawned by the player character (`BP_ThirdPersonCharacter`) when the **E** key is pressed. It utilizes the `NS_Fire_System` for its visual representation, which consists of a core fire effect and trailing embers.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireballShowcase.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireballShowcase.gif" alt="Fireball Projectile Placeholder" width="100%" />
</a>
*Figure 5: The Fireball projectile flying through the air.*

### Technical Details
*   **Blueprint:** `BP_Projectile` handles the projectile logic, including movement and collision.
*   **Niagara System:** `NS_Fire_System` is attached to the projectile.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireballBlueprint.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/FireballBlueprint.png" alt="Blueprint Logic for Fireball attack" width="100%" />
</a>
*Figure 6: Blueprint Logic for Fireball attack.*

*   **Emitters:**
    *   `NE_Fire`: Creates the main fire core using a Sprite Renderer and upward velocity.
    *   `NE_Embers`: Generates trailing sparks and embers.
*   **On Hit:** When the projectile hits an object, it spawns `NE_Test_System` to create an impact effect.

**Parameters:**
| Parameter Name | Type | Description |
| :--- | :--- | :--- |
| `User.ParticleScale` | Float | Scales the size of the fire particles. |
| `User.ParticleOrientation` | Vector | Controls the orientation of the particles, set by the projectile's rotation. |


## 3. Lightning Strike
### Visuals
The lightning strike effect creates a dynamic electrical arc between two points. It uses a ribbon renderer to simulate the jagged path of a lightning bolt, likely with a bright, emissive color that flashes briefly.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/LightningShowcase.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/LightningShowcase.gif" alt="Lightning Strike" width="100%" />
</a>
*Figure 7: The Lightning Strike effect.*

### Technical Details
**Blueprint:** `BP_LightningStrike`

**Niagara System:** `NS_Lightning`

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/LightningStrikeBlueprint.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/LightningStrikeBlueprint.png" alt="Blueprint Logic for Lightning Strike" width="100%" />
</a>
*Figure 8: Blueprint Logic for Lightning Strike.*

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/LightningSystem.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/LightningSystem.png" alt="NS_Lightning" width="100%" />
</a>
*Figure 9: Lightning Niagara System.*

*   **Renderer:** `Ribbon Renderer` to create the continuous arc.
*   **Logic:** The Blueprint calculates the start and end positions and passes them to the Niagara System via User Parameters.
*   **Modules:** Uses `FloatFromCurve` for various properties, likely controlling the jitter or width over the particle's lifetime.

| Parameter | Description |
| :--- | :--- |
| **User.StartPosition** | The starting location of the lightning bolt (Set in BP). |
| **User.EndPosition** | The target location of the lightning bolt (Set in BP). |
| **User.RibbonWidth** | Controls the thickness of the ribbon. |



## 4. Slash Attack VFX
### Visuals
The Slash Attack VFX consists of a lightning trail effect that follows the character's attacks, adding impact and visual flair to combat moves. It uses a ribbon renderer to create a continuous beam that jitters to simulate electricity.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/ComboShowcase.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/ComboShowcase.gif" alt="Slash Attack VFX" width="100%" />
</a>
*Figure 10: Slash Attack VFX in action.*

### Technical Details
**Niagara System:** `NS_Lightning`

*   **Emitter:** `Lightning`
*   **Renderer:** Ribbon Renderer (used to create the trail/beam effect).
*   **Key Modules:**
    *   **Beam Emitter Setup:** Configures the particle system to behave as a beam.
    *   **Spawn Beam:** Handles the spawning logic for the beam particles.
    *   **Beam Width:** Controls the width of the lightning ribbon, often modulated by a curve.
    *   **Jitter Position:** Adds random displacement to particle positions to create the jagged, electric look of lightning.
    *   **FloatFromCurve:** Used to scale properties like beam width or color over time.

### Usage in Montages
The VFX are triggered via `AnimNotifyState_TimedNiagaraEffect` in animation montages, attaching the system to specific sockets on the character mesh.

1.  **Punch Combo (`H2H_PunchCombo01_Montage`)**
    *   **System:** `NS_Lightning`
    *   **Socket:** `HandGrip_R` (Right Hand)
    *   **Trigger:** `AnimNotifyState_TimedNiagaraEffect` triggers the lightning trail during the punch animation.

2.  **Kick Attack (`H2H_Kick01_Montage`)**
    *   **System:** `NS_Fire_System` (Reused for kick impact/trail)
    *   **Socket:** `foot_r_Socket` (Right Foot)
    *   **Trigger:** `AnimNotifyState_TimedNiagaraEffect` triggers the fire effect on the foot during the kick.


## 5. Decal Impact (Animation Notify)
### Visuals
The Decal Impact effect creates a realistic blood splatter on the ground or walls where the attack connects. It combines a projected decal for the main pool/stain with sprite particles for flying droplets, giving it a visceral and dynamic feel.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/BloodDecalShowcase.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/BloodDecalShowcase.gif" alt="Blood Splat Showcase" width="100%" />
</a>
*Figure 11: Blood Splat Showcase.*

### Technical Details
**Niagara System:** `NS_BloodSplat`



*   **Emitters:**

    *   `Decal`: The primary emitter using a **Decal Renderer**. It projects the `M_SplashDecal` material onto the environment.
    *   `Splat`: A secondary emitter using a **Sprite Renderer** to spawn additional blood droplets/mist for added detail.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/BloodDecalSystem.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/BloodDecalSystem.png" alt="NS_BloodSplat" width="100%" />
</a>
*Figure 12: Blood Splatter Niagara System.*

*   **Material:** `M_SplashDecal`
    *   **Domain:** Deferred Decal
    *   **Textures:** Uses `T_Blood_Erosion` for the shape/color and `T_Blood_Erosion_N` for normal mapping to add depth.
    *   **Logic:** Includes a `SphereMask` for fading and `CustomRotator` to randomize the splatter orientation.

    
<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/DecalMaterialBlueprint.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/DecalMaterialBlueprint.png" alt="DecalMaterial" width="100%" />
</a>
*Figure 13: Decal Material Blueprint.*


## 6. SubUV Flipbook Variant
### Visuals
This uses a pre-rendered texture sheet to animate the fire, creating a stylized or specific look that differs from the procedural noise of the standard fire. It plays through a sequence of frames stored in a single texture.

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/SubUVFireShowcase.gif" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/SubUVFireShowcase.gif" alt="SubUV Fire Showcase" width="100%" />
</a>

*Figure 14: SubUV Fire Showcase.*

### Technical Details
**Material:** `M_Fire`

<a href="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/SubUVFlipbookMaterial.png" target="_blank">
  <img src="https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/NiagaraVFX/SubUVFlipbookMaterial.png" alt="SubUV Fire Material Blueprint" width="100%" />
</a>
*Figure 15: SubUV Fire Material Blueprint.*

*   **Texture:** `FlameSprites` (1024x1024) containing the fire animation frames.
*   **Grid Layout:** 4x4 (16 frames total), defined by the `Row/CollumnNum` parameter.
*   **Animation:** Uses the `FlipBook` material function driven by `Time` and `TimeRatio` to cycle through the UV coordinates.
*   **Motion:** Incorporates `SimpleGrassWind` for World Position Offset (WPO) to add swaying motion to the sprite.

| Parameter | Description |
| :--- | :--- |
| **TimeRatio** | Controls the playback speed of the flipbook animation. |
| **Row/CollumnNum** | Sets the grid dimensions (Default: 4). |
| **Glow** | Multiplier for the emissive intensity. |
| **WindIntensity/Speed** | Controls the swaying motion applied via WPO. |


