---
doctype: Reference
module: blob
function: MblobGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / blob / MblobGetResult"
---

# MblobGetResult

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

> Get results for a feature of the included blobs, from a blob result buffer.

## Syntax

```c
void MblobGetResult(
    AIL_ID    ResultBlobId,   //in
    AIL_INT   LabelOrIndex,   //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the results for a specified feature of one or all included blobs, from the specified blob analysis result buffer.

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MblobControl`](../../Reference/blob/MblobControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) control type set to [`M_PIXEL`](../../Reference/blob/MblobControl.md). Note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) to [`M_WORLD`](../../Reference/blob/MblobControl.md) without having specified a calibrated image from which to calculate the results, [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) will generate an error. Also note that when using world units, some results are only precise if calculated on a physically corrected image. If you require precise results in world units, you should correct the image using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) before calling [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

When calculated in pixel units, the pixel aspect ratio, specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md), is taken into account horizontally. Results are returned in units of "pixel height" since the pixel width is adjusted to be equal to the pixel height.

Note that [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) generates an error if you try to retrieve a result whose corresponding feature, or the group of features to which it belongs, was not enabled for calculation. You can add the [`M_AVAILABLE`](../../Reference/blob/MblobGetResult.md) combination constant to a feature to determine if the result is available for retrieval.

## Parameters

### `ResultBlobId` *(in, AIL_ID)*

Specifies the identifier of the blob analysis result buffer from which to get results. The specified feature(s) must have already been calculated with [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of the blob for which to get results, or specifies that you are getting general results related to all blobs. This parameter must be set to one of the following values:

*For specifying the label or index of the blob*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_INCLUDED_BLOBS`](../../Reference/blob/MblobGetResult.md) for features that apply to one or all occurrences. Same as [`M_GENERAL`](../../Reference/blob/MblobGetResult.md) for general result types. |
| `M_BLOB_INDEX` | Specifies the index of the blob for which to get results. You can get the number of included blobs in the result buffer using [`M_NUMBER`](../../Reference/blob/MblobGetResult.md). |
| `M_BLOB_LABEL` | Specifies the label of the blob for which to get results. You can get a list of valid blob label values with [`M_LABEL_VALUE`](../../Reference/blob/MblobGetResult.md). |
| `M_GENERAL` | Retrieves a result relating to the entire blob result buffer. |
| `M_INCLUDED_BLOBS` | Retrieves all included blob results. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write results.

## Parameter Associations

### For retrieving general results from the blob result buffer

To retrieve a general result, the [`ResultType`](../../Reference/blob/MblobGetResult.md) parameter should be set to one of the following values. In this case, you must set [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) to [`M_GENERAL`](../../Reference/blob/MblobGetResult.md).

---

### `M_BLOB_IDENTIFICATION_MODE`

Retrieves whether features were calculated for each blob, or for groups of blobs treated as single blobs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDIVIDUAL` *(default)* | Specifies that all blobs were measured individually. |
| `M_LABELED` | Specifies that blobs with the same label were grouped together, and that touching blobs with different labels were also grouped together. |
| `M_LABELED_TOUCHING` | Specifies that blobs with the same label were grouped together, and that touching blobs with different labels were measured individually. |
| `M_WHOLE_IMAGE` | Specifies that all blobs were grouped together. |

---

### `M_CALCULATION_TYPE`

Retrieves the type of calculation that was performed during the last call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), based on the types of images that were passed (that is, only a binary image, or both a binary and grayscale image).

| Value | Description |
| --- | --- |
| `M_BINARY` | Specifies that the previous call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) was only performed on a binary image, and that enabled features with a binary definition are available for retrieval. |
| `M_BINARY_AND_GRAYSCALE` | Specifies that the previous call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) was performed on both a binary and a grayscale image, and that enabled feature results with a binary and grayscale definition are available for retrieval. |
| `M_NOT_CALCULATED` | Specifies that [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) has not been called, and that feature results are not available for retrieval. |

---

### `M_MAX_BLOBS_END`

Retrieves whether the maximum number of blobs was reached. You can use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MAX_BLOBS`](../../Reference/blob/MblobControl.md) to set the limit.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the maximum number of blobs was not reached. |
| `M_TRUE` | Specifies that the maximum number of blobs was reached. |

---

### `M_MAX_LABEL_VALUE`

Retrieves the maximum label value of all blobs in the result buffer, regardless of included or excluded status.  Label values for blobs are generated when a call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) is made. The maximum label value is not necessarily the same as the total number of blobs because some label values might not be used, depending on the blob's shape. One of the uses for obtaining the maximum label value is to determine if an 8 or 16-bit image buffer is needed for [`MblobLabel`](../../Reference/blob/MblobLabel.md).  Note that if zero is returned, no blobs were found.

---

### `M_NUMBER`

Retrieves the number of currently included blobs. All blobs are included unless their status is changed using [`MblobSelect`](../../Reference/blob/MblobSelect.md). Included blobs will be included in future operations and result retrievals.

---

### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_TIMEOUT`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the timeout limit was not reached. |
| `M_TRUE` | Specifies that the timeout limit was reached. |

---

### `M_TOTAL_NUMBER_OF_CHAINED_PIXELS`

Retrieves the total number of chained pixels in all blobs in the target image. The calculation of this result type can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md).

---

### `M_TOTAL_NUMBER_OF_CONVEX_HULL_POINTS`

Retrieves the total number of convex hull points in all blobs in the target image. The calculation of this result type can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_TOTAL_NUMBER_OF_FERETS`

Retrieves the total number of Ferets for all blobs in the target image. The calculation of this result type can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).

---

### `M_TOTAL_NUMBER_OF_RUNS`

Retrieves the total number of runs in all blobs in the target image. The calculation of this result type can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RUNS`](../../Reference/blob/MblobControl.md).

### For retrieving results of a feature with a binary definition, and that cannot be used as a sorting key

