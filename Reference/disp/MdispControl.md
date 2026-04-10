---
doctype: Reference
module: disp
function: MdispControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / disp / MdispControl"
---

# MdispControl

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

> Control a display setting.

## Syntax

```c
void MdispControl(
    AIL_ID     DisplayId,    //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control the specified Aurora Imaging Library display setting.

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the target display.

### `ControlType` *(in, AIL_INT64)*

Specifies the type of display setting to control. The control types for windowed displays can control the default Aurora Imaging Library or user-specified window of the display ([`MdispSelect`](../../Reference/disp/MdispSelect.md) or [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md)).

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the new value to assign to the display setting specified by the [`ControlType`](../../Reference/disp/MdispControl.md) parameter.

## Parameter Associations

### For all types of displays

Unless otherwise specified, the following [`ControlType`](../../Reference/disp/MdispControl.md) and corresponding [`ControlValue`](../../Reference/disp/MdispControl.md) parameter settings are available for all types of displays.

---

### `M_ASSOCIATED_GRAPHIC_LIST_ID`

Sets the 2D graphics list to associate with the display. Only one 2D graphics list can be associated with the display at a time. If you associate a 2D graphics list with a display that already has been associated with one, the previous 2D graphics list is disassociated from the display and the new one is associated.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no 2D graphics list is associated with the display. |
| `2D graphics list identifier` | Specifies the identifier of the 2D graphics list to associate with the display. The 2D graphics list must have been previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). In this case, the vector-based graphics, contained within the 2D graphics list, will be used to annotate the display non-destructively.  By default, modifications to the graphics within the list (added, deleted, or altered graphics) will be immediately reflected in the non-destructive annotation of the display. To change this behavior, use [`M_UPDATE_GRAPHIC_LIST`](../../Reference/disp/MdispControl.md).  To release the 2D graphics list from memory, use [`MgraFree`](../../Reference/gra/MgraFree.md). If the associated 2D graphics list is freed, it is automatically disassociated from the display. |

---

### `M_BACKGROUND_COLOR`

Sets the background color for the display. If the selected image buffer is smaller than the display or does not fill the display completely, the surrounding area will be displayed with this color.

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

---

### `M_CENTER_DISPLAY`

Sets whether a selected image buffer will be centered in the display, both in X and Y.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is [`M_ENABLE`](../../Reference/disp/MdispControl.md) for an Aurora Imaging Library default window and an exclusive display. The default value is [`M_DISABLE`](../../Reference/disp/MdispControl.md) for a user-defined window display. |
| `M_DISABLE` | Specifies that the image buffer will not be centered in the display. This will overwrite the current panning offsets ([`MdispPan`](../../Reference/disp/MdispPan.md)) to (0,0). |
| `M_ENABLE` | Specifies that the image buffer will always be centered in the display, both in X and Y. This will offset the image buffer by ( (WindowSizeX - BufferSizeX)/2, (WindowSizeY - BufferSizeY)/2) in the window, and current panning offsets ([`MdispPan`](../../Reference/disp/MdispPan.md)) are overwritten with these values. |

---

### `M_GRAPHIC_LIST_INTERACTIVE`

Sets whether a user can interactively modify the graphics in the 2D graphics list associated with the display. Modifications to the graphics in the list include moving, resizing, and rotating.  The user interacts with the graphics using the mouse. Clicking on a graphic selects it, which causes the graphic to be surrounded with a selection box. The selection box has handles around it that can be dragged to resize, rotate, or move the graphic. The user can select multiple graphics by holding down the Ctrl key and clicking on each graphic. New graphics can be added to the 2D graphics list interactively using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md).  You can restrict the type of interactions the user can have using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the 2D graphics list associated with the display cannot be affected by user interaction. |
| `M_ENABLE` | Specifies that the 2D graphics list associated with the display can be affected by user interaction. |

---

### `M_GRAPHIC_LIST_OPACITY`

Sets the opacity level of annotations generated from the 2D graphics list associated with the display.  Note that this setting applies to the display, not to the associated 2D graphics list. If you associate another 2D graphics list with the display (using this function with[`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)), the opacity that you set is also used with the new 2D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_OVERLAY_OPACITY`](Reference/disp/MdispControl.md))* |  |

---

### `M_INTERPOLATION_MODE`

Sets the type of interpolation used to display a zoomed image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies averaging interpolation. |
| `M_INTERPOLATE` | Specifies interpolated resizing. This gives the best speed/result compromise for interpolated resizing. |
| `M_MAX` | Specifies an interpolation based on the maximum pixel value in the specified source image. |
| `M_MIN` | Specifies an interpolation based on the minimum pixel value in the specified source image. |
| `M_NEAREST_NEIGHBOR` *(default)* | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

---

### `M_KEYBOARD_USE`

Sets whether the user can interactively pan and zoom the display using the keyboard.  The default key usage is:  |   |   | | --- | --- | | + | Increases the X- and Y-zoom factors. | | - | Decreases the X- and Y-zoom factors. | | Page-up | Scrolls the image buffer up to the previous display section. | | Page-down | Scrolls the image buffer down to the next display section. | | Up arrow | Scrolls the image buffer up to the previous line. | | Down arrow | Scrolls the image buffer down to the next line. | | Left arrow | Scrolls the image buffer left by one pixel. | | Right arrow | Scrolls the image buffer right by one pixel. | | Ctrl-Up arrow | Scrolls the image buffer up to the previous display section. | | Ctrl-Down arrow | Scrolls the image buffer down to the next display section. | | Ctrl-Left arrow | Scrolls the image buffer left to the previous display section. | | Ctrl-Right arrow | Scrolls the image buffer right to the next display section. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | For user-defined windowed displays, this value is the same as [`M_DISABLE`](../../Reference/disp/MdispControl.md); for Aurora Imaging Library windowed and exclusive displays, this value is the same as [`M_ENABLE`](../../Reference/disp/MdispControl.md). |
| `M_DISABLE` | Specifies that using the keyboard to perform panning and zooming is disabled.  This is the default value for user-defined windowed displays. |
| `M_ENABLE` | Specifies that using the keyboard to perform panning and zooming is enabled.  This is the default value for Aurora Imaging Library windowed and exclusive displays. |

---

### `M_MOUSE_CURSOR_CHANGE`

Sets whether the mouse cursor changes shape in a display with mouse-interaction enabled ([`M_MOUSE_USE`](../../Reference/disp/MdispControl.md)). For example, if interactive panning is possible in the display, the cursor will change to a hand.  If you have enabled [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md), it is not necessary to enable this setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | For user-defined windowed displays, this value is the same as [`M_DISABLE`](../../Reference/disp/MdispControl.md); for Aurora Imaging Library windowed and exclusive displays, this value is the same as [`M_ENABLE`](../../Reference/disp/MdispControl.md). |
| `M_DISABLE` | Specifies that mouse cursor change is disabled.  This is the default value for user-defined windowed displays. |
| `M_ENABLE` | Specifies that mouse cursor change is enabled.  This is the default value for Aurora Imaging Library windowed and exclusive displays. |

---

### `M_MOUSE_USE`

Sets whether the user can interactively pan and zoom the display using the mouse.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | For user-defined windowed displays, this value is the same as [`M_DISABLE`](../../Reference/disp/MdispControl.md); for Aurora Imaging Library windowed and exclusive displays, this value is the same as [`M_ENABLE`](../../Reference/disp/MdispControl.md). |
| `M_DISABLE` | Specifies that mouse-interaction will not work inside a display.  This is the default value for user-defined windowed displays. |
| `M_ENABLE` | Specifies that mouse-interaction will work inside a display.  This is the default value for Aurora Imaging Library windowed and exclusive displays. |

---

### `M_OVERLAY`

Sets whether to enable the overlay mechanism. This mechanism allows you to annotate displayed image buffers non-destructively. For more information on overlay buffers, see [Annotating the displayed image non-destructively](../../UserGuide/C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md).  Once enabled, use [`MdispInquire`](../../Reference/disp/MdispInquire.md) with the [`M_OVERLAY_ID`](../../Reference/disp/MdispInquire.md) inquire type to determine the identifier of the overlay buffer. Note that once created, the overlay buffer is like any other image buffer. That is, you can perform operations on it as you would on a normal buffer (except for grabbing).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies to disable the overlay mechanism. |
| `M_ENABLE` | Specifies to enable the overlay mechanism. |

---

### `M_OVERLAY_CLEAR`

Sets the value to which the overlay buffer associated with the display should be cleared. The buffer is cleared to the specified value immediately after this control type setting is changed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the display's overlay buffer will be cleared for a non 8-bit display mode.  Specifies the RGB value. The buffer must be a non 8-bit 3-band buffer. |
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
| `M_TRANSPARENT_COLOR` *(default)* | Specifies that the display's overlay buffer will be cleared to the transparency color. Set the transparency color with the [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispControl.md) control type. |

---

### `M_OVERLAY_OPACITY`

Sets the opacity of the display's overlay buffer.  > **Note:** Any pixels in the overlay image which have the keying color ([`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispControl.md)) are always transparent, regardless of this setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use partial opacity. This is equivalent to setting an opacity of 100. |
| `0 <= Value <= 100` | Specifies the opacity (as a percentage), where 0 is completely transparent and 100 is completely opaque. |

