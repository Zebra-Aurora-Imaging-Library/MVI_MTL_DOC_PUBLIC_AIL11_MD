---
doctype: Reference
module: 3dmap
function: M3dmapClear
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmap / M3dmapClear"
---

# M3dmapClear

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

> Clear a 3D reconstruction result buffer of some or all of its content.

## Syntax

```c
void M3dmapClear(
    AIL_ID    Result3dmapId,  //out
    AIL_INT   LabelOrIndex,   //in
    AIL_INT64 Operation,      //in
    AIL_INT64 ControlFlag     //in
)
```

## Description

This function allows you to clear or delete the laser line data from a 3D reconstruction result buffer without having to free and reallocate the buffer.

For [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffers, you can clear a point cloud of its points while keeping a record of the ongoing conveyor movement with [`M_CLEAR`](../../Reference/3dmap/M3dmapClear.md). You can also delete a point cloud, which deletes its points, deletes its record of ongoing conveyor movement, and frees its label, with [`M_DELETE`](../../Reference/3dmap/M3dmapClear.md). Note that when you clear or delete a point cloud, the relative coordinate system and some other control value information remain in the [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer.

For [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffers, you can clear the entire result buffer of all laser data using [`M_CLEAR`](../../Reference/3dmap/M3dmapClear.md). The result buffer still retains some control value information.

During calibration of the 3D reconstruction setup, you can remove the last scan added to the [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, for instance because you know the last scan was in error, using [`M_REMOVE_LAST_SCAN`](../../Reference/3dmap/M3dmapClear.md).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer ([`Result3dmapId`](../../Reference/3dmap/M3dmapClear.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object. This is valid as long as the [`LabelOrIndex`](../../Reference/3dmap/M3dmapClear.md) parameter of the concurrent calls is set to a different index, and is not set to [`M_ALL`](../../Reference/3dmap/M3dmapClear.md).

## Parameters

### `Result3dmapId` *(out, AIL_ID)*

Specifies the identifier of the 3D reconstruction result buffer to clear. The 3D reconstruction result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md)with [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md), [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md), or [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md). Any other type of 3D reconstruction result buffer will cause an error.

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the point cloud(s) in the specified 3D reconstruction result buffer. Only 3D reconstruction result buffers of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) have point clouds that can be specified using this parameter. For all other types of 3D reconstruction result buffers, set this parameter to [`M_DEFAULT`](../../Reference/3dmap/M3dmapClear.md).

*For specifying a point cloud*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_POINT_CLOUD_INDEX` | Specifies the index of a point cloud in the specified 3D reconstruction result buffer. |
| `M_POINT_CLOUD_LABEL` | Specifies the label for a point cloud in the specified 3D reconstruction result buffer. |
| `M_ALL` *(default)* | Specifies all point clouds in the specified 3D reconstruction result buffer. |

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform. This parameter must be set to one of the following:

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_CLEAR` | Clears laser line data from a 3D reconstruction result buffer.

For an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, this clears all the points in the specified point cloud, while maintaining the Y-axis displacement (ongoing conveyor movement) since the first call to [`M3dmapAddScan`](../../Reference/3dmap/M3dmapAddScan.md). The relative coordinate system associated with the [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, along with some control values, remain in the result buffer.

For an [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, this clears all laser line data, although some control values remain. Note that result buffers of type [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) do not have a relative coordinate system. |
| `M_DELETE` | Deletes one (or all) point cloud(s) from the collection of point clouds in an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer. This deletes the points in the point cloud, the Y-axis displacement, and the point cloud label of the point cloud. You can create a new point cloud using this freed label. |
| `M_REMOVE_LAST_SCAN` | Discards the most recent scan added to the result buffer. This is available only for [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md) result buffers. This operation is useful during the 3D reconstruction calibration phase if you know that the quality of the most recent scan added to the result buffer is not good enough for an accurate calibration. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
