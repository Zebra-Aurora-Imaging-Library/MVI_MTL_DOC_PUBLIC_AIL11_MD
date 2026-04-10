---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: 3D_Registration_example
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / 3D Registration example"
---

# 3D Registration example

The following Aurora Imaging Library examples illustrate two of the main applications of the 3D Registration module.

The first main application of the module is to detect defects in a scanned object. This is typically done by comparing a perfect model with the point cloud of the scanned object. The perfect model and the scanned object need to be correctly aligned to quickly detect any defects. The resulting transformation matrix gives you information on the pose of the scanned object. Therefore, you could also use it as a 3D model finder in, for example, a robotic bin picking application (assuming the object's approximate location and orientation is known beforehand).

The second main application is to merge multiple overlapping point clouds into a single point cloud that represents a 3D object. You can use multiple point clouds of the object to represent an object from many different angles, but if the working coordinate systems of the different point clouds are not aligned and you try to merge the point clouds, the final point cloud will not be an accurate representation of the object. If you align overlapping point clouds before merging them, the final merged point cloud will be a more accurate representation of the entire object. Furthermore, once the alignment of the different cameras has been determined, the registration results can be saved and reused for merging newly acquired point clouds without performing the registration operation again.

To run these and other examples, use Aurora Imaging Example Launcher.

## Detecting defects

The _3DModelHeightDefect.cpp_ example uses 3D registration to detect defects in a scanned object by registering the point cloud of the scanned to a perfect model, and comparing their differences.

*[Image: 3dreg_example_3dModelHeightDefect.PNG]*

## Merging two point clouds

The _Simple3dStitching.cpp_ example below uses 3D registration to align the working coordinate system of two point clouds of a 3D object, and merge them into a single point cloud.

*[Image: 3dreg_example_Simple3dStitching.png]*

## Merging multiple point clouds

The _m3dreg.cpp_ example uses 3D registration to align the working coordinate system of multiple point clouds of a 3D object, and merge them into a single point cloud.

*[Image: 3dreg_example_m3dreg_robot.png]*

In addition to running the _m3dreg.cpp_ general example from Aurora Imaging Example Launcher, you can also view it here:

> **Code example:** [m3dreg.cpp](m3dreg.cpp)
