---
doctype: Reference
module: gra
function: MgraControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraControl"
---

# MgraControl

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

> Control a specified 2D graphics context setting.

## Syntax

```c
void MgraControl(
    AIL_ID     ContextGraId,  //out
    AIL_INT64  ControlType,   //in
    AIL_DOUBLE ControlValue   //in
)
```

## Description

This function allows you to control the specified 2D graphics context setting. Most of the control type settings can be inquired using [`MgraInquire`](../../Reference/gra/MgraInquire.md). To control a 2D graphics list, use [`MgraControlList`](../../Reference/gra/MgraControlList.md).

## Parameters

### `ContextGraId` *(out, AIL_ID)*

Specifies the identifier of the 2D graphics context to control. This parameter must be set to one of the following:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the 2D graphics context setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value to assign to the 2D graphics context setting.

## Parameter Associations

### For general 2D graphics context settings

The following [`ControlType`](../../Reference/gra/MgraControl.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControl.md) parameter settings are used to change general 2D graphics context settings. Unless otherwise specified, these affect all types of graphics.

---

### `M_COLOR`

Sets the foreground color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive.  When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands.  To specify a value for a specific band of a 16-bit or 32-bit color buffer, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md) combined with the appropriate constant ([`M_RED`](../../Reference/gra/MgraControl.md), [`M_GREEN`](../../Reference/gra/MgraControl.md), or [`M_BLUE`](../../Reference/gra/MgraControl.md)). |
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
| `Value` | Specifies a grayscale value. The buffer can be a 1-band or multi-band buffer. The specified value is cast to the buffer type and depth.  The default value is -1. For example, this value is equivalent to 0xFF for an 8-bit buffer.  Note that a grayscale value can be any integer or floating-point number. If the given value exceeds the range of the possible values that can be stored in each band of the destination buffer, the least significant bits of the value are used. |

---

### `M_DRAW_DIRECTION`

