---
doctype: Reference
module: blob
function: MblobInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / blob / MblobInquire"
---

# MblobInquire

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

> Inquire about a blob analysis setting.

## Syntax

```c
AIL_INT MblobInquire(
    AIL_ID    ContextOrResultBlobId,  //in
    AIL_INT64 InquireType,            //in
    void *    UserVarPtr              //out
)
```

## Description

This function inquires about a specified setting associated with a blob analysis context or result buffer.

You can use [`MblobControl`](../../Reference/blob/MblobControl.md) to change a setting associated with a context or a result buffer.

## Parameters

### `ContextOrResultBlobId` *(in, AIL_ID)*

Specifies the identifier of a blob analysis context or result buffer.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MblobInquire`](../../Reference/blob/MblobInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring the system on which the result buffer or context is allocated

For the following inquire type, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter can specify a blob analysis result buffer or context.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the blob result buffer or context is allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For inquiring about the global processing settings of a blob analysis context that cause results to be recalculated

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter can specify a blob analysis context.

---

### `M_BLOB_IDENTIFICATION_MODE`

Inquires the blob identification mode that was selected.

| Value | Description |
| --- | --- |
| `M_LABELED` | Specifies that blobs with the same label are grouped together, and that touching blobs with different labels are also grouped together. |
| `M_LABELED_TOUCHING` | Specifies that blobs with the same label are grouped together, and that touching blobs with different labels are measured individually. |
| `M_WHOLE_IMAGE` | Specifies that all blobs are grouped together. |

---

### `M_CONNECTIVITY`

Inquires the image lattice.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_4_CONNECTED` | Specifies that each pixel has 4 neighbors. |
| `M_8_CONNECTED` *(default)* | Specifies that each pixel has 8 neighbors. |

---

### `M_FERET_ANGLE_SEARCH_END`

Inquires the upper limit of the angular search range used in the calculation of the minimum or maximum Feret diameter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 180` *(default)* | Specifies the upper limit of the angular search range. |

---

### `M_FERET_ANGLE_SEARCH_START`

Inquires the lower limit of the angular search range used in the calculation of the minimum or maximum Feret diameter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 180` *(default)* | Specifies the lower limit of the angular search range. |

---

### `M_FOREGROUND_VALUE`

Inquires the pixel values that are considered to be in the foreground.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONZERO` *(default)* | Specifies the blobs consisting of non-zero pixels. |
| `M_ZERO` | Specifies the blobs consisting of zero pixels. |

---

### `M_NUMBER_OF_FERETS`

Inquires the number of Feret angles set to calculate a Feret feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies the use of a high precision algorithm to accurately calculate Feret features (and features based on Feret features). |
| `M_MIN_FERETS` | Specifies the minimum number of Feret angles. |
| `Value > M_MIN_FERETS` *(default)* | Specifies the number of Feret angles. |
| `M_DEFAULT` | Specifies the default value; the default value is 8. |

### For inquiring about a global processing settings of a blob analysis context that do not cause results to be recalculated

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter must specify a blob analysis context.

---

### `M_FERET_CONTACT_POINTS`

Inquires whether Feret contact points will be calculated. If enabled, the [`M_FERET_CONTACT_POINTS_...`](../../Reference/blob/MblobGetResult.md) combination constants are available in [`MblobGetResult`](../../Reference/blob/MblobGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret contact points for supported features will not be calculated. |
| `M_ENABLE` | Specifies that the Feret contact points for supported features will be calculated. |

---

### `M_FERET_GENERAL_ANGLE`

Inquires the angle to use when calculating the user-specified Feret.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-360.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_IDENTIFIER_TYPE`

Inquires the values that the non-zero pixels in the image have.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BINARY` | Specifies that non-zero pixels must have the maximum value of the buffer (for example, 0xff for an 8-bit image). |
| `M_GRAYSCALE` *(default)* | Specifies that non-zero pixels can have any value. |

---

### `M_MAX_BLOBS`

Inquires the maximum number of blobs to process.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no limit on the maximum number of blobs. |
| `Value >= 0` | Specifies the maximum number of blobs. |

---

### `M_MOMENT_GENERAL_MODE`

Inquires the type of moment that will be calculated when calculating a user-specified moment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTRAL` | Specifies a central moment. |
| `M_ORDINARY` *(default)* | Specifies an ordinary moment. |

---

### `M_MOMENT_GENERAL_ORDER_X`

Inquires the order of the X-component of the specified general moment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the X-order of the moment. |

---

### `M_MOMENT_GENERAL_ORDER_Y`

Inquires the order of the Y-component of the specified general moment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the Y-order of the moment. |

---

### `M_MOMENT_THIRD_ORDER_MODE`

