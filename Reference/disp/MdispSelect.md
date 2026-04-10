---
doctype: Reference
module: disp
function: MdispSelect
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispSelect"
---

# MdispSelect

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

> Select an image buffer to display.

## Syntax

```c
void MdispSelect(
    AIL_ID DisplayId,  //out
    AIL_ID ImageBufId  //in
)
```

## Description

This function outputs the content of the specified image buffer to the specified Aurora Imaging Library display. You can only display one image buffer at a time on a specific display.

For a windowed display, the image is presented in a window that has the same size as the image, unless such a window would not fit on the desktop or would be too small. If the image is too large to fit in the largest possible window given the selected display resolution, the top-left corner of the image will be aligned with the top-left corner of the largest possible window, and the right and bottom portion of the image, the part that exceeds the window area, will not be displayed; to view the missing portions of the image, you can pan the displayed image (see [Panning and zooming](../../UserGuide/C25_Displaying_an_image/S07_Panning_and_zooming.md) for more information). If the image is smaller in size than the smallest possible window, the image will be centered in the smallest possible window, and the surrounding area will be filled with the background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)). If the window is enlarged or maximized, the image will remain the same size and centered.

For an exclusive display, the image is presented full-screen, without a windowed border or frame, on one of the Windows desktop screens. The image is presented at the center of the screen. If the image is too large to fit on screen given the selected display resolution, the top-left corner of the image will be aligned with the top-left corner of the screen, and the right and bottom portion of the image, the part that exceeds the screen area, will not be displayed; to view the missing portions of the image, you can pan the displayed image (see [Panning and zooming](../../UserGuide/C25_Displaying_an_image/S07_Panning_and_zooming.md) for more information). If the image is smaller in size than the screen area, the image will be centered on screen, and the surrounding area will be filled with the background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)).

For an Aurora Imaging Web Library display, the image is presented in one or more connected instances of an Aurora Imaging Web Library client application (typically running on a remote computer). The size of the transmitted display is the same as the size of the image buffer. If the displayed image is panned or zoomed (see [Panning and zooming](../../UserGuide/C25_Displaying_an_image/S07_Panning_and_zooming.md) for more information) so that it does not fill the display, the surrounding area will be filled with the background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)). The Aurora Imaging Web Library client application might further crop or scale the displayed image after it is transmitted.

For an Aurora Imaging Library WPF display, the image will be displayed in the AILWPFControl with the bound display Id. If the control is larger than the selected image, the remainder of the control if filled with the display's background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)). If the control is smaller than the selected image, the image will be aligned with the top-left corner of the control; to view the rest of the image, you can pan the displayed view (see [Panning and zooming](../../UserGuide/C25_Displaying_an_image/S07_Panning_and_zooming.md) for more information).

You can control the displayed area using [`MdispControl`](../../Reference/disp/MdispControl.md), [`MdispPan`](../../Reference/disp/MdispPan.md), or [`MdispZoom`](../../Reference/disp/MdispZoom.md). For example, you can center the selected image in the display using [`MdispControl`](../../Reference/disp/MdispControl.md)with [`M_CENTER_DISPLAY`](../../Reference/disp/MdispControl.md).

You can remove an image buffer selected to the display using [`MdispSelect`](../../Reference/disp/MdispSelect.md) with [`M_NULL`](../../Reference/disp/MdispSelect.md). For windowed displays, this closes the associated window. For exclusive displays, this causes the display to disappear, allowing the desktop to reappear on the screen. For WPF, if the display was bound to a control, the content of the control is cleared with the display's background color ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_BACKGROUND_COLOR`](../../Reference/disp/MdispControl.md)).

You can only remove the entire image buffer from the display. Therefore, when displaying a parent buffer, you cannot remove one of its child buffers from the display.

Note that it is not necessary to stop displaying the image buffer before selecting another buffer for display.

You can annotate the image selected to the display non-destructively using the display's overlay mechanism ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY`](../../Reference/disp/MdispControl.md)) or using a 2D graphics list ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)). The display's overlay mechanism allows you to annotate the selected image using an automatically allocated, temporary, image buffer referred to as the display's overlay buffer; you can modify the overlay buffer with any function that writes to an image buffer (except a grabbing function). Annotating with a 2D graphics list allows you to annotate the selected image with the vector-based graphics in the 2D graphics list; you can add vector-based graphics to the 2D graphics list using [`Mgra...`](../../Reference/gra/MgraText.md) and [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) functions. Annotating using the latter technique avoids pixelization when the display is zoomed since the graphics are in vector format. For more information on non-destructive annotation see [Annotating the displayed image non-destructively](../../UserGuide/C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md).

If the selected image is calibrated, the image is not physically corrected on display. If the display is associated with an overlay buffer ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY`](../../Reference/disp/MdispControl.md)), the camera calibration information of the selected buffer is copied to the overlay buffer; this allows you to draw in the overlay buffer in world units. If the camera calibration information of the overlay buffer is modified using [`McalAssociate`](../../Reference/cal/McalAssociate.md), [`MbufClear`](../../Reference/buf/MbufClear.md), or some other function, the camera calibration information of the selected buffer will not be updated accordingly. However, changing the camera calibration information of the selected buffer will re-copy the camera calibration information of the selected buffer to the overlay buffer, even if the camera calibration information of the overlay buffer was manually changed.

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the display.

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to display. The image buffer must be a 1- or 3-band buffer. To be displayable, this buffer must be an image buffer that has an [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) + [`M_DISP`](../../Reference/buf/MbufAlloc2d.md) attribute.
