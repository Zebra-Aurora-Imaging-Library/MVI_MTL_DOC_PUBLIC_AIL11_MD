---
doctype: Reference
module: gra
function: MgraControlList
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraControlList"
---

# MgraControlList

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

> Control the graphics contained within the 2D graphics list.

## Syntax

```c
void MgraControlList(
    AIL_ID     GraListId,     //out
    AIL_INT    LabelOrIndex,  //in
    AIL_INT    SubIndex,      //in
    AIL_INT64  ControlType,   //in
    AIL_DOUBLE ControlValue   //in
)
```

## Description

This function allows you to control the settings of graphics contained within the 2D graphics list. Most of the control type settings can be inquired using [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

When graphics are added to the list, they inherit the current settings of the 2D graphics context. However, subsequent changes to the 2D graphics context (for example, with [`MgraControl`](../../Reference/gra/MgraControl.md)) do not affect graphics already in the list. The settings of graphics in the list can only be modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md).

Using this function, you can also control a sub-element of a graphic in a 2D graphics list. This is useful when a single point of a graphic requires repositioning. For example, you can reposition ([`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md)) a vertex point of a polygon ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md)) by specifying the polygon graphic ([`LabelOrIndex`](../../Reference/gra/MgraControlList.md)), and one of its points ([`SubIndex`](../../Reference/gra/MgraControlList.md)).

All geometric type changes that you make to the graphics (such as, rotation, scaling, and translation) are applied according to the current input units of each graphic in the list, as specified with [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md), on a per graphic basis. For example, if you change the position of two graphics in the list, a dot set in pixel units and a rectangle set in world units, the dot's position will change in pixel units and the rectangle's position will change in world units. However, changing [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md) to a different value will not convert a graphic's position.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`GraListId`](../../Reference/gra/MgraControlList.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `GraListId` *(out, AIL_ID)*

Specifies the identifier of the 2D graphics list to control. The 2D graphics list must have been previously allocated on the required system using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the graphic (one or all) to control. This parameter must be set to one of the following values:

*For specifying the 2D graphics list or graphic therein*

| Value | Description |
| --- | --- |
| `M_GRAPHIC_INDEX` | Specifies the index of an existing graphic on which to apply the control setting. |
| `M_GRAPHIC_LABEL` | Specifies the label of an existing graphic on which to apply the control setting. |
| `M_ALL` | Applies the specified control setting to all the graphics contained within the 2D graphics list. The graphics that do not support the specified control type setting remain unchanged. |
| `M_ALL_SELECTED` | Applies the specified control setting to all the graphics contained within the 2D graphics list that are currently selected (that is, all graphics with [`M_GRAPHIC_SELECTED`](../../Reference/gra/MgraControlList.md) set to [`M_TRUE`](../../Reference/gra/MgraControlList.md)). The graphics that are not selected remain unchanged. |
| `M_LIST` | Applies the specified control setting to the 2D graphics list itself. |

### `SubIndex` *(in, AIL_INT)*

Specifies the index of the sub-element of the graphic on which to apply the control setting. If this information is not required or supported, set this parameter to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

*For specifying the index of a graphic's sub-element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to apply the control setting to the graphic itself (instead of just a sub-element). |
| `0 <= Value <= M_NUMBER_OF_SUB_ELEMENTS` | Specifies the index of the sub-element of the graphic on which to apply the control setting. The following table lists all the graphics for which you can control their individual sub-elements and outlines how sub-indices are assigned for each of these.

| Graphics types | Index of sub-element values |
| --- | --- |
| Dots (created using [`MgraDots`](../../Reference/gra/MgraDots.md)). | The indices, starting from 0, are assigned to each dot in order of creation. |
| A single line (created using [`MgraLine`](../../Reference/gra/MgraLine.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_LINE`](../../Reference/gra/MgraInteractive.md)). | 0 is assigned to the specified start point, and 1 is assigned to the specified end point of the line. |
| Sets of finite length lines (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)). | The indices, starting from 0, are assigned to the specified start point and end point of each line, in order of creation. For example, the start point of the first line has an index of 0 and the end point has an index of 1, and the start point of the second line has an index of 2 and the end point has an index of 3. |
| Sets of infinite lines (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_INFINITE_LINES`](../../Reference/gra/MgraLines.md)). | The indices, starting from 0, are assigned to the first and second specified point of each line, in order of creation. For example, the first specified point of the first line has an index of 0 and its second specified point has an index of 1, and the first specified point of the second line has an index of 2 and its second specified point has an index of 3. |
| A polygon (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/gra/MgraInteractive.md)). | The indices, starting from 0, are assigned to each polygon vertex in order of creation. |
| A polyline (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYLINE`](../../Reference/gra/MgraLines.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_POLYLINE`](../../Reference/gra/MgraInteractive.md)). | The indices, starting from 0, are assigned to each polyline point in order of creation. | |

### `ControlType` *(in, AIL_INT64)*

Specifies the 2D graphics list setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value to assign to the 2D graphics list setting.

## Parameter Associations

### For changing general settings of the 2D graphics list itself

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to change general settings of a 2D graphics list itself. In this case, set the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter to [`M_LIST`](../../Reference/gra/MgraControlList.md) and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

---

### `M_ANGLE_SNAPPING_VALUE`

Sets the angular value to use as a multiple when rotating the graphic. This value only applies when using [`M_ANGLE_SNAPPING`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 180.0` *(default)* | Specifies the value to use as a multiple, in degrees. The value must be a divisor of 180.0. |

---

### `M_EASY_SELECTION`

Sets whether Aurora Imaging Library allows the easy selection of graphics on the display. When easy selection is enabled, you can select closed graphics (such as circles) by clicking anywhere inside them, even if they are not filled. To select non-filled graphics when easy selection is disabled, you must click on their contour.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not allow for the easy selection of graphics on the display. |
| `M_ENABLE` | Specifies that Aurora Imaging Library allows for the easy selection of graphics on the display. |

---

### `M_INTERACTIVE_ANNOTATIONS_COLOR`

Sets the color of the selection box and handles in interactive mode.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. The red, green, and blue values must be between 0 and 255, inclusive.  When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands.  To specify a value for a specific band of a 16-bit or 32-bit color buffer, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INTERACTIVE_ANNOTATIONS_COLOR`](../../Reference/gra/MgraControlList.md) combined with the appropriate constant ([`M_RED`](../../Reference/gra/MgraControlList.md), [`M_GREEN`](../../Reference/gra/MgraControlList.md), or [`M_BLUE`](../../Reference/gra/MgraControlList.md)). |
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
| `Value` | Specifies a grayscale value. The buffer can be a 1-band or multi-band buffer. The specified value is cast to the buffer type and depth.  Note that a grayscale value can be any integer or floating-point number. If the given value exceeds the range of the possible values that can be stored in each band of the destination buffer, the least significant bits of the value are used. |

---

### `M_MODE_RESIZE`

Sets the interactive behavior when resizing the graphic using its selection box. To resize the graphic, click and move one of its selection box's eight resize handles.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIXED_CENTER` | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic. |
| `M_FIXED_CORNER` *(default)* | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic. If you click on a selection box handle that is not a corner, the opposite side (which includes its corners) does not move when resizing. |

---

### `M_MODE_RESIZE_ALT`

Sets the interactive behavior when resizing the graphic using its selection box and pressing the Alt key. To resize the graphic, click and move (while pressing Alt) one of its selection box's eight resize handles.  If multiple key values ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md), [`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md), or [`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) are set to one of [`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), and multiple keys (Alt, Ctrl, or Shift) are pressed when resizing, [`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), and [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md).  If multiple key values ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md), [`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md), or [`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) are set to one of [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md), and multiple keys (Alt, Ctrl, or Shift) are pressed when resizing, [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md).  If the graphic's aspect ratio is set to 1 using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_CONSTRAIN_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), it will remain 1 ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md) will not change it).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Alt key does not affect how to resize the graphic. In this case, Aurora Imaging Library uses [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). |
| `M_FIXED_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic remains constant, when resizing the graphic while pressing the Alt key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_FIXED_CENTER` | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic while pressing the Alt key. This overrides the value set with [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The aspect ratio combination value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_FIXED_CORNER` | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic while pressing the Alt key. If you click on a selection box handle that is not a corner, the opposite side (which includes its corners) does not move when resizing. This overrides the value set with [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The aspect ratio combination value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_NO_CONSTRAINT` | Specifies that the aspect ratio of the graphic can change, when resizing it while pressing the Alt key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_SQUARE_ASPECT_RATIO` *(default)* | Specifies that the aspect ratio of the graphic will be 1:1 (square), when resizing it while pressing the Alt key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |

---

### `M_MODE_RESIZE_CTRL`

Sets the interactive behavior when resizing the graphic using its selection box and pressing the Ctrl key. To resize the graphic, click and move (while pressing Ctrl) one of its selection box's eight resize handles.  If multiple key values ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md), [`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md), or [`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) are set to one of [`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), and multiple keys (Alt, Ctrl, or Shift) are pressed when resizing, [`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), and [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md).  If multiple key values ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md), [`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md), or [`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) are set to one of [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md), and multiple keys (Alt, Ctrl, or Shift) are pressed when resizing, [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md).  If the graphic's aspect ratio is set to 1 using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_CONSTRAIN_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), it will remain 1 ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md) will not change it).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Ctrl key does not affect how to resize the graphic. In this case, Aurora Imaging Library uses [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). |
| `M_FIXED_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic remains constant, when resizing the graphic while pressing the Ctrl key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_FIXED_CENTER` *(default)* | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic while pressing the Ctrl key. This overrides the value set with [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The aspect ratio combination value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_FIXED_CORNER` | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic while pressing the Ctrl key. If you click on a selection box handle that is not a corner, the opposite side (which includes its corners) does not move when resizing. This overrides the value set with [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The aspect ratio combination value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_NO_CONSTRAINT` | Specifies that the aspect ratio of the graphic can change, when resizing it while pressing the Ctrl key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic will be 1:1 (square), when resizing it while pressing the Ctrl key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION`

Sets how to establish the graphic's secondary dimension when defining the graphic interactively on the display. This only applies when creating the graphic using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) set to [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md). In this case, the graphic's secondary dimension refers to the rectangle's height.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ONE_SIDED` *(default)* | Specifies to establish the graphic's secondary dimension on one side of a previously established part. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on one side of the width line. The side depends on whether you move your mouse above or below the width line. |
| `M_SYMMETRIC` | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on both sides of the width line. Moving your mouse above or below the width line adjusts the height equally on each side. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION_ALT`

Sets how to establish the graphic's secondary dimension when defining the graphic interactively on the display and pressing the Alt key. This only applies when creating the graphic using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) set to [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md). In this case, the graphic's secondary dimension refers to the rectangle's height.  Unless you disable [`M_MODE_RESIZE_SECONDARY_DIMENSION_ALT`](../../Reference/gra/MgraControlList.md), its value overrides the value set with [`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that pressing the Alt key does not affect how to establish the graphic's secondary dimension when defining it interactively on the display. In this case, Aurora Imaging Library uses [`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md). |
| `M_ONE_SIDED` | Specifies to establish the graphic's secondary dimension on one side of a previously established part, while pressing the Alt key. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on one side of the width line. The side depends on whether you move your mouse (while pressing Alt) above or below the width line. |
| `M_SYMMETRIC` | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part, while pressing the Alt key. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on both sides of the width line. Moving your mouse (while pressing Alt) above or below the width line adjusts the height equally on each side. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION_CTRL`

Sets how to establish the graphic's secondary dimension when defining the graphic interactively on the display and pressing the Ctrl key. This only applies when creating the graphic using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) set to [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md). In this case, the graphic's secondary dimension refers to the rectangle's height.  Unless you disable [`M_MODE_RESIZE_SECONDARY_DIMENSION_CTRL`](../../Reference/gra/MgraControlList.md), its value overrides the value set with [`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Ctrl key does not affect how to establish the graphic's secondary dimension when defining it interactively on the display. In this case, Aurora Imaging Library uses [`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md). |
| `M_ONE_SIDED` | Specifies to establish the graphic's secondary dimension on one side of a previously established part, while pressing the Ctrl key. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on one side of the width line. The side depends on whether you move your mouse (while pressing Ctrl) above or below the width line. |
| `M_SYMMETRIC` *(default)* | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part, while pressing the Ctrl key. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on both sides of the width line. Moving your mouse (while pressing Ctrl) above or below the width line adjusts the height equally on each side. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION_SHIFT`

Sets how to establish the graphic's secondary dimension when defining the graphic interactively on the display and pressing the Shift key. This only applies when creating the graphic using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) set to [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md). In this case, the graphic's secondary dimension refers to the rectangle's height.  Unless you disable [`M_MODE_RESIZE_SECONDARY_DIMENSION_SHIFT`](../../Reference/gra/MgraControlList.md), its value overrides the value set with [`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that pressing the Shift key does not affect how to establish the graphic's secondary dimension when defining it interactively on the display. In this case, Aurora Imaging Library uses [`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md). |
| `M_ONE_SIDED` | Specifies to establish the graphic's secondary dimension on one side of a previously established part, while pressing the Shift key. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on one side of the width line. The side depends on whether you move your mouse (while pressing Shift) above or below the width line. |
| `M_SYMMETRIC` | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part, while pressing the Shift key. For example, when interactively defining an oriented rectangle graphic, the first two clicks on the display define a line that represents the rectangle's width, and the third click defines its height on both sides of the width line. Moving your mouse (while pressing Shift) above or below the width line adjusts the height equally on each side. |

---

### `M_MODE_RESIZE_SHIFT`

Sets the interactive behavior when resizing the graphic using its selection box and pressing the Shift key. To resize the graphic, click and move (while pressing Shift) one of the selection box's eight resize handles.  If multiple key values ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md), [`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md), or [`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) are set to one of [`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), and multiple keys (Alt, Ctrl, or Shift) are pressed when resizing, [`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), and [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md).  If multiple key values ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md), [`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md), or [`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) are set to one of [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md), and multiple keys (Alt, Ctrl, or Shift) are pressed when resizing, [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) takes precedence over [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md).  If the graphic's aspect ratio is set to 1 using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_CONSTRAIN_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), it will remain 1 ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md) will not change it).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Shift key does not affect how to resize the graphic. In this case, Aurora Imaging Library uses [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). |
| `M_FIXED_ASPECT_RATIO` *(default)* | Specifies that the aspect ratio of the graphic remains constant, when resizing the graphic while pressing the Shift key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_FIXED_CENTER` | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic while pressing the Shift key. This overrides the value set with [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The aspect ratio combination value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_FIXED_CORNER` | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic while pressing the Shift key. If you click on a selection box handle that is not a corner, the opposite side (which includes its corners) does not move when resizing. This overrides the value set with [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The aspect ratio combination value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md), [`M_NO_CONSTRAINT`](../../Reference/gra/MgraControlList.md), or [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_NO_CONSTRAINT` | Specifies that the aspect ratio of the graphic can change, when resizing it while pressing the Shift key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic will be 1:1 (square), when resizing it while pressing the Shift key. This overrides the aspect ratio combination value set for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md). The value that [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) uses ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md) or [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) is not overridden. |

---

### `M_MODE_ROTATE`

Sets the interactive behavior when rotating the graphic using its selection box. To rotate the graphic, click and move its selection box's rotate handle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` *(default)* | Specifies to rotate the graphic freely. |

---

### `M_MODE_ROTATE_ALT`

Sets the interactive behavior when rotating the graphic using its selection box and pressing the Alt key. To rotate the graphic, click and move (while pressing Alt) its selection box's rotate handle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` *(default)* | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraControlList.md), when rotating it while pressing the Alt key. This overrides the value set with [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |
| `M_DISABLE` | Specifies that pressing the Alt key does not affect how to rotate the graphic. In this case, Aurora Imaging Library uses [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` | Specifies to rotate the graphic freely, when rotating it while pressing the Alt key. This overrides the value set with [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |

---

### `M_MODE_ROTATE_CTRL`

Sets the interactive behavior when rotating the graphic using its selection box and pressing the Ctrl key. To rotate the graphic, click and move (while pressing Ctrl) its selection box's rotate handle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraControlList.md), when rotating it while pressing the Ctrl key. This overrides the value set with [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |
| `M_DISABLE` *(default)* | Specifies that pressing the Ctrl key does not affect how to rotate the graphic. In this case, Aurora Imaging Library uses [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` | Specifies to rotate the graphic freely, when rotating it while pressing the Ctrl key. This overrides the value set with [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |

---

### `M_MODE_ROTATE_SHIFT`

Sets the interactive behavior when rotating the graphic using its selection box and pressing the Shift key. To rotate the graphic, click and move (while pressing Shift) its selection box's rotate handle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraControlList.md), when rotating it while pressing the Shift key. This overrides the value set with [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |
| `M_DISABLE` *(default)* | Specifies that pressing the Shift key does not affect how to rotate the graphic. In this case, Aurora Imaging Library uses [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` | Specifies to rotate the graphic freely, when rotating it while pressing the Shift key. This overrides the value set with [`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md). |

---

### `M_MODE_TRANSLATE`

Sets the interactive behavior when moving (translating) the graphic using its selection box. To move the graphic, hover over its selection box border until the mouse arrow pointer changes to the move pointer, and then click and drag the graphic to a different location on the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` | Specifies to move the graphic along the X- and Y-axes. |
| `M_NO_CONSTRAINT` *(default)* | Specifies to move the graphic freely. |

---

### `M_MODE_TRANSLATE_ALT`

Sets the interactive behavior when moving (translating) the graphic using its selection box and pressing the Alt key. To move the graphic, hover over its selection box border until the mouse arrow pointer changes to the move pointer, and then click and drag the graphic (while pressing Alt) to a different location on the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` | Specifies to move the graphic along the X- and Y-axes, when moving it while pressing the Alt key. This overrides the value set with [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |
| `M_DISABLE` *(default)* | Specifies that pressing the Alt key does not affect how to move the graphic. In this case, Aurora Imaging Library uses [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` | Specifies to move the graphic freely, when moving it while pressing the Alt key. This overrides the value set with [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |

---

### `M_MODE_TRANSLATE_CTRL`

Sets the interactive behavior when moving (translating) the graphic using its selection box and pressing the Ctrl key. To move the graphic, hover over its selection box border until the mouse arrow pointer changes to the move pointer, and then click and drag the graphic (while pressing Ctrl) to a different location on the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` | Specifies to move the graphic along the X- and Y-axes, when moving it while pressing the Ctrl key. This overrides the value set with [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |
| `M_DISABLE` *(default)* | Specifies that pressing the Ctrl key does not affect how to move the graphic. In this case, Aurora Imaging Library uses [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` | Specifies to move the graphic freely, when moving it while pressing the Ctrl key. This overrides the value set with [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |

---

### `M_MODE_TRANSLATE_SHIFT`

Sets the interactive behavior when moving (translating) the graphic using its selection box and pressing the Shift key. To move the graphic, hover over its selection box border until the mouse arrow pointer changes to the move pointer, and then click and drag the graphic (while pressing Shift) to a different location on the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` *(default)* | Specifies to move the graphic along the X- and Y-axes, when moving it while pressing the Shift key. This overrides the value set with [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |
| `M_DISABLE` | Specifies that pressing the Shift key does not affect how to move the graphic. In this case, Aurora Imaging Library uses [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |
| `M_NO_CONSTRAINT` | Specifies to move the graphic freely, when moving it while pressing the Shift key. This overrides the value set with [`M_MODE_TRANSLATE`](../../Reference/gra/MgraControlList.md). |

---

### `M_MULTIPLE_SELECTION`

Sets whether to permit interactive multiple selection using the Ctrl key. When this setting is enabled, a user can select multiple graphics by holding down the Ctrl key and clicking on the graphics that need to be selected. Note that, even if this is disabled, multiple selection is still possible programmatically using [`M_GRAPHIC_SELECTED`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that interactive multiple selection is not permitted. |
| `M_ENABLE` *(default)* | Specifies that interactive multiple selection is permitted. |

---

### `M_MULTIPLE_SELECTION_KEY`

Sets the keyboard key that you can press to select multiple graphics with your mouse on the display. Clicking on a graphic that you have already selected, while pressing the specified key, deselects that graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_KEY_ALT` | Specifies that you can press the Alt key to select (or deselect) multiple graphics. |
| `M_KEY_CTRL` *(default)* | Specifies that you can press the Ctrl key to select (or deselect) multiple graphics. |
| `M_KEY_SHIFT` | Specifies that you can press the Shift key to select (or deselect) multiple graphics. |

---

### `M_SELECTED_COLOR`

Sets the color of the selected graphics in interactive mode.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. The red, green, and blue values must be between 0 and 255, inclusive.  When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands.  To specify a value for a specific band of 16-bit or 32-bit color buffer, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_SELECTED_COLOR`](../../Reference/gra/MgraControlList.md) combined with the appropriate constant ([`M_RED`](../../Reference/gra/MgraControlList.md), [`M_GREEN`](../../Reference/gra/MgraControlList.md), or [`M_BLUE`](../../Reference/gra/MgraControlList.md)). |
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
| `Value` | Specifies a grayscale value. The buffer can be a 1-band or multi-band buffer. The specified value is cast to the buffer type and depth.  Note that a grayscale value can be any integer or floating-point number. If the given value exceeds the range of the possible values that can be stored in each band of the destination buffer, the least significant bits of the value are used. |

---

### `M_SELECTION_RADIUS`

Sets the size of the selection-radius in interactive mode. Selection-radius refers to how close a mouse click needs to be to a graphic to select it. For example if a graphic is at a position that is 4 units radially away from where you clicked, the graphic will appear selected if the selection-radius is set to a value greater than or equal to 4 units.

| Value | Description |
| --- | --- |
| `Value >= 1.0` *(default)* | Specifies the size of the selection-radius, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

### Combination Constants — For specifying the aspect ratio when resizing the graphic

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the aspect ratio when resizing the graphic.

The following values will not modify the graphic's aspect ratio if it is set to 1 using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_CONSTRAIN_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_FIXED_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic remains constant when resizing it. |
| `M_NO_CONSTRAINT` *(default)* | Specifies that the aspect ratio of the graphic can change when resizing it. |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic will be 1:1 (square) when resizing it. |

### For changing the general settings inherited from the 2D graphics context

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to change the general settings inherited from the 2D graphics context when a graphic is initially added to the list. Unless otherwise specified, these affect all types of graphics.  In this case, the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter can be set to one or all graphics and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter must be set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

---

### `M_COLOR`

Sets the foreground color of the graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. The red, green, and blue values must be between 0 and 255, inclusive.  When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands.  To specify a value for a specific band of a 16-bit or 32-bit color buffer, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_COLOR`](../../Reference/gra/MgraControlList.md) combined with the appropriate constant ([`M_RED`](../../Reference/gra/MgraControlList.md), [`M_GREEN`](../../Reference/gra/MgraControlList.md), or [`M_BLUE`](../../Reference/gra/MgraControlList.md)). |
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

Sets the scaling factor in the X-direction. This value is useful to zoom in on drawings of results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scaling factor in the X-direction. |

---

### `M_DRAW_ZOOM_Y`

Sets the scaling factor in the Y-direction. This value is useful to zoom in on drawings of results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scaling factor in the Y-direction. |

---

### `M_FIXTURE`

Sets the camera calibration information to use when rendering (drawing or displaying) a graphic that has been defined in world units ([`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md) set to [`M_WORLD`](../../Reference/gra/MgraControlList.md)).  Aurora Imaging Library can interpret positioning and dimensioning information of the graphic solely using the camera calibration information associated with the destination image buffer (or the image buffer selected to the display). Alternatively, you can have Aurora Imaging Library use the explicitly specified camera calibration information ([`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControlList.md)). If only the destination or the graphic is associated with camera calibration information, it will be used to render the graphic. However, if both are available, you should use [`M_FIXTURE`](../../Reference/gra/MgraControlList.md) to identify with respect to which of their relative coordinate systems has positioning and dimensioning information been specified. If with respect to that of the destination camera calibration information ([`M_USE_DESTINATION_FIRST`](../../Reference/gra/MgraControlList.md)), the source camera calibration information is not used. If with respect to that of the source camera calibration information ([`M_USE_SOURCE_FIRST`](../../Reference/gra/MgraControlList.md)), positions and dimensions are transformed from this relative coordinate system to the absolute coordinate system; then, since it is assumed that there is only one absolute coordinate system, these positions and dimensions are transformed from the absolute coordinate system to the pixel coordinate system, using the destination camera calibration information.

| Value | Description |
| --- | --- |
| `M_USE_DESTINATION_FIRST` | Specifies that the camera calibration information of the destination is used when rendering the graphic. In this case, the graphic follows the relative coordinate system of the destination. This is most often used when working with ROIs. |
| `M_USE_SOURCE_FIRST` | Specifies that the camera calibration information of the graphic, set with [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControlList.md), is used when rendering the graphic. In this case, the graphic is positioned independent of the destination's relative coordinate system; the destination camera calibration information is only used, if available, for the world to pixel mapping. |

---

### `M_GRAPHIC_CONVERSION_MODE`

Sets how the shape of a graphic, defined in world units, is converted to pixels (rendered).  Drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)) ignore this control type; instead the graphics are drawn using settings defined in their respective modules.

| Value | Description |
| --- | --- |
| `M_PRESERVE_SHAPE_AVERAGE` | Specifies to render the graphic so that its shape is preserved even if it means not respecting the camera calibration information exactly. For example, a rectangle in world units will be mapped to a rectangle in pixel units, even if the accurate conversion would not yield a rectangle.  This setting applies only to dots ([`MgraDot`](../../Reference/gra/MgraDot.md) and [`MgraDots`](../../Reference/gra/MgraDots.md)) and to rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md) and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)).  *[Image: ConversionMode_PreserveShapeAverage.png]* |
| `M_RESHAPE_FOLLOWING_DISTORTION` | Specifies that all points along the contour of the graphic will be converted using the camera calibration information, following any non-linear distortion. This mode is slower, but more accurate.  *[Image: ConversionMode_ReshapeFollowingDistortion.png]* |
| `M_RESHAPE_FROM_POINTS` | Specifies that only a few key points or features will be converted using the camera calibration information; from these points, the rest of the graphic will be rendered respecting the shape of the graphic. For example, when converting a line segment from world to pixel units, the two end points are mapped to pixel units, and then a straight line is drawn connecting these two points, ignoring any non-linear distortion in-between. This is the fastest mode, and is accurate as long as there is only linear distortion.  *[Image: ConversionMode_ReshapeFromPoints.png]* |

---

### `M_GRAPHIC_SOURCE_CALIBRATION`

Sets the camera calibration information to use to interpret positioning and dimensioning information of a graphic defined in world units ([`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md) set to [`M_WORLD`](../../Reference/gra/MgraControlList.md)), if you have set [`M_FIXTURE`](../../Reference/gra/MgraControlList.md) to [`M_USE_SOURCE_FIRST`](../../Reference/gra/MgraControlList.md). If the destination also has camera calibration information, positioning and dimensioning information are transformed from the relative coordinate system of the source camera calibration information to its absolute coordinate system. Then, since it is assumed that there is only one absolute coordinate system, the dimensions and positions are transformed from the absolute coordinate system to the pixel coordinate system, using the destination camera calibration information.  Note that this camera calibration information is also used if you have set [`M_FIXTURE`](../../Reference/gra/MgraControlList.md) to [`M_USE_DESTINATION_FIRST`](../../Reference/gra/MgraControlList.md), but the graphic is drawn in an image buffer that has no camera calibration information.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no source camera calibration information is available to interpret positioning and dimensioning information of a graphic defined in world units. |
| `Aurora Imaging Library identifier` | Specifies the identifier of a camera calibration context, image buffer or processing or analysis module result buffer, whose camera calibration information to use. Note that the camera calibration information of this object is copied to an internal camera calibration context; the specified object is not associated with the 2D graphics list. Since the information is being copied, [`MgraInquireList`](../../Reference/gra/MgraInquireList.md) will not return the same identifier as the one passed to [`MgraControlList`](../../Reference/gra/MgraControlList.md); instead it will return the identifier of the copy. This identifier is automatically generated by Aurora Imaging Library. If, for example, the relative coordinate system changes after using [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControlList.md), it will have no effect on the graphic. |

---

### `M_INPUT_UNITS`

Sets the unit with which to interpret the graphic's positional and dimensional information, for graphics already added to the 2D graphics list. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DISPLAY` | Specifies to interpret the values in pixel units that, unlike [`M_PIXEL`](../../Reference/gra/MgraControlList.md), are not altered when the display is panned or zoomed. This can be useful if, for example, you always want to display the camera's current frame rate at the top-left corner. [`M_DISPLAY`](../../Reference/gra/MgraControlList.md) must only be specified when the 2D graphics list is used to annotate the display non-destructively, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md). |
| `M_PIXEL` | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. If the 2D graphics list is used to annotate the display non-destructively ([`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)), the graphics will be zoomed and panned according to the display's zoom and pan settings. This is not the case when setting the units to [`M_DISPLAY`](../../Reference/gra/MgraControlList.md), which also uses pixel units. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If the 2D graphics list is used to annotate the display non-destructively ([`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md)), the graphic will be zoomed and panned according to the display's zoom and pan settings.  When using [`M_WORLD`](../../Reference/gra/MgraControlList.md), a graphic's coordinates are interpreted as real-world values and transformed to pixel values before being drawn or associated with the display non-destructively.  Note that using the camera calibration information associated with the image ([`M_WORLD`](../../Reference/gra/MgraControlList.md)), ensures that whenever the relative coordinate system moves (for example, with [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md), [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md), or [`McalFixture`](../../Reference/cal/McalFixture.md)), the non-destructive annotations move accordingly. In this case, the position of the graphics is set according to the position of the relative coordinate system.  If world units are specified, calling [`MgraDraw`](../../Reference/gra/MgraDraw.md) generates an error if the operation is performed on an uncalibrated image and [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraControlList.md) is set to [`M_NULL`](../../Reference/gra/MgraControlList.md). When the 2D graphics list is associated with the display, nothing will be drawn. |

---

### `M_LINE_THICKNESS`

Sets the thickness of the lines and the diameter of dots, depending on the drawing operation.  > **Note:** Note that for this setting, the thickness of the line is not affected by panning or zooming, and is always interpreted in pixel units.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the thickness of the lines and diameter of the dots, in pixels. The value must be an odd integer. |

### For changing the interactive properties of graphics in the list

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to change settings inherited from the 2D graphics context that affect the interactivity of a graphic, drawn in a 2D graphics list associated with a display, when interactive mode is enabled ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md)).  You can still change these settings even if interactive mode is disabled, but they will have no effect until interactive mode is enabled. If you attempt to change one of these settings to [`M_ENABLE`](../../Reference/gra/MgraControlList.md) for a graphics type that cannot be assigned this value, an error is generated.  In this case, the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter can be set to one or all graphics and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter must be set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

---

### `M_EDITABLE`

Sets whether a graphic can be edited, via user interaction in an interactive display. If a graphic is not visible or if it is not selectable, it is not editable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be edited via user interaction. Note that the graphic can still be modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` | Specifies that the graphic can be edited via user interaction. |

---

### `M_GRAPHIC_SELECTED`

Sets whether a graphic is selected. In an interactive display, a selected graphic will be surrounded by a selection box with handles that allow user interaction.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not selected. |
| `M_TRUE` | Specifies that the graphic is selected. |

---

### `M_RESIZABLE`

Sets whether a graphic can be resized, via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not resizable and the setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be resized via user interaction and the resize handles will not be displayed if a graphic is selected. Note that the graphic can still be resized using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` | Specifies that the graphic can be resized via user interaction by clicking and dragging one of the resize handle. |

---

### `M_ROTATABLE`

Sets whether a graphic can be rotated, via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not rotatable and the setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be rotated via user interaction and the rotate handle will not be displayed if a graphic is selected. Note that the graphic can still be rotated using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` | Specifies that the graphic can be rotated via user interaction by clicking and dragging the rotation handle. |

---

### `M_SELECTABLE`

Sets whether a graphic can be selected, via user interaction in an interactive display. If a graphic is not visible, it is not selectable and this setting is ignored.  This setting applies to all graphics except dots ([`MgraDots`](../../Reference/gra/MgraDots.md)) and sets of finite length lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)). Enabling this setting when specifying an unsupported graphic type will generate an error.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be selected via user interaction. Note that the graphic can still be selected using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` | Specifies that the graphic can be selected by clicking on it. |

---

### `M_SPECIFIC_FEATURES_EDITABLE`

Sets whether a graphic can be modified using handles that are specific to its graphic type via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it cannot be modified using type-specific handles and the setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be modified via user interaction and its specific feature handles will not be displayed if a graphic is selected. Note that the graphic can still be modified using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` | Specifies that the graphic can be modified via user interaction by clicking and dragging one of the type-specific handles. |

---

### `M_TRANSLATABLE`

Sets whether a graphic can be translated (moved), via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not movable and the setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be moved via user interaction. Note that the graphic can still be moved using [`MgraControlList`](../../Reference/gra/MgraControlList.md). |
| `M_ENABLE` | Specifies that the graphic can be moved via user interaction by clicking and dragging the graphic, its selection box, or its center handle. |

---

### `M_VISIBLE`

Sets whether a graphic is rendered on the display. Note that a graphic that is not visible will not be affected by any user interaction in an interactive display.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not rendered. |
| `M_TRUE` | Specifies that the graphic is rendered. |

### For changing the settings of text

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to change the settings inherited from the 2D graphics context when text ([`MgraText`](../../Reference/gra/MgraText.md)) is added to the list.  In this case, the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter can be set to the label or index of text in the 2D graphics list and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter must be set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md). Attempting to change one of the following settings on graphics other than text will generate an error.

---

### `M_BACKCOLOR`

Sets the background color of the text.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. The red, green, and blue values must be between 0 and 255, inclusive.  When drawing in a 16-bit or 32-bit color buffer, the bands of the RGB value are cast to the type of the destination buffer's bands.  To specify a value for a specific band of a 16-bit or 32-bit color buffer, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_BACKCOLOR`](../../Reference/gra/MgraControlList.md) combined with the appropriate constant ([`M_RED`](../../Reference/gra/MgraControlList.md), [`M_GREEN`](../../Reference/gra/MgraControlList.md), or [`M_BLUE`](../../Reference/gra/MgraControlList.md)). |
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
| `Value` | Specifies a grayscale value. The buffer can be a 1-band or multi-band buffer. The specified value is cast to the buffer type and depth.  Note that a grayscale value can be any integer or floating-point number. If the given value exceeds the range of the possible values that can be stored in each band of the destination buffer, the least significant bits of the value are used. |

---

### `M_BACKGROUND_MODE`

Sets whether to fill the text's background.

| Value | Description |
| --- | --- |
| `M_OPAQUE` | Specifies that the background will be filled with the current background color before drawing text. |
| `M_TRANSPARENT` | Specifies not to change the background before drawing text. This creates a transparent background for printed characters. |

---

### `M_FONT`

Sets the font of the characters in the text.  Note that the non-bitmap fonts will use TrueType and Unicode features to draw text. This allows you to draw text using different sizes and TrueType fonts installed on your computer. This also allows you to draw any Unicode text (depending on the font).

| Value | Description |
| --- | --- |
| `M_FONT_DEFAULT` | Same as [`M_FONT_DEFAULT_SMALL`](../../Reference/gra/MgraFont.md). |
| `M_FONT_DEFAULT_LARGE` | Specifies a large bitmap font, where each character is drawn in a 16x32 pixel area. |
| `M_FONT_DEFAULT_MEDIUM` | Specifies a medium bitmap font, where each character is drawn in a 12x24 pixel area. |
| `M_FONT_DEFAULT_SMALL` | Specifies a small bitmap font, where each character is drawn in a 8x16 pixel area. |
| `M_FONT_DEFAULT_TTF` | Specifies the default TrueType font of your operating system. |
| `"FontFamily:Weight:Slant"` | Specifies the font according to the following format, _Family_:_Weight_:_Slant_.  _Family_ must be set to the name of the font's family, such as Arial, Times New Roman, and Wingdings.  _Weight_ can be set to one of the following: Thin, ExtraLight, UltraLight, Light, Book, Normal, Regular, Medium, SemiBold, DemiBold, Bold, ExtraBold, UltraBold, Heavy, Black, ExtraBlack, or UltraBlack.  _Slant_ can be set to one of the following: Italic, Oblique, or Roman.  You can omit _Weight_ and _Slant_; also, you can provide the _Weight_ and _Slant_ in any order. |
| `"FontFile"` | Specifies a full path to a TrueType file name. |

---

### `M_FONT_AUTO_SELECT`

Sets whether Aurora Imaging Library should search for a suitable font to draw the text, if the currently selected font ([`M_FONT`](../../Reference/gra/MgraControlList.md)) is a TrueType font that does not support the character code. Aurora Imaging Library will first attempt to make its selection from already used fonts, and then from system fonts.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not search for a suitable font. An error is returned if the character code cannot be drawn using the current TrueType font. |
| `M_ENABLE` | Specifies that Aurora Imaging Library will search for a suitable font. |

---

### `M_FONT_SIZE`

Sets the font size, when using a TrueType font. When using a bitmap font, use [`M_FONT_X_SCALE`](../../Reference/gra/MgraControlList.md) and [`M_FONT_Y_SCALE`](../../Reference/gra/MgraControlList.md) to set the font size.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the text's font size, in points. |

---

### `M_FONT_X_SCALE`

Sets the font's horizontal scaling factor. This setting only applies to bitmap fonts. For TrueType fonts, use [`M_FONT_SIZE`](../../Reference/gra/MgraControlList.md) to set the font size.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the factor with which to multiply the width of the font characters. Note that using a font with a scale of 1.0 accelerates text drawing. |

---

### `M_FONT_Y_SCALE`

Sets the font's vertical scaling factor. This setting only applies to bitmap fonts. For TrueType fonts, use [`M_FONT_SIZE`](../../Reference/gra/MgraControlList.md) to set the font size.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the factor with which to multiply the height of the font characters. Note that using a font with a scale of 1.0 accelerates text drawing. |

---

### `M_TEXT_ALIGN_HORIZONTAL`

Sets the horizontal alignment of the text.  The alignment is relative to the point at which the text starts, as specified using [`MgraText`](../../Reference/gra/MgraText.md) or using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_POSITION_X`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies that the text is horizontally centered. |
| `M_LEFT` | Specifies that the text is left-aligned. |
| `M_RIGHT` | Specifies that the text is right-aligned. |

---

### `M_TEXT_ALIGN_VERTICAL`

Sets the vertical alignment of the text.  The alignment is relative to the point at which the text starts, as specified using [`MgraText`](../../Reference/gra/MgraText.md) or using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that the text is bottom-aligned. |
| `M_CENTER` | Specifies that the text is vertically centered. |
| `M_TOP` | Specifies that the text is top-aligned. |

---

### `M_TEXT_BORDER`

Sets borders around the text.  Note that the possible settings can be combined. For example, to draw a box around the text, use [`M_TOP`](../../Reference/gra/MgraControlList.md)+[`M_BOTTOM`](../../Reference/gra/MgraControlList.md)+[`M_LEFT`](../../Reference/gra/MgraControlList.md)+[`M_RIGHT`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that a line is drawn underneath the text. |
| `M_LEFT` | Specifies that a line is drawn to the left of the text. |
| `M_NONE` | Specifies that no border is drawn around the text. This setting cannot be combined with any other setting. |
| `M_RIGHT` | Specifies that a line is drawn to the right of the text. |
| `M_TOP` | Specifies that a line is drawn above the text. |

---

### `M_TEXT_DIRECTION`

Sets the direction to draw the text, when using a TrueType font.

| Value | Description |
| --- | --- |
| `M_LEFT_TO_RIGHT` | Specifies that text will be drawn from left to right. |
| `M_RIGHT_TO_LEFT` | Specifies that text will be drawn from right to left. |

### Combination Constants — For specifying the color band (for 16- or 32-bit color buffers)

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the color band.

| Value | Description |
| --- | --- |
| `M_BLUE` | Specifies the blue color band. |
| `M_GREEN` | Specifies the green color band. |
| `M_RED` | Specifies the red color band. |

### For changing the position and dimensional information of a graphic

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to change a graphic's characteristics that have been set using the [`Mgra...`](../../Reference/gra/MgraArc.md) function with which the graphic was added to the list. In this case, the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter can be set to one or all graphics. Unless otherwise specified, the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter should be set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

---

### `M_ANGLE`

Sets the angle of the graphic.

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the angle, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md). |

---

### `M_ANGLE_END`

Sets the angle at which to stop drawing the arc.

| Value | Description |
| --- | --- |
| `Value` | Specifies the end angle, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md). |

---

### `M_ANGLE_START`

Sets the angle at which to start drawing the arc.

| Value | Description |
| --- | --- |
| `Value` | Specifies the start angle, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md). |

---

### `M_ARC_STYLE`

Sets whether to draw an arc or a sector.

| Value | Description |
| --- | --- |
| `M_CONTOUR` | Specifies that the arc (the curve between the specified start and end angles) is drawn without lines extending from the center of the ellipse to the start and end points of the arc. |
| `M_SECTOR` | Specifies that a sector is drawn with lines extending from the center of the ellipse to the start and end points of the arc, unless the specified start and end angles form a closed curve. |

---

### `M_CONSTRAIN_ASPECT_RATIO`

Sets whether to force the width and height of a graphic to be equal. This affects both interactive and programmatic manipulations.  This setting applies to arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)) and rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the width and height of a graphic are not forced to be equal. |
| `1.0` | Specifies that the width and height of a graphic are forced to be equal. This is considered a 1:1 (square) aspect ratio. Changing this setting on a graphic that does not meet this condition will change the dimensions of the graphic accordingly. Any attempts to change one of the dimensions, either programmatically or interactively, will scale the other so that the condition (1:1) is met. |

---

### `M_FILLED`

Sets whether to fill the graphic.  This setting only applies to arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)), polygons ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md)), and rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not filled. |
| `M_TRUE` | Specifies that the graphic is filled. |

---

### `M_POSITION_TYPE`

Sets how to interpret [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) for a rectangle.

| Value | Description |
| --- | --- |
| `M_CENTER_AND_DIMENSION` | Specifies to interpret [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) as the rectangle's center. |
| `M_CORNER_AND_DIMENSION` | Specifies to interpret [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) as the rectangle's top-left corner. |

---

### `M_POSITION_X`

Sets the X-position of a graphic, or of one of its sub-elements (position-points).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-position. You can specify any positive or negative value. |

---

### `M_POSITION_Y`

Sets the Y-position of a graphic, or of one of its sub-elements (position-points).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-position. You can specify any positive or negative value. |

---

### `M_RADIUS_X`

Sets the radius of the arc in the X-direction.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the radius. |

---

### `M_RADIUS_Y`

Sets the radius of the arc in the Y-direction.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the radius. |

---

### `M_RECTANGLE_HEIGHT`

Sets the height of the rectangle.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height. |

---

### `M_RECTANGLE_WIDTH`

Sets the width of the rectangle.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width. |

### For performing geometric operations on graphics

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to perform geometric operations on graphics. Using one of the following operations on a graphic will change its current position and dimensional settings (the settings listed in the [`PositionDimensionSettings`](../../Reference/PositionDimensionSettings.md) table). Once an operation has occurred, the value specified cannot be inquired.  In this case, the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter can be set to one or all graphics. Unless otherwise specified, the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter should be set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

---

### `M_APPLY_SCALE`

Performs a uniform scaling operation on the graphic. A uniform scaling operation enlarges or shrinks graphics by a scale factor in both X- and Y-directions, relative to the point specified by [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md); the point specified by [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) remains the same after a uniform scaling operation. For example, if you specify a scale factor of one half to dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), each dot will have its distance from [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) reduced by one half.  This setting applies to all types of graphics except dot ([`MgraDot`](../../Reference/gra/MgraDot.md)) and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)).

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the scale factor. |

---

### `M_RESIZE_HEIGHT`

Increases or decreases the graphic's height. This does not affect the graphic's X- and Y-position ([`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) remains the same).  Aurora Imaging Library typically resizes the height on both sides of the graphic; therefore, the height is actually modified by twice the specified resize value. This does not occur for rectangle graphics whose position ([`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md)) is interpreted as its top-left corner ([`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraControlList.md)). In that case, Aurora Imaging Library only applies the resize value to the opposite side.

| Value | Description |
| --- | --- |
| `Value` | Specifies the value by which to resize the height. |

---

### `M_RESIZE_WIDTH`

Increases or decreases the graphic's width. This does not affect the graphic's X- and Y-position ([`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md) remains the same).  Aurora Imaging Library typically resizes the width on both sides of the graphic; therefore, the width is actually modified by twice the specified resize value. This does not occur for rectangle graphics whose position ([`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md)) is interpreted as its top-left corner ([`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraControlList.md)). In that case, Aurora Imaging Library only applies the resize value to the opposite side.

| Value | Description |
| --- | --- |
| `Value` | Specifies the value by which to resize the width. |

---

### `M_ROTATE`

Performs a rotation operation on the graphic, or one of its sub-elements. Graphics are rotated around the point specified with [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the angle of rotation, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md). The value specified will be added to the graphic's current angle ([`M_ANGLE`](../../Reference/gra/MgraControlList.md)). |

---

### `M_TRANSLATE_X`

Performs a horizontal translation of a graphic, or of one of its sub-elements.

| Value | Description |
| --- | --- |
| `Value` | Specifies the horizontal displacement. You can specify any positive or negative value. The value specified will be added to the graphic's current X-position ([`M_POSITION_X`](../../Reference/gra/MgraControlList.md)). |

---

### `M_TRANSLATE_Y`

Performs a vertical translation of a graphic, or of one of its sub-elements.

| Value | Description |
| --- | --- |
| `Value` | Specifies the vertical displacement. You can specify any positive or negative value. The value specified will be added to the graphic's current Y-position ([`M_POSITION_Y`](../../Reference/gra/MgraControlList.md)). |

### Combination Constants — For specifying that a rectangle should stay at the same location regardless of how Aurora Imaging Library interprets its position

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that a rectangle should stay at the same location in the image regardless of how Aurora Imaging Library interprets its reference position.

| Value | Description |
| --- | --- |
| `M_SAME_LOCATION` | Specifies that, when you set [`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md), Aurora Imaging Library also adjusts the rectangle's actual position in the image so it remains at the same location. |

### For deleting graphics (one or all) from the list

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to delete graphics from the 2D graphics list. The [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter can be set to one or all graphics and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter must be set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md).

---

### `M_DELETE`

Deletes the graphic (one or all), as specified by the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter setting. When a graphic is deleted, the indices of the 2D graphics list are reassigned; that is, index values greater than that of the removed graphic are reduced by one.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

### For specifying general keyboard key settings to interactively modify graphics

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to specify general settings for using keyboard keys to interactively modify graphics. In this case, set the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter to [`M_LIST`](../../Reference/gra/MgraControlList.md) and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md). These settings apply to the 2D graphics list itself.

---

### `M_ACTION_KEYS`

Sets whether the 2D graphics list allows you to interactively modify its graphics by pressing keys on the keyboard (keyboard events).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the 2D graphics list ignores keyboard events. |
| `M_ENABLE` | Specifies that the 2D graphics list allows keyboard events to interactively modify its graphics. In this case, you must still enable the 2D graphics list's interactivity ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md)). |

---

### `M_ACTION_MODIFIER_SPEED`

Set the additional keyboard key that you can press to select an alternate speed to rotate, resize, or translate interactively selected graphics with the keyboard. This affects [`M_ACTION_KEY_ROTATE_...`](../../Reference/gra/MgraControlList.md), [`M_ACTION_KEY_RESIZE_...`](../../Reference/gra/MgraControlList.md), and [`M_ACTION_KEY_TRANSLATE_...`](../../Reference/gra/MgraControlList.md)settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_KEY_ALT` | Specifies to use the Alt key to select an alternate speed. |
| `M_KEY_CTRL` *(default)* | Specifies to use the Ctrl key to select an alternate speed. |
| `M_KEY_SHIFT` | Specifies to use the Shift key to select an alternate speed. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_RESIZE_INCREMENT`

Sets the value with which to increase or decrease the width or height of the interactively selected graphic for a single key press. This affects the [`M_ACTION_KEY_RESIZE_...`](../../Reference/gra/MgraControlList.md) settings. Note that if the graphic is in world units, the increment is converted to world units using the calibration's average pixel size in the resizing direction.  To keep the graphic's position fixed, Aurora Imaging Library typically applies this increment value to both sides of the graphic. Therefore, the graphic's width or height is actually modified by twice the specified value. This does not occur for rectangle graphics whose position ([`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md)) is interpreted as its top-left corner ([`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraControlList.md)). In that case, Aurora Imaging Library only applies the increment to the opposite side.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_RESIZE_INCREMENT_ALTERNATE`

Sets an alternate value with which to increase or decrease the width or height of the interactively selected graphic for a single key press. [`M_ACTION_RESIZE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md) behaves like [`M_ACTION_RESIZE_INCREMENT`](../../Reference/gra/MgraControlList.md). Aurora Imaging Library uses [`M_ACTION_RESIZE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md) instead of [`M_ACTION_RESIZE_INCREMENT`](../../Reference/gra/MgraControlList.md) when you press the key specified for [`M_ACTION_MODIFIER_SPEED`](../../Reference/gra/MgraControlList.md) while you resize the graphic using the keyboard.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_ROTATE_INCREMENT`

Sets the angle with which to rotate the interactively selected graphic for a single key press. This affects the [`M_ACTION_KEY_ROTATE_...`](../../Reference/gra/MgraControlList.md) settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_ACTION_ROTATE_INCREMENT_ALTERNATE`

Sets an alternate angle with which to rotate the interactively selected graphic for a single key press. [`M_ACTION_ROTATE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md) behaves like [`M_ACTION_ROTATE_INCREMENT`](../../Reference/gra/MgraControlList.md). Aurora Imaging Library uses [`M_ACTION_ROTATE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md) instead of [`M_ACTION_ROTATE_INCREMENT`](../../Reference/gra/MgraControlList.md) when you press the key specified for [`M_ACTION_MODIFIER_SPEED`](../../Reference/gra/MgraControlList.md) while you rotate the graphic using the keyboard.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_ACTION_TRANSLATE_AXES`

Set the axes along which Aurora Imaging Library translates a graphic that is in world units, when the translation is done using the keyboard. This affects [`M_ACTION_TRANSLATE_INCREMENT`](../../Reference/gra/MgraControlList.md) and [`M_ACTION_TRANSLATE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` | Specifies to convert the translation increment to world units using the calibration's average pixel size in the translation direction, then apply it along the world axes. |
| `M_PIXEL` *(default)* | Specifies to apply the translation increment to the graphic position, along the image axes, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_TRANSLATE_INCREMENT`

Sets the value with which to translate (move) the interactively selected graphics for a single key press. This affects the [`M_ACTION_KEY_TRANSLATE_...`](../../Reference/gra/MgraControlList.md) settings.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). Note that if the graphic is in world units, the behavior depends on [`M_ACTION_TRANSLATE_AXES`](../../Reference/gra/MgraControlList.md). |