To retrieve one or more results for a feature that has only a binary definition and cannot be used as a sorting key, the [`ResultType`](../../Reference/blob/MblobGetResult.md) parameter should be set to one of the following values. In this case, you must set [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) to a specific blob, or all blobs, using **M_BLOB_INDEX()** or **M_BLOB_LABEL()** for individual blobs, or [`M_INCLUDED_BLOBS`](../../Reference/blob/MblobGetResult.md) for all blobs.

---

### `M_BLOB_INCLUSION_STATE`

Retrieves whether each blob is included in calculations and retrieval of results.

| Value | Description |
| --- | --- |
| `M_EXCLUDED` | Specifies that the blob is not included in calculations and retrieval of results. |
| `M_INCLUDED` | Specifies that the blob is included in calculations and retrieval of results. |

---

### `M_CHAIN_INDEX`

Retrieves the indices which indicate the chain to which each chained pixel belongs. Each blob's border chain is identified as index 1. Chains that delimit holes in the blob are identified by subsequent indices. When retrieving the chain index of each chained pixel for all included blobs in the image, you can use [`M_NUMBER_OF_CHAINED_PIXELS`](../../Reference/blob/MblobGetResult.md) to determine the number of chained pixels in each blob, and therefore determine to which blob each chained pixel belongs. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md).

---

### `M_CHAIN_X`

Retrieves the X-coordinate of each chained pixel in the specified blob, for all chains contained within the blob (both the pixels bordering a blob and those delimiting its holes). Chained pixels always form a closed chain. This implies that the starting pixel in the chain is also the closing one. If your blob has regions which are 1 pixel wide, these pixels are chained twice, once in the forward direction and then in the opposite direction. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md).  Blobs with holes have multiple chains. To determine the chain to which a pixel's X-coordinate belongs, use [`M_CHAIN_INDEX`](../../Reference/blob/MblobGetResult.md).

---

### `M_CHAIN_Y`

Retrieves the Y-coordinate of each chained pixel in the specified blob, for all chains contained within the blob (both the pixels bordering a blob and those delimiting its holes). Chained pixels always form a closed chain. This implies that the starting pixel in the chain is also the closing one. If your blob has regions which are 1 pixel wide, these pixels are chained twice, once in the forward direction and then in the opposite direction. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md).  Blobs with holes have multiple chains. To determine the chain to which a pixel's Y-coordinate belongs, use [`M_CHAIN_INDEX`](../../Reference/blob/MblobGetResult.md).

---

### `M_CONVEX_HULL_X`

Retrieves the X-coordinate of each point on the convex perimeter of the specified blob. Convex perimeter pixels always form a closed figure. This implies that the starting pixel on the convex perimeter is also the closing one. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_XY_PACKED`

Retrieves the packed coordinates (X, Y) of each point on the perimeter of the specified blob. Convex perimeter pixels always form a closed figure. This implies that the starting pixel on the convex perimeter is also the closing one. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_Y`

Retrieves the Y-coordinate of each point on the convex perimeter of the specified blob. Convex perimeter pixels always form a closed figure. This implies that the starting pixel on the convex perimeter is also the closing one. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_DIAMETERS`

Retrieves all the Feret diameters of the specified blob in the image.  This feature is not available if you have previously set [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) to [`M_INFINITE`](../../Reference/blob/MblobControl.md) using [`MblobControl`](../../Reference/blob/MblobControl.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).

---

### `M_INDEX_VALUE`

Retrieves the index value for the specified blob(s) in the image. Note that when blob's are excluded or deleted, the index of blobs with indices greater than that of the excluded/deleted blob are reduced by one. Therefore, if you need to specify a specific blob, you might want to use its blob label instead since it doesn't change.

---

### `M_MIN_AREA_BOX_Xn`

Retrieves the X-coordinate of the _n_ <sup>th</sup> vertex of the minimum-area bounding box of each blob, where _n_ stands for an integer between 1 and 4. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).  > **Note:** Note that the labeling of the vertices might be inconsistent from one blob to another within the blob identifier image.

---

### `M_MIN_AREA_BOX_Yn`

Retrieves the Y-coordinate of the _n_ <sup>th</sup> vertex of the minimum-area bounding box of each blob, where _n_ stands for an integer between 1 and 4. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).  > **Note:** Note that the labeling of the vertices might be inconsistent from one blob to another within the blob identifier image.

---

### `M_MIN_PERIMETER_BOX_Xn`

Retrieves the X-coordinate of the _n_ <sup>th</sup> vertex of the minimum-perimeter bounding box of each blob, where _n_ stands for an integer between 1 and 4. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).  > **Note:** Note that the labeling of the vertices might be inconsistent from one blob to another within the blob identifier image.

---

### `M_MIN_PERIMETER_BOX_Yn`

Retrieves the Y-coordinate of the _n_ <sup>th</sup> vertex of the minimum-perimeter bounding box of each blob, where _n_ stands for an integer between 1 and 4. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).  > **Note:** Note that the labeling of the vertices might be inconsistent from one blob to another within the blob identifier image.

---

### `M_NUMBER_OF_FERETS`

Retrieves the number of Feret diameters calculated for each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).

---

### `M_RUN_LENGTH`

Retrieves the length of each run in the specified blob(s). A run is defined as a horizontal series of consecutive foreground pixels. When retrieving the length of each run for all included blobs in the image, you can use [`M_NUMBER_OF_RUNS`](../../Reference/blob/MblobGetResult.md) to determine the number of runs each blob has, and therefore determine to which blob each length corresponds. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RUNS`](../../Reference/blob/MblobControl.md).

---

### `M_RUN_X`

Retrieves the X-coordinate of the start (left-most pixel) of each run in the specified blob(s). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RUNS`](../../Reference/blob/MblobControl.md).

---

### `M_RUN_Y`

Retrieves the Y-coordinate of the start (left-most pixel) of each run in the specified blob(s). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RUNS`](../../Reference/blob/MblobControl.md).

### For retrieving results of a feature with a binary definition, and that can be used as a sorting key

To retrieve one or more results for a feature that has only a binary definition and can be used as a sorting key, the [`ResultType`](../../Reference/blob/MblobGetResult.md) parameter should be set to one of the following values. In this case, you must set [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) to a specific blob, or all blobs, using **M_BLOB_INDEX()** or **M_BLOB_LABEL()** for individual blobs, or [`M_INCLUDED_BLOBS`](../../Reference/blob/MblobGetResult.md) for all blobs. Unless otherwise stated, the following features are calculated in the pixel coordinate system.

