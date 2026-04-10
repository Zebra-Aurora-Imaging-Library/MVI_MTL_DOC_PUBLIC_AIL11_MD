---
doctype: Reference
module: reg
function: MregInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregInquire"
---

# MregInquire

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

> Inquire about a registration context, registration element, or registration result buffer setting.

## Syntax

```c
AIL_INT MregInquire(
    AIL_ID    ContextOrResultId,  //in
    AIL_INT   Index,              //in
    AIL_INT64 InquireType,        //in
    void *    UserVarPtr          //out
)
```

## Description

This function allows you to inquire about the settings of a registration context itself, one (or all) of the registration elements contained therein (for correlation-stitching, photometric stereo, or high dynamic range registration contexts), or a registration result buffer.

Note that for a registration result buffer, this function only retrieves information about result buffer settings (set using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) or [`MregControl`](../../Reference/reg/MregControl.md)). To retrieve results from the registration result buffer, use [`MregGetResult`](../../Reference/reg/MregGetResult.md).

## Parameters

### `ContextOrResultId` *(in, AIL_ID)*

Specifies the identifier of the registration context or registration result buffer about which to inquire. The registration context must have been previously allocated on the required system using [`MregAlloc`](../../Reference/reg/MregAlloc.md). The registration result buffer must have been previously allocated on the required system using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md).

### `Index` *(in, AIL_INT)*

Specifies that a registration context, an individual registration element (of a correlation-stitching, photometric stereo, or high dynamic range registration context), or a registration result buffer is inquired. Set this parameter to one of the following values:

