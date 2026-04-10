---
doctype: Reference
module: 3dgeo
function: M3dgeoDraw3d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoDraw3d"
---

# M3dgeoDraw3d

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

> Draw the geometry, defined in the specified 3D geometry object, into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgeoDraw3d(
    AIL_ID    OperationDraw3dContext3dgeoId,  //in
    AIL_ID    SrcGeometry3dgeoId,             //in
    AIL_ID    DstList3dgraId,                 //out
    AIL_INT64 DstParentLabel,                 //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function draws a geometry, defined in the specified 3D geometry object, into the destination 3D graphics list.

## Parameters

### `OperationDraw3dContext3dgeoId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `SrcGeometry3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the 3D geometry object to draw into the 3D graphics list. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include box, cylinder, line, plane, point, and sphere.

*For specifying the geometry object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `3D geometry object identifier` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. The 3D graphics list must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to be used as the geometry's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgeo/M3dgeoDraw3d.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the 3D graphic added to the 3D graphics list.
