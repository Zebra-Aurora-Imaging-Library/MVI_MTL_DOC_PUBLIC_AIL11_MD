---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Calibration_overview
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Calibration overview"
---

# Camera calibration - overview

Aurora Imaging Library's Camera Calibration module ([`Mcal...`](../../Reference/cal/McalAlloc.md)) allows you to map pixel coordinates to real-world coordinates. Camera calibration relates pixels to real locations and distances. For example, you could create the following relationship between a pixel and its size and location in the real world.

*[Image: cal_uniform_mapping.png]*

This mapping can be used to get results from other Aurora Imaging Library modules in real-world units or to input information to some modules in real-world units. The mapping can also be used to physically correct an image's distortions.

By getting results in real-world units, you automatically compensate for any distortions in an image. Therefore, you can get accurate results despite an image's distortions.

Defining the pixel-to-world mapping is known as **camera calibration**. A camera calibration context is used to hold the defined mapping, as well as certain control settings.

Once you have created your camera calibration context, you can use it to:

- Transform pixel coordinates or results to their real-world equivalents.
- Physically correct an image.
- Automatically get results from applicable Aurora Imaging Library processing and analysis modules in real-world units.
- Input values to applicable Aurora Imaging Library analysis modules in real-world units.

Camera calibration maps pixels of images to one plane in the real world. Using only the Camera Calibration module is not enough to obtain data about the depth of different points in your images. To retrieve depth information from your two-dimensional images, you must perform a [3D Reconstruction](../C46_3D_reconstruction_using_laser_line_profiling/ChapterInformation.md).

## Types of distortions

You can use calibration if you have one or more of the following types of distortion:

- **Non-unity aspect ratio distortion**. Present when the X- and Y-axes have two different scale factors. This is evident, for example, if you know that the object in your image should be round and it appears as an ellipse.
- **Rotation distortion**. Present when the camera is perpendicular to the object grabbed in the image, but not aligned with the object's axes.
- **Perspective distortion**. Present when the camera is not perpendicular to the object grabbed in the image. Objects that are further away from the camera appear proportionally smaller than the same size objects closer to the camera.
- **Other spatial distortions**. Complex distortions, such as pin cushion and barrel-type distortions, fall in this category. These distortions can be compensated for by using a large number of small sections in the mapping function. The mapping in each area can be approximated with a linear interpolation function if the number of sections used is significantly larger than the corresponding area covered.

The following animation gives examples of the different types of distortion:

*[Image: CameraCalibration_Distortion]*

## Camera calibration mechanism

To perform the calibration, the module uses calibration points and a camera calibration mode, except in uniform camera calibration mode, which assumes only combinations of linear distortions. Calibration points are points in the image with known positions in the world. The module uses these points with the specified calibration mode to determine the world coordinates of all other points in the image. The calibration points can be explicitly specified or can be automatically calculated from an image of a grid and the world description of the grid. You can either use two-dimensional (2D) or three-dimensional (3D) camera calibration modes depending on the type of your imaging distortions and camera setup.

Four 3D-based camera calibration modes are available in Aurora Imaging Library. The first is based on the technique developed by Roger Y. Tsai. The second is based on the technique developed by Z. Zhang. The other two use the Tsai-based and Zhang-based camera models in robotics mode. These robotics modes are used for robotics setups that either have the camera mounted on the last joint of a robot arm, or where the camera is stationary and the robot arm is moving. They also estimate the position and orientation of the robot base coordinate system with respect to the absolute coordinate system. All four 3D-based camera calibration modes support full 3D movement of your camera. However, all results are returned in a 2D coordinate system. This means that all points in an image are assumed to be on the same plane even if the plane is slanted.
