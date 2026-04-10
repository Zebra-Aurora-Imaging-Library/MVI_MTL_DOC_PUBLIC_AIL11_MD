---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Filling_missing_data
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Filling missing data"
---

# Filling missing data points (gaps)

Gaps, or invalid values, are missing data points in a depth map, where the 3D Reconstruction module or 3D sensor could not pick up information, due to occlusion, reflections, or areas out of the camera's field of view.

*[Image: 3dmap_missing_laser_line.png]*

These missing data points form gaps in the generated depth map. There are two types of gaps: a gradual elevation gap and a sharp elevation gap. Gaps are categorized into one of the two depending on their underlying surface. If the depth of the gap's underlying surface is considered to be gradual, the gap is considered to be a gradual elevation gap; if the depth of the gap's underlying surface is considered to be sharp, the gap is considered to be a sharp elevation gap.

The 3D Image Processing module makes assumptions about the type of gap based on the difference between boundary values of the gap in the depth map. If there is a small difference, the module assumes that the gap is a gradual elevation gap and if there is a large difference, it assumes that the gap is a sharp elevation gap.

*[Image: 3dmap_gap.png]*

You can fill gaps in a depth map or intensity map. To do so, specify the image buffer(s) and gap filling context using [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md). Then, if default settings are not sufficient, set required control type values using [`M3dimControl`](../../Reference/3dim/M3dimControl.md).

## Filling mode

The gap filling operation can either ignore gaps or fill them using either the X-then-Y or the Y-then-X filling mode. To specify the filling mode, use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_FILL_MODE`](../../Reference/3dim/M3dimControl.md).

The X-then-Y filling mode ([`M_X_THEN_Y`](../../Reference/3dim/M3dimControl.md)) first analyzes each depth map row and fills any missing data points it encounters in that row, according to the specified filling operation (discussed later). It then analyzes each column and fills any gaps it finds in that column, also according to the specified filling operation. Note that gaps whose boundaries touch the border of the image are not filled.

*[Image: 3dmap_x_then_y_2.png]*

The Y-then-X filling mode ([`M_Y_THEN_X`](../../Reference/3dim/M3dimControl.md)) is the same as the X-then-Y filling mode, except that the columns and rows are filled in the reverse order. The Y-then-X filling mode first analyzes each depth map column and then it analyzes each row. Again, gaps whose boundaries touch the border of the image are not filled.

*[Image: 3dmap_Y_then_X.png]*

For an 8- or 16-bit depth map image buffer, the gray values 255 and 65535 are used to indicate missing data points (gaps), respectively.

The gray values used to fill the gaps are approximations. If you require accurate results, see [Phenomena causing missing data](S16_Filling_missing_data.md) for suggestions on improving your 3D reconstruction setup to minimize occurrences of gaps.

When gradually adding data to the image buffer (using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) with [`M_ACCUMULATE`](../../Reference/3dim/M3dimProject.md)), gap filling is not performed, since it must be done after extracting all the laser line data. However, after all necessary data have been extracted, you can fill gaps in the depth map using [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md).

## Filling operations

To fill gaps, two filling operations are available. You can either propagate the value of one of the boundaries or do a linear interpolation between the two boundaries on each row or column of the gap. The choice of filling operation is based on whether the gap is considered a gradual or a sharp elevation gap. To determine which operation to perform, you can set a threshold above which gaps are considered sharp elevation gaps and are therefore filled by boundary propagation. Below the threshold, gaps are considered gradual and are filled using linear interpolation. Default settings consider all gaps as gradual, and linear interpolation is always used.

### Linear interpolation

A gradual elevation gap is a range of missing data where the difference of the boundary values of that gap's row or column is less than the threshold specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md). The underlying surface of such a gap is considered to be part of the same surface as its boundaries, so this type of gap is always filled with linearly interpolated values. For example, in the following image, [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) is set to 75.0. The values at the gap boundaries are 42 and 60. Since the difference, 18, is less than the specified threshold of 75.0, the gap will be filled with linearly interpolated values.

*[Image: 3dmap_linear_interpolation_2.png]*

You can specify to always use linear interpolation to fill missing data points, regardless of the depth difference at the boundaries, by setting [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) to [`M_INFINITE`](../../Reference/3dim/M3dimControl.md). This treats any elevation gap as being a gradual elevation gap.

### Propagation of one of the boundary values

A sharp elevation gap is a range of missing data where the underlying surface is considered to be discontinuous at least at one of its boundaries. The depth difference of the boundaries of the gap's row or column must be greater or equal to [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) for the gap to be considered a sharp elevation gap. For these types of gaps, you can select to either:

- Fill the sharp elevation gaps by propagating one of their boundaries. You should choose this fill operation if the gaps are not due to holes in the object.
- Leave the gaps as is by setting [`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) to [`M_DISABLE`](../../Reference/3dim/M3dimControl.md) because the gaps are due to holes in the object.

