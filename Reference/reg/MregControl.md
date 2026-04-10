---
doctype: Reference
module: reg
function: MregControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregControl"
---

# MregControl

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Control a registration context, registration element, or registration result buffer setting.

## Syntax

```c
void MregControl(
    AIL_ID     ContextOrResultId,  //out
    AIL_INT    Index,              //in
    AIL_INT64  ControlType,        //in
    AIL_DOUBLE ControlValue        //in
)
```

## Description

This function controls the settings of a registration context or a registration result buffer. It also controls the registration elements of a correlation-stitching, photometric stereo, or high dynamic range registration context. For registration contexts and registration elements, these settings control the execution of [`MregSetLocation`](../../Reference/reg/MregSetLocation.md), [`MregCalculate`](../../Reference/reg/MregCalculate.md), and/or [`MregDraw`](../../Reference/reg/MregDraw.md) operations. For correlation-stitching registration result buffers, these settings control the execution of [`MregDraw`](../../Reference/reg/MregDraw.md) and [`MregTransformImage`](../../Reference/reg/MregTransformImage.md) operations.

Use [`MregInquire`](../../Reference/reg/MregInquire.md) to inquire about control type settings.

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies the identifier of the registration context or registration result buffer whose settings you can modify; you can only control settings of correlation-stitching or photometric stereo types of registration result buffers. The registration context must have been previously allocated on the system using [`MregAlloc`](../../Reference/reg/MregAlloc.md). The correlation-stitching or photometric stereo registration result buffer must have been previously allocated on the system using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) or [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md).

### `Index` *(in, AIL_INT)*

Specifies that a registration context, an individual registration element (of a correlation-stitching, photometric stereo, or high dynamic range registration context), or a registration result buffer is controlled. Set this parameter to one of the following values:

*For the registration context, registration element, or registration result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If an [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context is specified, same as [`M_ALL`](../../Reference/reg/MregControl.md).

If an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md), [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md), [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md), or [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md)registration context is specified, same as [`M_CONTEXT`](../../Reference/reg/MregControl.md).

If a registration result buffer is specified, same as [`M_GENERAL`](../../Reference/reg/MregControl.md). |
| `M_ALL` | Applies the specified control setting to all registration elements, if a correlation-stitching, photometric stereo, or high dynamic range registration context is specified. Note that this setting is not available for an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) or [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md) registration context. |
| `M_CONTEXT` | Controls a general setting of a registration context, if one is specified. |
| `M_GENERAL` | Controls a general setting of a registration result buffer, if one is specified. |
| `0 < Value < M_NUMBER_OF_REGISTRATION_ELEMENTS` | Specifies the index of the individual registration element to control, if a correlation-stitching, photometric stereo, or high dynamic range registration context is specified.

Note that this setting is not available for an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) or [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md)registration context. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_STITCHING`](../../Reference/reg/MregAlloc.md), [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) or [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md).

---

### `M_NUMBER_OF_REGISTRATION_ELEMENTS`

Sets the number of registration elements in the registration context. If you reduce the number of registration elements, the settings of the ones that remain are kept unchanged. If you increase the number of registration elements, the new ones are initialized with the default settings.  You should have at least as many registration elements as the number of images that you want to register.  For a photometric stereo registration context, the number of registration elements also corresponds to the maximum number of lights used during the photometric stereo computation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For a correlation-stitching registration context, the default value is 256. For a photometric stereo or high dynamic range registration context, the default value is 16. |
| `2 <= Value < 8192` | Specifies the number of registration elements for a correlation-stitching registration context. |
| `Value > 3` | Specifies the number of registration elements for a photometric stereo registration context. |
| `Value >= 2` | Specifies the number of registration elements for a high dynamic range registration context. |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md).

---

### `M_ACCURACY`

Sets the accuracy of the registration calculation. Accuracy depends on the image's dynamic range, sharpness, and noise. The best accuracy is achieved for well-contrasted noise-free images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies high accuracy. Registration will be calculated with subpixel accuracy. |
| `M_LOW` | Specifies low accuracy. Registration will be calculated with pixel accuracy. |

---

### `M_LOCATION_DELTA`

Sets the maximum displacement that will be applied to any pixel in the images during registration, relative to its initial location set using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md). With a larger maximum displacement, you can align the images even if the location that was set using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) was not precise. However, this can lead to a longer calculation time.  > **Note:** Note that if you have not set the rough location using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md), set [`M_LOCATION_DELTA`](../../Reference/reg/MregControl.md) to 100, so that all possible translations are searched.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum displacement as a percentage. The displacement is expressed an a percentage of the biggest dimension of the images to align. |

---

### `M_MIN_OVERLAP`

Sets the minimum overlap that should exist between an image and its reference image so that the registration calculation can optimize the match in their overlapping region. If the minimum overlap is not met, the optimization step will not be performed for this image. Instead, the transformation that was set using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) becomes the optimal transformation.  The image's registration result element will indicate that the registration operation failed for this image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the minimum overlap as a percentage. The overlap is expressed as a percentage of the smallest of the two images. |

---

### `M_SCORE_TYPE`

Sets the type of score calculated during registration. This calculated score can be retrieved after registration using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_SCORE`](../../Reference/reg/MregGetResult.md).

| Value | Description |
| --- | --- |
| `M_CORRELATION` | Specifies to calculate the score based on the normalized grayscale correlation in the overlapped region. |
| `M_NONE` *(default)* | Specifies that no score is calculated; it is set to 100%. |

---

### `M_STITCHING_LAST_LEVEL`

