---
doctype: Reference
module: gra
function: MgraCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraCopy"
---

# MgraCopy

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

> Copy one or more graphics from one 2D graphics list to another.

## Syntax

```c
void MgraCopy(
    AIL_ID          SrcListGraId,             //in
    AIL_ID          DstListGraId,             //out
    AIL_INT64       Operation,                //in
    AIL_INT         InsertLocation,           //in
    AIL_INT         NumGraphics,              //in
    const AIL_INT * SrcIndexOrLabelArrayPtr,  //in
    AIL_INT *       DstLabelArrayPtr,         //out
    AIL_INT64       ControlFlag               //in
)
```

## Description

This function copies or moves one or more graphics from a source 2D graphics list to a destination 2D graphics list. When moved, the graphics are erased from the source 2D graphics list. You can specify the location in the destination 2D graphics list at which to insert the copied graphics, which offers some control on the drawing order.

Note that if you want to replace all existing elements of the destination 2D graphics list, you must clear it first using [`MgraClear`](../../Reference/gra/MgraClear.md).

This function supports in-place processing: the source and destination 2D graphics list identifiers can refer to the same 2D graphics list. In the case of an in-place move operation ([`M_MOVE`](../../Reference/gra/MgraCopy.md)), the graphic referred to by the [`InsertLocation`](../../Reference/gra/MgraCopy.md) parameter must not be one of the graphics being moved ([`SrcIndexOrLabelArrayPtr`](../../Reference/gra/MgraCopy.md)).

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstListGraId`](../../Reference/gra/MgraCopy.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `SrcListGraId` *(in, AIL_ID)*

Specifies the identifier of a valid 2D graphics list from which to copy or move the graphics. You must have allocated the 2D graphics list using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

### `DstListGraId` *(out, AIL_ID)*

Specifies the identifier of a valid 2D graphics list in which to copy or move the graphics. You must have allocated the 2D graphics list using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). In-place processing is supported; [`DstListGraId`](../../Reference/gra/MgraCopy.md) can refer to the same 2D graphics list as [`SrcListGraId`](../../Reference/gra/MgraCopy.md).

### `Operation` *(in, AIL_INT64)*

Specifies the type of copy operation to perform. This parameter should be set to one of the following values:

*For specifying the type of copy operation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COPY`](../../Reference/gra/MgraCopy.md)+[`M_LABEL_VALUE`](../../Reference/gra/MgraCopy.md). |
| `M_COPY` | Specifies graphics from the source 2D graphics list are copied to the destination 2D graphics list. The graphics are not deleted from the source 2D graphics list. |
| `M_MOVE` | Specifies graphics from the source 2D graphics list are moved to the destination 2D graphics list. The graphics are deleted from the source 2D graphics list. |

*For specifying whether to use indices or labels*

| Value | Description |
| --- | --- |
| `M_INDEX_VALUE` | Specifies index values are given to the [`SrcIndexOrLabelArrayPtr`](../../Reference/gra/MgraCopy.md) parameter. |
| `M_LABEL_VALUE` *(default)* | Specifies labels are given to the [`SrcIndexOrLabelArrayPtr`](../../Reference/gra/MgraCopy.md) parameter. |

### `InsertLocation` *(in, AIL_INT)*

Specifies the location inside the destination 2D graphics list at which to insert the graphics. Note that the indices or labels passed to this parameter must refer to valid graphics. Otherwise, an error is generated.

*For specifying the insert location in the destination 2D graphics list*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_INDEX` | Specifies that the graphics will be inserted before the graphic with the specified index in the destination 2D graphics list. |
| `M_GRAPHIC_LABEL` | Specifies that the graphics will be inserted before the graphic with the specified label in the destination 2D graphics list. |
| `M_END_OF_LIST` *(default)* | Specifies that the graphics are appended to the end of the destination 2D graphics list. |

### `NumGraphics` *(in, AIL_INT)*

Specifies the number of graphics to copy or move.

*For specifying the number of graphics to copy or move*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies that all the graphics from the source 2D graphics list must be copied or moved. |
| `Value > 0` | Specifies the number of graphics from the source 2D graphics list to be copied or moved. |

### `SrcIndexOrLabelArrayPtr` *(in, *AIL_INT)*

Specifies the address of the array containing the indices or labels of the graphics to copy from the source 2D graphics list. If the [`M_INDEX_VALUE`](../../Reference/gra/MgraCopy.md) combination constant is used with the [`Operation`](../../Reference/gra/MgraCopy.md) parameter, Aurora Imaging Library expects the array to contain graphics indices. Otherwise, Aurora Imaging Library expects it to contain graphics labels.

### `DstLabelArrayPtr` *(out, *AIL_INT)*

Specifies the address of the array in which to write the labels that have been automatically given to the copied or moved graphics in the destination 2D graphics list. The labels are written in the same order as the graphics are specified in the [`SrcIndexOrLabelArrayPtr`](../../Reference/gra/MgraCopy.md) parameter. If [`NumGraphics`](../../Reference/gra/MgraCopy.md) is set to [`M_ALL`](../../Reference/gra/MgraCopy.md), the labels are written in index order.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
