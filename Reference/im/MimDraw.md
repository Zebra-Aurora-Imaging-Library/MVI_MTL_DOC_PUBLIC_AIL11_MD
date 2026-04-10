---
doctype: Reference
module: im
function: MimDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimDraw"
---

# MimDraw

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

> Draw the settings of an image processing context or the results of an image processing operation.

## Syntax

```c
void MimDraw(
    AIL_ID     ContextGraId,            //in
    AIL_ID     Src1AilId,               //in
    AIL_ID     Src2AilId,               //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_INT64  Operation,               //in
    AIL_DOUBLE Param1,                  //in
    AIL_DOUBLE Param2,                  //in
    AIL_INT64  ControlFlag              //in
)
```

## Description

The function draws the settings of an image processing context or the results of an Aurora Imaging Library image processing operation, into a destination image buffer.

> **Note:** You can also use [`MimDraw`](../../Reference/im/MimDraw.md) to draw the internal copy of a source image that was passed to [`MimControl`](../../Reference/im/MimControl.md) or an image processing function and saved in an image processing context or result buffer. Note that the drawn image might differ from the original source image (for example, in bit depth). This is because, for greater optimization, an image processing context or result buffer might store its internal image data in a different format than the original source image buffer.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use. This parameter must be set to one of the following:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `Src1AilId` *(in, AIL_ID)*

Specifies the identifier of the primary source image buffer, result buffer, or image processing context from which to extract the results to draw. The buffer must have been previously allocated on the required system, using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md), [`MimAllocResult`](../../Reference/im/MimAllocResult.md), or [`MimAlloc`](../../Reference/im/MimAlloc.md), respectively.

### `Src2AilId` *(in, AIL_ID)*

