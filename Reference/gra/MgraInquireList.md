---
doctype: Reference
module: gra
function: MgraInquireList
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraInquireList"
---

# MgraInquireList

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

> Inquire information about a 2D graphics list or a graphic contained within the 2D graphics list.

## Syntax

```c
AIL_INT MgraInquireList(
    AIL_ID    GraListId,     //in
    AIL_INT   LabelOrIndex,  //in
    AIL_INT   SubIndex,      //in
    AIL_INT64 InquireType,   //in
    void *    UserVarPtr     //out
)
```

## Description

This function inquires information about a specified 2D graphics list, or a graphic contained within the 2D graphics list.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`GraListId`](../../Reference/gra/MgraInquireList.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `GraListId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics list about which to inquire information. The 2D graphics list must have been previously allocated on the required system using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the graphic about which to inquire, or specifies to inquire about the 2D graphics list itself. This parameter should be set to one of the following values:

*For specifying the graphic or the 2D graphics list*

| Value | Description |
| --- | --- |
| `M_GRAPHIC_INDEX` | Specifies the index of an existing graphic about which to inquire. |
| `M_GRAPHIC_LABEL` | Specifies the label of an existing graphic about which to inquire. |
| `M_LIST` | Inquires information about the 2D graphics list itself. |

### `SubIndex` *(in, AIL_INT)*

Specifies the index of the sub-element of the graphic about which to inquire. If this information is not required or supported, set this parameter to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

*For specifying the index of a graphic's sub-element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to inquire about the graphic itself (instead of just a sub-element). |
| `0 <= Value <= M_NUMBER_OF_SUB_ELEMENTS` | Specifies the index of the sub-element of the graphic about which to inquire. The following table lists all the graphics for which you can inquire information about their individual sub-elements and outlines how sub-indices are assigned for each of these.

| Graphics types | Index of sub-element values |
| --- | --- |
| Dots (created using [`MgraDots`](../../Reference/gra/MgraDots.md)). | The indices, starting from 0, are assigned to each dot in order of creation. |
| A single line (created using [`MgraLine`](../../Reference/gra/MgraLine.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_LINE`](../../Reference/gra/MgraInteractive.md)). | 0 is assigned to the specified start point, and 1 is assigned to the specified end point of the line. |
| Sets of finite length lines (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)). | The indices, starting from 0, are assigned to the specified start point and end point of each line, in order of creation. For example, the start point of the first line has an index of 0 and the end point has an index of 1, and the start point of the second line has an index of 2 and the end point has an index of 3. |
| Sets of infinite lines (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_INFINITE_LINES`](../../Reference/gra/MgraLines.md)). | The indices, starting from 0, are assigned to the first and second specified point of each line, in order of creation. For example, the first specified point of the first line has an index of 0 and its second specified point has an index of 1, and the first specified point of the second line has an index of 2 and its second specified point has an index of 3. |
| A polygon (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/gra/MgraInteractive.md)). | The indices, starting from 0, are assigned to each polygon vertex in order of creation. |
| A polyline (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYLINE`](../../Reference/gra/MgraLines.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_POLYLINE`](../../Reference/gra/MgraInteractive.md)). | The indices, starting from 0, are assigned to each polyline point in order of creation. | |

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MgraInquireList`](../../Reference/gra/MgraInquireList.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a setting of a 2D graphics list itself

To inquire about a setting of a 2D graphics list, the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. In this case, you must set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_LIST`](../../Reference/gra/MgraInquireList.md) and the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

---

### `M_ANGLE_SNAPPING_VALUE`

Inquires the value to use as a multiple when rotating the graphic. This value only applies when using [`M_MODE_ROTATE`](../../Reference/gra/MgraInquireList.md) with [`M_ANGLE_SNAPPING`](../../Reference/gra/MgraInquireList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1.0 <= Value <= 180.0` *(default)* | Specifies the value to use as a multiple, in degrees. |

---

### `M_EASY_SELECTION`

Inquires whether Aurora Imaging Library allows for the easy selection of graphics on the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not allow for the easy selection of graphics on the display. |
| `M_ENABLE` | Specifies that Aurora Imaging Library allows for the easy selection of graphics on the display. |

---

### `M_INTERACTIVE_ANNOTATIONS_COLOR`

Inquires the color of the selection box and handles when in interactive mode.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. |
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

### `M_INTERACTIVE_GRAPHIC_STATE`

Inquires the current state of interactivity of the 2D graphics list.

| Value | Description |
| --- | --- |
| *(see [`M_INTERACTIVE_GRAPHIC_STATE`](Reference/gra/MgraGetHookInfo.md))* |  |

---

### `M_LAST_LABEL`

Inquires the label that was automatically assigned to the last graphic added to the list in the current thread. This label value will never change, even if the order of the graphics in the list changes.  After adding a graphic to the 2D graphics list, you should use this inquire type to establish the label assigned to it. This label can be used as a means of identifying the graphic independently from its index, which might change if a graphic is deleted from the list.  > **Note:** You cannot retrieve the label value of the last added graphic if it has been deleted or moved to another 2D graphics list. If a graphic was added to the list from a different thread, the value returned will be that of the last graphic added to the list from the current thread; it will not return the label value of graphics added from a separate thread. This can be problematic when using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) to add graphics to the list interactively, since the graphics might be added from a separate thread. In this case, use [`MgraGetHookInfo`](../../Reference/gra/MgraGetHookInfo.md) with [`M_GRAPHIC_LABEL_VALUE`](../../Reference/gra/MgraGetHookInfo.md). See [Creating and modifying graphics interactively](../../UserGuide/C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md) for an example of how to inquire the label of a graphic added interactively.

