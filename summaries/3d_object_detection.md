# Center-based 3D Object Detection and Tracking

Tianwei Yin, Xingyi Zhou, Philipp Krahenbuhl (UT Austin), CPVR 2021

## Summary 

This paper proposes that center-based 3D object detection and tracking is well performing over 3D bounding box representations. And how center based method detection and tracking is simple. This paper outperforms on Waymo Open Dataset and ranks first among all Lidar-only submissions.

## Abstract 

3D objects commonly represented as 3D boxes in point-cloud but this has many challenges. Like , point-clouds are sparse, and most regions of 3D space are without measurements, the resulting output three dimensional box is not well aligned with any global coordinate frame and objects having large size, shapes and aspect ratios. So In this paper, we will  represent, detect, and track 3D objects as points. This has many advantages - unlike bounding boxes, points have no intrinsic orientation, a center-based representation simplifies downstream tasks such as tracking. Now our framework, CenterPoint, first detects centers of objects using a keypoint detector and regresses to other attributes, including 3D size, 3D orientation, and velocity.. CenterPoint achieved state-of-the-art performance on the nuScenes benchmark for both 3D detection and tracking, with 65.5 NDS and 63.8 AMOTA for a single model.

## Methodology

1. First get 3D point cloud data from LiDAR sensors.CenterPoint uses a standard Lidar-based backbone network, i.e., VoxelNet or PointPillars, to build a flattening map view of the input point cloud data. I.e projecting 3D data on a 2D plane. Now this will be treated as a regular 2D image.
2. Now keypoint detector or in this case we will be using CenterNet algorithm which will  takes an input image and predicts a w × h heatmap Ŷ ∈ [0, 1]w×h×K for each of K classes. Each local maximum  in the output heatmap corresponds to the center of a detected object and also detect object size, rotation, and velocity using center features
3. Now, 3D object tracking simplifies to greedy closest-point matching. The resulting detection and tracking algorithm is simple, efficient, and effective

## Main Results

### Table 1: State-of-the-art comparisons for 3D detection on Waymo test set

<table>
  <tr>
    <th>Method</th>
    <th>mAP ↑</th>
    <th>NDS ↑</th>
    <th>PKL ↓</th>
  </tr>
  <tr>
    <td>PointPillars</td>
    <td>40.1</td>
    <td>55.0</td>
    <td>1.00</td>
  </tr>
  <tr>
    <td>CVCNet</td>
    <td>55.3</td>
    <td>64.4</td>
    <td>0.92</td>
  </tr>
  <tr>
    <td>CBGS</td>
    <td>52.8</td>
    <td>63.3</td>
    <td>0.77</td>
  </tr>
  <tr>
    <td>PointPainting</td>
    <td>46.4</td>
    <td>58.1</td>
    <td>0.89</td>
  </tr>
  <tr>
    <td>Ours</td>
    <td>58.0</td>
    <td>65.5</td>
    <td>0.69</td>
  </tr>
</table>


### Table 2: State-of-the-art comparisons for 3D detection on nuScenes test set

<table>
  <tr>
    <th>Method</th>
    <th colspan="2">MOTA ↑</th>
    <th colspan="2">MOTP ↓</th>
  </tr>
  <tr>
    <td></td>
    <td>Vehicle</td>
    <td>Ped</td>
    <td>Vehicle</td>
    <td>Ped</td>
  </tr>
  <tr>
    <td>AB3D</td>
    <td>42.5</td>
    <td>38.9</td>
    <td>18.6</td>
    <td>34.0</td>
  </tr>
  <tr>
    <td>Ours</td>
    <td>62.6</td>
    <td>58.3</td>
    <td>16.3</td>
    <td>31.1</td>
  </tr>
</table>

### Table 3: State-of-the-art comparisons for 3D tracking on Waymo test set

<table>
  <tr>
    <th>Method</th>
    <th>ΔMOTA↑</th>
    <th>FP↓</th>
    <th>FN↓</th>
    <th>IDS↓</th>
  </tr>
  <tr>
    <td>AB3D</td>
    <td>15.1</td>
    <td>15088</td>
    <td>75730</td>
    <td>9027</td>
  </tr>
  <tr>
    <td>Chiu et al.</td>
    <td>55.0</td>
    <td>17533</td>
    <td>33216</td>
    <td>950</td>
  </tr>
  <tr>
    <td>Ours</td>
    <td>63.8</td>
    <td>18612</td>
    <td>22928</td>
    <td>760</td>
  </tr>
</table>

### Table 4: State-of-the-art comparisons for 3D tracking on nuScenes test set

<table>
  <tr>
    <th>Encoder</th>
    <th>Method</th>
    <th>Vehicle</th>
    <th>Pedestrian</th>
    <th>mAPH</th>
  </tr>
  <tr>
    <td>VoxelNet</td>
    <td>Anchor-based</td>
    <td>66.1</td>
    <td>54.4</td>
    <td>60.3</td>
  </tr>
  <tr>
    <td></td>
    <td>Center-based</td>
    <td>66.5</td>
    <td>62.7</td>
    <td>64.6</td>
  </tr>
  <tr>
    <td>PointPillars</td>
    <td>Anchor-based</td>
    <td>64.1</td>
    <td>50.8</td>
    <td>57.5</td>
  </tr>
  <tr>
    <td></td>
    <td>Center-based</td>
    <td>66.5</td>
    <td>57.4</td>
    <td>62.0</td>
  </tr>
</table>

## Our two cents
**Simple:** It use standard 3D point cloud encoder with a few convolutional layers in the head to produce a bird-eye-view heatmap and other dense regression outputs including the offset to centers in the previous frame. Detection is a simple local peak extraction with refinement, and tracking is a closest-distance matching.

**Fast and Accurate:** Our best single model achieves 71.9 mAPH on Waymo and 65.5 NDS on nuScenes while running at 11FPS+.

## Resources
Paper:https://paperswithcode.com/paper/center-based-3d-object-detection-and-tracking

Code and pretrained models:https://github.com/tianweiy/CenterPoint
