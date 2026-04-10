---
doctype: Reference
module: gra
function: MgraInteractive
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraInteractive"
---

# MgraInteractive

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

> Change the state of interactivity of graphics in a 2D graphics list.

## Syntax

```c
void MgraInteractive(
    AIL_ID    ContextGraId,  //out
    AIL_ID    ListGraId,     //in
    AIL_INT64 GraphicType,   //in
    AIL_INT64 InitFlag,      //in
    AIL_INT64 CreationMode   //in
)
```

## Description

This function changes the state of interactivity ([`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraInquireList.md)) of graphics in a 2D graphics list. It is primarily used to change the state of interactivity of the graphics such that a user can interactively add a graphic of the specified type to the 2D graphics list ([`M_STATE_WAITING_FOR_CREATION`](../../Reference/gra/MgraInquireList.md)). In addition, the function can end any interactive manipulations occurring in the display and return to an idle state ([`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md)).

When in the waiting for creation state ([`GraphicType`](../../Reference/gra/MgraInteractive.md) set to an [`M_GRAPHIC_TYPE_...`](../../Reference/gra/MgraInteractive.md) value), the user is expected to define a graphic interactively in the display. Note, however, that [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) is asynchronous and only changes the state of interactivity; it will not wait for the user to define the graphic. You should have your application wait for the user to finish defining the graphic, before calling further functions; to do so, hook a function to the change in state of interactivity using [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md) with [`M_INTERACTIVE_GRAPHIC_STATE_MODIFIED`](../../Reference/gra/MgraHookFunction.md). When the user begins defining the graphic, the state of interactivity of the 2D graphics list ([`MgraInquireList`](../../Reference/gra/MgraInquireList.md) or [`MgraGetHookInfo`](../../Reference/gra/MgraGetHookInfo.md)with [`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraInquireList.md)) changes to [`M_STATE_BEING_CREATED`](../../Reference/gra/MgraInquireList.md). After a user has defined the graphic interactively, the state of interactivity returns to [`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md) and you can continue your application from that moment.

This function is especially useful if your application uses toolbars for a graphical user interface. For example, you can use another API to create a toolbar, with one of its buttons calling [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md). After clicking on the toolbar button, the user can define a rectangle in the display to specify where to perform processing or analysis in the image.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`ListGraId`](../../Reference/gra/MgraInteractive.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

[For more information, see Chapter 26: Generating graphics.](../../UserGuide/C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md)

## Parameters

### `ContextGraId` *(out, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the default 2D graphics context of the current Aurora Imaging Library application context.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `M_NULL` | Specifies a 2D graphics context is not necessary; you should use this setting only when [`GraphicType`](../../Reference/gra/MgraInteractive.md) is set to [`M_CANCEL`](../../Reference/gra/MgraInteractive.md) or [`M_STOP`](../../Reference/gra/MgraInteractive.md). |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ListGraId` *(in, AIL_ID)*

Specifies the identifier of a valid 2D graphics list whose state of interactivity must be changed. You must have allocated the 2D graphics list using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

### `GraphicType` *(in, AIL_INT64)*

Specifies the type of graphic to define interactively and add to the 2D graphics list, or specifies to cancel or stop the 2D graphics list's state of interactivity.

### `InitFlag` *(in, AIL_INT64)*

Specifies optional behavior when defining the graphic in the display. If this behavior is not required or supported, set this parameter to `M_DEFAULT`. This parameter can be set to one or a combination of the following values. If the value is not supported for the graphic you are defining, an error is generated.

*For specifying optional behavior when defining the graphic*

| Value | Description |
| --- | --- |
| `M_FILLED` | Specifies that the graphic is filled ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_FILLED`](../../Reference/gra/MgraControlList.md) set to [`M_TRUE`](../../Reference/gra/MgraControlList.md)).

This setting only applies to arcs ([`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md)), polygons ([`M_GRAPHIC_TYPE_POLYGON`](../../Reference/gra/MgraInteractive.md)), and rectangles ([`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md)).

*[Image: Interactive-Graphics-FILLED.png]* |
| `M_ROTATE_AROUND_CORNER` | Specifies that the rectangle's position is set to its corner ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md) set to [`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraControlList.md)). If not specified, the rectangle's position is set to its center by default.

