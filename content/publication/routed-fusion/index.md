---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "RoutedFusion: Learning Real-time Depth Map Fusion"
authors: ["[**Silvan Weder**](https://www.silvanweder.com)", "[Johannes L. Schönberger](https://demuc.de)", "[Marc Pollefeys](http://people.inf.ethz.ch/pomarc/)", "[Martin R. Oswald](http://people.inf.ethz.ch/moswald/)"]
date: 2020-05-01T16:17:32+02:00
doi: ""
slug: "routed-fusion"

# Schedule page publish date (NOT publication's date).
publishDate: 2020-05-01T16:17:32+02:00

# Publication type.
# Legend: 0 = Uncategorized; 1 = Conference paper; 2 = Journal article;
# 3 = Preprint / Working Paper; 4 = Report; 5 = Book; 6 = Book section;
# 7 = Thesis; 8 = Patent
publication_types: ["1"]

# Publication name and optional abbreviated publication name.
publication: "Conference on Computer Vision and Pattern Recognition (CVPR)
"
publication_short: '**CVPR 2020, Seattle (oral)**'

abstract: "The efficient fusion of depth maps is a key part of most state-of-the-art 3D reconstruction methods. Besides requiring high accuracy, these depth fusion methods need to be scalable and real-time capable. To this end, we present a novel real-time capable machine learning-based method for depth map fusion. Similar to the seminal depth map fusion approach by Curless and Levoy, we only update a local group of voxels to ensure real-time capability. Instead of a simple linear fusion of depth information, we propose a neural network that predicts non-linear updates to better account for typical fusion errors. Our network is composed of a 2D depth routing network and a 3D depth fusion network which efficiently handle sensor-specific noise and outliers. This is especially useful for surface edges and thin objects, for which the original approach suffers from thickening artifacts. Our method outperforms the traditional fusion approach and related learned approaches on both synthetic and real data. We demonstrate the performance of our method in reconstructing fine geometric details from noise and outlier contaminated data on various scenes."

# Summary. An optional shortened abstract.
summary: "Learning-based, real-time depth map fusion method for fusing noisy and outlier contaminated depth maps."

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

url_pdf: "https://arxiv.org/pdf/2001.04388.pdf"
url_code:
url_dataset:
url_poster:
url_project:
url_slides:
url_source:
url_video: "https://youtu.be/EOfHj542G7U"

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

{{< youtube id="EOfHj542G7U" autoplay="true">}}

### Method

Our method consists of four key stages. Firstly, the depth routing network estimates a corrected depth map and a confidence map from a noisy and outlier contaminated depth map. The corrected depth map guides the extraction of a canonical TSDF volume from the global TSDF volume. The canonical volume is passed together with the corrected depth map and the confidence map through the depth fusion network. This depth fusion network predicts optimal updates for the local canonical TSDF volume given its old state and the new measurement. Finally, the updated canonical TSDF volume is integrated back into the global TSDF volume.

<table align='center'>
<tr>
<td><img src='results/pipeline.jpg'/></td>
</tr>
<caption><b>Overview of the RoutedFusion pipeline.</b> It consists of four stages that refine and integrate new sensor measurements into the existing scene.</caption>
</table>

### Results

We evaluate our method on synthetic as well as real-world data. We compare the results to several existing state-of-the-art and baseline methods.

#### Synthetic Data
In order to compare our method to existing learning-based as well as handcrafted method, we evaluate the performance in fusing synthetic depth maps from the ShapeNet dataset that is augmented with an artificial depth-dependent noise distribution.

<table align='center'>
<tr>
<td><img src='results/car_deepsdf.gif' width='200' height='200'/></td>
<td><img src='results/car_occupancy.gif' width='200' height='200'/></td>
<td><img src='results/car_tsdf.gif' width='200' height='200'/></td>
<td><img src='results/car_ours.gif' width='200' height='200'/></td>
<td><img src='results/car_gt.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><img src='results/lamp_deepsdf.gif' width='200' height='200'/></td>
<td><img src='results/lamp_occupancy.gif' width='200' height='200'/></td>
<td><img src='results/lamp_tsdf.gif' width='200' height='200'/></td>
<td><img src='results/lamp_ours.gif' width='200' height='200'/></td>
<td><img src='results/lamp_gt.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><img src='results/chair_deepsdf.gif' width='200' height='200'/></td>
<td><img src='results/chair_occupancy.gif' width='200' height='200'/></td>
<td><img src='results/chair_tsdf.gif' width='200' height='200'/></td>
<td><img src='results/chair_ours.gif' width='200' height='200'/></td>
<td><img src='results/chair_gt.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><img src='results/plane_deepsdf.gif' width='200' height='200'/></td>
<td><img src='results/plane_occupancy.gif' width='200' height='200'/></td>
<td><img src='results/plane_tsdf.gif' width='200' height='200'/></td>
<td><img src='results/plane_ours.gif' width='200' height='200'/></td>
<td><img src='results/plane_gt.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><center><small>Park et al. 2019</small></center></td>
<td><center><small>Mescheder et al. 2019</small></center></td>
<td><center><small>Curless et al. 1996</small></center></td>
<td><center><small><b>Ours</b></small></center></td>
<td><center><small>Ground-truth</small></center></td>
</tr>
<caption><b>Qualitative results on ShapeNet.</b> We significantly better reconstruct fine details than existing methods. Furthermore, RoutedFusion does not show any overfitting to the training data due to its compact neural networks.</caption>
</table>

