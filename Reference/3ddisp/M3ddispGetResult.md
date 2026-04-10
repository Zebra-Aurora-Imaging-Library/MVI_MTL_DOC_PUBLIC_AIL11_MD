---
doctype: Reference
module: 3ddisp
function: M3ddispGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispGetResult"
---

# M3ddispGetResult

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

> Get the specified type of result from a picking result buffer.

## Syntax

```c
AIL_DOUBLE M3ddispGetResult(
    AIL_ID    Result3ddispId,  //in
    AIL_INT64 ResultIndex,     //in
    AIL_INT64 ResultType,      //in
    void *    ResultArrayPtr   //out
)
```

## Description

This function retrieves results of the specified type from a picking result buffer. Results are available after calling [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) or [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md).

## Parameters

### `Result3ddispId` *(in, AIL_ID)*

Specifies the identifier of the picking result buffer, previously allocated using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md) with [`M_PICKING_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) or [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md).

### `ResultIndex` *(in, AIL_INT64)*

Specifies the index of the result to get. Set this parameter to one of the values below:

*For specifying which result to retrieve*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PICKING_INDEX` | Specifies to retrieve results for a detected 3D graphic at the specified index. For an [`M_PICKING_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer, the sole result (if detected) is stored at index 0. |
| `M_GENERAL` *(default)* | Specifies to retrieve a general result from a picking result buffer. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to get.

### `ResultArrayPtr` *(out, *void)*

Specifies the address in which to write the results. Since [`M3ddispGetResult`](../../Reference/3ddisp/M3ddispGetResult.md) also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For retrieving general results from a picking result buffer

To retrieve general results from a picking result buffer, set the [`ResultIndex`](../../Reference/3ddisp/M3ddispGetResult.md) parameter to [`M_GENERAL`](../../Reference/3ddisp/M3ddispGetResult.md). In this case, you can set the [`ResultType`](../../Reference/3ddisp/M3ddispGetResult.md) parameter to one of the following values.

---

### `M_NUMBER`

Retrieves the number of 3D graphics in the picking result buffer.  Note that an [`M_PICKING_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer will store either 0 or 1 detected 3D graphics.  This result is always available.

---

### `M_TARGET_POSITION_DISPLAY_X`

Retrieves the X-pixel coordinate used for the pick operation.  This result is always available after an [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) operation.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_TARGET_POSITION_DISPLAY_Y`

Retrieves the Y-pixel coordinate used for the pick operation.  This result is always available after an [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) operation.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

### For retrieving results of a particular graphic that was picked

To retrieve results of a picked graphic, set the [`ResultIndex`](../../Reference/3ddisp/M3ddispGetResult.md) parameter to **M_PICKING_INDEX()**. The [`ResultIndex`](../../Reference/3ddisp/M3ddispGetResult.md) parameter must be set to **M_PICKING_INDEX(0)** when retrieving results from an [`M_PICKING_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer. Set the [`ResultType`](../../Reference/3ddisp/M3ddispGetResult.md) parameter to one of the following values:  > **Note:** Note, the following results are only available if the pick operation detected a 3D graphic.

---

### `M_CONTAINER_ID`

Retrieves the Aurora Imaging Library identifier of the container or depth map image buffer linked to the detected 3D graphic.  This result is only available if the pick operation detected a point cloud graphic, and that point cloud graphic was not created with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md).

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the Aurora Imaging Library identifier of the container linked to the 3D graphic. |
| `Image buffer identifier` | Specifies the Aurora Imaging Library identifier of the depth map image buffer linked to the 3D graphic. |

---

### `M_DISTANCE`

Retrieves the distance from the picked position to the 3D display's viewpoint.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_FACE`

Retrieves the index of the picked face.  This result is only available if the pick operation detected a box or a meshed point cloud graphic.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_GRAPHIC_LIST_ID`

Retrieves the Aurora Imaging Library identifier of the 3D graphics list that contains the detected 3D graphic.

| Value | Description |
| --- | --- |
| `3D graphics list identifier` | Specifies the Aurora Imaging Library identifier of the 3D graphics list containing the 3D graphic that was detected by the pick operation. |

---

### `M_GRAPHIC_SUB_INDEX`

Retrieves the index of the picked sub-element (for example, the index of the picked dot in the detected dots graphic).  This result is only available if the pick operation detected a dots or lines graphic. Note that lines graphics must not have their [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md).  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_GRAPHIC_SUB_INDEX_NUMBER`

Retrieves the number of graphic sub-elements were picked for the given graphic.  This result is only available if the pick operation detected a dots, lines, or point cloud graphic.  > **Note:** This result is only available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of sub-elements that were picked. |

---

### `M_GRAPHIC_SUB_INDICES`

Retrieves an array of the indices of the specified graphic's sub-elements within the picking area.  For example, for a point cloud graphic, its sub-elements are those points that were within the picking region during the call to [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md).  This result is only available if the pick operation detected a dots, lines, or point cloud graphic.  > **Note:** This result is only available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the indices of the sub-elements within the area. |

---

### `M_GRAPHIC_TYPE`

Retrieves the type of the detected 3D graphic.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_TYPE`](Reference/3dgra/M3dgraInquire.md))* |  |

---

### `M_LABEL`

Retrieves the label of the detected 3D graphic.

---

### `M_PICKED_POINT_PIXEL_X`

Retrieves the X-pixel coordinate of the picked point in the range component or depth map.  This result is only available if the pick operation detected a point cloud graphic.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_PICKED_POINT_PIXEL_Y`

Retrieves the Y-pixel coordinate of the picked point in the range component or depth map.  This result is only available if the pick operation detected a point cloud graphic.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_PICKED_POSITION_3D_X`

Retrieves the X-position where the ray intersected with the 3D graphic in world coordinates. For a point cloud or dots graphic, this is the X-position of the point in the 3D graphic.  Note that this result can differ (very slightly) from the value stored in the range component of a point cloud or dots graphic because, a point might not line up with a specific pixel on the display. For a point cloud graphic, you can use [`MbufGet`](../../Reference/buf/MbufGet.md) to copy the values in the range component into an array, and then retrieve the stored X-position of the picked point from this array, using [`M_PICKED_POINT_PIXEL_X`](../../Reference/3ddisp/M3ddispGetResult.md) and [`M_PICKED_POINT_PIXEL_Y`](../../Reference/3ddisp/M3ddispGetResult.md) and 0 as the indices. For a dots graphic, you can use [`M_GRAPHIC_SUB_INDEX`](../../Reference/3ddisp/M3ddispGetResult.md) to retrieve the X-position of the picked point from the array that you used to create the dots graphics.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_PICKED_POSITION_3D_Y`

Retrieves the Y-position where the ray intersected with the 3D graphic in world coordinates. For a point cloud or dots graphic, this is the Y-position of the point in the 3D graphic.  Note that this result can differ (very slightly) from the value stored in the range component of a point cloud or dots graphic because, a point might not line up with a specific pixel on the display. For a point cloud graphic, you can use [`MbufGet`](../../Reference/buf/MbufGet.md) to copy the values in the range component into an array, and then retrieve the stored Y-position of the picked point from this array, using [`M_PICKED_POINT_PIXEL_X`](../../Reference/3ddisp/M3ddispGetResult.md) and [`M_PICKED_POINT_PIXEL_Y`](../../Reference/3ddisp/M3ddispGetResult.md) and 0 as the indices. For a dots graphic, you can use [`M_GRAPHIC_SUB_INDEX`](../../Reference/3ddisp/M3ddispGetResult.md) to retrieve the index of the picked point from the array that you used to create the dots graphics.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

---

### `M_PICKED_POSITION_3D_Z`

Retrieves the Z-position where the ray intersected with the 3D graphic in world coordinates. For a point cloud or dots graphic, this is the Z-position of the point in the 3D graphic.  Note that this result can differ (very slightly) from the value stored in the range component of a point cloud or dots graphic because, a point might not line up with a specific pixel on the display. For a point cloud graphic, you can use [`MbufGet`](../../Reference/buf/MbufGet.md) to copy the values in the range component into an array, and then retrieve the stored Z-position of the picked point from this array, using [`M_PICKED_POINT_PIXEL_X`](../../Reference/3ddisp/M3ddispGetResult.md) and [`M_PICKED_POINT_PIXEL_Y`](../../Reference/3ddisp/M3ddispGetResult.md) and 0 as the indices. For a dots graphic, you can use [`M_GRAPHIC_SUB_INDEX`](../../Reference/3ddisp/M3ddispGetResult.md) to retrieve the index of the picked point from the array that you used to create the dots graphics.  > **Note:** This result is not available for an [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md) buffer.

### Combination Constants — For inquiring a position relative to the 3D graphics list's root node

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the position relative to the 3D graphic's root node.

| Value | Description |
| --- | --- |
| `M_RELATIVE_TO_ROOT` | Specifies to get the picked position relative to the 3D graphics list's root node. |

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the requested information, cast to an _AIL_DOUBLE_. If the requested information does not fit into an _AIL_DOUBLE_, this function will return `M_NULL`or truncate the information.
