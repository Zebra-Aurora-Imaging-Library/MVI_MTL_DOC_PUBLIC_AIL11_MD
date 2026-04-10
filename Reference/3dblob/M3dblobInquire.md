---
doctype: Reference
module: 3dblob
function: M3dblobInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dblob / M3dblobInquire"
---

# M3dblobInquire

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

> Inquire about a 3D blob analysis context or result buffer setting.

## Syntax

```c
AIL_INT64 M3dblobInquire(
    AIL_ID    ContextOrResult3dblobId,  //in
    AIL_INT64 LabelOrIndex,             //in
    AIL_INT64 InquireType,              //in
    void *    UserVarPtr                //out
)
```

## Description

This function inquires about a specified setting of a 3D blob analysis context or result buffer.

To inquire about draw 3D blob analysis settings for [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md), use [`M3dblobInquireDraw`](../../Reference/3dblob/M3dblobInquireDraw.md) instead.

## Parameters

### `ContextOrResult3dblobId` *(in, AIL_ID)*

Specifies the identifier of the 3D blob analysis context or result buffer to inquire.

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies that you are inquiring about a global setting of the 3D blob analysis context or result buffer.

*For specifying what to inquire*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

If a 3D blob analysis context is specified, same as [`M_CONTEXT`](../../Reference/3dblob/M3dblobInquire.md).

If a 3D blob analysis result buffer is specified, same as [`M_GENERAL`](../../Reference/3dblob/M3dblobInquire.md). |
| `M_CONTEXT` | Specifies to inquire a general setting of a 3D blob analysis context. |
| `M_GENERAL` | Specifies to inquire a general setting of a 3D blob analysis result buffer. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`M3dblobInquire`](../../Reference/3dblob/M3dblobInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring a 3D blob analysis context

For the following inquire types, the [`ContextOrResult3dblobId`](../../Reference/3dblob/M3dblobInquire.md) parameter must specify a 3D blob analysis context.

---

### `Calculate 3D blob analysis context ID`

Specifies a calculate 3D blob analysis context. allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_CALCULATE_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md), and used in [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) operations.

#### `M_BOUNDING_BOX`

Inquires whether to calculate the axis-aligned bounding box and its related Ferets ([`M_FERET_...`](../../Reference/3dblob/M3dblobGetResult.md)).

