---
doctype: Reference
module: 3dgra
function: M3dgraDots
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraDots"
---

# M3dgraDots

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

> Add a dots graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraDots(
    AIL_ID             List3dgraId,      //out
    AIL_INT64          ParentLabel,      //in
    AIL_INT            NumPoints,        //in
    const AIL_DOUBLE * CoordXArrayPtr,   //in
    const AIL_DOUBLE * CoordYArrayPtr,   //in
    const AIL_DOUBLE * CoordZArrayPtr,   //in
    const AIL_UINT8 *  PointsRArrayPtr,  //in
    const AIL_UINT8 *  PointsGArrayPtr,  //in
    const AIL_UINT8 *  PointsBArrayPtr,  //in
    AIL_INT64          ControlFlag       //in
)
```

## Description

This function adds a dots graphic to the specified 3D graphics list, allowing you to, for example, view the dots graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the dots graphic. When the dots graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the dots graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the dots graphic's parent.

The dots graphic has its own coordinate system that represents the position and orientation of the dots with respect to its parent's coordinate system. The coordinates of the dots, specified by [`CoordXArrayPtr`](../../Reference/3dgra/M3dgraDots.md), [`CoordYArrayPtr`](../../Reference/3dgra/M3dgraDots.md), and [`CoordZArrayPtr`](../../Reference/3dgra/M3dgraDots.md), are expressed in the dots graphic's coordinate system. Initially, the dots graphic's position and orientation is the identity matrix. This means that the dots graphic's position and orientation is the same as the position and orientation of its parent's coordinate system. You can change the position and orientation of the dots graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraDots.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the dots graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the dots graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraDots.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the dots graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points in the dots graphic.

### `CoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of X-coordinates of the points in the dots graphic.

### `CoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of Y-coordinates of the points in the dots graphic.

### `CoordZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of Z-coordinates of the points in the dots graphic.

### `PointsRArrayPtr` *(in, *AIL_UINT8)*

Specifies the list of red values of the color of the points in the dots graphic.

### `PointsGArrayPtr` *(in, *AIL_UINT8)*

Specifies the list of green values of the color of the points in the dots graphic.

### `PointsBArrayPtr` *(in, *AIL_UINT8)*

Specifies the list of blue values of the color of the points in the dots graphic.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the dots graphic added to the 3D graphics list.
