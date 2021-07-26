# The Kinematic3D Model

Kinematic3D seeks to utilize temporal motion to preceive the physical world in 3D for self-driving applications. They Propose a single model & kalman filter framework which is composed of 3 primary components; a 3D region proposal network (RPN), ego-motion estimation, and a novel kinematic model (which employs a kalman filter) to take advantage of temporal motion in videos.

## 3D RPN 
The RPN consists of a backbone network and a detection head which predicts 3D box outputs relative to a set of predefined anchors. 

Here, Kinematic3D also proposes a novel object orientation formulation where they decompose orientation into 3 components; axis, heading and offset. This is a comprimise between classifying orientation via a series of discrete bins then regressing an offset, & directly regressing an offset. 

![](assets/orientation.png)

## Ego Motion
Kinematic3D estimates the ego motion of the capturing camera itself, narrowing down the work of the kalman filter to account for only the objects's motion. They define ego-motion in the conventional six degrees of freedom: translation `[γx, γy, γz]` in meters and rotation `[ρx, ρy, ρz]` in radians. Ultimately, they are able to model both ego motion and per-object velocity.

## Kinematics 
In order to leverage temporal motion, Kinematic3D integrates the 3D RPN and ego-motion into a kalman filter.  

### Basic Kalman Filter Framework:
The Kalman Filter contains 2 general steps: 
1. Prediction step -> using motion model and the state estimate from `t-1`, giving us a predicted state for `t` which we will call `t'`
2. update step -> using observation model and prediction state, giving us final state estimate for `t`

### Kinematic3D Kalman Filter

![](assets/Kalman.png)

The framework uses an RPN to first estimate 3D boxes (Sec. 3.1). Ego-motion is estimated using boxes `Xt` and `Xt-1`. Previous frame tracks `τt−1` are then forcasted into `τt′` (prediction step) using the estimated Kalman velocity. Self-motion is then compensated for by applying the global ego-motion estimation to tracks `τt′`. Lastly, the 3D boxes are used as observations to update/fuse `τt′` with measurements using a kinematic 3D Kalman filter (Sec. 3.3). We now have the current estimated state of the track `τt`.

# The Code - WIP

The original kinematic3d README can be found here; [kinematic3d.md](kinematic3d.md). 

Kinematic3D has built the kalman filter framework within the PyTorch model itself, which means we can call inference on the model as usual, and it will hold the necessary information from previous frames and track data to compute the kalman filter. 

```python 
model = kinematic3DModel()
model.eval()
img = get_im()
img = preprocess(im)
output = model(im)
```

## Environment Setup

### linux

```
conda create -n kinematic3d python=3.6.5
conda activate kinematic3d

conda install -c menpo opencv3=3.1.0 openblas
conda install cython scikit-image
conda install -c conda-forge easydict shapely

conda install pytorch==1.8.0 torchvision==0.9.0 cudatoolkit=11.1 -c pytorch -c conda-forge

git clone https://github.com/garrickbrazil/kinematic3d.git
cd kinematic3d/
<build nms & >

apt update
apt install libatlas-base-dev
apt install libopenblas-dev
apt install libgtk2.0-dev

apt install unzip
wget https://www.cse.msu.edu/computervision/Kinematic3D-Release.zip
<then unzip it with -d into its own directory>

<dataset setup .. >

```

## Gstreamer PyTorch Pipeline 
```
conda create -n gst python=3.6.5
conda activate gst
conda install -c conda-forge pygobject
conda install -c conda-forge gtk3
conda install -c conda-forge gstreamer
```

