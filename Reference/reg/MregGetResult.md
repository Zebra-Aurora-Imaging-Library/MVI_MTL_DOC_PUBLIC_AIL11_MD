---
doctype: Reference
module: reg
function: MregGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregGetResult"
---

# MregGetResult

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

> Get the specified type of result(s) from a registration result buffer.

## Syntax

```c
void MregGetResult(
    AIL_ID    RegResultId,    //in
    AIL_INT   ResultIndex,    //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a registration result buffer.

The result(s) that can be retrieved from an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer are only available after calling [`MregCalculate`](../../Reference/reg/MregCalculate.md) with an [`M_STITCHING`](../../Reference/reg/MregAlloc.md) registration context. Result(s) from an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer can only be retrieved after calling [`MregCalculate`](../../Reference/reg/MregCalculate.md) with an [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md) registration context. Result(s) from an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer can only be retrieved after calling [`MregCalculate`](../../Reference/reg/MregCalculate.md) with an [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md) registration context. Result(s) from an [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer can only be retrieved after calling [`MregCalculate`](../../Reference/reg/MregCalculate.md) with an [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md) registration context. Result(s) from an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer can only be retrieved after calling [`MregCalculate`](../../Reference/reg/MregCalculate.md) with an [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md) registration context.

## Parameters

### `RegResultId` *(in, AIL_ID)*

Specifies the identifier of the registration result buffer from which to retrieve results.

### `ResultIndex` *(in, AIL_INT)*

Specifies which results to get. This parameter can be set to one of the following values:

*For retrieving results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, same as [`M_ALL`](../../Reference/reg/MregGetResult.md).

For an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md), or [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md)result buffer, same as [`M_GENERAL`](../../Reference/reg/MregGetResult.md). |
| `M_ALL` | Specifies to retrieve the results of the specified type for all registration result elements. This setting is only available for [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffers. |
| `M_GENERAL` | Specifies to retrieve a result relating to the entire registration result buffer. |
| `0 <= Value < M_NUMBER_OF_IMAGES` | Specifies the registration result element's index, for an [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer.

> **Note:** Note that the maximum index ([`M_NUMBER_OF_IMAGES`](../../Reference/reg/MregGetResult.md) - 1) is also equal to [`M_HDR_IMAGE_STATUS_SIZE`](../../Reference/reg/MregGetResult.md) - 1. |
| `0 <= Value < M_NUMBER_OF_REGISTRATION_ELEMENTS` | Specifies the registration result element's index, for an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) or [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write the requested information.

## Parameter Associations

### For general results or registration result element results from an

To retrieve a general result or the result for one or all registration result elements of an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the [`ResultType`](../../Reference/reg/MregGetResult.md) parameter can be set to one of the following values.

---

### `M_RESULT`

Retrieves whether the registration calculation ([`MregCalculate`](../../Reference/reg/MregCalculate.md)) was successful.  When using [`M_GENERAL`](../../Reference/reg/MregGetResult.md), and the calculation of one registration result element fails, [`M_RESULT`](../../Reference/reg/MregGetResult.md) returns a fail as well.

| Value | Description |
| --- | --- |
| `M_FAILURE` | Specifies that the registration calculation failed. |
| `M_SUCCESS` | Specifies that the registration calculation was successful. |

---

### `M_SCORE`

Retrieves the score of the registration calculation ([`MregCalculate`](../../Reference/reg/MregCalculate.md)). To determine the type of score to calculate, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_SCORE_TYPE`](../../Reference/reg/MregControl.md).  When using [`M_GENERAL`](../../Reference/reg/MregGetResult.md), the score is an average of the scores of all result elements together.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the score of the registration calculation. The lower the value, the less optimal the alignment. For example, 0.0 equals no alignment and 100.0 equals a perfect alignment. Note that image noise can also adversely affect the score. |

### For general results from an

To retrieve a general result from an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the [`ResultType`](../../Reference/reg/MregGetResult.md) parameter can be set to one of the following values:

---

### `M_MOSAIC_OFFSET_X`

Retrieves the horizontal offset that will exist between the origin of the mosaic's coordinate system and the left side of the destination image buffer, when you compose the mosaic.

---

### `M_MOSAIC_OFFSET_Y`

Retrieves the vertical offset that will exist between the origin of the mosaic's coordinate system and the top of the destination image buffer, when you compose the mosaic.

---

### `M_MOSAIC_SIZE_X`

Retrieves the width that the mosaic will have when it is composed. This value can be used to allocate an image buffer large enough to hold the entire mosaic composed by [`MregTransformImage`](../../Reference/reg/MregTransformImage.md).

---

