# The Kinematic3D Model

Kinematic3D seeks to utilize temporal motion to preceive the physical world in 3D for self-driving applications. They Proposes a single model & kalman filter framework which is composed of 3 primary components; a 3D region proposal network (RPN), ego-motion estimation, and a novel kinematic model (which employs a kalman filter) to take advantage of temporal motion in videos.

## 3D RPN 
The RPN consists of a backbone network and a detection head which predicts 3D box outputs relative to a set of predefined anchors. 

Here, Kinematic3D also proposes a novel object orientation formulation where they decompose orientation into 3 components; axis, heading and offset. This is a comprimise between classifying orientation via a series of discrete bins then regressing an offset, & directly regressing an offset. 

## Ego Motion
Kinematic3D estimates the ego motion of the capturing camera itself, narrowing down the work of the kalman filter to account for only the objects's motion. They define ego-motion in the conventional six degrees of freedom: translation [γx, γy, γz] in meters and rotation [ρx, ρy, ρz] in radians. Ultimately, they are able to model both ego motion and per-object velocity.

## Kinematics 
In order to leverage temporal motion, Kinematic3D integrates the 3D RPN and ego-motion into a kalman filter.  

Basic Kalman Filter Framework:
< do this >




# The Code 