Sets the resolution level for the final stage (highest resolution) of the correlation algorithm.  Note that if the specified level is not supported by the correlation algorithm, the highest acceptable level will be used. It only needs to be controlled in rare situations, where images sizes are large and contain small details.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the last level is set automatically based on image size. |
| `0 <= Value <= 16` | Specifies the resolution level for the final stage of the correlation algorithm. |

---

### `M_TRANSFORMATION_TYPE`

Sets the type of transformation that the registration calculation will use to optimize the match in the images' overlapping regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PERSPECTIVE` | Specifies that a perspective warping can be performed to optimize the match in the overlapping regions. |
| `M_TRANSLATION` *(default)* | Specifies that a translation can be performed to optimize the match in the overlapping regions. |
| `M_TRANSLATION_ROTATION` | Specifies that a translation and a rotation can be performed to optimize the match in the overlapping regions. |
| `M_TRANSLATION_ROTATION_SCALE` | Specifies that a translation, a rotation, and a scale operation can be performed to optimize the match in the overlapping regions. |

### For registration elements (one or all) of an

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control settings of registration elements (one or all) of an [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context.

---

### `M_OPTIMIZE_LOCATION`

Sets whether to perform the optimization step of the registration calculation for the registration element's image. If you choose not to perform the optimization calculation, the transformation that you set with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) becomes the optimal transformation for this image. When [`MregCalculate`](../../Reference/reg/MregCalculate.md) is called, the transformation is converted so that it maps the image into the global pixel coordinate system instead of into its reference image's pixel coordinate system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to perform the optimization calculation. |
| `M_ENABLE` *(default)* | Specifies to perform the optimization calculation. |

---

### `M_REFERENCE_X`

Sets the X-coordinate of the origin of the image's pixel coordinate system.  This is an advanced control type and changing the origin of an image's pixel coordinate system will affect various aspects of the registration process. This origin is used when specifying the approximate location of the image in its registration element's image ([`MregSetLocation`](../../Reference/reg/MregSetLocation.md)); you must specify the location of the origin with respect to the origin of the reference element's image. Similarly, if you use the image to position other images, specify locations relative to this origin. The position of your mosaic in the destination image buffer depends on the mosaic's reference coordinate system ([`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md)), which can be the coordinate system of a particular image. Finally, you can convert coordinates to and from an image's pixel coordinate system, with [`MregTransformCoordinate`](../../Reference/reg/MregTransformCoordinate.md) and [`MregTransformCoordinateList`](../../Reference/reg/MregTransformCoordinateList.md). If you modify the origin of an image's pixel coordinate system, you must keep the origin's new location in mind when performing any of these operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default X-coordinate of the origin will be used.  The default value is 0.0. |
| `M_CENTER_ELEMENT` | Specifies the X-coordinate of the origin to be at the center of the image. |
| `Value` | Specifies the X-coordinate of the origin, in pixels. The value can be specified with subpixel accuracy. |

---

### `M_REFERENCE_Y`

Sets the Y-coordinate of the origin of the image's pixel coordinate system.  This is an advanced control type and changing the origin of an image's pixel coordinate system will affect various aspects of the registration process. This origin is used when specifying the approximate location of the image in its registration element's image ([`MregSetLocation`](../../Reference/reg/MregSetLocation.md)); you must specify the location of the origin with respect to the origin of the reference element's image. Similarly, if you use this image to position other images, specify locations relative to this origin. The position of your mosaic in the destination image buffer depends on the mosaic's reference coordinate system ([`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md)), which can be the coordinate system of a particular image. Finally, you can convert coordinates to and from an image's pixel coordinate system, with [`MregTransformCoordinate`](../../Reference/reg/MregTransformCoordinate.md) and [`MregTransformCoordinateList`](../../Reference/reg/MregTransformCoordinateList.md). If you modify the origin of an image's pixel coordinate system, you must keep the origin's new location in mind when performing any of these operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default Y-coordinate of the origin will be used.  The default value is 0.0. |
| `M_CENTER_ELEMENT` | Specifies the Y-coordinate of the origin to be at the center of the image. |
| `Value` | Specifies the Y-coordinate of the origin, in pixels. The value can be specified with subpixel accuracy. |

### For

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control the settings of an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_GENERAL`](../../Reference/reg/MregControl.md).

---

### `M_MOSAIC_COMPOSITION`

Sets which image's pixel values to use when composing the mosaic and two or more images overlap.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE_IMAGE` | Specifies to use the average value of the images' pixels in the overlapping region. |
| `M_FIRST_IMAGE` | Specifies to use the pixels of the image associated with the registration result element with the lowest index. |
| `M_FUSION_IMAGE` | Specifies to fuse the images by progressively blending overlapping pixels. That is, overlapping pixel values are modified (blended) to form a transitional portion. Blending is based on the distance between each pixel and the edges of the images in the mosaic. |
| `M_LAST_IMAGE` *(default)* | Specifies to use the pixels of the image associated with the registration result element with the highest index. |
| `M_SUPER_RESOLUTION` | Specifies to use a super-resolution algorithm to create the mosaic.  For best results, the image alignment should be accurate. To perform registration calculations with subpixel accuracy, call [`MregControl`](../../Reference/reg/MregControl.md)with [`M_ACCURACY`](../../Reference/reg/MregControl.md) set to [`M_HIGH`](../../Reference/reg/MregControl.md). The source images should be of good quality with clear, distinct features in the overlapping region.  To customize the super-resolution process, set the point-spread function, its radius, and the degree of smoothness to use, with [`M_SR_PSF_TYPE`](../../Reference/reg/MregControl.md), [`M_SR_PSF_RADIUS`](../../Reference/reg/MregControl.md), and [`M_SR_SMOOTHNESS`](../../Reference/reg/MregControl.md) respectively. |

---

### `M_MOSAIC_OFFSET_X`

Sets the X-offset between the origin of the coordinate system used to compose the mosaic and the left side of the destination image buffer. The coordinate system used to compose the mosaic is the one specified with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALIGN_LEFT` *(default)* | Specifies that the X-offset (in pixels) will be calculated such that the left-most part of the mosaic will be aligned with the left side of the destination image buffer. |
| `Value` | Specifies the X-offset, in pixels. |

---

### `M_MOSAIC_OFFSET_Y`

Sets the Y-offset between the origin of the coordinate system used to compose the mosaic and the top of the destination image buffer. The coordinate system used to compose the mosaic is the one specified with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALIGN_TOP` *(default)* | Specifies that the Y-offset (in pixels) will be calculated such that the top-most part of the mosaic will be aligned with the top of the destination image buffer. |
| `Value` | Specifies the Y-offset, in pixels. |

---

### `M_MOSAIC_SCALE`

Sets the scale factor to apply to the images before composition of the mosaic. A value greater than 1.0 produces an enlarged mosaic, while a value less than 1.0 produces a reduced one.

| Value | Description |
| --- | --- |
| `0.0 < Value <= 10.0` *(default)* | Specifies the scale factor. |

---

### `M_MOSAIC_STATIC_INDEX`

Sets the coordinate system relative to which the mosaic will be composed. This can be a registered image's pixel coordinate system or the global pixel coordinate system. If you select an image's pixel coordinate system, that image will appear upright in the mosaic, and the other images will be oriented relative to it.  By default, the origin of the mosaic will appear at the top-left corner of the destination image buffer. You can adjust the horizontal and vertical offset of the origin using [`M_MOSAIC_OFFSET_X`](../../Reference/reg/MregControl.md) and [`M_MOSAIC_OFFSET_Y`](../../Reference/reg/MregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. By default, the mosaic will be composed with respect to the pixel coordinate system of the image associated with registration result element 0. |
| `M_ALL` | Specifies that the coordinate system will be chosen such that the minimum change is done on all images during the mosaic composition. This setting is only available when composing a mosaic with only two images. |
| `M_REGISTRATION_GLOBAL` | Specifies that the mosaic will be composed with respect to the global pixel coordinate system. |
| `0 <= Value < NumberOfElements` | Specifies the index of the registration result element whose image's pixel coordinate system will be used as the reference coordinate system. |

---

### `M_SR_PSF_RADIUS`

Sets the radius of the [`M_CIRCULAR`](../../Reference/reg/MregControl.md) and [`M_GAUSSIAN`](../../Reference/reg/MregControl.md) point-spread functions (PSF). For [`M_GAUSSIAN`](../../Reference/reg/MregControl.md), the radius corresponds to the standard deviation. For [`M_SQUARE`](../../Reference/reg/MregControl.md), the radius corresponds to half the size of the square side.  This control type is only taken into account when [`M_MOSAIC_COMPOSITION`](../../Reference/reg/MregControl.md) is set to [`M_SUPER_RESOLUTION`](../../Reference/reg/MregControl.md) and [`M_SR_PSF_TYPE`](../../Reference/reg/MregControl.md) is not set to [`M_DISABLE`](../../Reference/reg/MregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the radius of the PSF, in pixel units at the scale of the source images. |

---

### `M_SR_PSF_TYPE`

Sets the type of point-spread function (PSF) to use during super-resolution calculations to model the blurring in source images.  The point-spread function describes how a small point of light in the world is blurred by the acquisition setup (lens and CCD). Use [`M_SR_PSF_RADIUS`](../../Reference/reg/MregControl.md) to set the radius of the PSF.  The value of this control type is only taken into account when [`M_MOSAIC_COMPOSITION`](../../Reference/reg/MregControl.md) is set to [`M_SUPER_RESOLUTION`](../../Reference/reg/MregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCULAR` | Specifies to assume a PSF that models blurring of single points of light into uniform circles in the image. |
| `M_DISABLE` | Specifies that no PSF should be assumed during super-resolution calculations. |
| `M_GAUSSIAN` *(default)* | Specifies to assume a PSF that models blurring of single points of light into radially symmetric gaussian functions in the image. |
| `M_SQUARE` | Specifies to assume a PSF that models blurring of single points of light into symmetric square functions in the image. |

---

### `M_SR_SMOOTHNESS`

Sets the smoothness value to use during super-resolution calculations.  Use a high smoothness value to reduce local variations in the mosaic image. This is useful to prevent artifacts caused by noise in the source images.  Use a low smoothness value when there is little noise in the source image. This is useful to preserve the most detail in your mosaic.  This control type is only taken into account when [`M_MOSAIC_COMPOSITION`](../../Reference/reg/MregControl.md) is set to [`M_SUPER_RESOLUTION`](../../Reference/reg/MregControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value. |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md).

---

### `M_DRAW_WITH_NO_RESULT`

Sets the type of image to draw when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DRAW_ALBEDO_IMAGE` *(default)* | Specifies to draw the albedo image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.  The albedo image is a composite image derived from multiple image samples, each with a different lighting perspective. The different lighting perspectives help to show changes in reflectance, especially when a change in material occurs, such as when a scratch reveals an underlying material that differs from the surface material. Small defects become more apparent when reflectance changes are revealed using this technique.  This operation is equivalent to performing [`MregCalculate`](../../Reference/reg/MregCalculate.md) followed by [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_ALBEDO_IMAGE`](../../Reference/reg/MregDraw.md), without the need to allocate a result buffer. |
| `M_DRAW_GAUSSIAN_CURVATURE_IMAGE` | Specifies to draw the Gaussian curvature image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.  This operation is equivalent to performing [`MregCalculate`](../../Reference/reg/MregCalculate.md) when [`M_GAUSSIAN_CURVATURE`](../../Reference/reg/MregControl.md) is set to [`M_ENABLE`](../../Reference/reg/MregControl.md)followed by [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md), without the need to allocate a result buffer. |
| `M_DRAW_LOCAL_CONTRAST_IMAGE` | Specifies to draw the local contrast image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.  This operation is equivalent to performing [`MregCalculate`](../../Reference/reg/MregCalculate.md) when [`M_LOCAL_CONTRAST`](../../Reference/reg/MregControl.md) is set to [`M_ENABLE`](../../Reference/reg/MregControl.md) followed by [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_LOCAL_CONTRAST_IMAGE`](../../Reference/reg/MregDraw.md), without the need to allocate a result buffer. |
| `M_DRAW_LOCAL_SHAPE_IMAGE` | Specifies to draw the local shape image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.  This operation is equivalent to performing [`MregCalculate`](../../Reference/reg/MregCalculate.md) when [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) is set to [`M_ENABLE`](../../Reference/reg/MregControl.md) followed by [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md), without the need to allocate a result buffer. |
| `M_DRAW_MEAN_CURVATURE_IMAGE` | Specifies to draw the mean curvature image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.  This operation is equivalent to performing [`MregCalculate`](../../Reference/reg/MregCalculate.md) when [`M_MEAN_CURVATURE`](../../Reference/reg/MregControl.md) is set to [`M_ENABLE`](../../Reference/reg/MregControl.md) followed by [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md), without the need to allocate a result buffer. |
| `M_DRAW_TEXTURE_IMAGE` | Specifies to draw the texture image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.  This operation is equivalent to performing [`MregCalculate`](../../Reference/reg/MregCalculate.md) when [`M_TEXTURE_IMAGE`](../../Reference/reg/MregControl.md) is set to [`M_ENABLE`](../../Reference/reg/MregControl.md) followed by [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_TEXTURE_IMAGE`](../../Reference/reg/MregDraw.md), without the need to allocate a result buffer. |

---

### `M_GAUSSIAN_CURVATURE`

Sets whether to compute the Gaussian curvature image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.  The Gaussian curvature image reveals defects on smooth surfaces, because surface curvature values change abruptly when a defect occurs on such surfaces. This setting is useful when examining a flat printed surface where the printing can obscure defects. Note that this setting is not suitable for finely textured surfaces. Try [`M_MEAN_CURVATURE`](../../Reference/reg/MregControl.md) instead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the Gaussian curvature image. [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregControl.md) becomes unavailable in the draw. |
| `M_ENABLE` | Specifies to compute the Gaussian curvature image. |

---

### `M_IMPROVED_LOCAL_SHAPE`

Sets whether to compute an improved version of the local shape image.  This value is only used when [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) is set to [`M_ENABLE`](../../Reference/reg/MregControl.md); otherwise, it is ignored.  > **Note:** Note that this value cannot be set to [`M_ENABLE`](../../Reference/reg/MregControl.md) unless there are 4 input images, and the azimuth angles of their light sources are 0, 90, 180, and 270, respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to compute the default version of the local shape image. |
| `M_ENABLE` | Specifies to compute an improved version of the local shape image. |

---

### `M_LOCAL_CONTRAST`

Sets whether to compute the local contrast image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.  [`M_LOCAL_CONTRAST`](../../Reference/reg/MregControl.md) reveals surface height variations by increasing the contrast between highlights and shadows. This setting performs morphological operations using [`M_OBJECT_SIZE`](../../Reference/reg/MregControl.md) to control the number of iterations to perform.  A local contrast operation can remove printed text on flat surface areas in the resulting enhanced image, provided source image illumination is even (equal relative intensities across the light sources).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the local contrast image. [`M_DRAW_LOCAL_CONTRAST_IMAGE`](../../Reference/reg/MregControl.md) becomes unavailable in the draw. |
| `M_ENABLE` | Specifies to compute the local contrast image. |

---

### `M_LOCAL_SHAPE`

Sets whether to compute the local shape image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.  A local shape image captures raised and recessed features on a surface that are not apparent with a single image using one light source. Multiple acquisitions using many light sources allow the extraction of structural content not otherwise visible, facilitating further image analysis.  You can control shape smoothness in the resulting image with [`M_SHAPE_SMOOTHNESS`](../../Reference/reg/MregControl.md).  Note that, while each image uses one light source, your lights must be set up in opposite pairs. That is, for each light in the setup, there should be another light positioned 180 degrees opposite.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the local shape image. [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md) becomes unavailable in the draw. |
| `M_ENABLE` | Specifies to compute the local shape image. |

---

### `M_MEAN_CURVATURE`

Sets whether to compute the mean curvature image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.  A mean curvature image can reveal small (local) curvature changes, because the curvature is calculated at every pixel, resulting in more local information. Fine scratches or very small features on a surface are revealed.  Note that the mean curvature operation can substitute for local shape when opposite-paired lighting is not achievable.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the mean curvature image. [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregControl.md) becomes unavailable in the draw. |
| `M_ENABLE` | Specifies to compute the mean curvature image. |

---

### `M_NON_UNIFORMITY_CORRECTION`

Sets whether to compute a correction for non-uniform illumination on the input image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to compute a correction for non-uniform illumination on the input image. The correction is done without reference images, and uses a pixel neighborhood process. It is recommended to use this setting only when the image illumination is severely non-uniform. |
| `M_DISABLE` *(default)* | Specifies not to compute a correction for non-uniform illumination on the input image. |

---

### `M_OBJECT_SIZE`

Sets the number of iterations to perform when computing a local contrast image ([`M_LOCAL_CONTRAST`](../../Reference/reg/MregControl.md) or [`M_DRAW_WITH_NO_RESULT`](../../Reference/reg/MregControl.md) set to [`M_DRAW_LOCAL_CONTRAST_IMAGE`](../../Reference/reg/MregControl.md)).  For the local contrast operation, [`M_OBJECT_SIZE`](../../Reference/reg/MregControl.md) controls the number of iterations of a morphological operation. The number of iterations to perform is related to the feature size of the object(s) in the scene. For larger features, more iterations might be needed. The default is 1 iteration, but it is recommended to experiment to find the best setting for your needs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of iterations of morphological operations to perform.  Note that setting a value of zero is permitted. If your scene is mostly flat, try setting [`M_OBJECT_SIZE`](../../Reference/reg/MregControl.md) to zero. In this case, calculations are performed that do not include morphological operations, resulting in reduced prominence of printed text or imagery on flat surfaces, allowing easier detection of surface defects. |

---

### `M_SHAPE_NORMALIZATION`

Sets whether to perform shape normalization when computing a local shape image ([`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) or [`M_DRAW_WITH_NO_RESULT`](../../Reference/reg/MregControl.md) set to [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregControl.md)).  Shape normalization is performed by default. You can choose to disable shape normalization if the region undergoing photometric stereo registration is mostly flat (perpendicular to the camera). Disabling shape normalization can improve results in such instances, because surface normals from flat regions will be ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to normalize the shape information during local shape computation. |
| `M_ENABLE` *(default)* | Specifies to normalize the shape information during local shape computation. |

---

### `M_SHAPE_SMOOTHNESS`

Sets the amount of smoothness to apply to surface variations when computing the [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md),[`M_MEAN_CURVATURE`](../../Reference/reg/MregControl.md), or[`M_GAUSSIAN_CURVATURE`](../../Reference/reg/MregControl.md) image (or when using [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregControl.md), [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregControl.md), or [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the degree of smoothness applied to frontiers between raised and recessed regions. Large values apply more smoothing, and result in the suppression of smaller features on an object's surface, making the dominant structure more prominent. |

---

### `M_TEXTURE_IMAGE`

Sets whether to compute the texture image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.  This operation targets surface luminance and can remove spectral reflections.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the texture image. [`M_DRAW_TEXTURE_IMAGE`](../../Reference/reg/MregControl.md) becomes unavailable in the draw. |
| `M_ENABLE` | Specifies to compute the texture image. |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration result buffer settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_GENERAL`](../../Reference/reg/MregControl.md).

---

### `M_DRAW_REMAP_FACTOR_MODE`

Sets whether to automatically calculate the remap factor, or to use a user-defined value. The remap factor is the factor needed to remap the result of an [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md), [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md), or [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md) operation when the destination is not a 32-bit floating-point image buffer.  This control type is useful when you are performing several of these operations and want to ensure that the same factor is used to remap the results of the operation from 32-bit floating-point data to the data type of the destination buffer (ensuring that the stored results are directly comparable).  If you specify [`M_USER_DEFINED`](../../Reference/reg/MregControl.md), and set a remap factor using [`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregControl.md), the same remapping will be used to remap the result every time you perform one of the relevant operations. If you specify [`M_AUTO`](../../Reference/reg/MregControl.md) (or [`M_DEFAULT`](../../Reference/reg/MregControl.md)), a different remap factor is calculated each time, based on the maximum and minimum values in the result. With [`M_AUTO`](../../Reference/reg/MregControl.md), the remap factor is automatically defined so that the entire range of the drawn image is represented.  You can retrieve the automatically defined remap factor value using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_RANGE_FACTOR_GAUSSIAN_CURVATURE`](../../Reference/reg/MregGetResult.md), [`M_RANGE_FACTOR_LOCAL_SHAPE`](../../Reference/reg/MregGetResult.md), or [`M_RANGE_FACTOR_MEAN_CURVATURE`](../../Reference/reg/MregGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the remap factor so that the entire range of the drawn image is represented. |
| `M_USER_DEFINED` | Specifies to use the remap factor set with [`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregControl.md). |

---

### `M_DRAW_REMAP_FACTOR_VALUE`

Sets the remap factor value when [`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) is set to [`M_USER_DEFINED`](../../Reference/reg/MregControl.md) and the destination is not a 32-bit floating-point image buffer.  Pixel intensities are multiplied by `((2<sup> _n_ </sup> - 1)/2) * [`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregControl.md)`, then offset by `((2<sup> _n_ </sup> - 1)/2)`; _n_ is the number of bits per pixel.  > **Note:** When [`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md), this value isn't used.  To specify an appropriate remap factor value, it is recommended to obtain the automatically calculated remap factor (using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_RANGE_FACTOR_...`](../../Reference/reg/MregGetResult.md)) for some typical images. If the resulting output is satisfactory, set [`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregControl.md) to the automatically calculated value. If not, you can try setting the remap factor to a higher value; this increases image contrast and helps to bring out smaller details in the resulting photometric stereo image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the remap factor. |

---

### `M_STOP_CALCULATE`

Stops the current photometric stereo calculation operation. You must call [`MregControl`](../../Reference/reg/MregControl.md) with [`M_STOP_CALCULATE`](../../Reference/reg/MregControl.md) from another thread of higher priority. Results already available in the result buffer become invalid and will be discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### For registration elements of an

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control registration elements of an [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration context; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_ALL`](../../Reference/reg/MregControl.md) or a specific value.  > **Note:** Note that you must specify your lights in a counter-clockwise order, starting at the positive X-axis. To illustrate, if you have four lights spaced evenly around a scene, and your first light is placed in line with the X-axis (zero degrees), lights 2, 3, and 4 are placed at 90, 180, and 270 degrees, respectively, and must be specified in the same order.  Note that the origin is located at the center of the plane upon which the object is placed for the photometric stereo registration operation, directly below the camera (assuming the camera is perpendicular to the scene).

---

### `M_LIGHT_VECTOR_COMPONENT_1`

Sets the first component of the light vector for the specified registration element. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md) is set to [`M_SPHERICAL`](../../Reference/reg/MregControl.md), this component is the polar (zenith) angle. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md) is set to [`M_CARTESIAN`](../../Reference/reg/MregControl.md), this component is the vector's X-coordinate in pixel units.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 90.0` | Specifies the polar (zenith) angle, in degrees, if the light mode is set to the spherical coordinate system. The polar angle is defined as the angle between the image's normal vector (the primary axis) and the light vector. The polar angle is measured from the vertical; that is, the primary axis is at zero degrees.  The primary axis is the vector that goes from the camera to the image. It is typical for the camera to be perpendicular to the object's surface for this application.  *[Image: Photometric_Polar_Angle.png]* |
| `Value` | Specifies the X-coordinate of the light vector, if the light mode is set to the Cartesian coordinate system.  Set this value in pixel units, considering the light vector projected onto the XY-plane. |

---

### `M_LIGHT_VECTOR_COMPONENT_2`

Sets the second component of the light vector for the specified registration element. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md) is set to [`M_SPHERICAL`](../../Reference/reg/MregControl.md), this component is the azimuth angle. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md) is set to [`M_CARTESIAN`](../../Reference/reg/MregControl.md), this component is the vector's Y-coordinate in pixel units.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 360.0` | Specifies the azimuth angle, in degrees, if the light mode is set to the spherical coordinate system. The azimuth angle is the angular distance from the positive X-axis to the light vector when projected on the image's XY-plane; the azimuth angle increases counterclockwise.  *[Image: Photometric_Azimuth_Angle.png]* |
| `Value` | Specifies the Y-coordinate of the light vector, if the light mode is set to the Cartesian coordinate system.  Set this value in pixel units, considering the light vector projected onto the XY-plane. |