### `M_MOSAIC_SIZE_Y`

Retrieves the height that the mosaic will have when it is composed. This value can be used to allocate an image buffer large enough to hold the entire mosaic composed by [`MregTransformImage`](../../Reference/reg/MregTransformImage.md).

### For results from one or all registration result elements of an

To retrieve the result from one or all the registration result elements of an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the [`ResultType`](../../Reference/reg/MregGetResult.md) parameter can be set to one of the following values:

---

### `M_POSITION_X`

Retrieves the X-coordinate of the registration result element image's origin in the global/reference coordinate system.

---

### `M_POSITION_Y`

Retrieves the Y-coordinate of the registration result element image's origin in the global/reference coordinate system.

---

### `M_REVERSE_TRANSFORMATION_MATRIX`

Retrieves the 9 elements of the 3x3 transformation matrix that performs a reverse transformation.  The transformation matrix can transform any position in the specified image (`x<sub>s</sub>`, `y<sub>s</sub>`) with a position in the global/reference coordinate system (`x<sub>g</sub>`, `y<sub>g</sub>`).

---

### `M_REVERSE_TRANSFORMATION_MATRIX_ID`

Retrieves the identifier of the Aurora Imaging Library array buffer containing the 3x3 transformation matrix that performs a reverse transformation.  The transformation matrix can transform any position in the specified image (`x<sub>s</sub>`, `y<sub>s</sub>`) with a position in the global/reference coordinate system (`x<sub>g</sub>`, `y<sub>g</sub>`).  Note that you cannot free the retrieved buffer.

---

### `M_TRANSFORMATION_MATRIX`

Retrieves the 9 elements of the 3x3 transformation matrix.  The transformation matrix can transform any position in the global/reference coordinate system (`x<sub>g</sub>`, `y<sub>g</sub>`) with a position in the specified image (`x<sub>s</sub>`, `y<sub>s</sub>`).  You can put the matrix data into an Aurora Imaging Library buffer ([`MbufPut`](../../Reference/buf/MbufPut.md)) and pass the matrix (buffer) to [`MimWarp`](../../Reference/im/MimWarp.md) to transform the source image to the global/reference coordinate system.

---

### `M_TRANSFORMATION_MATRIX_ID`

Retrieves the identifier of the Aurora Imaging Library array buffer containing the 3x3 transformation matrix.  The transformation matrix can transform any position in the global/reference coordinate system (`x<sub>g</sub>`, `y<sub>g</sub>`) with a position in the specified image (`x<sub>s</sub>`, `y<sub>s</sub>`).  You can pass the matrix to [`MimWarp`](../../Reference/im/MimWarp.md) to transform the source image to the global/reference coordinate system.  Note that you cannot free the retrieved buffer.

---

### `M_TRANSFORMED_BL_X`

Retrieves the X-coordinate of the bottom-left corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_BL_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_BR_X`

Retrieves the X-coordinate of the bottom-right corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_BR_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_UL_X`

Retrieves the X-coordinate of the upper left corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_UL_Y`

Retrieves the Y-coordinate of the upper left corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_UR_X`

Retrieves the X-coordinate of the upper right corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

---

### `M_TRANSFORMED_UR_Y`

Retrieves the Y-coordinate of the upper right corner of the specified registration result element's image. This coordinate is with respect to the global/reference coordinate system.

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the coordinate system

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the requested results with respect to the required coordinate system.

| Value | Description |
| --- | --- |
| `M_REFERENCE` | Retrieves the results with respect to the reference coordinate system. The reference coordinate system is associated with the registration element that was specified in the [`Target`](../../Reference/reg/MregSetLocation.md) parameter of [`MregSetLocation`](../../Reference/reg/MregSetLocation.md). |
| `M_REGISTRATION_GLOBAL` *(default)* | Retrieves the results with respect to the global pixel coordinate system. |

### For general results from an

To retrieve a general result from an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md)result buffer, the result type parameter can be set to one of the following values:

---

### `M_GAUSSIAN_CURVATURE`

Retrieves whether the Gaussian curvature image was computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the Gaussian curvature image was not computed and is not available to draw. |
| `M_ENABLE` | Specifies that the Gaussian curvature image was computed and is available to draw using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md). |

---

### `M_LOCAL_CONTRAST`