| Value | Description |
| --- | --- |
| `M_NO_LABEL` | Specifies the last-added graphic is no longer in the list or no graphics have been added to the list. |
| `Value` | Specifies the label of the last-added graphic in the list. |

---

### `M_MODE_RESIZE`

Inquires the interactive behavior when resizing the graphic using its selection box.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FIXED_CENTER` | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic. |
| `M_FIXED_CORNER` *(default)* | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic. |

---

### `M_MODE_RESIZE_ALT`

Inquires the interactive behavior when resizing the graphic using its selection box and pressing the Alt key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Alt key does not affect how to resize the graphic. |
| `M_FIXED_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic remains constant, when resizing the graphic while pressing the Alt key. |
| `M_FIXED_CENTER` | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic while pressing the Alt key. |
| `M_FIXED_CORNER` | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic while pressing the Alt key. |
| `M_NO_CONSTRAINT` | Specifies that the aspect ratio of the graphic can change, when resizing it while pressing the Alt key. |
| `M_SQUARE_ASPECT_RATIO` *(default)* | Specifies that the aspect ratio of the graphic will be 1:1 (square), when resizing it while pressing the Alt key. |

---

### `M_MODE_RESIZE_CTRL`

Inquires the interactive behavior when resizing the graphic using its selection box and pressing the Ctrl key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Ctrl key does not affect how to resize the graphic. |
| `M_FIXED_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic remains constant, when resizing the graphic while pressing the Ctrl key. |
| `M_FIXED_CENTER` *(default)* | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic while pressing the Ctrl key. |
| `M_FIXED_CORNER` | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic while pressing the Ctrl key. |
| `M_NO_CONSTRAINT` | Specifies that the aspect ratio of the graphic can change, when resizing it while pressing the Ctrl key. |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic will be 1:1 (square), when resizing it while pressing the Ctrl key. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION`

Inquires the interactive behavior when changing the graphic's secondary dimension during its creation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ONE_SIDED` *(default)* | Specifies to establish the graphic's secondary dimension on one side of a previously established part. |
| `M_SYMMETRIC` | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION_ALT`

Inquires the interactive behavior when changing the graphic's secondary dimension during its creation, and pressing the Alt key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that pressing the Alt key does not affect how to establish the graphic's secondary dimension when defining it interactively on the display. |
| `M_ONE_SIDED` | Specifies to establish the graphic's secondary dimension on one side of a previously established part, while pressing the Alt key. |
| `M_SYMMETRIC` | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part, while pressing the Alt key. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION_CTRL`

Inquires the interactive behavior when changing the graphic's secondary dimension during its creation, and pressing the Ctrl key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Ctrl key does not affect how to establish the graphic's secondary dimension when defining it interactively on the display. |
| `M_ONE_SIDED` | Specifies to establish the graphic's secondary dimension on one side of a previously established part, while pressing the Ctrl key. |
| `M_SYMMETRIC` *(default)* | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part, while pressing the Ctrl key. |

---

### `M_MODE_RESIZE_SECONDARY_DIMENSION_SHIFT`

Inquires the interactive behavior when changing the graphic's secondary dimension during its creation, and pressing the Shift key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that pressing the Shift key does not affect how to establish the graphic's secondary dimension when defining it interactively on the display. |
| `M_ONE_SIDED` | Specifies to establish the graphic's secondary dimension on one side of a previously established part, while pressing the Shift key. |
| `M_SYMMETRIC` | Specifies to establish the graphic's secondary dimension symmetrically on both sides of a previously established part, while pressing the Shift key. |

---

### `M_MODE_RESIZE_SHIFT`

Inquires the interactive behavior when resizing the graphic using its selection box and pressing the Shift key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that pressing the Shift key does not affect how to resize the graphic. |
| `M_FIXED_ASPECT_RATIO` *(default)* | Specifies that the aspect ratio of the graphic remains constant, when resizing the graphic while pressing the Shift key. |
| `M_FIXED_CENTER` | Specifies that all sides of the selection box move symmetrically, and the center does not move, when resizing the graphic while pressing the Shift key. |
| `M_FIXED_CORNER` | Specifies that the center moves, and the opposite corner does not move, when resizing the graphic while pressing the Shift key. |
| `M_NO_CONSTRAINT` | Specifies that the aspect ratio of the graphic can change, when resizing it while pressing the Shift key. |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic will be 1:1 (square), when resizing it while pressing the Shift key. |

---

### `M_MODE_ROTATE`

Inquires the interactive behavior when rotating the graphic using its selection box.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraInquireList.md). |
| `M_NO_CONSTRAINT` *(default)* | Specifies to rotate the graphic freely. |

---

### `M_MODE_ROTATE_ALT`

