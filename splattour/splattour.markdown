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
Recent advances in radiance field rendering techniques, such as 3D Gaussian Splatting, are promising for immersive web-based virtual tours. This research evaluates the feasibility and performance of 3D Gaussian splatting on consumer-grade systems using a two-phased iterative approach. The first phase assesses the limitations of 3D Gaussian Splatting in Unity3D on WebGPU, showing acceptable frame rates of ~20FPS on mid-ranged devices, with room for optimizations in loading times and memory usage due to the large size of 3D Gaussian Splat assets. In phase two, an optimization is introduced by using spatial partitioning combined with asset streaming. The optimization significantly **reduced loading times with up to 70%** and records **25% less memory usage**. The findings confirm that 3D Gaussian Splatting is a viable technique for web-based virtual tours and demonstrate that optimizing the loading strategy plays a crucial role in enhancing performance. Future work may explore alternative engines such as PlayCanvas and advanced optimization techniques like progressive loading or LOD systems to further improve scalability.

## **Data and Sources**
This research made use of the  photos in the dataset "New York City" and "Alameda" from the [Zip-NeRF](https://jonbarron.info/zipnerf/) paper by Barron et al. 

3D Gaussian Splat assets were synthesized with [nerfstudio](https://docs.nerf.studio/) and loaded into Unity. 

The implementation in Unity is based on the [UnityGaussianSplatting implementation](https://github.com/aras-p/UnityGaussianSplatting) by Aras Pranckevičius.

## **Optimization techniques**

![Partitions](/assets/Partitions.png)

Phase 1 testing revealed that the primary performance bottleneck was the loading strategy, particularly the time it took to load large 3D Gaussian Splat assets at runtime, taking up to 14 seconds. To address this, developers implemented **Spatial Partitioning** and **Asset Streaming**, which significantly reduced loading times. Spatial Partitioning involved dividing scenes into a 2D grid, with each cell representing a separate splat asset. These partitions were then streamed into the application using Unity’s Addressables system. Additionally, this setup enabled efficient view frustum culling, further optimizing performance by ensuring only visible partitions were rendered.