Sets how to draw (render) an arrow pointing in the direction with which a graphic was defined. This only affects graphics that are not filled, and that have such a direction as part of their definition; for example, arc, ellipse, line, rectangle, and ring. Aurora Imaging Library ignores this setting for other graphics. This setting does not affect drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)).  For more information about how the directions are drawn for different graphics, see [Drawing graphics in a calibrated image](../../UserGuide/C26_Generating_graphics/S04_Drawing_graphics.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that no direction is drawn. |
| `M_PRIMARY_DIRECTION` | Specifies to draw an arrow showing the primary direction of the graphic. For example, if you draw the primary direction for a rectangle defined with [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), the arrow will point in the angular direction set with the [`Angle`](../../Reference/gra/MgraRectAngle.md) parameter. |
| `M_PRIMARY_DIRECTION + M_SECONDARY_DIRECTION` | Specifies to draw an arrow showing both the primary and secondary direction of the graphic. |
| `M_SECONDARY_DIRECTION` | Specifies to draw an arrow showing the secondary direction of the graphic. For example, a rectangle's secondary direction is equal to its `_PrimaryDirection_ - 90°`. |

---

### `M_DRAW_OFFSET_X`

Sets the offset to subtract from the source X-coordinates before rendering the graphic.  This is useful to take results obtained relative to an offset and draw them relative to the top-left corner of the destination image of a draw operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate offset to subtract, in pixels. |

---

### `M_DRAW_OFFSET_Y`

Sets the offset to subtract from the source Y-coordinates before rendering the graphic.  This is useful to take results obtained relative to an offset and draw them relative to the top-left corner of the destination image of a draw operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate offset to subtract, in pixels. |

---

### `M_DRAW_ZOOM_X`

Sets the scale factor in the X-direction. This value is useful to zoom drawings of results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scale factor in the X-direction. |

---

### `M_DRAW_ZOOM_Y`

Sets the scale factor in the Y-direction. This value is useful to zoom drawings of results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scale factor in the Y-direction. |

---

### `M_FIXTURE`

Sets which camera calibration information to use when rendering (drawing or displaying) a graphic that has been defined in world units ([`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to [`M_WORLD`](../../Reference/gra/MgraControl.md)).  Aurora Imaging Library can interpret positioning and dimensioning information of the graphic solely using the camera calibration information associated with the destination image buffer (or the image buffer selected to the display). Alternatively, you can have Aurora Imaging Library use the explicitly specified camera calibration information ([`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControl.md)). If only the destination of the graphic is associated with camera calibration information, it will be used to render the graphic. However, if both are available, you should use [`M_FIXTURE`](../../Reference/gra/MgraControl.md) to identify with respect to which of their relative coordinate systems has positioning and dimensioning information been specified. If with respect to that of the destination camera calibration information ([`M_USE_DESTINATION_FIRST`](../../Reference/gra/MgraControl.md)), the source camera calibration information is not used. If with respect to that of the source camera calibration information ([`M_USE_SOURCE_FIRST`](../../Reference/gra/MgraControl.md)), positions and dimensions are transformed from this relative coordinate system to the absolute coordinate system; then, since it is assumed that there is only one absolute coordinate system, these positions and dimensions are transformed from the absolute coordinate system to the pixel coordinate system, using the destination camera calibration information.

| Value | Description |
| --- | --- |
| `M_USE_DESTINATION_FIRST` *(default)* | Specifies that the camera calibration information of the destination is used when rendering the graphic. In this case, the graphic follows the relative coordinate system of the destination. This is most often used when working with ROIs. |
| `M_USE_SOURCE_FIRST` | Specifies that the camera calibration information of the graphic, set with [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControl.md), is used when rendering the graphic. In this case, the graphic is positioned independent of the destination's relative coordinate system; the destination camera calibration information is only used, if available, for the world to pixel mapping. |

---

### `M_GRAPHIC_CONVERSION_MODE`

Sets how the shape of a graphic, defined in world units, is converted to pixels (rendered).  Drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)) ignore this control type; instead the graphics are drawn using settings defined in their respective modules.

| Value | Description |
| --- | --- |
| `M_PRESERVE_SHAPE_AVERAGE` | Specifies to render the graphic so that its shape is preserved, even if it means not respecting the camera calibration information exactly. For example, a rectangle in world units will be mapped to a rectangle in pixel units, even if the accurate conversion would not yield a rectangle.  This setting applies only to dots ([`MgraDot`](../../Reference/gra/MgraDot.md) and [`MgraDots`](../../Reference/gra/MgraDots.md)) and to rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md) and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)).  *[Image: ConversionMode_PreserveShapeAverage.png]* |
| `M_RESHAPE_FOLLOWING_DISTORTION` | Specifies that all points along the contour of the graphic will be converted using the camera calibration information, following any non-linear distortion. This mode is slower, but more accurate.  *[Image: ConversionMode_ReshapeFollowingDistortion.png]* |
| `M_RESHAPE_FROM_POINTS` *(default)* | Specifies that only a few key points or features will be converted using the camera calibration information; from these converted points, the rest of the graphic will be rendered respecting the shape of the graphic. For example, when converting a line segment from world to pixel units, the two end points are mapped to pixel units, and then a straight line is drawn connecting these two points, ignoring any non-linear distortion in-between. This is the fastest mode, and is accurate as long as there is only linear distortion.  *[Image: ConversionMode_ReshapeFromPoints.png]* |

---

### `M_GRAPHIC_SOURCE_CALIBRATION`

