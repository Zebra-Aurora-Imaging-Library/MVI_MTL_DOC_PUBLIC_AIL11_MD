---
doctype: Reference
module: 3dblob
function: M3dblobSort
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobSort"
---

# M3dblobSort

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

> Sort the indices assigned to blobs based on their blob features, and set the destination result buffer to recognize the blobs using these indices; corresponding blob identifying information and feature results are copied to the specified destination result buffer.

## Syntax

```c
void M3dblobSort(
    AIL_ID    SrcResult3dblobId,  //in
    AIL_ID    DstResult3dblobId,  //out
    AIL_INT64 Feature1,           //in
    AIL_INT64 Feature2,           //in
    AIL_INT64 Feature3,           //in
    AIL_INT   NbBlobsToKeep,      //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function sorts the indices assigned to blobs based on their blob features, and sets the destination result buffer to recognize the blobs using these indices; corresponding blob identifying information and feature results are copied to the specified destination result buffer. Sorting changes the blobs' indices; the labels remain unchanged. [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) must have been called at least once before calling this function.

By default, blob indices are sorted in ascending order. For example, the first index in the list is assigned to the blob whose calculated feature value is the smallest. Note that you can change the order (for example, to sort in descending order).

Note that, when passing the sorted result buffer to functions where you might specify an index (for example, [`M3dblobExtract`](../../Reference/3dblob/M3dblobExtract.md)), the blob indices will have changed because of the sort. For other functions (such as [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md)), the sorted result buffer is still compatible with other result buffers that come from the same blob segmentation ([`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md)).

This function supports in-place processing; you can specify the identifier of the same result buffer for the source and the destination.

## Parameters

### `SrcResult3dblobId` *(in, AIL_ID)*

Specifies the identifier of the source 3D blob analysis result buffer from which to sort the blob indices.

### `DstResult3dblobId` *(out, AIL_ID)*

Specifies the identifier of the destination 3D blob analysis result buffer into which to copy the sorted indices, feature results, and other blob information. In-place processing is supported; [`DstResult3dblobId`](../../Reference/3dblob/M3dblobSort.md) can refer to the same result buffer as [`SrcResult3dblobId`](../../Reference/3dblob/M3dblobSort.md).

### `Feature1` *(in, AIL_INT64)*

Specifies the first feature to use for sorting. This can be any of the following features; these features correspond to any non-array feature available using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) that is also retrievable using a specific blob label.

