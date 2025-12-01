# Material Systems Task - 2309516

## 1. Stylized Water Material with World Position Offset


### How does WPO affect performance compared to animated meshes?

- One of the main performance benefits of using World Position Offset (WPO) is that it reduces how much work the CPU has to do compared to animated meshes. Normally, the CPU would need to update the position of every vertex on the water surface each frame, which can quickly become expensive and lower performance. (WPO animated instanced static mesh smearing - Development / Rendering, 2023)

- The vertex movement is handled directly on the GPU through the material’s shader using WPO. This means the CPU doesn’t have to worry about those calculations and can instead focus on things like AI or physics. Since the water geometry itself stays static, multiple objects using the same water material can also be grouped into a single draw call, which helps further improve performance. (Frame vs. Mesh Animations, s.d.)


![Gif of moving water](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/2025-10-09%2013-05-29.gif)


*Figure 1: Gif of final water texture.*

![Gif of moving water Topdown](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WavesTopdownGif.gif)

*Figure 2: Gif of waves*

![Image of Material Editor](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WaterMaterialInEditor.png)

*Figure 3: Screenshot of material editor*

### How can you control wave direction and intensity?

![Image of WPO Nodes](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/WorldPositionOffsetNodes.png)

*Figure 4: Screenshot of nodes which control visual waves*

- The water material I made lets me easily control how the waves move and behave through a few exposed parameters. I set it up so that the direction of the waves can be changed by adjusting the X and Y values of a vector parameter, which are connected to a panner node to control the texture’s flow. (UE 5.1 : Single Layer Water With WPO that makes vertex normal render issue. - Development / Rendering, 2022) The height or intensity of the waves is managed with a simple scalar parameter that multiplies the wave displacement value. This makes it easy to tweak the wave amplitude, from gentle ripples to more intense, choppy water.


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

- A Material Parameter Collection lets multiple materials share the same parameters, which makes managing global effects much easier. Instead of changing values in each material one by one, I can just update a single parameter in the collection, and all materials using it will update automatically. (Using Material Parameter Collections in Unreal Engine | Unreal Engine 5.6 Documentation | Epic Developer Community, s.d.) This is great for things like time-of-day or weather systems, where lots of materials need to react at once. It’s also more efficient since the values are stored once on the GPU and shared across all materials, saving both time and performance. (Pros and Cons to control Material parameters via MPCs as opposed to DMIs - Programming & Scripting / Blueprint, 2024)


![Material Parameter Collection](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/MPCImage.png)


*Figure 8: Image of MPC_Effets, my Material Parameter Collection (MPC)*

### How does your system maintain visual consistency across materials?

- Using a Material Parameter Collection made it much easier to keep everything in the scene visually consistent. I put visual settings like wind direction, wind strength, and wetness levels into one shared collection, so all the materials that referenced it would react the same way. This meant that things like the water material direcion, underwater post proces material, landscape and foliage materials always matched, and I could update the whole scene’s look just by changing one or two values.


### What performance benefits does MPC provide?

- A few performance benefits that I personally noticed compared to managing lots of separate material instances is that updates were also much faster because changes to a single parameter automatically affected every material using it, instead of having to update each one individually. This lowered the amount of work the CPU had to do and also saved memory, since all the materials could share the same parameters instead of storing their own copies. MPC's also reduce the number of draw calls since fewer unique material states needed to be handled by the renderer. (Pat Florczak Portfolio - Tutorial: Intro to Unreal Engine Material Parameter Collections, s.d.)


## Dynamic Material Instance with Runtime Control

- When should you use DMI versus Material Instance Constant?
- How do you optimize DMI creation and updates?
- What gameplay scenarios benefit from runtime material modification?


## Parallax Occlusion Mapping (POM) Material

![Parallax Occlusion Mapping (POM) Material](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/POM_Material.png)

*Figure 9: Image of material with POM Effect*

### How does step count affect visual quality and performance?

- The step count in Parallax Occlusion Mapping (POM) was an important setting I had to balance between visual quality and performance. Increasing the step count made the depth effect look much more realistic and helped reduce visual issues like “stair-stepping.” However, I found that higher values also made the material heavier to render, since the GPU had to do more calculations for each pixel. (Been trying to optimize parallax occlusion mapping,here are some progress,come discuss!(need help!) - Development / Rendering, 2016)


![POM Nodes 1](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/ParallaxEffect1.png)

*Figure 10: Nodes for the Parallax Occlusion Mapping Material (Part 1)*

![POM Nodes 2](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/ParallaxEffect2.png)

*Figure 11: Nodes for the Parallax Occlusion Mapping Material (Part 2)*

### What types of surfaces benefit most from POM?

Parallax Occlusion Mapping works best on materials which would have small bumps and grooves like brick walls and cobblestone floors.

### How does POM compare to simple bump offset?

