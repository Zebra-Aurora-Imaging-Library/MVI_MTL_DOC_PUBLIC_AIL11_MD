---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Panning_and_zooming
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Panning and zooming"
---

# Panning and zooming

At times, your image buffer might be larger than the display, or have details that are too fine or too small to see. You can associate panning and zooming effects with the display to view specific parts of the image. Panning displaces an image horizontally and/or vertically on the display; panning is also known as scrolling. Zooming replicates or subsamples the pixels of an image upon display by a specified factor.

You can explicitly specify the amount to pan and zoom; alternatively, for windowed and exclusive displays, you can allow a user to pan and zoom the display interactively.

To show the zoom factor in the display's title bar, press `Ctrl+O` on the keyboard. The zoom factor will appear (for example, [2:1] for a twice zoomed-in display). To hide the zoom factor, press `Ctrl+Shift+O`.

## Explicitly specifying the amount to pan and zoom

Regardless of the display type, you can explicitly specify the amount to pan and zoom the display.

To pan displayed images, use [`MdispPan`](../../Reference/disp/MdispPan.md) and specify the required X- and Y-panning offsets in image pixels. To center images in the display, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_CENTER_DISPLAY`](../../Reference/disp/MdispControl.md) instead.

To zoom displayed images in the horizontal and/or vertical direction with floating-point precision, use [`MdispZoom`](../../Reference/disp/MdispZoom.md) with the required X- and Y-zoom factors; to reduce the size of the displayed images, specify a zoom factor less than 1.

The specified zoom factors also affect the panning offsets since these offsets are specified in image pixels. For example, if the X-zoom factor is set to 4, panning by an X-offset of one image pixel results in panning by 4 pixels in the horizontal direction on the display.

Instead of explicitly specifying a zoom factor, you can automatically scale the displayed images to fit the display, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_SCALE_DISPLAY`](../../Reference/disp/MdispControl.md). [`M_SCALE_DISPLAY`](../../Reference/disp/MdispControl.md) ensures that the image aspect ratio is maintained. If you use [`M_SCALE_DISPLAY`](../../Reference/disp/MdispControl.md), the explicitly specified zoom factors ([`MdispZoom`](../../Reference/disp/MdispZoom.md)) and pan settings ([`MdispPan`](../../Reference/disp/MdispPan.md)) will be overwritten.

For an example of zooming, refer to [mimconvolve.cpp](../../Examples/mimconvolve.cpp.md), where the display is zoomed by a factor of 2 to better demonstrate the result of an image processing operation (spatial filtering operation).

## Panning and zooming the display interactively

With windowed and exclusive displays, it is possible to pan and zoom images interactively, using either the mouse or the keyboard. You can use the mouse wheel to zoom in or out of the image; to do so, scroll the wheel up or down, respectively. In addition, you can pan the image by clicking on the image and dragging in the required direction.

You can also use the keyboard to pan and zoom the display interactively. To do so, you can use the keys associated with the Aurora Imaging Library display.

|   |   |
| --- | --- |
| + | Increases the X- and Y-zoom factors. |
| - | Decreases the X- and Y-zoom factors. |
| Page-up | Scrolls the image buffer up to the previous display section. |
| Page-down | Scrolls the image buffer down to the next display section. |
| Up arrow | Scrolls the image buffer up to the previous row. |
| Down arrow | Scrolls the image buffer down to the next row. |
| Left arrow | Scrolls the image buffer left by one pixel. |
| Right arrow | Scrolls the image buffer right by one pixel. |
| Ctrl-Up arrow | Scrolls the image buffer up to the previous display section. |
| Ctrl-Down arrow | Scrolls the image buffer down to the next display section. |
| Ctrl-Left arrow | Scrolls the image buffer left to the previous display section. |
| Ctrl-Right arrow | Scrolls the image buffer right to the next display section. |

> **Note:** Note that the window must have the focus for the keys to work. To give focus to an exclusive display, you must permit the mouse cursor to move over the display, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_RESTRICT_CURSOR`](../../Reference/disp/MdispControl.md).

By default, mouse and keyboard use is enabled for Aurora Imaging Library windowed and exclusive displays and disabled for user-defined windowed displays. To enable or disable mouse use in the display, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_MOUSE_USE`](../../Reference/disp/MdispControl.md). You can also set whether the mouse cursor changes shape if interactive panning or zooming is possible using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_MOUSE_CURSOR_CHANGE`](../../Reference/disp/MdispControl.md). To enable or disable keyboard use, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_KEYBOARD_USE`](../../Reference/disp/MdispControl.md).
