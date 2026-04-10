---
doctype: Reference
module: blob
function: MblobSelect
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / blob / MblobSelect"
---

# MblobSelect

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

> Select blobs for calculations and result retrieval.

## Syntax

```c
void MblobSelect(
    AIL_ID     ResultBlobId,        //out
    AIL_INT64  Operation,           //in
    AIL_INT64  SelectionCriterion,  //in
    AIL_INT64  Condition,           //in
    AIL_DOUBLE CondLow,             //in
    AIL_DOUBLE CondHigh             //in
)
```

## Description

This function selects or merges blobs that meet a specified criterion. These blobs will be included in or excluded from future operations (calculations or result retrieval), or deleted entirely from the result buffer. Selection criterion can be based on a calculated feature or on the current status of the blobs. [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) must have been called at least once before calling this function.

If this function is not called at least once, all blobs are included by default. If there is more than one call to this function, the effect of the calls is cumulative unless [`M_INCLUDE_ONLY`](../../Reference/blob/MblobSelect.md) or [`M_EXCLUDE_ONLY`](../../Reference/blob/MblobSelect.md) is specified as the operation to perform.

If a blob is excluded or deleted, its index is reassigned; that is, the index of blobs with indices greater than that of the excluded/deleted blob are reduced by one. This ensures that included blobs have consecutive indices. Label values of included and excluded blobs, however, are left as is; therefore, it is possible to specify the label value of an excluded blob. You can get a list of valid blob label values by calling [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_LABEL_VALUE`](../../Reference/blob/MblobGetResult.md). Deleted blobs' label values are set to 0.

Once a blob has been excluded, it can normally be re-included only by specifying [`M_INCLUDE`](../../Reference/blob/MblobSelect.md) or [`M_INCLUDE_ONLY`](../../Reference/blob/MblobSelect.md) in a future call to this function (with the correct criterion). However, if you change the processing mode of a Blob Analysis context (using [`MblobControl`](../../Reference/blob/MblobControl.md)), or use the result buffer with different images, the subsequent call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) will cause all results in the buffer to be discarded and all blobs will be re-included.

The limits of the blob selection criterion are set using the [`CondLow`](../../Reference/blob/MblobSelect.md) and [`CondHigh`](../../Reference/blob/MblobSelect.md) parameters. If the blobs were taken from a calibrated image, these parameters can be set in pixel units or world units. To set the units, use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). Note that if you set this control type to [`M_WORLD`](../../Reference/blob/MblobControl.md) but you don't pass [`MblobSelect`](../../Reference/blob/MblobSelect.md) a result buffer whose results were obtained from a calibrated target image, the function will generate an error.

## Parameters

### `ResultBlobId` *(out, AIL_ID)*

Specifies the identifier of the blob analysis result buffer to use in the blob selection process.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform on the specified blobs. This parameter can be set to one of the following.

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_DELETE` | Deletes blobs that meet the specified condition. [`M_DELETE`](../../Reference/blob/MblobSelect.md) affects only included blobs, unless otherwise stated by the [`SelectionCriterion`](../../Reference/blob/MblobSelect.md) parameter.

[`M_DELETE`](../../Reference/blob/MblobSelect.md) removes blobs permanently from the result buffer and, consequently, prevents these blobs from being re-included. |
| `M_EXCLUDE` | Excludes all blobs that meet the specified condition.

[`M_EXCLUDE`](../../Reference/blob/MblobSelect.md) affects only the status of currently included blobs. |
| `M_EXCLUDE_ONLY` | Excludes only those blobs that meet the specified condition and includes all others.

The exclusion does not consider the present status of blobs (whether they are excluded), except for blobs that have been deleted ([`M_DELETE`](../../Reference/blob/MblobSelect.md)), unless otherwise stated by the [`SelectionCriterion`](../../Reference/blob/MblobSelect.md) parameter. |
| `M_INCLUDE` | Includes all blobs that meet the specified condition.

[`M_INCLUDE`](../../Reference/blob/MblobSelect.md) affects only the status of currently excluded blobs. |
| `M_INCLUDE_ONLY` | Includes only those blobs that meet the specified condition and excludes all others.

The inclusion does not consider the present status of blobs (whether they are included), except for blobs that have been deleted ([`M_DELETE`](../../Reference/blob/MblobSelect.md)), unless otherwise stated by the [`SelectionCriterion`](../../Reference/blob/MblobSelect.md) parameter. |
| `M_MERGE` | Groups all the blobs that meet the specified condition.

Once grouped, the blobs are treated as one blob (a grouped blob). The grouped blob is assigned a unique blob label. Only features that were calculated for all the individual blobs in the group are re-calculated for the new grouped blob; calculations are made using the results of the individual blobs in the group.

Note that if the source results include user-specified moments (added using[`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md)) or third-order moments calculated in precise mode (added using [`MblobControl`](../../Reference/blob/MblobControl.md) with[`M_MOMENT_THIRD_ORDER`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md), and [`M_MOMENT_THIRD_ORDER_MODE`](../../Reference/blob/MblobControl.md) set to [`M_PRECISE`](../../Reference/blob/MblobControl.md)) [`M_MERGE`](../../Reference/blob/MblobSelect.md) will only be able to merge the moments if they were calculated using their binary definition and [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) was enabled. If they were calculated using their grayscale definition or [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) was not enabled, the moments will not be merged.

Except when using [`M_EXCLUDED_BLOBS`](../../Reference/blob/MblobSelect.md) as the feature for selection, this grouping operation only applies to included blobs.

If you are using [`M_MERGE`](../../Reference/blob/MblobSelect.md), you cannot use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md). |

### `SelectionCriterion` *(in, AIL_INT64)*

Specifies the feature to use as part of the selection criterion or the status of the blobs to affect. See [`MblobControl`](../../Reference/blob/MblobControl.md) for a full description of the features. The specified result buffer must already contain the results for the specified feature.

*For a selection criterion based on a feature that has only a binary definition*

| Value | Description |
| --- | --- |
| `M_AREA` | Uses the number of foreground pixels in a blob (holes are not counted). |
| `M_BLOB_TOUCHING_IMAGE_BORDERS` | Uses all blobs that touched the border of the blob identifier image. |
| `M_BOX_AREA` | Uses the area covered by the bounding box of a blob. |
| `M_BOX_ASPECT_RATIO` | Uses the ratio of the horizontal size to the vertical size of the bounding box of a blob. |
| `M_BOX_FILL_RATIO` | Uses the ratio of the area of a blob to the area of the bounding box of the blob. |
| `M_BOX_X_MAX` | Uses the extreme right coordinate of the bounding box of a blob. |
| `M_BOX_X_MIN` | Uses the extreme left coordinate of the bounding box of a blob. |
| `M_BOX_Y_MAX` | Uses the extreme bottom coordinate of the bounding box of a blob. |
| `M_BOX_Y_MIN` | Uses the extreme top coordinate of the bounding box of a blob. |
| `M_BREADTH` | Uses the breadth of a blob. |
| `M_COMPACTNESS` | Uses the compactness feature. |
| `M_CONVEX_HULL_AREA` | Uses the area of the convex hull of a blob. |
| `M_CONVEX_HULL_COG_X` | Uses the X-component of the center of gravity of a blob. |
| `M_CONVEX_HULL_COG_Y` | Uses the Y-component of the center of gravity of a blob. |
| `M_CONVEX_HULL_FILL_RATIO` | Uses the ratio of the area of a blob to the area of its convex hull. |
| `M_CONVEX_HULL_PERIMETER` | Uses the perimeter of the convex hull of a blob. |
| `M_CONVEX_PERIMETER` | Uses the approximation of the perimeter of the convex hull of a blob. |
| `M_ELONGATION` | Uses the elongation of a blob. |
| `M_EULER_NUMBER` | Uses the number of blobs - number of holes. |
| `M_FERET_ELONGATION` | Uses the measure of the shape of a blob. It is equal to [`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md) / [`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md). |
| `M_FERET_GENERAL` | Uses the Feret diameter feature. |
| `M_FERET_MAX_ANGLE` | Uses the angle at which the maximum Feret diameter is found, in degrees, relative to the input coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). |
| `M_FERET_MAX_DIAMETER` | Uses the largest Feret diameter found after checking a certain number of angles. |
| `M_FERET_MAX_DIAMETER_ELONGATION` | Uses the ratio of the maximum Feret diameter by its perpendicular Feret diameter. |
| `M_FERET_MEAN_DIAMETER` | Uses the average Feret diameter at all the angles checked. |
| `M_FERET_MIN_ANGLE` | Uses the angle at which the minimum Feret diameter is found, in degrees, relative to the input coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). |
| `M_FERET_MIN_DIAMETER` | Uses the smallest Feret diameter found after checking a certain number of angles. |
| `M_FERET_MIN_DIAMETER_ELONGATION` | Uses the ratio of the minimum Feret diameter by its perpendicular Feret diameter. |
| `M_FERET_PERPENDICULAR_TO_MAX_DIAMETER` | Uses the Feret diameter that is perpendicular to the maximum Feret diameter. |
| `M_FERET_PERPENDICULAR_TO_MIN_DIAMETER` | Uses the Feret diameter that is perpendicular to the minimum Feret diameter. |
| `M_FERET_X` | Uses the dimension of the minimum bounding box of a blob in the horizontal direction. |
| `M_FERET_Y` | Uses the dimension of the minimum bounding box of a blob in the vertical direction. |
| `M_FIRST_POINT_X` | Uses the x-coordinate of the top left-most pixel along the perimeter of a blob. |
| `M_FIRST_POINT_Y` | Uses the y-coordinate of the top left-most pixel along the perimeter of a blob. |
| `M_INDEX_VALUE` | Uses the index value. |
| `M_INTERCEPT_0` | Uses the number of times a transition from background to foreground (not vice versa) occurs in the horizontal direction for the entire blob. In other words, it is equal to the number of times the neighborhood configuration [_B_, _F_] occurs in a blob, where _B_ is a background pixel and _F_ is a foreground pixel. |
| `M_INTERCEPT_45` | Determines the number of times that the neighborhood configuration *[Image: mblobselectfeature_in45.png]*

 occurs in a blob, where _F_ is a foreground pixel, _B_ is a background pixel, and a dot can be any pixel value. |
| `M_INTERCEPT_90` | Determines the number of times that the neighborhood configuration *[Image: mblobselectfeature_in90.png]*

 occurs in a blob. |
| `M_INTERCEPT_135` | Determines the number of times that the neighborhood configuration *[Image: mblobselectfeature_in135.png]*

 occurs in a blob. |
| `M_LABEL_VALUE` | Uses the label feature. |
| `M_LENGTH` | Uses the measure of the true length of a blob. |
| `M_MIN_AREA_BOX_ANGLE` | Uses the angle of the minimum-area bounding box. |
| `M_MIN_AREA_BOX_AREA` | Uses the area of the minimum-area bounding box. |
| `M_MIN_AREA_BOX_CENTER_X` | Uses the X-coordinate of the center of the minimum-area bounding box. |
| `M_MIN_AREA_BOX_CENTER_Y` | Uses the Y-coordinate of the center of the minimum-area bounding box. |
| `M_MIN_AREA_BOX_HEIGHT` | Uses the height of the minimum-area bounding box. |
| `M_MIN_AREA_BOX_PERIMETER` | Uses the perimeter of the minimum-area bounding box. |
| `M_MIN_AREA_BOX_WIDTH` | Uses the width of the minimum-area bounding box. |
| `M_MIN_PERIMETER_BOX_ANGLE` | Uses the angle of the minimum-perimeter bounding box. |
| `M_MIN_PERIMETER_BOX_AREA` | Uses the area of the minimum-perimeter bounding box. |
| `M_MIN_PERIMETER_BOX_CENTER_X` | Uses the X-coordinate of the center of the minimum-perimeter bounding box. |
| `M_MIN_PERIMETER_BOX_CENTER_Y` | Uses the Y-coordinate of the center of the minimum-perimeter bounding box. |
| `M_MIN_PERIMETER_BOX_HEIGHT` | Uses the height of the minimum-perimeter bounding box. |
| `M_MIN_PERIMETER_BOX_PERIMETER` | Uses the perimeter of the minimum-perimeter bounding box. |
| `M_MIN_PERIMETER_BOX_WIDTH` | Uses the width of the minimum-perimeter bounding box. |
| `M_MOMENT_GENERAL` | Uses the moment calculation. |
| `M_NUMBER_OF_CHAINED_PIXELS` | Uses the number of chained pixels in a blob. |
| `M_NUMBER_OF_HOLES` | Uses the number of holes in a blob. |
| `M_NUMBER_OF_RUNS` | Uses the total number of runs in a blob. |
| `M_PERIMETER` | Uses the total length of edges in a blob (including the edges of any holes), with an allowance made for the staircase effect that is produced when diagonal edges are digitized. |
| `M_RECTANGULARITY` | Uses the degree to which a blob is similar to a rectangle. To do this, [`MblobSelect`](../../Reference/blob/MblobSelect.md) calculates the ratio of the blob's area to the product of its minimum Feret diameter and the Feret diameter perpendicular to the minimum Feret diameter. |
| `M_ROUGHNESS` | Uses the roughness feature. |
| `M_WORLD_BOX_X_MAX` | Uses the extreme right X-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_BOX_X_MIN` | Uses the extreme left X-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_BOX_Y_MAX` | Uses the extreme bottom Y-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_BOX_Y_MIN` | Uses the extreme top Y-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_FERET_X` | Uses the dimension of the minimum bounding box of a blob in the horizontal direction, calculated in the relative coordinate system (that is, [`M_WORLD_BOX_X_MAX`](../../Reference/blob/MblobGetResult.md) - [`M_WORLD_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md) + 1). |
| `M_WORLD_FERET_Y` | Uses the dimension of the minimum bounding box of a blob in the vertical direction, calculated in the relative coordinate system (that is, [`M_WORLD_BOX_Y_MAX`](../../Reference/blob/MblobGetResult.md) - [`M_WORLD_BOX_Y_MIN`](../../Reference/blob/MblobGetResult.md) + 1). |
| `M_WORLD_X_AT_Y_MAX` | Uses the X-coordinate at the maximum Y-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_X_AT_Y_MIN` | Uses the X-coordinate at the minimum Y-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_Y_AT_X_MAX` | Uses the Y-coordinate at the maximum X-coordinate of a blob, calculated in the relative coordinate system. |
| `M_WORLD_Y_AT_X_MIN` | Uses the Y-coordinate at the minimum X-coordinate of a blob, calculated in the relative coordinate system. |
| `M_X_MAX_AT_Y_MAX` | Uses the maximum X-coordinate at the maximum Y-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_X_MAX_AT_Y_MIN` | Uses the maximum X-coordinate at the minimum Y-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_X_MIN_AT_Y_MAX` | Uses the minimum X-coordinate at the maximum Y-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_X_MIN_AT_Y_MIN` | Uses the minimum X-coordinate at the minimum Y-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_Y_MAX_AT_X_MAX` | Uses the maximum Y-coordinate at the maximum X-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_Y_MAX_AT_X_MIN` | Uses the maximum Y-coordinate at the minimum X-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_Y_MIN_AT_X_MAX` | Uses the minimum Y-coordinate at the maximum X-coordinate of the blob, calculated in the pixel coordinate system. |
| `M_Y_MIN_AT_X_MIN` | Uses the minimum Y-coordinate at the minimum X-coordinate of the blob, calculated in the pixel coordinate system. |

*For a selection criterion based on a feature that has only a grayscale definition*

| Value | Description |
| --- | --- |
| `M_BLOB_CONTRAST` | Uses the difference between the maximum and minimum pixel values of a blob. |
| `M_DEPTH_MAP_MAX_ELEVATION` | Uses the feature specifying the maximum elevation of a blob. |
| `M_DEPTH_MAP_MEAN_ELEVATION` | Uses the feature specifying the mean elevation of a blob. |
| `M_DEPTH_MAP_MIN_ELEVATION` | Uses the feature specifying the minimum elevation of a blob. |
| `M_DEPTH_MAP_SIZE_Z` | Uses the feature specifying the Z-size (difference between maximum and minimum elevation) of a blob. |
| `M_DEPTH_MAP_VOLUME` | Uses the feature specifying the volume of a blob. |
| `M_MAX_PIXEL` | Uses the maximum pixel value in a blob. |
| `M_MEAN_PIXEL` | Uses the mean pixel value in a blob. |
| `M_MIN_PIXEL` | Uses the minimum pixel value in a blob. |
| `M_SIGMA_PIXEL` | Uses the standard deviation of pixel values in a blob. |
| `M_SUM_PIXEL` | Uses the sum of all pixel values in a blob. |
| `M_SUM_PIXEL_SQUARED` | Uses the sum of the squares of the pixel values in a blob. |

*For a selection criterion based on a feature that has two different definitions*

| Value | Description |
| --- | --- |
| `M_AXIS_PRINCIPAL_ANGLE` | Uses the feature specifying the angle, in degrees, at which a blob has the least moment of inertia, relative to the input coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). |
| `M_AXIS_SECONDARY_ANGLE` | Uses the feature specifying the angle, in degrees, perpendicular to [`M_AXIS_PRINCIPAL_ANGLE`](../../Reference/blob/MblobGetResult.md), relative to the input coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). |
| `M_CENTER_OF_GRAVITY_X` | Uses the feature specifying the X-position of the center of gravity of a blob. |
| `M_CENTER_OF_GRAVITY_Y` | Uses the feature specifying the Y-position of the center of gravity of a blob. |
| `M_FERET_AT_PRINCIPAL_AXIS_ANGLE` | Uses the Feret diameter at the principal axis of a blob.

