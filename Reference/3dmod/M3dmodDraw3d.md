---
doctype: Reference
module: 3dmod
function: M3dmodDraw3d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodDraw3d"
---

# M3dmodDraw3d

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

> Draw features of models, or results of found model occurrences, into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dmodDraw3d(
    AIL_ID    OperationDraw3dContext3dmodId,  //in
    AIL_ID    SrcContextOrResult3dmodId,      //in
    AIL_INT64 SrcIndex,                       //in
    AIL_ID    DstList3dgraId,                 //out
    AIL_INT64 DstParentLabel,                 //in
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function draws the specified feature of a surface model, or result of any found model occurrence(s), stored in the specified find surface or planar surface 3D model finder context or find 3D model finder result buffer, into a 3D graphics list. Set the draw operations and options for the draw using [`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md).

> **Note:** Note that for a 3D model finder context, the model must be defined using [`M3dmodDefine`](../../Reference/3dmod/M3dmodDefine.md) and, if [`M_DRAW_MODEL_PREPROCESSED`](../../Reference/3dmod/M3dmodControlDraw.md) is active, the context must be preprocessed using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md), before calling this function. For a 3D model finder result buffer, [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) must be called before calling this function.

## Parameters

### `OperationDraw3dContext3dmodId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D model finder context that specifies the annotations to draw and how to draw them. This parameter must be set to one of the following values:

*For specifying the draw 3D model finder context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a predefined draw 3D model finder context with all draw operations ([`M3dmodControlDraw`](../../Reference/3dmod/M3dmodControlDraw.md)) set to use default settings. |
| `Draw 3D model finder context identifier` | Specifies a valid draw 3D model finder context identifier, which you have allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with either [`M_DRAW_3D_GEOMETRIC_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw occurrences of geometric models) or [`M_DRAW_3D_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw occurrences of surface models or to draw features of surface models). |

### `SrcContextOrResult3dmodId` *(in, AIL_ID)*

Specifies the identifier of the 3D model finder context, previously allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) or [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), or the identifier of the 3D model finder result buffer, previously allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_BOX_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), [`M_FIND_CYLINDER_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), [`M_FIND_RECTANGULAR_PLANE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), [`M_FIND_SPHERE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), [`M_FIND_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md), or [`M_FIND_PLANAR_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md).

### `SrcIndex` *(in, AIL_INT64)*

Specifies the index of the model in the 3D model finder context, or of the occurrence in the 3D model finder result buffer.

*For specifying the index of the model or occurrence to draw*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Draws the specified feature(s) of all models or all occurrences. A sub-parent label for each model or occurrence will be created under the parent label returned by this function. |
| `0 <= Value < M_NUMBER` | Specifies the index of the model occurrence. |
| `0 <= Value < M_NUMBER_MODELS` | Specifies the index of the model. |

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. You can specify a 3D graphics list that you have previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to use as the model annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dmod/M3dmodDraw3d.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the parent label of all 3D graphics that the function added to the 3D graphics list. If the specified 3D model finder result buffer contains no occurrences, a node 3D graphic is added to the graphics list and the node's label is returned.
