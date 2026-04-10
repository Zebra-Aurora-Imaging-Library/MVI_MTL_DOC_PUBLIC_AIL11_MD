---
doctype: Reference
module: disp
function: MdispSelectWindow
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispSelectWindow"
---

# MdispSelectWindow

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

> Select an image buffer to display in a user-defined window.

## Syntax

```c
void MdispSelectWindow(
    AIL_ID            DisplayId,          //out
    AIL_ID            ImageBufId,         //in
    AIL_WINDOW_HANDLE ClientWindowHandle  //in
)
```

## Description

This function outputs the content of the specified image buffer to the specified user-defined window, using the specified Aurora Imaging Library display.

If the specified image buffer is smaller in size than the target window size, the border outside the image is not modified. If the specified buffer is larger in size than the target window, the right and bottom portion of the buffer, the part that exceeds the window, is not displayed.

You can control the displayed area using [`MdispControl`](../../Reference/disp/MdispControl.md), [`MdispPan`](../../Reference/disp/MdispPan.md), or [`MdispZoom`](../../Reference/disp/MdispZoom.md). For example, you can center the selected image in the display using [`MdispControl`](../../Reference/disp/MdispControl.md)with [`M_CENTER_DISPLAY`](../../Reference/disp/MdispControl.md).

To remove an image buffer selected to the display using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md), you can use [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) with [`M_NULL`](../../Reference/disp/MdispSelectWindow.md). This leaves the associated window open but leaves it blank.

Note that, to use a display with a window created using the third-party GTK, Tkinter, or Qt frameworks, you should enable a special mode. You can enable the mode using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GTK_MODE`](../../Reference/disp/MdispControl.md), [`M_TK_MODE`](../../Reference/disp/MdispControl.md), or [`M_QT_MODE`](../../Reference/disp/MdispControl.md), respectively. These modes prevent the display from appearing incorrectly when the window is moved or resized, while introducing a small amount of latency.

> **Note:** This function only supports local windowed displays ([`MdispAlloc`](../../Reference/disp/MdispAlloc.md) with [`M_WINDOWED`](../../Reference/disp/MdispAlloc.md) and without [`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md)). To select an image buffer to all other display types, use [`MdispSelect`](../../Reference/disp/MdispSelect.md).

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the display. The display must have been previously allocated using MdispAlloc with [`M_WINDOWED`](../../Reference/disp/MdispAlloc.md) (and without [`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md)).

### `ImageBufId` *(in, AIL_ID)*

Specifies the image buffer to display. The image buffer must be a 1- or 3-band buffer. To be displayable, this buffer must be an image buffer that has an [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) + [`M_DISP`](../../Reference/buf/MbufAlloc2d.md) attribute.

### `ClientWindowHandle` *(in, AIL_WINDOW_HANDLE)*

Specifies the window handle of the user-defined window or child window. This window must have a window handle of type _HWND_ or X11. For example, the Windows API functions can create a window with an _HWND_ handle, and third-party functions (like Qt) can create an X11 window handle. If this parameter is set to zero, this function behaves like [`MdispSelect`](../../Reference/disp/MdispSelect.md).
