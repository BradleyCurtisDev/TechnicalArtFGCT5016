# 2309516: Material Systems Task

## 1. Stylized Water Material with World Position Offset


### How does WPO affect performance compared to animated meshes?

- World Position Offset provides a performance advantage over using animated meshes for water because the WPO calculations only occur in the GPU for each instance of the mesh rather than updating every frame for animated meshes. Animated meshes cause more strain on the CPU because the water meshes vertex positions update ever frame which reduces frame rate. Animated meshes can be good for more detailed assets however with large or repetitive objects, using WPO materials reduces CPU overhead as they can be batched into a single draw call

![Gif of moving water](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/2025-10-09%2013-05-29.gif)


*Figure 1: Gif of final water texture.*

![Gif of moving water Topdown](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WavesTopdownGif.gif)

*Figure X: Gif of waves*

![Image of Material Editor](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WaterMaterialInEditor.png)

*Figure X: Screenshot of material editor*

### How can you control wave direction and intensity?

![Image of WPO Nodes](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WorldPositionOffsetNodes.png)

*Figure X: Screenshot of nodes which control visual waves*

- In the above screenshot I have a paramater named "Wave Size" which if increases the amplitude of each wave. TThe speed pin of the "Panner" node controls wave speed and direction. If the X and Y values of the Vector2d which links into speed were to be negative the direction of the waves would be flipped. If you were to change only one to negative you could also make the waves move in any of the four cardinal directions.

### What vertex density is required for a smooth wave?

![Lit Wireframe of object with wave material](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LitWireframe.png)

*Figure X: Lit Wireframe of object with wave material which shows wave displacement.*

### How does my water material respond to different lighting conditions?

- Due to the foam of the water being emissive, it still shows no matter what light is being applied to the water. Additionally when the scene is very dark it gets more difficult to see the normal map movement as opposed to when the scene is lit.

![Image is nodes for foam](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/FoamNodes.png)

*Figure X: Nodes for creating the foam visuals around objects which tough the water*

![Image of the water texture in the dark](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/FoamInDark.png)

*Figure X: Screenshot of in engine water material without bringt scene lighting*


## Material Parameter Collection (MPC) System

- Why use MPC instead of individual material parameters?
- How does your system maintain visual consistency across materials?
- What performance benefits does MPC provide?

## Dynamic Material Instance with Runtime Control

- When should you use DMI versus Material Instance Constant?
- How do you optimize DMI creation and updates?
- What gameplay scenarios benefit from runtime material modification?



## Parallax Occlusion Mapping (POM) Material

- How does step count affect visual quality and performance?
- What types of surfaces benefit most from POM?
- How does POM compare to simple bump offset?
- What are the limitations at silhouette edges?



## Optional A: Post Process Material

- Create a post-process material with a visible screen effect
- Choose blendable location (before/after tonemapping) appropriately
- Implement effect using Scene Texture nodes
- Examples: outline shader, screen distortion, custom color grading, stylized effect


## Documentation

- Make Sure to Cite these correctly later


https://youtu.be/-I-yuORUJVQ?si=8_RFpvSCU0QEsNNm

https://dev.epicgames.com/community/learning/tutorials/pBal/epic-for-indies-tutorial-creating-a-water-shader-in-unreal-engine-5-6-using-blueprints-material-editor