---

### `M_AREA`

Retrieves the number of foreground pixels in each blob (holes are not counted). This feature is always calculated.

---

### `M_BLOB_TOUCHING_IMAGE_BORDERS`

Retrieves whether each blob is touching the borders of the image. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the blob is not touching the borders of the image. |
| `M_YES` | Specifies that the blob is touching one or more borders of the image. |

---

### `M_BOX_AREA`

Retrieves the area covered by the image-axis-aligned bounding box of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BOX_ASPECT_RATIO`

Retrieves the ratio between the horizontal size and the vertical size of the image-axis-aligned bounding box of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BOX_FILL_RATIO`

Retrieves the ratio between the area of the blob and the area of the image-axis-aligned bounding box of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BOX_X_MAX`

Retrieves the extreme right X-coordinate of each blob, of the image-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BOX_X_MIN`

Retrieves the extreme left X-coordinate of each blob, of the image-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BOX_Y_MAX`

Retrieves the extreme bottom Y-coordinate of each blob, of the image-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BOX_Y_MIN`

Retrieves the extreme top Y-coordinate of each blob, of the image-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_BREADTH`

Retrieves the breadth of each blob. This feature is only accurate for certain blob types (for example, long thin blobs) because it is derived from the perimeter (P) and area (A) assuming that `P = 2(length + breadth) and A = length x breadth`. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BREADTH`](../../Reference/blob/MblobControl.md).  *[Image: M_BREADTH.png]*

---

### `M_COMPACTNESS`

Retrieves the compactness of each blob. This is the ratio between the area of a circle with the same perimeter as the blob in question, and the area of the blob itself. The minimum theoretical value of 1.0 is obtained if the blob is a perfect circle. In practice, the minimum obtainable value is slightly above 1. This is due to the effect of square pixel discretization. The more convoluted the shape, the greater the value.  The formula used is equal to *[Image: mblobselectfeature_compactness.png]*   where _A_ is the area of the blob and _p_ is the perimeter of the blob.  *[Image: M_COMPACTNESS.png]*  In the illustration above, the blobs have similar sizes but can be distinguished by their shapes. The compactness of the left blob is slightly above 1.0, while the compactness of the right blob is 1.24. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_COMPACTNESS`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_AREA`

Retrieves the area of the convex hull of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_COG_X`

Retrieves the X-component of the center of gravity of the convex hull of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_COG_Y`

Retrieves the Y-component of the center of gravity of the convex hull of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_FILL_RATIO`

Retrieves the ratio between the blob's area and the area of its convex hull.  *[Image: M_CONVEX_HULL_FILL_RATIO.png]*  In the example above, the left blob has a convex hull fill ratio of 1.0, whereas the middle and right-most blobs have a convex hull fill ratio of 0.9. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_HULL_PERIMETER`

Retrieves the perimeter of the convex hull of each blob. The perimeter is calculated by summing up the distance between every 2 consecutive points on the convex hull of a blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_CONVEX_PERIMETER`

Retrieves an approximation of the perimeter of the convex hull of each blob. It is derived from several Feret diameters; so, a larger number of Ferets gives a more accurate result. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobControl.md).

---

### `M_ELONGATION`

Retrieves a value that is equal to [`M_LENGTH`](../../Reference/blob/MblobControl.md) / [`M_BREADTH`](../../Reference/blob/MblobControl.md) of each blob. It is similar to [`M_FERET_ELONGATION`](../../Reference/blob/MblobGetResult.md), except that it should be used for long thin objects. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_ELONGATION`](../../Reference/blob/MblobControl.md).  *[Image: M_ELONGATION.png]*

---

### `M_EULER_NUMBER`

Retrieves the number of blobs minus the number of holes (number of blobs - number of holes). This value is more useful if calculated using the [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md) blob identification mode instead of the [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) blob identification mode. Unlike its usual effect, when using the [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md) blob identification mode, each blob is treated individually, as opposed to grouping all of the blobs in an image together. So [`M_EULER_NUMBER`](../../Reference/blob/MblobGetResult.md) returns (number of blobs - number of holes). Whereas as usual, when using the [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) blob identification mode, each blob is treated separately; therefore, the Euler number for each blob is just `1 - the number of holes`. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_EULER_NUMBER`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_ELONGATION`

Retrieves the measure of the shape of each blob. It is equal to [`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md) / [`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).  It is accurate for reasonably compact objects, but becomes less accurate for very elongated objects (because [`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md) becomes less accurate). For very elongated objects, [`M_ELONGATION`](../../Reference/blob/MblobGetResult.md) should be used.  *[Image: M_FERET_ELONGATION.png]*

---

### `M_FERET_GENERAL`

Retrieves the Feret diameter at the user-specified angle (set using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_GENERAL_ANGLE`](../../Reference/blob/MblobControl.md)). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_GENERAL`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_MAX_ANGLE`

Retrieves the angle, in degrees, at which the maximum Feret diameter is found for each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_MAX_DIAMETER`

Retrieves the largest Feret diameter found after checking a certain number of angles. More angles will give a more accurate result, but will take longer to calculate. With regards to the number of angles evaluated, the maximum Feret diameter is less sensitive than the minimum Feret diameter; generally, evaluating the potential maximum Feret diameter over 8 angles will give an accurate result.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_MAX_DIAMETER.png]*

---

### `M_FERET_MAX_DIAMETER_ELONGATION`

Retrieves the ratio between the maximum Feret diameter of each blob and its perpendicular Feret diameter. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_MAX_DIAMETER_ELONGATION`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_MAX_DIAMETER_ELONGATION.png]*

---

### `M_FERET_MEAN_DIAMETER`

Retrieves the average of the Feret diameters at the angles checked for each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_MIN_ANGLE`

Retrieves the angle, in degrees, at which the minimum Feret diameter is found for each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_MIN_DIAMETER`

