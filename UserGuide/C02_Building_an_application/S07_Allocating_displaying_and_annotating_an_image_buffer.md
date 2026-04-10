---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Allocating_displaying_and_annotating_an_image_buffer
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Allocating displaying and annotating an image buffer"
---

# Allocating, displaying, and annotating an image buffer

After you have allocated an application context along with any required system(s), you are ready to grab and display an image. This section covers how to allocate and display an image buffer. For the basics required to start grabbing, see [Grabbing images](S08_Grabbing_images.md).

## Allocating an image buffer

Image buffers are storage areas that can hold image data so that it can be displayed, manipulated, grabbed, and/or analyzed. To store image data, Aurora Imaging Library supports the concept of bands; your buffer needs one band per color band, or other type of data, that your image contains. For example, an RGB image needs a buffer with three bands.

You can allocate a monochrome image buffer using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). This function requires that you specify the following image buffer attributes:

- The system on which to allocate the buffer.
- The image buffer's size in X and Y.
- The depth of the buffer: 1-, 8-, 16-, or 32-bit.
- The image buffer's data type. Signed, unsigned, and floating-point buffers are all supported by Aurora Imaging Library.
- The image buffer's intended use (for example, processing or display). An image buffer without an intended use can only be used with the Buffer and Graphics modules.

You can allocate a multi-band image buffer using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). This function requires that you specify the number of bands in addition to the attributes listed above. To operate on a specific band or region of an image buffer, you can use [`MbufChild...`](../../Reference/buf/MbufChild1d.md). For more information on how to deal with color using Aurora Imaging Library, see [Dealing with color](S11_Dealing_with_color.md).

When you allocate an image buffer, you need to specify its intended uses so that it will be appropriately allocated in memory. Passing the identifier of image buffer with no intended use to a function, besides those in the Buffer and Graphics modules, will generate an error. If you intend to use the buffer to store grabbed data, the buffer must be allocated with an [`M_GRAB`](../../Reference/buf/MbufAlloc2d.md) attribute. If you intend to display the buffer, you must allocate it with an [`M_DISP`](../../Reference/buf/MbufAlloc2d.md) attribute. If you intend to use the buffer as the source or destination buffer of a processing or analysis operation, you must allocate the buffer with an [`M_PROC`](../../Reference/buf/MbufAlloc2d.md) attribute. Note that you can combine these attributes so that the buffer can be allocated for multiple uses. Also note that [`M_PROC`](../../Reference/buf/MbufAlloc2d.md) is the only attribute available for non-color multi-band buffers. For more information on non-color buffers, see [Non-color buffers](../C23_Data_buffers/S08_Multi_band_buffers.md).

For more information on buffers and other buffer attributes, see [Data buffers](../C23_Data_buffers/ChapterInformation.md).

## Displaying an image buffer

Especially during application development, it is useful to display the image buffer that you are manipulating. You must first allocate an Aurora Imaging Library 2D display on the target system, using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md). If you have allocated a displayable image buffer ([`M_DISP`](../../Reference/buf/MbufAlloc2d.md)), select it to this 2D display, using [`MdispSelect`](../../Reference/disp/MdispSelect.md); you can use the same function to stop displaying it.

> **Note:** Note that the image buffer and the 2D display should be allocated on the same system.

## Annotating an image on display

You can also annotate an image on display. This can be done either destructively, which alters the original image, or non-destructively, which maintains the original image but displays annotations placed on top of it. To draw directly in an image, which replaces the image's original pixel values with the newly drawn values, you can use the Aurora Imaging Library 2D graphics functions. For non-destructive image annotation, Aurora Imaging Library offers two main options: using a 2D graphics list and using the overlay mechanism.

To draw directly in an image, you can use the functions in the Aurora Imaging Library 2D graphics module, such as [`MgraArcFill`](../../Reference/gra/MgraArcFill.md) or [`MgraText`](../../Reference/gra/MgraText.md) in the following example. You can specify a default 2D graphics context that uses default settings for background and foreground color and text alignment. Alternatively, you can allocate a new 2D graphics context. For both the default and user-allocated 2D graphics contexts, you can specify the graphics settings and these contexts can be re-used for multiple Aurora Imaging Library 2D graphics functions.

To annotate an image non-destructively, you can allocate a 2D graphics list, using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), populate it with graphics created using Aurora Imaging Library 2D graphics functions or the draw functions of other Aurora Imaging Library modules, and associate the 2D graphics list with the 2D display using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md). Once the 2D graphics list is associated with the 2D display, the graphics are drawn over the image buffer without altering the image buffer itself. When annotating with a 2D graphics list, zooming the 2D display will not affect the size or orientation of the annotations.

You can also use the overlay mechanism to annotate an image non-destructively, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md). Once you have enabled the overlay mechanism, a temporary overlay buffer is allocated which you can populate with annotations using, for example, the Aurora Imaging Library 2D graphics module or Windows GDI annotations. You can directly access this temporary overlay buffer using [`MdispInquire`](../../Reference/disp/MdispInquire.md) with [`M_OVERLAY_ID`](../../Reference/disp/MdispInquire.md).

The overlay buffer has a transparency (keying) color that helps determine whether the overlay buffer's pixels or the image buffer's pixels will be displayed. The pixels in the overlay buffer that are set to the transparency color are rendered invisible, displaying the underlying image buffer's pixels. Every pixel in the overlay buffer that has a color other than the transparency color will be displayed. You can determine the transparency color using [`MdispInquire`](../../Reference/disp/MdispInquire.md) with [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispInquire.md), and you can select another color using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispControl.md).

When annotating with the overlay mechanism, zooming the 2D display will resize the annotations. This can lead to pixelation, the loss of some annotations, or changes in line thickness.

The following code snippet shows you how to allocate, display, and annotate an image buffer. Note that the annotations are destructive because there is neither a 2D graphics list specified, nor is the overlay mechanism enabled.

> **Code example:** [userguide.image_buffer_allocation_example](userguide.image_buffer_allocation_example)

> **Note:** For more information on displaying and annotating an image, see [Displaying an image](../C25_Displaying_an_image/ChapterInformation.md). To allocate a display on a Distributed Aurora Imaging Library remote system, see [Remote displays](../C63_Distributed_AIL/S04_Controlling_configuration.md). For more information on Aurora Imaging Library 2D graphics functions and 2D graphics lists, see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md).