Specifies the identifier of the secondary source image buffer for the drawing operation. The buffer must have been previously allocated on the required system using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid Aurora Imaging Library image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). However, when the source buffers are image buffers, the destination image buffer should be greater than or equal in size and bit depth to the source image buffers. If the destination image buffer is smaller than the source image buffers, the excess will not be part of the destination image.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the operation to perform.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Specifies drawing constraints. You must set this parameter to [`M_DEFAULT`](../../Reference/im/MimDraw.md) unless you are drawing [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) or [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results.

*For specifying the default drawing behavior*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. Except when drawing [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) or [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results, Aurora Imaging Library ignores this parameter.

When drawing [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) results, the default is [`M_FIXED_POINT + n`](../../Reference/im/MimDraw.md), where _n_ = 0.

When drawing [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results, the default is [`M_AUTO_SCALE`](../../Reference/im/MimDraw.md). |

*For specifying the number of fractional bits when using*

| Value | Description |
| --- | --- |
| `M_FIXED_POINT + n` | Specifies the number of fractional bits in the source values, when they are in a fixed-point format. Set _n_ to an _integer_ between 0 and 7, inclusive. |

*For specifying how to manage the range of pixel values when using*

| Value | Description |
| --- | --- |
| `M_AUTO_SCALE` | Remaps pixel values ([`Src1AilId`](../../Reference/im/MimDraw.md)) according to the range of pixel values allowed in the destination ([`DstImageBufOrListGraId`](../../Reference/im/MimDraw.md)). |
| `M_SATURATION` | Clips pixel values ([`Src1AilId`](../../Reference/im/MimDraw.md)) that are outside the range of pixel values allowed in the destination ([`DstImageBufOrListGraId`](../../Reference/im/MimDraw.md)). |

## Parameter Associations

### For specifying to perform the draw operation with the specified image processing context

In the following table, [`Src1AilId`](../../Reference/im/MimDraw.md) specifies an image processing context and [`Operation`](../../Reference/im/MimDraw.md) specifies the operation to perform with the specified image processing context. [`Src2AilId`](../../Reference/im/MimDraw.md) is not required and must be set to [`M_NULL`](../../Reference/im/MimDraw.md). Set [`Param1`](../../Reference/im/MimDraw.md) and [`Param2`](../../Reference/im/MimDraw.md) to 0, unless otherwise specified.

---

### `Dead pixel correction image processing context ID`

Specifies a dead pixel correction image processing context used in [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md) operations, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_DEAD_PIXEL_CONTEXT`](../../Reference/im/MimAlloc.md).

#### `M_DRAW_DEAD_PIXELS`

Draws the dead pixels image from the dead pixel correction image processing context.

---

### `Flat-field image processing context ID`

Specifies an image processing context used in [`MimFlatField`](../../Reference/im/MimFlatField.md) operations, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FLAT_FIELD_CONTEXT`](../../Reference/im/MimAlloc.md).

#### `M_DRAW_DARK_IMAGE`

Draws the dark image from the flat-field image processing context. The dark image represents the thermal agitation of the camera's CCD.

#### `M_DRAW_FLAT_IMAGE`

Draws the flat image from the flat-field context. The flat image represents the variation in sensitivity of each element of the camera's CCD.

#### `M_DRAW_OFFSET_IMAGE`

Draws the offset image from the flat-field context. The offset image represents the electrical bias of the camera's CCD.

---

### `Match image processing context ID`

Specifies an image processing context used in [`MimMatch`](../../Reference/im/MimMatch.md) operations, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_MATCH_CONTEXT`](../../Reference/im/MimAlloc.md).

#### `M_DRAW_MASK`

Draws the mask image from the match image processing context. All non-zero values in the image are considered mask pixels; these pixels will mask out corresponding pixels in the model image when [`MimMatch`](../../Reference/im/MimMatch.md) performs the match.

#### `M_DRAW_MODEL`

Draws the model image from the match context. [`MimMatch`](../../Reference/im/MimMatch.md) compares its source image against this model image.

---

### `Unwarp along path image processing context ID`

Specifies an image processing context used in [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) operations, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md).

#### `M_DRAW_WORK_GRID_VERTICAL_LINES`

Draws the vertical lines used to generate the source grid corresponding to destination pixels. You can retrieve the X- and Y-coordinates of the top and bottom of the vertical working grid lines using [`MimGet`](../../Reference/im/MimGet.md) with [`M_XY_WORK_TOP_GRID_VERTICAL_LINES`](../../Reference/im/MimGet.md) and [`M_XY_WORK_BOTTOM_GRID_VERTICAL_LINES`](../../Reference/im/MimGet.md), respectively.

#### `M_DRAW_WORK_PATH`

Draws the working path used to generate the source grid corresponding to destination pixels. You can retrieve the X- and Y-coordinates of the working path using [`MimGet`](../../Reference/im/MimGet.md) with [`M_XY_WORK_PATH`](../../Reference/im/MimGet.md).

#### `M_DRAW_WORK_QUADRILATERALS`

Draws the quadrilaterals used to generate the source grid corresponding to destination pixels. You can retrieve the X- and Y-coordinates of the top and bottom working paths using [`MimGet`](../../Reference/im/MimGet.md) with [`M_XY_WORK_TOP_PATH`](../../Reference/im/MimGet.md) and [`M_XY_WORK_BOTTOM_PATH`](../../Reference/im/MimGet.md), respectively.

### For specifying a draw operation that uses image processing results

In the following table, [`Src1AilId`](../../Reference/im/MimDraw.md) specifies a result buffer and [`Operation`](../../Reference/im/MimDraw.md) specifies the operation to perform with the specified result buffer. [`Src2AilId`](../../Reference/im/MimDraw.md) is not required and must be set to [`M_NULL`](../../Reference/im/MimDraw.md). Set [`Param1`](../../Reference/im/MimDraw.md) and [`Param2`](../../Reference/im/MimDraw.md) to 0, unless otherwise specified.

---

### `Augmentation result buffer ID`

Specifies an augmentation result buffer, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with[`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md), and used in [`MimAugment`](../../Reference/im/MimAugment.md) operations.  The [`DstImageBufOrListGraId`](../../Reference/im/MimDraw.md) parameter must specify an image buffer that can be processed ([`MbufAlloc...`](../../Reference/buf/MbufAllocColor.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_PROC`](../../Reference/buf/MbufAllocColor.md)). It must have the same number of bands and be the same type as the source used with [`MimAugment`](../../Reference/im/MimAugment.md). To specify an image buffer with an optimal size, so it can hold the results of all possible augmentations, call [`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_AUG_OPTIMAL_SIZE_X`](../../Reference/im/MimGetResult.md) and [`M_AUG_OPTIMAL_SIZE_Y`](../../Reference/im/MimGetResult.md).

#### `M_DRAW_AUG_IMAGE`

Draws the resulting image of the augmentation.

---

### `Find orientation result buffer ID`

Specifies a find orientation image processing result buffer identifier, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md).  The result buffer must contain the dominant orientations (angle values found using [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md)) of the image and the orientation scores (between 0 and 100).

#### `M_DRAW_IMAGE_ORIENTATION`

Draws arrows to represent the found image orientations (viewing angles) calculated by [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md). The center of each arrow will be at the center of the destination image buffer. The arrow's length is proportional to its score and the available space in the destination buffer ([`DstImageBufOrListGraId`](../../Reference/im/MimDraw.md)). With multiple calls to [`MimDraw`](../../Reference/im/MimDraw.md), you can set each arrow to have a different color, dictated by the 2D graphics context specified ([`ContextGraId`](../../Reference/im/MimDraw.md)). Depending on your requirements, you would either change the foreground color associated with the 2D graphics context or allocate multiple 2D graphics context each with a different foreground color. This can be useful for debugging your application. This operation can draw in a 2D graphics list.  *[Image: FindOrientationDrawRef.png]*

---

### `Statistics result buffer ID`

Specifies a result buffer used to store [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) results, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md).

#### `M_DRAW_GLCM_MATRIX`

Draws the internally generated co-occurrence matrix. Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation.  Note that only co-occurrence matrix statistics calculated using the entire source image can be drawn. Using results from a co-occurrence matrix window will generate an error.

#### `M_DRAW_STAT_RESULT`

Draws the specified statistical results of the image processing result buffer. Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation.

| Value | Description |
| --- | --- |
| `M_STAT_GLCM_CONTRAST` | Draws the results of the co-occurrence contrast matrix statistic.  Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation. Only co-occurrence matrix statistics calculated using the entire source image can be drawn. If results from a co-occurrence matrix window, an error occurs. |
| `M_STAT_GLCM_CORRELATION` | Draws the results of the co-occurrence correlation matrix statistic.  Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation. Only co-occurrence matrix statistics calculated using the entire source image can be drawn. If results from a co-occurrence matrix window, an error occurs. |
| `M_STAT_GLCM_DISSIMILARITY` | Draws the results of the co-occurrence dissimilarity matrix statistic.  Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation. Only co-occurrence matrix statistics calculated using the entire source image can be drawn. If results from a co-occurrence matrix window, an error occurs. |
| `M_STAT_GLCM_ENERGY` | Draws the results of the co-occurrence energy matrix statistic.  Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation. Only co-occurrence matrix statistics calculated using the entire source image can be drawn. If results from a co-occurrence matrix window, an error occurs. |
| `M_STAT_GLCM_ENTROPY` | Draws the results of the co-occurrence entropy matrix statistic.  Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation. Only co-occurrence matrix statistics calculated using the entire source image can be drawn. If results from a co-occurrence matrix window, an error occurs. |
| `M_STAT_GLCM_HOMOGENEITY` | Draws the results of the co-occurrence homogeneity matrix statistic.  Note that the results are saturated to the maximum possible value for the specified destination buffer of the draw operation. The resulting co-occurrence matrix is resized to fit into the provided buffer, using closest neighbor interpolation. Only co-occurrence matrix statistics calculated using the entire source image can be drawn. If results from a co-occurrence matrix window, an error occurs. |
| `M_STAT_MAX` | Draws the results of the maximum pixel operation. |
| `M_STAT_MAX_ABS` | Draws the results of the maximum absolute pixel operation. |
| `M_STAT_MEAN` | Draws the results of the mean value of the pixels operation. |
| `M_STAT_MIN` | Draws the results of the minimum pixel operation. |
| `M_STAT_MIN_ABS` | Draws the results of the minimum absolute pixel operation. |
| `M_STAT_STANDARD_DEVIATION` | Draws the results of the standard deviation operation. |
| `M_STAT_SUM` | Draws the results of the sum of the pixel operation. |
| `M_STAT_SUM_ABS` | Draws the results of the sum of the absolute pixel operation. |
| `M_STAT_SUM_OF_SQUARES` | Draws the results of the sum of the squared pixel operation. |

---

### `Wavelet transformation result buffer ID`

Specifies a result buffer used to store [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results, allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md).  The [`DstImageBufOrListGraId`](../../Reference/im/MimDraw.md) parameter must specify an image buffer that can be processed ([`MbufAlloc...`](../../Reference/buf/MbufAllocColor.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_PROC`](../../Reference/buf/MbufAllocColor.md)). It must have the same number of bands and be the same type as the source used with [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md).  For dyadic modes ([`MimControl`](../../Reference/im/MimControl.md)with [`M_TRANSFORMATION_MODE`](../../Reference/im/MimControl.md) set to [`M_DYADIC`](../../Reference/im/MimControl.md)), drawings are in the top-right (vertical coefficient), bottom-right (diagonal coefficient), and bottom-left (horizontal coefficient) corners of the display. This drawing pattern repeats for each level calculated ([`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_NUMBER_OF_LEVELS`](../../Reference/im/MimGetResult.md)). Since dyadic transformations sample wavelet coefficients by a factor of 2 per level, drawings are resized at each level. Aurora Imaging Library also draws the approximation (the low frequency rendition) of the wavelet transformation at the last level, in the top-left corner of the display.  For undecimated modes ([`M_UNDECIMATED`](../../Reference/im/MimControl.md)), drawings are in one row, per level. Each row is split into three columns, representing the horizontal (left column), diagonal (middle column), and vertical (right column) wavelet coefficients for that level. Since undecimated transformations are not sampled, drawings are all the same size, regardless of level. Aurora Imaging Library also draws the approximation (the low frequency rendition) of the wavelet transformation at the last level, in the first column of the first row. The middle and right columns in this row are blank.

#### `M_DRAW_WAVELET`

Draws the resulting image of the wavelet transformation.  During the wavelet transformation, calculations can require Aurora Imaging Library to internally add padding data to the image's border. [`M_DRAW_WAVELET`](../../Reference/im/MimDraw.md) does not draw this padding with the resulting image.  To retrieve the image size required to perform this operation, use [`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_WAVELET_DRAW_SIZE_X`](../../Reference/im/MimGetResult.md) and [`M_WAVELET_DRAW_SIZE_Y`](../../Reference/im/MimGetResult.md).

#### `M_DRAW_WAVELET_WITH_PADDING`

Draws the resulting image of the wavelet transformation, with padding.  During the wavelet transformation, calculations can require Aurora Imaging Library to internally add padding data to the image's border. [`M_DRAW_WAVELET_WITH_PADDING`](../../Reference/im/MimDraw.md) draws the resulting image with this padding.  To retrieve the image size required to perform this operation, use [`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_WAVELET_DRAW_SIZE_X_WITH_PADDING`](../../Reference/im/MimGetResult.md) and [`M_WAVELET_DRAW_SIZE_Y_WITH_PADDING`](../../Reference/im/MimGetResult.md).

### Combination Constants — For specifying whether to draw using the real or imaginary numbers in the wavelet result

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether the drawing operation uses the real or imaginary numbers in the wavelet result.

| Value | Description |
| --- | --- |
| `M_IMAGINARY_PART` | Draws using the imaginary part of the values in the wavelet result. Only available for complex wavelet transformations ([`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_TRANSFORMATION_DOMAIN`](../../Reference/im/MimGetResult.md)must return [`M_COMPLEX`](../../Reference/im/MimGetResult.md)). |
| `M_REAL_PART` *(default)* | Draws using only the real numbers in the result. Available for any type of wavelet transformation. |

### For specifying to perform the draw operation from image buffer(s) containing a depth map and/or intensity map

In the following table, [`Src1AilId`](../../Reference/im/MimDraw.md), and optionally [`Src2AilId`](../../Reference/im/MimDraw.md), specify image buffer(s) and [`Operation`](../../Reference/im/MimDraw.md) specifies the operation to perform with the specified image buffer(s).

---

### `Uncorrected depth map image buffer ID`

Specifies an image buffer containing an uncorrected depth map.  The image buffer must be properly formatted. This is the format that would result if you iterated calls to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) and then drew the results using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_DEPTH_MAP_ROW`](../../Reference/im/MimDraw.md). For more information, see [Generating an uncorrected depth map](../../UserGuide/C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).

#### `M_DRAW_PEAKS`

Draws the peaks at the position at which they were found in the original grayscale source image. It draws each peak using the peak's calculated intensity value or using the foreground color of the specified 2D graphics context.  This operation cannot be rendered in world units ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) cannot be set to [`M_WORLD`](../../Reference/gra/MgraControl.md)) when drawing in either an image buffer or a 2D graphics list. Additionally, this operation cannot be drawn with offset or zoom values ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) or [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) set to values other than 0.0, and [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) or [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) set to values other than 1.0).  This operation can draw in a 2D graphics list if no intensity buffer is specified ([`Src2AilId`](../../Reference/im/MimDraw.md) set to [`M_NULL`](../../Reference/im/MimDraw.md)).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to draw the peaks in the foreground color of the 2D graphics context specified using [`ContextGraId`](../../Reference/im/MimDraw.md). |
| `Uncorrected intensity map image buffer ID` | Specifies an image buffer containing an uncorrected intensity map. When this buffer is specified, the peaks are drawn using their corresponding intensity value.  The image buffer must be properly formatted. This is the format that would result if you iterated calls to [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) and then drew the results using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_INTENSITY_MAP_ROW`](../../Reference/im/MimDraw.md). For more information, see [Generating an uncorrected depth map](../../UserGuide/C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).  If you supply an intensity buffer, the intensity buffer defines the drawing color when drawing each position, regardless of the setting of [`ContextGraId`](../../Reference/im/MimDraw.md). [`ContextGraId`](../../Reference/im/MimDraw.md) must be set to [`M_DEFAULT`](../../Reference/im/MimDraw.md). |
| `M_FROM_LINE_THICKNESS` | Specifies to use the [`M_LINE_THICKNESS`](../../Reference/gra/MgraInquire.md) setting of the 2D graphics context as the size of the drawn representation of valid peaks. |
| `Value >= 1` | Specifies the size of the drawn representation of valid peaks, in pixels. |