Retrieves the minimum Feret diameter found after checking a certain number of angles. More angles will give a more accurate result, but will take longer to calculate. Note that this feature will not be very accurate for long thin blobs. However, you can get an accurate measure of the breadth of long thin blobs more quickly using [`M_BREADTH`](../../Reference/blob/MblobGetResult.md).  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERETS`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_MIN_DIAMETER.png]*

---

### `M_FERET_MIN_DIAMETER_ELONGATION`

Retrieves the ratio between the minimum Feret diameter of each blob and its perpendicular Feret diameter. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_MIN_DIAMETER_ELONGATION`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_MIN_DIAMETER_ELONGATION.png]*

---

### `M_FERET_PERPENDICULAR_TO_MAX_DIAMETER`

Retrieves the Feret diameter that is perpendicular to the maximum Feret diameter of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_PERPENDICULAR_TO_MAX_DIAMETER`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_PERPENDICULAR_TO_MIN_DIAMETER`

Retrieves the Feret diameter that is perpendicular to the minimum Feret diameter of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_PERPENDICULAR_TO_MIN_DIAMETER`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_X`

Retrieves the dimension of the image-axis-aligned minimum bounding box of each blob, along the X-axis of the pixel coordinate system (that is, [`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md) - [`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md) + 1).  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_X.png]*

---

### `M_FERET_Y`

Retrieves the dimension of the image-axis-aligned minimum bounding box of each blob, along the Y-axis of the pixel coordinate system (that is, [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md) - [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md) + 1).  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_Y.png]*

---

### `M_FIRST_POINT_X`

Retrieves the X-coordinate of a unique point (along with [`M_FIRST_POINT_Y`](../../Reference/blob/MblobGetResult.md)) for each blob, that is on the perimeter of the blob. The X-coordinate is that of the left-most pixel on the top-most line of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_FIRST_POINT_Y`

Retrieves the Y-coordinate of a unique point (along with [`M_FIRST_POINT_X`](../../Reference/blob/MblobGetResult.md)) for each blob, that is on the perimeter of the blob. The Y-coordinate is that of the top-most line of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_INTERCEPT_0`

Retrieves the number of times a transition from background to foreground (not vice versa) occurs in the horizontal direction for the entire blob. In other words, it is equal to the number of times the neighborhood configuration [B, F] occurs in a blob, where B is a background pixel and F is a foreground pixel. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INTERCEPT`](../../Reference/blob/MblobControl.md).

---

### `M_INTERCEPT_45`

Retrieves the number of times that the neighborhood configuration *[Image: mblobselectfeature_in45.png]*   occurs in a blob, where _F_ is a foreground pixel, _B_ is a background pixel, and a dot can be any pixel value. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INTERCEPT`](../../Reference/blob/MblobControl.md).

---

### `M_INTERCEPT_90`

Retrieves the number of times that the neighborhood configuration *[Image: mblobselectfeature_in90.png]*   occurs in a blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INTERCEPT`](../../Reference/blob/MblobControl.md).

---

### `M_INTERCEPT_135`

Retrieves the number of times that the neighborhood configuration *[Image: mblobselectfeature_in135.png]*   occurs in a blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INTERCEPT`](../../Reference/blob/MblobControl.md).

---

### `M_LABEL_VALUE`

Retrieves the label value of each blob. You can retrieve the label value of included or excluded blobs, but not deleted blobs. The label value is a positive integer (>= 1) that is unique for each blob.

---

### `M_LENGTH`

Retrieves the length of each blob. This feature is only accurate for certain object types (for example, long thin blobs) because it is derived from the perimeter (P) and area (A) assuming that `P = 2(length + breadth) and A = length x breadth`. It complements [`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md) because it is accurate for different blob types (for example, long thin blobs). Note, it is calculated much faster than the maximum Feret diameter. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_LENGTH`](../../Reference/blob/MblobControl.md).  *[Image: M_LENGTH.png]*

---

### `M_MIN_AREA_BOX_ANGLE`

Retrieves the angle of the minimum-area bounding box of each blob. The angle is always measured from the X-axis of the pixel coordinate system, to the side of the box from which the width is measured. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).  *[Image: MblobMinAreaPerimeterAngle.png]*

---

### `M_MIN_AREA_BOX_AREA`

Retrieves the area of the minimum-area bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_AREA_BOX_CENTER_X`

Retrieves the X-coordinate of the center of the minimum-area bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_AREA_BOX_CENTER_Y`

Retrieves the Y-coordinate of the center of the minimum-area bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_AREA_BOX_HEIGHT`

Retrieves the height (shortest side) of the minimum-area bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_AREA_BOX_PERIMETER`

Retrieves the perimeter of the minimum-area bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_AREA_BOX_WIDTH`

Retrieves the width (longest side) of the minimum-area bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_AREA_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PERIMETER_BOX_ANGLE`

Retrieves the angle of the minimum-perimeter bounding box of each blob. The angle is always measured from the X-axis of the pixel coordinate system to the side of the box from which the width is measured. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).  *[Image: MblobMinAreaPerimeterAngle.png]*

---

### `M_MIN_PERIMETER_BOX_AREA`

Retrieves the area of the minimum-perimeter bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PERIMETER_BOX_CENTER_X`

Retrieves the X-coordinate of the center of the minimum-perimeter bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PERIMETER_BOX_CENTER_Y`

Retrieves the Y-coordinate of the center of the minimum-perimeter bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PERIMETER_BOX_HEIGHT`

Retrieves the height (shortest side) of the minimum-perimeter bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PERIMETER_BOX_PERIMETER`

Retrieves the perimeter of the minimum-perimeter bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PERIMETER_BOX_WIDTH`

Retrieves the width (longest side) of the minimum-perimeter bounding box of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PERIMETER_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_NUMBER_OF_CHAINED_PIXELS`

Retrieves the number of chained pixels in each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md).

---

### `M_NUMBER_OF_CONVEX_HULL_POINTS`