Retrieves whether the local contrast image was computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the local contrast image was not computed and is not available to draw. |
| `M_ENABLE` | Specifies that the local contrast image was computed and is available to draw using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_LOCAL_CONTRAST_IMAGE`](../../Reference/reg/MregDraw.md). |

---

### `M_LOCAL_SHAPE`

Retrieves whether the local shape image was computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the local shape image was not computed and is not available to draw. |
| `M_ENABLE` | Specifies that the local shape image was computed and is available to draw using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md). |

---

### `M_MEAN_CURVATURE`

Retrieves whether the mean curvature image was computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the mean curvature image was not computed and is not available to draw. |
| `M_ENABLE` | Specifies that the mean curvature image was computed and is available to draw using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md). |

---

### `M_RANGE_FACTOR_GAUSSIAN_CURVATURE`

Retrieves the automatically calculated remap factor value for an [`M_DRAW_GAUSSIAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md) operation, when [`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md). The calculated value is equal to `1 / (max | _Gaussian curvature image_ |)`, where (max |_Gaussian curvature image_|) is the maximum of the absolute value of the pixel intensity values in the Gaussian curvature image.

---

### `M_RANGE_FACTOR_LOCAL_SHAPE`

Retrieves the automatically calculated remap factor value for an [`M_DRAW_LOCAL_SHAPE_IMAGE`](../../Reference/reg/MregDraw.md) operation, when [`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md). The calculated value is equal to `1 / (max | _local shape image_ |)`, where (max |_local shape image_|) is the maximum of the absolute value of the pixel intensity values in the local shape image.

---

### `M_RANGE_FACTOR_MEAN_CURVATURE`

Retrieves the automatically calculated remap factor value for an [`M_DRAW_MEAN_CURVATURE_IMAGE`](../../Reference/reg/MregDraw.md) operation, when [`M_DRAW_REMAP_FACTOR_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md). The calculated value is equal to `1 / (max | _mean curvature image_ |)`, where (max |_mean curvature image_|) is the maximum of the absolute value of the pixel intensity values in the mean curvature image.

---

### `M_TEXTURE_IMAGE`

Retrieves whether the texture image was computed when using [`MregCalculate`](../../Reference/reg/MregCalculate.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the texture image was not computed and is not available to draw. |
| `M_ENABLE` | Specifies that the texture image was computed and is available to draw using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_TEXTURE_IMAGE`](../../Reference/reg/MregDraw.md). |

### For general results from an

To retrieve a general result from an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md), or [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the result type parameter can be set to one of the values listed in the table below.  Note that for an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md), or an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, [`M_STATUS`](../../Reference/reg/MregGetResult.md) must return [`M_COMPLETE`](../../Reference/reg/MregGetResult.md).

---

### `M_IMAGE_SIZE_BAND`

Retrieves the number of bands of the images used as input in the [`MregCalculate`](../../Reference/reg/MregCalculate.md)operation.  This value can be used to allocate an image buffer large enough to hold the image drawn using [`MregDraw`](../../Reference/reg/MregDraw.md).

---

### `M_IMAGE_SIZE_X`

Retrieves the size of the X-dimension of the images used as input in the [`MregCalculate`](../../Reference/reg/MregCalculate.md)operation.  This value can be used to allocate an image buffer large enough to hold the image drawn using [`MregDraw`](../../Reference/reg/MregDraw.md).

---

### `M_IMAGE_SIZE_Y`

Retrieves the size of the Y-dimension of the images used as input in the [`MregCalculate`](../../Reference/reg/MregCalculate.md)operation.  This value can be used to allocate an image buffer large enough to hold the image drawn using [`MregDraw`](../../Reference/reg/MregDraw.md).

---

### `M_IMAGE_TYPE`

Retrieves the data type and depth of the images used as input in the [`MregCalculate`](../../Reference/reg/MregCalculate.md)operation.  This value can be used to allocate an image buffer with the appropriate settings to hold the image drawn using [`MregDraw`](../../Reference/reg/MregDraw.md). In the case of a raw HDR image, its bit depth must be twice that of the input images. For example, if the input images are 8-bit, then the raw HDR image must be written to a 16-bit buffer.

| Value | Description |
| --- | --- |
| `M_FLOAT + 32` | Specifies 32-bit float data. |
| `M_SIGNED + 8` | Specifies 8-bit signed data. |
| `M_SIGNED + 16` | Specifies 16-bit signed data. |
| `M_SIGNED + 32` | Specifies 32-bit signed data. |
| `M_UNSIGNED + 1` | Specifies 1-bit unsigned data. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |
| `M_UNSIGNED + 32` | Specifies 32-bit unsigned data. |

---

### `M_MAX_INDEX_VALUE`

Retrieves the highest index value of the non-null images used as input in the [`MregCalculate`](../../Reference/reg/MregCalculate.md)operation.  This result type is only available for [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md).

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the highest index value. |

### For general results from an

To retrieve a general result from an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the result type parameter can be set to one of the following values:

---

