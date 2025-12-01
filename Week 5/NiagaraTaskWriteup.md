# Week 5 Task – Niagara VFX Systems

[Place holder for Overview Image/GIF of all effects]
*Figure 1: Overview of the Niagara VFX Showcase.*

## 1. Fire Effect
### Description
[Describe the fire effect here. Is it stylized or realistic? How does it move?]

[Place holder for Fire Effect GIF]
*Figure 2: The Fire effect in action.*

### Technical Details
I used a [Sprite/Mesh/Ribbon] renderer for this effect. The motion is driven by...

| Parameter | Description |
| :--- | :--- |
| **User.Color** | Controls the tint of the fire. |
| **User.Intensity** | Adjusts the brightness/emissive power. |
| **[Other Param]** | [Description] |

### Curves
I utilized curves to control [Size/Alpha] over the particle's life (Normalized Age). This ensures the fire fades out naturally at the top.

## 2. Fireball Projectile
### Description
[Describe the fireball projectile. How does the core look? What about the trail?]

[Place holder for Fireball GIF]
*Figure 3: The Fireball projectile flying through the air.*

### Technical Details
The projectile consists of a core and a trail. The trail uses a [Ribbon/Sprite] renderer...

| Parameter | Description |
| :--- | :--- |
| **User.CoreColor** | Sets the color of the projectile core. |
| **User.TrailWidth** | Controls the width of the trail. |
| **User.Speed** | Adjusts the velocity of the projectile. |

## 3. Lightning Strike
### Description
[Describe the lightning strike. How does it arc? Does it flash?]

[Place holder for Lightning GIF]
*Figure 4: The Lightning Strike effect.*

### Technical Details
This effect uses a [Beam/Ribbon] renderer to create the arc between two points.

| Parameter | Description |
| :--- | :--- |
| **User.Start** | The starting position of the lightning. |
| **User.End** | The target position of the lightning. |
| **User.Color** | The color of the bolt. |
| **User.Width** | The thickness of the bolt. |

## 4. Slash Attack VFX
### Description
[Describe the slash effect. Does it follow the weapon? Is it stylized?]

[Place holder for Slash Attack GIF]
*Figure 5: The Slash Attack VFX synced with animation.*

### Technical Details
The slash is attached to the weapon socket and uses...

| Parameter | Description |
| :--- | :--- |
| **User.Color** | The color of the slash trail. |
| **User.Duration** | How long the trail persists. |
| **[Other Param]** | [Description] |

## 5. Decal Impact (Animation Notify)
### Description
[Describe the decal impact. What does it look like? How is it triggered?]

[Place holder for Decal Impact GIF]
*Figure 6: The Decal Impact triggered by the attack.*

### Technical Details
This effect is spawned via an Animation Notify (`ANS_DecalImpact`) in the montage. It performs a line trace to find the surface normal.

| Parameter | Description |
| :--- | :--- |
| **User.Scale** | The size of the scorch mark/crack. |
| **User.SurfaceNormal** | Aligns the decal to the hit surface. |

## 6. SubUV Flipbook Variant
### Description
[Describe the flipbook variant. How does it compare to the standard fire?]

[Place holder for Flipbook Comparison GIF]
*Figure 7: Side-by-side comparison of Standard Fire vs. Flipbook Fire.*

### Technical Details
This variant uses a texture sheet (SubUV) to animate the fire frames instead of procedural noise.

## Reflection & Challenges
### Guiding Questions
**How did you ensure the effects were performant?**
[Discuss CPU vs GPU choices, particle counts, etc.]

**How did you integrate the effects with gameplay?**
[Discuss Animation Notifies, Blueprints, etc.]

**What was the biggest challenge in creating these effects?**
[Discuss a specific technical hurdle and how you solved it.]

## References
- [Reference 1]
- [Reference 2]

## Declared Assets
- [Asset 1]
- [Asset 2]
