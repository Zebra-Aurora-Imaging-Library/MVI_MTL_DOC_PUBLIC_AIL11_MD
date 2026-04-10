---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: 3D_reconstruction_using_laser_line_profiling_overview
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / 3D reconstruction using laser line profiling overview"
---

# 3D reconstruction using laser line profiling overview

The Aurora Imaging Library 3D Reconstruction module allows you to extract 3D information from 2D images of an object taken using a 3D reconstruction setup built for laser line profiling. From the grabbed images of objects passing through a sheet of light or laser plane, the module can create a cloud of 3D points or a partially-corrected depth map.

> **Note:** Some 3D sensors (including some laser profilers) perform all steps of 3D reconstruction on-board and transmit 3D data in the form of a precalibrated point cloud or depth map. If you have one of these devices, you do not need to use Aurora Imaging Library to perform laser line profiling. This chapter only applies to laser line setups that use standard cameras, or cameras that perform laser-line extraction (but not calibration) on-board. For information on grabbing from 3D sensors that perform all steps of 3D reconstruction on-board, see[Grabbing from 3D sensors overview](../C42_Grabbing_from_3D_sensors/S01_Grabbing_from_3D_sensors_overview.md).

A basic laser line profiling setup consists of a device projecting a sheet of light (usually a laser diode), a camera, and a mechanism to move the object under the laser plane (for example, a conveyor belt). An object is then moved at a known speed and on a straight path perpendicular to the laser plane. As the object moves through the laser plane, the camera is used to grab images of the intersection of that laser plane with the object.

*[Image: 3dmap_introduction.png]*

As a result, multiple images are grabbed such that the laser line in each image represents a slice of the object being scanned. The incidence of the laser light on the object, at an angle from the optical axis of the camera, leads to an image whereby the line of laser light is deformed. From the position of the displaced laser line, the 3D reconstruction module can obtain height (depth) information for the slice of the object. The depth information is measured from the top of the image to the base laser line. If the laser line is not visible, the value of the cell is saturated with the maximum value.

The following animation illustrates obtaining a depth map.

|   |   |
| --- | --- |
| *[Image: 3dmap_obtaining_a_depth_map_object.png]* | *[Image: 3dmap_obtaining_a_depth_map]* |

When your laser line profiling setup is fully calibrated, including camera calibration, passing an object through a sheet of light or laser plane will result in a cloud of 3D points. The cloud of points is a 3D representation of the object's surface. You can process, analyze, and create a mesh from the 3D points using the other Aurora Imaging Library 3D modules ([`M3d...`](../../Reference/3dim/M3dimArith.md)). You can display the point cloud using the Aurora Imaging Library 3D display module ([`M3ddisp...`](../../Reference/3ddisp/M3ddispAlloc.md)).

*[Image: 3dmap_cloud_mesh.png]*

With a fully calibrated setup, you can add additional cameras and/or lasers. The additional cameras could be used to increase the resolution of the sheet of light data or to overcome the occlusion of sections of the scanned object due to the camera angle and placement. Multiple cameras or multiple lasers creates multiple point clouds.

If you are only interested in the depth of an object and not its shape, the 3D reconstruction module can generate a partially corrected depth map, which is corrected for depth, but not for camera distortion or shape. No camera calibration is used in the generation of the depth map.

*[Image: 3dmap_example_types_depthmaps.png]*