Inquires the interactive behavior when rotating the graphic using its selection box and pressing the Alt key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` *(default)* | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraInquireList.md), when rotating it while pressing the Alt key. |
| `M_DISABLE` | Specifies that pressing the Alt key does not affect how to rotate the graphic. |
| `M_NO_CONSTRAINT` | Specifies to rotate the graphic freely, when rotating it while pressing the Alt key. |

---

### `M_MODE_ROTATE_CTRL`

Inquires the interactive behavior when rotating the graphic using its selection box and pressing the Ctrl key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraInquireList.md), when rotating it while pressing the Ctrl key. |
| `M_DISABLE` *(default)* | Specifies that pressing the Ctrl key does not affect how to rotate the graphic. |
| `M_NO_CONSTRAINT` | Specifies to rotate the graphic freely, when rotating it while pressing the Ctrl key. |

---

### `M_MODE_ROTATE_SHIFT`

Inquires the interactive behavior when rotating the graphic using its selection box and pressing the Shift key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANGLE_SNAPPING` | Specifies to rotate the graphic by snapping it to an angle that is a multiple of the value set by [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraInquireList.md), when rotating it while pressing the Shift key. |
| `M_DISABLE` *(default)* | Specifies that pressing the Shift key does not affect how to rotate the graphic. |
| `M_NO_CONSTRAINT` | Specifies to rotate the graphic freely, when rotating it while pressing the Shift key. |

---

### `M_MODE_TRANSLATE`

Inquires the interactive behavior when moving (translating) the graphic.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` | Specifies to move the graphic along the X- and Y-axes. |
| `M_NO_CONSTRAINT` *(default)* | Specifies to move the graphic freely. |

---

### `M_MODE_TRANSLATE_ALT`

Inquires the interactive behavior when moving (translating) the graphic and pressing the Alt key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` | Specifies to move the graphic along the X- and Y-axes, when moving it while pressing the Alt key. |
| `M_DISABLE` *(default)* | Specifies that pressing the Alt key does not affect how to move the graphic. |
| `M_NO_CONSTRAINT` | Specifies to move the graphic freely, when moving it while pressing the Alt key. |

---

### `M_MODE_TRANSLATE_CTRL`

Inquires the interactive behavior when moving (translating) the graphic and pressing the Ctrl key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` | Specifies to move the graphic along the X- and Y-axes, when moving it while pressing the Ctrl key. |
| `M_DISABLE` *(default)* | Specifies that pressing the Ctrl key does not affect how to move the graphic. |
| `M_NO_CONSTRAINT` | Specifies to move the graphic freely, when moving it while pressing the Ctrl key. |

---

### `M_MODE_TRANSLATE_SHIFT`

Inquires the interactive behavior when moving (translating) the graphic and pressing the Shift key.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AXIS_ALIGNED` *(default)* | Specifies to move the graphic along the X- and Y-axes, when moving it while pressing the Shift key. |
| `M_DISABLE` | Specifies that pressing the Shift key does not affect how to move the graphic. |
| `M_NO_CONSTRAINT` | Specifies to move the graphic freely, when moving it while pressing the Shift key. |

---

### `M_MULTIPLE_SELECTION`

Inquires whether interactive multiple selection is permitted.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that interactive multiple selection is not permitted. |
| `M_ENABLE` *(default)* | Specifies that interactive multiple selection is permitted. |

---

### `M_MULTIPLE_SELECTION_KEY`

Inquires the keyboard key that you can press to select multiple graphics with your mouse on the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_KEY_ALT` | Specifies that you can press the Alt key to select (or deselect) multiple graphics. |
| `M_KEY_CTRL` *(default)* | Specifies that you can press the Ctrl key to select (or deselect) multiple graphics. |
| `M_KEY_SHIFT` | Specifies that you can press the Shift key to select (or deselect) multiple graphics. |

---

### `M_NUMBER_OF_GRAPHICS`

Inquires the number of graphics in the list.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of graphics. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the 2D graphics list was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_OWNER_SYSTEM_TYPE`

Inquires the type of system on which the 2D graphics list was allocated.

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

---

### `M_SELECTED_COLOR`

Inquires the color of the selected graphics in interactive mode.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. |
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

### `M_SELECTION_RADIUS`

Inquires the size of the selection-radius when in interactive mode.

| Value | Description |
| --- | --- |
| `Value >= 1.0` *(default)* | Specifies the size of the selection-radius, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

### Combination Constants — For inquiring the aspect ratio when resizing the graphic

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the aspect ratio used when resizing the graphic.

The following values will not modify the graphic's aspect ratio if it is set to 1 using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_CONSTRAIN_ASPECT_RATIO`](../../Reference/gra/MgraInquireList.md).