*For specifying the first feature*

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
| `M_BOX_CENTER_X` | Uses the X-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_BOX_CENTER_Y` | Uses the Y-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_BOX_CENTER_Z` | Uses the Z-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_FERET_AT_PRINCIPAL_AXIS_1` | Uses the length of the Feret along the first principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_2` | Uses the length of the Feret along the second principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_3` | Uses the length of the Feret along the third principal axis. |
| `M_FERET_GENERAL` | Uses the length of the Feret in the custom direction that was specified using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobControl.md). |
| `M_FERET_MAX_DIAMETER` | Uses the length of the longest Feret in the blob. |
| `M_FERET_MIN_DIAMETER` | Uses the length of the shortest Feret in the blob. |
| `M_FERET_SEMI_ORIENTED_HEIGHT` | Uses the height (smallest XY side) of the semi-oriented box. |
| `M_FERET_SEMI_ORIENTED_WIDTH` | Uses the width (longest XY side) of the semi-oriented box. |
| `M_FERET_X` | Uses the length of the X-aligned Feret (same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_FERET_Y` | Uses the length of the Y-aligned Feret (same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_FERET_Z` | Uses the length of the Z-aligned Feret (same as [`M_SIZE_Z`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_NEAREST_BLOB` | Uses the label of the nearest blob. |
| `M_SIZE_X` | Uses the size in X of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_SIZE_Y` | Uses the size in Y of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_SIZE_Z` | Uses the size in Z of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |

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
| `M_FERET_DIRECTION_X` | Uses the X-component of the Feret's direction vector. For example, the direction of [`M_FERET_X`](../../Reference/3dblob/M3dblobSort.md) is (1, 0, 0). |
| `M_FERET_DIRECTION_Y` | Uses the Y-component of the Feret's direction vector. For example, the direction of [`M_FERET_Y`](../../Reference/3dblob/M3dblobSort.md) is (0, 1, 0). |
| `M_FERET_DIRECTION_Z` | Uses the Z-component of the Feret's direction vector. For example, the direction of [`M_FERET_Z`](../../Reference/3dblob/M3dblobSort.md) is (0, 0, 1). |

*For using the closest points between neighboring blobs*

| Value | Description |
| --- | --- |
| `M_NEAREST_POINT_X1` | Uses the X-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_X2` | Uses the X-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |
| `M_NEAREST_POINT_Y1` | Uses the Y-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_Y2` | Uses the Y-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |
| `M_NEAREST_POINT_Z1` | Uses the Z-coordinate of the point inside the current blob that is closest to the nearest neighboring blob. |
| `M_NEAREST_POINT_Z2` | Uses the Z-coordinate of the point inside the nearest neighboring blob that is closest to the current blob. |

*For specifying the sorting order*

| Value | Description |
| --- | --- |
| `M_DOWN` | Specifies to sort blob indices in descending order, from the largest calculated feature value to the smallest. |
| `M_UP` *(default)* | Specifies to sort blob indices in ascending order, from the smallest calculated feature value to the largest. |

### `Feature2` *(in, AIL_INT64)*

Specifies the second feature to use for sorting, when the comparison using [`Feature1`](../../Reference/3dblob/M3dblobSort.md) is indeterminate. This feature can be any of the following.

*For specifying the second feature*

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
| `M_BOX_CENTER_X` | Uses the X-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_BOX_CENTER_Y` | Uses the Y-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_BOX_CENTER_Z` | Uses the Z-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_FERET_AT_PRINCIPAL_AXIS_1` | Uses the length of the Feret along the first principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_2` | Uses the length of the Feret along the second principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_3` | Uses the length of the Feret along the third principal axis. |
| `M_FERET_GENERAL` | Uses the length of the Feret in the custom direction that was specified using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobControl.md). |
| `M_FERET_MAX_DIAMETER` | Uses the length of the longest Feret in the blob. |
| `M_FERET_MIN_DIAMETER` | Uses the length of the shortest Feret in the blob. |
| `M_FERET_SEMI_ORIENTED_HEIGHT` | Uses the height (smallest XY side) of the semi-oriented box. |
| `M_FERET_SEMI_ORIENTED_WIDTH` | Uses the width (longest XY side) of the semi-oriented box. |
| `M_FERET_X` | Uses the length of the X-aligned Feret (same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_FERET_Y` | Uses the length of the Y-aligned Feret (same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_FERET_Z` | Uses the length of the Z-aligned Feret (same as [`M_SIZE_Z`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_NEAREST_BLOB` | Uses the label of the nearest blob. |
| `M_SIZE_X` | Uses the size in X of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_SIZE_Y` | Uses the size in Y of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_SIZE_Z` | Uses the size in Z of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |

*For specifying the type of bounding box for the second feature*

| Value | Description |
| --- | --- |
| `M_PCA_BOX` | Specifies to use results associated with the PCA bounding box. |
| `M_SEMI_ORIENTED_BOX` | Specifies to use results associated with the semi-oriented bounding box. |

*For using Feret contact points for the second feature*

| Value | Description |
| --- | --- |
| *(see `FeretContactPoints`)* |  |

*For using the Feret direction for the second feature*

| Value | Description |
| --- | --- |
| *(see `FeretDirection`)* |  |

*For using the closest points between neighboring blobs for the second feature*

| Value | Description |
| --- | --- |
| *(see `NearestPoint`)* |  |

*For specifying the sorting order for the second feature*

| Value | Description |
| --- | --- |
| `M_DOWN` | Specifies to sort blob indices in descending order, from the largest calculated feature value to the smallest. |
| `M_UP` | Specifies to sort blob indices in ascending order, from the smallest calculated feature value to the largest. |

### `Feature3` *(in, AIL_INT64)*

Specifies the third feature to use for sorting, when the comparisons using [`Feature1`](../../Reference/3dblob/M3dblobSort.md) and [`Feature2`](../../Reference/3dblob/M3dblobSort.md) are indeterminate. This feature can be any of the following.

*For specifying the third feature*

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
| `M_BOX_CENTER_X` | Uses the X-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_BOX_CENTER_Y` | Uses the Y-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_BOX_CENTER_Z` | Uses the Z-coordinate of the center of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination value [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_FERET_AT_PRINCIPAL_AXIS_1` | Uses the length of the Feret along the first principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_2` | Uses the length of the Feret along the second principal axis. |
| `M_FERET_AT_PRINCIPAL_AXIS_3` | Uses the length of the Feret along the third principal axis. |
| `M_FERET_GENERAL` | Uses the length of the Feret in the custom direction that was specified using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobControl.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobControl.md). |
| `M_FERET_MAX_DIAMETER` | Uses the length of the longest Feret in the blob. |
| `M_FERET_MIN_DIAMETER` | Uses the length of the shortest Feret in the blob. |
| `M_FERET_SEMI_ORIENTED_HEIGHT` | Uses the height (smallest XY side) of the semi-oriented box. |
| `M_FERET_SEMI_ORIENTED_WIDTH` | Uses the width (longest XY side) of the semi-oriented box. |
| `M_FERET_X` | Uses the length of the X-aligned Feret (same as [`M_SIZE_X`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_FERET_Y` | Uses the length of the Y-aligned Feret (same as [`M_SIZE_Y`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_FERET_Z` | Uses the length of the Z-aligned Feret (same as [`M_SIZE_Z`](../../Reference/3dblob/M3dblobSort.md)). |
| `M_NEAREST_BLOB` | Uses the label of the nearest blob. |
| `M_SIZE_X` | Uses the size in X of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_SIZE_Y` | Uses the size in Y of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |
| `M_SIZE_Z` | Uses the size in Z of the blob's bounding box. The default bounding box is axis-aligned. To specify the PCA or semi-oriented box, use the respective combination values [`M_PCA_BOX`](../../Reference/3dblob/M3dblobSort.md) or [`M_SEMI_ORIENTED_BOX`](../../Reference/3dblob/M3dblobSort.md). |

*For specifying the type of bounding box for the third feature*

| Value | Description |
| --- | --- |
| `M_PCA_BOX` | Specifies to use results associated with the PCA bounding box. |
| `M_SEMI_ORIENTED_BOX` | Specifies to use results associated with the semi-oriented bounding box. |

*For using Feret contact points for the third feature*

| Value | Description |
| --- | --- |
| *(see `FeretContactPoints`)* |  |

*For using the Feret direction for the third feature*

| Value | Description |
| --- | --- |
| *(see `FeretDirection`)* |  |

*For using the closest points between neighboring blobs for the third feature*

| Value | Description |
| --- | --- |
| *(see `NearestPoint`)* |  |

*For specifying the sorting order for the third feature*

| Value | Description |
| --- | --- |
| `M_DOWN` | Specifies to sort blob indices in descending order, from the largest calculated feature value to the smallest. |
| `M_UP` | Specifies to sort blob indices in ascending order, from the smallest calculated feature value to the largest. |

### `NbBlobsToKeep` *(in, AIL_INT)*

Specifies to keep the first N blobs, if required. To keep all blobs, set this to [`M_INFINITE`](../../Reference/3dblob/M3dblobSort.md) or [`M_DEFAULT`](../../Reference/3dblob/M3dblobSort.md).

*For specifying which blobs to keep*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to keep all blobs. |
| `Value > 0` | Specifies the number of blobs to keep. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to deal with blob indices for which the blob's specified feature was not calculated. This is only applied when necessary. For example, if both blobs have a [`Feature1`](../../Reference/3dblob/M3dblobSort.md) result and the comparison is successful, then whether or not both blobs have a [`Feature2`](../../Reference/3dblob/M3dblobSort.md) result is irrelevant, and the [`ControlFlag`](../../Reference/3dblob/M3dblobSort.md) setting is ignored.

*For dealing with blob indices for which the blob's specified feature was not calculated*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BACK` | Specifies to put blob indices, for blobs that do not have the feature result, at the end of the list. |
| `M_FEATURE_REQUIRED` *(default)* | Specifies that a calculated feature result must exist for the specified feature; otherwise, an error is reported. |
| `M_FRONT` | Specifies to put blob indices, for blobs that do not have the feature result, at the front of the list. |
| `M_GREATER` | Specifies that blobs that do not have the feature result compare larger than blobs that do. This means that, when comparing two blobs (one with and one without the feature result), the missing feature result's value is considered larger for that blob, which affects the blob's index placement in the list. For example, if the sort is ascending ([`M_UP`](../../Reference/3dblob/M3dblobSort.md)), the blob without the feature result is placed in the list after the blob that has the feature result. |
| `M_LESS` | Specifies that blobs that do not have the feature result compare smaller than blobs that do. This means that, when comparing two blobs (one with and one without the feature result), the missing feature result's value is considered smaller for that blob, which affects the blob's placement in the list. For example, if the sort is ascending ([`M_UP`](../../Reference/3dblob/M3dblobSort.md)), the blob without the feature result is placed in the list before the blob that has the feature result. |

You can add a combination value from the following table:

- [`SortingOrder`](../../Reference/SortingOrder.md).

You can add a combination value from the following table:

- [`SortingOrder2`](../../Reference/SortingOrder2.md).

You can add a combination value from the following table:

- [`SortingOrder3`](../../Reference/SortingOrder3.md).