---

### `M_OVERLAY_SHOW`

Sets whether the display's overlay buffer is visible.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the overlay buffer is not visible. Display the image buffer selected on the display only. |
| `M_ENABLE` *(default)* | Specifies that the overlay buffer is visible. Annotations or other changes made to the overlay buffer in a color other than the transparency color will annotate the image selected to the display. Set the transparency color with the [`M_TRANSPARENT_COLOR`](../../Reference/disp/MdispControl.md) control type. |
| `M_OPAQUE` | Specifies that only the overlay buffer is visible. The image buffer selected will be hidden. |

---

### `M_REGION_INSIDE_COLOR`

Sets the color that will be used to clear the pixels that are part of a region of interest associated with the selected buffer. The pixels are cleared to the specified color immediately after this control type setting is changed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the pixels that are part of a region will be cleared for a non 8-bit display mode.  Specifies the RGB value. The buffer must be a non 8-bit 3-band buffer. |
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

---

### `M_REGION_INSIDE_SHOW`

Sets how to display the pixels that are part of a region of interest associated with the selected buffer, if such a region exist.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_OPACITY` | Specifies that pixels in the region are displayed with a color overlay using the opacity specified with [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispControl.md). The color is specified with [`M_REGION_INSIDE_COLOR`](../../Reference/disp/MdispControl.md). |
| `M_OPAQUE` | Specifies that pixels in the region are cleared to the color specified using [`M_REGION_INSIDE_COLOR`](../../Reference/disp/MdispControl.md). |
| `M_TRANSPARENT` *(default)* | Specifies that pixels in the region are displayed as is. |

---

### `M_REGION_OUTSIDE_COLOR`

Sets the color that will be used to clear the pixels that are not part of a region of interest associated with the selected buffer. The pixels are cleared to the specified color immediately after this control type setting is changed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RGB888` | Specifies the RGB value to which the pixels that are part of a region will be cleared for a non 8-bit display mode.  Specifies the RGB value. The buffer must be a non 8-bit 3-band buffer. |
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