| Value | Description |
| --- | --- |
| `M_FIXED_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic remains constant when resizing it. |
| `M_NO_CONSTRAINT` *(default)* | Specifies that the aspect ratio of the graphic can change when resizing it. |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the aspect ratio of the graphic will be 1:1 (square) when resizing it. |

### For inquiring about a general graphic setting

To inquire about a general graphic setting, the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. You can set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to the label or index of a graphic in the list, and the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md). Unless otherwise specified, these affect all types of graphics.

---

### `M_COLOR`

Inquires the foreground color of the graphic.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. |
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
| *(see [`M_DRAW_DIRECTION`](Reference/gra/MgraControlList.md))* |  |

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
| `Value > 0` *(default)* | Specifies the scaling factor in the X-direction. |

---

### `M_DRAW_ZOOM_Y`

Inquires the scale factor in the Y-direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scaling factor in the Y-direction. |

---

### `M_FIXTURE`

Inquires the camera calibration information used when rendering (drawing or displaying) a graphic defined in world units, when both source camera calibration information and destination camera calibration information are available to interpret positioning and dimensioning information.  If with respect to that of the source camera calibration information ([`M_USE_SOURCE_FIRST`](../../Reference/gra/MgraControlList.md)), positions and dimensions are transformed from this relative coordinate system to the absolute coordinate system; then, since it is assumed that there is only one absolute coordinate system, these positions and dimensions are transformed from the absolute coordinate system to the pixel coordinate system, using the destination camera calibration information.

| Value | Description |
| --- | --- |
| `M_USE_DESTINATION_FIRST` | Specifies that the camera calibration information of the destination is used when rendering the graphic. |
| `M_USE_SOURCE_FIRST` | Specifies that the camera calibration information of the graphic, set with [`M_GRAPHIC_SOURCE_CALIBRATION`](../../Reference/gra/MgraInquireList.md), is used when rendering the graphic. |

---

### `M_GRAPHIC_CONVERSION_MODE`

Inquires how the shape of a graphic, defined in world units, is converted to pixels (rendered).

| Value | Description |
| --- | --- |
| `M_PRESERVE_SHAPE_AVERAGE` | Specifies to render the graphic so that its shape is preserved even if it means not respecting the camera calibration information exactly. |
| `M_RESHAPE_FOLLOWING_DISTORTION` | Specifies that all points along the contour of the graphic will be converted using the camera calibration information, following any non-linear distortion. |
| `M_RESHAPE_FROM_POINTS` | Specifies that only a few key points or features will be converted using the camera calibration information; from these points, the rest of the graphic will be rendered respecting the shape of the graphic. |

---

### `M_GRAPHIC_SOURCE_CALIBRATION`

Inquires the identifier of the camera calibration information used to interpret positioning and dimensioning information of a graphic defined in world units.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no source camera calibration information is available to interpret positioning and dimensioning information of a graphic defined in world units. |
| `Aurora Imaging Library identifier` | Specifies the identifier of a camera calibration context, image buffer or processing or analysis module result buffer, whose camera calibration information to use. |
| `Aurora Imaging Library Identifier` | Specifies the identifier of the internal camera calibration context that will be used. |

---

### `M_GRAPHIC_TYPE`

Inquires the type of the graphic.

| Value | Description |
| --- | --- |
| `M_GRAPHIC_TYPE_ARC` | Specifies an arc (created using [`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md), or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md)). |
| `M_GRAPHIC_TYPE_COLLECTION` | Specifies a drawing created using the [`M...Draw`](../../Reference/3dmap/M3dmapDraw.md) function of a processing or analysis module, like [`MmodDraw`](../../Reference/mod/MmodDraw.md) or [`MmeasDraw`](../../Reference/meas/MmeasDraw.md). |
| `M_GRAPHIC_TYPE_DOT` | Specifies a dot (created using [`MgraDot`](../../Reference/gra/MgraDot.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_DOT`](../../Reference/gra/MgraInteractive.md)). |
| `M_GRAPHIC_TYPE_DOTS` | Specifies dots (created using [`MgraDots`](../../Reference/gra/MgraDots.md)). |
| `M_GRAPHIC_TYPE_INFINITE_LINES` | Specifies infinite lines (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_INFINITE_LINES`](../../Reference/gra/MgraLines.md)). |
| `M_GRAPHIC_TYPE_LINE` | Specifies a line (created using [`MgraLine`](../../Reference/gra/MgraLine.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_LINE`](../../Reference/gra/MgraInteractive.md)). |
| `M_GRAPHIC_TYPE_LINES` | Specifies lines (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)). |
| `M_GRAPHIC_TYPE_POLYGON` | Specifies a polygon (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_POLYGON`](../../Reference/gra/MgraInteractive.md)). |
| `M_GRAPHIC_TYPE_POLYLINE` | Specifies a polyline (created using [`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYLINE`](../../Reference/gra/MgraLines.md) or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_POLYLINE`](../../Reference/gra/MgraInteractive.md)). |
| `M_GRAPHIC_TYPE_RECT` | Specifies a rectangle (created using [`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md), or user-defined using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md)). |
| `M_GRAPHIC_TYPE_TEXT` | Specifies text (created using [`MgraText`](../../Reference/gra/MgraText.md)). |

---

### `M_INDEX_VALUE`

Inquires the index associated with the graphic. In this case, the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter must be set to the label of the required graphic.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the graphic label is invalid. This is returned if the specified label is not associated with a graphic. |
| `Value >= 0` | Specifies the index value associated with the specified graphic. |

---

### `M_INPUT_UNITS`

Inquires the units with which to interpret the graphic's position and dimensional information, for graphics already added to the 2D graphics list.

| Value | Description |
| --- | --- |
| `M_DISPLAY` | Specifies to interpret the values in pixel units that, unlike [`M_PIXEL`](../../Reference/gra/MgraInquireList.md), are not altered when the display is panned or zoomed. |
| `M_PIXEL` | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_LABEL_VALUE`

Inquires the label that was automatically associated with the graphic when it was added to the list. This label value will never change, even if the order of the graphics in the list changes.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label value that was automatically associated with the specified graphic. |

---

### `M_LINE_THICKNESS`

Inquires the thickness of the lines and diameter of the dots.

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the thickness of the lines and diameter of the dots, in pixels. |

---

### `M_NUMBER_OF_SUB_ELEMENTS`

Inquires the number of sub-elements (position-points) within the specified graphic. Note that in this case the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter must be set to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of sub-elements. This value can only be greater than zero for the following graphics: dots, line, lines, polylines, infinite lines, and polygons. For example, a line contains two sub-elements (position points). |

### For inquiring about a setting related to interactivity

To inquire 2D graphics context settings that affect the interactivity of a graphic, drawn in a 2D graphics list associated with a display when interactive mode is enabled ([`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md) set to [`M_ENABLE`](../../Reference/disp/MdispControl.md)), the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. You can set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to the label or index of a specific graphic, and the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter must be set to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

