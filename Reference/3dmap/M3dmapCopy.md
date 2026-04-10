---
doctype: Reference
module: 3dmap
function: M3dmapCopy
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmap / M3dmapCopy"
---

# M3dmapCopy

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

> Copy one or more point clouds from one [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer to another.

## Syntax

```c
void M3dmapCopy(
    AIL_ID    SrcResult3dmapId,  //in
    AIL_INT   SrcLabelOrIndex,   //in
    AIL_ID    DstResult3dmapId,  //out
    AIL_INT   DstLabel,          //in
    AIL_INT64 Operation,         //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function copies or moves one or more point clouds from a source [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer to a destination [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer.

When copying/moving a specific point cloud, you must specify the label to assign it in the destination result buffer (using [`DstLabel`](../../Reference/3dmap/M3dmapCopy.md)); the label cannot already be assigned to an existing point cloud in the destination result buffer. If copying/moving all point clouds in the source result buffer ([`M_ALL`](../../Reference/3dmap/M3dmapCopy.md)), the destination result buffer must be empty; the copied/moved point clouds retain their original labels.

## Parameters

### `SrcResult3dmapId` *(in, AIL_ID)*

Specifies the identifier of the 3D reconstruction result buffer containing the source point clouds to copy/move. The 3D reconstruction result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md).

### `SrcLabelOrIndex` *(in, AIL_INT)*

Specifies the point cloud(s) to copy/move from the source [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer.

*For specifying the label or index of a specific source point cloud*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_POINT_CLOUD_INDEX` | Specifies an existing point cloud with the given index. |
| `M_POINT_CLOUD_LABEL` | Specifies an existing point cloud with the given label. |
| `M_ALL` *(default)* | Specifies all point clouds in the source [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer. |

### `DstResult3dmapId` *(out, AIL_ID)*

Specifies the identifier of the 3D reconstruction result buffer in which to copy/move the source point cloud(s). The 3D reconstruction result buffer must have been previously allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). You can specify the same identifier as the [`SrcResult3dmapId`](../../Reference/3dmap/M3dmapCopy.md) identifier, but the label to assign the copied/moved point cloud must be different.

### `DstLabel` *(in, AIL_INT)*

Specifies a new label in the destination [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer in which to copy/move the source point cloud(s).

*For specifying the label in the destination result buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_POINT_CLOUD_LABEL` | Specifies the label to assign the specific point cloud in the destination buffer. This point cloud should not already exist in the destination result buffer. |
| `M_ALL` *(default)* | Specifies all labels in the destination [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer, which should not contain any point clouds. |

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. This parameter should be set to one of the following values:

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_COPY` | Specifies the point cloud is copied to the destination [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer; the point cloud is not deleted from the source [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer.

> **Note:** Note that the specified point cloud retains its organizational type when copied, unless only valid points are copied ([`M_EXCLUDE_INVALID_POINTS`](../../Reference/3dmap/M3dmapCopy.md)). |
| `M_MOVE` | Specifies the point cloud is moved to the destination [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer; the point cloud is deleted from the source [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer. This operation is faster than copying.

Any point cloud moved using [`M3dmapCopy`](../../Reference/3dmap/M3dmapCopy.md) retains its organizational type. |

*For specifying whether to exclude invalid points*

| Value | Description |
| --- | --- |
| `M_EXCLUDE_INVALID_POINTS` | Specifies to exclude all points in the specified [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) result buffer that are set to **M_INVALID_POINT**. Only the valid points will be copied. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