This setting only applies to rectangles ([`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md)).

*[Image: interactive-Graphics-ROTATE_AROUND_CORNER.png]* |
| `M_SECTOR` | Specifies that the arc is drawn as a sector ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_ARC_STYLE`](../../Reference/gra/MgraControlList.md) set to [`M_SECTOR`](../../Reference/gra/MgraControlList.md)).

This setting only applies to arcs ([`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md)).

*[Image: Interactive-Graphics-Constant_sector.png]* |
| `M_SQUARE_ASPECT_RATIO` | Specifies that the graphic has a 1:1 (square) aspect ratio ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_CONSTRAIN_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) set to 1.0).

This setting only applies to arcs ([`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md)) and rectangles ([`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md)) that are not oriented ([`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) with [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md)).

*[Image: Interactive-Graphics-SQUARE-ASPECT-RATIO.png]* |

### `CreationMode` *(in, AIL_INT64)*

Specifies how to interactively define the graphic.

## Parameter Associations

### For defining the graphic interactively

The following [`GraphicType`](../../Reference/gra/MgraInteractive.md) and corresponding [`CreationMode`](../../Reference/gra/MgraInteractive.md) parameter settings let you define the graphic interactively. Specifically, the following values change the 2D graphics list's state of interactivity ([`MgraInquireList`](../../Reference/gra/MgraInquireList.md) with [`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraInquireList.md)) to [`M_STATE_WAITING_FOR_CREATION`](../../Reference/gra/MgraInquireList.md), allowing you to add a graphic to the 2D graphics list by defining the graphic interactively in the display.

---

### `M_GRAPHIC_TYPE_ARC`