### `M_CIRCLE_OF_CONFUSION_RADIUS_MAX`

Retrieves the radius of the maximum circle of confusion (blurring circle) used during calculations, in pixels.

---

### `M_MODE`

Retrieves the computation mode used.

| Value | Description |
| --- | --- |
| `M_FAST` | Specifies that the extended depth of field registration operation used the fast computation mode. This mode favored reducing computation time over accuracy. |
| `M_RECONSTRUCTION` | Specifies that the extended depth of field registration operation used the reconstruction computation mode. This mode favored accuracy over reducing computation time. |

---

### `M_TRANSLATION_TOLERANCE`

Retrieves the maximum distance, used during calculations, between a point of an object that is in focus in one image, and the same point in an image in which the object is out of focus, in pixels.

### For general results or registration result element results from an

To retrieve a general result or the result of one registration result element from an [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the result type parameter can be set to one of the following values:

---

### `M_HDR_IMAGE_STATUS`

Retrieves the status of the image associated with the specified registration result element.

| Value | Description |
| --- | --- |
| `M_HDR_IMAGE_LIMITED_TO_SINGLE_COLOR` | Specifies that the number of colors in the current HDR image is reduced to a single color within the merge area. |
| `M_HDR_INSUFFICIENT_CONTRAST` | Specifies that there is inadequate contrast in the merge area of the input image. |
| `M_HDR_INSUFFICIENT_CONTRIBUTION` | Specifies that the input image does not increase the dynamic range already present in the current HDR image. |
| `M_HDR_MERGE_AREA_GAIN_INCORRECT` | Specifies that the gain to be applied to the input image is less that 1. |
| `M_HDR_MERGE_AREA_SIZE_INSUFFICIENT` | Specifies that the calculated merge area, where the pixel values are between [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md), on both the working HDR image and the input image, is below the threshold specified with [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md). |
| `M_HDR_SUCCESSFUL` | Specifies that the image was successfully merged into the intermediate, working HDR image. |

---

### `M_HDR_IMAGE_STATUS_SIZE`

Retrieves the number of images that have been processed and that have their status available.

---

### `M_HDR_STATUS`

Retrieves the ratio of images used with success to the total number of input images, when calculating the HDR image. This is a quick check value to evaluate the relative quality of the HDR image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the HDR status ratio. A ratio of 1.0 signifies that every image was used. |

---

### `M_IMAGE_GAIN`

Retrieves the gain used to merge the corresponding input image with the intermediate, working HDR image.  The image gain is always 1.0 for the first input image added.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the corresponding input image was not successfully merged with the intermediate, working HDR image. An image was successfully merged during the HDR operation if [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) is [`M_HDR_SUCCESSFUL`](../../Reference/reg/MregGetResult.md). |
| `Value >= 1.0` | Specifies the gain used for the corresponding input image. |

---

### `M_TONE_MAPPING`

Retrieves the tone mapping mode used for the resulting HDR image.

### Combination Constants — For requesting availability

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### For retrieving results for depth-from-focus, extended depth of field, high dynamic range, or photometric stereo registration operations

To retrieve a general result from an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) or [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, the result type parameter can be set to one of the following values:

---

### `M_NUMBER_OF_IMAGES`

Retrieves the number of non-null images used to calculate the results that are stored in the result buffer.

---

### `M_STATUS`

Retrieves the status of the registration operation.

| Value | Description |
| --- | --- |
| `M_ACCUMULATE` | Specifies that the result buffer has been filled with some preprocessing information, but the EDoF or HDR image is not yet available. The EDoF image can be calculated using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md). The HDR image can be calculated using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md).  This value can only be returned for an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md) or an [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. |
| `M_COMPLETE` | Specifies that the resulting image (EDoF image, photometric stereo image, HDR image, or index image) is available and can be retrieved using [`MregDraw`](../../Reference/reg/MregDraw.md). |
| `M_EMPTY` | Specifies that the result buffer is empty. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected internal error occurred during the registration operation.  This value can only be returned for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. |
| `M_NOT_ENOUGH_MEMORY` | Specifies that this operation was not completed because of a lack of available memory.  This value can only be returned for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. |
| `M_STOPPED_BY_REQUEST` | Specifies that the registration operation was stopped from another thread using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_STOP_CALCULATE`](../../Reference/reg/MregControl.md).  This value can only be returned for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. |
| `M_TIMEOUT_REACHED` | Specifies that the operation took longer than the allowed value, specified by [`MregControl`](../../Reference/reg/MregControl.md) with [`M_TIMEOUT`](../../Reference/reg/MregControl.md), and has stopped before completion.  This value can only be returned for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.
