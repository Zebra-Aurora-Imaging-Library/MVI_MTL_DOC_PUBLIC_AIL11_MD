---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Multiple_camera_laser_pairs
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Multiple camera laser pairs"
---

# Multiple camera-laser pairs

When performing 3D reconstruction to create a point cloud, it is often useful to use multiple cameras and multiple laser planes to solve laser and camera occlusion problems. Using multiple cameras and multiple laser planes, you can create multiple point clouds of the same object(s), often revealing parts of the object(s) that would be occluded if using only one camera or laser plane.

All the cameras and lasers are divided into camera-laser pairs, one pair for each camera and laser combination. A camera or a laser can be used in multiple camera-laser pairs, but each camera-laser pair must be unique. Before being able to use multiple cameras and/or lasers, you must consider nuances to the setup and 3D reconstruction calibration of the multiple camera-laser pairs.

## Multiple camera-laser pair setup

In a multiple camera-laser pair setup, each camera and each laser can be used in one or more camera-laser pairs. Each camera-laser pair is defined by its own 3D reconstruction context. During allocation, you must specify a camera label ([`M_CAMERA_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md)) for the camera being used and a laser label ([`M_LASER_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md)) for the laser being used. A camera label can be any positive integer that is unique among the labels assigned to other cameras; the same applies for a laser label. The following example illustrates two cameras and two lasers organized into three camera-laser pairs. Note that camera label 1 is used in two contexts (context 1 and context 2) and laser label 2 is also used in two contexts (context 2 and context 3).

*[Image: 3dmap_multiple_lasers_multiple_cameras_setup.png]*

|   |   |   |
| --- | --- | --- |
| 3D reconstruction context | Camera label | Laser label |
| Context1 | 1 | 1 |
| Context2 | 1 | 2 |
| Context3 | 2 | 2 |

> **Note:** Note that multiple camera-laser pairs are only available in [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) mode.

### Two lasers one camera

In the case where you have a camera that is being used in two separate camera-laser pairs, images grabbed by the camera contain both laser lines, as shown in the image below. Passing an image with two distinct laser lines to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) will produce unpredictable results, so you must partition the image into two child image buffers ([`MbufChild2d`](../../Reference/buf/MbufChild2d.md)). Each child buffer must show only the laser line for a single laser. For the procedure to partition an image buffer into child buffers, see[Child buffers, regions of interest, and fixturing](../C02_Building_an_application/S10_Child_buffers_regions_of_interest_and_fixturing.md).

*[Image: 3dmap_two_laser_lines_in_one_image.png]*

To ensure that the laser line of one laser does not cross over into the child image buffer intended for the other laser, you can setup your lasers such that their laser planes intersect above the tallest point of the objects that you are scanning; you must place your camera above the object looking down at it. The following image demonstrates such a setup.

*[Image: 3dmap_multiple_lasers_one_camera.png]*

Remember that you must pass the identifier of the child buffer to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) and not the identifier of the original image buffer.

## Calibrating the 3D reconstruction contexts in a multiple camera-laser pair setup