---

### `M_LIGHT_VECTOR_COMPONENT_3`

Sets the third component of the light vector for the specified registration element.  When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md) is set to [`M_SPHERICAL`](../../Reference/reg/MregControl.md), this component is the relative intensity of the light source. If relative light intensity is set to 1.0 for all light sources, each light provides the same intensity to the setup, which is the ideal arrangement. If equal light intensities are not possible, set the strongest light to 1.0 and set the other light intensity values relative to this.  When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregControl.md) is set to [`M_CARTESIAN`](../../Reference/reg/MregControl.md), this component is the vector's Z-coordinate, specified in a pixel unit equivalent. That is, specify the Z-distance in units equivalent to those used for the X- and Y-coordinates. By default, the Z-axis points downward, so the light source is usually in the negative Z-region of the relative coordinate system. Use an absolute value for this component.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the value of the third component. |

---

### `M_LIGHT_VECTOR_TYPE`

Sets the coordinate system in which the input light vector is represented.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CARTESIAN` | Specifies that the light vector is defined in the Cartesian coordinate system. Coordinates follow the standard Aurora Imaging Library right-hand 3D coordinate system.  The origin is located at the center of the plane upon which the object is placed; that is, directly below the camera. The Z-axis follows the primary axis, along the line from the image's normal vector to the camera. |
| `M_SPHERICAL` *(default)* | Specifies that the light vector is defined in the spherical coordinate system. The components include the polar (zenith) angle, the azimuth angle, and the relative intensity of the light source. |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md).

---

### `M_CIRCLE_OF_CONFUSION_RADIUS_MAX`

Sets the maximum radius of the circle of confusion (blurring circle), among the input images. If this control type is set incorrectly, the EDoF image might not be completely in focus, even if it should be possible given the input images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 256` *(default)* | Specifies the maximum radius of the circle of confusion (blurring circle) in pixels. |

