---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "NeuralFusion: Online Depth Fusion in Latent Space"
authors: ["[**Silvan Weder**](https://www.silvanweder.com)", "[Johannes L. Schönberger](https://demuc.de)", "[Marc Pollefeys](http://people.inf.ethz.ch/pomarc/)", "[Martin R. Oswald](http://people.inf.ethz.ch/moswald/)"]
date: 2021-04-27T16:17:32+02:00
doi: ""
slug: "neural-fusion"

# Schedule page publish date (NOT publication's date).
publishDate: 

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: "Conference on Computer Vision and Pattern Recognition (CVPR)
"
publication_short: '**CVPR 2021, Nashville**'

abstract: "We present a novel online depth map fusion approach that learns depth map aggregation in a latent feature space. While previous fusion methods use an explicit scene representation like signed distance functions (SDFs), we propose a learned feature representation for the fusion. The key idea is a separation between the scene representation used for the fusion and the output scene representation, via an additional translator network. Our neural network architecture consists of two main parts: a depth and feature fusion sub-network, which is followed by a translator sub-network to produce the final surface representation (e.g. TSDF) for visualization or other tasks. Our approach is an online process, handles high noise levels, and is particularly able to deal with gross outliers common for photometric stereo-based depth maps. Experiments on real and synthetic data demonstrate improved results compared to the state of the art, especially in challenging scenarios with large amounts of noise and outliers."

# Summary. An optional shortened abstract.
summary: "Online depth map fusion method in a learned latent representation for increased outlier robustness and completeness."

tags: []
categories: []
featured: true

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_pdf: "https://arxiv.org/pdf/2011.14791.pdf"
url_code: "https://github.com/weders/NeuralFusion"
url_dataset:
url_poster:
url_project:
url_slides:
url_source:
url_video: "https://youtu.be/J3Zga17WP9I"

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: "Left"
  preview_only: false

# Associated Projects (optional).
#   Associate this publication with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `internal-project` references `content/project/internal-project/index.md`.
#   Otherwise, set `projects: []`.
projects: []

# Slides (optional).
#   Associate this publication with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides: "example"` references `content/slides/example/index.md`.
#   Otherwise, set `slides: ""`.
slides: ""
---

{{< youtube id="J3Zga17WP9I" autoplay="true">}}


### Method
In this work, we propose a novel depth fusion pipeline that leverages a learned latent space to integrate noisy and outlier contaminated depth measurements.

<figure>
<img src='assets/pipeline_overview.png'
     height=838
     width=3840></img>
<figcaption style="text-align: left"><b>Overview about the proposed fusion pipeline.</b> The proposed pipeline consists of four stages. The first three stage integrate new measurements into the latent scene representation while the last stage translates the features to TSDF values.</figcaption>
</figure>

#### Pipeline Overview
The proposed pipeline consists of four stages. Firstly, we extract the current state of the global scene representation into a local, view-aligned feature volume using the new depth measurement together with its camera parameters.
This local feature volume is concatenated with the new depth map and passed through the fusion network.
The fusion networks predicts feature updates in the local feature volume. 
These feature updates are integrated back into the global scene representation using the integration layer.
As the scene representation is not interpretable, we propose the usage of a translator network that translates the features into human-readable output such as TSDF values. 
From the translated TSDF grid, the mesh can be extracted using marching cubes.

#### Motivation for Latent Scene Representation
We motivate the usage of a learned latent scene representation with the observation that it is hard for a neural network in a fusion pipeline to distinguish between an outlier and a first measurement of a new geometry.
Therefore, do not use a learned filter in the fusion step to avoid unnecessary filtering and integrate all measurements into the learned scene representation. 
Instead, we let the translation network make the decision about outliers, noise, or true geometry from aggregated measurements in the latent feature space. 
As we can show in our experiments, this leads to more complete yet cleaner reconstructions. 


### Results

We evaluate our method on synthetic ShapeNet and ModelNet data as well as real-world time-of-flight and multi-view stereo data. We compare our method to existing depth fusion as well as scene representation methods.

#### Evaluation on ShapeNet

First, we evaluate our method on fusing noisy depth maps rendered from ShapeNet shapes and compare it to other baselines. We compare it not only to existing fusion methods but also to recent implicit scene representation methods. 
We can show that our method improves the accuracy of the reconstructed meshes, when fusing noisy depth maps. 