Inquires how third-order moments are calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST` *(default)* | Specifies that third-order moments will be calculated quickly. |
| `M_PRECISE` | Specifies that third-order moments will be calculated accurately. |

---

### `M_PIXEL_ASPECT_RATIO`

Inquires the pixel aspect ratio that you set for the image(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the pixel width/pixel height. |

---

### `M_RETURN_PARTIAL_RESULTS`

Inquires whether results from partially scanned images will be available after a stop condition is met.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to discard results of partially scanned images when processing is interrupted. |
| `M_ENABLE` | Specifies to make results of partially scanned images available when processing is interrupted. |

---

### `M_SAVE_RUNS`

Inquires whether the run information will be saved when calling [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to save run information. |
| `M_ENABLE` *(default)* | Specifies to save run information. |

---

### `M_SORTn`

Inquires the feature used as the _n_ <sup>th</sup> sorting key, where _n_ stands for an integer between 1 and 3.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the blob is not touching the borders of the image. |
| `M_YES` | Specifies that the blob is touching one or more borders of the image. |
| `M_DEFAULT` |  |
| `M_NO_SORT` *(default)* | Specifies that the sorting key is removed. |

---

### `M_SORTn_DIRECTION`

Inquires the direction in which the _n_ <sup>th</sup> sorting key was sorted, where _n_ stands for an integer between 1 and 3.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` | Specifies the feature as being sorted in descending order. |
| `M_SORT_UP` *(default)* | Specifies the feature as being sorted in ascending order. |

---

### `M_TIMEOUT`