Adds an arc to the 2D graphics list by defining the arc interactively in the display. In addition to defining an arc, you can use this value to define a circle or an ellipse (which are extensions of the arc mechanism).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the legacy default behavior; instead of [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md), Aurora Imaging Library recommends using [`M_AXIS_ALIGNED_ELLIPSE`](../../Reference/gra/MgraInteractive.md).  Do not specify [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) unless required for backward compatibility. [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) ignores 2D graphics list settings ([`MgraControlList`](../../Reference/gra/MgraControlList.md)) related to whether resizing is with respect to the center ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md)) or the corner ([`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) of the selection box. [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) requires you to click on the ellipse center in the display, and drag both ellipse radii with the mouse (this can be considered a legacy version of an axis-aligned ellipse). Aurora Imaging Library adds the ellipse to the 2D graphics list on the first mouse-down event. |
| `M_ARC_THREE_POINTS` | Specifies to define a circular arc. This requires three clicks in the display. The first and third clicks determine the arc's start angle and end angle (not necessarily in this order); the second click establishes the position of the arc. Aurora Imaging Library adds the arc to the 2D graphics list on the first mouse-move event after the second mouse-up event. Aurora Imaging Library does not allow the three clicks to form a straight line. |
| `M_AXIS_ALIGNED_ELLIPSE` | Specifies to define an axis-aligned ellipse. This requires you to click on one corner of the ellipse bounding box in the display, and then drag the mouse to the opposite corner. Aurora Imaging Library adds the ellipse to the 2D graphics list on the first mouse-down event.  Instead of defining a corner of the ellipse bounding box, the first click can define the ellipse center. This can be the default behavior ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) set to [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md)), or the behavior when resizing while pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key.  You can use [`M_AXIS_ALIGNED_ELLIPSE`](../../Reference/gra/MgraInteractive.md) to define a circle (which is a special type of ellipse), by setting the [`InitFlag`](../../Reference/gra/MgraInteractive.md) parameter to [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraInteractive.md). You can also call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) as a combination value for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) to specify this as the default behavior, or the behavior when pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key. |
| `M_CIRCLE` | Specifies to define a circle. This requires you to click on a point on the circle's circumference in the display, and then drag the mouse to the diametrically opposite point. Aurora Imaging Library adds the circle to the 2D graphics list on the first mouse-down event.  Instead of defining a point on the circle's circumference, the first click can define the circle's center. This can be the default behavior ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) set to [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md)), or the behavior when resizing while pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key. |

---

### `M_GRAPHIC_TYPE_DOT`

Adds a dot to the 2D graphics list by defining the dot interactively in the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to define a dot. This requires you to click on the position in the display at which to place the dot. Aurora Imaging Library adds the dot to the 2D graphics list on the mouse-down event. |

---

### `M_GRAPHIC_TYPE_LINE`

Adds a line to the 2D graphics list by defining the line interactively in the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to define a line. This requires you to click on the line's start point in the display, and then drag the mouse to the line's end point. Aurora Imaging Library adds the line to the 2D graphics list on the first mouse-down event.  Instead of defining the line's start point, the first click can define the line's center. This can be the default behavior ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) set to [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md), or the behavior when resizing while pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key.  When creating the line, its end point can be seen as its angle of rotation. You can use [`MgraControlList`](../../Reference/gra/MgraControlList.md) to specify the default rotation (orientation) behavior ([`M_MODE_ROTATE`](../../Reference/gra/MgraControlList.md)), or the rotation (orientation) behavior while pressing the Alt ([`M_MODE_ROTATE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_ROTATE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_ROTATE_SHIFT`](../../Reference/gra/MgraControlList.md)) key. |

---

### `M_GRAPHIC_TYPE_POLYGON`

Adds a polygon to the 2D graphics list by defining the polygon interactively in the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to define a polygon. This requires you to left-click each polygon vertex in the display, except for the last vertex, which you must define with a right-click. Aurora Imaging Library adds the polygon to the 2D graphics list on the first mouse-down event.  By default, creating a polygon while pressing the Alt key snaps the next polygon segment to 15° angles. You can call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_MODE_ROTATE...`](../../Reference/gra/MgraControlList.md) to change this behavior. |

---

### `M_GRAPHIC_TYPE_POLYLINE`

Adds a polyline to the 2D graphics list by defining the polyline interactively in the display.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to define a polyline. This requires you to left-click each polyline vertex in the display, except for the last vertex, which you must define with a right-click. Aurora Imaging Library adds the polyline to the 2D graphics list on the first mouse-down event.  By default, creating a polyline while pressing the Alt key snaps the next polyline segment to 15° angles. You can call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_MODE_ROTATE...`](../../Reference/gra/MgraControlList.md) to change this behavior. |

---

### `M_GRAPHIC_TYPE_RECT`

Adds a rectangle to the 2D graphics list by defining the rectangle interactively in the display. By default, you define the rectangle according to its center. This default behavior is established using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md) set to [`M_CENTER_AND_DIMENSION`](../../Reference/gra/MgraControlList.md). To define the rectangle according to its top-left corner, set the [`InitFlag`](../../Reference/gra/MgraInteractive.md) parameter ([`MgraInteractive`](../../Reference/gra/MgraInteractive.md)) to [`M_ROTATE_AROUND_CORNER`](../../Reference/gra/MgraInteractive.md). This is equivalent to setting [`M_POSITION_TYPE`](../../Reference/gra/MgraControlList.md) to [`M_CORNER_AND_DIMENSION`](../../Reference/gra/MgraControlList.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the legacy default behavior; instead of [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md), Aurora Imaging Library recommends using [`M_AXIS_ALIGNED_RECT`](../../Reference/gra/MgraInteractive.md).  Do not specify [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) unless required for backward compatibility. [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) ignores 2D graphics list settings ([`MgraControlList`](../../Reference/gra/MgraControlList.md)) related to whether resizing is with respect to the center ([`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md)) or the corner ([`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md)) of the selection box. You also cannot use [`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) with [`M_ROTATE_AROUND_CORNER`](../../Reference/gra/MgraInteractive.md).[`M_DEFAULT`](../../Reference/gra/MgraInteractive.md) requires you to click on a rectangle corner in the display, and then drag the mouse to the opposite corner (this can be considered a legacy version of an axis-aligned rectangle). Aurora Imaging Library adds the rectangle to the 2D graphics list on the first mouse-down event. |
| `M_AXIS_ALIGNED_RECT` | Specifies to define an axis-aligned rectangle. This requires you to click on a rectangle corner in the display, and then drag the mouse to the opposite corner. Aurora Imaging Library adds the rectangle to the 2D graphics list on the first mouse-down event. Instead of defining a rectangle corner, the first click can define the rectangle center. This can be the default behavior ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) set to [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md)), or the behavior when resizing while pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key.  You can use [`M_AXIS_ALIGNED_RECT`](../../Reference/gra/MgraInteractive.md) to define a square (which is a special type of rectangle), by setting the [`InitFlag`](../../Reference/gra/MgraInteractive.md) parameter to [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraInteractive.md). You can also call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) as a combination value for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) to specify this as the default behavior, or the behavior when pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key. |
| `M_ORIENTED_RECT` | Specifies to define an oriented rectangle. This requires three clicks in the display. By default, the first click defines a fixed corner of the rectangle, the second click defines its width (a line extending to the fixed corner), and the third click defines its height on one side of the width line. Aurora Imaging Library adds the rectangle to the 2D graphics list on the first mouse-up event.  Instead of defining the rectangle's width using a fixed corner, the first two clicks can define the width symmetrically on both sides of a center point; that is, the first click represents the fixed center of the width line, and the second click extends the width line equally on both sides of the fixed center. This can be the default behavior ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) set to [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md)), or the behavior when pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key.  Instead of defining the rectangle's height on one side of the width line, the third click can define the height symmetrically on both sides; that is, moving your mouse above or below the width line (established by the first two clicks) adjusts the height equally on the other side until the third click. This can be the default behavior ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MODE_RESIZE_SECONDARY_DIMENSION`](../../Reference/gra/MgraControlList.md)), or the behavior when pressing the Alt ([`M_MODE_RESIZE_SECONDARY_DIMENSION_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_SECONDARY_DIMENSION_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SECONDARY_DIMENSION_SHIFT`](../../Reference/gra/MgraControlList.md)) key.  You can use [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md) to define a square (which is special type of rectangle), by setting the [`InitFlag`](../../Reference/gra/MgraInteractive.md) parameter to [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraInteractive.md). You can also call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_SQUARE_ASPECT_RATIO`](../../Reference/gra/MgraControlList.md) as a combination value for [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) to specify this as the default behavior, or the behavior when pressing the Alt ([`M_MODE_RESIZE_ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_RESIZE_CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_RESIZE_SHIFT`](../../Reference/gra/MgraControlList.md)) key. |