---

### `M_REGION_OUTSIDE_SHOW`

Sets how to display the pixels that are not part of a region of interest associated with the selected buffer, if such a region exist.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GRAPHIC_LIST_OPACITY` | Specifies that pixels outside of the region are displayed with a color overlay using the opacity specified with [`M_GRAPHIC_LIST_OPACITY`](../../Reference/disp/MdispControl.md). The color is specified with [`M_REGION_OUTSIDE_COLOR`](../../Reference/disp/MdispControl.md). |
| `M_OPAQUE` | Specifies that pixels outside of the region are cleared to the color specified using [`M_REGION_OUTSIDE_COLOR`](../../Reference/disp/MdispControl.md). |
| `M_TRANSPARENT` *(default)* | Specifies that pixels outside of the region are displayed as is. |

---

### `M_RESTRICT_CURSOR`

Sets whether to restrict the mouse cursor so that it cannot move over the exclusive display. This control type is only applicable to exclusive displays.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies to permit the mouse cursor to move over the display. |
| `M_ENABLE` *(default)* | Specifies to restrict the mouse cursor so that it cannot move over the display. |

---

### `M_SCALE_DISPLAY`

Sets whether to fill the display with the selected image buffer, while keeping the same aspect ratio. You can inquire the automatically calculated zoom factor used to scale the image, using [`MdispInquire`](../../Reference/disp/MdispInquire.md) with [`M_ZOOM_FACTOR_X`](../../Reference/disp/MdispInquire.md) and [`M_ZOOM_FACTOR_Y`](../../Reference/disp/MdispInquire.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that the image will not be scaled to fit the display. Unless a zoom factor has been applied, using [`MdispZoom`](../../Reference/disp/MdispZoom.md), the image will retain its original size and aspect ratio, leaving unused portions of the display to the default background color. |
| `M_ENABLE` | Specifies to always scale the selected image buffer to fit the display, while keeping the same aspect ratio. In this case, the image is not distorted.  This setting overrides current pannning offsets ([`MdispPan`](../../Reference/disp/MdispPan.md)), as well as the previous zoom factors ([`MdispZoom`](../../Reference/disp/MdispZoom.md)). |
| `M_ONCE` | Specifies to compute the scale factors corresponding to [`M_ENABLE`](../../Reference/disp/MdispControl.md) and call [`MdispZoom`](../../Reference/disp/MdispZoom.md) with these factors.  Unlike [`M_ENABLE`](../../Reference/disp/MdispControl.md), this setting changes the scale factors only once and these scale factors will not change if an image buffer with a different size is selected to the display. |

---

### `M_TITLE`

Sets the display's title to the specified string. For windowed displays, set whether the window's title bar is visible using the [`M_WINDOW_TITLE_BAR`](../../Reference/disp/MdispControl.md) control type. If _M_NULL_ is specified, the window is given a default title ("Aurora Imaging Library Display #x").

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies to use the default title. |
| `"WindowTitle"` | Specifies the name of the window title (or display's title). |

---

### `M_TRANSPARENT_COLOR`

Sets the transparency (keying) color. Pixels of the image buffer selected to the display are displayed only where the corresponding pixels of the overlay buffer are set to the transparency color.

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

---

### `M_UPDATE`

Sets whether Aurora Imaging Library should update the display. This control type can be used to temporarily disable display updates when the image buffer is being modified by more than one operation; the display can then be updated at the end of all operations.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to update the display. |
| `M_ENABLE` *(default)* | Specifies to update the display. Also, the display is forced to update immediately. |
| `M_NOW` | Specifies to force an immediate update of the display. In this case, the display is updated even if [`M_UPDATE`](../../Reference/disp/MdispControl.md) is set to [`M_DISABLE`](../../Reference/disp/MdispControl.md). |

---

### `M_UPDATE_GRAPHIC_LIST`

Sets whether Aurora Imaging Library should update the display when modifications are made to the display's associated 2D graphics list. To associate a 2D graphics list to the display, use [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md).  With [`M_UPDATE_GRAPHIC_LIST`](../../Reference/disp/MdispControl.md), you can temporarily disable display updates ([`M_DISABLE`](../../Reference/disp/MdispControl.md)) when the 2D graphics list is being modified by multiple operations; you can then enable display updates ([`M_ENABLE`](../../Reference/disp/MdispControl.md)) at the end of all operations.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the display is not updated when the associated 2D graphics list is modified. |
| `M_ENABLE` *(default)* | Specifies that the display is immediately updated when the associated 2D graphics list is modified (refresh not required). |

---

### `M_UPDATE_RATE_DIVIDER`

Sets after how many buffer modifications to update the display, regardless of the time lapsed. For example, if you set [`M_UPDATE_RATE_DIVIDER`](../../Reference/disp/MdispControl.md) to 10, one out of every 10 buffer modifications will cause a display update. If fewer than the specified number of buffer modifications occur, the display is not updated. The display is only updated with the last buffer modification. This control type is useful when performing a continuous grab.  If the display is allocated on a Distributed Aurora Imaging Library remote system but displayed on the local computer, another mechanism is also available to limit the number of display updates. You can set an upper limit to the number of display updates to perform per second, using [`M_UPDATE_RATE_MAX`](../../Reference/disp/MdispControl.md). In this case, updates of modified areas of the buffer are accumulated into one update between transmissions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the frequency of the update. |

---

### `M_UPDATE_RATE_MAX`

Sets the maximum rate at which to update the display.  To update the display only after a specific number of buffer modifications, regardless of the time lapsed, use [`M_UPDATE_RATE_DIVIDER`](../../Reference/disp/MdispControl.md) instead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies to perform updates as fast as possible (limited by the available bandwidth and transmission delay). |
| `Value > 0` | Specifies the maximum number of updates per second. Only integer values are accepted. A minimum delay of 1/_Value_ seconds will be respected between two consecutive updates. |

---

### `M_UPDATE_SYNCHRONIZATION`

Sets the update mode currently used to update the display.  This control type allows you to serialize paint messages sent to update the display. When the display uses a user-defined window, one paint message can interrupt another. This can cause deadlock if, for example, you set up an event handler for the paint message which tries to update the image buffer selected on the display. This will cause another paint message to be called to update the display. This results in the paint message recursively calling itself, causing a deadlock. To solve this problem [`M_UPDATE_SYNCHRONIZATION`](../../Reference/disp/MdispControl.md) can be set to enforce the serialization of paint messages.  > **Note:** This control type allows you to serialize the published image data stream with your remote display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default update mode.  The default mode corresponds to [`M_ASYNCHRONOUS`](../../Reference/disp/MdispControl.md), if the user-defined window thread is the same as the current one. Otherwise, it is [`M_SYNCHRONOUS`](../../Reference/disp/MdispControl.md).  Same as [`M_ASYNCHRONOUS`](../../Reference/disp/MdispControl.md). |
| `M_ASYNCHRONOUS` | Specifies that the display is updated when possible.  The paint messages are queued and that the application's window procedure (WndProc) will update the display whenever possible. Basically, when a paint message is issued, Aurora Imaging Library calls the Windows `InvalidateRect()` function. For more information on `InvalidateRect()`, refer to the MSDN library.  The published image data stream to be asynchronous. The function will return immediately while internal threads are sending data to all clients that are listening for the data stream. [`MdispControl`](../../Reference/disp/MdispControl.md) using [`M_UPDATE_SYNCHRONIZATION`](../../Reference/disp/MdispControl.md) with a valid display identifier will result in a call to the Remote module's `RemoteSetAttribute` function, with the proper parameters set. |
| `M_SYNCHRONOUS` | Specifies that the display is updated immediately.  The display is updated immediately upon reception of a paint message. Basically, when a paint message is issued, Aurora Imaging Library calls the Windows `InvalidateRect()` function followed by the Windows `UpdateWindow()` function. For more information on `InvalidateRect()` and `UpdateWindow()`, refer to the MSDN library.  The published image data stream will be synchronous. The function will return only when the information is sent to all clients that are listening for the data stream. The call to [`MdispControl`](../../Reference/disp/MdispControl.md) using [`M_UPDATE_SYNCHRONIZATION`](../../Reference/disp/MdispControl.md) with a valid display ID will result in a call to the Remote module's `RemoteSetAttribute` function, with the proper parameters set. |

---

### `M_VIEW_BIT_SHIFT`

Sets the number of bits by which to bit-shift the pixel values of the image buffer selected to the display, when the [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) control type is set to [`M_BIT_SHIFT`](../../Reference/disp/MdispControl.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the number of bits. This value should be set to the number of significant bits in the image buffer minus 8. For example, if a 16-bit image buffer contains data grabbed from a 10-bit digitizer, a shift of 2 should be used. |

---

### `M_VIEW_MODE`

Sets the view mode of the display. The view mode establishes how an image buffer gets remapped to the display; this is especially useful when displaying a non 8-bit image buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is [`M_TRANSPARENT`](../../Reference/disp/MdispControl.md) unless the buffer selected on the display is a 1-bit buffer; when a 1-bit buffer is used, the default value is [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md). |
| `M_AUTO_SCALE` | Specifies to remap the pixel values to the display so that the minimum and maximum values in the image (not the full range of the image buffer) are set to 0 and 255, respectively. If the image buffer contains a single value, the displayed value is determined by linearly remapping the full range of the image buffer to that of the display (for example, (0 to 64K) to (0 to 255)). |
| `M_BIT_SHIFT` | Specifies to bit-shift the pixel values of the image buffer by the specified number of bits upon updating the display. Specify the number of bits with the [`M_VIEW_BIT_SHIFT`](../../Reference/disp/MdispControl.md) control type. |
| `M_MULTI_BYTES` | Specifies to display each byte of the image buffer in separate display pixels. In other words, each pixel of a 16-bit image buffer will occupy two consecutive display pixels. Each pixel of a 32-bit image buffer will occupy four consecutive display pixels. This mode is only supported for 16-bit and 32-bit 1-band buffer. This mode is primarily useful when grabbing from a multi-tap camera. |
| `M_TRANSPARENT` | Specifies that no pixel remapping will be performed. Note that only the 8 least-significant bits of the image buffer will be displayed. |

### For windowed displays

The following [`ControlType`](../../Reference/disp/MdispControl.md) and corresponding [`ControlValue`](../../Reference/disp/MdispControl.md) parameter settings are available for all windowed displays.

---

### `M_WINDOW_ANNOTATIONS`

Advises Aurora Imaging Library whether annotations will be drawn directly to the window's device context (DC) using GDI functions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies to advise Aurora Imaging Library that the X display connection has not been set. |
| `M_DISABLE` | Specifies not to draw directly to the window's device context using GDI functions. |
| `M_ENABLE` *(default)* | Specifies to draw directly to the window's device context using GDI functions. |
| `Value` | Specifies the pointer to the X display connection. |

---

### `M_WINDOW_UPDATE_ON_PAINT`

Sets when the display will be updated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Allows Aurora Imaging Library to decide which message to receive before updating the display. |
| `M_DISABLE` | Updates the display on reception of a `WM_ERASEBKGND` message in Windows. |
| `M_ENABLE` | Updates the display on reception of a `WM_PAINT` message in Windows. |

### For Aurora Imaging Library windowed displays

The following [`ControlType`](../../Reference/disp/MdispControl.md) and corresponding [`ControlValue`](../../Reference/disp/MdispControl.md) parameter settings are only available for Aurora Imaging Library windowed displays (selected using [`MdispSelect`](../../Reference/disp/MdispSelect.md)), and are not available for user windowed displays (selected using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md)):

---

### `M_FULL_SCREEN`

Sets whether the window should be maximized and shown without the title bar or menu (full screen mode).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the window will not be maximized and shown with the title bar and menu. |
| `M_ENABLE` | Specifies that the window will be maximized and shown without the title bar or menu. |

---

### `M_WINDOW_INITIAL_POSITION_X`

Sets the initial, left-most, X-coordinate of the window.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate, in pixels. |

---

### `M_WINDOW_INITIAL_POSITION_Y`

Sets the initial, top-most, Y-coordinate of the window.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate, in pixels. |

---

### `M_WINDOW_INITIAL_SIZE_X`

Sets the initial width of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will calculate the optimal width. |
| `Value > 0` | Specifies the width, in pixels. |

---

### `M_WINDOW_INITIAL_SIZE_Y`

Sets the initial height of the window.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will calculate the optimal height. |
| `Value > 0` | Specifies the height, in pixels. |

---

### `M_WINDOW_MAXBUTTON`

Sets whether the window's maximize button is visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MINBUTTON`