The principal axis is the axis at which the blob has the least moment of inertia. Also, if the blob is symmetrical, the principal axis is aligned with the blob's axis of symmetry. |
| `M_FERET_AT_SECONDARY_AXIS_ANGLE` | Uses the Feret diameter at the secondary axis of a blob.

The secondary axis is perpendicular to the principal axis. |
| `M_FERET_PRINCIPAL_AXIS_ELONGATION` | Uses the ratio of the Feret diameter at the principal axis to the Feret diameter at the secondary axis. |
| `M_MOMENT_CENTRAL_X0_Y2` | Uses the feature specifying the central moment of a blob, where the order of X equals 0 and the order of Y equals 2. |
| `M_MOMENT_CENTRAL_X0_Y3` | Uses the feature specifying the central moment of a blob, where the order of X equals 0 and the order of Y equals 3. |
| `M_MOMENT_CENTRAL_X1_Y1` | Uses the feature specifying the central moment of a blob, where the order of X equals 1 and the order of Y equals 1. |
| `M_MOMENT_CENTRAL_X1_Y2` | Uses the feature specifying the central moment of a blob, where the order of X equals 1 and the order of Y equals 2. |
| `M_MOMENT_CENTRAL_X2_Y0` | Uses the feature specifying the central moment of a blob, where the order of X equals 2 and the order of Y equals 0. |
| `M_MOMENT_CENTRAL_X2_Y1` | Uses the feature specifying the central moment of a blob, where the order of X equals 2 and the order of Y equals 1. |
| `M_MOMENT_CENTRAL_X3_Y0` | Uses the feature specifying the central moment of a blob, where the order of X equals 3 and the order of Y equals 0. |
| `M_MOMENT_HU_1` | Uses the feature specifying the first Hu moment of a blob. |
| `M_MOMENT_HU_2` | Uses the feature specifying the second Hu moment of a blob. |
| `M_MOMENT_HU_3` | Uses the feature specifying the third Hu moment of a blob. |
| `M_MOMENT_HU_4` | Uses the feature specifying the fourth Hu moment of a blob. |
| `M_MOMENT_HU_5` | Uses the feature specifying the fifth Hu moment of a blob. |
| `M_MOMENT_HU_6` | Uses the feature specifying the sixth Hu moment of a blob. |
| `M_MOMENT_HU_7` | Uses the feature specifying the seventh Hu moment of a blob. |
| `M_MOMENT_X0_Y1` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 0 and the order of Y equals 1. |
| `M_MOMENT_X0_Y2` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 0 and the order of Y equals 2. |
| `M_MOMENT_X0_Y3` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 0 and the order of Y equals 3. |
| `M_MOMENT_X1_Y0` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 1 and the order of Y equals 0. |
| `M_MOMENT_X1_Y1` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 1 and the order of Y equals 1. |
| `M_MOMENT_X1_Y2` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 1 and the order of Y equals 2. |
| `M_MOMENT_X2_Y0` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 2 and the order of Y equals 0. |
| `M_MOMENT_X2_Y1` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 2 and the order of Y equals 1. |
| `M_MOMENT_X3_Y0` | Uses the feature specifying the ordinary moment of a blob, where the order of X equals 3 and the order of Y equals 0. |

