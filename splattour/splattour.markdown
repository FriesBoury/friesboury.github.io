---
layout: page
title: "Optimizing Radiance Field Rendering for Web-Based Virtual Tours: A Two-Phased Approach"
permalink: /splattour/
---
{% include fontawesome.html %}

<p style="margin-top: 0; font-size: 1.1em;">
  <strong>Fries Boury</strong> — Master Thesis in Game Technology, Breda University of Applied Sciences
</p>

<div style="text-align: center; margin: 20px 0;">
  <a href="https://www.researchgate.net/publication/392796834" target="blank" class="button"><i class="fas fa-file-pdf"></i> Paper</a>
  <a href="https://www.researchgate.net/publication/392200164" target="blank" class="button"><i class="fas fa-image"></i> Poster</a>
  <a href="https://github.com/DAE-FriesB/Unity_SplatTour" target="blank" class="button"><i class="fab fa-github"></i> Repository</a>
  <a href="https://friesboury.com" target="blank" class="button"><i class="fas fa-play-circle"></i> Demos</a>
</div>

<style>
.button {
  display: inline-block;
  padding: 10px 20px;
  margin: 5px;
  font-size: 16px;
  text-decoration: none;
  color: white;
  background-color: #007acc;
  border-radius: 5px;
  transition: background-color 0.3s ease;
}
.button:visited {
  color: white;
}
.button:visited:hover {
  color: #111;
}
.button:hover {
  background-color: #005f99;
}
.button i {
  margin-right: 8px;
}

</style>

![Demo Preview](/assets/Culling.gif)

## **Abstract**
Recent advances in radiance field rendering techniques, such as 3D Gaussian Splatting, are promising for immersive web-based virtual tours. This research evaluates the feasibility and performance of 3D Gaussian splatting on consumer-grade systems using a two-phased iterative approach. The first phase assesses the limitations of 3D Gaussian Splatting in Unity3D on WebGPU, showing acceptable frame rates of ~20FPS on mid-ranged devices, with room for optimizations in loading times and memory usage due to the large size of 3D Gaussian Splat assets. In phase two, an optimization is introduced by using spatial partitioning combined with asset streaming. The optimization significantly reduced loading times with up to 70% and 25% less memory usage. The findings confirm that 3D Gaussian Splatting is a viable technique for web-based virtual tours and demonstrate that optimizing the loading strategy plays a crucial role in enhancing performance. Future work may explore alternative engines such as PlayCanvas and advanced optimization techniques like progressive loading or LOD systems to further improve scalability.

## **Sources**
This research made use of the  photos in the dataset "New York City" and "Alameda" from the [Zip-NeRF](https://jonbarron.info/zipnerf/) paper by Barron et al. 
3D Gaussian Splat assets were synthesized with [nerfstudio](https://docs.nerf.studio/) and loaded into Unity. The implementation of 3D Gaussian splat asset rendering in Unity is based on the [UnityGaussianSplatting implementation](https://github.com/aras-p/UnityGaussianSplatting) by Aras Pranckevičius.

## **Optimization techniques**
Phase 1 tests showed that the most room for improvement could be found in the loading strategy of the application. Loading the big 3D Gaussian Splat assets at runtime was significantly slow, taking up to 14 seconds for the application to load in. By making use of Spatial Partitioning and Asset Streaming, loading times were reduced significantly.

### Spatial Partitioning
The scenes were split up in spatial partitions based on a 2D Grid, where each grid cell represents a separate 3D gaussian splat asset.

![Partitions for Scene01](/assets/Partitions_Scene1.png)
![Partitions for Scene02](/assets/Partitions_Scene2.png)

### Asset Streaming
All partitions are streamed into the application by making use of Unity's Addressables system.

![Unity Cloud Content](/assets/StreamedAssets.png)



### Culling

![FrustumCulling](/assets/FrustumCulling.png)

The spatial partitions allowed the application to calculate culling based on the view frustum. 

### Sorting
![Sorting](/assets/Sorting.png)

Partitions could be sorted more efficiently by first moving the partitions in the data buffer based on the distance of the partition center from the camera.
In a second step, each partition is sorted locally, which greatly reduced the range of each sorting pass.
## **Results**

### Loading Times
![](/assets/LoadingTimes2.png)

 The time is recorded between the start of the application and the first moment. Loading times are decreased with a factor between **58%** and **70%**.

### Memory Usage
![alt text](/assets/MemoryUsage.png)
The optimization shows a notable improvement in memory usage. The initial memory usage dropped from ~650 MB to only ~360 MB of RAM usage, with an overall 25% drop in memory usage.

### Frame Rate
![FPS Comparison Scene 01](/assets/FPS_Scene1.png)

![FPS Comparison Scene 02](/assets/FPS_Scene2.png)

Comparisons show a slight improvement in frame rates on dedicated GPUs (~20% increase for Scene 1, ~30% increase for Scene 2). 

There's no notable impact on integrated GPUs.

## **Conclusion**
The two-phased iterative approach successfully highlighted limitations and potential optimizations for implementing radiance field rendering in web-based virtual tours on consumer-grade hardware and shows that 3D gaussian splatting is a viable technique for developing web-based virtual tours. In the first phase, the evaluation of the 3D Gaussian Splatting technique showed acceptable performance on consumer-grade devices with room for improvement in loading times and memory usage. In the second phase, an optimization based on spatial partitioning and data streaming was developed, which showed a positive impact in all three performance metrics, with the biggest impact on loading times. 