*For specifying a registration context, registration element, or registration result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If an [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context is specified, this parameter inquires information about registration element 0.

If an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md), [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md), [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md), or [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration context is specified, same as [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

If a registration result buffer is specified, same as [`M_GENERAL`](../../Reference/reg/MregInquire.md). |
| `M_CONTEXT` | Inquires information about a registration context, if one is specified. |
| `M_GENERAL` | Inquires general information about the registration result buffer, if one is specified. |
| `0 < Value < M_NUMBER_OF_REGISTRATION_ELEMENTS` | Inquires information about a registration element, if you specify a correlation-stitching, photometric stereo registration, or high dynamic range registration context. Set the [`Index`](../../Reference/reg/MregInquire.md) parameter to the index of the required registration element.

Note that this parameter is only available for [`M_STITCHING`](../../Reference/reg/MregAlloc.md), [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md), or [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration contexts. |

### `InquireType` *(in, AIL_INT64)*

Specifies the setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MregInquire`](../../Reference/reg/MregInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring general settings of a registration context or result buffer

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify a registration context or result buffer. The [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md) or [`M_GENERAL`](../../Reference/reg/MregInquire.md), for a registration context or result buffer respectively.

---

### `M_NUMBER_OF_REGISTRATION_ELEMENTS`

Inquires the number of registration elements in the registration context or the number of registration result elements in the result buffer.  Note that this inquire is only available for an[`M_STITCHING`](../../Reference/reg/MregAlloc.md), [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) or [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context, or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) or [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `2 <= Value < 8192` | Specifies the number of registration elements for a correlation-stitching registration context. |
| `Value > 3` | Specifies the number of registration elements for a photometric stereo registration context. |
| `Value >= 2` | Specifies the number of registration elements for a high dynamic range registration context. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the specified registration context or registration result buffer was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For inquiring correlation context settings

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context. In this case, the [`Index`](../../Reference/reg/MregInquire.md)parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

---

### `M_ACCURACY`

Inquires the accuracy of the registration calculation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies high accuracy. |
| `M_LOW` | Specifies low accuracy. |

---

### `M_LOCATION_DELTA`

Inquires the maximum displacement that will be applied to any pixel in the image during registration, relative to its initial location.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the maximum displacement as a percentage. |

---

### `M_MIN_OVERLAP`

Inquires the minimum overlap that should exist between an image and its reference image so that the registration calculation can optimize the match in their overlapping region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 100.0` *(default)* | Specifies the minimum overlap as a percentage. |

---

### `M_SCORE_TYPE`

Inquires the type of score calculated during registration. This calculated score can be retrieved after registration using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_SCORE`](../../Reference/reg/MregGetResult.md).

| Value | Description |
| --- | --- |
| `M_CORRELATION` | Specifies to calculate the score based on the normalized grayscale correlation in the overlapped region. |
| `M_NONE` *(default)* | Specifies that no score is calculated; it is set to 100%. |

---

### `M_STITCHING_LAST_LEVEL`

Inquires the resolution level for the final stage (highest level) of the correlation algorithm.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the last level is set automatically based on image size. |
| `0 <= Value <= 16` | Specifies the resolution level for the final stage of the correlation algorithm. |

---

### `M_TRANSFORMATION_TYPE`

Inquires the type of transformation that the registration calculation will use to optimize the match in the images' overlapping regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PERSPECTIVE` | Specifies that a perspective warping can be performed to optimize the match in the overlapping regions. |
| `M_TRANSLATION` *(default)* | Specifies that a translation can be performed to optimize the match in the overlapping regions. |
| `M_TRANSLATION_ROTATION` | Specifies that a translation and a rotation can be performed to optimize the match in the overlapping regions. |
| `M_TRANSLATION_ROTATION_SCALE` | Specifies that a translation, a rotation, and a scale operation can be performed to optimize the match in the overlapping regions. |

### For registration elements of correlation-stitching contexts and correlation-stitching result buffers

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context or an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. In this case the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to a specific element of the registration context or registration result buffer.

---

### `M_OPTIMIZE_LOCATION`

Inquires whether the optimization step of the registration calculation will be performed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to perform the optimization calculation. |
| `M_ENABLE` *(default)* | Specifies to perform the optimization calculation. |

---

### `M_REFERENCE_X`

Inquires the X-coordinate of the origin of the image's pixel coordinate system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default X-coordinate of the origin will be used. |
| `M_CENTER_ELEMENT` | Specifies the X-coordinate of the origin to be at the center of the image. |
| `Value` | Specifies the X-coordinate of the origin, in pixels. |

---

### `M_REFERENCE_Y`

Inquires the Y-coordinate of the origin of the image's pixel coordinate system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the default Y-coordinate of the origin will be used. |
| `M_CENTER_ELEMENT` | Specifies the Y-coordinate of the origin to be at the center of the image. |
| `Value` | Specifies the Y-coordinate of the origin, in pixels. |

---

### `M_SET_LOCATION_PARAM_1`

Inquires the first attribute of the transformation used to set the rough location of the registration element's image. If the first attribute was set to an Aurora Imaging Library identifier, this identifier cannot be returned; [`M_NULL`](../../Reference/reg/MregInquire.md) is returned instead. This is the case when the rough location was set using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with, for example, [`M_COPY_REG_CONTEXT`](../../Reference/reg/MregSetLocation.md) or [`M_COPY_REG_RESULT`](../../Reference/reg/MregSetLocation.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the first attribute was set to an Aurora Imaging Library identifier. |
| `Value` | Specifies the X-coordinate. |

---

### `M_SET_LOCATION_PARAM_2`

Inquires the second attribute of the transformation used to set the rough location of the registration element's image.

| Value | Description |
| --- | --- |
| *(see [`Param2`](Reference/reg/MregSetLocation.md))* |  |
| `Value` | Specifies the Y-coordinate. |

---

### `M_SET_LOCATION_PARAM_3`

Inquires the third attribute of the transformation used to set the rough location of the registration element's image.

| Value | Description |
| --- | --- |
| *(see [`Param3`](Reference/reg/MregSetLocation.md))* |  |

---

### `M_SET_LOCATION_PARAM_TYPE`

Inquires the type of transformation used to set the rough location of the registration element's image.

| Value | Description |
| --- | --- |
| *(see [`ParamType`](Reference/reg/MregSetLocation.md))* |  |

---

### `M_SET_LOCATION_TARGET`

Inquires the reference of the registration element's image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COPY` | Uses the same index as the reference of the element from which positional information is being copied. |
| `M_NEXT` | Specifies the reference to be the image associated with the registration element whose index follows the specified registration element's index. |
| `M_PREVIOUS` *(default)* | Specifies the reference to be the image associated with the registration element whose index precedes the specified registration element's index. |
| `M_REGISTRATION_GLOBAL` | Specifies that the reference is the global pixel coordinate system. |
| `M_UNCHANGED` | Specifies that only the rough locations are changed; the same reference is used. |
| `0 <= Value < Number of source elements` | Specifies the index of the registration element of the reference image. |

### For correlation-stitching result buffers

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_GENERAL`](../../Reference/reg/MregInquire.md).

---

### `M_MOSAIC_COMPOSITION`

Inquires which image's pixel values to use when composing the mosaic and two or more images overlap.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE_IMAGE` | Specifies to use the average value of the images' pixels in the overlapping region. |
| `M_FIRST_IMAGE` | Specifies to use the pixels of the image associated with the registration result element with the lowest index. |
| `M_FUSION_IMAGE` | Specifies to fuse the images by progressively blending overlapping pixels. |
| `M_LAST_IMAGE` *(default)* | Specifies to use the pixels of the image associated with the registration result element with the highest index. |
| `M_SUPER_RESOLUTION` | Specifies to use a super-resolution algorithm to create the mosaic. |

---

### `M_MOSAIC_OFFSET_X`

Inquires the X-offset between the origin of the coordinate system used to compose the mosaic and the left side of the destination image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALIGN_LEFT` *(default)* | Specifies that the X-offset (in pixels) will be calculated such that the left-most part of the mosaic will be aligned with the left side of the destination image buffer. |
| `Value` | Specifies the X-offset, in pixels. |

---

### `M_MOSAIC_OFFSET_Y`

Inquires the Y-offset between the origin of the coordinate system used to compose the mosaic and the top of the destination image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALIGN_TOP` *(default)* | Specifies that the Y-offset (in pixels) will be calculated such that the top-most part of the mosaic will be aligned with the top of the destination image buffer. |
| `Value` | Specifies the Y-offset, in pixels. |

---

### `M_MOSAIC_SCALE`

Inquires the scaling factor used on images while creating the mosaic.

| Value | Description |
| --- | --- |
| `0.0 < Value <= 10.0` *(default)* | Specifies the scale factor. |

---

### `M_MOSAIC_STATIC_INDEX`

Inquires the element whose coordinate system is used when generating the mosaic. If the coordinate system of one of the images is used, the index of its corresponding registration element is returned. If the coordinate system which minimizes changes to the images or the global pixel coordinate system is used, [`M_ALL`](../../Reference/reg/MregControl.md) or [`M_REGISTRATION_GLOBAL`](../../Reference/reg/MregControl.md) will be returned, respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_ALL` | Specifies that the coordinate system will be chosen such that the minimum change is done on all images during the mosaic composition. |
| `M_REGISTRATION_GLOBAL` | Specifies that the mosaic will be composed with respect to the global pixel coordinate system. |
| `0 <= Value < NumberOfElements` | Specifies the index of the registration result element whose image's pixel coordinate system will be used as the reference coordinate system. |

---

### `M_SR_PSF_RADIUS`

Inquires the radius of the [`M_CIRCULAR`](../../Reference/reg/MregControl.md) or [`M_GAUSSIAN`](../../Reference/reg/MregControl.md) point-spread functions (PSF). For [`M_GAUSSIAN`](../../Reference/reg/MregControl.md), the radius corresponds to the standard deviation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the radius of the PSF, in pixel units at the scale of the source images. |

---

### `M_SR_PSF_TYPE`

Inquires the type of point-spread function (PSF) to use during super-resolution calculations to model the blurring in source images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CIRCULAR` | Specifies to assume a PSF that models blurring of single points of light into uniform circles in the image. |
| `M_DISABLE` | Specifies that no PSF should be assumed during super-resolution calculations. |
| `M_GAUSSIAN` *(default)* | Specifies to assume a PSF that models blurring of single points of light into radially symmetric gaussian functions in the image. |
| `M_SQUARE` | Specifies to assume a PSF that models blurring of single points of light into symmetric square functions in the image. |

---

### `M_SR_SMOOTHNESS`

Inquires the smoothness value used during super-resolution calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value. |

### For operation settings of

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

---

### `M_DRAW_WITH_NO_RESULT`

Inquires the type of image to draw when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DRAW_ALBEDO_IMAGE` *(default)* | Specifies to draw the albedo image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer. |
| `M_DRAW_GAUSSIAN_CURVATURE_IMAGE` | Specifies to draw the Gaussian curvature image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer. |
| `M_DRAW_LOCAL_CONTRAST_IMAGE` | Specifies to draw the local contrast image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer. |
| `M_DRAW_LOCAL_SHAPE_IMAGE` | Specifies to draw the local shape image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer. |
| `M_DRAW_MEAN_CURVATURE_IMAGE` | Specifies to draw the mean curvature image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer. |
| `M_DRAW_TEXTURE_IMAGE` | Specifies to draw the texture image into the image buffer when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to an image buffer. |

---

### `M_GAUSSIAN_CURVATURE`

Inquires whether the Gaussian curvature image is computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the Gaussian curvature image. |
| `M_ENABLE` | Specifies to compute the Gaussian curvature image. |

---

### `M_IMPROVED_LOCAL_SHAPE`

Inquires the mode used to compute the local shape image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to compute the default version of the local shape image. |
| `M_ENABLE` | Specifies to compute an improved version of the local shape image. |

---

### `M_LOCAL_CONTRAST`

Inquires whether to compute the local contrast image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the local contrast image. |
| `M_ENABLE` | Specifies to compute the local contrast image. |

---

### `M_LOCAL_SHAPE`

Inquires whether to compute the local shape image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the local shape image. |
| `M_ENABLE` | Specifies to compute the local shape image. |

---

### `M_MEAN_CURVATURE`

Inquires whether the mean curvature image is computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the mean curvature image. |
| `M_ENABLE` | Specifies to compute the mean curvature image. |

---

### `M_NON_UNIFORMITY_CORRECTION`

Inquires whether to compute a correction for non-uniform illumination on the input image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to compute a correction for non-uniform illumination on the input image. |
| `M_DISABLE` *(default)* | Specifies not to compute a correction for non-uniform illumination on the input image. |

---

### `M_OBJECT_SIZE`

Inquires the number of iterations to perform when [`M_LOCAL_CONTRAST`](../../Reference/reg/MregControl.md) or [`M_DRAW_LOCAL_CONTRAST_IMAGE`](../../Reference/reg/MregDraw.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the number of iterations of morphological operations to perform. |

---

### `M_SHAPE_NORMALIZATION`

Inquires whether shape normalization is performed when computing a local shape image ([`M_LOCAL_SHAPE`](../../Reference/reg/MregInquire.md) or [`M_DRAW_WITH_NO_RESULT`](../../Reference/reg/MregInquire.md) set to [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to normalize the shape information during local shape computation. |
| `M_ENABLE` *(default)* | Specifies to normalize the shape information during local shape computation. |

---

### `M_SHAPE_SMOOTHNESS`

Inquires the amount of smoothness to apply to surface variations when computing the [`M_LOCAL_SHAPE`](../../Reference/reg/MregInquire.md),[`M_MEAN_CURVATURE`](../../Reference/reg/MregInquire.md), or [`M_GAUSSIAN_CURVATURE`](../../Reference/reg/MregInquire.md) image (or when using [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregInquire.md), [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregInquire.md), or [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the degree of smoothness applied to frontiers between raised and recessed regions. |

---

### `M_TEXTURE_IMAGE`

Inquires whether to compute the texture image when using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`RegResultOrImageId`](../../Reference/reg/MregCalculate.md) set to a result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to compute the texture image. |
| `M_ENABLE` | Specifies to compute the texture image. |

### For operation settings of

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_GENERAL`](../../Reference/reg/MregInquire.md).

---

### `M_DRAW_REMAP_FACTOR_MODE`

Inquires whether to automatically calculate the remap factor, or to use a user-defined value. The remap factor is the factor needed to remap the result of an [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md), [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md), or [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md) operation when the destination is not a 32-bit floating-point image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically calculate the remap factor so that the entire range of the drawn image is represented. |
| `M_USER_DEFINED` | Specifies to use the remap factor set with [`M_DRAW_REMAP_FACTOR_VALUE`](../../Reference/reg/MregInquire.md). |

---

### `M_DRAW_REMAP_FACTOR_VALUE`

Inquires the remap factor value when [`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregInquire.md) is set to [`M_USER_DEFINED`](../../Reference/reg/MregInquire.md) and the destination is not a 32-bit floating-point image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the remap factor. |

### For registration settings of an

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md) or a specific value.

---

### `M_LIGHT_VECTOR_COMPONENT_1`

Inquires the first component of the light vector. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregInquire.md) is set to [`M_SPHERICAL`](../../Reference/reg/MregInquire.md), this component is the polar (zenith) angle. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregInquire.md) is set to [`M_CARTESIAN`](../../Reference/reg/MregInquire.md), this component is the vector's X-coordinate.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 90.0` | Specifies the polar (zenith) angle, in degrees, if the light mode is set to the spherical coordinate system. |
| `Value` | Specifies the X-coordinate of the light vector, if the light mode is set to the Cartesian coordinate system. |

---

### `M_LIGHT_VECTOR_COMPONENT_2`

Inquires the second component of the light vector.  When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregInquire.md) is set to [`M_SPHERICAL`](../../Reference/reg/MregInquire.md), this component is the azimuth angle. When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregInquire.md) is set to [`M_CARTESIAN`](../../Reference/reg/MregInquire.md), this component is the vector's Y-coordinate.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 360.0` | Specifies the azimuth angle, in degrees, if the light mode is set to the spherical coordinate system. |
| `Value` | Specifies the Y-coordinate of the light vector, if the light mode is set to the Cartesian coordinate system. |

---

### `M_LIGHT_VECTOR_COMPONENT_3`

Inquires the third component of the light vector.  When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregInquire.md) is set to [`M_SPHERICAL`](../../Reference/reg/MregInquire.md), this component is the relative intensity of the light source.  When [`M_LIGHT_VECTOR_TYPE`](../../Reference/reg/MregInquire.md) is set to [`M_CARTESIAN`](../../Reference/reg/MregInquire.md), this component is the vector's Z-coordinate.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the value of the third component. |