### For specifying to perform the draw operation with the result buffer(s) used in

In the following table, [`Src1AilId`](../../Reference/im/MimDraw.md) specifies the result buffer and [`Operation`](../../Reference/im/MimDraw.md) specifies the operation to perform with the specified result buffer.

---

### `Locate peak 1D result buffer ID`

Specifies a result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md) and used in [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operations.

#### `M_DRAW_DEPTH_MAP_ROW`

Specifies to write the position values of the specified result buffer into a single row of the specified image buffer.  > **Note:** Note that if the result buffer contains results from multiple frames, and you specify to draw all results ([`M_ALL`](../../Reference/im/MimDraw.md)), each frame's results will be drawn into a separate row, beginning at the row index specified with [`M_DRAW_DEPTH_MAP_ROW`](../../Reference/im/MimDraw.md).

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to draw results from all frames whose results have been accumulated in the result buffer. |
| `0 <= Value < M_NUMBER_OF_FRAMES` | Specifies the index of the specific frame for which to draw results. |

#### `M_DRAW_INTENSITY_MAP_ROW`

Specifies to write the intensity values of the specified result buffer into a single row of the specified image buffer.  The default intensity value for missing peaks, [`M_INVALID`](../../Reference/im/MimControl.md), corresponds to the value -1 (or an unsigned buffer's maximum value); to specify a different value, use [`MimControl`](../../Reference/im/MimControl.md) with [`M_PEAK_INTENSITY_INVALID_VALUE`](../../Reference/im/MimControl.md).  > **Note:** Note that if the result buffer contains results from multiple frames, and you specify to draw all results ([`M_ALL`](../../Reference/im/MimDraw.md)), each frame's results will be drawn into a separate row, beginning at the row index specified with [`M_DRAW_INTENSITY_MAP_ROW`](../../Reference/im/MimDraw.md).

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to draw results from all frames whose results have been accumulated in the result buffer. |
| `0 <= Value < M_NUMBER_OF_FRAMES` | Specifies the index of the specific frame for which to draw results. |

#### `M_DRAW_PEAKS`

Draws the peaks at the position at which they were found in the original source image. It draws each peak using the foreground color of the specified 2D graphics context.  This operation cannot be rendered in world units ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) cannot be set to [`M_WORLD`](../../Reference/gra/MgraControl.md)) when drawing in either an image buffer or a 2D graphics list. Additionally, this operation cannot be drawn with offset or zoom values ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md) or [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md) set to values other than 0.0, and [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md) or [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) set to values other than 1.0).