<table align='center'>
<tr>
<td><img src='assets/sofa_deepsdf.gif' width='200' height='200'/></td>
<td><img src='assets/sofa_if.gif' width='200' height='200'/></td>
<td><img src='assets/sofa_tsdf.gif' width='200' height='200'/></td>
<td><img src='assets/sofa_routed.gif' width='200' height='200'/></td>
<td><img src='assets/sofa_neural.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><img src='assets/plane_deepsdf.gif' width='200' height='200'/></td>
<td><img src='assets/plane_if_net.gif' width='200' height='200'/></td>
<td><img src='assets/plane_tsdf.gif' width='200' height='200'/></td>
<td><img src='assets/plane_routed.gif' width='200' height='200'/></td>
<td><img src='assets/plane_neural.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><img src='assets/chair_deepsdf.gif' width='200' height='200'/></td>
<td><img src='assets/chair_if_net.gif' width='200' height='200'/></td>
<td><img src='assets/chair_tsdf.gif' width='200' height='200'/></td>
<td><img src='assets/chair_routed.gif' width='200' height='200'/></td>
<td><img src='assets/chair_neural.gif' width='200' height='200'/></td>
</tr>
<td><center><small>Park et al. 2019</small></center></td>
<td><center><small>Chibane et al. 2020</small></center></td>
<td><center><small>Curless et al. 1996</small></center></td>
<td><center><small>Weder et al. 2020</small></center></td>
<td><center><small><b>Ours</b></small></center></td>
</tr>
<caption><b>Qualitative results on ShapeNet.</b> We improve the reconstruction quality from noisy depth maps rendered from ShapeNet shapes compared to existing scene representations and fusion methods</caption>
</table>

We also investigate the accuracy of the reconstructed meshes using different fusion methods. In the table below, we visualize the meshes color-coded using the mesh accuracy of the different methods.

<table align='center'>
<tr>
<td><center><small>Curless et al. 1996</small></center></td>
<td><img src='assets/chair_precision_tsdf.png' width='100', height='300'/></td>
<td><img src='assets/car_precision_tsdf.png' width='200', height='300'/></td>
<td><img src='assets/plane_precision_tsdf.png' width='200', height='300'/></td>
<td><img src='assets/lamp_1_precision_tsdf.png' width='100', height='300'/></td>
<td><img src='assets/table_precision_tsdf.png' width='200', height='300'/></td>
</tr>
<tr>
<td><center><small>Weder et al. 2020</small></center></td>
<td><img src='assets/chair_precision_routed.png' width='100', height='300'/></td>
<td><img src='assets/car_precision_routed.png' width='200', height='300'/></td>
<td><img src='assets/plane_precision_routed.png' width='200', height='300'/></td>
<td><img src='assets/lamp_1_precision_routed.png' width='100', height='300'/></td>
<td><img src='assets/table_precision_routed.png' width='200', height='300'/></td>
</tr>
<tr>
<td><center><small><b>Ours</b></small></center></td>
<td><img src='assets/chair_precision_ours.png' width='100', height='300'/></td>
<td><img src='assets/car_precision_ours.png' width='200', height='300'/></td>
<td><img src='assets/plane_precision_ours.png' width='200', height='300'/></td>
<td><img src='assets/lamp_1_precision_ours.png' width='100', height='300'/></td>
<td><img src='assets/table_precision_ours.png' width='200', height='300'/></td>
</tr>
<caption><b>Visualizing the error on ShapeNet meshes.</b>  We reconstruct the objects with higher accuracy then existing fusion methods.</caption>
</table>

We show that our method reconstructs the meshes with higher accuracy as wells as further mitigate thickening artefacts that corrupt the result in TSDF Fusion and RoutedFusion.

#### Evaluating Outlier Filtering on ModelNet
Further, as learned end-to-end outlier filtering is one of the main goals of our pipeline, we evaluate the pipeline's outlier filtering capabilities. 
Therefore, we contaminated depth maps rendered from ModelNet shapes with different amounts of outliers. We add 1%, 5%, and 10% of outliers.
We can show in the table below that our method filters almost all outliers from the scene, while TSDF Fusion as well as RoutedFusion have troubles in reconstructing clean, outlier-free meshes.

<table align='center'>
<tr>
<td><center><small>0.01</small></center></td>
<td><img src='assets/tsdf_99.gif' width='200', height='200'/></td>
<td><img src='assets/routed_99.gif' width='200', height='200'/></td>
<td><img src='assets/ours_99.gif' width='200', height='200'/></td>
</tr>
<tr>
<td><center><small>0.05</small></center></td>
<td><img src='assets/tsdf_95.gif' width='200', height='200'/></td>
<td><img src='assets/routed_95.gif' width='200', height='200'/></td>
<td><img src='assets/ours_95.gif' width='200', height='200'/></td>
</tr>
<tr>
<td><center><small><b>0.1</b></small></center></td>
<td><img src='assets/tsdf_9.gif' width='200', height='200'/></td>
<td><img src='assets/routed_9.gif' width='200', height='200'/></td>
<td><img src='assets/ours_9.gif' width='200', height='200'/></td>
</tr>
<tr>
<td><center><small>Outlier Fraction</small></center></td>
<td><center><small>Curless et al. 1996</small></center></td>
<td><center><small>Weder et al. 2020</small></center></td>
<td><center><small>Ours</small></center></td>
</tr>
<caption><b>Filtering outliers from ModelNet reconstructions.</b> We significantly improve the outlier filtering capability with our learned end-to-end fusion pipeline.</caption>
</table>

