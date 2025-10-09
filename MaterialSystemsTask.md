# 2309516: Material Systems Task

## 1. Stylized Water Material with World Position Offset

- Using World Position Offset I created a simple visual waves.  

### QUESTION: How does WPO affect performance compared to animated meshes?

![Gif of moving water](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtFGCT5016/refs/heads/main/ImagesAndGifs/2025-10-09%2013-05-29.gif?token=GHSAT0AAAAAADMZXQJVEH2CLZA4JVPKXSSI2HHU53A)


*Figure 1: Gif of final water texture.*

![Gif of moving water Topdown](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtFGCT5016/refs/heads/main/ImagesAndGifs/WavesTopdownGif.gif?token=GHSAT0AAAAAADMZXQJVSXN4646NMOP547RS2HHV6ZA)

*Figure X: Gif of waves*

### How can you control wave direction and intensity?

![Image of WPO Nodes](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtFGCT5016/refs/heads/main/ImagesAndGifs/WorldPositionOffsetNodes.png?token=GHSAT0AAAAAADMZXQJU5VI7IIHQKXXAB4MW2HHVT2A)

*Figure X: Screenshot of nodes which control visual waves*

- In the above screenshot I have a paramater named "Wave Size" which if increases the amplitude of each wave. TThe speed pin of the "Panner" node controls wave speed and direction. If the X and Y values of the Vector2d which links into speed were to be negative the direction of the waves would be flipped. If you were to change only one to negative you could also make the waves move in any of the four cardinal directions.

### What vertex density is required for a smooth wave?

### How does my water material respond to different lighting conditions?

- Due to the foam of the water being emissive, it still shows no matter what light is being applied to the water. Additionally when the scene is very dark it gets more difficult to see the normal map movement as opposed to when the scene is lit.

![Image is nodes for foam](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtFGCT5016/refs/heads/main/ImagesAndGifs/FoamNodes.png?token=GHSAT0AAAAAADMZXQJVYYQXOIF46YJEJWW22HHVO2A)

*Figure X: Nodes for creating the foam visuals around objects which tough the water*

![Image of the water texture in the dark](https://raw.githubusercontent.com/BradleyCurtisDev/TechnicalArtFGCT5016/refs/heads/main/ImagesAndGifs/FoamInDark.png?token=GHSAT0AAAAAADMZXQJV3DUEZM4TMR3R32I62HHVLSQ)

*Figure X: Screenshot of in engine water material without bringt scene lighting*


























## Documentation

- Make Sure to Cite these correctly later

https://dev.epicgames.com/community/learning/tutorials/pBal/epic-for-indies-tutorial-creating-a-water-shader-in-unreal-engine-5-6-using-blueprints-material-editor
