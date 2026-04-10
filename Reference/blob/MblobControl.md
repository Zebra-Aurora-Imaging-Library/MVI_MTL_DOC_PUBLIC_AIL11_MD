---
doctype: Reference
module: blob
function: MblobControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / blob / MblobControl"
---

# MblobControl

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

> Control a blob analysis setting.

## Syntax

```c
void MblobControl(
    AIL_ID     ContextOrResultBlobId,  //out
    AIL_INT64  ControlType,            //in
    AIL_DOUBLE ControlValue            //in
)
```

## Description

This function allows you to control a setting of the specified blob analysis context or result buffer.

For blob analysis contexts, these settings control the execution of [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), and which blob features are enabled for calculation. For blob analysis result buffers, these settings control the post-manipulation of results. If [`MblobControl`](../../Reference/blob/MblobControl.md) is not called, default processing controls and values are used in calculations.

You can inquire about most of these settings using [`MblobInquire`](../../Reference/blob/MblobInquire.md).

Changing certain control type settings will cause [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) to discard all results currently in your result buffer prior to calculation. If this is the case, it will be mentioned in the preamble to the table containing these control types.

## Parameters

### `ContextOrResultBlobId` *(out, AIL_ID)*

Specifies the identifier of the blob analysis context or result buffer.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For global processing settings of a blob analysis context that cause results to be recalculated