---

### `M_ACTION_TRANSLATE_INCREMENT_ALTERNATE`

Sets an alternate value with which to translate (move) the interactively selected graphics for a single key press. [`M_ACTION_TRANSLATE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md) behaves like [`M_ACTION_TRANSLATE_INCREMENT`](../../Reference/gra/MgraControlList.md). Aurora Imaging Library uses [`M_ACTION_TRANSLATE_INCREMENT_ALTERNATE`](../../Reference/gra/MgraControlList.md) instead of [`M_ACTION_TRANSLATE_INCREMENT`](../../Reference/gra/MgraControlList.md) when you press the key specified for [`M_ACTION_MODIFIER_SPEED`](../../Reference/gra/MgraControlList.md) while you move the graphic using the keyboard.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). Note that if the graphic is in world units, the increment depends on [`M_ACTION_TRANSLATE_AXES`](../../Reference/gra/MgraControlList.md). |

### For specifying explicit keyboard keys to interactively modify graphics

The following [`ControlType`](../../Reference/gra/MgraControlList.md) and corresponding [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter settings are used to specify explicit keyboard keys to interactively modify graphics. In this case, set the [`LabelOrIndex`](../../Reference/gra/MgraControlList.md) parameter to [`M_LIST`](../../Reference/gra/MgraControlList.md) and the [`SubIndex`](../../Reference/gra/MgraControlList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md). These settings apply to the 2D graphics list itself.

---

### `M_ACTION_KEY_CANCEL`

Sets the keyboard keys that you can press to cancel the interactive graphic modification you are currently performing. Aurora Imaging Library recommends using the Esc key. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  If you press the specified cancel key while you are interactively translating (moving), resizing, or rotating the graphic, Aurora Imaging Library returns the graphic to its initial state (before you started interacting with it). If you press the specified cancel key while interactively creating the graphic, Aurora Imaging Library removes from the display any part of the graphic you started to define, and also removes the graphic from the 2D graphics list (the graphic is typically added to the 2D graphics list before it is entirely defined). If you press the specified cancel key when there is no interaction to cancel, Aurora Imaging Library deselects all selected graphics.  [`M_ACTION_KEY_CANCEL`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_DELETE`

Sets the keyboard keys that you can press to delete the interactively selected graphics. Aurora Imaging Library recommends using the Delete key. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_DELETE`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_HEIGHT_DOWN`

Sets the keyboard keys that you can press to reduce the height of the interactively selected graphics. Aurora Imaging Library recommends using the Down Arrow key and the Shift key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_RESIZE_HEIGHT_DOWN`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_DOWN`](../../Reference/gra/MgraControlList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_HEIGHT_UP`

Sets the keyboard keys that you can press to increase the height of the interactively selected graphics. Aurora Imaging Library recommends using the Up Arrow key and the Shift key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_RESIZE_HEIGHT_UP`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_UP`](../../Reference/gra/MgraControlList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_WIDTH_DOWN`

Sets the keyboard keys that you can press to reduce the width of the interactively selected graphics. Aurora Imaging Library recommends using the Left Arrow key and the Shift key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_RESIZE_WIDTH_DOWN`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_LEFT`](../../Reference/gra/MgraControlList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_WIDTH_UP`

Sets the keyboard keys that you can press to increase the width of the interactively selected graphics. Aurora Imaging Library recommends using the Right Arrow key and the Shift key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_RESIZE_WIDTH_UP`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraControlList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROTATE_CLOCKWISE`

Sets the keyboard keys that you can press to rotate the interactively selected graphics clockwise. Aurora Imaging Library recommends using the Right Arrow key and the Alt key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_ROTATE_CLOCKWISE`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraControlList.md) + [`M_KEY_ALT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROTATE_COUNTER_CLOCKWISE`

Sets the keyboard keys that you can press to rotate the interactively selected graphics counter-clockwise. Aurora Imaging Library recommends using the Left Arrow key and the Alt key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_ROTATE_COUNTER_CLOCKWISE`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_LEFT`](../../Reference/gra/MgraControlList.md) + [`M_KEY_ALT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_DOWN`

Sets the keyboard keys that you can press to translate (move) the interactively selected graphics down. Aurora Imaging Library recommends specifying the Down Arrow key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_TRANSLATE_DOWN`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_DOWN`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_LEFT`

Sets the keyboard keys that you can press to translate (move) the interactively selected graphics left. Aurora Imaging Library recommends specifying the Left Arrow key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_TRANSLATE_LEFT`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_LEFT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_RIGHT`

Sets the keyboard keys that you can press to translate (move) the interactively selected graphics right. Aurora Imaging Library recommends specifying the Right Arrow key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_TRANSLATE_RIGHT`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_UP`

Sets the keyboard keys that you can press to translate (move) the interactively selected graphics up. Aurora Imaging Library recommends specifying the Up Arrow key, which is the default. If the keyboard key that you specify is used by another action setting ([`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md)), Aurora Imaging Library sets that other action setting to [`M_NONE`](../../Reference/gra/MgraControlList.md).  [`M_ACTION_KEY_TRANSLATE_UP`](../../Reference/gra/MgraControlList.md) only has an effect if you enable [`M_ACTION_KEYS`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ADD` | Specifies the Add key (+). |
| `M_KEY_ARROW_DOWN` | Specifies the Down Arrow key. |
| `M_KEY_ARROW_LEFT` | Specifies the Left Arrow key. |
| `M_KEY_ARROW_RIGHT` | Specifies the Right Arrow key. |
| `M_KEY_ARROW_UP` | Specifies the Up Arrow key. |
| `M_KEY_BACK` | Specifies the Back key. |
| `M_KEY_CLEAR` | Specifies the Clear key. |
| `M_KEY_DECIMAL` | Specifies the Period key located on your number pad. |
| `M_KEY_DELETE` | Specifies the Delete key. |
| `M_KEY_DIVIDE` | Specifies the Divide key (/). |
| `M_KEY_END` | Specifies the End key. |
| `M_KEY_ESC` | Specifies the Esc key. |
| `M_KEY_EXECUTE` | Specifies the Execute key. |
| `M_KEY_Fn` | Specifies the_n_key, where _n_ is a value from 1 to 24. |
| `M_KEY_HOME` | Specifies the Home key. |
| `M_KEY_INSERT` | Specifies the Insert key. |
| `M_KEY_MULTIPLY` | Specifies the Multiply key (*). |
| `M_KEY_n` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_NUM_LOCK` | Specifies the Num Lock key. |
| `M_KEY_NUMPADn` | Specifies the_n_key, where _n_ is a value from 0 to 9. |
| `M_KEY_PAGEDOWN` | Specifies the Pagedown key. |
| `M_KEY_PAGEUP` | Specifies the Pageup key. |
| `M_KEY_PAUSE` | Specifies the Pause key. |
| `M_KEY_RETURN` | Specifies the Return key (Enter). |
| `M_KEY_SCROLL_LOCK` | Specifies the Scroll Lock key. |
| `M_KEY_SEPARATOR` | Specifies the Separator key. |
| `M_KEY_SPACE` | Specifies the Space key. |
| `M_KEY_SUBTRACT` | Specifies the Subtract key (-). |
| `M_KEY_TAB` | Specifies the Tab key. |
| `M_KEY_xxx` | Specifies the letter key_xxx_, where_xxx_ can be a letter from A to Z. |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_UP`](../../Reference/gra/MgraControlList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

### Combination Constants — For specifying additional keyboard keys

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify an additional keyboard key.

For example, to specify that you must press the Right Arrow key while pressing the Alt key to rotate the interactively selected graphic clockwise, set the [`ControlType`](../../Reference/gra/MgraControlList.md) parameter to [`M_ACTION_KEY_ROTATE_CLOCKWISE`](../../Reference/gra/MgraControlList.md), and set the [`ControlValue`](../../Reference/gra/MgraControlList.md) parameter to [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraControlList.md) + [`M_KEY_ALT`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_KEY_ALT` | Specifies the Alt key. |
| `M_KEY_CTRL` | Specifies the Ctrl key. |
| `M_KEY_SHIFT` | Specifies the Shift key. |

This setting applies to all types of graphics except dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), text ([`MgraText`](../../Reference/gra/MgraText.md)), sets of finite length lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). Enabling this setting when specifying an unsupported graphic type will generate an error.

This setting applies to all types of graphics except dot ([`MgraDot`](../../Reference/gra/MgraDot.md)), text ([`MgraText`](../../Reference/gra/MgraText.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)).

If [`SubIndex`](../../Reference/gra/MgraControlList.md) is set to [`M_DEFAULT`](../../Reference/gra/MgraControlList.md), this setting affects the entire graphic. If a sub-element is specified, a single point, within the graphic, is affected by this setting.

This setting applies to all types of graphics except a dot ([`MgraDot`](../../Reference/gra/MgraDot.md)), dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), text ([`MgraText`](../../Reference/gra/MgraText.md)), sets of finite length lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). Enabling this setting when specifying an unsupported graphic type will generate an error.

This setting applies to all types of graphics.

This setting applies to all types of graphics except drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)).

This setting only applies to arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)).

This setting only applies to rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)).

An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).
