---
doctype: Reference
module: gra
function: MgraInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraInquire"
---

# MgraInquire

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

> Inquire information about a 2D graphics context.

## Syntax

```c
AIL_INT MgraInquire(
    AIL_ID    ContextGraId,  //in
    AIL_INT64 InquireType,   //in
    void *    UserVarPtr     //out
)
```

## Description

This function inquires information about a specified 2D graphics context. To inquire about a 2D graphics list, use [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context about which to inquire information. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `InquireType` *(in, AIL_INT64)*

Specifies the 2D graphics context setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MgraInquire`](../../Reference/gra/MgraInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about general 2D graphics context settings

To inquire general 2D graphics context settings, the [`InquireType`](../../Reference/gra/MgraInquire.md) parameter should be set to one of the following values. Unless otherwise specified, these affect all types of graphics.

---

### `M_COLOR`

Inquires the foreground color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
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
| `Value` | Specifies a grayscale value. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_DRAW_DIRECTION`

Inquires how to draw (render) an arrow pointing in the direction with which a graphic was defined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that no direction is drawn. |
| `M_PRIMARY_DIRECTION` | Specifies to draw an arrow showing the primary direction of the graphic. |
| `M_PRIMARY_DIRECTION + M_SECONDARY_DIRECTION` | Specifies to draw an arrow showing both the primary and secondary direction of the graphic. |
| `M_SECONDARY_DIRECTION` | Specifies to draw an arrow showing the secondary direction of the graphic. |

---

### `M_DRAW_OFFSET_X`

Inquires the offset subtracted from the source X-coordinates.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate offset to subtract, in pixels. |

---

### `M_DRAW_OFFSET_Y`

Inquires the offset subtracted from the source Y-coordinates.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate offset to subtract, in pixels. |

---

### `M_DRAW_ZOOM_X`

Inquires the scale factor in the X-direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scale factor in the X-direction. |

---

### `M_DRAW_ZOOM_Y`

Inquires the scale factor in the Y-direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scale factor in the Y-direction. |

---

### `M_FIXTURE`

Inquires the camera calibration information used when rendering (drawing or displaying) a graphic defined in world units, when both source camera calibration information and destination camera calibration information are available to interpret positioning and dimensioning information.  If with respect to that of the source camera calibration information ([`M_USE_SOURCE_FIRST`](../../Reference/gra/MgraControlList.md)), positions and dimensions are transformed from this relative coordinate system to the absolute coordinate system; then, since it is assumed that there is only one absolute coordinate system, these positions and dimensions are transformed from the absolute coordinate system to the pixel coordinate system, using the destination camera calibration information.

| Value | Description |
| --- | --- |
| `M_USE_DESTINATION_FIRST` *(default)* | Specifies that the camera calibration information of the destination is used when rendering the graphic. |
| `M_USE_SOURCE_FIRST` | Specifies that the camera calibration information of the graphic, set with [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraInquire.md), is used when rendering the graphic. |

---

### `M_GRAPHIC_CONVERSION_MODE`

Inquires how the shape of a graphic, defined in world units, is converted to pixels (rendered).

| Value | Description |
| --- | --- |
| `M_PRESERVE_SHAPE_AVERAGE` | Specifies to render the graphic so that its shape is preserved, even if it means not respecting the camera calibration information exactly. |
| `M_RESHAPE_FOLLOWING_DISTORTION` | Specifies that all points along the contour of the graphic will be converted using the camera calibration information, following any non-linear distortion. |
| `M_RESHAPE_FROM_POINTS` *(default)* | Specifies that only a few key points or features will be converted using the camera calibration information; from these converted points, the rest of the graphic will be rendered respecting the shape of the graphic. |

---

### `M_GRAPHIC_SOURCE_CALIBRATION`

Inquires the identifier of the camera calibration information used to interpret positioning and dimensioning information of a graphic defined in world units.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no source camera calibration information is available to interpret positioning and dimensioning information of a graphic defined in world units, except when the graphic is drawn using the draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)) of a processing or analysis module; in this case, the camera calibration information of the context or result will be used. |
| `Aurora Imaging Library Identifier` | Specifies the identifier of a camera calibration context, image buffer or processing or analysis module result buffer, whose camera calibration information to use; this object is only associated with the 2D graphics context, not copied. |

---