---

### `M_EDITABLE`

Inquires whether a graphic can be edited via user interaction in an interactive display. If a graphic is not visible or if it is not selectable, it is not editable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be edited via user interaction. |
| `M_ENABLE` | Specifies that the graphic can be edited via user interaction. |

---

### `M_GRAPHIC_SELECTED`

Inquires whether the graphic is selected.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not selected. |
| `M_TRUE` | Specifies that the graphic is selected. |

---

### `M_RESIZABLE`

Inquires whether a graphic can be resized via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not resizeable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be resized via user interaction and the resize handles will not be displayed if a graphic is selected. |
| `M_ENABLE` | Specifies that the graphic can be resized via user interaction by clicking and dragging one of the resize handle. |

---

### `M_ROTATABLE`

Inquires whether a graphic can be rotated via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not rotatable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be rotated via user interaction and the rotate handle will not be displayed if a graphic is selected. |
| `M_ENABLE` | Specifies that the graphic can be rotated via user interaction by clicking and dragging the rotation handle. |

---

### `M_SELECTABLE`

Inquires whether a graphic in a 2D graphics list can be selected via user interaction in an interactive display. If a graphic is not visible, it is not selectable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be selected via user interaction. |
| `M_ENABLE` | Specifies that the graphic can be selected by clicking on it. |

---

### `M_SPECIFIC_FEATURES_EDITABLE`

Inquires whether a graphic can be modified via user interaction in an interactive display using handles that are specific to its graphic type. If a graphic is not visible, not selectable, or not editable, it cannot be modified using its type-specific handles and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be modified via user interaction and its specific feature handles will not be displayed if a graphic is selected. |
| `M_ENABLE` | Specifies that the graphic can be modified via user interaction by clicking and dragging one of the type-specific handles. |

---

### `M_TRANSLATABLE`

Inquires whether a graphic can be moved (translated) via user interaction in an interactive display. If a graphic is not visible, not selectable, or not editable, it is not movable and this setting is ignored.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the graphic cannot be moved via user interaction. |
| `M_ENABLE` | Specifies that the graphic can be moved via user interaction by clicking and dragging the graphic, its selection box, or its center handle. |

---

### `M_VISIBLE`

Inquires whether a graphic is rendered on the display.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not rendered. |
| `M_TRUE` | Specifies that the graphic is rendered. |

### For inquiring about a setting specific to text

To inquire about a setting specific to text in the 2D graphics list, the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. You must set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to the label or index of text, and the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md). Inquiring one of the following settings for graphics that are not text will generate an error.

---

### `M_BACKCOLOR`

Inquires the background color of the text.

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value when drawing in an 8-bit, 3-band buffer. |
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
| `M_OPAQUE` | Specifies that the background will be filled with the current background color before drawing text. |
| `M_TRANSPARENT` | Specifies not to change the background before drawing text. |

---

### `M_FONT`

Inquires the font of the characters in the text.