---

### `M_MODE`

Sets the computation mode of the extended depth of field operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST` | Specifies a faster computation mode. Note that using a faster computation mode can decrease robustness of the registration operation. |
| `M_RECONSTRUCTION` *(default)* | Specifies a computation mode that favors the quality of the extended depth of field (EDoF) image. This setting can lead to longer computation times. |

---

### `M_TRANSLATION_TOLERANCE`

Sets the maximum distance between a point of an object that is in focus in one image, and the same point in an image in which the object is out of focus, in pixels. If this control type is set incorrectly, the EDoF image might not be completely in focus, even if it should be possible given the input images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 4` *(default)* | Specifies the maximum translation distance of a point between two images, in pixels. |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md).

---

### `M_ADAPTIVE_INTENSITY_DELTA`

Sets the maximum difference between the intensity values for an object in an image. This value is used when performing the intensity component of an [`M_ADAPTIVE`](../../Reference/reg/MregControl.md) regularization. This determines whether neighboring pixels are considered part of the same object when using [`M_ADAPTIVE`](../../Reference/reg/MregControl.md) regularization mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the maximum difference of intensities. |

---

### `M_ADAPTIVE_SMOOTHING`

Sets which neighborhood pixels are given more weight when performing the smoothing component of the adaptive regularization. This determines how neighborhood pixels are weighted when using an [`M_ADAPTIVE`](../../Reference/reg/MregControl.md) regularization mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies a distance, in pixels, from the pixel being evaluated. Neighborhood pixels that are within this distance from the pixel being evaluated are given more weight. A lower adaptive smoothing value gives more weight to pixels closer to the pixel being evaluated; while a higher adaptive smoothing value equalizes the weight given to pixels in the neighborhood. If the distance is larger than the regularization neighborhood ([`M_REGULARIZATION_SIZE`](../../Reference/reg/MregControl.md)), any pixels outside of the regularization neighborhood will have a weight of 0. |