For the following global processing setting control types, you must set the[`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context.  Changing the value of the following control types between calls to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) will cause all the results currently in your Blob analysis result buffer to be discarded upon the next call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

---

### `M_BLOB_IDENTIFICATION_MODE`

Sets whether to calculate features for each blob, or to treat groups of blobs as single blobs and calculate the features of these blobs as if they were one blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDIVIDUAL` *(default)* | Specifies that all blobs are measured individually. |
| `M_LABELED` | Specifies that blobs with the same label are grouped together, and that touching blobs with different labels are also grouped together.  The foreground value of the blob identifier image will be used as the label value of the blob; therefore, when using [`M_LABELED`](../../Reference/blob/MblobControl.md), the [`M_FOREGROUND_VALUE`](../../Reference/blob/MblobControl.md) control type cannot be set to [`M_ZERO`](../../Reference/blob/MblobControl.md), and the identifier image cannot be binary (1-bit).  [`M_LABELED`](../../Reference/blob/MblobControl.md) is not supported for merge operations using [`MblobMerge`](../../Reference/blob/MblobMerge.md). |
| `M_LABELED_TOUCHING` | Specifies that blobs with the same label are grouped together, and that touching blobs with different labels are measured individually.  The foreground value of the blob identifier image will be used as the label value of the blob; therefore, when using [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md), the [`M_FOREGROUND_VALUE`](../../Reference/blob/MblobControl.md) control type cannot be set to [`M_ZERO`](../../Reference/blob/MblobControl.md). In addition, the blob identifier image cannot be binary (1-bit).  [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md) is not supported with the following:  - [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), when chain features are enabled for calculation ([`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md), or [`M_ALL_FEATURES`](../../Reference/blob/MblobControl.md)). - [`MblobDraw`](../../Reference/blob/MblobDraw.md) with [`M_DRAW_BLOBS_CONTOUR`](../../Reference/blob/MblobDraw.md), [`M_DRAW_HOLES_CONTOUR`](../../Reference/blob/MblobDraw.md), or a combination of the two. - [`MblobSelect`](../../Reference/blob/MblobSelect.md) with [`M_MERGE`](../../Reference/blob/MblobSelect.md). |
| `M_WHOLE_IMAGE` | Specifies that all blobs are grouped together.  If the blob identifier image is already binarized (for example, pixel values for an 8-bit image are either 0 or 0xff), you can set [`M_IDENTIFIER_TYPE`](../../Reference/blob/MblobControl.md) to [`M_BINARY`](../../Reference/blob/MblobControl.md) to calculate features faster. |

---

### `M_CONNECTIVITY`

Sets the image lattice for blob analysis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_4_CONNECTED` | Specifies that each pixel has 4 neighbors. |
| `M_8_CONNECTED` *(default)* | Specifies that each pixel has 8 neighbors. |

---

### `M_FERET_ANGLE_SEARCH_END`

Sets the upper limit of the angular search range in which to search for the maximum ([`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobSelect.md)) or minimum ([`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobSelect.md)) Feret diameters.  > **Note:** Note that [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) only searches through the specified range when calculating Feret features. Better results for the features can exist outside of this search range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 180` *(default)* | Specifies the upper limit of the angular search range.  The end value must be greater than the start value. |

---

### `M_FERET_ANGLE_SEARCH_START`

Sets the lower limit of the angular search range in which to search for the maximum ([`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobSelect.md)) or minimum ([`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobSelect.md)) Feret diameters.  > **Note:** Note that [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) only searches through the specified range when calculating Feret features. Better results for the features can exist outside of this search range.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 180` *(default)* | Specifies the lower limit of the angular search range.  The start value must be less than the end value. |

---

### `M_FOREGROUND_VALUE`

Sets which pixel values are considered to be in the foreground.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONZERO` *(default)* | Specifies the blobs consisting of non-zero pixels. |
| `M_ZERO` | Specifies the blobs consisting of zero pixels. |

---

### `M_NUMBER_OF_FERETS`

Sets the number of Feret angles to use when calculating a Feret feature ([`M_FERET_...`](../../Reference/blob/MblobGetResult.md)) or a feature based on Ferets (that is, [`M_RECTANGULARITY`](../../Reference/blob/MblobControl.md), [`M_ROUGHNESS`](../../Reference/blob/MblobControl.md), or [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies the use of a high precision algorithm to accurately calculate Feret features (and features based on Feret features).  > **Note:** The [`M_FERET_DIAMETERS`](../../Reference/blob/MblobGetResult.md) result type is not supported when you use this setting. |
| `M_MIN_FERETS` | Specifies the minimum number of Feret angles. The minimum number of Feret angles is 2. |
| `Value > M_MIN_FERETS` *(default)* | Specifies the number of Feret angles.  The first Feret angle used is always 0°, and the difference between successive angles is 180° / number of Ferets. |

### For global processing settings of a blob analysis context that do not cause results to be recalculated

For the following global processing setting control types, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context. These control types do not cause any results currently in the result buffer to be discarded when [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) is called.

---

### `M_FERET_CONTACT_POINTS`

Sets whether to calculate Feret contact points for features that support it (for example, [`M_FERET_AT_PRINCIPAL_AXIS_ANGLE`](../../Reference/blob/MblobControl.md)) if the feature has been enabled for calculation. This enables the use of the [`M_FERET_CONTACT_POINTS_...`](../../Reference/blob/MblobGetResult.md) combination constants in [`MblobGetResult`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret contact points for supported features will not be calculated. |
| `M_ENABLE` | Specifies that the Feret contact points for supported features will be calculated. |

---

### `M_FERET_GENERAL_ANGLE`

Sets the angle to use when calculating [`M_FERET_GENERAL`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-360.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_IDENTIFIER_TYPE`

Sets the values that non-zero pixels in the blob identifier image can have.  If the blob identifier image is already binarized (for example, pixel values for an 8-bit image are either 0 or 0xff), you can set this control type to [`M_BINARY`](../../Reference/blob/MblobControl.md) to calculate features faster.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BINARY` | Specifies that non-zero pixels must have the maximum value of the buffer (for example, 0xff for an 8-bit image). |
| `M_GRAYSCALE` *(default)* | Specifies that non-zero pixels can have any value. |

---

### `M_MAX_BLOBS`

Sets the maximum number of blobs to process. If this number is reached, processing is stopped, and results available in the result buffer become invalid and are discarded unless[`M_RETURN_PARTIAL_RESULTS`](../../Reference/blob/MblobControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no limit on the maximum number of blobs. |
| `Value >= 0` | Specifies the maximum number of blobs. |

---

### `M_MOMENT_GENERAL_MODE`

Sets the type of moment to calculate when calculating a user-specified moment ([`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTRAL` | Specifies a central moment.  A central moment is defined as *[Image: mblobselectfeature_moment_central.png]*  , where _i_ runs from 1 to the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate, _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _, *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _, _q_ and _r_ are the order of the moment in X and Y respectively.  The weighted mean is the first-order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).  Central moments use coordinates relative to each blob's center of gravity, and are therefore independent of a blob's position within an image. |
| `M_ORDINARY` *(default)* | Specifies an ordinary moment.  An ordinary moment is defined as *[Image: mblobselectfeature_moment.png]*  , where _i_ runs from 1 to the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate, _y<sub>i</sub> _ = its Y-coordinate, and (_q_+_r_) are the order of the moment in X and Y respectively.  Ordinary moments are affected by the blob position because they use coordinates relative to the image origin (top-left corner). |

---

### `M_MOMENT_GENERAL_ORDER_X`

Sets the order of the X-component of the user-specified moment. Use this control type when calculating a user-specified moment ([`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the X-order of the moment. |

---

### `M_MOMENT_GENERAL_ORDER_Y`

Sets the order of the Y-component of the user-specified moment. Use this control type when calculating a user-specified moment ([`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the Y-order of the moment. |

---

### `M_MOMENT_THIRD_ORDER_MODE`

Sets how to calculate third-order moments.  [`M_FAST`](../../Reference/blob/MblobControl.md) is faster than [`M_PRECISE`](../../Reference/blob/MblobControl.md), but it might cause some imprecisions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST` *(default)* | Specifies that third-order moments will be calculated quickly. |
| `M_PRECISE` | Specifies that third-order moments will be calculated accurately. |

---

### `M_PIXEL_ASPECT_RATIO`

Sets the pixel aspect ratio of the image(s). This is only useful if the image has a uniform pixel aspect ratio with different X- and Y-scales. For non-uniform distortion, you can use [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) to calibrate the image, and [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) to physically correct the image. All results returned by [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) are affected by [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md), including those that are simply positions within the image. For more information, see [Pixel aspect ratio](../../UserGuide/C06_Blob_analysis/S04_Adjusting_blob_analysis_processing_controls.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the pixel width/pixel height. |

---

### `M_RETURN_PARTIAL_RESULTS`

Sets whether results from partially scanned images will be available after a stop condition is met.  The three stop conditions are:  - The maximum number of blobs ([`M_MAX_BLOBS`](../../Reference/blob/MblobControl.md)) has been processed. - The timeout limit ([`M_TIMEOUT`](../../Reference/blob/MblobControl.md)) is exceeded. - The calculation has been stopped by the user ([`M_STOP_CALCULATE`](../../Reference/blob/MblobControl.md)).  When a stop condition is met, the image is only partially scanned and results about blobs found in the partially scanned image are discarded. When [`M_RETURN_PARTIAL_RESULTS`](../../Reference/blob/MblobControl.md) is set to [`M_ENABLE`](../../Reference/blob/MblobControl.md), you can access the results of those blobs that are completely defined (those blobs whose entire perimeter was scanned) in the partially scanned image.  > **Note:** Note that when using multi-processing ([`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_MP_USE`](../../Reference/app/MappControlMp.md) set to [`M_ENABLE`](../../Reference/app/MappControlMp.md)) and this control type is enabled, it is possible that the number of blobs returned can exceed the number set using [`M_MAX_BLOBS`](../../Reference/blob/MblobControl.md). This is because multi-processing might divide the image into pieces and process each piece independently; in this case, there is no global number of blobs counted for the entire image until everything is processed. When a stop condition is met in one of the image pieces, processing is stopped everywhere. The total number of blobs is then combined and returned, which can be greater than [`M_MAX_BLOBS`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to discard results of partially scanned images when processing is interrupted. |
| `M_ENABLE` | Specifies to make results of partially scanned images available when processing is interrupted. |

---

### `M_SAVE_RUNS`

Sets whether to save run information when calling [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to save run information. Disabling saves time when performing the first call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) and reduces the memory requirements for the result buffer. However, you cannot use [`MblobLabel`](../../Reference/blob/MblobLabel.md), or [`MblobGetLabel`](../../Reference/blob/MblobGetLabel.md), and certain draw operations are disabled. See [`MblobDraw`](../../Reference/blob/MblobDraw.md) for more information on which draw operations require run information to be saved. In addition, using [`MblobControl`](../../Reference/blob/MblobControl.md), you cannot enable for calculation the chained pixels features ([`M_CHAINS`](../../Reference/blob/MblobControl.md)), the convex hull features ([`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md)), [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) when it is set to [`M_INFINITE`](../../Reference/blob/MblobControl.md), and the Feret features ([`M_FERETS`](../../Reference/blob/MblobControl.md)). |
| `M_ENABLE` *(default)* | Specifies to save run information. Calls to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) will save run information from the blob identifier image in the result buffer. |

---

### `M_SORTn`

Sets the feature to use as the _n_ <sup>th</sup> sorting key for results, where _n_ stands for an integer between 1 and 3. Changes to this control type take effect upon the next call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md). If you select a feature as a sorting key that is not enabled, an error will occur on the next call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the blob is not touching the borders of the image. |
| `M_YES` | Specifies that the blob is touching one or more borders of the image. |
| `M_DEFAULT` |  |
| `M_NO_SORT` *(default)* | Specifies to remove the sorting key. |

---

### `M_SORTn_DIRECTION`

Sets the direction in which the _n_ <sup>th</sup> sorting key is sorted, where _n_ stands for an integer between 1 and 3.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` | Specifies the feature as being sorted in descending order. |
| `M_SORT_UP` *(default)* | Specifies the feature as being sorted in ascending order. |

---

### `M_TIMEOUT`

Sets the maximum processing time, in msec. If the timeout limit is exceeded, processing is stopped, and results already available in the result buffer become invalid and will be discarded, unless[`M_RETURN_PARTIAL_RESULTS`](../../Reference/blob/MblobControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no maximum processing time. |
| `Value >= 0` | Specifies the maximum processing time, in msec. |

### For specifying to calculate all features in a single call

The following control type allows you to enable or disable all features for calculation using a single call. For the following control type, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context.

---

### `M_ALL_FEATURES`

Sets whether to calculate all features. You can then selectively enable or disable specific features for calculation.  It is not recommended to use [`M_ALL_FEATURES`](../../Reference/blob/MblobControl.md) in your final application. This is because programs using [`M_ALL_FEATURES`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md) might become slower when new Aurora Imaging Library versions are released, if new features have been added.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the calculation of all features, regardless of their previous setting. This can be used as a quick method to disable the calculation of all features. |
| `M_ENABLE` | Specifies that all of the features will be calculated, regardless of their previous setting. |

### For specifying to calculate features with only a binary definition

The following control types establish which of the blob features, that have only a binary definition, are calculated for each blob; these blob features are calculated using only the blob identifier image. For the following control types, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context. Note that, unless otherwise stated, these features are calculated in pixel units; although you can retrieve them in world units using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) if calculated on a calibrated blob identifier image and [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) is set to [`M_WORLD`](../../Reference/blob/MblobControl.md) or [`M_ACCORDING_TO_CALIBRATION`](../../Reference/blob/MblobControl.md).

---

### `M_BOX`

Sets whether to calculate all image-axis-aligned bounding box features plus X- and Y-Ferets. The image-axis-aligned bounding box is the bounding box that is aligned with the pixel coordinate system's axes. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_BLOB_TOUCHING_IMAGE_BORDERS`](../../Reference/blob/MblobGetResult.md), [`M_BOX_AREA`](../../Reference/blob/MblobGetResult.md), [`M_BOX_ASPECT_RATIO`](../../Reference/blob/MblobGetResult.md), [`M_BOX_FILL_RATIO`](../../Reference/blob/MblobGetResult.md), [`M_FIRST_POINT_X`](../../Reference/blob/MblobGetResult.md), [`M_FIRST_POINT_Y`](../../Reference/blob/MblobGetResult.md), [`M_FERET_X`](../../Reference/blob/MblobGetResult.md), and [`M_FERET_Y`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_BREADTH`

Sets whether to calculate the breadth of a blob. This feature has the same advantages and disadvantages as [`M_LENGTH`](../../Reference/blob/MblobControl.md); that is, this feature is more accurate for long thin blobs because it is derived from the perimeter (P) and area (A) assuming that `P = 2(length + breadth) and A = length x breadth`. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_BREADTH`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_CHAINS`

Sets whether to calculate all 4 chain features. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_CHAIN_X`](../../Reference/blob/MblobGetResult.md), [`M_CHAIN_Y`](../../Reference/blob/MblobGetResult.md), [`M_CHAIN_INDEX`](../../Reference/blob/MblobGetResult.md), [`M_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md), and [`M_TOTAL_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md). Note that the chain code is computed depending on the lattice and foreground values set with [`M_CONNECTIVITY`](../../Reference/blob/MblobControl.md) and [`M_FOREGROUND_VALUE`](../../Reference/blob/MblobControl.md), respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_COMPACTNESS`

Sets whether to calculate the compactness value of a blob. This is defined as the ratio between the area of a circle with the same perimeter as the blob in question, and the area of the blob itself. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_COMPACTNESS`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_CONTACT_POINTS`

Sets whether to calculate the contact features of the image-axis-aligned bounding box. The image-axis-aligned bounding box is the bounding box that is aligned with the pixel coordinate system's axes. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_X_MIN_AT_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_X_MAX_AT_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_X_MAX_AT_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_X_MIN_AT_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_Y_MIN_AT_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_Y_MAX_AT_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_Y_MAX_AT_X_MIN`](../../Reference/blob/MblobGetResult.md), and [`M_Y_MIN_AT_X_MIN`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_CONVEX_HULL`

Sets whether to calculate all convex hull features. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_CONVEX_HULL_AREA`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_COG_X`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_COG_Y`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_FILL_RATIO`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_PERIMETER`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_X`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_XY_PACKED`](../../Reference/blob/MblobGetResult.md), [`M_CONVEX_HULL_Y`](../../Reference/blob/MblobGetResult.md), [`M_NUMBER_OF_CONVEX_HULL_POINTS`](../../Reference/blob/MblobGetResult.md), and [`M_TOTAL_NUMBER_OF_CONVEX_HULL_POINTS`](../../Reference/blob/MblobGetResult.md).  > **Note:** For a quicker result, you can select to calculate[`M_CONVEX_PERIMETER`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_CONVEX_PERIMETER`

Sets whether to calculate the approximation of the perimeter of the convex hull of a blob. It is derived from several Feret diameters; so, a larger number of Ferets gives a more accurate result. You can use [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) to change the number of Ferets angles used when calculating this feature. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobGetResult.md).  > **Note:** For a more accurate result, you can select to calculate[`M_CONVEX_HULL_PERIMETER`](../../Reference/blob/MblobGetResult.md) by enabling [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_ELONGATION`

Sets whether to calculate a value that is equal to [`M_LENGTH`](../../Reference/blob/MblobControl.md) / [`M_BREADTH`](../../Reference/blob/MblobControl.md). It is similar to [`M_FERET_ELONGATION`](../../Reference/blob/MblobGetResult.md), except that it should be used for long thin blobs. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_ELONGATION`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_EULER_NUMBER`

Sets whether to calculate the number of blobs - number of holes. This value is more useful for [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md) than for [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) processing mode. When using the [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md) processing mode, each blob is treated individually, as opposed to grouping all of the blobs in an image together. When using the [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) processing mode, the number of blobs is effectively 1. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_EULER_NUMBER`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_GENERAL`

Sets whether to calculate a Feret diameter at a user-specified angle specified using [`M_FERET_GENERAL_ANGLE`](../../Reference/blob/MblobControl.md). You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_GENERAL`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_MAX_DIAMETER_ELONGATION`

Sets whether to calculate the ratio between the maximum Feret diameter and its perpendicular Feret diameter. Use the [`M_FERET_ANGLE_SEARCH_...`](../../Reference/blob/MblobControl.md) constants to control the angular range in which to find the maximum Feret diameter. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_MAX_DIAMETER_ELONGATION`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_MIN_DIAMETER_ELONGATION`

Sets whether to calculate the ratio between the minimum Feret diameter and its perpendicular Feret diameter. Use the [`M_FERET_ANGLE_SEARCH_...`](../../Reference/blob/MblobControl.md) constants to control the angular range in which to find the minimum Feret diameter. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_MIN_DIAMETER_ELONGATION`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_PERPENDICULAR_TO_MAX_DIAMETER`

Sets whether to calculate the Feret diameter that is perpendicular to the maximum Feret diameter. Use the [`M_FERET_ANGLE_SEARCH_...`](../../Reference/blob/MblobControl.md) constants to control the angular range in which to find the maximum Feret diameter. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_PERPENDICULAR_TO_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_PERPENDICULAR_TO_MIN_DIAMETER`

Sets whether to calculate the Feret diameter that is perpendicular to the minimum Feret diameter. Use the [`M_FERET_ANGLE_SEARCH_...`](../../Reference/blob/MblobControl.md) constants to control the angular range in which to find the minimum Feret diameter. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_PERPENDICULAR_TO_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERETS`

Sets whether to calculate the Feret features. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_DIAMETERS`](../../Reference/blob/MblobGetResult.md), [`M_TOTAL_NUMBER_OF_FERETS`](../../Reference/blob/MblobGetResult.md),[`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobGetResult.md), [`M_FERET_ELONGATION`](../../Reference/blob/MblobGetResult.md), [`M_FERET_MAX_ANGLE`](../../Reference/blob/MblobGetResult.md), [`M_FERET_MIN_ANGLE`](../../Reference/blob/MblobGetResult.md), [`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md), [`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md), and [`M_FERET_MEAN_DIAMETER`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_INTERCEPT`

Sets whether to calculate the intercept features. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_INTERCEPT_0`](../../Reference/blob/MblobGetResult.md), [`M_INTERCEPT_45`](../../Reference/blob/MblobGetResult.md), [`M_INTERCEPT_90`](../../Reference/blob/MblobGetResult.md), and [`M_INTERCEPT_135`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_LENGTH`

Sets whether to calculate the length of a blob. This feature is more accurate for long thin blobs because it is derived from the perimeter (P) and area (A) assuming that `P = 2(length + breadth) and A = length x breadth`. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_LENGTH`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MIN_AREA_BOX`

Sets whether to calculate all the minimum-area bounding box features. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MIN_AREA_BOX_AREA`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_ANGLE`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_CENTER_X`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_CENTER_Y`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_HEIGHT`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_PERIMETER`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_WIDTH`](../../Reference/blob/MblobGetResult.md), [`M_MIN_AREA_BOX_Xn`](../../Reference/blob/MblobGetResult.md), and [`M_MIN_AREA_BOX_Yn`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_MIN_PERIMETER_BOX`

Sets whether to calculate all the minimum-perimeter bounding box features. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MIN_PERIMETER_BOX_AREA`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_ANGLE`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_CENTER_X`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_CENTER_Y`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_HEIGHT`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_PERIMETER`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_WIDTH`](../../Reference/blob/MblobGetResult.md), [`M_MIN_PERIMETER_BOX_Xn`](../../Reference/blob/MblobGetResult.md), and [`M_MIN_PERIMETER_BOX_Yn`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_NUMBER_OF_HOLES`

Sets whether to calculate the number of holes in a blob. Holes that intersect the edge of the image are not counted (they might not be holes). This value is equal to 1 - [`M_EULER_NUMBER`](../../Reference/blob/MblobControl.md) and is therefore a true hole count only in [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) processing mode. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_NUMBER_OF_HOLES`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_PERIMETER`

Sets whether to calculate the total length of edges in a blob (including the edges of any holes), with an allowance made for the staircase effect that is produced when diagonal edges are digitized (inside corners are counted as 1.414, rather than 2.0). A single pixel blob (area = 1) has a perimeter of 4.0. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_PERIMETER`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_RECTANGULARITY`

Sets whether to calculate the degree to which a blob resembles a rectangle. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_RECTANGULARITY`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_ROUGHNESS`

Sets whether to calculate the roughness and irregularity of a blob, and is equal to [`M_PERIMETER`](../../Reference/blob/MblobControl.md) / [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobControl.md). You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_ROUGHNESS`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_RUNS`

Sets whether to calculate the blob run-length encoding information. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_RUN_X`](../../Reference/blob/MblobGetResult.md), [`M_RUN_Y`](../../Reference/blob/MblobGetResult.md), [`M_RUN_LENGTH`](../../Reference/blob/MblobGetResult.md), [`M_NUMBER_OF_RUNS`](../../Reference/blob/MblobGetResult.md), and [`M_TOTAL_NUMBER_OF_RUNS`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_WORLD_BOX`

Sets whether to calculate all of the features related to the world-axis-aligned bounding box. The world-axis-aligned bounding box is the bounding box that is aligned with the relative coordinate system's axes. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_WORLD_FERET_X`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_FERET_Y`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_X_AT_Y_MAX`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_X_AT_Y_MIN`](../../Reference/blob/MblobGetResult.md), [`M_WORLD_Y_AT_X_MAX`](../../Reference/blob/MblobGetResult.md), and[`M_WORLD_Y_AT_X_MIN`](../../Reference/blob/MblobGetResult.md). In a case where more than one X-coordinate touches the minimum Y-coordinate of a blob, it is not possible to predict which coordinate will be calculated and returned. This also applies to a Y-coordinate touching the minimum X-coordinate of a blob.  > **Note:** These features are actually calculated in the world coordinate system. If enabled for calculation, you should pass [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) a calibrated blob identifier image. If an uncalibrated image is passed to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), these features are the equivalent of the [`M_BOX`](../../Reference/blob/MblobControl.md) feature group.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

### For specifying to calculate features with only a grayscale definition

The following control types establish which of the features, that have only a grayscale definition, are calculated for each blob. These blob features can only be calculated if you pass [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) both a blob identifier image and a grayscale buffer (or a grayscale buffer with an ROI that acts as the blob identifier image). For the following control types, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context.

---

### `M_BLOB_CONTRAST`

Sets whether to calculate the difference between the maximum and minimum pixel values of a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_BLOB_CONTRAST`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MAX_PIXEL`

Sets whether to calculate the maximum pixel value in a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MAX_PIXEL`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MEAN_PIXEL`

Sets whether to calculate the mean pixel value in a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MEAN_PIXEL`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MIN_PIXEL`

Sets whether to calculate the minimum pixel value in a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MIN_PIXEL`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_SIGMA_PIXEL`

Sets whether to calculate the standard deviation of pixel values in a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_SIGMA_PIXEL`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_SUM_PIXEL`

Sets whether to calculate the sum of all pixel values in a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_SUM_PIXEL_SQUARED`

Sets whether to calculate the sum of the squares of each pixel value in a blob. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_SUM_PIXEL_SQUARED`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

### For specifying to calculate features with two definitions (binary and grayscale)

The following control types establish which of the features, that have both a binary and a grayscale definition, are calculated for each blob. The binary definition considers all pixels equal; the grayscale definition weighs pixels by their value in the grayscale buffer (the grayscale definition is much slower to calculate).  When you do not specify a combination value for the following control types, both versions of the feature (or groups of features) are enabled for calculation by default. If you do not provide a grayscale buffer to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), only the binary version can be calculated. Specifying a combination value for the following control types will overwrite the settings that were set when no combination value was specified. If a combination value was initially specified for a control type, and a different setting was specified for the same control type without a combination value, it will also overwrite the previous setting. For example, if you specify [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md) with [`M_ENABLE`](../../Reference/blob/MblobControl.md), both the binary and grayscale versions of the feature will be calculated. If you subsequently specify [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md) + [`M_GRAYSCALE`](../../Reference/blob/MblobControl.md) with [`M_DISABLE`](../../Reference/blob/MblobControl.md), only the binary version of the feature will be calculated.  For the following control types, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context.

---

### `M_CENTER_OF_GRAVITY`

Sets whether to calculate both X- and Y-coordinates of the center of gravity. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_CENTER_OF_GRAVITY_X`](../../Reference/blob/MblobGetResult.md), and [`M_CENTER_OF_GRAVITY_Y`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_FERET_AT_PRINCIPAL_AXIS_ANGLE`

Sets whether to calculate the Feret diameter at the principal axis of a blob. Use the [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) control type to set the number of Feret angles that are evaluated. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_AT_PRINCIPAL_AXIS_ANGLE`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_AT_SECONDARY_AXIS_ANGLE`

Sets whether to calculate the Feret diameter at the secondary axis of a blob. Use the [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) control type to set the number of Feret angles that are evaluated. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_AT_SECONDARY_AXIS_ANGLE`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_PRINCIPAL_AXIS_ELONGATION`

Sets whether to calculate the ratio between the Feret diameter at the principal axis and the Feret diameter at the secondary axis. Use the [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) control type to set the number of Feret angles that are evaluated. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_PRINCIPAL_AXIS_ELONGATION`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MOMENT_FIRST_ORDER`

Sets whether to calculate the first-order moments where the order of either the X- or Y-component equals 0 and the order of the other component equals 1. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MOMENT_X0_Y1`](../../Reference/blob/MblobGetResult.md), and [`M_MOMENT_X1_Y0`](../../Reference/blob/MblobGetResult.md). To calculate higher order moments, use [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md) for predefined second-order moments, [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md) for predefined third-order moments, or [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md) for user-specified moments. Note that first-order moment features are calculated faster when using[`M_MOMENT_FIRST_ORDER`](../../Reference/blob/MblobControl.md) than when using [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_MOMENT_GENERAL`

Sets whether to calculate a user-specified moment. Use the [`M_MOMENT_GENERAL_MODE`](../../Reference/blob/MblobControl.md), [`M_MOMENT_GENERAL_ORDER_X`](../../Reference/blob/MblobControl.md), and [`M_MOMENT_GENERAL_ORDER_Y`](../../Reference/blob/MblobControl.md) control types to specify the settings of the user-specified moment. You can retrieve results for this feature using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MOMENT_GENERAL`](../../Reference/blob/MblobGetResult.md).  You could alternatively enable the calculation of [`M_MOMENT_FIRST_ORDER`](../../Reference/blob/MblobControl.md), [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md), or [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md) to calculate a set of first, second, or third-order moments, respectively. Note that first and second-order moment features are calculated faster when using [`M_MOMENT_FIRST_ORDER`](../../Reference/blob/MblobControl.md) or [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md) than when using [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MOMENT_SECOND_ORDER`

Sets whether to calculate the second-order moments. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MOMENT_X0_Y2`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_X1_Y1`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_X2_Y0`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X0_Y2`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X1_Y1`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X2_Y0`](../../Reference/blob/MblobGetResult.md), [`M_AXIS_PRINCIPAL_ANGLE`](../../Reference/blob/MblobGetResult.md), and [`M_AXIS_SECONDARY_ANGLE`](../../Reference/blob/MblobGetResult.md). To calculate higher order moments, use [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md) for predefined third-order moments, or [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md) for user-specified moments. Note that second-order moment features are calculated faster when using [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md) than when using [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_MOMENT_THIRD_ORDER`

Sets whether to calculate the third-order moments. Calculating these moments makes the Hu moment invariants available. You can retrieve results for these features using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_MOMENT_X1_Y2`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_X3_Y0`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_X0_Y3`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X1_Y2`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X2_Y1`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X3_Y0`](../../Reference/blob/MblobGetResult.md), [`M_MOMENT_CENTRAL_X0_Y3`](../../Reference/blob/MblobGetResult.md), and [`M_MOMENT_HU_n`](../../Reference/blob/MblobGetResult.md), where `_n_` ranges from 1 to 7. To calculate higher order moments, use [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

### Combination Constants — For feature parameters that have two definitions

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set whether to enable, disable, or sort on only the binary or grayscale calculation of the feature.

For control values of the [`M_SORTn`](../../Reference/blob/MblobControl.md) control type, this combination is essential (that is, you must add one of these combination values to the control value).

| Value | Description |
| --- | --- |
| `M_BINARY` | Selects the binary definition of the selected feature. |
| `M_GRAYSCALE` *(default)* | Selects the grayscale definition of the selected feature. |

### For specifying to calculate features related to depth maps

The following control types establish which of the features, that are related to depth maps, are calculated for each blob. These blob features can only be calculated if you pass [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) a grayscale buffer that is a fully corrected depth map image buffer or 3D-processable depth map container, and has an ROI that acts as the blob identifier image as well as identifies invalid pixels. If you passed an image buffer, you can verify that it contains a fully corrected depth map using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md). You can use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md) to merge confidence information with the blob identifier image. If you passed a container, you can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map. For the following control types, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis context.

---

### `M_DEPTH_MAP_MAX_ELEVATION`

Sets whether to calculate the maximum elevation of a blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum elevation of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the maximum elevation of the blob will be calculated. |

---

### `M_DEPTH_MAP_MEAN_ELEVATION`

Sets whether to calculate the mean elevation of a blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the mean elevation of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the mean elevation of the blob will be calculated. |

---

### `M_DEPTH_MAP_MIN_ELEVATION`

Sets whether to calculate the minimum elevation of a blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum elevation of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the minimum elevation of the blob will be calculated. |

---

### `M_DEPTH_MAP_SIZE_Z`

Sets whether to calculate the Z-size (difference between maximum and minimum elevation) of a blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Z-size of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the Z-size of the blob will be calculated. |

---

### `M_DEPTH_MAP_VOLUME`

Sets whether to calculate the volume of a blob.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the volume of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the volume of the blob will be calculated. |

### For a blob analysis result buffer

For the following control types, you must set the [`ContextOrResultBlobId`](../../Reference/blob/MblobControl.md) parameter to a blob analysis result buffer.

---

### `M_INDEX_MODE`

Sets the index mode to use when retrieving results using[`MblobGetResult`](../../Reference/blob/MblobGetResult.md) and when drawing results using [`MblobDraw`](../../Reference/blob/MblobDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDEX` | Specifies that the index will be passed directly to the[`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) or [`MblobDraw`](../../Reference/blob/MblobDraw.md), without any macros. |
| `M_LABEL` | Specifies that the label will be passed directly to the[`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) or [`MblobDraw`](../../Reference/blob/MblobDraw.md), without any macros. |
| `M_USE_MACRO` *(default)* | Specifies that the values passed to the [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) or [`MblobDraw`](../../Reference/blob/MblobDraw.md) can be the macros [`M_BLOB_INDEX()`](../../Reference/blob/MblobDraw.md) or [`M_BLOB_LABEL()`](../../Reference/blob/MblobDraw.md).  Note that [`M_USE_MACRO`](../../Reference/blob/MblobControl.md) cannot be used in[`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md) mode with a 32-bit blob identifier image. |

---

### `M_INPUT_SELECT_UNITS`

Sets which units to use with the [`CondLow`](../../Reference/blob/MblobSelect.md) and [`CondHigh`](../../Reference/blob/MblobSelect.md) parameters of the [`MblobSelect`](../../Reference/blob/MblobSelect.md) function, when specifying the limit values of the blob selection condition. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_PIXEL` *(default)* | Specifies that the limit values passed to [`MblobSelect`](../../Reference/blob/MblobSelect.md) be interpreted in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that the limit values passed to [`MblobSelect`](../../Reference/blob/MblobSelect.md) be interpreted in world units, with respect to the relative coordinate system. If world units are specified, calling [`MblobSelect`](../../Reference/blob/MblobSelect.md) generates an error if the specified result buffer is not associated with a camera calibration context. |

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.  Note that when returned in world units, some results are only precise if calculated on a physically corrected image. If you require precise results in world units, you should correct the image using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) before calling [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) generates an error if the result was not calculated on a calibrated image. |

---

### `M_STOP_CALCULATE`

Stops the current blob analysis calculation. You must call [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_STOP_CALCULATE`](../../Reference/blob/MblobControl.md) from another thread (typically of higher priority). Results already available in the result buffer become invalid and will be discarded, unless[`M_RETURN_PARTIAL_RESULTS`](../../Reference/blob/MblobControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
