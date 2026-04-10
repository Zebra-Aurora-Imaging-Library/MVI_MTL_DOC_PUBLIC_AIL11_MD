---
doctype: UserGuide
part: "2D related information"
chapter: Generating_graphics
section: Creating_and_modifying_graphics_interactively
module_tag: gra
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / graphics / Creating and modifying graphics interactively"
---

# Creating and modifying graphics interactively

You can create and edit graphics interactively in a display. This can be useful to, for example, interactively define a graphic that covers a required region, and then use the graphic to define or modify a region of interest.

The following steps provide a basic methodology for enabling and controlling interactivity with graphics:

1. Associate a 2D graphics list with your display using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/disp/MdispControl.md).
2. If necessary, restrict interactive properties of graphics using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md).
3. Hook a function to the 2D graphics list event caused by a change in the state of interactivity of a graphic, using [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md) with [`M_INTERACTIVE_GRAPHIC_STATE_MODIFIED`](../../Reference/gra/MgraHookFunction.md). For example, if the state of interactivity changes from [`M_STATE_..._DRAGGED`](../../Reference/gra/MgraInquireList.md) to [`M_STATE_..._HOVERED`](../../Reference/gra/MgraInquireList.md), a graphic has been modified interactively.
4. Make graphics in the 2D graphics list interactive using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_GRAPHIC_LIST_INTERACTIVE`](../../Reference/disp/MdispControl.md).
5. If necessary, allow the user to add graphics interactively to the 2D graphics list using [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) set to one of the possible graphics types.

With a graphic that has interactivity enabled, you can move a graphic's position, resize the graphic, or rotate the graphic. When a user clicks on a displayed graphic with interactivity enabled, the graphic is considered selected. By default, you must click on the contour of a graphic to select it. To select closed graphics (such as circles) by clicking anywhere on them, even if they are not filled, call [`MgraControlList`](../../Reference/gra/MgraControlList.md) and enable [`M_EASY_SELECTION`](../../Reference/gra/MgraControlList.md).

A selected graphic is typically surrounded by a selection box (represented with a dotted line) with handles around it that allows a user to resize or rotate the graphic. Certain graphics can also have handles specific to that graphic. For example, an arc has handles to increase its length and the polygon has handles to change the position of each vertex.

*[Image: gra_interactive_arc.png]*

When you restrict any or all of the interactive properties of a graphic using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md), the handles specific to that property will not appear. For example, disabling interactive rotation will cause the rotate handle to not appear when the graphic is selected. You can select multiple graphics on the display by pressing the Ctrl key while clicking on the required graphics. To change this behavior, call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with[`M_MULTIPLE_SELECTION_KEY`](../../Reference/gra/MgraControlList.md).

The 2D graphics list has default behaviors for interactively moving, resizing, or rotating its graphics. To modify these behaviors, call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_MODE_...`](../../Reference/gra/MgraControlList.md). For example, if you click on a corner of a graphic's selection box and drag your mouse to resize the graphic, the opposite corner remains fixed. This happens because [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) is set to [`M_FIXED_CORNER`](../../Reference/gra/MgraControlList.md) by default. To modify this resizing behavior so that, as you drag a corner of the selection box with your mouse, all corners move symmetrically (and the center remains fixed), set [`M_MODE_RESIZE`](../../Reference/gra/MgraControlList.md) to [`M_FIXED_CENTER`](../../Reference/gra/MgraControlList.md). The 2D graphics list also has default behaviors for interactively moving, resizing, or rotating its graphics with your mouse while pressing the Alt ([`M_MODE_..._ALT`](../../Reference/gra/MgraControlList.md)), Ctrl ([`M_MODE_..._CTRL`](../../Reference/gra/MgraControlList.md)), or Shift ([`M_MODE_..._SHIFT`](../../Reference/gra/MgraControlList.md)) key.

