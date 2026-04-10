---
doctype: Reference
module: disp
function: MdispPan
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispPan"
---

# MdispPan

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

> Pan (scroll) a display.

## Syntax

```c
void MdispPan(
    AIL_ID     DisplayId,  //out
    AIL_DOUBLE XOffset,    //in
    AIL_DOUBLE YOffset     //in
)
```

## Description

This function associates X- and Y-panning offsets with the specified display. When an image buffer is selected for display, the image is displaced on the display, from the top-left corner of the window (for windowed displays) or the screen (for exclusive displays), according to these offsets.

Note, the offsets ([`XOffset`](../../Reference/disp/MdispPan.md) and [`YOffset`](../../Reference/disp/MdispPan.md)) are in image pixels. Since the current zoom factors affect the displayed size of an image pixel, the panning offsets are also affected by the zoom factors. For example, if the display has an associated X-zoom factor of 4, panning by an X-offset of one image pixel results in panning by 4 pixels in the horizontal direction on the display.

To center the selected image buffer in the display, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_CENTER_DISPLAY`](../../Reference/disp/MdispControl.md), instead of using [`MdispPan`](../../Reference/disp/MdispPan.md).

For windowed and exclusive displays, you can also pan the display interactively using the Page-up/down and arrow keys, if the keys associated with the display have not been disabled using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_KEYBOARD_USE`](../../Reference/disp/MdispControl.md). You can also pan the display by clicking on the image and dragging in the required direction with the mouse, if the mouse associated with the display has not been disabled using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_MOUSE_USE`](../../Reference/disp/MdispControl.md). Note that these features are not enabled by default when using a user-defined window.

Note that the window must have the focus for the keys to work. To give focus to an exclusive display, you must permit the mouse cursor to move over the display, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_RESTRICT_CURSOR`](../../Reference/disp/MdispControl.md).

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the display.

### `XOffset` *(in, AIL_DOUBLE)*

Specifies the number of image pixels by which to pan an image buffer horizontally when it is displayed. Specify the offset relative to the top-left corner of the image buffer. Specify a positive offset value to displace the image to the left.

### `YOffset` *(in, AIL_DOUBLE)*

Specifies the number of image pixels by which to pan an image buffer vertically when it is displayed. Specify the offset relative to the top-left corner of the image buffer. Specify a positive offset value to displace the image upwards.