| Value | Description |
| --- | --- |
| `M_SELECT_PEAK` | Specifies the frame and rank of the peak(s) for which to draw the requested result.  This macro is required if a [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operation accumulated results from multiple frames in the result buffer ([`MimControl`](../../Reference/im/MimControl.md) with[`M_NUMBER_OF_FRAMES`](../../Reference/im/MimControl.md) set to a value greater than 1). |
| `M_ALL` | Specifies to draw all the peaks. |
| `Value >= 0` | Specifies the index of the peak to draw. |
| `M_FROM_LINE_THICKNESS` | Specifies to use the [`M_LINE_THICKNESS`](../../Reference/gra/MgraInquire.md) setting of the 2D graphics context as the size of the drawn representation of valid peaks. |
| `Value >= 1` | Specifies the size of the drawn representation of valid peaks, in pixels. |

### Combination Constants — For specifying the style in which to draw

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the style in which to draw.

| Value | Description |
| --- | --- |
| `M_CROSS` | Specifies to draw the valid result positions as crosses. The destination buffer must be a 2D graphics list.  When [`M_CROSS`](../../Reference/im/MimDraw.md) is specified, only valid positions are drawn; if a peak was not found for a row/column, nothing will be drawn for that row/column. |
| `M_DOTS` | Specifies to draw the valid result positions as dots.  When [`M_DOTS`](../../Reference/im/MimDraw.md) is specified, only valid positions are drawn; if a peak was not found for a row/column, nothing will be drawn for that row/column. |
| `M_LINES` *(default)* | Specifies to draw line segments between valid positions. |

### Combination Constants — For specifying the direction in which the peak detection was performed

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the direction in which the peak detection was performed.

| Value | Description |
| --- | --- |
| `M_HORIZONTAL` | Specifies that the peak detection was performed in the horizontal direction. |
| `M_VERTICAL` *(default)* | Specifies that the peak detection was performed in the vertical direction. |