---

### `M_LIGHT_VECTOR_TYPE`

Inquires the coordinate system in which the input light vector is represented.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CARTESIAN` | Specifies that the light vector is defined in the Cartesian coordinate system. |
| `M_SPHERICAL` *(default)* | Specifies that the light vector is defined in the spherical coordinate system. |

### For

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) registration context. In this case, the [`Index`](../../Reference/reg/MregInquire.md)parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

---

### `M_CIRCLE_OF_CONFUSION_RADIUS_MAX`

Inquires the radius of the maximum circle of confusion (blurring circle), among the input images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 256` *(default)* | Specifies the maximum radius of the circle of confusion (blurring circle) in pixels. |

---

### `M_MODE`

Inquires the computation mode of the registration operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST` | Specifies a faster computation mode. |
| `M_RECONSTRUCTION` *(default)* | Specifies a computation mode that favors the quality of the extended depth of field (EDoF) image. |

---

### `M_TRANSLATION_TOLERANCE`

Inquires the maximum distance between a point of an object that is in focus in one image, and the same point in an image in which the object is out of focus, in pixels, among the input images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 4` *(default)* | Specifies the maximum translation distance of a point between two images, in pixels. |

### For

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md) registration context. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

---

### `M_ADAPTIVE_INTENSITY_DELTA`