We quantitatively and qualitatively show that our method outperforms existing depth fusion and 3D representation methods. Especially, our method generalizes significantly better to unseen shapes than existing learning-based methods.

<table align='center'">
<tr>
<th>Method</th>
<th>MSE [e-05]</th>
<th>MAD</th>
<th>Acc. [%]</th>
<th>IoU. [0, 1]</th>
</tr>
<tr>
<td><left>Park et al. 2019</left></td>
<td><right>464.00</right></td>
<td><right>0.0499</right></td>
<td><right>66.48</right></td>
<td><right>0.538</right></td>
</tr>
<td><left>Mescheder et al. 2019</left></td>
<td><right>56.80</right></td>
<td><right>0.0166</right></td>
<td><right>85.66</right></td>
<td><right>0.484</right></td>
</tr>
<td><left>Curless et al. 1996</left></td>
<td><right>11.00</right></td>
<td><right>0.0078</right></td>
<td><right>88.06</right></td>
<td><right>0.659</right></td>
</tr>
<td><left>Ours</left></td>
<td><right><b>5.90</b></right></td>
<td><right><b>0.0050</b></right></td>
<td><right><b>94.77</b></right></td>
<td><right><b>0.785</b></right></td>
</tr>
<caption><b>Quantitative results on ShapeNet.</b> We outperform existing learning-based and handcrafted methods in fusing depth maps augmented with an artificial depth-dependent noise distribution.</caption>
</table>

We also investigate the robustness of our method to higher input noise levels. Therefore, we augment the input depth maps with different noise levels and fuse them using standard TSDF Fusion as well as our RoutedFusion.

<table align='center'>
<tr>
<td><small><center>Curless et al. 1996</center></small></td>
<td><img src='results/tsdf_noise01.gif' width='200' height='200'/></td>
<td><img src='results/tsdf_noise03.gif' width='200' height='200'/></td>
<td><img src='results/tsdf_noise05.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><small><center>Ours</center></small></td>
<td><img src='results/ours_noise01.gif' width='200' height='200'/></td>
<td><img src='results/ours_noise03.gif' width='200' height='200'/></td>
<td><img src='results/ours_noise05.gif' width='200' height='200'/></td>
</tr>
<tr>
<td><center><small>Input Noise Level</small></center></td>
<td><center><small>0.01</small></center></td>
<td><center><small>0.03</small></center></td>
<td><center><small>0.05</small></center></td>
</tr>

<caption><b>Comparison to Standard TSDF Fusion in fusing noisy depth maps.</b> We increase the input noise level in order to demonstrate the robustness of our method to high noise levels. </caption>
</table>

#### Real-World Data

We also run our pipeline on real-world data. We evaluate our pipeline on the Scene3D dataset. We show that our method compares favourably to existing depth fusion methods.

<table align='center'>
<tr>
<td><img src='results/burghers_tsdf00.png' width='200' height='200'/></td>
<td><img src='results/burghers_psdf00.png' width='200' height='200'/></td>
<td><img src='results/burghers_learned00.png' width='200' height='200'/></td>
</tr>
<tr>
<td><img src='results/burghers_tsdf_closeup00.png' width='200' height='200'/></td>
<td><img src='results/burghers_psdf_closeup00.png' width='200' height='200'/></td>
<td><img src='results/burghers_learned_closeup00.png' width='200' height='200'/></td>
</tr>
<tr>
<td><small><center>Curless et al. 1996</center></small></td>
<td><small><center>Dong et al. 2018</center></small></td>
<td><small><center>Ours</center></small></td>
</tr>
<caption><b>Comparison on the Burghers of Calais scene.</b> We favourably compare to existing online depth map fusion methods. Especially, the reconstruction of fine details is significantly improved.</caption>
</table>

### Conclusion

With RoutedFusion, we propose a real-time, learning-based depth map fusion method. This method allows for better fusion of imperfect depth maps and reconstruction of 3D geometry from range imagery in real-world applications. Although only very little training data is required to train RoutedFusion, we show that compact neural networks can be effectively used for real-world applications without the risk of overfitting. This allows for easy transfer to new sensors and scenarios, where only little training data is available.
