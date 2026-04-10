---
doctype: Reference
module: 3dgra
function: M3dgraText
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraText"
---

# M3dgraText

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

> Add a text graphic to a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dgraText(
    AIL_ID             List3dgraId,    //out
    AIL_INT64          ParentLabel,    //in
    AIL_CONST_TEXT_PTR Text,           //in
    AIL_ID             Matrix3dgeoId,  //in
    AIL_INT64          Options,        //in
    AIL_INT64          ControlFlag     //in
)
```

## Description

This function adds a text graphic to the specified 3D graphics list, allowing you to, for example, view the text graphic on a 3D display.

You must specify the label of the 3D graphic, in the 3D graphics list, to use as the parent of the text graphic. When the text graphic is added to the 3D graphics list's tree structure, it is added as a child under the specified parent. If the 3D graphics list is empty, the text graphic's parent must be the root node.

The text graphic has its own coordinate system that represents the text's position and orientation with respect to its parent's coordinate system. The text is justified at its coordinate system's origin, and is written in the direction of its coordinate system's X-axis, by default, facing the direction of the Z-axis. By default, the text is top- and left-aligned, so the top-left corner of the text is positioned at its coordinate system's origin. You can set the text's position and orientation using the [`Matrix3dgeoId`](../../Reference/3dgra/M3dgraText.md) parameter.

You can set the text's font and size using M3dgraControl with [`M_FONT`](../../Reference/3dgra/M3dgraInquire.md) and [`M_FONT_SIZE`](../../Reference/3dgra/M3dgraInquire.md).

To modify or inquire 3D graphics list settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) or [`M3dgraInquire`](../../Reference/3dgra/M3dgraInquire.md), respectively.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 3D graphics list ([`List3dgraId`](../../Reference/3dgra/M3dgraText.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `List3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to add the text graphic. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `ParentLabel` *(in, AIL_INT64)*

Specifies the label of the parent of the text graphic in the 3D graphics list.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraText.md). |
| `M_ROOT_NODE` | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the parent of the text graphic in the 3D graphics list. Label 0 is the 3D graphics list's root node. |

### `Text` *(in, AIL_CONST_TEXT_PTR)*

Specifies the address of the string containing the text that must be shown in the text graphic.

*For specifying the string*

| Value | Description |
| --- | --- |
| `"String"` | Specifies the address of the null-terminated (\0) ASCII string that must be shown in the text graphic. There is no restriction on the length of the string. |

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix that defines the text graphic's position and orientation with respect to its parent's coordinate system.

*For specifying the transformation matrix object identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_IDENTITY_MATRIX`](../../Reference/3dgra/M3dgraText.md). |
| `M_IDENTITY_MATRIX` | Specifies the identity matrix. This means that the text graphic's position and orientation is the same as the position and orientation of its parent's coordinate system. |
| `Transformation matrix object identifier` | Specifies the identifier of the transformation matrix that defines the text graphic's position and rotation with respect to its parent's coordinate system. The transformation matrix must be of type [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md). |

### `Options` *(in, AIL_INT64)*

Specifies extra options for the appearance of the text graphic.

*For specifying extra options*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the text is written in the direction of the text graphic's coordinate system's X-axis, and that the text faces the direction of the Z-axis of the text graphic's coordinate system.*[Image: 3dgra_text_option_default.png]* |
| `M_FIXED_ROTATION` | Specifies that the text is rotated with the display. The text will always face the viewpoint of the display. This is used to keep text readable regardless of the rotation of the display. |
| `M_FIXED_SCALE` | Specifies that the text is scaled with respect to the display. The text will always appear at the same scale regardless of the zoom or translation of the display. When using this mode, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_FIXED_FONT_SIZE`](../../Reference/3dgra/M3dgraControl.md) to change the font size. |
| `M_FLIP` | Specifies that the text is mirrored across the X-axis. The text faces the direction of the negative Z-axis of the text graphic's coordinate system.*[Image: 3dgra_text_option_flip.png]* |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the text graphic added to the 3D graphics list.
