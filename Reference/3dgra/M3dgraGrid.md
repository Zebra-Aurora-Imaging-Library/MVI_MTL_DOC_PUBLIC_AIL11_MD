---
doctype: Reference
module: 3dgra
function: M3dgraGrid
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraGrid"
---

# M3dgraGrid

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

> Add a grid graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraGrid(
    AIL_ID     List3dgraId,    //out
    AIL_INT64  ParentLabel,    //in
    AIL_INT64  CreationMode,   //in
    AIL_ID     Matrix3dgeoId,  //in
    AIL_DOUBLE Param1,         //in
    AIL_DOUBLE Param2,         //in
    AIL_DOUBLE Param3,         //in
    AIL_DOUBLE Param4,         //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function adds a grid graphic to the specified 3D graphics list, allowing you to, for example, view the grid graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the grid graphic. When the grid graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the grid graphic's parent must be the root node.

The grid graphic has its own coordinate system that represents the grid's position and orientation with respect to its parent's coordinate system. The origin of the grid's coordinate system is the grid's center, and its orientation is the grid's orientation. The grid lies on its coordinate system's XY (Z=0) plane. You can set the grid's position and orientation using the [`Matrix3dgeoId`](../../Reference/3dgra/M3dgraGrid.md) parameter. You can change the position and orientation of the grid graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraGrid.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the grid graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the grid graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraGrid.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the grid graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the grid graphic is defined.

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix that defines the grid graphic's position and orientation with respect to its parent's coordinate system.

*For specifying the transformation matrix object identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_IDENTITY_MATRIX`](../../Reference/3dgra/M3dgraGrid.md). |
| `M_IDENTITY_MATRIX` | Specifies the identity matrix. This means that the grid graphic's position and orientation is the same as the position and orientation of its parent's coordinate system. The resulting grid is on its parent's coordinate system's XY (Z=0) plane, and is centered on its parent's coordinate system's origin. |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix that defines the grid graphic's position and orientation with respect to its parent's coordinate system. The transformation matrix must be of type [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md). |

### `Param1` *(in, AIL_DOUBLE)*

Specifies the first parameter used to define the grid.

### `Param2` *(in, AIL_DOUBLE)*

Specifies the second parameter used to define the grid.

### `Param3` *(in, AIL_DOUBLE)*

Specifies the third parameter used to define the grid.

### `Param4` *(in, AIL_DOUBLE)*

Specifies the fourth parameter used to define the grid.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the grid graphic

---

### `M_SIZE_AND_SPACING`

Defines the grid graphic using its size and line spacing. If the specified size is not a multiple of the specified line spacing, the size is rounded down to the nearest multiple of the line spacing. The grid always has at least one cell. If the specified size is smaller than the specified line spacing, there will be a single cell with the dimensions of the line spacing.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the grid's size along its coordinate system's X-axis, in world units. |
| `Value > 0.0` | Specifies the grid's size along its coordinate system's Y-axis, in world units. |
| `Value > 0.0` | Specifies the grid's line spacing along its coordinate system's X-axis, in world units. |
| `Value > 0.0` | Specifies the grid's line spacing along its coordinate system's Y-axis, in world units. |

---

### `M_TILES_AND_SPACING`

Defines the grid graphic using its number of cells and line spacing. The grid's size along its coordinate system's X- or Y-axis is equal to the number of cells along the given axis multiplied by the line spacing along the same axis.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the grid's number of cells along its coordinate system's X-axis. |
| `Value >= 1` | Specifies the grid's number of cells along its coordinate system's Y-axis. |
| `Value > 0.0` | Specifies the grid's line spacing along its coordinate system's X-axis, in world units. |
| `Value > 0.0` | Specifies the grid's line spacing along its coordinate system's Y-axis, in world units. |

## Return Value

**Type:** `AIL_INT64`

Returns the label of the grid graphic added to the 3D graphics list.
