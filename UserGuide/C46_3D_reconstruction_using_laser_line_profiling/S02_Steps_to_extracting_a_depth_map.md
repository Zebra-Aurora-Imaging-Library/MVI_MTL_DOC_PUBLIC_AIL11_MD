---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Steps_to_extracting_a_depth_map
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Steps to extracting a depth map"
---

# Steps to creating a cloud of 3D points using laser line profiling

The following steps provide a basic methodology for using laser line profiling to create a point cloud using the Aurora Imaging Library 3D Reconstruction module:

1. Allocate a 3D reconstruction context to hold your laser line profiling settings, using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md).
2. Optionally, use [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) to change the settings of the [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) context.
3. If you need to adjust settings for a 1D locate peak image processing context (which specifies how to find the laser line in the image), inquire the identifier of the internal peak intensity image processing context using [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) with [`M_LOCATE_PEAK_1D_CONTEXT_ID`](../../Reference/3dmap/M3dmapInquire.md). Then, use [`MimControl`](../../Reference/im/MimControl.md) to adjust the settings of this internal image processing context.
4. Fully calibrate your 3D reconstruction setup; this includes calibrating your camera.
   1. Calibrate your camera by allocating a camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md); then, calibrate it using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md).
   2. Allocate a 3D reconstruction calibration result buffer using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).
   3. Optionally, use [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) to change the settings of the [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer.
   4. Accumulate required 3D reconstruction calibration information. This involves grabbing laser line images of planar objects with known heights. Extract the laser data from the images into the [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), while specifying the known height of the object using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_CORRECTED_DEPTH`](../../Reference/3dmap/M3dmapControl.md).
   5. Complete the 3D reconstruction calibration using [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md).
   6. Free the [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, once 3D reconstruction calibration is complete, using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md).
5. Allocate a new 3D reconstruction result buffer to hold the point cloud, using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md).
6. Grab a series of images of an object as it passes through the laser plane. For each image, extract the laser line information from the image and add the data to the [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) and a point cloud label. Note that you should specify the same label for each image; specifying another label will create a new point cloud.
   The 3D reconstruction result buffer now has a point cloud that contains 3D points.
7. To operate on the 3D points, copy the point cloud into a point cloud container using [`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md).
8. If required, obtain additional information about the 3D points in your point cloud using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md).
9. Free all your allocated objects using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md) and [`McalFree`](../../Reference/cal/McalFree.md), unless [`M_UNIQUE_ID`](../../Reference/3dmap/M3dmapAlloc.md) was specified during allocation.

Steps 1 to 5 are usually performed only once for a given 3D reconstruction setup. You can repeat steps 6 to 9 for every object that you want to reconstruct. Note that before every new object being scanned, you should clear the contents of the result buffer using [`M3dmapClear`](../../Reference/3dmap/M3dmapClear.md).

> **Note:** Most of this chapter describes how to generate point clouds ([`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode). To generate a partially corrected depth map, see [Partially corrected depth maps](S10_Partially_corrected_depth_maps.md).