Inquires the adaptive intensity delta. The adaptive intensity delta is the difference between the maximum and minimum intensity values for an object in an image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the maximum difference of intensities. |

---

### `M_ADAPTIVE_SMOOTHING`

Inquires the size of the neighborhood on which to apply the adaptive smoothing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies a distance, in pixels, from the pixel being evaluated. |

---

### `M_CONFIDENCE_MAP`

Inquires whether the confidence map is computed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the confidence map is not computed. |
| `M_ENABLE` | Specifies that the confidence map is computed. |

---

### `M_FOCUS_DEPTH_SIZE`

Inquires the focus depth size. The focus depth size is the number of images for an object in the image to go from out of focus, to in focus, to out of focus again.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 1` *(default)* | Specifies the number of images. |

---

### `M_INTENSITY_MAP`

Inquires whether the intensity map is computed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the intensity map is not computed. |
| `M_ENABLE` | Specifies that the intensity map is computed. |

---

### `M_REGULARIZATION_MODE`

Inquires the mode of regularization used to adjust the coherency of neighboring pixels in the index map.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ADAPTIVE` | Specifies that the regularization takes into account the local geometry and the change in intensity of the content in the source image. |
| `M_AVERAGE` *(default)* | Specifies that the regularization takes into account the average dominance of neighboring pixels. |
| `M_DISABLE` | Specifies that the regularization is based on the raw pixel values and that no post-processing is done to ensure the coherency of neighboring pixels. |