| Value | Description |
| --- | --- |
| *(see [`M_BOUNDING_BOX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_CENTROID`

Inquires whether to calculate the centroid.

| Value | Description |
| --- | --- |
| *(see [`M_CENTROID`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_FERET_CONTACT_POINTS`

Inquires whether to save the contact points at each Feret; the contact points are the coordinates of the Feret diameter's start and end points.

| Value | Description |
| --- | --- |
| *(see [`M_FERET_CONTACT_POINTS`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_FERET_GENERAL`

Inquires whether to calculate the Feret along a custom direction specified with [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobInquire.md) + [`M_FERET_DIRECTION_...`](../../Reference/3dblob/M3dblobInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_FERET_GENERAL`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_FERET_MAX_DIAMETER`

Inquires whether to calculate the longest Feret.

| Value | Description |
| --- | --- |
| *(see [`M_FERET_MAX_DIAMETER`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_FERET_MIN_DIAMETER`

Inquires whether to calculate the shortest Feret.

| Value | Description |
| --- | --- |
| *(see [`M_FERET_MIN_DIAMETER`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_FERET_PRECISION`

Inquires the accuracy when calculating the minimum and maximum Feret directions.

| Value | Description |
| --- | --- |
| *(see [`M_FERET_PRECISION`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_LINEARITY`

Inquires whether to calculate the degree to which the blobs are linear.

| Value | Description |
| --- | --- |
| *(see [`M_LINEARITY`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_MOMENT_ORDER`

Inquires the order up to which moments are calculated.

| Value | Description |
| --- | --- |
| *(see [`M_MOMENT_ORDER`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_MOMENTS`

Inquires whether to calculate all moments up to [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_MOMENTS`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NEAREST_BLOB`

Inquires whether to calculate features related to the nearest blob.

| Value | Description |
| --- | --- |
| *(see [`M_NEAREST_BLOB`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_PCA`

Inquires whether to calculate the principal components, which are used in a principal component analysis (PCA) or when calculating [`M_PCA_BOX`](../../Reference/3dblob/M3dblobInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_PCA`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_PCA_BOX`

Inquires whether to calculate the PCA-aligned bounding box and its related Ferets.

| Value | Description |
| --- | --- |
| *(see [`M_PCA_BOX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_PLANARITY`

Inquires whether to calculate the degree to which the blobs are planar.

| Value | Description |
| --- | --- |
| *(see [`M_PLANARITY`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_SEMI_ORIENTED_BOX`

Inquires whether to calculate the semi-oriented bounding box and its related Ferets.

| Value | Description |
| --- | --- |
| *(see [`M_SEMI_ORIENTED_BOX`](Reference/3dblob/M3dblobControl.md))* |  |

---

### `Segmentation 3D blob analysis context ID`

Specifies a segmentation 3D blob analysis context. allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md), and used in [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) operations.

#### `M_COLOR_DISTANCE_MAX`

Inquires the maximum difference between the color of two points for them to be considered neighbors.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_DISTANCE_MAX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_COLOR_DISTANCE_MODE`

Inquires how to calculate the difference in color between two points.

| Value | Description |
| --- | --- |
| *(see [`M_COLOR_DISTANCE_MODE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_CUSTOM_DISTANCE_COMPONENT`

Inquires whether to use an extra component during the segmentation.

| Value | Description |
| --- | --- |
| *(see [`M_CUSTOM_DISTANCE_COMPONENT`](Reference/3dblob/M3dblobControl.md))* |  |
| `Bit-encoded value that represents the component` | Specifies an encoded value that represents the component type, or, if a macro was used, the macro used and the component's index or Aurora Imaging Library identifier.  You can use the **M_COMPONENT_EXTRACT_VALUE** macro to retrieve the component type, or, if a macro was used, the component's index or Aurora Imaging Library identifier.  To retrieve the type of macro that was used to pass the component, use the **M_COMPONENT_EXTRACT_FLAG** macro, which returns **M_COMPONENT_BY_ID_FLAG** or **M_COMPONENT_BY_INDEX_FLAG**; 0 is returned if no macro was used.  Note that the encoded value can be passed wherever the component can be expressed using its type or using a macro (for example, [`Component`](../../Reference/buf/MbufInquireContainer.md) parameter of [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md)). |

#### `M_CUSTOM_DISTANCE_MAX`

Inquires the maximum difference within which two points are considered neighbors in the custom component.

| Value | Description |
| --- | --- |
| *(see [`M_CUSTOM_DISTANCE_MAX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_GLOBAL_NORMAL_DISTANCE_MAX`

Inquires the maximum angle between the normal of a point and a blob's average normal, for the point to be added to the blob.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_NORMAL_DISTANCE_MAX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_GLOBAL_PLANE_DISTANCE_MAX`

Inquires the maximum distance between a point and a blob's average plane, for the point to be added to the blob.

| Value | Description |
| --- | --- |
| *(see [`M_GLOBAL_PLANE_DISTANCE_MAX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_MAX_DISTANCE`

Inquires the maximum distance within which two points are considered neighbors.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_DISTANCE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_MAX_DISTANCE_MODE`

Inquires whether to use, for the maximum distance between points, the value set with [`M_MAX_DISTANCE`](../../Reference/3dblob/M3dblobInquire.md) or an automatic threshold.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_DISTANCE_MODE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_MAXIMUM_NUMBER_NEIGHBORS`

Inquires the maximum number of neighbors to consider at each point.

| Value | Description |
| --- | --- |
| *(see [`M_MAXIMUM_NUMBER_NEIGHBORS`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NEIGHBOR_SEARCH_MODE`

Inquires the search mode for finding the neighbors of a point.

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBOR_SEARCH_MODE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NEIGHBORHOOD_ORGANIZED_SIZE`

Inquires the neighborhood size, when finding the neighbors of a point in an organized point cloud.  This inquire type is supported only when using an organized point cloud and only if [`M_NEIGHBOR_SEARCH_MODE`](../../Reference/3dblob/M3dblobInquire.md) is set to [`M_ORGANIZED`](../../Reference/3dblob/M3dblobInquire.md).

| Value | Description |
| --- | --- |
| *(see [`M_NEIGHBORHOOD_ORGANIZED_SIZE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NORMAL_DISTANCE_MAX`

Inquires the maximum angle between normal vectors within which two points are considered neighbors.  This inquire type is available when an [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md) component exists in the source container.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_DISTANCE_MAX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NORMAL_DISTANCE_MAX_MODE`

Inquires whether to use, for the maximum angle between normal vectors, the value set with [`M_NORMAL_DISTANCE_MAX`](../../Reference/3dblob/M3dblobInquire.md) or an automatic threshold.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_DISTANCE_MAX_MODE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NORMAL_DISTANCE_MODE`

Inquires how to calculate the angle difference between the normal vectors of two points.

| Value | Description |
| --- | --- |
| *(see [`M_NORMAL_DISTANCE_MODE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MAX`

Inquires the maximum number of points for a blob to be considered valid.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MAX`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_NUMBER_OF_POINTS_MIN`

Inquires the minimum number of points for a blob to be considered valid.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_OF_POINTS_MIN`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_RELABEL_CONSECUTIVE`

Inquires whether to force the labels to be consecutive from 1 to [`M_MAX_LABEL_VALUE`](../../Reference/3dblob/M3dblobGetResult.md).

| Value | Description |
| --- | --- |
| *(see [`M_RELABEL_CONSECUTIVE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_SEGMENTATION_MODE`

Inquires the type of segmentation to perform.

| Value | Description |
| --- | --- |
| *(see [`M_SEGMENTATION_MODE`](Reference/3dblob/M3dblobControl.md))* |  |

#### `M_TIMEOUT`

Inquires the maximum amount of time for [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) to complete the blob segmentation operation before generating a time-out error.

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT`](Reference/3dblob/M3dblobControl.md))* |  |

### For inquiring a 3D blob analysis result buffer

For the following inquire type, the [`ContextOrResult3dblobId`](../../Reference/3dblob/M3dblobInquire.md) parameter must specify a 3D blob analysis result buffer.

---

### `Segmentation 3D blob analysis result ID, for general results`

Specifies a segmentation 3D blob analysis result buffer, allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md), and used to store [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md) results.

#### `M_INDEX_MODE`

Inquires how to interpret the [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter in all functions (including this function and, for example, [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md)) that use the 3D blob analysis result buffer.

| Value | Description |
| --- | --- |
| `M_INDEX` | Specifies that the index will be passed directly to the function's [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter, without any macros. |
| `M_LABEL` | Specifies that the label will be passed directly to the function's [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter, without any macros. |
| `M_USE_MACRO` | Specifies that the values passed to the [`LabelOrIndex`](../../Reference/blob/MblobGetResult.md) parameter of the function can be the macros [`M_BLOB_INDEX()`](../../Reference/3dblob/M3dblobControl.md) or [`M_BLOB_LABEL()`](../../Reference/3dblob/M3dblobControl.md). |

### Combination Constants — For specifying the maximum distance to the nearest blob

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the maximum distance to the nearest blob.

#### `M_MAX_DISTANCE`

Inquires the maximum distance within which Aurora Imaging Library considers a nearby blob to be the closest.

### Combination Constants — For specifying the Feret direction

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the direction along which to calculate [`M_FERET_GENERAL`](../../Reference/3dblob/M3dblobInquire.md).

#### `M_FERET_DIRECTION_X`

Inquires the X-component of the direction vector.

#### `M_FERET_DIRECTION_Y`

Inquires the Y-component of the direction vector.

#### `M_FERET_DIRECTION_Z`

Inquires the Z-component of the direction vector.

### Combination Constants — For inquiring about the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get information about the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

#### `M_IS_SET_TO_DEFAULT`

Inquires whether the specified inquire type is set to its default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not set to its default value. |
| `M_TRUE` | Specifies that the inquire type is set to its default value. |

### Combination Constants — For inquiring whether an inquire type has a default value or whether it is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type has a default value or whether it is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested information to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.

In this case, you must set [`LabelOrIndex`](../../Reference/3dblob/M3dblobInquire.md) to [`M_CONTEXT`](../../Reference/3dblob/M3dblobInquire.md) or [`M_DEFAULT`](../../Reference/3dblob/M3dblobInquire.md).
