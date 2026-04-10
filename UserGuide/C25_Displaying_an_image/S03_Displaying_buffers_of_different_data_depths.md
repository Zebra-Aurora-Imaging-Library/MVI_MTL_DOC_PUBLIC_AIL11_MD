---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Displaying_buffers_of_different_data_depths
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Displaying buffers of different data depths"
---

# Displaying buffers of different data depths

Displayable image buffers usually have a depth of 8 bits (or 8 bits per band, in the case of color images). You can also display images of other depths (for example, 1-bit or 16-bit images). Using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md), you can control the way such buffers are actually displayed.

The [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) control type provides different modes of displaying non 8-bit images:

- [`M_BIT_SHIFT`](../../Reference/disp/MdispControl.md).
- [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md).
- [`M_MULTI_BYTES`](../../Reference/disp/MdispControl.md).
- [`M_TRANSPARENT`](../../Reference/disp/MdispControl.md).
- [`M_DEFAULT`](../../Reference/disp/MdispControl.md).

Note that [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) is not available for network displays.

## M_BIT_SHIFT

The [`M_BIT_SHIFT`](../../Reference/disp/MdispControl.md) setting will right bit-shift the pixel values of the image by the specified number of bits upon updating the display. Specify the number of bits by which to shift using the [`M_VIEW_BIT_SHIFT`](../../Reference/disp/MdispControl.md) control type.

*[Image: M_BIT_SHIFT.png]*

## M_AUTO_SCALE

The [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md) setting remaps the pixel values to the display range such that the minimum and maximum values in the image (not the full range of the buffer) are set to 0 and 255, respectively. [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md) is the default setting of [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) when displaying a 1-bit buffer.

*[Image: M_AUTO_SCALE.png]*

If the image buffer contains a single value, its corresponding displayed value is determined by linearly remapping the full range of the buffer (for example, (0 to 64K) to (0 to 255)).

*[Image: M_AUTO_SCALE_1.png]*

> **Note:** Aurora Imaging Library Lite does not support displaying a 32-bit image buffer in a display that has its [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) control type set to [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md), unless you have purchased the Aurora Imaging Library Image Analysis license.

## M_MULTI_BYTES

The [`M_MULTI_BYTES`](../../Reference/disp/MdispControl.md) setting is primarily useful when grabbing from a multi-tap camera. This setting displays each byte of the image in separate display pixels. For instance, each pixel of a 16-bit image will occupy two consecutive display pixels; each pixel of a 32-bit image will occupy four consecutive display pixels. This mode is only supported for 16-bit and 32-bit 1-band images.

*[Image: M_MULTI_BYTES.png]*

## M_TRANSPARENT

The [`M_TRANSPARENT`](../../Reference/disp/MdispControl.md) setting will display only the 8 least-significant bits of the image. No pixel remapping is performed. This is the default setting unless a 1-bit buffer is used.