Instead of using your mouse to move, resize, or rotate interactively selected graphics, you can use keys on the keyboard. For example, you can specify that pressing the Arrow keys will move the selected graphic. To specify keyboard keys with which to manipulate (perform actions on) interactively selected graphics, call [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_ACTION_KEY_...`](../../Reference/gra/MgraControlList.md).

For a complete list of settings that let you control interactivity in your application, refer to [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md).

## Hooking a function to a 2D graphics list event

The Aurora Imaging Library Graphics module allows you to hook a function to a 2D graphics list event using [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md). When developing applications that allow interactive user manipulations of graphics, it can be advantageous if a function is hooked to the event caused by a change in the state of interactivity in the 2D graphics list ([`M_INTERACTIVE_GRAPHIC_STATE_MODIFIED`](../../Reference/gra/MgraHookFunction.md)). The associated function will automatically be triggered whenever the state of interactivity is changed. The state of interactivity ([`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraInquireList.md)) indicates the active interaction state occurring on the selected graphics in the 2D graphics list. If, for example, a graphic is displayed with interactive mode enabled, and the cursor is not hovered over the graphic or one of its handles, then the state of interactivity is [`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md). If the user then hovers over the graphic or one of its handles with the cursor, the state of interactivity changes to [`M_STATE_GRAPHIC_HOVERED`](../../Reference/gra/MgraInquireList.md) or [`M_STATE_HANDLE_HOVERED`](../../Reference/gra/MgraInquireList.md), respectively. If the user then holds down the left mouse button when hovering over the graphic or one of its handles, the state of interactivity changes to [`M_STATE_GRAPHIC_DRAGGED`](../../Reference/gra/MgraInquireList.md) or [`M_STATE_HANDLE_DRAGGED`](../../Reference/gra/MgraInquireList.md), respectively.

There are other events to which you can hook a function, such as [`M_GRAPHIC_LIST_MODIFIED`](../../Reference/gra/MgraHookFunction.md), [`M_GRAPHIC_MODIFIED`](../../Reference/gra/MgraHookFunction.md), and [`M_GRAPHIC_SELECTION_MODIFIED`](../../Reference/gra/MgraHookFunction.md). These events will trigger due to both interactive and programmatic changes. For an example of using [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md) in an application with an interactive graphic, see:

> **Code example:** [mgrainteractive.cpp](mgrainteractive.cpp)

## Creating graphics interactively

You can allow a user to interactively add a graphic to a 2D graphics list. The function [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) changes the state of interactivity of the 2D graphics list to allow the user to define graphics in the display using the mouse. The graphic types that can be created interactively are: an arc ([`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md)), a dot ([`M_GRAPHIC_TYPE_DOT`](../../Reference/gra/MgraInteractive.md)), a line ([`M_GRAPHIC_TYPE_LINE`](../../Reference/gra/MgraInteractive.md)), a polygon ([`M_GRAPHIC_TYPE_POLYGON`](../../Reference/gra/MgraInteractive.md)), a polyline ([`M_GRAPHIC_TYPE_POLYLINE`](../../Reference/gra/MgraInteractive.md)), and a rectangle ([`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md)). By calling [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with one of these values, the state of interactivity changes to [`M_STATE_WAITING_FOR_CREATION`](../../Reference/gra/MgraInquireList.md). For more information about how to create these graphics interactively, see [Details about creating graphics interactively](S07_Creating_and_modifying_graphics_interactively.md).

When in an [`M_STATE_WAITING_FOR_CREATION`](../../Reference/gra/MgraInquireList.md) state, the user is expected to define a graphic interactively in the display. Note, however, that [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) is asynchronous and only changes the state of interactivity; it will not wait for the user to define the graphic. You should have your application wait for the user to finish defining the graphic, before calling further functions.

When the user begins defining the graphic, the state of interactivity ([`M_INTERACTIVE_GRAPHIC_STATE`](../../Reference/gra/MgraInquireList.md)) changes to [`M_STATE_BEING_CREATED`](../../Reference/gra/MgraInquireList.md). After a user has defined the graphic interactively, the state of interactivity returns to [`M_STATE_IDLE`](../../Reference/gra/MgraInquireList.md) and you can continue your application from that moment.

If the program does not wait for the user to create the graphic after a call to [`MgraInteractive`](../../Reference/gra/MgraInteractive.md), your application might not perform as you initially intended. Waiting for user interaction can be accomplished by hooking a function to [`M_INTERACTIVE_GRAPHIC_STATE_MODIFIED`](../../Reference/gra/MgraHookFunction.md) event and having the hook function signal the end of the thread wait event. The following example shows how to wait for the user to interactively add a rectangle to the 2D graphics list and then use this graphic to define the ROI of an image buffer. The hook-handler function is called each time the state of interactivity changes, and checks if the state has changed to [`M_STATE_BEING_CREATED`](../../Reference/gra/MgraInquireList.md). A data structure is used to store information inquired in the hook-handler function. Note that when using interactive graphics, the graphics can potentially be added in a different thread. In the following example, the label of the graphic is obtained using [`MgraGetHookInfo`](../../Reference/gra/MgraGetHookInfo.md) with [`M_GRAPHIC_LABEL_VALUE`](../../Reference/gra/MgraGetHookInfo.md) rather than using [`MgraInquireList`](../../Reference/gra/MgraInquireList.md) with [`M_LAST_LABEL`](../../Reference/gra/MgraInquireList.md). This is due to the fact that [`MgraGetHookInfo`](../../Reference/gra/MgraGetHookInfo.md) with [`M_GRAPHIC_LABEL_VALUE`](../../Reference/gra/MgraGetHookInfo.md) will return the label value of an interactively created graphic regardless of the thread in which the graphic was added.

> **Code example:** [userguide.generating_graphics.creating_and_modifying_graphics_interactively01](userguide.generating_graphics.creating_and_modifying_graphics_interactively01)

Where the hooked function is:

> **Code example:** [userguide.generating_graphics.creating_and_modifying_graphics_interactively02](userguide.generating_graphics.creating_and_modifying_graphics_interactively02)

[`MgraInteractive`](../../Reference/gra/MgraInteractive.md) can be useful if your application requires toolbars for a graphical user interface. For example, you can use another API to create a toolbar, with one of its buttons calling [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md). After clicking on the toolbar button, the user can create a rectangle in the display to specify where to perform processing or analysis in the image.

## Details about creating graphics interactively

The following subsections illustrate some details about how to interactively create graphics, by calling [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) with [`M_GRAPHIC_TYPE_...`](../../Reference/gra/MgraInteractive.md).

### Rectangle

[`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) adds a rectangle to the 2D graphics list by defining it interactively in the display. Typically, [`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) is set to [`M_AXIS_ALIGNED_RECT`](../../Reference/gra/MgraInteractive.md), to define an axis-aligned rectangle. This requires you to click on a rectangle's corner in the display, and then drag the mouse to the opposite corner. An example of this behavior can be seen in the animation below.

*[Image: MgraRECT_AXIS_ALIGNED]*

[`M_GRAPHIC_TYPE_RECT`](../../Reference/gra/MgraInteractive.md) can also be set to [`M_ORIENTED_RECT`](../../Reference/gra/MgraInteractive.md), to define an oriented rectangle. This requires three clicks in the display. By default, the first click defines a fixed corner of the rectangle, the second click defines its width (a line extending to the fixed corner) and orientation, and the third click defines its height on one side of the width line. An example of this behavior can be seen in the animation below.

*[Image: MgraRECT_M_ORIENTED_RECT]*

### Arc, ellipse, and circle

[`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md) adds an arc to the 2D graphics list by defining it interactively in the display. You can also use [`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md) to interactively define a circle or ellipse.

Typically, [`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md) is to [`M_AXIS_ALIGNED_ELLIPSE`](../../Reference/gra/MgraInteractive.md), to define an axis-aligned ellipse (or circle). This requires you to click on one corner of the ellipse bounding box in the display, and then drag the mouse to the opposite corner. An example of this behavior can be seen in the animation below.

*[Image: MgraARC_ALIGNED_ELLIPSE]*

By setting [`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md) to [`M_ARC_THREE_POINTS`](../../Reference/gra/MgraInteractive.md), you can define a circular arc. This requires three clicks in the display. The first and third clicks determine the arc's start angle and end angle (not necessarily in this order); the second click establishes the position of the arc. An example of this behavior can be seen in the animation below.

*[Image: MgraARC_THREE_POINTS]*

Instead of using the ellipse's bounding box, you can set [`M_GRAPHIC_TYPE_ARC`](../../Reference/gra/MgraInteractive.md) to [`M_CIRCLE`](../../Reference/gra/MgraInteractive.md), to use the circumference of a circle. An example of this behavior can be seen in the animation below.

*[Image: MgraARC_CIRCLE]*

### Polygon

[`M_GRAPHIC_TYPE_POLYGON`](../../Reference/gra/MgraInteractive.md) adds a polygon to the 2D graphics list by defining it interactively in the display. This requires you to left-click each polygon vertex in the display, except for the last vertex, which you must define with a right-click. By default, creating a polygon while holding down the Alt key snaps the next polygon segment to 15° angles. An example of this behavior can be seen in the animation below.

*[Image: MgraPOLYGON_M_DEFAULT]*

### Polyline

[`M_GRAPHIC_TYPE_POLYLINE`](../../Reference/gra/MgraInteractive.md) adds a polyline to the 2D graphics list by defining it interactively in the display. This requires you to left-click each polyline vertex in the display, except for the last vertex, which you must define with a right-click. By default, creating a polyline while holding down the Alt key snaps the next polyline segment to 15° angles. An example of this behavior can be seen in the animation below.

*[Image: MgraPOLYLINE_M_DEFAULT]*

### Line

[`M_GRAPHIC_TYPE_LINE`](../../Reference/gra/MgraInteractive.md) adds a line to the 2D graphics list by defining the line interactively in the display. This requires you to click on the line's start point in the display, and then drag the mouse to the line's end point. Holding Alt during line manipulations will restrict its orientation to specific angles; use [`M_ANGLE_SNAPPING_VALUE`](../../Reference/gra/MgraControlList.md)to specify the snapping angle values. An example of this behavior can be seen in the animation below.

*[Image: MgraLINE_M_DEFAULT]*