### `M_INPUT_UNITS`

Inquires the units with which to interpret the graphic's positional and dimensional information.

| Value | Description |
| --- | --- |
| `M_DISPLAY` | Specifies to interpret the values in pixel units that, unlike [`M_PIXEL`](../../Reference/gra/MgraInquire.md), are not altered when the display is panned or zoomed. |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_LINE_THICKNESS`

Inquires the thickness of the lines and diameter of the dots.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the thickness of the lines and diameter of the dots, in pixels. |

---

### `M_OWNER_SYSTEM`

Inquires the Aurora Imaging Library identifier (_AIL_ID_) of the system on which the 2D graphics context has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_OWNER_SYSTEM_TYPE`

Inquires the type of system on which the 2D graphics context was allocated.

| Value | Description |
| --- | --- |
| `M_SYSTEM_CLARITY_UHD_TYPE` | Specifies an Aurora Imaging Library Clarity UHD system. |
| `M_SYSTEM_CONCORD_POE_TYPE` | Specifies an Aurora Imaging Library Concord POE system. |
| `M_SYSTEM_GENTL_TYPE` | Specifies an Aurora Imaging Library GenTL system. |
| `M_SYSTEM_GEVIQ_TYPE` | Specifies an Aurora Imaging Library GevIQ system. |
| `M_SYSTEM_GIGE_VISION_TYPE` | Specifies an Aurora Imaging Library GigE Vision system. |
| `M_SYSTEM_HOST_TYPE` | Specifies the Host. |
| `M_SYSTEM_INDIO_TYPE` | Specifies an Aurora Imaging Library Indio system. |
| `M_SYSTEM_IRIS_GTX_TYPE` | Specifies an Aurora Imaging Library Iris GTX system. |
| `M_SYSTEM_RADIENTEVCL_TYPE` | Specifies an Aurora Imaging Library Radient eV-CL system. |
| `M_SYSTEM_RAPIXOCL_TYPE` | Specifies an Aurora Imaging Library Rapixo Pro CL system. |
| `M_SYSTEM_RAPIXOCOF_TYPE` | Specifies an Aurora Imaging Library Rapixo CoF system. |
| `M_SYSTEM_RAPIXOCXP_TYPE` | Specifies an Aurora Imaging Library Rapixo CXP system. |
| `M_SYSTEM_USB3_VISION_TYPE` | Specifies an Aurora Imaging Library USB3 Vision system. |
| `M_SYSTEM_V4L2_TYPE` | Specifies an Aurora Imaging Library Video4Linux2 system. |

### For inquiring about a setting related to interactivity

To inquire 2D graphics context settings that affect the interactivity of a graphic, drawn in a 2D graphics list associated with a display when interactive mode is enabled ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md)), the [`InquireType`](../../Reference/gra/MgraInquire.md) parameter should be set to one of the following values.

---

### `M_EDITABLE`

Inquires whether a graphic can be edited via user interaction in an interactive display. If a graphic is not visible or if it is not selectable, it is not editable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be edited via user interaction. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be edited via user interaction. |

---

### `M_RESIZABLE`

Inquires whether a graphic can be resized via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not resizable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be resized via user interaction and the resize handles will not be displayed if a graphic is selected. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be resized via user interaction by clicking and dragging one of the resize handles. |

---

### `M_ROTATABLE`

Inquires whether a graphic can be rotated via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not rotatable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be rotated via user interaction and the rotate handle will not be displayed if a graphic is selected. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be rotated via user interaction by clicking and dragging the rotation handle. |

---

### `M_SELECTABLE`

Inquires whether a graphic in a 2D graphics list can be selected via user interaction in an interactive display. If a graphic is not visible, it is not selectable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be selected via user interaction. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be selected by clicking on it. |

---

### `M_SPECIFIC_FEATURES_EDITABLE`

Inquires whether a graphic can be modified via user interaction in an interactive display using handles that are specific to its graphic type. If a graphic is not visible, not selectable, or not editable, it cannot be modified using its type-specific handles and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be modified via user interaction and its specific feature handles will not be displayed if a graphic is selected. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be modified via user interaction by clicking and dragging one of the specific feature handles. |

---

### `M_TRANSLATABLE`

Inquires whether a graphic can be moved (translated) via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not movable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be moved via user interaction. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be moved via user interaction by clicking and dragging the graphic, its selection box, or its center handle. |