---

### `M_CONFIDENCE_MAP`

Sets whether the confidence map is computed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the confidence map is not computed. |
| `M_ENABLE` | Specifies that the confidence map is computed. |

---

### `M_FOCUS_DEPTH_SIZE`

Sets the number of images for a portion of the image to go from out of focus to in focus, to out of focus again. A larger number of images indicates a more precise operation, and a longer processing time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 1` *(default)* | Specifies the number of images. |

---

### `M_INTENSITY_MAP`

Sets whether the intensity map is computed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the intensity map is not computed. |
| `M_ENABLE` | Specifies that the intensity map is computed. |

---

### `M_REGULARIZATION_MODE`

Sets the mode of regularization. The mode establishes how to adjust the coherency of neighboring pixels in the index map.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADAPTIVE` | Specifies that the regularization takes into account the local geometry and the change in intensity of the content in the source image. This regularization mode is made up of two components: the smoothing ([`M_ADAPTIVE_SMOOTHING`](../../Reference/reg/MregControl.md)) and the intensity ([`M_ADAPTIVE_INTENSITY_DELTA`](../../Reference/reg/MregControl.md)) components. |
| `M_AVERAGE` *(default)* | Specifies that the regularization takes into account the average dominance of neighboring pixels. |
| `M_DISABLE` | Specifies that the regularization is based on the raw pixel values and that no post-processing is done to ensure the coherency of neighboring pixels. |

