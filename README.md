# DECO：Depth-guided co-visibility reasoning for low-altitude UAV visual localization

Low-altitude UAV images often contain many vertical structures, such as building facades and walls. However, orthographic reference maps mainly preserve top-down surfaces such as roofs and ground planes. This creates a **co-visibility gap**: many visually salient UAV keypoints do not have valid counterparts in the reference map. DECO addresses this problem by selecting keypoints that are not only visually distinctive, but also geometrically co-visible with the map.

<p align="center">
  <img src="framework_new1.png" width="90%">
</p>


## Core Idea

DECO works as a plug-and-play module before feature matching:

1. **Estimate monocular depth** from the UAV image.
2. **Recover local surface geometry** and compare surface normals with the gravity direction.
3. **Compute a Geometry-Saliency Coupled Co-visibility Score (GS-CoVis)** for each keypoint.
4. **Keep reliable UAV keypoints** that are both geometrically co-visible and visually distinctive.
5. **Match selected keypoints** with the orthographic reference map and estimate the UAV pose using PnP-RANSAC.

In short, DECO filters out keypoints on geometrically non-co-visible regions before matching, making UAV-to-map localization more robust.

## Results

We evaluate DECO on two low-altitude UAV localization benchmarks: **AnyVisLoc** and **OrthoLoC**. Experiments show that DECO consistently improves localization accuracy and correspondence quality across different depth models, feature detectors, and matchers.

Representative results with **SuperPoint + LightGlue**:

| Dataset / Scene       | Baseline T@1 | DECO T@1 | Baseline T@3 | DECO T@3 |
| --------------------- | -----------: | -------: | -----------: | -------: |
| AnyVisLoc Scene1      |         71.7 | **79.3** |         95.3 | **99.7** |
| AnyVisLoc Scene2      |         24.6 | **35.5** |         48.4 | **58.6** |
| AnyVisLoc Scene3      |         23.0 | **25.4** |         77.8 | **80.6** |
| AnyVisLoc Scene4      |         44.6 |     44.3 |         90.0 | **91.8** |
| OrthoLoC Cross-domain |         30.7 | **32.6** |         52.4 | **53.0** |
| OrthoLoC Same-domain  |         55.1 | **55.7** |         64.1 | **64.5** |

<p align="center">
  <img src="visualization_new.png" width="90%">
</p>


## Highlights

- Training-free co-visibility reasoning for UAV-to-map localization.
- Uses monocular depth priors to suppress non-co-visible regions.
- Plug-and-play with different MDE models, detectors, and matchers.
- Improves PnP inlier ratio and localization accuracy under low-altitude oblique-view conditions.
