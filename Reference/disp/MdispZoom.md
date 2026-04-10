---
doctype: Reference
module: disp
function: MdispZoom
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispZoom"
---

# MdispZoom

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

> Zoom a display.

## Syntax

```c
void MdispZoom(
    AIL_ID     DisplayId,  //out
    AIL_DOUBLE XFactor,    //in
    AIL_DOUBLE YFactor     //in
)
```

## Description

This function associates a zoom factor in X and/or in Y with the specified display. When an image buffer is selected for display, it will be zoomed according to these factors. The image buffer is displayed starting from its top-left corner, unless panning is also associated with the display (using [`MdispPan`](../../Reference/disp/MdispPan.md)).

Instead of explicitly specifying a zoom factor, you can automatically scale the selected image buffer to fit the display, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_SCALE_DISPLAY`](../../Reference/disp/MdispControl.md). [`M_SCALE_DISPLAY`](../../Reference/disp/MdispControl.md) ensures that the image aspect ratio is maintained. If you use [`M_SCALE_DISPLAY`](../../Reference/disp/MdispControl.md), the explicitly specified zoom factors ([`MdispZoom`](../../Reference/disp/MdispZoom.md)) and pan settings ([`MdispPan`](../../Reference/disp/MdispPan.md)) will be overwritten.

For windowed and exclusive displays, you can also zoom the display interactively using the + and - keys, if the keys associated with the display have not been disabled using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_KEYBOARD_USE`](../../Reference/disp/MdispControl.md).

Note that the window must have the focus for the keys to work. To give focus to an exclusive display, you must permit the mouse cursor to move over the display, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_RESTRICT_CURSOR`](../../Reference/disp/MdispControl.md).

Once you have permitted the mouse cursor to move over the display, you can use the mouse wheel to zoom in or out of the image interactively; to do so, scroll the wheel up or down, respectively.

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the display.

### `XFactor` *(in, AIL_DOUBLE)*

Specifies the zoom factor for the X-direction of the display. This parameter must be set to one of the following values:

*For specifying the zoom factor in the X-direction*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the zoom factor for the X-direction of the display. A value greater than 1.0 will zoom in, while a factor less than 1.0 will zoom out. |

### `YFactor` *(in, AIL_DOUBLE)*

Specifies the zoom factor for the Y-direction of the display. This parameter must be set to one of the following values:

*For specifying the zoom factor in the Y-direction*

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the zoom factor for the Y-direction of the display. A value greater than 1.0 will zoom in, while a factor less than 1.0 will zoom out. |
