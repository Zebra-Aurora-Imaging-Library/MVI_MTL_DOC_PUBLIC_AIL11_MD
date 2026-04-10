---
doctype: Reference
module: 3dim
function: M3dimDraw3d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimDraw3d"
---

# M3dimDraw3d

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

> Draw the result of a profile operation into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dimDraw3d(
    AIL_ID    OperationDraw3dContext3dimId,  //in
    AIL_ID    SrcProfileResult3dimId,        //in
    AIL_ID    DstList3dgraId,                //out
    AIL_INT64 DstParentLabel,                //in
    AIL_INT64 ControlFlag                    //in
)
```

## Description

This function draws the profile points, stored in the specified 3D profile result buffer, into a 3D graphics list. To take a profile of a depth map, use [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md) or [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md). To take a profile of a 3D geometry, mesh, or point cloud, use [`M3dimProfile`](../../Reference/3dim/M3dimProfile.md).

## Parameters

### `OperationDraw3dContext3dimId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `SrcProfileResult3dimId` *(in, AIL_ID)*

Specifies the identifier of the profile 3D image processing result buffer, previously allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md).

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. You can specify a 3D graphics list that you have previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to use as the profile annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ROOT_NODE` *(default)* | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the parent label of all 3D graphics that the function added to the 3D graphics list. If the specified profile 3D image processing result buffer has 0 points, a node 3D graphic is added to the graphics list and the node's label is returned.