| Value | Description |
| --- | --- |
| `M_FONT_DEFAULT` | Same as [`M_FONT_DEFAULT_SMALL`](../../Reference/gra/MgraFont.md). |
| `M_FONT_DEFAULT_LARGE` | Specifies a large bitmap font, where each character is drawn in a 16x32 pixel area. |
| `M_FONT_DEFAULT_MEDIUM` | Specifies a medium bitmap font, where each character is drawn in a 12x24 pixel area. |
| `M_FONT_DEFAULT_SMALL` | Specifies a small bitmap font, where each character is drawn in a 8x16 pixel area. |
| `M_FONT_DEFAULT_TTF` | Specifies the default TrueType font of your operating system. |
| `"FontFamily:Weight:Slant"` | Specifies the font according to the following format, _Family_:_Weight_:_Slant_. |
| `"FontFile"` | Specifies a full path to a TrueType file name. |
| `M_FONT_TTF` | Specifies a TrueType font. This value is returned when using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_FONT`](../../Reference/gra/MgraControlList.md). |

---

### `M_FONT_AUTO_SELECT`

Inquires whether Aurora Imaging Library will search for a suitable font to draw text if the currently selected font is a TrueType font that does not support the character code.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library will not search for a suitable font. |
| `M_ENABLE` | Specifies that Aurora Imaging Library will search for a suitable font. |

---

### `M_FONT_SIZE`

Inquires the size in which text is drawn for a TrueType font.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the text's font size, in points. |

---

### `M_FONT_X_SCALE`

Inquires the bitmap font's horizontal scaling factor.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the factor with which to multiply the width of the font characters. |

---

### `M_FONT_Y_SCALE`

Inquires the bitmap font's vertical scaling factor.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the factor with which to multiply the height of the font characters. |

---

### `M_TEXT_ALIGN_HORIZONTAL`

Inquires the horizontal alignment of the text.

| Value | Description |
| --- | --- |
| `M_CENTER` | Specifies that the text is horizontally centered. |
| `M_LEFT` | Specifies that the text is left-aligned. |
| `M_RIGHT` | Specifies that the text is right-aligned. |

---

### `M_TEXT_ALIGN_VERTICAL`

Inquires the vertical alignment of the text.

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that the text is bottom-aligned. |
| `M_CENTER` | Specifies that the text is vertically centered. |
| `M_TOP` | Specifies that the text is top-aligned. |

---

### `M_TEXT_BORDER`

Inquires how borders are drawn around the text.  Note that a combination of the values below can be returned. Bitwise operators must be used to verify the presence of a specific border.

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies that a line is drawn underneath the text. |
| `M_LEFT` | Specifies that a line is drawn to the left of the text. |
| `M_NONE` | Specifies that no border is drawn around the text. |
| `M_RIGHT` | Specifies that a line is drawn to the right of the text. |
| `M_TOP` | Specifies that a line is drawn above the text. |

---

### `M_TEXT_DIRECTION`

Inquires the direction text is drawn when using a TrueType font.

| Value | Description |
| --- | --- |
| `M_LEFT_TO_RIGHT` | Specifies that text will be drawn from left to right. |
| `M_RIGHT_TO_LEFT` | Specifies that text will be drawn from right to left. |

### Combination Constants — For inquiring the color value used (for 16- or 32-bit color buffers)

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the color used for a 16-bit or 32-bit color buffer.

You must inquire each color band (R,G, and B) separately.

| Value | Description |
| --- | --- |
| `M_BLUE` | Inquires the blue color band. |
| `M_GREEN` | Inquires the green color band. |
| `M_RED` | Inquires the red color band. |

### For inquiring about a setting regarding the position and dimensional information of a graphic

To inquire about the position and dimensional information settings of a graphic (typically set when the graphic is added to the list), the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. You must set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to the label or index of a graphic that supports that setting. Unless otherwise specified, the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter should be set to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

---

### `M_ANGLE`

Inquires the angle of the graphic.  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).  You can inquire this value for all graphics except a single dot ([`MgraDot`](../../Reference/gra/MgraDot.md)), multiple dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), text ([`MgraText`](../../Reference/gra/MgraText.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). Inquiring this value for any of the unsupported types of graphics will generate an error.

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 360.0` | Specifies the angle, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraInquireList.md). |

---

### `M_ANGLE_END`

Inquires the angle at which to stop drawing the arc.

| Value | Description |
| --- | --- |
| `Value` | Specifies the end angle, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraInquireList.md). |

---

### `M_ANGLE_START`

Inquires the angle at which to start drawing the arc.

| Value | Description |
| --- | --- |
| `Value` | Specifies the start angle, in degrees, relative to the input coordinate system specified using [`M_INPUT_UNITS`](../../Reference/gra/MgraInquireList.md). |

---

### `M_ARC_STYLE`

Inquires whether to draw an arc or a sector.

| Value | Description |
| --- | --- |
| `M_CONTOUR` | Specifies that the arc (the curve between the specified start and end angles) is drawn without lines extending from the center of the ellipse to the start and end points of the arc. |
| `M_SECTOR` | Specifies that a sector is drawn with lines extending from the center of the ellipse to the start and end points of the arc, unless the specified start and end angles form a closed curve. |

---

### `M_CENTER_X`

Inquires the X-coordinate of a graphic's center.  Except for rectangles, [`M_CENTER_X`](../../Reference/gra/MgraInquireList.md) returns the same value as [`M_POSITION_X`](../../Reference/gra/MgraInquireList.md). For rectangles, [`M_CENTER_X`](../../Reference/gra/MgraInquireList.md) and [`M_POSITION_X`](../../Reference/gra/MgraInquireList.md) can have different values, depending on how Aurora Imaging Library interprets the rectangle's position (which is established with [`M_POSITION_TYPE`](../../Reference/gra/MgraInquireList.md)). Use [`M_CENTER_X`](../../Reference/gra/MgraInquireList.md) to be sure you are inquiring about the center position of any graphic.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of a graphic's center. |

---

### `M_CENTER_Y`

Inquires the Y-coordinate of a graphic's center.  Except for rectangles, [`M_CENTER_Y`](../../Reference/gra/MgraInquireList.md) returns the same value as [`M_POSITION_Y`](../../Reference/gra/MgraInquireList.md). For rectangles, [`M_CENTER_Y`](../../Reference/gra/MgraInquireList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraInquireList.md) can have different values, depending on how Aurora Imaging Library interprets the rectangle's position (which is established with [`M_POSITION_TYPE`](../../Reference/gra/MgraInquireList.md)). Use [`M_CENTER_Y`](../../Reference/gra/MgraInquireList.md) to be sure you are inquiring about the center position of any graphic.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of a graphic's center. |

---

### `M_CONSTRAIN_ASPECT_RATIO`

Inquires whether the width and height are forced to be equal.  You can inquire this value for arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)) and rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)). Inquiring this value for any other type of graphic will generate an Aurora Imaging Library error.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the width and height of a graphic are not forced to be equal. |
| `1.0` | Specifies that the width and height of a graphic are forced to be equal. |

---

### `M_CORNER_BOTTOM_LEFT_X`

Inquires the X-coordinate of the bottom-left corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the selection box's bottom-left corner. |

---

### `M_CORNER_BOTTOM_LEFT_Y`

Inquires the Y-coordinate of the bottom-left corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the selection box's bottom-left corner. |

---

### `M_CORNER_BOTTOM_RIGHT_X`

