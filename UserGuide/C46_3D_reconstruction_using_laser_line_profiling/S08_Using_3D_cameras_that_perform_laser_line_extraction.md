---
doctype: UserGuide
part: "3D related information"
chapter: 3D_reconstruction_using_laser_line_profiling
section: Using_3D_cameras_that_perform_laser_line_extraction
module_tag: 3dmap
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_reconstruction_using_laser_line_profiling / Using 3D cameras that perform laser line extraction"
---

# Using 3D cameras that output uncorrected depth maps

The 3D Reconstruction module provides support for 3D cameras that perform laser line extraction on-board and output an uncorrected depth map. You can use the 3D reconstruction module to convert this data into a point cloud. However, to use the 3D Reconstruction module, you must reformat the uncorrected depth map to the input format of [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). You must then call [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with [`M_LINE_ALREADY_EXTRACTED`](../../Reference/3dmap/M3dmapAddScan.md) and the reformatted uncorrected depth map. You can find examples of how this can be accomplished by following the **Aurora Imaging Library Processing Examples** link in the **Aurora Imaging Control Center** and looking under the **CameraLaser3d** directory.

> **Note:** Some 3D sensors (including some laser profilers) perform all steps of 3D reconstruction on-board and transmit 3D data in the form of a precalibrated point cloud or depth map. If you have one of these devices, you do not need to use Aurora Imaging Library to perform laser line profiling. This section only applies to data grabbed from cameras that perform laser-line extraction (but not calibration) on-board. For information on grabbing from 3D sensors that perform all steps of 3D reconstruction on-board, see[Grabbing from 3D sensors overview](../C42_Grabbing_from_3D_sensors/S01_Grabbing_from_3D_sensors_overview.md).

[`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md) with [`M_LINE_ALREADY_EXTRACTED`](../../Reference/3dmap/M3dmapAddScan.md) also supports uncorrected depth maps obtained using other means, provided they are in a supported format. This might be useful if any preprocessing or postprocessing is necessary during laser line extraction. For example, you can use the [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) function to create the uncorrected depth map. This function has an output format compatible with the input format of [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md), so no manipulation of the data is necessary. For information on how to use the [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) function to create an uncorrected depth map, see [Peak intensity detection and depth maps](../C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).

## Calibrating a 3D reconstruction setup that uses a 3D camera

Even when using a 3D camera that performs laser line extraction, you must calibrate your 3D reconstruction setup as described in [Calibrating your 3D reconstruction setup to create a point cloud](S05_Calibrating_your_reconstruction_setup.md). In this case, you should not pass an entire uncorrected depth map to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). Instead, for each gray value/height to use for calibration, you should only pass the uncorrected depth map row that corresponds to the laser line at the specified height ([`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_CORRECTED_DEPTH`](../../Reference/3dmap/M3dmapControl.md)); you must pass the row in a 1D image buffer (created, for example, using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md)).

## Laser data format

If the laser line data is extracted without making use of the 3D Reconstruction module, you must specify the format of the uncorrected depth map using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) and [`MimControl`](../../Reference/im/MimControl.md).

You must specify the number of bits used for the fractional part of each pixel's gray value in the resulting uncorrected depth map, using [`M3dmapControl`](../../Reference/3dmap/M3dmapControl.md) with [`M_EXTRACTION_FIXED_POINT`](../../Reference/3dmap/M3dmapControl.md) control type. For example, if the gray value 47 (101111 in binary) must be interpreted as 23.5 (10111.1), you will need to set [`M_EXTRACTION_FIXED_POINT`](../../Reference/3dmap/M3dmapControl.md) to 1.

You must also specify the scan lane direction of the laser line in the camera's field of view, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md). Essentially, you must specify the orientation of the laser line in the internal images that the camera takes to generate the depth map. You can select either [`M_VERTICAL`](../../Reference/im/MimControl.md), if the images contain a horizontal laser line, or [`M_HORIZONTAL`](../../Reference/im/MimControl.md), if the images contain a vertical laser line.

If the [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) control type is set to [`M_VERTICAL`](../../Reference/im/MimControl.md), the uncorrected depth map must contain _M_ rows and _N_ columns, where:

- _M_ is the number of internal  taken containing the laser line. One  generates one row of data, where  _m_ corresponds to row _m_ in the uncorrected depth map `(0 &lt;= _m_ &lt;= _M_ - 1)`.
- _N_ is the X-size, in pixels, of the laser line images. One pixel corresponds to one column of data, where the pixel distance of the laser line in column _n_ of the laser line image, corresponds to the pixel value in column _n_ in the uncorrected depth map `(0 &lt;= _n_ &lt;= _N_ - 1)`.

Each entry (_m_, _n_) in the uncorrected depth map must contain a value _d_, which is the vertical distance of the laser line in column _n_ of the laser line image _m_.

*[Image: 3dmap_orientation_columns.png]*

> **Note:** Note that the data _d_ should be stored in the uncorrected depth map as a pixel with a gray level value bit-shifted left by the number of times equal to the value of [`M_EXTRACTION_FIXED_POINT`](../../Reference/3dmap/M3dmapControl.md). For example, if [`M_EXTRACTION_FIXED_POINT`](../../Reference/3dmap/M3dmapControl.md) is set to 1, 23.5 (10111.1 in binary) should be stored as 47 (101111 in binary).

If the [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) control type is set to [`M_HORIZONTAL`](../../Reference/im/MimControl.md), the uncorrected depth map must contain _M_ rows and _N_ columns, where:

- _M_ is the number of internal  taken containing the laser line. One  generates one row of data, where  _m_ corresponds to row _m_ in the uncorrected depth map `(0 &lt;= _m_ &lt;= _M_ - 1)`.
- _N_ is the Y-size, in pixels, of the laser line images. One pixel corresponds to one row of data, where the pixel distance of the laser line in row _n_ of the laser line image corresponds to the pixel in column _n_ in the uncorrected depth map `(0 &lt;= _n_ &lt;= _N_ - 1)`.

Each entry (_m_, _n_) in the uncorrected depth map must contain a value _d_, which is the horizontal distance of the laser line at row _n_ in the laser line image _m_.

Note that while the orientation of the laser line in the image changes in this mode, the orientation of the uncorrected depth map does not. Consequently, row _n_ of pixels in the laser line images actually corresponds to column _n_ in the uncorrected depth map. This fact is illustrated in the following image.

*[Image: 3dmap_orientation_rows.png]*
