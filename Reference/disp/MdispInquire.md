---
doctype: Reference
module: disp
function: MdispInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / disp / MdispInquire"
---

# MdispInquire

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

> Inquire about a display setting.

## Syntax

```c
AIL_INT MdispInquire(
    AIL_ID    DisplayId,    //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires about a specified display setting.

## Parameters

### `DisplayId` *(in, AIL_ID)*

Specifies the identifier of the display.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of display setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdispInquire`](../../Reference/disp/MdispInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For all types of displays

Unless otherwise specified, the following [`InquireType`](../../Reference/disp/MdispInquire.md) parameter settings are available for all types of displays.

---

### `M_ASSOCIATED_GRAPHIC_LIST_ID`

Inquires the identifier of the 2D graphics list.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no 2D graphics list is associated with the display. |
| `2D graphics list identifier` | Specifies the identifier of the 2D graphics list that is associated with the display. |

---

### `M_BACKGROUND_COLOR`

Inquires the background color of the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the background color will be set. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` *(default)* | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_CENTER_DISPLAY`

Inquires whether a selected image buffer should be centered in the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DISABLE` | Specifies that the image buffer will not be centered in the display. |
| `M_ENABLE` | Specifies that the image buffer will always be centered in the display, both in X and Y. |

---

### `M_DISPLAY_TYPE`

Inquires the display type.

| Value | Description |
| --- | --- |
| `M_EXCLUSIVE` | Specifies that the display is an exclusive display.  > **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |
| `M_WEB` | Specifies that the display is an Aurora Imaging Web Library display.  > **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |
| `M_WINDOWED` | Specifies that the display is a windowed display.  > **Note:** This display type can be used with [`MdispSelect`](../../Reference/disp/MdispSelect.md) and [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md). A remote display requires[`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. To inquire if a display is remote, use [`M_EXTENDED_INIT_FLAG`](../../Reference/disp/MdispInquire.md). |
| `M_WPF` | Specifies that the display is a WPF display.  > **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |

---

### `M_EXTENDED_INIT_FLAG`

Inquires the initialization flag specified upon allocating the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default display type, which can be set using the Aurora Imaging Configurator utility. |
| `M_EXCLUSIVE` | Specifies to present the image buffer selected for display full-screen, without a windowed border, in one of Windows desktop screens. |
| `M_WEB` | Specifies to present the image buffer selected for display in one or more instances of an Aurora Imaging Web Library client application (for example, running in a compatible web browser). |
| `M_WINDOWED` | Specifies to present the image buffer selected for display in its own window on the Windows desktop screen(s). |
| `M_WPF` | Specifies to present the image buffer selected for display using an AILWPFDisplay control. |
| `M_REMOTE_DISPLAY` | Specifies that the display is displayed on the remote computer in a Distributed Aurora Imaging Library application. |

---

### `M_FORMAT`

Inquires the display's data format.

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies the default display format. |
| `M_CURRENT_RESOLUTION` | Specifies to use the current resolution of the screen. |
| `"vcf-file-name.vcf"` | Specifies the name of the video configuration format (VCF) that defines the resolution and refresh rate to use. |

---

### `M_GRAPHIC_LIST_INTERACTIVE`

Inquires whether a user can interactively modify the graphics in the 2D graphics list associated with the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the 2D graphics list associated with the display cannot be affected by user interaction. |
| `M_ENABLE` | Specifies that the 2D graphics list associated with the display can be affected by user interaction. |

---

### `M_GRAPHIC_LIST_OPACITY`

Inquires the opacity of annotations generated from the 2D graphics list associated with the display.

| Value | Description |
| --- | --- |
| *(see [`M_GRAPHIC_LIST_OPACITY`](Reference/disp/MdispControl.md))* |  |

---

### `M_INTERPOLATION_MODE`

Inquires the type of interpolation used to display a zoomed image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies averaging interpolation. |
| `M_INTERPOLATE` | Specifies interpolated resizing. |
| `M_MAX` | Specifies an interpolation based on the maximum pixel value in the specified source image. |
| `M_MIN` | Specifies an interpolation based on the minimum pixel value in the specified source image. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. |

---

### `M_KEYBOARD_USE`

Inquires whether the user can interactively pan and zoom the display using the keyboard.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | For user-defined windowed displays, this value is the same as [`M_DISABLE`](../../Reference/disp/MdispInquire.md); for Aurora Imaging Library windowed and exclusive displays, this value is the same as [`M_ENABLE`](../../Reference/disp/MdispInquire.md). |
| `M_DISABLE` | Specifies that using the keyboard to perform panning and zooming is disabled. |
| `M_ENABLE` | Specifies that using the keyboard to perform panning and zooming is enabled. |

---

### `M_LUT_ID`

Inquires the identifier of the LUT buffer associated with the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that any association to a LUT buffer is removed from the specified display. |
| `M_COLORMAP_DISTINCT_256` | Specifies the LUT buffer with a distinct colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_GRAYSCALE` | Specifies the LUT buffer with a grayscale colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_HOT` | Specifies the LUT buffer with a hot colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_HUE` | Specifies the LUT buffer with a hue colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_JET` | Specifies the LUT buffer with a jet colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_SPECTRUM` | Specifies the LUT buffer with a spectrum colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_TURBO` | Specifies the LUT buffer with a turbo colormap as the LUT buffer that is associated with the display. |
| `M_COLORMAP_WHEEL` | Specifies the LUT buffer with a colorwheel colormap as the LUT buffer that is associated with the display. |
| `M_PSEUDO` | Specifies the default pseudo-color LUT buffer as the LUT buffer that is associated with the display. |
| `LUT buffer identifier` | Specifies the identifier of the custom LUT buffer (allocated with [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) with [`M_LUT`](../../Reference/buf/MbufAllocColor.md)) that is associated with the display. |

---

### `M_LUT_SUPPORTED`

Inquires whether LUTs are supported by the display.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that LUTs are not supported. |
| `M_YES` | Specifies that LUTs are supported. |

---

### `M_MOUSE_CURSOR_CHANGE`

Inquires whether the mouse cursor changes shape in a display with mouse-interaction enabled.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | For user-defined windowed displays, this value is the same as [`M_DISABLE`](../../Reference/disp/MdispInquire.md); for Aurora Imaging Library windowed and exclusive displays, this value is the same as [`M_ENABLE`](../../Reference/disp/MdispInquire.md). |
| `M_DISABLE` | Specifies that mouse cursor change is disabled. |
| `M_ENABLE` | Specifies that mouse cursor change is enabled. |

---

### `M_MOUSE_USE`

Inquires whether the user can interactively pan and zoom the display using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | For user-defined windowed displays, this value is the same as [`M_DISABLE`](../../Reference/disp/MdispInquire.md); for Aurora Imaging Library windowed and exclusive displays, this value is the same as [`M_ENABLE`](../../Reference/disp/MdispInquire.md). |
| `M_DISABLE` | Specifies that mouse-interaction will not work inside a display. |
| `M_ENABLE` | Specifies that mouse-interaction will work inside a display. |

---

### `M_NUMBER`

Inquires the display device's rank in the system. Note that the returned value can also be a combination of [`M_TOP`](../../Reference/disp/MdispAlloc.md), [`M_BOTTOM`](../../Reference/disp/MdispAlloc.md), or [`M_CENTER`](../../Reference/disp/MdispAlloc.md)+ [`M_LEFT`](../../Reference/disp/MdispAlloc.md) or [`M_RIGHT`](../../Reference/disp/MdispAlloc.md), specifying the position of the display device in screen arrangements of up to 3x3 screens.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will use the most appropriate device for display purposes. |
| `M_BOTTOM` | Specifies to use the bottom-most device for an exclusive display. |
| `M_CENTER` | Specifies to use the center device for an exclusive display. |
| `M_LEFT` | Specifies to use the left-most device for an exclusive display. |
| `M_RIGHT` | Specifies to use the right-most device for an exclusive display. |
| `M_TOP` | Specifies to use the top-most device for an exclusive display. |
| `M_BOTTOM + M_LEFT` | Specifies to use the bottom left device for an exclusive display. |
| `M_BOTTOM + M_RIGHT` | Specifies to use the bottom right device for an exclusive display. |
| `M_CENTER + M_LEFT` | Specifies to use the center left device for an exclusive display. |
| `M_CENTER + M_RIGHT` | Specifies to use the center right device for an exclusive display. |
| `M_TOP + M_LEFT` | Specifies to use the top left device for an exclusive display. |
| `M_TOP + M_RIGHT` | Specifies to use the top right device for an exclusive display. |

---

### `M_OVERLAY`

Inquires whether or not the overlay mechanism is enabled for the display.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the overlay mechanism. |
| `M_ENABLE` | Specifies to enable the overlay mechanism. |

---

### `M_OVERLAY_ID`

Inquires the identifier of the overlay buffer associated with the display.  Note that the overlay buffer is a normal buffer and, knowing its Aurora Imaging Library identifier, you can perform operations on it as you would on a normal buffer (except for grabbing).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no overlay buffer associated with the display. |
| `Buffer identifier` | Specifies the Aurora Imaging Library identifier of the overlay buffer. |

---

### `M_OVERLAY_OPACITY`

Inquires the opacity of the display's overlay buffer.  > **Note:** Any pixels in the overlay image which have the keying color ([`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispInquire.md)) are always transparent, regardless of this setting.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to use partial opacity. |
| `0 <= Value <= 100` | Specifies the opacity (as a percentage), where 0 is completely transparent and 100 is completely opaque. |

---

### `M_OVERLAY_SHOW`

Inquires the visible state of the overlay.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the overlay buffer is not visible. |
| `M_ENABLE` *(default)* | Specifies that the overlay buffer is visible. |
| `M_OPAQUE` | Specifies that only the overlay buffer is visible. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the display has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_PAN_OFFSET_X`

Inquires the number of pixels in X by which the image buffer is panned.  Note that when you interactively pan the image with the mouse or keyboard, [`M_PAN_OFFSET_X`](../../Reference/disp/MdispInquire.md) will reflect the offset of the image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of pixels by which the image is panned horizontally. |

---

### `M_PAN_OFFSET_Y`

Inquires the number of pixels in Y by which to the image buffer is panned.  Note that when you interactively pan the image with the mouse or keyboard, [`M_PAN_OFFSET_Y`](../../Reference/disp/MdispInquire.md) will reflect the offset of the image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of pixels by which the image is panned vertically. |

---

### `M_PIXEL_FORMAT`

Inquires the pixel format of the current display resolution set by your graphics controller. Allocating a displayable image buffer with the same format will ensure maximum performance with regards to display updates.

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit BGR format. |
| `M_BGR32` | Specifies 32-bit BGR format. |
| `M_MONO8` | Specifies 8-bit monochrome format. |
| `M_RGB15` | Specifies 15-bit RGB format. |
| `M_RGB16` | Specifies 16-bit RGB format. |
| `M_YUV16_UYVY` | Specifies YUV16 format, stored in the UYVY order. |
| `M_YUV16_YUYV` | Specifies YUV16 format, stored in the YUYV order. |

---

### `M_REGION_INSIDE_COLOR`

Inquires the color that will be used to clear the pixels that are part of a region of interest associated with the selected buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the pixels that are part of a region will be cleared for a non 8-bit display mode. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` *(default)* | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_REGION_INSIDE_SHOW`

Inquires how to display the pixels that are part of a region of interest associated with the selected buffer, if such a region exist.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_OPACITY` | Specifies that pixels in the region are displayed with a color overlay using the opacity specified with [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispInquire.md). |
| `M_OPAQUE` | Specifies that pixels in the region are cleared to the color specified using [`M_REGION_INSIDE_COLOR`](../../Reference/disp/MdispInquire.md). |
| `M_TRANSPARENT` *(default)* | Specifies that pixels in the region are displayed as is. |

---

### `M_REGION_OUTSIDE_COLOR`

Inquires the color that will be used to clear the pixels that are not part of a region of interest associated with the selected buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the pixels that are part of a region will be cleared for a non 8-bit display mode. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` *(default)* | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_REGION_OUTSIDE_SHOW`

Inquires how to display the pixels that are not part of a region of interest associated with the selected buffer, if such a region exist.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_OPACITY` | Specifies that pixels outside of the region are displayed with a color overlay using the opacity specified with [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispInquire.md). |
| `M_OPAQUE` | Specifies that pixels outside of the region are cleared to the color specified using [`M_REGION_OUTSIDE_COLOR`](../../Reference/disp/MdispInquire.md). |
| `M_TRANSPARENT` *(default)* | Specifies that pixels outside of the region are displayed as is. |

---

### `M_RESTRICT_CURSOR`

Inquires whether the mouse cursor is restricted so that it cannot move over the exclusive display. This inquire type is only applicable to exclusive displays.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies to permit the mouse cursor to move over the display. |
| `M_ENABLE` *(default)* | Specifies to restrict the mouse cursor so that it cannot move over the display. |

---

### `M_SCALE_DISPLAY`

Inquires whether a selected buffer will fill the display, maintaining its aspect ratio.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that the image will not be scaled to fit the display. |
| `M_ENABLE` | Specifies to always scale the selected image buffer to fit the display, while keeping the same aspect ratio. |
| `M_ONCE` | Specifies to compute the scale factors corresponding to [`M_ENABLE`](../../Reference/disp/MdispInquire.md) and call [`MdispZoom`](../../Reference/disp/MdispZoom.md) with these factors. |

---

### `M_SELECTED`

Inquires the identifier of the image buffer currently displayed.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no buffer is currently being displayed. |
| `Buffer identifier` | Specifies the Aurora Imaging Library identifier of the image buffer selected to the display. |

---

### `M_SIZE_BAND`

Inquires the number of color bands that the display is capable of displaying.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of color bands. |

---

### `M_SIZE_BAND_LUT`

Inquires the number of color bands of the LUT (if any) associated with the display.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of color bands. |

---

### `M_SIZE_BIT`

Inquires the number of bits (depth) of the display per band.

| Value | Description |
| --- | --- |
| `Value` | Specifies the depth per band, in bits. |

---

### `M_SIZE_X`

Inquires the display's width. For windowed displays, the returned value is equal to the width of the client area of the window. For exclusive displays, the returned width is the width specified in the VCF file. For Aurora Imaging Web Library displays, the returned value is equal to the width ([`M_SIZE_X`](../../Reference/buf/MbufInquire.md)) of the image buffer currently selected to the display.

| Value | Description |
| --- | --- |
| `Value` | Specifies the display's width, in pixels. |

---

### `M_SIZE_Y`

Inquires the display's height. For windowed displays, the returned value is equal to the height of the client area of the window. For exclusive displays, the returned height is the height specified in the VCF file. For Aurora Imaging Web Library displays, the returned value is equal to the height ([`M_SIZE_Y`](../../Reference/buf/MbufInquire.md)) of the image buffer currently selected to the display.

| Value | Description |
| --- | --- |
| `Value` | Specifies the display's height, in pixels. |

---

### `M_TITLE`

Inquires the display's title.

| Value | Description |
| --- | --- |
| `"Title"` | Specifies the title of the display. |

---

### `M_TRANSPARENT_COLOR`

Inquires the transparency (keying) color.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the optimal transparency color. |
| `M_RGB888` | Specifies the RGB value to which to set the transparency color for a non 8-bit display mode. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_UPDATE`

Inquires whether Aurora Imaging Library should update the display.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to update the display. |
| `M_ENABLE` *(default)* | Specifies to update the display. |

---

### `M_UPDATE_GRAPHIC_LIST`

Inquires whether Aurora Imaging Library should update the display when modifications are made to the display's associated 2D graphics list.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the display is not updated when the associated 2D graphics list is modified. |
| `M_ENABLE` *(default)* | Specifies that the display is immediately updated when the associated 2D graphics list is modified (refresh not required). |

---

### `M_UPDATE_RATE`

Inquires the display update rate. This is the number of display updates that occurred during the last second.

| Value | Description |
| --- | --- |
| `0` | Specifies that the buffer selected to the display was not modified. |
| `Value > 0` | Specifies the display update rate. |

---

### `M_UPDATE_RATE_DIVIDER`

Inquires how often the display should be updated with buffer modifications.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the frequency of the update. |

---

### `M_UPDATE_RATE_MAX`

Inquires the maximum rate at which to perform updates.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to perform updates as fast as possible (limited by the available bandwidth and transmission delay). |
| `Value > 0` | Specifies the maximum number of updates per second. |

---

### `M_UPDATE_SYNCHRONIZATION`

Inquires the update mode currently used by the display for Windows paint messages.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default update mode. |
| `M_ASYNCHRONOUS` | Specifies that the display is updated when possible. |
| `M_SYNCHRONOUS` | Specifies that the display is updated immediately. |

---

### `M_VIEW_BIT_SHIFT`

Inquires the number of bits by which the buffer data gets shifted when [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) is set to [`M_BIT_SHIFT`](../../Reference/disp/MdispControl.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the number of bits. |

---

### `M_VIEW_MODE`

Inquires how a buffer gets remapped to the display (view mode).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_AUTO_SCALE` | Specifies to remap the pixel values to the display so that the minimum and maximum values in the image (not the full range of the image buffer) are set to 0 and 255, respectively. |
| `M_BIT_SHIFT` | Specifies to bit-shift the pixel values of the image buffer by the specified number of bits upon updating the display. |
| `M_MULTI_BYTES` | Specifies to display each byte of the image buffer in separate display pixels. |
| `M_TRANSPARENT` | Specifies that no pixel remapping will be performed. |

---

### `M_ZOOM_FACTOR_X`

Inquires the zoom factor in the X-direction.  Note that when you interactively zoom the image with the mouse or keyboard, [`M_ZOOM_FACTOR_X`](../../Reference/disp/MdispInquire.md) will reflect the zoom of the image.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the zoom factor for the X-direction of the display. |

---

### `M_ZOOM_FACTOR_Y`

Inquires the zoom factor in the Y-direction.  Note that when you interactively zoom the image with the mouse or keyboard, [`M_ZOOM_FACTOR_Y`](../../Reference/disp/MdispInquire.md) will reflect the zoom of the image.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the zoom factor for the Y-direction of the display. |

### Combination Constants — Returns whether the display was allocated on a remote computer

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the display was allocated on a remote computer.

| Value | Description |
| --- | --- |
| `M_REMOTE_DISPLAY` | Specifies that the display is displayed on the remote computer in a Distributed Aurora Imaging Library application. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

> **Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — Returns whether the display is an Aurora Imaging Library window or a user-defined window

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether the display is an Aurora Imaging Library window or a user-defined window.

| Value | Description |
| --- | --- |
| `M_AIL_WINDOW` | Specifies that the display is an Aurora Imaging Library display, selected using [`MdispSelect`](../../Reference/disp/MdispSelect.md). |
| `M_USER_WINDOW` | Specifies that the display is a user-defined display, selected using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md). |

### For all windowed displays

The following [`InquireType`](../../Reference/disp/MdispInquire.md) parameter settings are available for all windowed displays.

---

### `M_WINDOW_ANNOTATIONS`

Inquires whether annotations will be drawn directly to the window's device context (DC) using GDI functions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies to advise Aurora Imaging Library that the X display connection has not been set. |
| `M_DISABLE` | Specifies not to draw directly to the window's device context using GDI functions. |
| `M_ENABLE` *(default)* | Specifies to draw directly to the window's device context using GDI functions. |
| `Value` | Specifies the pointer to the X display connection. |

---

### `M_WINDOW_UPDATE_ON_PAINT`

Inquires when the display will be updated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Allows Aurora Imaging Library to decide which message to receive before updating the display. |
| `M_DISABLE` | Updates the display on reception of a `WM_ERASEBKGND` message in Windows. |
| `M_ENABLE` | Updates the display on reception of a `WM_PAINT` message in Windows. |

### For Aurora Imaging Library windowed displays

The following are only available for Aurora Imaging Library windowed displays (selected using [`MdispSelect`](../../Reference/disp/MdispSelect.md)) and are not available for user windowed displays (selected using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md)):

---

### `M_FULL_SCREEN`

Inquires whether the window should be maximized and shown without the title bar or menu (full screen mode).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the window will not be maximized and shown with the title bar and menu. |
| `M_ENABLE` | Specifies that the window will be maximized and shown without the title bar or menu. |

---

### `M_WINDOW_HANDLE`

Inquires the Windows handle (_HWND_) of the display's window.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Windows handle of the display's window. |

---

### `M_WINDOW_INITIAL_POSITION_X`

Inquires the initial, left-most, X-coordinate specified for the window.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in pixels. |

---

### `M_WINDOW_INITIAL_POSITION_Y`

Inquires the initial, left-most, Y-coordinate specified for the window.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in pixels. |

---

### `M_WINDOW_INITIAL_SIZE_X`

Inquires the initial width of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will calculate the optimal width. |
| `Value > 0` | Specifies the width, in pixels. |

---

### `M_WINDOW_INITIAL_SIZE_Y`

Inquires the initial height of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will calculate the optimal height. |
| `Value > 0` | Specifies the height, in pixels. |

---

### `M_WINDOW_MAXBUTTON`

Inquires whether the window's maximize button should be visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MINBUTTON`

Inquires whether the window's minimize button should be visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MOVE`

Inquires whether window movement is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window movement. |
| `M_ENABLE` *(default)* | Specifies to allow window movement. |

---

### `M_WINDOW_OVERLAP`

Inquires whether the window can be overlapped by another.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow the window to be overlapped by another (keep window on top). |
| `M_ENABLE` *(default)* | Specifies to allow the window to be overlapped by another. |

---

### `M_WINDOW_RESIZE`

Inquires whether window resizing is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window resizing. |
| `M_ENABLE` *(default)* | Specifies to allow window resizing. |

---

### `M_WINDOW_SHOW`

Inquires whether to show or hide the window.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the window should be hidden. |
| `M_ENABLE` *(default)* | Specifies that the window should be shown. |

---

### `M_WINDOW_SIZE_AUTO_RESET`

Inquires whether the window should be resized when using [`MdispSelect`](../../Reference/disp/MdispSelect.md).  By default, [`MdispSelect`](../../Reference/disp/MdispSelect.md) automatically resizes the Aurora Imaging Library display window to be the same size of the selected buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not resize the window. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library will resize the window to be the same size of the selected buffer. |

---

### `M_WINDOW_SYSBUTTON`

Inquires whether the window system buttons (minimize, maximize, and close buttons) should be visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_TITLE_BAR`

Inquires whether the title bar should be visible.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the title bar is not visible. |
| `M_ENABLE` *(default)* | Specifies that the title bar is visible. |

---

### `M_WINDOW_TITLE_BAR_CHANGE`

Inquires whether the presence of the window's title bar can be toggled with the [`M_WINDOW_TITLE_BAR`](../../Reference/disp/MdispControl.md) control type.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow toggling. |
| `M_ENABLE` *(default)* | Specifies to allow toggling. |

### For user windowed displays

The following are only available for user windowed displays (selected using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md)) and are not available for Aurora Imaging Library windowed displays (selected using [`MdispSelect`](../../Reference/disp/MdispSelect.md)):

---

### `M_GTK_MODE`

Inquires whether the display is configured for use in a window created using the third-party GTK framework.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to configure the display for use in a window created using the GTK framework. |
| `M_ENABLE` | Specifies not to configure the display for use in a window created using the GTK framework. |

---

### `M_QT_MODE`

Inquires whether the display is configured for use in a window created using the third-party Qt framework.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to configure the display for use in a window created using the Qt framework. |
| `M_ENABLE` | Specifies to configure the display for use in a window created using the Qt framework. |

---

### `M_TK_MODE`

Inquires whether the display is configured for use in a window created using the third-party Tk framework.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to configure the display for use in a window created using the Tk framework. |
| `M_ENABLE` | Specifies to configure the display for use in a window created using the Tk framework. |

### For displays allocated on a Distributed Aurora Imaging Library remote system but displayed on the local computer

The following inquire types are only available with windowed displays allocated on a remote system but displayed on the local computer.

---

### `M_ASYNC_UPDATE`

Inquires whether the data from the remote computer is transmitted to the local computer asynchronously or synchronously for display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default mode, which can be selected using the Aurora Imaging Configurator utility. |
| `M_DISABLE` | Specifies to send display updates in synchronous mode. |
| `M_ENABLE` | Specifies that data will be sent asynchronously. |

---

### `M_COMPRESSION_TYPE`

Inquires the way in which data is compressed when transmitted from the remote computer to the local computer for display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default compression type, which can be selected using the Aurora Imaging Configurator utility. |
| `M_NULL` | Specifies that compression is disabled. |
| `M_JPEG2000_LOSSLESS` | Specifies that JPEG2000 lossless compression will be used. |
| `M_JPEG2000_LOSSY` | Specifies JPEG2000 lossy compression will be used. |
| `M_JPEG_LOSSLESS` | Specifies JPEG lossless compression will be used. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that JPEG lossless compression will be used in separate fields. |
| `M_JPEG_LOSSY` | Specifies that JPEG lossy compression will be used. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that JPEG lossy compression will be used in separate fields. |

---

### `M_Q_FACTOR`

Inquires the quantization factor used when compressing data transmitted from the remote computer to the local computer using a lossy compression.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 99` *(default)* | Specifies the quantization factor. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

## Remarks

> Note that during development and at runtime, compression support requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