Retrieves the number of points on the convex perimeter of each blob. A better approximation of convex hull features can be taken by increasing the number of Feret diameters evaluated using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md); this has the effect of increasing the number of convex hull points evaluated. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONVEX_HULL`](../../Reference/blob/MblobControl.md).

---

### `M_NUMBER_OF_HOLES`

Retrieves the number of holes in each blob. Holes that intersect the edge of the image are not counted (they might not be holes). This value is equal to `1 - [`M_EULER_NUMBER`](../../Reference/blob/MblobControl.md)` `(1 - (1 - number of holes))` and is therefore a true hole count if calculated using the [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) blob identification mode. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_NUMBER_OF_HOLES`](../../Reference/blob/MblobControl.md).

---

### `M_NUMBER_OF_RUNS`

Retrieves the total number of runs in each blob. A run is defined as a horizontal series of consecutive foreground pixels. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RUNS`](../../Reference/blob/MblobControl.md).

---

### `M_PERIMETER`

Retrieves the total length of edges in each blob (including the edges of any holes), whereby an allowance made for the staircase effect that is produced when diagonal edges are digitized (inside corners are counted as 1.414, rather than 2.0). A single pixel blob (area = 1) has a perimeter of 4.0. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_PERIMETER`](../../Reference/blob/MblobControl.md).

---

### `M_RECTANGULARITY`

Retrieves the degree to which each blob is similar to a rectangle. To do this, [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) calculates the ratio between the blob's area and the product of its minimum Feret diameter and the Feret diameter perpendicular to the minimum Feret diameter.  *[Image: M_RECTANGULARITY.png]*  The three blobs above have the same minimum Feret diameter and the same Feret diameter perpendicular to the minimum Feret diameter. Only their shape and area differs. The rectangularity of the left blob is 1.0, indicating a perfect rectangle. The rectangularity of the middle blob is 0.8 and the rectangularity of the right-most blob is 0.5. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RECTANGULARITY`](../../Reference/blob/MblobControl.md).

---

### `M_ROUGHNESS`

Retrieves the roughness and irregularity of a blob is, and is equal to [`M_PERIMETER`](../../Reference/blob/MblobControl.md) / [`M_CONVEX_PERIMETER`](../../Reference/blob/MblobControl.md). A smooth convex blob will have the minimum roughness of 1.0.  *[Image: M_ROUGHNESS.png]*  In the example above, the left blob has a roughness of 1.0, the middle blob has a roughness of 1.1, and the right blob has a roughess of 1.5. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_ROUGHNESS`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_BOX_X_MAX`

Retrieves the extreme right X-coordinate of the world-axis-aligned bounding box, of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_BOX_X_MIN`

Retrieves the extreme left X-coordinate of the world-axis-aligned bounding box, of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_BOX_Y_MAX`

Retrieves the extreme bottom Y-coordinate of the world-axis-aligned bounding box, of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_BOX_Y_MIN`

Retrieves the extreme top Y-coordinate of the world-axis-aligned bounding box, of each blob.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_FERET_X`

Retrieves the dimension of the world-axis-aligned minimum bounding box, along the X-axis of the relative coordinate system (that is, `[`M_WORLD_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md) - [`M_WORLD_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md) + 1`).  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_FERET_Y`

Retrieves the dimension of the world-axis-aligned minimum bounding box, along the Y-axis of the relative coordinate system (that is, `[`M_WORLD_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md) - [`M_WORLD_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md) + 1`).  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_X_AT_Y_MAX`

Retrieves the X-coordinate at the maximum Y-coordinate of each blob, of the world-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_X_AT_Y_MIN`

Retrieves the X-coordinate at the minimum Y-coordinate of each blob, of the world-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_Y_AT_X_MAX`

Retrieves the Y-coordinate at the maximum X-coordinate of each blob, of the world-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_WORLD_Y_AT_X_MIN`

Retrieves the Y-coordinate at the minimum X-coordinate of each blob, of the world-axis-aligned bounding box.  The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_WORLD_BOX`](../../Reference/blob/MblobControl.md).

---

### `M_X_MAX_AT_Y_MAX`