*For a selection criterion based on the current status of blobs*

| Value | Description |
| --- | --- |
| `M_ALL_BLOBS` | Uses all blobs. |
| `M_EXCLUDED_BLOBS` | Uses all excluded blobs. |
| `M_INCLUDED_BLOBS` | Uses all included blobs. |

*For features with a binary and grayscale definition (both have been calculated)*

| Value | Description |
| --- | --- |
| `M_BINARY` | Uses the binary version of the selected feature. |
| `M_GRAYSCALE` *(default)* | Uses the grayscale version of the selected feature. |

### `Condition` *(in, AIL_INT64)*

Specifies the condition for the blob selection. This parameter must be set to one of the values below.

*For blob selection using two limits*

| Value | Description |
| --- | --- |
| `M_IN_RANGE` | Specifies that blobs with values for the specified feature in the range [`CondLow`](../../Reference/blob/MblobSelect.md) to [`CondHigh`](../../Reference/blob/MblobSelect.md), inclusive, are included, excluded, or deleted from future operations on the specified result buffer. |
| `M_OUT_RANGE` | Specifies that blobs with values for the specified feature less than [`CondLow`](../../Reference/blob/MblobSelect.md), or greater than [`CondHigh`](../../Reference/blob/MblobSelect.md), are included, excluded, or deleted from future operations on the specified result buffer. |

