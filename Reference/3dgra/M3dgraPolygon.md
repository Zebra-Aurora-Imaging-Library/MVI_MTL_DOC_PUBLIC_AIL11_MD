---
doctype: Reference
module: 3dgra
function: M3dgraPolygon
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraPolygon"
---

# M3dgraPolygon

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

> Add a polygon graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraPolygon(
    AIL_ID             List3dgraId,       //out
    AIL_INT64          ParentLabel,       //in
    AIL_INT64          CreationMode,      //in
    AIL_INT            NumPoints,         //in
    const AIL_DOUBLE * CoordXArrayPtr,    //in
    const AIL_DOUBLE * CoordYArrayPtr,    //in
    const AIL_DOUBLE * CoordZArrayPtr,    //in
    const AIL_DOUBLE * TextureXArrayPtr,  //in
    const AIL_DOUBLE * TextureYArrayPtr,  //in
    AIL_ID             TextureBufId,      //in
    AIL_INT64          ControlFlag        //in
)
```

## Description

This function adds a polygon graphic to the specified 3D graphics list, allowing you to, for example, view the polygon graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the polygon graphic. When the polygon graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the polygon graphic's parent must be the root node. All coordinates are expressed in the coordinate system of the polygon graphic's parent.

You must specify the coordinates of the polygon's vertices using [`CoordXArrayPtr`](../../Reference/3dgra/M3dgraPolygon.md), [`CoordYArrayPtr`](../../Reference/3dgra/M3dgraPolygon.md), and [`CoordZArrayPtr`](../../Reference/3dgra/M3dgraPolygon.md). The coordinates of the polygon's vertices are expressed in the coordinate system of the polygon graphic's parent. Note that the given points must be coplanar, non-self-folding, and non-self-intersecting. The resulting polygon must also be convex. That is, it must not have a concavity or indent around its outer edges, since this can cause unexpected results when working with the polygon graphic.

The polygon graphic has its own coordinate system that represents the polygon's position and orientation with respect to its parent's coordinate system. Initially, the polygon graphic's position and orientation is the identity matrix. This means that the polygon graphic's position and orientation is the same as the position of its parent's coordinate system. You can change the position and orientation of the polygon graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md)with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

You can optionally specify an image buffer to use as a texture on the 3D polygon graphic using the [`TextureBufId`](../../Reference/3dgra/M3dgraPolygon.md) parameter. In this case, you can specify the texture mapping using the [`TextureXArrayPtr`](../../Reference/3dgra/M3dgraPolygon.md) and [`TextureYArrayPtr`](../../Reference/3dgra/M3dgraPolygon.md) parameters. These parameters define the points in the texture image buffer to map to corresponding vertices in the polygon. For more information, see [Working with texture images of polygon 3D graphics](../../UserGuide/C43_3D_Display_and_graphics/S06_Annotating_the_3D_display.md).

> **Note:** Note that the position (0,0) is the center of the top-left pixel of the texture image buffer. To specify the top-left corner of the first pixel, specify (-0.5,-0.5). For more information, see [Pixel units and the pixel coordinate system](../../UserGuide/C02_Building_an_application/S09_Pixel_and_real_world_units.md).

Once the polygon is added, you can specify that a particular value in the texture indicates transparency (using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with [`M_KEYING_COLOR`](../../Reference/3dgra/M3dgraControl.md). This can be used to cut out parts of the polygon graphic.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the polygon graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the polygon graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraPolygon.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the polygon graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `CreationMode` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `NumPoints` *(in, AIL_INT)*

Specifies the number of vertices in the polygon.

*For specifying the number of vertices in the polygon*

| Value | Description |
| --- | --- |
| `Value >= 3` | Specifies the number of vertices in the polygon. |

### `CoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of X-coordinates of the vertices.

### `CoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of Y-coordinates of the vertices.

### `CoordZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of Z-coordinates of the vertices.

### `TextureXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of X-coordinates of the points in the texture image that map to the specified vertices for the texture mapping.

### `TextureYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the list of Y-coordinates of the points in the texture image that map to the specified vertices for the texture mapping.

### `TextureBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to be used as a texture. The image buffer must be an 8-bit, unsigned, 1- or 3-band buffer.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the polygon graphic added to the 3D graphics list.