Retrieves the maximum X-coordinate at the maximum Y-coordinate of each blob. Together with [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_X_MAX_AT_Y_MIN`

Retrieves the maximum X-coordinate at the minimum Y-coordinate of each blob. Together with [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_X_MIN_AT_Y_MAX`

Retrieves the minimum X-coordinate at the maximum Y-coordinate of each blob. Together with [`M_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_X_MIN_AT_Y_MIN`

Retrieves the minimum X-coordinate at the minimum Y-coordinate of each blob. Together with [`M_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_Y_MAX_AT_X_MAX`

Retrieves the maximum Y-coordinate at the maximum X-coordinate of each blob. Together with [`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_Y_MAX_AT_X_MIN`

Retrieves the maximum Y-coordinate at the minimum X-coordinate of each blob. Together with [`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points of the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_Y_MIN_AT_X_MAX`

Retrieves the minimum Y-coordinate at the maximum X-coordinate of each blob. Together with [`M_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

---

### `M_Y_MIN_AT_X_MIN`

Retrieves the minimum Y-coordinate at the minimum X-coordinate of each blob. Together with [`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md), this determines one of the contact points on the convex perimeter of the blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONTACT_POINTS`](../../Reference/blob/MblobControl.md).

### For retrieving results for a feature with a grayscale definition, and that can be used as a sorting key

To retrieve the result for a feature with a grayscale definition, and that can be used as a sorting key, select one of the following. Results for these features are only available if both a blob identifier image and a grayscale buffer (or a grayscale buffer with an ROI that acts as the blob identifier image) were passed to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) (and the corresponding features were selected for calculation). In this case, you must set [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) to a specific blob or all blobs, using **M_BLOB_INDEX()** or **M_BLOB_LABEL()** for a specific blob, or [`M_INCLUDED_BLOBS`](../../Reference/blob/MblobGetResult.md) for all blobs.

---

### `M_BLOB_CONTRAST`

Retrieves the difference between the maximum and minimum pixel values of a blob. It is equal to [`M_MAX_PIXEL`](../../Reference/blob/MblobGetResult.md) - [`M_MIN_PIXEL`](../../Reference/blob/MblobGetResult.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BLOB_CONTRAST`](../../Reference/blob/MblobControl.md).

---

### `M_MAX_PIXEL`

Retrieves the maximum pixel value found in each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MAX_PIXEL`](../../Reference/blob/MblobControl.md).

---

### `M_MEAN_PIXEL`

Retrieves the mean pixel value in each blob. It is equal to [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) / [`M_AREA`](../../Reference/blob/MblobGetResult.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MEAN_PIXEL`](../../Reference/blob/MblobControl.md).

---

### `M_MIN_PIXEL`

Retrieves the minimum pixel value found in each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MIN_PIXEL`](../../Reference/blob/MblobControl.md).

---

### `M_SIGMA_PIXEL`

Retrieves the standard deviation of pixel values in each blob. It is equal to *[Image: mblobselectfeature_sigma.png]*  , where _N_ = number of pixels and _p_ = pixel value. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SIGMA_PIXEL`](../../Reference/blob/MblobControl.md).

---

### `M_SUM_PIXEL`

Retrieves the sum of all pixel values in each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SUM_PIXEL`](../../Reference/blob/MblobControl.md).

---

### `M_SUM_PIXEL_SQUARED`

Retrieves the sum of the squares of each pixel value in each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SUM_PIXEL_SQUARED`](../../Reference/blob/MblobControl.md).

### For retrieving results for a result of a feature that has two different definitions, and that can be used as a sorting key

To retrieve the result for a feature that has two different definitions (a binary and a grayscale definition), and that can be used as a sorting key, select one of the following values. If you did not provide both a blob identifier image and a grayscale buffer, only the binary version was calculated. If you did provide a grayscale buffer, both versions were calculated, unless otherwise specified. You must set [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) to a specific blob or all blobs, using **M_BLOB_INDEX()** or **M_BLOB_LABEL()** for a specific blob, or [`M_INCLUDED_BLOBS`](../../Reference/blob/MblobGetResult.md) for all blobs.

---

### `M_AXIS_PRINCIPAL_ANGLE`

Retrieves the angle, in degrees, at which each blob has the least moment of inertia. When the blob identifier image is calibrated, this feature is calculated in calibrated units; otherwise, it is calculated in pixel units. For elongated blobs, the angle of the blobs' longest length is taken. The result is calculated as: *[Image: mblobselectfeature_prinangle.png]*   The result is always between -90° and +90°, relative to the output coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).

---

### `M_AXIS_SECONDARY_ANGLE`

Retrieves the angle perpendicular to [`M_AXIS_PRINCIPAL_ANGLE`](../../Reference/blob/MblobGetResult.md) of each blob, in degrees. When the blob identifier image is calibrated, this feature is calculated in calibrated units; otherwise, it is calculated in pixel units. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md). The result is always between -90° and +90°, relative to the output coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md).

---

### `M_CENTER_OF_GRAVITY_X`

Retrieves the X-position of the center of gravity of each blob. The grayscale version is [`M_MOMENT_X1_Y0`](../../Reference/blob/MblobGetResult.md) / [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md). The binary version uses [`M_AREA`](../../Reference/blob/MblobGetResult.md) instead of [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CENTER_OF_GRAVITY`](../../Reference/blob/MblobControl.md).

---

### `M_CENTER_OF_GRAVITY_Y`

Retrieves the Y-position of the center of gravity of each blob. The grayscale version is [`M_MOMENT_X0_Y1`](../../Reference/blob/MblobGetResult.md) / [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md). The binary version uses [`M_AREA`](../../Reference/blob/MblobGetResult.md) instead of [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CENTER_OF_GRAVITY`](../../Reference/blob/MblobControl.md).

---

### `M_FERET_AT_PRINCIPAL_AXIS_ANGLE`

Retrieves the Feret diameter at the principal axis of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_AT_PRINCIPAL_AXIS_ANGLE`](../../Reference/blob/MblobControl.md).  The principal axis is the axis at which the blob has the least moment of inertia. Also, if the blob is symmetrical, the principal axis is aligned with the blob's axis of symmetry.  You can retrieve the angle of the principal axis, by calling [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_AXIS_PRINCIPAL_ANGLE`](../../Reference/blob/MblobGetResult.md).

---

### `M_FERET_AT_SECONDARY_AXIS_ANGLE`

Retrieves the Feret diameter at the secondary axis of each blob. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_AT_SECONDARY_AXIS_ANGLE`](../../Reference/blob/MblobControl.md).  The secondary axis is perpendicular to the principal axis. You can retrieve the angle of the secondary axis by calling [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_AXIS_SECONDARY_ANGLE`](../../Reference/blob/MblobGetResult.md).

---

### `M_FERET_PRINCIPAL_AXIS_ELONGATION`

Retrieves the ratio between the Feret diameter at the principal axis and the Feret diameter at the secondary axis of each blob. It is equal to [`M_FERET_AT_PRINCIPAL_AXIS_ANGLE`](../../Reference/blob/MblobGetResult.md) / [`M_FERET_AT_SECONDARY_AXIS_ANGLE`](../../Reference/blob/MblobGetResult.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_PRINCIPAL_AXIS_ELONGATION`](../../Reference/blob/MblobControl.md).  *[Image: M_FERET_PRINCIPAL_AXIS_ELONGATION.png]*

---

### `M_MOMENT_CENTRAL_X0_Y2`

Retrieves the central moment of each blob where the order of X equals 0 and the order of Y equals 2. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: mblobselectfeature_moment_central_0-2.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_CENTRAL_X0_Y3`

Retrieves the central moment of each blob where the order of X equals 0 and the order of Y equals 3. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: moment_central_x0_y3_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_CENTRAL_X1_Y1`

Retrieves the central moment of each blob where the order of X equals 1 and the order of Y equals 1. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: mblobselectfeature_moment_central_1-1.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and, _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _, and *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_CENTRAL_X1_Y2`

Retrieves the central moment of each blob where the order of X equals 1 and the order of Y equals 2. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: m_moment_central_x1_y2_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and, _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _, and *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_CENTRAL_X2_Y0`

Retrieves the central moment of each blob where the order of X equals 2 and the order of Y equals 0. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: mblobselectfeature_moment_central_2-0.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_CENTRAL_X2_Y1`

Retrieves the central moment of each blob where the order of X equals 2 and the order of Y equals 1. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: m_moment_central_x2_y1_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and, _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _, and *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_CENTRAL_X3_Y0`

Retrieves the central moment of each blob where the order of X equals 3 and the order of Y equals 0. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This central moment is defined as *[Image: m_moment_central_x3_y0_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _.  The weighted mean is the first order ordinary moment divided by the sum of the pixel values ([`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) for grayscale definitions, [`M_AREA`](../../Reference/blob/MblobGetResult.md) for binary definitions).

