---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_analysis_using_planar_views_of_an_object
section: 3D_analysis_using_planar_views_of_an_object_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_analysis_using_planar_views_of_an_object / 3D analysis using planar views of an object overview"
---

# 3D analysis using planar views of an object overview

Aurora Imaging Library offers a variety of procedures to perform 3D analysis of planar views of an object (assuming the images have been calibrated using a 3D mode).

If your setup has objects that rest on surfaces of different heights, comparative measurements can be difficult to obtain due to perspective distortion. Using Aurora Imaging Library's 3D-based camera calibration modes, the Z-coordinate is assigned a real-world height based on the distance of the camera to the camera calibration plane. This allows you to displace the relative coordinate system to the known height of an object in your image before performing measurements, allowing dimensions of different features in the image to be compared.

If you are trying to manipulate an object that can move freely in a camera's field of view, you might need to know its **pose** (that is, its position and orientation) in the world. Using 3D-based camera calibration modes, you can use the Camera Calibration module to establish the pose of an object in the absolute coordinate system, if you have a **3D virtual model** of the object, defined using CAD software, and you can find characteristics on the object for which you know their position relative to the origin of the 3D virtual model.

If you want to construct a 3D representation of an object, Aurora Imaging Library's 3D Reconstruction module allows you to extract 3D information from images taken from two types of 3D reconstruction setups:

- A laser line profiling setup.
- A stereo vision triangulation of points setup.

Using images taken from a 3D reconstruction setup built for laser line profiling, the module can generate a cloud of 3D points (point cloud) of an object.

Using images taken from a 3D reconstruction setup for the stereo vision triangulation of points, the module can calculate the world coordinates of a point when two or more cameras, for which you have camera calibration contexts, are used.

This chapter discusses how to perform most of the above-mentioned 3D analysis using planar views of an object. For information when using a laser line profiling setup, see [3D reconstruction using laser line profiling](../C46_3D_reconstruction_using_laser_line_profiling/ChapterInformation.md).

If you have a point cloud or, in some cases, a fully corrected depth map, you can use the 3D modules that perform 3D processing and analysis instead of using the methods described in this chapter.
