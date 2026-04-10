---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Mapping_1band_images_through_a_LUT_upon_display
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Mapping 1band images through a LUT upon display"
---

# Mapping 1-band images through a LUT upon display

Upon display, you can map 1-band images through a specified LUT to control their gray levels or to display them in color; the LUT maps the image pixels to precalculated values on display. The following are some situations for which you can use a LUT to produce required display effects:

- When displaying monochrome images, you might want to view the images with each gray intensity in a different color. For example, you can associate specific colors to ranges of temperatures obtained by an infrared camera.
- When displaying monochrome images, you might want to invert the image values. For example, when grabbing a film negative, you can negate the video and display the film as it will be printed.

## Selecting the LUT to use for display

Upon display, you can map all 1-band image buffers selected to a specified display through a specified LUT, or you can map only a specified 1-band image buffer through a LUT. To do so:

1. Allocate an 8-bit LUT buffer using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) with [`M_LUT`](../../Reference/buf/MbufAlloc1d.md). The LUT buffer must have `2<sup> _depth of image buffer(s) to map_ </sup>`number of entries (for example, to map 8-bit images, the LUT buffer should have 256 entries).
2. Generate the data into the LUT buffer using [`MgenLutRamp`](../../Reference/gen/MgenLutRamp.md), or load the data into it, using [`MbufPut`](../../Reference/buf/MbufPut.md).
3. Associate the LUT buffer with the required display using [`MdispLut`](../../Reference/disp/MdispLut.md), or to a particular image buffer using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_ASSOCIATED_LUT`](../../Reference/buf/MbufControl.md). In the former case, all image buffers selected to the specified display are mapped through the LUT. In the latter case, only the specified image buffer is mapped and when the image buffer is saved, the LUT data is saved with it.

Depending on the required display effect, associate either a 1-band or 3-band LUT buffer with the display or image buffer. For example, to invert the values of an image on display, use a 1-band [`M_LUT`](../../Reference/buf/MbufAllocColor.md) buffer that maps each pixel to the maximum pixel value minus the current pixel value; you can use [`MgenLutRamp`](../../Reference/gen/MgenLutRamp.md) to generate the LUT data for such a mapping. To view a 1-band image buffer with each gray intensity in a different color, use a 3-band [`M_LUT`](../../Reference/buf/MbufAllocColor.md) buffer; to achieve this effect, you can also use the [`MdispLut`](../../Reference/disp/MdispLut.md) predefined 3-band pseudo-color LUT buffer, [`M_PSEUDO`](../../Reference/disp/MdispLut.md), instead of allocating your own LUT buffer.

If both the display and the selected image buffer have an associated LUT buffer, the one associated with the display is respected.

To disassociate a LUT buffer from a display or image buffer, use [`MdispLut`](../../Reference/disp/MdispLut.md) with [`M_DEFAULT`](../../Reference/disp/MdispLut.md), or [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_ASSOCIATED_LUT`](../../Reference/buf/MbufControl.md) and [`M_DEFAULT`](../../Reference/disp/MdispLut.md), respectively.

## Restrictions when displaying using LUT

When displaying using a LUT, note the following:

- If the content of a LUT buffer changes while the image is selected on the display, the changes will not take effect until you call [`MdispLut`](../../Reference/disp/MdispLut.md) again.
- You cannot associate a multi-band image buffer with a LUT buffer, nor can you select a multi-band image buffer to a display that is associated with a LUT buffer. In addition, the image buffer must be an 8- or 16-bit buffer. If these conditions are not respected, an error is generated.
- The view mode of the display cannot be set to [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md) or [`M_MULTI_BYTES`](../../Reference/disp/MdispControl.md).
- The number of LUT buffer entries must be the same as the maximum number of intensities that can be represented in the displayed image buffer. In other words, if you want to map an 8-bit image buffer (that is, an image that can have 256 intensities), your LUT buffer must also have 256 entries.