---

### `M_MOMENT_GENERAL`

Retrieves the moment calculation, based on the settings specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL_MODE`](../../Reference/blob/MblobControl.md), [`M_MOMENT_GENERAL_ORDER_X`](../../Reference/blob/MblobControl.md), and [`M_MOMENT_GENERAL_ORDER_Y`](../../Reference/blob/MblobControl.md). The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md).

---

### `M_MOMENT_HU_n`

Retrieves the nth Hu moment of each blob. The calculation of these features can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  The seven Hu moments are defined as:  `_I<sub>1</sub> _ = _η<sub>20</sub> _ + _η<sub>02</sub> _`  `_I<sub>2</sub> _ = (_η<sub>20</sub> _ - _η<sub>02</sub> _)<sup>2</sup>+ 4_η<sub>11</sub> <sup>2</sup> _`  `_I<sub>3</sub> _ = (_η<sub>30</sub> _ - 3_η<sub>12</sub> _)<sup>2</sup>+ (3_η<sub>21</sub> _ - _η<sub>03</sub> _)<sup>2</sup>`  `_I<sub>4</sub> _ = (_η<sub>30</sub> _ + _η<sub>12</sub> _)<sup>2</sup>+ (_η<sub>21</sub> _ + _η<sub>03</sub> _)<sup>2</sup>`  `_I<sub>5</sub> _ = (_η<sub>30</sub> _ - 3_η<sub>12</sub> _)(_η<sub>30</sub> _ + _η<sub>12</sub> _)[(_η<sub>30</sub> _ + _η<sub>12</sub> _)<sup>2</sup> - 3(_η<sub>21</sub> _ + _η<sub>03</sub> _)<sup>2</sup>] + (3_η<sub>21</sub> _ - _η<sub>03</sub> _)(_η<sub>21</sub> _ + _η<sub>03</sub> _)[3(_η<sub>30</sub> _ + _η<sub>12</sub> _)<sup>2</sup> - (_η<sub>21</sub> _ + _η<sub>03</sub> _)<sup>2</sup>]`  `_I<sub>6</sub> _ = (_η<sub>20</sub> _ - _η<sub>02</sub> _)[(_η<sub>30</sub> _ + _η<sub>12</sub> _)<sup>2</sup> - (_η<sub>21</sub> _ + _η<sub>03</sub> _)<sup>2</sup>] + 4_η<sub>11</sub> _(_η<sub>30</sub> _ + _η<sub>12</sub> _)(_η<sub>21</sub> _ + _η<sub>03</sub> _)`  `_I<sub>7</sub> _ = (3_η<sub>21</sub> _ - _η<sub>03</sub> _)(_η<sub>30</sub> _ + _η<sub>12</sub> _)[(_η<sub>30</sub> _ + _η<sub>12</sub> _)<sup>2</sup> - 3(_η<sub>21</sub> _ + _η<sub>03</sub> _)<sup>2</sup>] - (_η<sub>30</sub> _ - 3_η<sub>12</sub> _)(_η<sub>21</sub> _ + _η<sub>03</sub> _)[3(_η<sub>30</sub> _ + _η<sub>12</sub> _)<sup>2</sup> - (_η<sub>21</sub> _ + _η<sub>03</sub> _)<sup>2</sup>]`  Each scale invariant is defined with: *[Image: m_MOMENT_HU_eq.png]*  where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and, _y<sub>i</sub> _ = its Y-coordinate, *[Image: mblob_xbar.png]*   = the weighted mean of _x<sub>i</sub> _, and *[Image: mblob_ybar.png]*   = the weighted mean of _y<sub>i</sub> _.

---

### `M_MOMENT_X0_Y1`

Retrieves the ordinary moment of each blob where the order of X equals 0 and the order of Y equals 1. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_FIRST_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: mblobselectfeature_moment_0-1.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments) and _y<sub>i</sub> _ = its Y-coordinate.  This calculation returns the mean of Y for each blob. Therefore, in order to calculate the weighted arithmetic mean about _Y_, take the result returned by the calculation of this feature and divide it by the [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) feature (for a grayscale definition), for a binary definition divide it by the number of pixels in the blob ([`M_AREA`](../../Reference/blob/MblobGetResult.md)).

---

### `M_MOMENT_X0_Y2`

Retrieves the ordinary moment of each blob where the order of X equals 0 and the order of Y equals 2. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: mblobselectfeature_moment_0-2.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments) and_y<sub>i</sub> _ = its Y-coordinate.

---

### `M_MOMENT_X0_Y3`

Retrieves the ordinary moment of each blob where the order of X equals 0 and the order of Y equals 3. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: central_moment_x0_y3.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments) and _y<sub>i</sub> _ = its Y-coordinate.

---

### `M_MOMENT_X1_Y0`

Retrieves the ordinary moment of each blob where the order of X equals 1 and the order of Y equals 0. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_FIRST_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: mblobselectfeature_moment_1-0.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments) and `_x<sub>i</sub> _` = its X-coordinate.  This calculation returns the mean of X for each blob. Therefore, in order to calculate the weighted arithmetic mean about _X_, take the result returned by the calculation of this feature and divide it by the [`M_SUM_PIXEL`](../../Reference/blob/MblobGetResult.md) feature (for a grayscale definition), for a binary definition divide it by the number of pixels in the blob ([`M_AREA`](../../Reference/blob/MblobGetResult.md)).

---

### `M_MOMENT_X1_Y1`

Retrieves the ordinary moment of each blob where the order of X equals 1 and the order of Y equals 1. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: mblobselectfeature_moment_1-1.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and _y<sub>i</sub> _ = its Y-coordinate.

---

### `M_MOMENT_X1_Y2`

Retrieves the ordinary moment of each blob where the order of X equals 1 and the order of Y equals 2. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: moment_x1_y2_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and _y<sub>i</sub> _ = its Y-coordinate.

---

### `M_MOMENT_X2_Y0`

Retrieves the ordinary moment of each blob where the order of X equals 2 and the order of Y equals 0. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_SECOND_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: mblobselectfeature_moment_2-0.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments) and `_x<sub>i</sub> _` = its X-coordinate.

---

### `M_MOMENT_X2_Y1`

Retrieves the ordinary moment of each blob where the order of X equals 2 and the order of Y equals 1. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: moment_x2_y1_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments), `_x<sub>i</sub> _` = its X-coordinate and _y<sub>i</sub> _ = its Y-coordinate.

---

### `M_MOMENT_X3_Y0`

Retrieves the ordinary moment of each blob where the order of X equals 3 and the order of Y equals 0. The calculation of this feature can be enabled using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md).  This ordinary moment is defined as *[Image: moment_x3_y0_eq.png]*  , where _i_ runs from 1 to the number of pixels in the blob, where _n_ is the number of pixels in the blob, `_p<sub>i</sub> _` = value of a pixel (always 1 for binary moments) and `_x<sub>i</sub> _` = its X-coordinate.

### For retrieving results of a depth map specific feature, and that can be used as a sorting key

To retrieve the result for a depth map specific feature, and that can be used as a sorting key, select one of the following. Results for these features are only available if a grayscale buffer that is a fully corrected depth map image buffer or 3D-processable depth map container was passed to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) (and the corresponding features were selected for calculation); in addition, the blob identifier image and the confidence information of the depth map must have been merged and passed as the ROI of the grayscale buffer. To retrieve these results, you must set [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) to a specific blob or all blobs, using **M_BLOB_INDEX()** or **M_BLOB_LABEL()** for a specific blob, or [`M_INCLUDED_BLOBS`](../../Reference/blob/MblobGetResult.md) for all blobs.

---

### `M_DEPTH_MAP_MAX_ELEVATION`

Retrieves the maximum elevation of each blob.

---

### `M_DEPTH_MAP_MEAN_ELEVATION`

Retrieves the mean elevation of each blob.

---

### `M_DEPTH_MAP_MIN_ELEVATION`

Retrieves the minimum elevation of each blob.

---

### `M_DEPTH_MAP_SIZE_Z`

Retrieves the Z-size (difference between maximum and minimum elevation) of each blob.

---

### `M_DEPTH_MAP_VOLUME`

Retrieves the volume of each blob.

### Combination Constants — For retrieving Feret contact points

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to retrieve the contact points of the Feret.

The calculation of Feret contact points ([`M_FERET_CONTACT_POINTS`](../../Reference/blob/MblobControl.md)) must have been enabled to use these combination constants.

| Value | Description |
| --- | --- |
| `M_FERET_CONTACT_POINTS_X1` | Retrieves the X-coordinate for the first contact point of the Feret diameter of each blob. |
| `M_FERET_CONTACT_POINTS_X2` | Retrieves the X-coordinate for the second contact point of the Feret diameter of each blob. |
| `M_FERET_CONTACT_POINTS_Y1` | Retrieves the Y-coordinate for the first contact point of the Feret diameter of each blob. |
| `M_FERET_CONTACT_POINTS_Y2` | Retrieves the Y-coordinate for the second contact point of the Feret diameter of each blob. |

### Combination Constants — For features that have two definitions

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set whether the results should be returned for the binary or grayscale definition of the selected feature.

| Value | Description |
| --- | --- |
| `M_BINARY` | Retrieves the result for the binary definition of the selected feature. |
| `M_GRAYSCALE` *(default)* | Retrieves the result for the grayscale definition of the selected feature. |

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying a data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the requested results to a data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT16`

