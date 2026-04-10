---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Calibrating_your_reconstruction_setup
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Calibrating your reconstruction setup"
---

# Calibrating your 3D reconstruction setup to create a point cloud

After ensuring that your 3D reconstruction setup meets the requirements described in [Configuring the laser line profiling setup](S04_Configuring_the_laser_line_profiling_setup.md), you must calibrate the 3D reconstruction setup. The objective of 3D reconstruction calibration is to establish the relationship between the laser line's displacement when over an object and the real-world depth of the object. Calibrating your 3D reconstruction setup is mandatory before generating point clouds or partially corrected depth maps using the 3D Reconstruction module.

3D reconstruction calibration requires a specific type of 3D reconstruction result buffer, allocated using[`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).

> **Note:** Note that there are slight differences when calibrating the 3D reconstruction context for a partial depth map. These differences are explained in [Partially corrected depth maps](S10_Partially_corrected_depth_maps.md).

## Calibrating to create a point cloud

To create a point cloud, you must first perform two different calibrations: a camera calibration and a 3D reconstruction setup calibration.

To calibrate a camera intended for a 3D reconstruction setup:

1. Allocate a camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md).
2. Calibrate your camera setup using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md). For more details, see [Calibrating using calibration points from a grid](../C28_Calibration/S10_Calibrating_using_calibration_points_from_a_grid.md) and [Calibrating using calibration points from a list](../C28_Calibration/S12_Calibrating_using_calibration_points_from_a_list.md).
   > **Note:** When calibrating your camera, the XY-plane (Z=0) of the absolute coordinate system must be parallel to the conveyor belt; ideally, the XY-plane lies on the same surface as the object being mapped. Note that the grid used during camera calibration might have a height itself, which can skew some calculations. In this case, specify the height (thickness) of the grid using [`McalGrid`](../../Reference/cal/McalGrid.md) with [`GridOffsetZ`](../../Reference/cal/McalGrid.md), so that the XY-plane of the absolute coordinate system is flush with the surface on which the grid is resting. For more information, see [Moving the relative coordinate system to account for height](../C34_3D_analysis_using_planar_views_of_an_object/S02_Moving_the_relative_coordinate_system_to_account_for_height.md).
3. Use [`McalInquire`](../../Reference/cal/McalInquire.md) with the camera calibration context and [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md) to ensure that the camera calibration of the camera setup has been successfully performed.

Once your camera is calibrated, you must calibrate the 3D reconstruction setup. To do so:

1. Allocate a 3D reconstruction context using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md).
2. Allocate a 3D reconstruction calibration result buffer using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).
3. Grab an image of a laser line on a planar object that has a known height. The image of a laser line on an object at a known height is called a reference plane.
4. Specify the Z-coordinate of that plane in the absolute coordinate system, using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_CORRECTED_DEPTH`](../../Reference/3dmap/M3dmapControl.md). Since by default the Z-axis of the absolute coordinate system is pointing down, this value is often negative.
5. Use [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) to extract the laser line information from the grabbed image and add this information to the 3D reconstruction calibration result buffer. For other methods of providing the laser line data, see [Using 3D cameras that output uncorrected depth maps](S08_Using_3D_cameras_that_perform_laser_line_extraction.md).
   > **Note:** If, after calling [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), but before calling [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md), you need to discard the last scan added (for example, as the result of a diagnosis using [`M3dmapDraw`](../../Reference/3dmap/M3dmapDraw.md) with [`M_DRAW_PEAKS_LAST`](../../Reference/3dmap/M3dmapDraw.md)), it is possible to do so by calling [`M3dmapClear`](../../Reference/3dmap/M3dmapClear.md) with [`M_REMOVE_LAST_SCAN`](../../Reference/3dmap/M3dmapClear.md).