Calibrating a 3D reconstruction setup with multiple camera-laser pairs is similar to calibrating with a single camera-laser pair, which requires that you pass a 3D reconstruction context, an [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer with laser line data in it, and the camera calibration context to [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md). With multiple camera-laser pairs, you supply an array of 3D reconstruction contexts, an array of [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffers with laser line data in them, and an array of camera calibration contexts to [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md).

The most important thing to consider in the process of creating these arrays and filling them with all the data [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) needs to calibrate the setup, is the order of the contexts and result buffers in their respective arrays. The first 3D reconstruction context in its array, the first [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer in its array, and the first camera calibration context in its array all correspond to the first camera-laser pair. The second 3D reconstruction context, the second [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md)result buffer, and the second camera calibration context correspond to the second camera-laser pair, and so on. Consequently, all three arrays must be the same size, which is the total number of camera-laser pairs in your setup. It is important to note that for the camera calibration context array to be the same size as the other two arrays, you might repeat the same camera calibration context identifier.

You pass these three arrays to [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md), which calibrates each 3D reconstruction context/camera-laser pair.

The following table shows the three arrays needed for the two camera/two laser setup used in the example found in [Multiple camera-laser pair setup](S09_Multiple_camera_laser_pairs.md).

|   |   |   |   |
| --- | --- | --- | --- |
| Camera-laser pair | 3D reconstruction context array [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) | 3D reconstruction calibration result buffer array [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) | Camera calibration context array [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) |
| Camera1/Laser1 (Array index 0) | 3dmap context 1 | 3dmap result buffer 1 | Camera calibration context 1 |
| Camera1/Laser2 (Array index 1) | 3dmap context 2 | 3dmap result buffer 2 | Camera calibration context 1 |
| Camera2/Laser2 (Array index 2) | 3dmap context 3 | 3dmap result buffer 3 | Camera calibration context 2 |

To calibrate the 3D reconstruction contexts in a multiple camera-laser pair setup, you need three arrays: an array of 3D contexts, an array of 3D result buffers, and an array of calibrated camera calibration contexts. To create and fill the arrays, perform the following:

1. Create an array and fill it with the identifiers of 3D reconstruction contexts. Each camera-laser pair in your setup must have its own 3D reconstruction context, allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md). Each context must be set to [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) and must have a value for [`M_CAMERA_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md) and [`M_LASER_LABEL()`](../../Reference/3dmap/M3dmapAlloc.md).
2. Create a second array and fill it with the identifiers of 3D reconstruction result buffers. Each camera-laser pair in your setup must have its own 3D reconstruction result buffer, allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md). Each result buffer must be of type [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).
   > **Note:** Note that both the first and second array must be the same size, since they both require an index for each camera-laser pair.
3. Create a third array the same size as the first two and fill it with the identifiers of camera calibration contexts. Each camera in your setup must have its own camera calibration context, allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), and calibrated using[`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md). For details, see [Calibrating using calibration points from a grid](../C28_Calibration/S10_Calibrating_using_calibration_points_from_a_grid.md) and [Calibrating using calibration points from a list](../C28_Calibration/S12_Calibrating_using_calibration_points_from_a_list.md). For each camera, use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md) to ensure that the camera calibration has been successfully performed.
   In the case of a camera being used in more than one camera-laser pair, the camera calibration context array must repeat the identifier for its camera calibration, as in the above example.
   When calibrating multiple cameras for a 3D reconstruction setup, there are three special considerations:
   - Some cameras might have to specify an X and Y offset from the first camera, using [`McalGrid`](../../Reference/cal/McalGrid.md) with [`GridOffsetX`](../../Reference/cal/McalGrid.md) and [`GridOffsetY`](../../Reference/cal/McalGrid.md), so that all cameras exist in the same absolute coordinate system. See [Several fixed cameras and fixed object: grid offset example](../C28_Calibration/S16_Calibrating_a_camera_setup_that_analyzes_large_objects.md).
   - You might have to specify the orientation of the grid used to calibrate each camera, via a hint pixel. See [Determining the grid calibration reference point](../C28_Calibration/S10_Calibrating_using_calibration_points_from_a_grid.md).
   - For greater accuracy, you might have to specify the thickness of your grid for each camera calibration using [`McalGrid`](../../Reference/cal/McalGrid.md) with [`GridOffsetZ`](../../Reference/cal/McalGrid.md). See [Moving the relative coordinate system to account for height](../C34_3D_analysis_using_planar_views_of_an_object/S02_Moving_the_relative_coordinate_system_to_account_for_height.md).

Once you have the three arrays, you must extract laser line information from images into the [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffers. To extract the laser line information, perform the following:

1. Grab an image of a laser line on a planar object that has a known height.
   Use the camera associated with the first 3D reconstruction context in the 3D reconstruction context array.
2. Specify the Z-coordinate of the top of that planar object in the absolute coordinate system, using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with the first 3D reconstruction context in the 3D reconstruction context array and with [`M_CORRECTED_DEPTH`](../../Reference/3dmap/M3dmapControl.md). Since by default the Z-axis of the absolute coordinate system is pointing down, this value is often negative.
3. Use [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) to extract the laser line information from the grabbed image and store it in the result buffer. When calling [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), the 3D reconstruction calibration result buffer and the 3D reconstruction context must be in the same array position in their respective arrays. For example, if you specify the 3D reconstruction context in the first array position of the context array, you must also specify the 3D reconstruction calibration result buffer in the first position in the result buffer array.
   For other methods of providing the laser line data, see [Using 3D cameras that output uncorrected depth maps](S08_Using_3D_cameras_that_perform_laser_line_extraction.md).
4. Repeat this process for each camera-laser pair, ensuring the use of the correct result buffer and context in their respective arrays.
5. Repeat steps 1 to 4 for every plane height that you want to use to calibrate your 3D reconstruction setup for that camera-laser pair. If you are calibrating for a vertical laser plane (parallel to the XZ-plane), only one reference plane is required. If the laser plane is not vertical (that is, the laser plane is at an angle from the XZ-plane), you must provide a minimum of 2 reference planes. However, even if you are using a vertical laser plane, your results will be more accurate if you calibrate using more plane heights. Note that you can use the surface on which the object is resting as a reference plane.
   > **Note:** The size and pixel depth of the source image buffer containing the laser lines must not change when making multiple calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) while using the same destination result buffer.

Once you have provided the 3D reconstruction contexts and result buffers with depth information, perform the following:

1. Call [`M3dmapCalibrateMultiple`](../../Reference/3dmap/M3dmapCalibrateMultiple.md) with the 3D reconstruction context array, the 3D reconstruction result buffer array, and the camera calibration context array, and specify the size of the arrays.
2. For each 3D reconstruction context, use [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/3dmap/M3dmapInquire.md) to ensure that the calibration of the 3D reconstruction setup has been successfully performed.
3. Free the 3D reconstruction calibration result buffers using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md).