Sets the camera calibration information to use to interpret positioning and dimensioning information of a graphic defined in world units ([`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to [`M_WORLD`](../../Reference/gra/MgraControl.md)), if you have set [`M_FIXTURE`](../../Reference/gra/MgraControl.md) to [`M_USE_SOURCE_FIRST`](../../Reference/gra/MgraControl.md). If the destination also has camera calibration information, positioning and dimensioning information are transformed from the relative coordinate system of the source camera calibration information to its absolute coordinate system. Then, since it is assumed that there is only one absolute coordinate system, the dimensions and positions are transformed from the absolute coordinate system to the pixel coordinate system, using the destination camera calibration information.  Note that this camera calibration information is also used if you have set [`M_FIXTURE`](../../Reference/gra/MgraControl.md) to [`M_USE_DESTINATION_FIRST`](../../Reference/gra/MgraControl.md), but the graphic is drawn in an image buffer that has no camera calibration information.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Specifies that no source camera calibration information is available to interpret positioning and dimensioning information of a graphic defined in world units, except when the graphic is drawn using the draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)) of a processing or analysis module; in this case, the camera calibration information of the context or result will be used. |
| `Aurora Imaging Library Identifier` | Specifies the identifier of a camera calibration context, image buffer or processing or analysis module result buffer, whose camera calibration information to use; this object is only associated with the 2D graphics context, not copied. If, for example, the relative coordinate system changes after using [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControl.md), it will be reflected in the camera calibration information that is used. |

---

### `M_INPUT_UNITS`

Sets the units with which to interpret the graphic's position and dimensional information. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DISPLAY` | Specifies to interpret the values in pixel units that, unlike [`M_PIXEL`](../../Reference/gra/MgraControl.md), are not altered when the display is panned or zoomed. This can be useful if, for example, you always want to display the camera's current frame rate at the top-left corner. [`M_DISPLAY`](../../Reference/gra/MgraControl.md) must only be specified when drawing in a 2D graphics list and the 2D graphics list is used to annotate the display non-destructively, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md). |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. If you are drawing in a 2D graphics list and the 2D graphics list is used to annotate the display non-destructively ([`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)), the graphic will be zoomed and panned according to the display's zoom and pan settings. This is not the case when setting the units to [`M_DISPLAY`](../../Reference/gra/MgraControl.md), which also uses pixel units. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If you are drawing in a 2D graphics list and the 2D graphics list is used to annotate the display non-destructively ([`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)), the graphic will be zoomed and panned according to the display's zoom and pan settings.  When using [`M_WORLD`](../../Reference/gra/MgraControl.md), a graphic's coordinates are interpreted as real-world values and transformed to pixel values before being drawn or associated with the display non-destructively.  Note that using the camera calibration information associated with the image ([`M_WORLD`](../../Reference/gra/MgraControl.md)), ensures that whenever the relative coordinate system moves (for example, with [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md), [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md), or [`McalFixture`](../../Reference/cal/McalFixture.md)), the non-destructive annotations move accordingly. In this case, the position of the graphics is set according to the position of the relative coordinate system.  If world units are specified, drawing a graphic or calling [`MgraFill`](../../Reference/gra/MgraFill.md) generates an error if the operation is performed on an uncalibrated image and [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControl.md) is set to [`M_NULL`](../../Reference/gra/MgraControl.md). |

---

### `M_LINE_THICKNESS`

Sets the thickness of the lines and the diameter of dots, depending on the drawing operation.  > **Note:** Note that for this setting, the thickness of the line is not affected by panning or zooming, and is always interpreted in pixel units.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the thickness of the lines and diameter of the dots, in pixels. The value must be an odd integer. |

### For settings that apply to graphics when interactive mode is enabled

The following [`ControlType`](../../Reference/gra/MgraControl.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControl.md) parameter settings are used to change the 2D graphics context settings that affect the interactivity of a graphic, drawn in a 2D graphics list associated with a display, when interactive mode is enabled ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md)).  Note, you can still change these settings even if interactive mode is disabled, but they will have no effect until interactive mode is enabled. Adding a graphic that does not support one of the following settings to a 2D graphics list will automatically assign the setting of the graphic to [`M_DISABLE`](../../Reference/gra/MgraControlList.md).

---

### `M_EDITABLE`

Sets whether a graphic can be edited via user interaction in an interactive display. If a graphic is not visible or if it is not selectable, it is not editable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be edited via user interaction. Note that the graphic can still be modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` *(default)* | Specifies that the graphic can be edited via user interaction. |

---

### `M_RESIZABLE`

Sets whether a graphic can be resized via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not resizable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be resized via user interaction and the resize handles will not be displayed if a graphic is selected. Note that the graphic can still be resized using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` *(default)* | Specifies that the graphic can be resized via user interaction by clicking and dragging one of the resize handles. |

---

### `M_ROTATABLE`

Sets whether a graphic can be rotated via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not rotatable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be rotated via user interaction and the rotate handle will not be displayed if a graphic is selected. Note that the graphic can still be rotated using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` *(default)* | Specifies that the graphic can be rotated via user interaction by clicking and dragging the rotation handle. |

---

### `M_SELECTABLE`

Sets whether a graphic can be selected via user interaction in an interactive display. If a graphic is not visible, it is not selectable and this setting is ignored.  This setting applies to all graphics except dots ([`MgraDots`](../../Reference/gra/MgraDots.md)) and sets of finite length lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be selected via user interaction. Note that the selection can still be changed using [`MgraControlList`](../../Reference/gra/MgraControlList.md), but it will not be apparent on the display. |
| `M_ENABLE` *(default)* | Specifies that the graphic can be selected by clicking on it. |

---

### `M_SPECIFIC_FEATURES_EDITABLE`

Sets whether a graphic can be modified via user interaction in an interactive display using handles that are specific to its graphic type. If a graphic is not visible, not selectable, or not editable, it cannot be modified using its type-specific handles and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be modified via user interaction and its specific feature handles will not be displayed if a graphic is selected. Note that the graphic can still be modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` *(default)* | Specifies that the graphic can be modified via user interaction by clicking and dragging one of the specific feature handles. |

---

### `M_TRANSLATABLE`

Sets whether a graphic can be moved (translated) via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not movable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be moved via user interaction. Note that the graphic can still be moved using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` *(default)* | Specifies that the graphic can be moved via user interaction by clicking and dragging the graphic, its selection box, or its center handle. |

---

### `M_VISIBLE`

Sets whether a graphic is rendered on the display. Note that a graphic that is not visible cannot be affected by any user interaction in an interactive display.  This setting applies to all types of graphics.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not rendered. |
| `M_TRUE` *(default)* | Specifies that the graphic is rendered. |

### For settings that apply only to text

The following [`ControlType`](../../Reference/gra/MgraControl.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControl.md) parameter settings are used to change 2D graphics context settings that apply only to text ([`MgraText`](../../Reference/gra/MgraText.md)).

---

### `M_BACKCOLOR`

Sets the background color.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when using the 2D graphics context to draw in an 8-bit, 3-band buffer. The red, green, and blue values must be values between 0 and 255, inclusive.  When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands.  To specify a value for a specific band of a 16-bit or 32-bit color buffer, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_BACKCOLOR`](../../Reference/gra/MgraControl.md) combined with the appropriate constant ([`M_RED`](../../Reference/gra/MgraControl.md), [`M_GREEN`](../../Reference/gra/MgraControl.md), or [`M_BLUE`](../../Reference/gra/MgraControl.md)). |
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
| `Value` | Specifies a grayscale value. The buffer can be a 1-band or multi-band buffer. The specified value is cast to the buffer type and depth.  The default value is 0.  Note that a grayscale value can be any integer or floating-point number. If the given value exceeds the range of the possible values that can be stored in each band of the destination buffer, the least significant bits of the value are used. |

---

### `M_BACKGROUND_MODE`

Sets whether to fill the text's background.

| Value | Description |
| --- | --- |
| `M_OPAQUE` *(default)* | Specifies that the background will be filled with the current background color before drawing text. |
| `M_TRANSPARENT` | Specifies not to change the background before drawing text. This creates a transparent background for printed characters. |

---

### `M_FONT_AUTO_SELECT`

Sets whether Aurora Imaging Library should search for a suitable font to draw text if the currently selected font ([`MgraFont`](../../Reference/gra/MgraFont.md)) is a TrueType font that does not support the character code. Aurora Imaging Library will first attempt to make its selection from already used fonts, and then from system fonts.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not search for a suitable font. An error is returned if the character code cannot be drawn using the current TrueType font. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library will search for a suitable font. |

---

### `M_FONT_SIZE`

Sets the size to draw text when using a TrueType font. When using a bitmap font, use [`M_FONT_X_SCALE`](../../Reference/gra/MgraControl.md) and [`M_FONT_Y_SCALE`](../../Reference/gra/MgraControl.md) to set the font size.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the text's font size, in points. |

---

### `M_FONT_X_SCALE`

Sets the font's horizontal scaling factor. This setting only applies to bitmap fonts. For TrueType fonts, use [`M_FONT_SIZE`](../../Reference/gra/MgraControl.md) to set the font size.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the factor by which to multiply the width of the font's characters. Note that using a font with a scale of 1.0 accelerates text drawing. |

---

### `M_FONT_Y_SCALE`

Sets the font's vertical scaling factor. This setting only applies to bitmap fonts. For TrueType fonts, use [`M_FONT_SIZE`](../../Reference/gra/MgraControl.md) to set the font size.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the factor by which to multiply the height of the font's characters. Note that using a font with a scale of 1.0 accelerates text drawing. |

---

### `M_TEXT_ALIGN_HORIZONTAL`

Sets the horizontal alignment of text.  The alignment is relative to the point at which the text starts, as specified with [`MgraText`](../../Reference/gra/MgraText.md).

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies that text is horizontally centered. |
| `M_LEFT` *(default)* | Specifies that text is left-aligned. |
| `M_RIGHT` | Specifies that text is right-aligned. |

---

### `M_TEXT_ALIGN_VERTICAL`

Sets the vertical alignment of text.  The alignment is relative to the point at which the text starts, as specified with [`MgraText`](../../Reference/gra/MgraText.md).

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that text is bottom-aligned. |
| `M_CENTER` | Specifies that text is vertically centered. |
| `M_TOP` *(default)* | Specifies that text is top-aligned. |

---

### `M_TEXT_BORDER`

Sets borders around the text.  Note that the possible settings can be combined. For example, to draw a box around the text, specify [`M_TOP`](../../Reference/gra/MgraControl.md)+[`M_BOTTOM`](../../Reference/gra/MgraControl.md)+[`M_LEFT`](../../Reference/gra/MgraControl.md)+[`M_RIGHT`](../../Reference/gra/MgraControl.md).

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that a line is drawn underneath the text. |
| `M_LEFT` | Specifies that a line is drawn to the left of the text. |
| `M_NONE` *(default)* | Specifies that no border is drawn around the text. This setting cannot be combined with any other setting. |
| `M_RIGHT` | Specifies that a line is drawn to the right of the text. |
| `M_TOP` | Specifies that a line is drawn above the text. |

---

### `M_TEXT_DIRECTION`

Sets the direction to draw text when using a TrueType font.

| Value | Description |
| --- | --- |
| `M_LEFT_TO_RIGHT` *(default)* | Specifies that text will be drawn from left to right. |
| `M_RIGHT_TO_LEFT` | Specifies that text will be drawn from right to left. |

### Combination Constants — For specifying the color band (for 16- or 32-bit color buffers)

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the color band.

| Value | Description |
| --- | --- |
| `M_BLUE` | Specifies the blue color band. |
| `M_GREEN` | Specifies the green color band. |
| `M_RED` | Specifies the red color band. |

This setting applies to all types of graphics except dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), text ([`MgraText`](../../Reference/gra/MgraText.md)), sets of finite length lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)).

This setting applies to all types of graphics except a dot ([`MgraDot`](../../Reference/gra/MgraDot.md)), dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), text ([`MgraText`](../../Reference/gra/MgraText.md)), sets of finite length lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)).