---

### `M_REGULARIZATION_SIZE`

Sets the neighborhood size when regularization is used. The neighborhood should be the size of the minimum texture pattern in an object in the image, but no larger than the smallest object dimension in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the size of the neighborhood, in pixels. The size of the neighborhood used is always an odd number. If you specify an even number, the next largest odd number is applied. |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md). Note that these controls apply to all input images.

---

### `M_FUSION_COVERAGE`

Sets the required proportion of non-saturated pixels within each input image that qualifies the image for inclusion in the HDR calculation. Non-saturated pixels are neither underexposed nor overexposed, according to the limits set with [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md). The specified [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md) value is the ratio between the number of qualifying, non-saturated pixels in the input image and the total number of pixels in the image, from 0.0-1.0.  A lower value for this setting is less restrictive because fewer pixels are required per input image. Setting [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md) to a larger value requires more qualifying pixels and is more restrictive.  Note that qualifying pixels are those that are also non-saturated in the working HDR image (that is, those that are in the common merge area).  When [`M_GAIN_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md) (or [`M_DEFAULT`](../../Reference/reg/MregControl.md)), the [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md) ratio determines the number of pixels used to automatically calculate the gain applied to the input image to successfully merge it with the intermediate, working HDR image. If the input image gives a calculated ratio that is below the specified ratio, the addition of this image to the final HDR image fails.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the fusion coverage ratio. |

---

### `M_FUSION_HIGH_THRESHOLD`

Sets the threshold to filter out the saturated pixels (that is, the near maximum values) of the current input image and the working HDR image. The threshold is specified as a percentage of the total number of pixels in the working HDR image. Aurora Imaging Library will internally convert this threshold to the lowest pixel value that allows for at least [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) percent of pixels in the image to be included. All pixels with values above this threshold will be excluded. For example, if [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) is set to 95%, then Aurora Imaging Library will internally select the lowest pixel value that allows for at least 95% of pixels in the image to be included (that is, at most 5% of the highest pixels in the image to be excluded).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the fusion high threshold, as a percentage. |

---

### `M_FUSION_LOW_THRESHOLD`

Sets the threshold to filter out the underexposed pixels (that is, the near 0 values) of the current input image and the working HDR image. The threshold is specified as a percentage of the total number of pixels in the working HDR image. Aurora Imaging Library will internally convert this threshold to the highest pixel value that excludes at most [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) percent of pixels. All pixels with values below this threshold will be excluded. For example, if [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) is set to 5%, then Aurora Imaging Library will internally select the highest pixel value where at most 5% of the lowest pixels in the image would be excluded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the fusion low threshold, as a percentage. |

---

### `M_FUSION_MODE`

Sets the criteria by which input image pixels are selected for contribution to the HDR image calculation. Saturated and underexposed pixels are excluded, and the remaining pixels are then used to calculate the gain to apply to the input image to successfully merge it with the intermediate, working HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_PERCENTILE_VALUE`](../../Reference/reg/MregControl.md) + [`M_IN_RANGE`](../../Reference/reg/MregControl.md). |
| `M_PERCENTILE_VALUE` | Specifies to eliminate from the calculation those pixels whose values exceed or fall short of the limits set with [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md), respectively. |

