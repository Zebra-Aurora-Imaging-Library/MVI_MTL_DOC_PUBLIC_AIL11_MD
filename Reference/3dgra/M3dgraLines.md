---
doctype: Reference
module: 3dgra
function: M3dgraLines
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dgra / M3dgraLines"
---

# M3dgraLines

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

> Add a lines graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraLines(
    AIL_ID             List3dgraId,               //out
    AIL_INT64          ParentLabel,               //in
    AIL_INT64          CreationMode,              //in
    AIL_INT            NumLines,                  //in
    const AIL_DOUBLE * PointsXArrayPtr,           //in
    const AIL_DOUBLE * PointsYArrayPtr,           //in
    const AIL_DOUBLE * PointsZArrayPtr,           //in
    const AIL_DOUBLE * PointsOrVectorsXArrayPtr,  //in
    const AIL_DOUBLE * PointsOrVectorsYArrayPtr,  //in
    const AIL_DOUBLE * PointsOrVectorsZArrayPtr,  //in
    const AIL_UINT8 *  LinesRArrayPtr,            //in
    const AIL_UINT8 *  LinesGArrayPtr,            //in
    const AIL_UINT8 *  LinesBArrayPtr,            //in
    AIL_DOUBLE         Length,                    //in
    AIL_INT64          ControlFlag                //in
)
```

## Description

This function adds a lines graphic to the specified 3D graphics list, allowing you to, for example, view the lines graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the lines graphic. When the lines graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the lines graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the lines graphic's parent.

The lines graphic has its own coordinates system that represents the lines graphic's position and orientation with respect to its parent's coordinate system. The origin of the lines graphic's coordinate system and axes directions are the same as the lines graphic's parent. You can change the position and orientation of the lines graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

To create a single line without the option of creating multiple lines, use [`M3dgraLine`](../../Reference/3dgra/M3dgraLine.md).

Unlike [`M3dgraLine`](../../Reference/3dgra/M3dgraLine.md), the length of the lines in a lines graphic cannot be infinite.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraLines.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the lines graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the lines graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraLines.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the lines graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Specifies how the lines graphic is defined.

### `NumLines` *(in, AIL_INT)*

Specifies the number of lines in the lines graphic.

### `PointsXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinate(s) of the start points used to define the lines.

### `PointsYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinate(s) of the start points used to define the lines.

### `PointsZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Z-coordinate(s) of the start points used to define the lines.

### `PointsOrVectorsXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing either the X-coordinates of the end points or X-components of the direction vectors used to define the lines.

### `PointsOrVectorsYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing either the Y-coordinates of the end points or Y-components of the direction vectors used to define the lines.

### `PointsOrVectorsZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing either the Z-coordinates of the end points or Z-components of the direction vectors used to define the lines.

### `LinesRArrayPtr` *(in, *AIL_UINT8)*

Specifies the list of red values for the colors of the lines.

### `LinesGArrayPtr` *(in, *AIL_UINT8)*

Specifies the list of green values for the colors of the lines.

### `LinesBArrayPtr` *(in, *AIL_UINT8)*

Specifies the list of blue values for the colors of the lines.

### `Length` *(in, AIL_DOUBLE)*

Specifies to override the default line length.

*For specifying to override the default line length*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default length defined by the creation mode. |
| `Value > 0.0` | Specifies to override the line's length with a specific value. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the lines graphic

---

### `M_POINT_AND_VECTOR`

Defines the lines graphic using a start point and nonzero direction vector for each required line.  By default, each line's length is set to its respective vector's magnitude.

---

### `M_TWO_POINTS`

Defines the lines graphic using two arrays of points that define start and end points of the lines; the start and end points must be non-identical.  By default, the lengths of the lines defined as the distance between the two specified points.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the lines graphic added to the 3D graphics list.