---

### `M_REGULARIZATION_SIZE`

Inquires the neighborhood size when regulation is used. This should typically not be larger than the smallest object's dimension in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 3` *(default)* | Specifies the size of the neighborhood, in pixels. |

### For inquiring about the operation settings of an

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

---

### `M_FUSION_COVERAGE`

Inquires the required proportion of non-saturated pixels within each input image that qualifies the image for inclusion in the HDR calculation. Non-saturated pixels are neither underexposed nor overexposed, according to the limits set with [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregInquire.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregInquire.md). The specified [`M_FUSION_COVERAGE`](../../Reference/reg/MregInquire.md) value is the ratio between the number of qualifying, non-saturated pixels in the input image and the total number of pixels in the image, from 0.0-1.0.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the fusion coverage ratio. |

---

### `M_FUSION_HIGH_THRESHOLD`

Inquires the threshold to filter out the saturated pixels (that is, the near maximum values) of the image. Specify the threshold as a percentage of the current working range of pixel values, which widens with each input image that is merged into the intermediate, working HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the fusion high threshold, as a percentage. |

---

### `M_FUSION_LOW_THRESHOLD`

Inquires the threshold to filter out the underexposed pixels (that is, the near 0 values) of the image. Specify the threshold as a percentage of the current working range of pixel values, which widens with each input image that is merged into the intermediate, working HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the fusion low threshold, as a percentage. |