---

### `M_GAIN_MODE`

Sets how Aurora Imaging Library establishes the image gain to apply to an input image when merging it with the intermediate, working HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the image gains will be automatically estimated by Aurora Imaging Library. |
| `M_USER_DEFINED` | Specifies that you will explicitly set the image gain to use for a given input image, using [`M_IMAGE_GAIN`](../../Reference/reg/MregControl.md). |

---

### `M_TONE_MAPPING_COEFFICIENT`

Sets the tone mapping coefficient when calculating the tone-mapped HDR image.  The specified [`M_TONE_MAPPING_COEFFICIENT`](../../Reference/reg/MregControl.md) affects the pixel intensity distribution during the tone mapping between the HDR image and the end image. A higher value (closer to 1.0) gives a broader intensity range to bright areas, improving bright area detail and lowering the overall brightness of the final image. Conversely, a lower value (closer to 0.0), gives a broader intensity range to dark areas, improving dark area detail and increasing the overall brightness of the final image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the tone mapping coefficient. |

---

### `M_TONE_MAPPING_HIGH_THRESHOLD`

Sets the tone mapping high threshold to use when calculating the tone-mapped HDR image. Specify the threshold as a percentage of the raw HDR image's pixel intensity levels. This clamps unnecessary pixel intensity levels at the saturated end, and allows more intensity distribution elsewhere in the image.  For example, if you set the tone mapping high threshold to 90%, pixels in the raw HDR image with intensities in the brightest 10% are mapped to a constant saturated value in the tone-mapped result.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tone mapping high threshold, as a percentage. |