Inquires the maximum processing time for [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that there is no maximum processing time. |
| `Value >= 0` | Specifies the maximum processing time, in msec. |

### For inquiring about features with only a binary definition

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter can specify a blob analysis context. The following group of features do not use grayscale pixel values; they are calculated using only the blob identifier image.

---

### `M_BOX`

Inquires whether image-axis-aligned bounding box features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_BREADTH`

Inquires whether the breadth of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_CHAINS`

Inquires whether chain features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_COMPACTNESS`

Inquires whether a measure of the compactness of an object will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_CONTACT_POINTS`

Inquires whether the contact features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_CONVEX_HULL`

Inquires whether all convex hull features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_CONVEX_PERIMETER`

Inquires whether the approximation of the perimeter of the convex hull of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_ELONGATION`

Inquires whether a measure of the elongation of an object will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_EULER_NUMBER`

Inquires whether the number of blobs - number of holes will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_GENERAL`

Inquires whether the user-specified Feret diameter will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_MAX_DIAMETER_ELONGATION`

Inquires whether the ratio between the maximum Feret diameter and its perpendicular Feret diameter will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_MIN_DIAMETER_ELONGATION`

Inquires whether the ratio between the minimum Feret diameter and its perpendicular Feret diameter will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_PERPENDICULAR_TO_MAX_DIAMETER`

Inquires whether the Feret diameter that is perpendicular to the maximum Feret diameter will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_PERPENDICULAR_TO_MIN_DIAMETER`

Inquires whether the Feret diameter that is perpendicular to the minimum Feret diameter will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERETS`

Inquires whether the Feret features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_INTERCEPT`

Inquires whether the intercept features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_LENGTH`

Inquires whether the length of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MIN_AREA_BOX`

Inquires whether all the minimum-area bounding box features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_MIN_PERIMETER_BOX`

Inquires whether all the minimum-perimeter bounding box features will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_NUMBER_OF_HOLES`

Inquires whether the number of holes in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_PERIMETER`

Inquires whether the total length of edges in a blob (including the edges of any holes) will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_RECTANGULARITY`

Inquires whether the degree to which a blob resembles a rectangle will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_ROUGHNESS`

Inquires if the roughness and irregularity of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_RUNS`

Inquires whether the blob run-length encoding information will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_WORLD_BOX`

Inquires whether all of the features related to the world-axis-aligned bounding box will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

### For inquiring about features with only a grayscale definition

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter can specify a blob analysis context. The following features require grayscale pixel values, and can only be calculated if you provide a grayscale buffer.

---

### `M_BLOB_CONTRAST`

Inquires whether the difference between the maximum and minimum pixel values of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MAX_PIXEL`

Inquires whether the maximum pixel value in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MEAN_PIXEL`

Inquires whether the mean pixel value in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MIN_PIXEL`

Inquires whether the minimum pixel value in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_SIGMA_PIXEL`

Inquires whether the standard deviation of pixel values in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_SUM_PIXEL`

Inquires whether the sum of all pixel values in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_SUM_PIXEL_SQUARED`

Inquires whether the sum of the squares of each pixel value in a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

### For inquiring about features with two definitions (binary and grayscale)

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter can specify a blob analysis context. The following features have two different definitions: a binary definition, where all pixels are considered equal, and a grayscale, where pixels are weighted by their value in the grayscale buffer (the grayscale version is much slower to calculate). If you do not provide a grayscale buffer, only the binary version can be calculated. If you do provide a grayscale buffer, both versions are calculated.

---

### `M_CENTER_OF_GRAVITY`

Inquires whether both X- and Y-coordinates of the center of gravity will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_FERET_AT_PRINCIPAL_AXIS_ANGLE`

Inquires whether the Feret diameter at the principal axis of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_AT_SECONDARY_AXIS_ANGLE`

Inquires whether the Feret diameter at the secondary axis of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_FERET_PRINCIPAL_AXIS_ELONGATION`

Inquires whether the ratio between the Feret diameter at the principal axis and the Feret diameter at the secondary axis will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MOMENT_FIRST_ORDER`

Inquires whether the first-order moments will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_MOMENT_GENERAL`

Inquires whether the user-specified moment will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that this feature will not be calculated. |
| `M_ENABLE` | Specifies that this feature will be calculated. |

---

### `M_MOMENT_SECOND_ORDER`

Inquires whether the second-order moments will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

---

### `M_MOMENT_THIRD_ORDER`

Inquires whether the third-order moments and Hu moment invariants will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that these features will not be calculated. |
| `M_ENABLE` | Specifies that these features will be calculated. |

### For inquiring about depth map specific features

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter can specify a blob analysis context. These blob features can only be calculated if you pass [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) a grayscale buffer that is a fully corrected depth map image buffer or 3D-processable depth map container, and has an ROI that acts as the blob identifier image as well as identifies invalid pixels. If you passed an image buffer, you can verify that it contains a fully corrected depth map using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md). You can use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with [`M_RASTERIZE_DEPTH_MAP_VALID_PIXELS`](../../Reference/buf/MbufSetRegion.md) to merge confidence information with the blob identifier image. If you passed a container, you can use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md) to ensure that the container contains a 3D-processable depth map.

---

### `M_DEPTH_MAP_MAX_ELEVATION`

Inquires whether the maximum elevation of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum elevation of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the maximum elevation of the blob will be calculated. |

---

### `M_DEPTH_MAP_MEAN_ELEVATION`

Inquires whether the mean elevation of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the mean elevation of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the mean elevation of the blob will be calculated. |

---

### `M_DEPTH_MAP_MIN_ELEVATION`

Inquires whether the minimum elevation of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum elevation of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the minimum elevation of the blob will be calculated. |

---

### `M_DEPTH_MAP_SIZE_Z`

Inquires whether the Z-size (difference between maximum and minimum elevation) of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Z-size of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the Z-size of the blob will be calculated. |

---

### `M_DEPTH_MAP_VOLUME`

Inquires whether the volume of a blob will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the volume of the blob will not be calculated. |
| `M_ENABLE` | Specifies that the volume of the blob will be calculated. |

### Combination Constants — For feature parameters that have two definitions

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set whether the results should be returned for the binary or grayscale version of the selected feature.

| Value | Description |
| --- | --- |
| `M_BINARY` | Selects the binary definition of the selected feature. |
| `M_GRAYSCALE` *(default)* | Selects the grayscale definition of the selected feature. |

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### For inquiring about a processing setting in a blob analysis result buffer

For the following inquire types, the [`ContextOrResultBlobId`](../../Reference/blob/MblobInquire.md) parameter must specify a blob analysis result buffer.

---

### `M_INDEX_MODE`

Inquires the index mode that will be used to retrieve results with [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) and to draw results with[`MblobDraw`](../../Reference/blob/MblobDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INDEX` | Specifies that the index will be passed directly to the[`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) or [`MblobDraw`](../../Reference/blob/MblobDraw.md), without any macros. |
| `M_LABEL` | Specifies that the label will be passed directly to the[`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) or [`MblobDraw`](../../Reference/blob/MblobDraw.md), without any macros. |
| `M_USE_MACRO` *(default)* | Specifies that the values passed to the [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) or [`MblobDraw`](../../Reference/blob/MblobDraw.md) can be the macros [`M_BLOB_INDEX()`](../../Reference/blob/MblobDraw.md) or [`M_BLOB_LABEL()`](../../Reference/blob/MblobDraw.md). |

---

### `M_INPUT_SELECT_UNITS`

Inquires the units in which the [`CondLow`](../../Reference/blob/MblobSelect.md) and [`CondHigh`](../../Reference/blob/MblobSelect.md) parameters of [`MblobSelect`](../../Reference/blob/MblobSelect.md) expect the limits of the blob selection condition.

| Value | Description |
| --- | --- |
| `M_PIXEL` *(default)* | Specifies that the limit values passed to [`MblobSelect`](../../Reference/blob/MblobSelect.md) be interpreted in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that the limit values passed to [`MblobSelect`](../../Reference/blob/MblobSelect.md) be interpreted in world units, with respect to the relative coordinate system. |

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