Casts the requested results to an _AIL_INT16_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

If you requested to retrieve results in world units with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) and the calculation was performed on appropriate buffers, the results returned are transformed into world units, taking into account distortions as best as possible. If you require precise results, you should correct the image using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) before calling [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

If you requested to retrieve results in world units with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) and the calculation was performed on appropriate buffers, the results returned are transformed into the relative coordinate system, taking into account distortions as best as possible. If you require precise results, you should correct the image using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) before calling [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

Even if you requested to retrieve results in world units with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md), this result is calculated and returned in the pixel coordinate system.

Results are only available in pixel units.

The [`M_WORLD_BOX...`](../../Reference/blob/MblobGetResult.md) features are actually calculated in the world coordinate system. If these features were enabled for calculation in [`MblobControl`](../../Reference/blob/MblobControl.md), you should have passed [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) a calibrated blob identifier image. If an uncalibrated image was passed to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), these features are the equivalent to the corresponding [`M_BOX_...`](../../Reference/blob/MblobGetResult.md) features.

The [`M_WORLD_FERET...`](../../Reference/blob/MblobGetResult.md) features are actually calculated in the world coordinate system. If these features were enabled for calculation in [`MblobControl`](../../Reference/blob/MblobControl.md), you should have passed [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) a calibrated blob identifier image. If an uncalibrated image was passed to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), these features are the equivalent to the corresponding [`M_FERET_...`](../../Reference/blob/MblobGetResult.md) features.

The image-axis-aligned bounding box is the bounding box that is aligned with the pixel coordinate system's axes.

The world-axis-aligned bounding box is the bounding box that is aligned with the relative coordinate system's axes.

This result is not affected by the setting of [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md).

For central moments, coordinates are relative to each blob's center of gravity, as opposed to ordinary moments, which use coordinates that are relative to the image origin (top-left corner).

For ordinary moments, coordinates are relative to the image origin (top-left corner), as opposed to central moments, which use coordinates that are relative to each blob's center of gravity.

An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).