---

### `M_FUSION_MODE`

Inquires the criteria by which input image pixels are selected for contribution to the HDR image calculation. Saturated and underexposed pixels are excluded, and the remaining pixels are then used to calculate the gain to apply to the input image to successfully merge it with the intermediate, working HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_PERCENTILE_VALUE`](../../Reference/reg/MregInquire.md) + [`M_IN_RANGE`](../../Reference/reg/MregInquire.md). |
| `M_PERCENTILE_VALUE` | Specifies to eliminate from the calculation those pixels whose values exceed or fall short of the limits set with [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregInquire.md) and [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregInquire.md), respectively. |

---

### `M_GAIN_MODE`

Inquires how Aurora Imaging Library establishes the image gain to apply to an input image when merging it with the intermediate, working HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the image gains will be automatically estimated by Aurora Imaging Library. |
| `M_USER_DEFINED` | Specifies that you will explicitly set the image gain to use for a given input image, using [`M_IMAGE_GAIN`](../../Reference/reg/MregInquire.md). |

---

### `M_TONE_MAPPING_COEFFICIENT`

Inquires the tone mapping coefficient when calculating the tone-mapped HDR image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 1.0` *(default)* | Specifies the tone mapping coefficient. |

---

### `M_TONE_MAPPING_HIGH_THRESHOLD`

Inquires the tone mapping high threshold to use when calculating the tone-mapped HDR image. Specify the threshold as a percentage of the raw HDR image's pixel intensity levels. This removes unnecessary pixel intensity levels at the saturated end, and allows more intensity distribution elsewhere in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tone mapping high threshold, as a percentage. |

---

### `M_TONE_MAPPING_LOW_THRESHOLD`

Inquires the tone mapping low threshold to use when calculating the tone-mapped HDR image. Specify the threshold as a percentage of the raw HDR image's pixel intensity levels. This removes unnecessary pixel intensity levels at the near-zero end, and allows more intensity distribution elsewhere in the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the tone mapping low threshold, as a percentage. |

---

### `M_TONE_MAPPING_MODE`

Inquires the tone mapping mode to use when calculating the tone-mapped HDR image. Tone mapping distributes pixel intensity values within the specified range. You can also specify to disable tone mapping ([`M_DISABLE`](../../Reference/reg/MregInquire.md)), in which case the raw HDR image is drawn into the destination buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`M_PERCENTILE_VALUE`](../../Reference/reg/MregInquire.md) + [`M_IN_RANGE`](../../Reference/reg/MregInquire.md). |
| `M_DISABLE` | Specifies not to use a tone mapping mode. |
| `M_PERCENTILE_VALUE` | Specifies to apply tone mapping to input image pixels whose intensity levels fall within the limits set with [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregInquire.md) and [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregInquire.md), respectively. |

### For operation settings of

For the following inquire types, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context. In this case, the [`Index`](../../Reference/reg/MregInquire.md) parameter must be set to the index of the input image to inquire.

---

### `M_IMAGE_GAIN`

Inquires the image gain that [`MregCalculate`](../../Reference/reg/MregCalculate.md) should use to merge the given input image with the intermediate, working HDR image, when [`M_GAIN_MODE`](../../Reference/reg/MregInquire.md) is set to [`M_USER_DEFINED`](../../Reference/reg/MregInquire.md).

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
| `M_IN_RANGE` | Specifies to apply the fusion or tone mapping to pixels within a specified range. |

### For inquiring the timeout value for

For the following inquire type, the [`ContextOrResultId`](../../Reference/reg/MregInquire.md) parameter must specify an [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration context. In this case, the [`Index`](../../Reference/reg/MregInquire.md)parameter must be set to [`M_CONTEXT`](../../Reference/reg/MregInquire.md).

---

### `M_TIMEOUT`

Inquires the timeout value for [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 2000.0 msec. |
| `M_DISABLE` | Specifies that there is no timeout value. |
| `Value` | Specifies the timeout value, in msec. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/reg/MregInquire.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/edge/MedgeInquire.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