Inquires the X-coordinate of the bottom-right corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the selection box's bottom-right corner. |

---

### `M_CORNER_BOTTOM_RIGHT_Y`

Inquires the Y-coordinate of the bottom-right corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the selection box's bottom-right corner. |

---

### `M_CORNER_TOP_LEFT_X`

Inquires the X-coordinate of the top-left corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the selection box's top-left corner. |

---

### `M_CORNER_TOP_LEFT_Y`

Inquires the Y-coordinate of the top-left corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the selection box's top-left corner. |

---

### `M_CORNER_TOP_RIGHT_X`

Inquires the X-coordinate of the top-right corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinate of the selection box's top-right corner. |

---

### `M_CORNER_TOP_RIGHT_Y`

Inquires the Y-coordinate of the top-right corner of the graphic's selection box.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinate of the selection box's top-right corner. |

---

### `M_FILLED`

Inquires whether the graphic is filled.  You can inquire this value for arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)), polygons ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md)), and rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)). Inquiring this value for any other type of graphic will generate an error.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the graphic is not filled. |
| `M_TRUE` | Specifies that the graphic is filled. |

---

### `M_POSITION_TYPE`

Inquires how to interpret [`M_POSITION_X`](../../Reference/gra/MgraInquireList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraInquireList.md).

| Value | Description |
| --- | --- |
| `M_CENTER_AND_DIMENSION` | Specifies to interpret [`M_POSITION_X`](../../Reference/gra/MgraInquireList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraInquireList.md) as the rectangle's center. |
| `M_CORNER_AND_DIMENSION` | Specifies to interpret [`M_POSITION_X`](../../Reference/gra/MgraInquireList.md) and [`M_POSITION_Y`](../../Reference/gra/MgraInquireList.md) as the rectangle's top-left corner. |

---

### `M_POSITION_X`