---

### `M_TONE_MAPPING_LOW_THRESHOLD`

Sets the tone mapping low threshold to use when calculating the tone-mapped HDR image. Specify the threshold as a percentage of the raw HDR image's pixel intensity levels. This clamps unnecessary pixel intensity levels at the near-zero end, and allows more intensity distribution elsewhere in the image.  For example, if you set the tone mapping low threshold to 10%, pixels in the raw HDR image with intensities in the darkest 10% are mapped to a constant low value in the tone-mapped result.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tone mapping low threshold, as a percentage. |

---

### `M_TONE_MAPPING_MODE`

Sets the tone mapping mode to use when calculating the tone-mapped HDR image. Tone mapping distributes pixel intensity values within the specified range. You can also specify to disable tone mapping ([`M_DISABLE`](../../Reference/reg/MregControl.md)), in which case the raw HDR image is drawn into the destination buffer.  Note that, when writing results to a result buffer (using [`MregCalculate`](../../Reference/reg/MregCalculate.md)), the raw HDR image is always available to draw (using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_HDR_FULL_IMAGE`](../../Reference/reg/MregDraw.md)), whether or not tone mapping was applied. Use [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_HDR_IMAGE`](../../Reference/reg/MregDraw.md) to draw the tone-mapped HDR image.  When enabled, the tone mapping result is written to the result buffer or the image buffer passed to [`MregCalculate`](../../Reference/reg/MregCalculate.md) or [`MregDraw`](../../Reference/reg/MregDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_PERCENTILE_VALUE`](../../Reference/reg/MregControl.md) + [`M_IN_RANGE`](../../Reference/reg/MregControl.md). |
| `M_DISABLE` | Specifies not to use a tone mapping mode. |
| `M_PERCENTILE_VALUE` | Specifies to apply tone mapping to input image pixels whose intensity levels fall within the limits set with [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md), respectively.  > **Note:** Tone mapping uses specified percentages to calculate upper and lower limits for the operation. To specify explicit values instead of percent thresholds, use [`MgenLutFunction`](../../Reference/gen/MgenLutFunction.md) with [`M_TONE_MAPPING`](../../Reference/gen/MgenLutFunction.md) to generate the mapping, and then use, for example, [`MimLutMap`](../../Reference/im/MimLutMap.md) to map the raw HDR image. Note that [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) corresponds to [`a`](../../Reference/gen/MgenLutFunction.md), [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) corresponds to [`b`](../../Reference/gen/MgenLutFunction.md), and [`M_TONE_MAPPING_COEFFICIENT`](../../Reference/reg/MregControl.md) corresponds to [`c`](../../Reference/gen/MgenLutFunction.md). |

### For operation settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context settings; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to the index of the input image to control, or [`M_ALL`](../../Reference/reg/MregControl.md).

---

### `M_IMAGE_GAIN`

Sets the image gain that [`MregCalculate`](../../Reference/reg/MregCalculate.md) should use to merge the given input image with the intermediate, working HDR image, when [`M_GAIN_MODE`](../../Reference/reg/MregControl.md) is set to [`M_USER_DEFINED`](../../Reference/reg/MregControl.md).  Note that this setting is ignored when [`M_GAIN_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md) (or [`M_DEFAULT`](../../Reference/reg/MregControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INVALID` | Specifies that the image will be ignored. |
| `Value >= 1.0` *(default)* | Specifies the image gain to be used by [`MregCalculate`](../../Reference/reg/MregCalculate.md). |

### Combination Constants — For specifying a threshold condition

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify a threshold condition.

| Value | Description |
| --- | --- |
| `M_IN_RANGE` | Specifies to apply the fusion or tone mapping to pixels within a specified range. Set the bounds of the range with [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md), or with [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md). |

### For operation timeout settings of

The following [`ControlType`](../../Reference/reg/MregControl.md) and corresponding [`ControlValue`](../../Reference/reg/MregControl.md) parameter settings are used to control the timeout value for an [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration context; in this case, the [`Index`](../../Reference/reg/MregControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregControl.md).

---

### `M_TIMEOUT`

Sets the timeout value for [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 2000.0 msec. |
| `M_DISABLE` | Specifies that there is no timeout value. |
| `Value` | Specifies the timeout value, in msec. |
