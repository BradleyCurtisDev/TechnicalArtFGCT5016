# Auto Landscape Materials Task

[![FinalLandscapeImage](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/LadscapeOverviewImage.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/LadscapeOverviewImage.png)

## 1. Height-Based Blending System

### How does the height blending system function?

The main part of my landscape material uses a height blending system. This automatically changes between different layers like Dirt, Grass, Rock, and Snow based on the height of the landscape. I used the **LandscapeLayerBlend** node to handle these layers. This makes sure that each biome shows up at the right height.

To make the transitions smooth, I made some **Scalar Parameters** that control the height settings for each layer. For example, `SnowHeight` sets where the snow starts. `RockHeight` and `GrassHeight` control the lower parts. This setup lets me change the look of the landscape in real time without needing to recompile the shader.

| Parameter Name | Default Value | Description |
| :--- | :--- | :--- |
| **SnowHeight** | *Adjustable* | Controls the world height at which the snow layer begins to blend in. |
| **RockHeight** | 1.0 | Sets the elevation threshold for the rock layer. |
| **GrassHeight** | 1.0 | Defines the height range for the grass layer. |
| **DirtHeight** | 1.0 | Controls the base elevation for the dirt layer. |
| **FalloffRange** | *Adjustable* | Determines the softness of the transition between height layers. |


[![HeightBlendingNodes](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/MaterialBlendNodes.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/MaterialBlendNodes.png)


### How do you prevent harsh transitions?

To stop harsh lines appearing between materials, I used **Material Functions** for each layer. These are `MF_Snow`, `MF_Rock`, `MF_Grass`, and `MF_Dirt`. I blended them using alpha masks based on height. I added a `FalloffRange` parameter to control how soft the blend is. This makes sure it fades naturally instead of having a hard cut. This is important for keeping the biomes looking distinct while making the landscape look natural.

[![Image of Landscape Height Blend](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/MaterialBlendExample1.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/MaterialBlendExample1.png)

---

## 2. Slope-Based Blending System

### How is slope blending implemented?

For the slope blending, I used the **WorldAlignedBlend** function. This node works out the angle of the surface compared to the world. It lets me mask out areas based on how steep they are. This is needed to automatically put rock textures on cliffs and steep hills where grass or snow would not naturally be.

I made `BlendSharpness` and `BlendBias` parameters visible so I could adjust where the rock texture appears. A higher sharpness value makes a clearer edge for cliffs. The bias changes the angle threshold.

| Parameter Name | Default Value | Description |
| :--- | :--- | :--- |
| **BlendSharpness** | 15.0 | Controls how sharp the transition is between flat and steep surfaces. |
| **BlendBias** | -5.0 | Offsets the slope angle threshold, determining how steep a surface must be to change texture. |
| **RockBlendSharpness** | 15.0 | Specific sharpness control for the rock layer transitions. |
| **DirtBlendSharpness** | 15.0 | Specific sharpness control for the dirt layer transitions. |

[![Image of Slope Blending Nodes](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/SlopeBlend.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/SlopeBlend.png)



### How does slope blending enhance realism?

Slope blending makes the landscape look much more realistic. It ensures that vertical surfaces look structurally correct. By automatically putting a rock material on steep angles, the landscape looks like it has natural erosion. I also made sure that the normal maps were blended correctly. This means the lighting interacts realistically with the cliff faces and adds depth to the shape.

[![Image of Cliff Face](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/SlopeBlendExample.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/SlopeBlendExample.png)


---

## 3. Foliage Type Integration

### How is the foliage distributed?

I set up a **Procedural Foliage Spawner** called `PFS_Grassland` to put plants on the landscape automatically. This system works together with the landscape material blend layers. This makes sure that trees and flowers only spawn in places that make sense like grass layers. They are kept away from steep cliffs or snowy peaks.

The system includes a mix of foliage assets to create a varied ecosystem.

| Foliage Asset | Type | Description |
| :--- | :--- | :--- |
| **SFM_Grass** | Static Mesh | Basic grass clumps for general coverage. |
| **SFM_Flowers** | Static Mesh | Flowering plants to add color variance to the grasslands. |
| **SFM_Tree1** | Static Mesh | Primary tree type for forested areas. |
| **SFM_Tree2** | Static Mesh | Secondary tree variation for visual interest. |
| **SFM_Grass1** | Static Mesh | Additional grass variation for density. |

[![Image of Foliage Spawner Settings](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/PFS_Grassland.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/PFS_Grassland.png)



### How do you balance visual richness with performance?

To keep performance good while making it look dense, I used **Instanced Static Meshes** for all foliage types. This lets the engine render thousands of instances with very few draw calls. I also set up density rules in the foliage types to stop too many spawning in areas where the camera is less likely to look.

[![Image of Final Landscape](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/FoliageAreaViewport.png)](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LandscapeImages/FoliageAreaViewport.png)


---

## Documentation

- Landscape Materials in Unreal Engine | Unreal Engine 5.7 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/landscape-materials-in-unreal-engine (Accessed  16/10/2025).

- Material Functions in Unreal Engine | Unreal Engine 5.7 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/material-functions-in-unreal-engine (Accessed  16/10/2025).

- Procedural Foliage Tool in Unreal Engine | Unreal Engine 5.7 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/procedural-foliage-tool-in-unreal-engine (Accessed  20/10/2025).

- Save hours! Procedural Foliage in Unreal Engine 5 - Photoreal Landscape Tutorial (2023) Directed by Aziel Arts. At: https://www.youtube.com/watch?v=JpbWi95Bz20 (Accessed  20/10/2025).

- Slope Blending for Static Meshes - Development / Rendering (2016) At: https://forums.unrealengine.com/t/slope-blending-for-static-meshes/61903 (Accessed  17/10/2025).

- UE4 Tutorial: Material Functions (2019) Directed by underscore. At: https://www.youtube.com/watch?v=PRJnfMH7hxc (Accessed  16/10/2025).

- UE5 Landscape Auto Material - Slope Mask 101 (2022) Directed by Wonderscape Creations. At: https://www.youtube.com/watch?v=t0bJmNaoF08 (Accessed  17/10/2025).

- What are the difference between landscape layer blend types? - Development / Rendering (2016) At: https://forums.unrealengine.com/t/what-are-the-difference-between-landscape-layer-blend-types/380276 (Accessed  16/10/2025).