### For ending user interaction

The following [`GraphicType`](../../Reference/gra/MgraInteractive.md) and corresponding [`CreationMode`](../../Reference/gra/MgraInteractive.md) parameter settings change the 2D graphics list's state of interactivity to idle ([`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md)) and will cancel or stop any user manipulations in progress.

---

### `M_CANCEL`

Changes the 2D graphics list's state of interactivity to idle ([`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md)) and cancels any user manipulations in progress. A cancel operation will stop any user manipulation in progress and undo any changes that occurred. If, for example, a user was rotating a graphic with the rotate handle, canceling will stop the user from rotating it further and return the graphic to the angle it was set to before the user started manipulating it. If a user is interactively defining a graphic to add to the 2D graphics list, canceling will stop the defining process and not add any graphic to the list.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not required. |

---

### `M_STOP`

Changes the 2D graphics list's state of interactivity to idle ([`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md)) and stops any user manipulations in progress. A stop operation ends any user manipulations in progress and leaves any changes made to the graphic. If, for example, a user was rotating a graphic with the rotate handle, stopping will prevent the user from rotating it further and leave the graphic at that angle. If a user is interactively defining a graphic to add to the 2D graphics list, stopping will prevent the user from defining it further and adds the graphic to the list with the current dimensions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to ignore this parameter is not required. |
