---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Annotating_the_displayed_image_nondestructively
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Annotating the displayed image nondestructively"
---

# Annotating the displayed image non-destructively

There are four ways to annotate an image on display:

- Using Aurora Imaging Library's overlay mechanism.
- Using a 2D graphics list.
- Using GDI annotations.
- Using a region of interest (ROI).

## Annotating images using the overlay mechanism

For all types of displays, you can annotate the displayed image non-destructively using Aurora Imaging Library's overlay mechanism. To make use of this functionality, do the following:

1. Enable the overlay mechanism, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md).
2. Select an image buffer to the display, using [`MdispSelect`](../../Reference/disp/MdispSelect.md).
   Since the overlay mechanism is enabled, this will not only display the selected image, but it will also associate a temporary overlay buffer with the display. This buffer is referred to as the **display's overlay buffer**. The overlay buffer can be used to annotate the underlying image with an effect called transparency, or keying. For more information, see [Transparency (Keying)](S08_Annotating_the_displayed_image_nondestructively.md).
3. To access the display's overlay buffer, use [`MdispInquire`](../../Reference/disp/MdispInquire.md) with [`M_OVERLAY_ID`](../../Reference/disp/MdispInquire.md) to determine the Aurora Imaging Library identifier of the buffer.
4. Annotate the display's overlay buffer as required. For example, to write text in the overlay buffer, you can use [`MgraText`](../../Reference/gra/MgraText.md). Note that since this temporary overlay buffer is a real image buffer, any function (except grabbing) can be used.
   You can also annotate the displayed image buffer or the overlay buffer with Windows GDI annotations. For information, see [Using GDI annotations](S08_Annotating_the_displayed_image_nondestructively.md).

The overlay buffer will have the same size as its associated image (not the size of the display). In addition, for windowed displays, it will have the same number of bands and depth as the desktop; for exclusive displays, it will have the same number of bands and depth as the selected display format.

### Overlay buffer behavior

When an image is selected to a display that has an associated overlay buffer, and you select another image to that display, one of the following occurs:

- If the new image has the same format and size as the image currently selected to that display, the current overlay buffer is not freed. Any annotations will, therefore, remain until you clear the overlay buffer (either using [`MbufClear`](../../Reference/buf/MbufClear.md), or using[`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY_CLEAR`](../../Reference/disp/MdispControl.md)).
- If the new image has a different format or size than the image currently selected to that display, the contents of the current overlay buffer are copied to a temporary buffer, the current overlay buffer is freed, another overlay buffer is created, and the annotations of the old overlay buffer are copied from the temporary buffer into the new overlay buffer. The new overlay buffer has the same size as the new image selected to the display.
  The identifier of the old overlay buffer is now obsolete. To inquire the identifier of this newly created overlay buffer, use [`MdispInquire`](../../Reference/disp/MdispInquire.md) with the [`M_OVERLAY_ID`](../../Reference/disp/MdispInquire.md) inquire type.

### Camera calibration and overlay considerations

Aurora Imaging Library tries to keep the camera calibration information of the overlay buffer synchronized with the camera calibration information of the buffer selected to the display.

> **Note:** Note that camera calibration is not available with Aurora Imaging Library Lite.

Aurora Imaging Library will copy the camera calibration information of the selected buffer to the overlay buffer at the following moments:

- When the overlay buffer is allocated.
- When a buffer is selected to the display.
- When the camera calibration information of the selected buffer is modified.

Note that if the camera calibration information of the overlay buffer is modified with [`McalAssociate`](../../Reference/cal/McalAssociate.md), [`MbufClear`](../../Reference/buf/MbufClear.md), or some other function, the camera calibration information of the selected buffer will not be updated accordingly. However, changing the camera calibration information of the selected buffer will re-copy the camera calibration information of the selected buffer to the overlay buffer, even if the camera calibration information of the overlay buffer was manually changed.

### Transparency (keying)

The display's overlay buffer annotates the image selected to the display using transparency. Transparency, also known as keying, is a mechanism that replaces pixels of an image that are of the specified transparency (keying) color with the underlying areas of another image. Using transparency, annotations made to the overlay buffer in a color other than the transparency color will annotate the image selected to the display.

*[Image: Transparent_color.png]*

When allocating a display ([`MdispAlloc`](../../Reference/disp/MdispAlloc.md)), the transparency color is automatically set to a default color, which is generally appropriate. Use [`MdispInquire`](../../Reference/disp/MdispInquire.md) with [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispInquire.md) to determine the transparency color. If required, select another transparency color using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispControl.md).

If you are using an 8-bit display resolution (256 colors), set the transparency color to a value between 0 and 255. If you are using a non 8-bit display resolution (15-bit, 16-bit, 24-bit, or 32-bit color resolution), call the macro [`M_RGB888()`](../../Reference/disp/MdispControl.md) and specify the RGB value. For example:

> **Code example:** [userguide.displaying_an_image.annotating_the_displayed_image_non_destructively04](userguide.displaying_an_image.annotating_the_displayed_image_non_destructively04)

When the display's overlay buffer is created, it is cleared to the effective transparency color. If the transparency color is changed after the overlay buffer is created, the buffer will not be cleared to the new color.