Sets whether the window's minimize button is visible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_MOVE`

Sets whether window movement is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window movement. |
| `M_ENABLE` *(default)* | Specifies to allow window movement. |

---

### `M_WINDOW_OVERLAP`

Sets whether the window can be overlapped by another.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow the window to be overlapped by another (keep window on top). |
| `M_ENABLE` *(default)* | Specifies to allow the window to be overlapped by another. |

---

### `M_WINDOW_RESIZE`

Sets whether window resizing is allowed.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow window resizing. |
| `M_ENABLE` *(default)* | Specifies to allow window resizing. |

---

### `M_WINDOW_SHOW`

Sets whether the window should be shown or hidden.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the window should be hidden. |
| `M_ENABLE` *(default)* | Specifies that the window should be shown. |

---

### `M_WINDOW_SIZE_AUTO_RESET`

Sets whether the window should be resized when using [`MdispSelect`](../../Reference/disp/MdispSelect.md).  By default, [`MdispSelect`](../../Reference/disp/MdispSelect.md) automatically resizes the Aurora Imaging Library display window to be the same size of the selected buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not resize the window. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library will resize the window to be the same size of the selected buffer. |

---

### `M_WINDOW_SYSBUTTON`

Sets whether the window's system buttons are visible. The system buttons refer to the collection of the window's minimize, maximize, and close buttons.  To individually set whether the minimize and maximize buttons are visible, change the [`M_WINDOW_MAXBUTTON`](../../Reference/disp/MdispControl.md) or [`M_WINDOW_MINBUTTON`](../../Reference/disp/MdispControl.md) control type settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the button is not visible. |
| `M_ENABLE` *(default)* | Specifies that the button is visible. |