### Real-World Results
We also evaluate our method on real-world data obtained from multi-view stereo methods and time-of-flight sensors. 

#### Multi-view Stereo Data
Firstly, we compare the fusion performance of our method compared to existing methods on depth maps reconstructed with COLMAP. 
These depth maps are usually heavily contaminated with outliers. Therefore, a powerful outlier handling is crucial to obtain clean 3D reconstructions.
We demonstrate this by fusing the depth maps using TSDF Fusion, RoutedFusion with its outlier filter heuristic, and our method that filters the outliers in a learned end-to-end fashion.

<table align='center'>
<tr>
<td><img src="assets/caterpillar_rgb.jpg", width="200", height="200"/></td>
<td><img src="assets/caterpillar_psr.png", width="200", height="200"/></td>
<td><img src="assets/caterpillar_tsdf.png", width="200", height="200"/></td>
<td><img src="assets/caterpillar_routed.png", width="200", height="200"/></td>
<td><img src="assets/caterpillar_ours.png", width="200", height="200"/></td>
</tr>
<td><img src="assets/truck_rgb.jpg", width="200", height="200"/></td>
<td><img src="assets/truck_psr.png", width="200", height="200"/></td>
<td><img src="assets/truck_tsdf.png", width="200", height="200"/></td>
<td><img src="assets/truck_routed.png", width="200", height="200"/></td>
<td><img src="assets/truck_ours.png", width="200", height="200"/></td>
</tr>
<td><img src="assets/m60_rgb.jpg", width="200", height="200"/></td>
<td><img src="assets/m60_psr.png", width="200", height="200"/></td>
<td><img src="assets/m60_tsdf.png", width="200", height="200"/></td>
<td><img src="assets/m60_routed.png", width="200", height="200"/></td>
<td><img src="assets/m60_ours.png", width="200", height="200"/></td>
</tr>
<tr>
<td></td>
<td><center><small>PSR</small></center></td>
<td><center><small>Curless et al. 1996</small></center></td>
<td><center><small>Weder et al. 2020</small></center></td>
<td><center><small>Ours</small></center></td>
</tr>
<caption><b>Reconstructing Tanks and Temples scenes from multi-view stereo depth maps.</b> We reconstruct significantly cleaner meshes from MVS depth maps heavily contaminated with outliers compared to existing fusion methods.</caption>
</table>

In the figure above, we can clearly show that our proposed end-to-end pipeline fusing the measurements into a learned latent space is superior in reconstructing clean meshes compared to the existing fusion and meshing methods.

#### Time-of-flight Data
Secondly, we fuse time-of-flight data taken from the Scene3D dataset. We also compare our method to the two other baselines. We can not only show that our method improves the accuracy but also would like to highlight the increased completeness due to our learned translation opposed to the handcrafted outlier filtering heuristics used in the other methods.

<table align="center">
<tr>
<td><img src="assets/lounge_rgb.png", width=200, height=200/></td>
<td><img src="assets/lounge_tsdf.png", width=200, height=200/></td>
<td><img src="assets/lounge_routed.png", width=200, height=200/></td>
<td><img src="assets/lounge_ours.png", width=200, height=200/></td>
</tr>
<tr>
<td><img src="assets/stonewall_rgb.png", width=200, height=200/></td>
<td><img src="assets/stonewall_tsdf.png", width=200, height=200/></td>
<td><img src="assets/stonewall_routed.png", width=200, height=200/></td>
<td><img src="assets/stonewall_ours.png", width=200, height=200/></td>
</tr>
<tr>
<td></td>
<td><center><small>Curless et al. 1996</small></center></td>
<td><center><small>Weder et al. 2020</small></center></td>
<td><center><small>Ours</small></center></td>
</tr>
<caption><b>Fusing time-of-flight data from Scene3D.</b> Our learned translation improves the completeness compared to the handcrafted heuristics used in existing methods.</caption>
</table>

#### Iterative Fusion
Finally, we would like to show the iterative fusion results of our pipeline in the animation below. You can see the iterative reconstruction of a Tanks and Temples scene.
<img src="assets/family_w_cam_ours.gif"/>

### Conclusion
We have presented a novel depth fusion pipeline that leverages a learned latent scene representation and can be trained in an end-to-end way. 
The latent scene representation is efficiently translated into human-readable output (TSDF values) to allow mesh extraction using the marching cubes algorithm. 
This translation allows to effectively filter outliers and denoise the reconstruction while improving the completeness of the mesh. 
Together with the powerful scene representation, the pipeline outperforms existing methods in reconstructing scenes from noisy and outlier contaminated depth maps. 