> **Note:** In addition to making parts of the overly buffer transparent using keying, you can change the opacity of the annotations in the overlay buffer using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY_OPACITY`](../../Reference/disp/MdispControl.md). For more information, see [Opacity](S08_Annotating_the_displayed_image_nondestructively.md).

## Using a 2D graphics list

You can associate a 2D graphics list to the display. In this case, the vector-based graphics, contained within the 2D graphics list, will be used to annotate the display non-destructively. To perform such annotations, you must specify a valid 2D graphics list identifier, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md). You must have previously allocated the 2D graphics list, using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), and added one or more graphics to it, using Aurora Imaging Library graphic functions (for example, [`MgraText`](../../Reference/gra/MgraText.md) or using the [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) functions). For more information, see [2D graphics list](../C26_Generating_graphics/S06_Graphics_list.md).

By default, modifications to the graphics within the list (added, deleted, or altered graphics) will be immediately reflected in the non-destructive annotation of the display. To change this behavior, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_UPDATE_GRAPHIC_LIST`](../../Reference/disp/MdispControl.md). To release the 2D graphics list from memory, use [`MgraFree`](../../Reference/gra/MgraFree.md). If the associated 2D graphics list is freed, it is automatically disassociated from the display.

For information about interactively modifying the graphics within the list, see [Creating and modifying graphics interactively](../C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md).

When you annotate the display non-destructively using a 2D graphics list, the zooming behavior of the display will be different than if you had annotated the display using the typical overlay mechanism. With the overlay mechanism, annotations are zoomed by resizing the overlay pixels, which can cause pixelation (loss of clarity), the loss of some annotations, or a change in line thickness. However, such effects do not occur when zooming a display that has 2D graphics list annotations (which are vector-based).

*[Image: GraphicsZoomAnnotation.png]*

Notice how, in the example above, neither the size of the text nor the quality of the rectangle is affected by the zoom factor, for annotations with a 2D graphics list.

In addition, using a 2D graphics list, you can draw anywhere on the display. It does not have to be over the image buffer selected to the display.

> **Note:** You can change the opacity of the annotations in the 2D graphics list using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY_OPACITY`](../../Reference/disp/MdispControl.md). For more information, see [Opacity](S08_Annotating_the_displayed_image_nondestructively.md).

## Opacity

When annotating a display using an overlay buffer or 2D graphics list, you can set the opacity of the annotations. To do so, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_OVERLAY_OPACITY`](../../Reference/disp/MdispControl.md) (for the overlay buffer) or [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispControl.md) (for the 2D graphics list) to set the opacity.

Opacity is expressed as a percentage. A value of 100 specifies to show the annotations as completely opaque, and an opacity of 0 specifies to show the annotations as completely transparent (the annotations will not be visible).

*[Image: disp_annotations_opacity.png]*

> **Note:** For an overlay buffer, pixels that you have made transparent using keying remain completely transparent regardless of this setting.

## Using GDI annotations

Besides using Aurora Imaging Library functions (for example, [`MgraText`](../../Reference/gra/MgraText.md)) to annotate your display, you can draw GDI annotations in the buffer selected to the display or in the display's overlay buffer.

To perform this technique with a Windows operating system:

1. Allocate a device context (DC) for either the image buffer or the overlay buffer of the display using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_DC_ALLOC`](../../Reference/buf/MbufControl.md). Windows GDI annotation functions require a DC to draw. You cannot allocate a DC for an image buffer that is not associated with a display. If you draw using the DC of the image buffer selected to the display, drawing will be destructive (that is, the data of the image buffer is actually changed).
2. Inquire the identifier of the device context (DC) created in the previous step using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with the [`M_DC_HANDLE`](../../Reference/buf/MbufInquire.md) inquire type.
3. Paint your annotations using Windows GDI annotation functions with the DC.
4. Free the device context (DC), using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_DC_FREE`](../../Reference/buf/MbufControl.md).
5. Notify Aurora Imaging Library that the display needs to be updated with the GDI annotations, using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MODIFIED`](../../Reference/buf/MbufControl.md).

When drawing GDI annotations using this technique, the images are drawn using the buffer's coordinates, and the annotations will follow the underlying image when the display is zoomed or panned. However, the size of the overlay buffer is limited to the size of the selected image buffer. It is not possible to draw outside those limits.

The following portion of Aurora Imaging Library code shows the creation of the device context for the overlay buffer, the inquiring of the device context, and the drawing and writing in the overlay buffer (see also, [MDispOverlay.cpp](../../Examples/MDispOverlay.cpp.md)).

> **Code example:** [userguide.displaying_an_image.annotating_the_displayed_image_non_destructively05](userguide.displaying_an_image.annotating_the_displayed_image_non_destructively05)

## Using regions of interest

Regions of interest (ROIs) can be used to restrict processing/analysis to a subset of data in an image buffer; see [](../C02_Building_an_application/S10_Child_buffers_regions_of_interest_and_fixturing.md) for details. You can set how to display the pixels that are in and not in the ROI using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_REGION_INSIDE_SHOW`](../../Reference/disp/MdispControl.md) and [`M_REGION_OUTSIDE_SHOW`](../../Reference/disp/MdispControl.md) respectively; you can display the pixels with or without an overlay.

### Opacity

When applying an overlay to the pixels in or not in the ROI, you can adjust the opacity. To set the overlay opacity for the pixels in the ROI, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_REGION_INSIDE_SHOW`](../../Reference/disp/MdispControl.md) set to [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispControl.md). To set the overlay opacity for the pixels not in the ROI, use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_REGION_OUTSIDE_SHOW`](../../Reference/disp/MdispControl.md) set to [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispControl.md).

### Examples

Below are two examples showing an overlay applied to the pixels not in the ROI of an image with an overlay opacity of 100% (left) and 50% (right).

*[Image: ROI_Overlay_Opacity.png]*

The image on the left is created using this code snippet.

> **Code example:** [userguide.displaying_an_image.roi_opacity_overlay01](userguide.displaying_an_image.roi_opacity_overlay01)

The image on the right is created using this code snippet.

> **Code example:** [userguide.displaying_an_image.roi_opacity_overlay02](userguide.displaying_an_image.roi_opacity_overlay02)