---

### `M_WINDOW_TITLE_BAR`

Sets whether the window's title bar is visible.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the title bar is not visible. |
| `M_ENABLE` *(default)* | Specifies that the title bar is visible. |

---

### `M_WINDOW_TITLE_BAR_CHANGE`

Sets whether the presence of the window's title bar can be toggled with the [`M_WINDOW_TITLE_BAR`](../../Reference/disp/MdispControl.md) control type.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to allow toggling. |
| `M_ENABLE` *(default)* | Specifies to allow toggling. |

### For user windowed displays

The following [`ControlType`](../../Reference/disp/MdispControl.md) and corresponding [`ControlValue`](../../Reference/disp/MdispControl.md) parameter settings are only available for user windowed displays (selected using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md)), and are not available for Aurora Imaging Library windowed displays (selected using [`MdispSelect`](../../Reference/disp/MdispSelect.md)):

---

### `M_GTK_MODE`

Sets whether to configure the display for use in a window created using the third-party GTK framework. Present the display in this window using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md).  If this setting is disabled and the display is selected to a window created using GTK, the display might not appear correctly.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to configure the display for use in a window created using the GTK framework. |
| `M_ENABLE` | Specifies not to configure the display for use in a window created using the GTK framework. |

---

### `M_QT_MODE`