To propagate one of the boundary values, you can choose to fill the gaps with either the minimum or maximum pixel value of the gaps' boundaries. To do so, set [`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) to [`M_MIN`](../../Reference/3dim/M3dimControl.md) or [`M_MAX`](../../Reference/3dim/M3dimControl.md), respectively. If the X-then-Y filling mode is used, then gaps in each row will be filled first by propagating one of the boundaries on either side of the gap in each row, followed by the filling of the gaps in each column. Similarly, if the Y-then-X filling mode is used, gaps in each column will be filled first by propagating one of the boundaries on either side of the gap in each column, followed by the filling of the gaps in each row.

Note if one of the boundaries is invalid (in an 8-bit image, it has a value of 255), then the gap will not be filled.

*[Image: 3dmap_propagation_2.png]*

To use propagation of the boundary values to fill any type of gap, set [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) to 0.0. All gaps will be treated as being sharp elevation gaps.

## Phenomena causing missing data

Laser line information might be missing in the image due to different phenomena. Depending on your setup, you might be able to reduce the amount of missing data.

- **Camera occlusion and laser occlusion**. Camera occlusion occurs when the laser line is hidden from the camera's field of view by the object's geometry. Laser occlusion occurs when the object's geometry prevents the laser line from illuminating parts of the object's surface.
  To fix camera occlusion problems, try to position your camera in such a way that it is able to capture the laser line at all times. If this is not possible, add a camera to grab the laser lines that are hidden from the first camera. For information on setting up multiple cameras, see [Multiple camera-laser pairs](../C46_3D_reconstruction_using_laser_line_profiling/S09_Multiple_camera_laser_pairs.md).
  To fix laser occlusion problems, add a second laser to the setup, whose laser plane aligns and overlaps with the plane of the first laser and whose laser line illuminates the regions that are not illuminated by the first laser.
- **Reflectance**. The object's surface can absorb enough light to prevent laser line detection. For example, dark markings might not reflect enough light back to the camera.
  Increase the laser line's intensity to compensate for the light being absorbed. If this is not possible, adjust the iris opening (aperture) of your camera to gain more exposure.
- **Scaling**. The aspect ratio of the object in the depth map might be maintained by filling rows with missing data (255). This occurs when the camera is not able to grab an image with the laser line for each slice of the object.
  Decrease the speed at which the object is moving under the laser plane or increase the frame rate or resolution of the camera to provide more 3D data to the module. You can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_PIXEL_SIZE_X`](../../Reference/3dim/M3dimControl.md), [`M_PIXEL_SIZE_Y`](../../Reference/3dim/M3dimControl.md), and [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) to adjust the scale of the pixels and obtain a smaller sized depth map.
  > **Note:** This type of gap can only appear if the 3D reconstruction context was allocated in [`M_CALIBRATED_CAMERA_LINEAR_MOTION`](../../Reference/3dmap/M3dmapAlloc.md) 3D reconstruction mode.

The following image shows an example of camera occlusion, laser occlusion, and reflectance. In the case of camera occlusion and laser occlusion, you could use propagation of one of the boundary values because the gaps might be caused by sharp elevation differences. For the gaps due to reflectance, for example, you could use linear interpolation because the underlying surface of the gap is flat.

*[Image: 3dmap_gap_camera_laser_occlusion.png]*

## Examples

The following image shows the generated depth map of a cookie with missing laser data due to scaling as well as camera and laser occlusion.

*[Image: 3dmap_no_gap_filling.png]*

To fill these gaps, you can choose linear interpolation, propagation of one of the boundary values, or a mix of both. You can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_FILL_SHARP_ELEVATION`](../../Reference/3dim/M3dimControl.md) and [`M_FILL_SHARP_ELEVATION_DEPTH`](../../Reference/3dim/M3dimControl.md) to specify the filling operation.

*[Image: 3dmap_gaps_filling_behaviors.png]*