*For blob selection using only one limit*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Specifies that blobs with values for the specified feature equal to [`CondLow`](../../Reference/blob/MblobSelect.md) are included, excluded, or deleted from future operations on the specified result buffer. |
| `M_GREATER` | Specifies that blobs with values for the specified feature greater than [`CondLow`](../../Reference/blob/MblobSelect.md) are included, excluded, or deleted from future operations on the specified result buffer. |
| `M_GREATER_OR_EQUAL` | Specifies that blobs with values for the specified feature greater than or equal to [`CondLow`](../../Reference/blob/MblobSelect.md) are included, excluded, or deleted from future operations on the specified result buffer. |
| `M_LESS` | Specifies that blobs with values for the specified feature less than [`CondLow`](../../Reference/blob/MblobSelect.md) are included, excluded, or deleted from future operations on the specified result buffer. |
| `M_LESS_OR_EQUAL` | Specifies that blobs with values for the specified feature less than or equal to [`CondLow`](../../Reference/blob/MblobSelect.md) are included, excluded, or deleted from future operations on the specified result buffer. |
| `M_NOT_EQUAL` | Specifies that blobs with values for the specified feature not equal to [`CondLow`](../../Reference/blob/MblobSelect.md) are included, excluded, or deleted from future operations on the specified result buffer. |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower limit of the selected condition.

*For the lower limit of the selected condition*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is not applicable. Specify this value when and only when the selection criterion is based on the status of blobs or [`M_BLOB_TOUCHING_IMAGE_BORDERS`](../../Reference/blob/MblobSelect.md). |
| `Value` | Specifies the lower limit of the condition, relative to the input coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). |

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper limit of the selected condition.

*For the upper limit of the selected condition*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is not applicable. Specify this value when: the condition uses only one limit, the selection criterion is based on the status of blobs, or the selection criterion is based on [`M_BLOB_TOUCHING_IMAGE_BORDERS`](../../Reference/blob/MblobSelect.md). |
| `Value` | Specifies the upper limit of the condition, relative to the input coordinate system specified using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md). |

An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).