Sets whether to configure the display for use in a window created using the third-party Qt framework. Present the display in this window using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md).  If this setting is disabled and the display is selected to a window created using Qt, the display might not appear correctly.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to configure the display for use in a window created using the Qt framework. |
| `M_ENABLE` | Specifies to configure the display for use in a window created using the Qt framework. |

---

### `M_TK_MODE`

Sets whether to configure the display for use in a window created using the third-party Tk framework. Present the display in this window using [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md).  If this setting is disabled and the display is selected to a window created using Tk, the display might not appear correctly.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to configure the display for use in a window created using the Tk framework. |
| `M_ENABLE` | Specifies to configure the display for use in a window created using the Tk framework. |

### For displays allocated on a Distributed Aurora Imaging Library remote system but displayed on the local computer

The following [`ControlType`](../../Reference/disp/MdispControl.md) and corresponding [`ControlValue`](../../Reference/disp/MdispControl.md) parameter settings are available for displays allocated on a Distributed Aurora Imaging Library remote system but displayed on the local computer.

---

### `M_ASYNC_UPDATE`

Sets whether to send display updates from the remote computer to the local computer in asynchronous or synchronous mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default mode, which can be selected using the Aurora Imaging Configurator utility. In the Aurora Imaging Configurator utility, if you select the **High quality** display option, the default mode is [`M_DISABLE`](../../Reference/disp/MdispControl.md); if you select the **Optimized for bandwidth usage** display option, the default mode is [`M_ENABLE`](../../Reference/disp/MdispControl.md). |
| `M_DISABLE` | Specifies to send display updates in synchronous mode. In synchronous mode, the display is updated after each modification to the displayed image buffer (or the overlay buffer), and the application waits for the data to be transferred to the display before continuing. When an image, located on a remote computer, is displayed on the local computer, the update time can significantly slow your application. |
| `M_ENABLE` | Specifies that data will be sent asynchronously. In asynchronous mode, the required display updates are queued using multiple internal image bufferss and the application continues once the update has been queued. This mode maximizes your bandwidth usage while minimizing processing delays. There will be a delay of 1/[`M_UPDATE_RATE_MAX`](../../Reference/disp/MdispControl.md) between two consecutive updates. |