- POM is basically a more advanced version of simple bump offset. Both techniques try to make flat surfaces look like they have depth, but they work in different ways. Simple bump offset is simpler and lighter on the GPU because it just shifts the texture slightly to fake height, but it doesn’t handle steep angles very well, so the effect can look flat or broken from certain views. (Using Bump Offset in Unreal Engine | Unreal Engine 5.6 Documentation | Epic Developer Community, s.d.) POM, on the other hand, takes multiple samples along the view direction, which creates a much more convincing sense of depth, including overhangs and better perspective. The trade-off is that POM is more expensive to render, so it looks better but uses more GPU resources. (POM, Tessalation and Bump Offset, when should i use what? - Development / Rendering, 2017)


## Optional A: Post Process Material

![Post Process Effect](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/PostprocessEffectGif.gif)

*Figure X: Gif of Post Process Material effect*

- I created a simple underwater post process material using water normal maps to make a rippling effect on screen. I also added parameters from my MPC to control the direction of the waves as well as the intensity of the effect.


![Image of nodes for my Post Process Material](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtImages/refs/heads/main/PostProcessEffectNodes.png)

*Figure X: Image of nodes for my Post Process Material*


## Documentation

Been trying to optimize parallax occlusion mapping,here are some progress,come discuss!(need help!) - Development / Rendering (2016) At: https://forums.unrealengine.com/t/been-trying-to-optimize-parallax-occlusion-mapping-here-are-some-progress-come-discuss-need-help/80857
 (Accessed 09/10/2025).

Creation of Parallax Occlusion Mapping (POM) in details | Community tutorial (2024) At: https://dev.epicgames.com/community/learning/tutorials/kyXK/unreal-engine-creation-of-parallax-occlusion-mapping-pom-in-details
 (Accessed 09/10/2025).

Frame vs. Mesh Animations (s.d.) At: https://wulverblade.com/frame-vs-mesh-animations/
 (Accessed 08/10/2025).

How do I add the underwater post processing effect to a custom water body in UE? : r/unrealengine (s.d.) At: https://www.reddit.com/r/unrealengine/comments/1kju7tt/how_do_i_add_the_underwater_post_processing/
 (Accessed 15/10/2025).

Pat Florczak Portfolio - Tutorial: Intro to Unreal Engine Material Parameter Collections (s.d.) At: https://qdwach.artstation.com/blog/leE3/tutorial-intro-to-unreal-engine-material-parameter-collections
 (Accessed 11/10/2025).

POM, Tessalation and Bump Offset, when should i use what? - Development / Rendering (2017) At: https://forums.unrealengine.com/t/pom-tessalation-and-bump-offset-when-should-i-use-what/99812
 (Accessed 09/10/2025).

Pros and Cons to control Material parameters via MPCs as opposed to DMIs - Programming & Scripting / Blueprint (2024) At: https://forums.unrealengine.com/t/pros-and-cons-to-control-material-parameters-via-mpcs-as-opposed-to-dmis/2203056
 (Accessed 11/10/2025).

Tutorial: Creating a Water Shader in Unreal Engine 5.6 Using Blueprints & Material Editor | Community tutorial (2025) At: https://dev.epicgames.com/community/learning/tutorials/pBal/epic-for-indies-tutorial-creating-a-water-shader-in-unreal-engine-5-6-using-blueprints-material-editor
 (Accessed 15/10/2025).

UE 5.1 : Single Layer Water With WPO that makes vertex normal render issue. - Development / Rendering (2022) At: https://forums.unrealengine.com/t/ue-5-1-single-layer-water-with-wpo-that-makes-vertex-normal-render-issue/723032
 (Accessed 08/10/2025).

UE4 l Wavy Water Post Process Effect l 5-Minute Post Process Tutorial l Unreal Engine 4.26 - YouTube (s.d.) At: https://www.youtube.com/watch?v=2mIhnGa3q7o
 (Accessed 15/10/2025).

UE5 l Realistic Textures with Parallax Occlusion Mapping (POM) l Material Tutorial l Unreal Engine 5 - YouTube (s.d.) At: https://www.youtube.com/watch?v=K18qfcTFkNw
 (Accessed 09/10/2025).

UE5: Create Realistic & Customizable Pool or Still Water Material w/Single Layer Water Shading Model (2025) Directed by WorldofLevelDesign. At: https://www.youtube.com/watch?v=eVT_rwCjRZU
 (Accessed 08/10/2025).

Unreal Engine 5.2 Tutorial: How to make a Underwater effects (2023) Directed by Thunder Splash. At: https://www.youtube.com/watch?v=-I-yuORUJVQ
 (Accessed 15/10/2025).

Using Bump Offset in Unreal Engine | Unreal Engine 5.6 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/using-bump-offset-in-unreal-engine
 (Accessed 12/10/2025).

Using Material Parameter Collections in Unreal Engine | Unreal Engine 5.6 Documentation | Epic Developer Community (s.d.) At: https://dev.epicgames.com/documentation/en-us/unreal-engine/using-material-parameter-collections-in-unreal-engine
 (Accessed 12/10/2025).

World Displacement Material (water) (2021) Directed by Gordon Vart. At: https://www.youtube.com/watch?v=OFmWEIb_Z70
 (Accessed 11/10/2025).

WPO animated instanced static mesh smearing - Development / Rendering (2023) At: https://forums.unrealengine.com/t/wpo-animated-instanced-static-mesh-smearing/776920
 (Accessed 10/10/2025).

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