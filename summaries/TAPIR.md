# TAPIR: Tracking Any Point with per-frame Initialization and temporal Refinement
Carl Doersch, Yi Yang, Mel Vecerik, Dilara Gokay, Ankush Gupta, Yusuf Aytar, Joao Carreira, Andrew Zisserman

## Summary
A model that effectively tracks any queried point on any physical surface throughout a video sequence. 
- The approach employs two stages:
1. The matching stage - locating a suitable candidate point match for the query point on each frame.
2. Refinement stage - updates both the trajectory and query features based on local correlations.
- the initial ‘coarse’ tracking consists of an occlusion-robust matching performed separately on every frame, where tracks are hypothesized using low resolution features, without enforcing temporal continuity
- The ‘fine’ refinement iteratively uses local, spatio-temporal information at a higher resolution, wherein a neural network can trade-off smoothness of motion with local appearance cues to produce the most likely track.
  
## Model and Concepts
- Given a video and (potentially dense) query points on solid surfaces, an algorithm should reliably output the locations those points correspond to in every other frame where they are visible, and indicate frames where they are not.

- In the matching stage TAPIR analyzes each video frame independently to identify a suitable candidate point match for the query point (point of interest that needs to be tracked). To find the candidate point match for the query point in each frame taper utilizes a deep neural network. The deep neural network takes as input an image patch around the query point and produces a feature vector that represents the appearance of the patch. This feature vector captures relevant information about the query point's visual characteristics. TAPIR then compares this feature vector with the feature vectors of all possible points in each frame using cosine similarity as a measure. The point with the highest cosine similarity score is selected as the candidate Point match for that frame.

- In the refinement stage TAPIR updates both the trajectory and the query features based on local correlations. The trajectory represents the path followed by the query point throughout the video sequence while the query features are the feature vectors that capture its appearance. To update the trajectory and query features TAPIR employs another deep neural network, this network takes a small image patch around the candidate point match in each frame as input and produces a displacement vector. The displacement vector indicates how much the candidate Point match should be shifted to more accurately align with the true query point. TAPIR applies this displacement vector to the candidate point match resulting in a refined point match that is closer to the actual query point. In simpler terms, TAPIR examines small parts of the image calculates the necessary adjustment to align a selected point with a Target point and then moves the selected point closer to the Target. this process is repeated across frames to refine the trajectory of the query.


![TAPIR 5 pics](https://github.com/zenithgpta/TAPIR_summary/assets/113705191/a6307fb7-9d65-436c-b81a-b75d16e25d72)



## Contributions and Results
- A new model: TAP (Tracking any Point) with per-frame Initialization and temporal Refinement (TAPIR).
- It can have multidimensional applications like in security it can assist in identifying and tracking specific individuals or objects of interest within surveillance footage, in entertainment it can enhance special effects and enable more immersive virtual reality experiences
- TAPIR improves over prior works by a large margin, as measured by performance on the TAP-Vid benchmark [12]. On TAP-Vid-DAVIS, TAPIR outperforms TAP-Net by ∼20% while on the more challenging TAP-Vid-Kinetics, TAPIR outperforms PIPs by ∼20%, and substantially reduces its inference runtime
  
![TAPIR 3](https://github.com/zenithgpta/TAPIR_summary/assets/113705191/765aa601-9ef7-46d9-b531-9d7004c8ec5a)
![TAPIR 4-results](https://github.com/zenithgpta/TAPIR_summary/assets/113705191/9d09a0d9-2f88-4eec-8d69-4b1200fa96a0)

## Our two cents 
- This model essentially combines the two previous models TAPNet and PIPs (Persistent Independent Particles)while achieving the benefits from both.
   - TAP-Net performs a global search on every frame independently, providing a coarse track that is robust to occlusions. However, it does not make use of the continuity of videos, 
 resulting in jittery, unrealistic tracks.
   - PIPs, meanwhile, gives a recipe for refinement: given an initialization, it searches over a local neighborhood and smooths the track over time. However, PIPs processes videos sequentially in chunks, initializing each chunk with the output from the last. The procedure
struggles with occlusion and is difficult to parallelize, resulting in slow processing.

- By combining the matching stage which identifies candidate Point matches in each frame and the refinement stage which considers temporal information for accurate tracking taper achieves impressive performance in tracking any point on any physical surface throughout a video sequence the approach is not limited to specific objects or scenarios making it highly versatile and applicable across various domains to encourage further research and collaboration the team has published their findings in a paper (https://arxiv.org/abs/2306.08637). they've also generously open sourced the code and pre-trained models which can be accessed on GitHub (https://github.com/deepmind/tapnet)