Inquires the specified X-position of a graphic or of one of its sub-elements.  This value is set whenever a graphic is defined, either programmatically or interactively. The point returned for the position of a graphic is its center for arcs, sets of dots, a single line, sets of finite-length lines, sets of infinite lines, polygons, and polylines. The point returned for the position of text is dependent upon [`M_TEXT_ALIGN_HORIZONTAL`](../../Reference/gra/MgraControlList.md) and [`M_TEXT_ALIGN_VERTICAL`](../../Reference/gra/MgraControlList.md). The point returned for the position of a rectangle is its center or its top-left corner, dependent upon the setting of [`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-position. |

---

### `M_POSITION_Y`

Inquires the specified Y-position of a graphic or of one of its sub-elements.  This value is set whenever a graphic is defined, either programmatically or interactively. The point returned for the position of a graphic is its center for arcs, sets of dots, a single line, sets of finite-length lines, sets of infinite lines, polygons, and polylines. The point returned for the position of text is dependent upon [`M_TEXT_ALIGN_HORIZONTAL`](../../Reference/gra/MgraControlList.md) and [`M_TEXT_ALIGN_VERTICAL`](../../Reference/gra/MgraControlList.md). The point returned for the position of a rectangle is its center or its top-left corner, dependent upon the setting of [`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-position. |

---

### `M_RADIUS_X`

Inquires the radius of the arc, in the X-direction.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the radius. |

---

### `M_RADIUS_Y`

Inquires the radius of the arc, in the Y-direction.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the radius. |

---

### `M_RECTANGLE_HEIGHT`

Inquires the height of the rectangle.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height. |

---

### `M_RECTANGLE_WIDTH`

Inquires the width of a rectangle.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width. |

### For inquiring about general keyboard key settings to interactively modify graphics

To inquire about general settings for using keyboard keys to interactively modify graphics, the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. These settings apply to the 2D graphics list itself. In this case, you must set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_LIST`](../../Reference/gra/MgraInquireList.md) and the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

---

### `M_ACTION_KEYS`

Inquires whether the 2D graphics list allows you to interactively modify its graphics by pressing keys on the keyboard (keyboard events).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the 2D graphics list ignores keyboard events. |
| `M_ENABLE` | Specifies that the 2D graphics list allows keyboard events to interactively modify its graphics. |

---

### `M_ACTION_MODIFIER_SPEED`

Inquires the combination keyboard key that you can press to select an alternate speed to rotate, resize, or translate interactively selected graphics with the keyboard.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_KEY_ALT` | Specifies to use the Alt key to select an alternate speed. |
| `M_KEY_CTRL` *(default)* | Specifies to use the Ctrl key to select an alternate speed. |
| `M_KEY_SHIFT` | Specifies to use the Shift key to select an alternate speed. |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_RESIZE_INCREMENT`

Inquires the value with which to increase or decrease the width or height of the interactively selected graphic for a single key press.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_RESIZE_INCREMENT_ALTERNATE`

Inquires an alternate value with which to increase or decrease the width or height of the interactively selected graphic for a single key press.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_ROTATE_INCREMENT`

Inquires the angle with which to rotate the interactively selected graphic for a single key press.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_ACTION_ROTATE_INCREMENT_ALTERNATE`

Inquires an alternate angle with which to rotate the interactively selected graphic for a single key press.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_ACTION_TRANSLATE_AXES`

Inquires the axes along which Aurora Imaging Library translates a graphic that is in world units, when the translation is done using the keyboard.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` | Specifies to convert the translation increment to world units using the calibration's average pixel size in the translation direction, then apply it along the world axes. |
| `M_PIXEL` *(default)* | Specifies to apply the translation increment to the graphic position, along the image axes, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_TRANSLATE_INCREMENT`

Inquires the value with which to translate (move) the interactively selected graphics for a single key press.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

---

### `M_ACTION_TRANSLATE_INCREMENT_ALTERNATE`

Inquires an alternate value with which to translate (move) the interactively selected graphics for a single key press.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the increment, in display units (these are pixel units that remain unaltered when you pan or zoom the display). |

### For inquiring about explicit keyboard keys to interactively modify graphics

To inquire about explicit keyboard keys to interactively modify graphics, the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter should be set to one of the following values. These settings apply to the 2D graphics list itself. In this case, you must set the [`LabelOrIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_LIST`](../../Reference/gra/MgraInquireList.md) and the [`SubIndex`](../../Reference/gra/MgraInquireList.md) parameter to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md).

---

### `M_ACTION_KEY_CANCEL`

Inquires the keyboard keys that you can press to cancel the interactive graphic modification you are currently performing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_DELETE`

Inquires the keyboard keys that you can press to delete the interactively selected graphics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_HEIGHT_DOWN`

Inquires the keyboard keys that you can press to reduce the height of the interactively selected graphics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_DOWN`](../../Reference/gra/MgraInquireList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_HEIGHT_UP`

Inquires the keyboard keys that you can press to increase the height of the interactively selected graphics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_UP`](../../Reference/gra/MgraInquireList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_WIDTH_DOWN`

Inquires the keyboard keys that you can press to reduce the width of the interactively selected graphics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_LEFT`](../../Reference/gra/MgraInquireList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_RESIZE_WIDTH_UP`

Inquires the keyboard keys that you can press to increase the width of the interactively selected graphics.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraInquireList.md) + [`M_KEY_SHIFT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROTATE_CLOCKWISE`

Inquires the keyboard keys that you can press to rotate the interactively selected graphics clockwise.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraInquireList.md) + [`M_KEY_ALT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_ROTATE_COUNTER_CLOCKWISE`

Inquires the keyboard keys that you can press to rotate the interactively selected graphics counter-clockwise.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_LEFT`](../../Reference/gra/MgraInquireList.md) + [`M_KEY_ALT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_DOWN`

Inquires the keyboard keys that you can press to translate (move) the interactively selected graphics down.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_DOWN`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_LEFT`

Inquires the keyboard keys that you can press to translate (move) the interactively selected graphics left.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_LEFT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_RIGHT`

Inquires the keyboard keys that you can press to translate (move) the interactively selected graphics right.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_RIGHT`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

---

### `M_ACTION_KEY_TRANSLATE_UP`

Inquires the keyboard keys that you can press to translate (move) the interactively selected graphics up.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_KEY_ARROW_UP`](../../Reference/gra/MgraInquireList.md). |
| `M_NONE` | Specifies that there are no keyboard keys for this action setting. |

### Combination Constants — For inquiring about additional keyboard keys (the values you get back with the

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to get additional keyboard keys that were explicitly specified (the values you get back with the UserVarPtr parameter might be combined).

> **Note:** You do not add the following values to the above mentioned values. One or more of the following values might be combined with the value returned ([`UserVarPtr`](../../Reference/gra/MgraInquireList.md)) when the [`InquireType`](../../Reference/gra/MgraInquireList.md) parameter is set to one of the above mentioned values.

| Value | Description |
| --- | --- |
| `M_KEY_ALT` | Specifies the Alt key. |
| `M_KEY_CTRL` | Specifies the Ctrl key. |
| `M_KEY_SHIFT` | Specifies the Shift key. |

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

If [`SubIndex`](../../Reference/gra/MgraInquireList.md) is set to [`M_DEFAULT`](../../Reference/gra/MgraInquireList.md), this setting is inquired for the entire graphic. If a sub-element is specified, this setting is inquired for a single point within the graphic.

You can inquire this value for arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)). Inquiring this value for any other type of graphic will generate an error.

You can inquire this value for rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)). Inquiring this value for any other type of graphic will generate an error.

You can inquire this value for all graphics except drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). Inquiring this value on an unsupported graphic will generate an error.

You can inquire this value for all graphics except text ([`MgraText`](../../Reference/gra/MgraText.md)), and drawings created using a processing or analysis module draw function ([`M...Draw`](../../Reference/3dmap/M3dmapDraw.md)). Inquiring this value for any of the unsupported types of graphics will generate an error.

You can inquire this value for arcs ([`MgraArc`](../../Reference/gra/MgraArc.md), [`MgraArcFill`](../../Reference/gra/MgraArcFill.md), and [`MgraArcAngle`](../../Reference/gra/MgraArcAngle.md)), dots ([`MgraDots`](../../Reference/gra/MgraDots.md)), lines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_LINE_LIST`](../../Reference/gra/MgraLines.md)), polygons ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYGON`](../../Reference/gra/MgraLines.md)), polylines ([`MgraLines`](../../Reference/gra/MgraLines.md) with [`M_POLYLINE`](../../Reference/gra/MgraLines.md)), and rectangles ([`MgraRect`](../../Reference/gra/MgraRect.md), [`MgraRectFill`](../../Reference/gra/MgraRectFill.md), and [`MgraRectAngle`](../../Reference/gra/MgraRectAngle.md)). Inquiring this value for any other type of graphic will generate an error.