---

### `M_COMPRESSION_TYPE`

Sets how to compress data being transmitted from the remote computer to the local computer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default compression type, which can be selected using the Aurora Imaging Configurator utility. In the Aurora Imaging Configurator utility, if you select the **High quality** display option, compression is disabled ([`M_NULL`](../../Reference/disp/MdispControl.md)); if you select the **Optimized for bandwidth usage** display option, compression is enabled and the compression type is set to [`M_JPEG_LOSSY`](../../Reference/disp/MdispControl.md). |
| `M_NULL` | Specifies that compression is disabled. |
| `M_JPEG2000_LOSSLESS` | Specifies that JPEG2000 lossless compression will be used. The supported data formats are: 1-band, 8- or 16-bit data. JPEG2000 compression supports much higher ratios of compression. |
| `M_JPEG2000_LOSSY` | Specifies JPEG2000 lossy compression will be used. JPEG2000 compression supports much higher ratios of compression without compromising image quality. |
| `M_JPEG_LOSSLESS` | Specifies JPEG lossless compression will be used. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that JPEG lossless compression will be used in separate fields. The supported data formats are 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` | Specifies that JPEG lossy compression will be used. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that JPEG lossy compression will be used in separate fields. |

---

### `M_Q_FACTOR`

Sets the quantization factor. This control type is only applied when lossy compression is used.  The Q factor is applied to all bands.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1 <= Value <= 99` *(default)* | Specifies the quantization factor. Only integer values are accepted. The higher the factor, the more the compression, but the lower the image quality. |

## Remarks

> Aurora Imaging Library Lite does not support displaying a 32-bit image buffer in a display that has its [`M_VIEW_MODE`](../../Reference/disp/MdispControl.md) control type set to [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md), unless you have purchased the Aurora Imaging Library Image Analysis license.

> Note that during development and at runtime, compression support requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

You might need to use this compression type, for example, when there are fine details in your image or overlay buffer. Note, however, you should avoid this compression type unless absolutely necessary since compression/decompression time is high; you should only use it when the benefits of compression outweigh the overhead associated with the compression/decompression.

You can manually set the compression factor using [`M_Q_FACTOR`](../../Reference/disp/MdispControl.md).
