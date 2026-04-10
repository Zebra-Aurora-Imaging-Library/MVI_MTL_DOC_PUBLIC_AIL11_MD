---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Partially_corrected_depth_maps
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Partially corrected depth maps"
---

# Partially corrected depth maps

Depth maps come in three types: uncorrected, partially corrected, and fully corrected. Partially corrected depth maps are corrected (calibrated) for depth, but not for camera distortion. Their pixel intensities correspond to the heights of the scanned objects, but their pixel coordinates do not correspond to the real-world coordinates of scanned objects.

Generating a partially corrected depth map is similar to generating a fully corrected depth map, with a few exceptions. To generate a partially corrected depth map, you must allocate an [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction context. Once this is done, you must calibrate your 3D reconstruction context for depth; however, to generate a partially corrected depth map, you don't need to calibrate the camera. Once the setup has been calibrated and tested, you must allocate a 3D reconstruction result buffer of type [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) for runtime scanning.

*[Image: 3dmap_example_types_depthmaps.png]*

Note that a partially corrected depth map can only be generated from a single camera-laser pair.

## Calibrating for a partially corrected depth map

To generate a partially corrected depth map, you must first have a properly configured 3D reconstruction setup, which includes the physical setup (such as camera placement and laser line setup) and can include adjusting Aurora Imaging Library settings. For more information, see [Configuring the laser line profiling setup](S04_Configuring_the_laser_line_profiling_setup.md). Once this is complete, you must calibrate the 3D reconstruction context for depth as follows. Note that the following procedure will calibrate the 3D setup for depth and does not calibrate the camera, which is unnecessary for partial depth maps.

1. Allocate a 3D reconstruction context using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md).
2. Allocate a 3D reconstruction calibration result buffer using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).
3. Grab an image of a laser line on a planar object that has a known height. The image of a laser line on an object at a known height is called a reference plane.
4. Specify the gray value associated with the height of the reference plane, using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_CORRECTED_DEPTH`](../../Reference/3dmap/M3dmapControl.md).
5. Use [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) to extract the laser line information from the grabbed image and add this information to the 3D reconstruction calibration result buffer. For other methods of providing the laser line data, see [Using 3D cameras that output uncorrected depth maps](S08_Using_3D_cameras_that_perform_laser_line_extraction.md).
   > **Note:** If, after calling [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), but before calling [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md), you need to discard the last scan added (for example, as the result of a diagnosis using [`M3dmapDraw`](../../Reference/3dmap/M3dmapDraw.md) with [`M_DRAW_PEAKS_LAST`](../../Reference/3dmap/M3dmapDraw.md)), it is possible to do so by calling [`M3dmapClear`](../../Reference/3dmap/M3dmapClear.md) with [`M_REMOVE_LAST_SCAN`](../../Reference/3dmap/M3dmapClear.md).
6. Repeat steps 3 to 5 for every reference plane that you want to use to calibrate your 3D reconstruction setup. You can calibrate using only a single reference plane; however, your results will be more accurate if you calibrate using more plane heights. Note that you can use the surface on which the object is resting as a reference plane.
   > **Note:** The size and depth of the source image buffer containing the laser lines must not change when making multiple calls to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) and you must use the same destination result buffer.
7. Call [`M3dmapCalibrate`](../../Reference/3dmap/M3dmapCalibrate.md) with the 3D reconstruction context, calibration result buffer, and camera calibration context.
8. Use [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/3dmap/M3dmapInquire.md), while specifying the 3D reconstruction context, to ensure that the calibration of the 3D reconstruction setup has been successfully performed.
9. Free the 3D reconstruction calibration result buffer using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md).

For heights between those defined during 3D reconstruction calibration, the module will perform linear interpolation.

You can inspect the 3D reconstruction calibration state of the pixels once the 3D context has been calibrated. See [Inspecting the calibration state of a 3D reconstruction context](S10_Partially_corrected_depth_maps.md).

The following code snippet is an example of the steps required to calibrate a 3D reconstruction setup that can generate a partially corrected depth map.

> **Code example:** [userguide.3d_reconstruction.calibrating_for_partially_corrected_depth_maps](userguide.3d_reconstruction.calibrating_for_partially_corrected_depth_maps)

## Generating a partially corrected depth map

Once your [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction context has been calibrated, you are ready for runtime scanning. The following steps provide a basic methodology for runtime scanning using the Aurora Imaging Library 3D Reconstruction module.

1. Allocate a new 3D reconstruction result buffer to hold the partially corrected depth map, using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md).
2. Grab a series of images of an object as it passes through the laser plane. For each image, extract the laser line information from the image and add the data to the [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer, using [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md).
   The 3D reconstruction result buffer now contains a partially corrected depth map.
3. Copy the depth map to an image buffer using [`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md)with [`M_PARTIALLY_CORRECTED_DEPTH_MAP`](../../Reference/3dmap/M3dmapCopyResult.md).
4. If required, obtain additional information about the depth map using [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md).
5. Free all your allocated objects using [`M3dmapFree`](../../Reference/3dmap/M3dmapFree.md) and [`McalFree`](../../Reference/cal/McalFree.md).

