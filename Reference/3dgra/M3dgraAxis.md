---
doctype: Reference
module: 3dgra
function: M3dgraAxis
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraAxis"
---

# M3dgraAxis

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

> Add an axis graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraAxis(
    AIL_ID             List3dgraId,    //out
    AIL_INT64          ParentLabel,    //in
    AIL_ID             Matrix3dgeoId,  //in
    AIL_DOUBLE         AxisLength,     //in
    AIL_CONST_TEXT_PTR Name,           //in
    AIL_INT64          TextOptions,    //in
    AIL_INT64          ControlFlag     //in
)
```

## Description

This function adds an axis graphic to the specified 3D graphics list, allowing you to, for example, view the axis graphic on a 3D display.

Axis graphics consist of three orthogonal right-handed axes (X, Y, and Z) that intersect at a single point. This 3D graphic can be used to represent a coordinate system.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the axis graphic. When the axis graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the axis graphic's parent must be the root node.

The axis graphic has its own coordinate system that represents the position and orientation of the axis graphic's axes with respect to its parent's coordinate system. The origin of the axis graphic's coordinate system is the origin of the axes, and its orientation is the orientation of the axes. Therefore, the axis graphic's coordinate system is the axis graphic itself. You can set the axis graphic's position and orientation using the [`Matrix3dgeoId`](../../Reference/3dgra/M3dgraAxis.md) parameter. You can change the position and orientation of the axis graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraAxis.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the axis graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the axis graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraAxis.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the axis graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix that defines the axis graphic's position and orientation with respect to its parent's coordinate system.

*For specifying the transformation matrix object identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_IDENTITY_MATRIX`](../../Reference/3dgra/M3dgraAxis.md). |
| `M_IDENTITY_MATRIX` | Specifies the identity matrix. This means that the axis graphic's position and orientation is the same as the position and orientation of its parent's coordinate system. |
| `Tansformation matrix object identifier` | Specifies the identifier of the transformation matrix that defines the axis graphic's position and orientation with respect to its parent's coordinate system. The transformation matrix must be of type [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md). |

### `AxisLength` *(in, AIL_DOUBLE)*

Specifies the length of the axes.

*For specifying the length of the axes*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the length of the axes, in world units. |

### `Name` *(in, AIL_CONST_TEXT_PTR)*

Specifies an optional name for the axis graphic (for example, "Working coordinate system"). If this parameter is not empty, this creates a text graphic with the axis graphic as its parent, and the axis name as its text.

*For specifying the string*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies not to give the axis graphic a name. |
| `"String"` | Specifies the address of the null-terminated (\0) ASCII string that must be shown in the text graphic. There is no restriction on the length of the string. |

### `TextOptions` *(in, AIL_INT64)*

Specifies extra options for the appearance of the [`Name`](../../Reference/3dgra/M3dgraAxis.md) text graphic. If [`Name`](../../Reference/3dgra/M3dgraAxis.md) is set to an empty string, or to [`M_NULL`](../../Reference/3dgra/M3dgraAxis.md), this parameter is ignored.

*For specifying extra options*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the text is written in the direction of the text graphic's coordinate system's X-axis, and that the text faces the direction of the Z-axis of the text graphic's coordinate system.*[Image: 3dgra_text_option_default.png]* |
| `M_FIXED_ROTATION` | Specifies that the text is rotated with the display. |
| `M_FIXED_SCALE` | Specifies that the text is scaled with respect to the display. |
| `M_FLIP` | Specifies that the text is mirrored across the X-axis. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the axis graphic added to the 3D graphics list.
