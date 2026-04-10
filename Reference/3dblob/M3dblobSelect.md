---
doctype: Reference
module: 3dblob
function: M3dblobSelect
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobSelect"
---

# M3dblobSelect

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

> Select which blobs meet a criterion and set the destination result buffer to recognize only these as blobs; corresponding blob identifying information and feature results are copied to the specified destination result buffer.

## Syntax

```c
void M3dblobSelect(
    AIL_ID     SrcResult3dblobId,   //in
    AIL_ID     DstResult3dblobId,   //out
    AIL_INT64  SelectionCriterion,  //in
    AIL_INT64  Condition,           //in
    AIL_DOUBLE CondLow,             //in
    AIL_DOUBLE CondHigh,            //in
    AIL_INT64  ControlFlag          //in
)
```

## Description

This function selects which blobs meet a criterion and sets the destination result buffer to recognize only these as blobs; corresponding blob identifying information and feature results are copied to the specified destination result buffer.

To perform a compound selection (criterion1 AND/OR criterion2), call this function twice using two separate destination result buffers. Select criterion1 with the first call, and then select criterion2 with the second call. Then, pass the two result buffers to [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) to combine the selections into a single result buffer that holds only the required blobs' data (based on the specified [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) operation).

This function supports in-place processing; you can specify the identifier of the same buffer for the source and the destination. Note that non-selected blob identifying information and feature results are deleted when using in-place processing.

## Parameters

### `SrcResult3dblobId` *(in, AIL_ID)*

Specifies the identifier of the source 3D blob analysis result buffer from which to select blob data. This is the result buffer that stores the blob segmentation and any calculated feature results.

### `DstResult3dblobId` *(out, AIL_ID)*

Specifies the identifier of the destination 3D blob analysis result buffer into which to copy the selected blob data. In-place processing is supported; [`DstResult3dblobId`](../../Reference/3dblob/M3dblobSelect.md) can refer to the same result buffer as [`SrcResult3dblobId`](../../Reference/3dblob/M3dblobSelect.md).

### `SelectionCriterion` *(in, AIL_INT64)*

Specifies the feature upon which to base the selection. This can be any of the following features; these features correspond to any non-array feature available using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) that is also retrievable using a specific blob label.

*For specifying the feature*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no blob touches a side. |
| `M_BOTTOM` | Specifies that the blob touches the bottom of the container or label image. |
| `M_LEFT` | Specifies that the blob touches the left of the container or label image. |
| `M_RIGHT` | Specifies that the blob touches the right of the container or label image. |
| `M_TOP` | Specifies that the blob touches the top of the container or label image. |
| `M_INVALID` | Specifies an invalid index. |
| `0 <= Value < M_NUMBER` | Specifies the index of the blob. |
| `0 < Value <= M_MAX_LABEL_VALUE` | Specifies the label of the blob. |
| `0 <= Value < 1024` | Specifies the valid index of the custom feature. |
| `Value >= 0` | Specifies the power. |
| `Value >= 0` | Specifies the power. |
| `Value >= 0` | Specifies the power. |
| `Value >= 0` | Specifies the power. |
| `Value >= 0` | Specifies the power. |
| `Value >= 0` | Specifies the power. |
| `0.0 <= Value <= 1.0` | Specifies the degree to which the blob is linear. |
| `0.0 <= Value <= 1.0` | Specifies the degree to which the blob is planar. |
| `0.0 <= Value < 180.0` | Specifies the angle of the semi-oriented bounding box. |
| `M_CUSTOM_FEATURE` | Uses the computed values of the specified custom feature that was manually added to the source result buffer using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_CUSTOM_FEATURE()`](../../Reference/3dblob/M3dblobControl.md). This feature was not calculated using [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md). |
| `M_MOMENT_CENTRAL_XYZ` | Uses the central moment corresponding to the specified powers of X, Y, and Z. This is equivalent to the following formula, where _N_ is the number of points in the blob, _µ_ is the centroid, and_(x<sub>i</sub> _,_y<sub>i</sub> _,_z<sub>i</sub>)_is the _i_ <sup>th</sup> point:

*[Image: 3dim_MomentFormula_Central_PCandDM.png]* |
| `M_MOMENT_XYZ` | Uses the (ordinary, non-central) moment corresponding to the specified powers of X, Y, and Z. This is equivalent to the following formula, where _N_ is the number of points in the blob and_(x<sub>i</sub> _,_y<sub>i</sub> _,_z<sub>i</sub>)_is the _i_ <sup>th</sup> point:

*[Image: 3dim_MomentFormula_NonCentral_PCandDM.png]* |
| `M_BOX_CENTER_X` | Uses the X-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSelect.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSelect.md). |
| `M_BOX_CENTER_Y` | Uses the Y-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSelect.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSelect.md). |
| `M_BOX_CENTER_Z` | Uses the Z-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSelect.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSelect.md). |
| `M_FERET_AT_PRINCIPAL_AXIS_1` | Uses the length of the Feret along the first principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_2` | Uses the length of the Feret along the second principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_3` | Uses the length of the Feret along the third principal axis. |
| `M_FERET_GENERAL` | Uses the length of the Feret in the custom direction that was specified using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobControl.md). |
| `M_FERET_MAX_DIAMETER` | Uses the length of the longest Feret in the blob. |
| `M_FERET_MIN_DIAMETER` | Uses the length of the shortest Feret in the blob. |
| `M_FERET_SEMI_ORIENTED_HEIGHT` | Uses the height (smallest XY side) of the semi-oriented box. |
| `M_FERET_SEMI_ORIENTED_WIDTH` | Uses the width (longest XY side) of the semi-oriented box. |
| `M_FERET_X` | Uses the length of the X-aligned Feret (same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobSelect.md)). |
| `M_FERET_Y` | Uses the length of the Y-aligned Feret (same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobSelect.md)). |
| `M_FERET_Z` | Uses the length of the Z-aligned Feret (same as [`M_SIZE_Z`](../../Reference/3dblob/M3dblobSelect.md)). |
| `M_NEAREST_BLOB` | Uses the label of the nearest blob. |
| `M_SIZE_X` | Uses the size in X of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSelect.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSelect.md). |
| `M_SIZE_Y` | Uses the size in Y of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSelect.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSelect.md). |
| `M_SIZE_Z` | Uses the size in Z of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSelect.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSelect.md). |

*For specifying the type of bounding box*

| Value | Description |
| --- | --- |
| `M_PCA_BOX` | Specifies to use results associated with the PCA bounding box. |
| `M_SEMI_ORIENTED_BOX` | Specifies to use results associated with the semi-oriented bounding box. |

*For using Feret contact points*

| Value | Description |
| --- | --- |
| `M_FERET_CONTACT_POINTS_X1` | Uses the X-coordinate of the start point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_X2` | Uses the X-coordinate of the end point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Y1` | Uses the Y-coordinate of the start point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Y2` | Uses the Y-coordinate of the end point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Z1` | Uses the Z-coordinate of the start point of the blob's Feret diameter. |
| `M_FERET_CONTACT_POINTS_Z2` | Uses the Z-coordinate of the end point of the blob's Feret diameter. |

*For using the Feret direction*

| Value | Description |
| --- | --- |
| `M_FERET_DIRECTION_X` | Uses the X-component of the Feret's direction vector. For example, the direction of [`M_FERET_X`](../../Reference/3dblob/M3dblobSelect.md) is (1, 0, 0). |
| `M_FERET_DIRECTION_Y` | Uses the Y-component of the Feret's direction vector. For example, the direction of [`M_FERET_Y`](../../Reference/3dblob/M3dblobSelect.md) is (0, 1, 0). |
| `M_FERET_DIRECTION_Z` | Uses the Z-component of the Feret's direction vector. For example, the direction of [`M_FERET_Z`](../../Reference/3dblob/M3dblobSelect.md) is (0, 0, 1). |

*For using the closest points between neighboring blobs*

| Value | Description |
| --- | --- |
| `M_NEAREST_POINT_X1` | Uses the X-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_X2` | Uses the X-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |
| `M_NEAREST_POINT_Y1` | Uses the Y-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_Y2` | Uses the Y-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |
| `M_NEAREST_POINT_Z1` | Uses the Z-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_Z2` | Uses the Z-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |

### `Condition` *(in, AIL_INT64)*

Specifies the condition that the specified [`SelectionCriterion`](../../Reference/3dblob/M3dblobSelect.md) must satisfy.

*For selection using a feature result's availability*

| Value | Description |
| --- | --- |
| `M_AVAILABLE` | Specifies to select for feature results that are available in the source result buffer. |
| `M_NOT_AVAILABLE` | Specifies to select for feature results that are not available in the source result buffer. |

*For selection using two limits*

| Value | Description |
| --- | --- |
| `M_IN_RANGE` | Specifies to base the selection on the specified feature values that are in the range [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) to [`CondHigh`](../../Reference/3dblob/M3dblobSelect.md), inclusive. |
| `M_OUT_RANGE` | Specifies to base the selection on the specified feature values that are less than [`CondLow`](../../Reference/3dblob/M3dblobSelect.md), or greater than [`CondHigh`](../../Reference/3dblob/M3dblobSelect.md). |

*For selection using only one limit*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Specifies to base the selection on the specified feature values that are equal to [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) . |
| `M_GREATER` | Specifies to base the selection on the specified feature values that are greater than [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) . |
| `M_GREATER_OR_EQUAL` | Specifies to base the selection on the specified feature values that are greater than or equal to [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) . |
| `M_LESS` | Specifies to base the selection on the specified feature values that are less than [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) . |
| `M_LESS_OR_EQUAL` | Specifies to base the selection on the specified feature values that are less than or equal to [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) . |
| `M_NOT_EQUAL` | Specifies to base the selection on the specified feature values that are not equal to [`CondLow`](../../Reference/3dblob/M3dblobSelect.md) . |

### `CondLow` *(in, AIL_DOUBLE)*

Specifies the lower limit of the selected condition.

*For specifying the lower limit*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no lower limit is required. This setting should be selected when [`Condition`](../../Reference/3dblob/M3dblobSelect.md) is set to [`M_AVAILABLE`](../../Reference/3dblob/M3dblobSelect.md) or [`M_NOT_AVAILABLE`](../../Reference/3dblob/M3dblobSelect.md). |
| `Value` | Specifies the lower limit. |

### `CondHigh` *(in, AIL_DOUBLE)*

Specifies the upper limit of the selected condition.

*For specifying the upper limit*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no upper limit is required. This setting should be selected for conditions that use only one limit, or when [`Condition`](../../Reference/3dblob/M3dblobSelect.md) is set to [`M_AVAILABLE`](../../Reference/3dblob/M3dblobSelect.md) or [`M_NOT_AVAILABLE`](../../Reference/3dblob/M3dblobSelect.md). |
| `Value` | Specifies the upper limit of the condition. This setting is required only when [`Condition`](../../Reference/3dblob/M3dblobSelect.md) is set to [`M_IN_RANGE`](../../Reference/3dblob/M3dblobSelect.md) or [`M_OUT_RANGE`](../../Reference/3dblob/M3dblobSelect.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
