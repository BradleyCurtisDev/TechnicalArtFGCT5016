# 2309516: Material Systems Task

## 1. Stylized Water Material with World Position Offset


### How does WPO affect performance compared to animated meshes?

- One of the main performance benefits of using World Position Offset (WPO) is that it reduces how much work the CPU has to do compared to animated meshes. Normally, the CPU would need to update the position of every vertex on the water surface each frame, which can quickly become expensive and lower performance.

- The vertex movement is handled directly on the GPU through the material’s shader using WPO. This means the CPU doesn’t have to worry about those calculations and can instead focus on things like AI or physics. Since the water geometry itself stays static, multiple objects using the same water material can also be grouped into a single draw call, which helps further improve performance.

https://wulverblade.com/frame-vs-mesh-animations/

https://forums.unrealengine.com/t/wpo-animated-instanced-static-mesh-smearing/776920



![Gif of moving water](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/2025-10-09%2013-05-29.gif)


*Figure 1: Gif of final water texture.*

![Gif of moving water Topdown](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WavesTopdownGif.gif)

*Figure 2: Gif of waves*

![Image of Material Editor](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WaterMaterialInEditor.png)

*Figure 3: Screenshot of material editor*

### How can you control wave direction and intensity?

![Image of WPO Nodes](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WorldPositionOffsetNodes.png)

*Figure 4: Screenshot of nodes which control visual waves*

- The water material I made lets me easily control how the waves move and behave through a few exposed parameters. I set it up so that the direction of the waves can be changed by adjusting the X and Y values of a vector parameter, which are connected to a panner node to control the texture’s flow. The height or intensity of the waves is managed with a simple scalar parameter that multiplies the wave displacement value. This makes it easy to tweak the wave amplitude, from gentle ripples to more intense, choppy water.

https://youtu.be/eVT_rwCjRZU?si=12qyHuU_uoQ-KakM

https://youtu.be/OFmWEIb_Z70?si=RfbAsgohn7eCqNfc

https://forums.unrealengine.com/t/ue-5-1-single-layer-water-with-wpo-that-makes-vertex-normal-render-issue/723032

![Lit Wireframe of object with wave material](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/LitWireframe.png)

*Figure 5: Lit Wireframe of object with wave material which shows wave displacement.*

### How does my water material respond to different lighting conditions?

- The water material is transparent which means that light can pass through and affect objects beneath the water. There is also a refraction effect which makes objects look like they are actually submerged.


![Image is nodes for foam](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/FoamNodes.png)

*Figure 6: Nodes for creating the foam visuals around objects which tough the water*

- Due to the foam of the water being emissive, it still shows no matter what light is being applied to the water. Additionally when the scene is very dark it gets more difficult to see the normal map movement as opposed to when the scene is lit.

![Image of the water texture in the dark](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/FoamInDark.png)

*Figure 7: Screenshot of in engine water material without bringt scene lighting*


## Material Parameter Collection (MPC) System

### Why use MPC instead of individual material parameters?

- A Material Parameter Collection lets multiple materials share the same parameters, which makes managing global effects much easier. Instead of changing values in each material one by one, I can just update a single parameter in the collection, and all materials using it will update automatically. This is great for things like time-of-day or weather systems, where lots of materials need to react at once. It’s also more efficient since the values are stored once on the GPU and shared across all materials, saving both time and performance.

https://dev.epicgames.com/documentation/en-us/unreal-engine/using-material-parameter-collections-in-unreal-engine

https://forums.unrealengine.com/t/pros-and-cons-to-control-material-parameters-via-mpcs-as-opposed-to-dmis/2203056

![Material Parameter Collection](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/MPCImage.png)


*Figure 8: Image of MPC_Effets, my Material Parameter Collection (MPC)*

### How does your system maintain visual consistency across materials?

- Using a Material Parameter Collection made it much easier to keep everything in the scene visually consistent. I put visual settings like wind direction, wind strength, and wetness levels into one shared collection, so all the materials that referenced it would react the same way. This meant that things like the water material direcion, underwater post proces material, landscape and foliage materials always matched, and I could update the whole scene’s look just by changing one or two values.

https://qdwach.artstation.com/blog/leE3/tutorial-intro-to-unreal-engine-material-parameter-collections


### What performance benefits does MPC provide?

- A few performance benefits that I personally noticed compared to managing lots of separate material instances is that updates were also much faster because changes to a single parameter automatically affected every material using it, instead of having to update each one individually. This lowered the amount of work the CPU had to do and also saved memory, since all the materials could share the same parameters instead of storing their own copies. MPC's also reduce the number of draw calls since fewer unique material states needed to be handled by the renderer.


## Dynamic Material Instance with Runtime Control

- When should you use DMI versus Material Instance Constant?
- How do you optimize DMI creation and updates?
- What gameplay scenarios benefit from runtime material modification?


## Parallax Occlusion Mapping (POM) Material

![Parallax Occlusion Mapping (POM) Material](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/POM_Material.png)

*Figure 9: Image of material with POM Effect*

### How does step count affect visual quality and performance?

- The step count in Parallax Occlusion Mapping (POM) was an important setting I had to balance between visual quality and performance. Increasing the step count made the depth effect look much more realistic and helped reduce visual issues like “stair-stepping.” However, I found that higher values also made the material heavier to render, since the GPU had to do more calculations for each pixel.

https://forums.unrealengine.com/t/been-trying-to-optimize-parallax-occlusion-mapping-here-are-some-progress-come-discuss-need-help/80857

https://dev.epicgames.com/community/learning/tutorials/kyXK/unreal-engine-creation-of-parallax-occlusion-mapping-pom-in-details



### What types of surfaces benefit most from POM?

Parallax Occlusion Mapping works best on materials which would have small bumps and grooves like brick walls and cobblestone floors.

### How does POM compare to simple bump offset?



### What are the limitations at silhouette edges?



## Optional A: Post Process Material

- Create a post-process material with a visible screen effect
- Choose blendable location (before/after tonemapping) appropriately
- Implement effect using Scene Texture nodes
- Examples: outline shader, screen distortion, custom color grading, stylized effect


## Documentation

- Make Sure to Cite these correctly later


https://youtu.be/-I-yuORUJVQ?si=8_RFpvSCU0QEsNNm

https://dev.epicgames.com/community/learning/tutorials/pBal/epic-for-indies-tutorial-creating-a-water-shader-in-unreal-engine-5-6-using-blueprints-material-editor