In [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode, [`M3dmapCopyResult`](../../Reference/3dmap/M3dmapCopyResult.md) with [`M_PARTIALLY_CORRECTED_DEPTH_MAP`](../../Reference/3dmap/M3dmapCopyResult.md) generates a partially corrected depth map and tries to store the entire depth map in the specified image buffer. As such, there are restrictions on the dimensions of the image buffer used to store the generated depth map.

- The X-size of the depth map image buffer must be at least the X-size of the laser line images, if [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_VERTICAL`](../../Reference/im/MimControl.md). If [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_HORIZONTAL`](../../Reference/im/MimControl.md), the X-size of the depth map image buffer must be at least the Y-size of the laser line images.
  *[Image: 3dmap_orientation.png]*
  > **Note:** Note that to inquire the scan lane direction, you must first call [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md) with [`M_LOCATE_PEAK_1D_CONTEXT_ID`](../../Reference/3dmap/M3dmapInquire.md) to acquire the internal image processing context, then use it on [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimInquire.md).
- The required Y-size of the depth map image buffer depends on the number of laser scan lines stored in the result buffer. As such, the Y-size of the depth map image buffer must be bigger than either the number of laser lines extracted or the value of [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_MAX_FRAMES`](../../Reference/3dmap/M3dmapControl.md), whichever is smaller.
- The depth map image buffer must be an 8-bit or 16-bit unsigned buffer.

> **Note:** Note that you can call [`M3dmapGetResult`](../../Reference/3dmap/M3dmapGetResult.md) with [`M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_X`](../../Reference/3dmap/M3dmapGetResult.md) or [`M_PARTIALLY_CORRECTED_DEPTH_MAP_SIZE_Y`](../../Reference/3dmap/M3dmapGetResult.md) to determine the minimum required X-size and Y-size of the depth map image buffer, respectively.

The following code snippet shows how to generate a partially corrected depth map.

> **Code example:** [userguide.3d_reconstruction.extracting_depth_map](userguide.3d_reconstruction.extracting_depth_map)

### Inspecting the calibration state of a 3D reconstruction context

After calibrating a 3D reconstruction context in [`M_DEPTH_CORRECTION`](../../Reference/3dmap/M3dmapAlloc.md) mode, it is possible to inspect the 3D reconstruction calibration state of each pixel in subsequent laser line images, using either [`M3dmapDraw`](../../Reference/3dmap/M3dmapDraw.md) or [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md). By doing so, you can determine whether the pixels are well calibrated, or if they are interpolated, inverted, or left uncalibrated due to missing data. Unless otherwise specified, if you have regions with missing or invalid data, you should probably recalibrate your 3D reconstruction setup for depth.

Using [`M3dmapDraw`](../../Reference/3dmap/M3dmapDraw.md), you can draw:

- The region(s) where the laser line can appear in subsequent laser line images ([`M_DRAW_REGION_VALID`](../../Reference/3dmap/M3dmapDraw.md)). This operation draws all pixels that have a valid height associated with them.
  *[Image: 3dmap_draw_valid.png]*
- The region(s) where the laser line cannot appear in subsequent laser line images, because the images are not calibrated in this region ([`M_DRAW_REGION_UNCALIBRATED`](../../Reference/3dmap/M3dmapDraw.md)). This operation draws all pixels which are not associated with a valid height, because they are found outside the region bounded by the calibration laser lines.
  *[Image: 3dmap_draw_uncalibrated.png]*
- The region(s) where the laser line can appear in subsequent laser line images, but where results are less accurate due to interpolation ([`M_DRAW_REGION_INTERPOLATED`](../../Reference/3dmap/M3dmapDraw.md)). This operation draws all pixels that are calibrated by interpolation, due to missing data (gaps) in the calibration laser lines. These pixels have a valid height associated with them, but the height is less accurate than if there had been no missing data in the calibration laser lines.
  *[Image: 3dmap_draw_interpolated.png]*
- The region(s) in subsequent laser line images where the laser line is interpreted incorrectly due to an inversion during 3D reconstruction calibration ([`M_DRAW_REGION_INVERTED`](../../Reference/3dmap/M3dmapDraw.md)). An inversion occurs when a calibration laser line specified to be at a lower height is found above a calibration laser line specified to be at a higher height. For example, if the laser line representing a height of 24 is found above the laser line representing a height of 36, the region between these laser lines is inverted. These pixels have a valid height associated with them; however, they will be incorrect. Within an inverted region, pixels that should be associated with a height greater than that of pixels on the previous row (or column, depending on the scan orientation) are instead associated with lower heights.
  *[Image: 3dmap_draw_inverted.png]*
- The region(s) where the laser line cannot appear in subsequent laser line images because of missing data in the calibration laser lines, and where the pixels cannot be calibrated by interpolation ([`M_DRAW_REGION_MISSING_DATA`](../../Reference/3dmap/M3dmapDraw.md)). This operation draws all pixels that are not associated with a valid height because of missing data (gaps) in the calibration laser lines and that cannot be calibrated by interpolation.
  *[Image: 3dmap_draw_missing_data.png]*

The following image illustrates how to use [`M_DRAW_REGION_...`](../../Reference/3dmap/M3dmapDraw.md) to diagnose your calibration of the laser line image.

*[Image: 3dmap_draw_region.png]*

With [`M3dmapInquire`](../../Reference/3dmap/M3dmapInquire.md), it is possible to obtain this 3D reconstruction calibration information as statistics. Based on the number of laser line calibration planes grabbed during 3D reconstruction calibration, you can retrieve an array detailing the number of missing depth values (data points) or inversions per column (using [`M_NUMBER_OF_MISSING_DATA_PER_COLUMN`](../../Reference/3dmap/M3dmapInquire.md) or [`M_NUMBER_OF_INVERSIONS_PER_COLUMN`](../../Reference/3dmap/M3dmapInquire.md), respectively). For example, in the image above, 5 depth maps planes were grabbed during 3D reconstructions calibration and therefore the number of missing data points or inversions can be any whole number from 0 to 5 for every column. Alternatively, you can retrieve the number of columns with missing data points or with inversions (using [`M_NUMBER_OF_COLUMNS_WITH_MISSING_DATA`](../../Reference/3dmap/M3dmapInquire.md) or [`M_NUMBER_OF_COLUMNS_WITH_INVERSIONS`](../../Reference/3dmap/M3dmapInquire.md), respectively).