6. Repeat steps 3 to 5 for every reference plane that you want to use to calibrate your 3D reconstruction setup. If you are calibrating for a vertical laser plane (parallel to the XZ-plane), only one reference plane is required. If the laser plane is not vertical, as shown below, you must provide a minimum of 2 reference planes.
   *[Image: 3Dmap_setup_requirements_multiple_calibrations.png]*
   However, even if you are using a vertical laser plane, your results will be more accurate if you calibrate using more plane heights. Note that you can use the surface on which the object is resting as a reference plane.
   > **Note:** The size and depth of the source image buffer containing the laser lines must not change when making multiple calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), and you must use the same destination result buffer.
7. Call [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) with the 3D reconstruction context, calibration result buffer, and camera calibration context.
8. Use [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/3dmap/M3dmapInquire.md), while specifying the 3D reconstruction context, to ensure that the calibration of the 3D reconstruction setup has been successfully performed.
9. Free the 3D reconstruction calibration result buffer using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md).

The following code snippet is an example of the steps required to calibrate in [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode, in the case of a vertical laser plane setup (which requires only one reference plane).

> **Code example:** [userguide.3d_reconstruction.calibrating_for_fully_corrected_depth_maps](userguide.3d_reconstruction.calibrating_for_fully_corrected_depth_maps)

When you are set up to create a point cloud ([`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md)), calibrating your 3D reconstruction setup also defines the laser line coordinate system, a real-world coordinate system with its X-axis on the intersection between the laser plane and the conveyor belt, and its Y-axis along the path of the conveyor's movement. Initially, the relative coordinate system of the 3D reconstruction result buffer has the same pose as the laser line coordinate system. The relative coordinate system is used to return all 3D points generated by the 3D Reconstruction module. For more information, see [Understanding the laser line coordinate system](S07_3D_coordinate_systems.md).

## Inspecting laser line calibration

After calibrating an [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction context, it is possible to see how the laser lines detected at 3D reconstruction calibration were mathematically altered to best fit the equation of the laser plane; to do so, use [`M3dmapDraw`](../../Reference/3dmap/M3dmapDraw.md) with [`M_DRAW_CALIBRATION_LINES`](../../Reference/3dmap/M3dmapDraw.md) and the 3D reconstruction context.

You can draw all the extracted laser lines used to calibrate the 3D reconstruction context, using [`M3dmapDraw`](../../Reference/3dmap/M3dmapDraw.md) with [`M_DRAW_CALIBRATION_PEAKS`](../../Reference/3dmap/M3dmapDraw.md).

You can inquire the root mean squared error of the fit, using [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) with [`M_FIT_RMS_ERROR`](../../Reference/3dmap/M3dmapInquire.md).

## Specifying offsets when the 3D reconstruction calibration image is a subset of the grabbed image

When calibrating the 3D reconstruction setup, and you have an [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction context, it is important to note whether you are using a subset of the entire grabbed image during the runtime application. The subset of the grabbed image, whether through source offsets ([`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_SOURCE_OFFSET_...`](../../Reference/dig/MdigControl.md) and [`M_SOURCE_SIZE_...`](../../Reference/dig/MdigControl.md)) or child buffers ([`MbufChild2d`](../../Reference/buf/MbufChild2d.md)), must be the same during 3D Reconstruction calibration as it is during the runtime application.

If you are supplying a subset of the grabbed image, whether through source offsets or child buffers, to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), the origin of the image buffer passed to [`McalGrid`](../../Reference/cal/McalGrid.md) must correspond to the origin of the image buffer passed to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). If these two points do not correspond to the same real-world coordinate, you must specify the offset(s) of the image buffer used during 3D reconstruction calibration, before calling [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md)for the first time. Specify the offsets using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_EXTRACTION_CHILD_OFFSET_X`](../../Reference/3dmap/M3dmapControl.md) and [`M_EXTRACTION_CHILD_OFFSET_Y`](../../Reference/3dmap/M3dmapControl.md).

*[Image: 3dmap_child_buffer_simple.png]*

In the case where the image buffer used in camera calibration is a subset of the entire grabbed image, and the image buffer used in 3D reconstruction calibration is a subset of the subset, you must specify the offset with respect to the subset used for camera calibration.

*[Image: 3dmap_child_offset.png]*