---

### `M_VISIBLE`

Inquires whether a graphic is rendered on the display.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not rendered. |
| `M_TRUE` *(default)* | Specifies that the graphic is rendered. |

### For inquiring about a setting specific to text

To inquire 2D graphics context settings that apply only to text ([`MgraText`](../../Reference/gra/MgraText.md)), the [`InquireType`](../../Reference/gra/MgraInquire.md) parameter should be set to one of the following values.

---

### `M_BACKCOLOR`

Inquires the background color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. |
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
| `Value` | Specifies a grayscale value. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_BACKGROUND_MODE`

Inquires whether the text's background is filled.

| Value | Description |
| --- | --- |
| `M_OPAQUE` *(default)* | Specifies that the background will be filled with the current background color before drawing text. |
| `M_TRANSPARENT` | Specifies not to change the background before drawing text. |

---

### `M_FONT`

Inquires the character font.

| Value | Description |
| --- | --- |
| `M_FONT_DEFAULT` | Same as [`M_FONT_DEFAULT_SMALL`](../../Reference/gra/MgraInquire.md). |
| `M_FONT_DEFAULT_LARGE` | Specifies a large bitmap font, where each character is drawn in a 16x32 pixel area. |
| `M_FONT_DEFAULT_MEDIUM` | Specifies a medium bitmap font, where each character is drawn in a 12x24 pixel area. |
| `M_FONT_DEFAULT_SMALL` | Specifies a small bitmap font, where each character is drawn in a 8x16 pixel area. |
| `M_FONT_DEFAULT_TTF` | Specifies the default TrueType font of your operating system. |
| `M_FONT_TTF` | Specifies a TrueType font. This value is returned when you have specified a TrueType font using [`MgraFont`](../../Reference/gra/MgraFont.md). |

---

### `M_FONT_AUTO_SELECT`

Inquires whether Aurora Imaging Library will search for a suitable font to draw text if the currently selected font is a TrueType font that does not support the character code.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not search for a suitable font. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library will search for a suitable font. |

---

### `M_FONT_SIZE`

Inquires the size text is drawn when using a TrueType font.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the text's font size, in points. |

---

### `M_FONT_X_SCALE`

Inquires the font's horizontal scaling factor.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the factor by which to multiply the width of the font's characters. |

---

### `M_FONT_Y_SCALE`

Inquires the font's vertical scaling factor.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the factor by which to multiply the height of the font's characters. |

---

### `M_TEXT_ALIGN_HORIZONTAL`

Inquires the horizontal alignment of text.

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies that text is horizontally centered. |
| `M_LEFT` *(default)* | Specifies that text is left-aligned. |
| `M_RIGHT` | Specifies that text is right-aligned. |

---

### `M_TEXT_ALIGN_VERTICAL`

Inquires the vertical alignment of text.

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that text is bottom-aligned. |
| `M_CENTER` | Specifies that text is vertically centered. |
| `M_TOP` *(default)* | Specifies that text is top-aligned. |

---

### `M_TEXT_BORDER`

Inquires how borders are drawn around the text.  Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific border.

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that a line is drawn underneath the text. |
| `M_LEFT` | Specifies that a line is drawn to the left of the text. |
| `M_NONE` *(default)* | Specifies that no border is drawn around the text. |
| `M_RIGHT` | Specifies that a line is drawn to the right of the text. |
| `M_TOP` | Specifies that a line is drawn above the text. |

---

### `M_TEXT_DIRECTION`

Inquires the direction text is drawn when using a TrueType font.

| Value | Description |
| --- | --- |
| `M_LEFT_TO_RIGHT` *(default)* | Specifies that text will be drawn from left to right. |
| `M_RIGHT_TO_LEFT` | Specifies that text will be drawn from right to left. |

### Combination Constants — For inquiring the color value used (for 16- or 32-bit color buffers)

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the color used in the 2D graphics context for a 16-bit or 32-bit color buffer.

You must inquire each color band (R,G, and B) separately.

| Value | Description |
| --- | --- |
| `M_BLUE` | Inquires the blue color band. |
| `M_GREEN` | Inquires the green color band. |
| `M_RED` | Inquires the red color band. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/gra/MgraInquireList.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/gra/MgraInquireList.md) and [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraInquireList.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
