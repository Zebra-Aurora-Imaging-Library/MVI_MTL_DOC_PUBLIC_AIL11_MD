---
doctype: Reference
module: 3dgra
function: M3dgraSphere
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraSphere"
---

# M3dgraSphere

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

> Add a sphere graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraSphere(
    AIL_ID     List3dgraId,  //out
    AIL_INT64  ParentLabel,  //in
    AIL_DOUBLE CenterX,      //in
    AIL_DOUBLE CenterY,      //in
    AIL_DOUBLE CenterZ,      //in
    AIL_DOUBLE Radius,       //in
    AIL_INT64  ControlFlag   //in
)
```

## Description

This function adds a sphere graphic to the specified 3D graphics list, allowing you to, for example, view the sphere graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the sphere graphic. When the sphere graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the sphere graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the sphere graphic's parent.

The sphere graphic has its own coordinate system that represents the sphere graphic's position and orientation with respect to its parent's coordinate system. The origin of the sphere graphic's coordinate system is the sphere graphic's center, and its orientation is inherited, upon creation, from its parent's coordinate system. This means that the sphere graphic's orientation is the same as the orientation of its parent's coordinate system. You can change the position and orientation of the sphere graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

> **Note:** Note that although a sphere graphic's coordinate system has an orientation, the orientation has no effect on the appearance of the sphere graphic.

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraSphere.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the sphere graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the sphere graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraSphere.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the sphere graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CenterX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the center of the sphere graphic.

### `CenterY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the center of the sphere graphic.

### `CenterZ` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the center of the sphere graphic.

### `Radius` *(in, AIL_DOUBLE)*

Specifies the sphere graphic's radius.

*For specifying the sphere graphic's radius*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the sphere graphic's radius. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the sphere graphic added to the 3D graphics list.
