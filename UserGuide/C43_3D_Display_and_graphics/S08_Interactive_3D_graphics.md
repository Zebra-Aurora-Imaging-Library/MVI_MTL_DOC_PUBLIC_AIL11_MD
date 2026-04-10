---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: Interactive_3D_graphics
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / Interactive 3D graphics"
---

# Interactive 3D graphics

It is possible to interactively manipulate any type of 3D graphic in a display. For convenience, box, grid, and plane graphics have built-in handle graphics for interactive editing; to enable the handles and make these types of graphics editable, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md). To make any 3D graphic interactive, you must set up handle graphics.

## Box, grid, and plane graphics

You can interactively edit box, grid, and plane graphics with the built-in handles. This can be useful to, for example, interactively define a box graphic that covers a required region, and then use the graphic to define a 3D box geometry that delimits the region.

The following steps provide a basic methodology for enabling and controlling interactivity with box, grid, and plane graphics:

1. Inquire the identifier of the internal 3D graphics list of a 3D display, using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md). Alternatively, inquire the identifier of other 3D graphics lists associated with the 3D display, using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with[`M_SHOWN_LISTS`](../../Reference/3ddisp/M3ddispInquire.md).
   Note that a 3D display is always associated with a default 3D graphics list, which is automatically allocated and freed at the same time as the 3D display. To display the contents of another 3D graphics list on a 3D display, you can copy the contents of the source 3D graphics list into the 3D display's internal 3D graphics list using[`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md). You can also associate a user-defined 3D graphics list with the 3D display using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md).
2. Add or draw a box, grid, or plane graphic into an associated 3D graphics list of the 3D display. You can add a box, grid, or plane graphic using [`M3dgraBox`](../../Reference/3dgra/M3dgraBox.md), [`M3dgraGrid`](../../Reference/3dgra/M3dgraGrid.md), or [`M3dgraPlane`](../../Reference/3dgra/M3dgraPlane.md), respectively. Alternatively, you can draw a graphic using [`M...Draw3d`](../../Reference/3dmap/M3dmapDraw3d.md).
3. If necessary, change how the graphic will appear when editable using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_EDITABLE_...`](../../Reference/3dgra/M3dgraControl.md).
4. Optionally, hook a function to the 3D graphics list event caused each time the box, grid, or plane graphic is interactively modified, using [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md) with [`M_EDITABLE_GRAPHIC_MODIFIED`](../../Reference/3dgra/M3dgraHookFunction.md).
5. Make the graphic interactive using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with the respective box, grid, or plane [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md) control type set to [`M_ENABLE`](../../Reference/3dgra/M3dgraControl.md). This adds handles to the graphic when displayed so that you can modify it interactively. Only one box, grid, or plane graphic is editable at a time.

When the graphic is editable interactively, you can resize the graphic, move the graphic's position, or rotate the graphic. To resize the graphic, click and drag on a scaling handle (shown as a brown cube on each face). To move the graphic's position, click and drag on one of its axes (or an axis arrowhead). To rotate the graphic, click and drag on one of the curved arcs that sit at the center of the displayed graphic. If necessary, you can change which box, grid, or plane graphic is editable, by disabling interactivity for one and enabling it for another, using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_EDITABLE`](../../Reference/3dgra/M3dgraControl.md).

The following animation shows how the graphic is manipulated with the mouse.

*[Image: 3dgra_editable_handle_animation]*

The selectable handles of an interactively editable box, grid, or plane graphic make it easily identifiable. To change the graphic's color, appearance, and opacity, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_EDITABLE_COLOR`](../../Reference/3dgra/M3dgraControl.md), [`M_EDITABLE_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md), and [`M_EDITABLE_OPACITY`](../../Reference/3dgra/M3dgraControl.md), respectively. These control types apply to the 3D graphics list, so the specified settings will apply even when you switch to a different interactively editable graphic.

## Handle graphics

It is possible to make any 3D graphic interactive. To do so, you must set up its handles. Any 3D graphic can be used as a handle for another graphic. You can make a 3D graphic an interactive handle using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_HANDLE_TYPE`](../../Reference/3dgra/M3dgraControl.md) to specify whether its target can be translated, rotated, or clicked using the mouse. Specify the target graphic for the handle using [`M_HANDLE_TARGET_LABEL`](../../Reference/3dgra/M3dgraControl.md). When the handle graphic is interacted with, its motion is applied to the target graphic. In the case where [`M_HANDLE_TARGET_LABEL`](../../Reference/3dgra/M3dgraControl.md) is set to [`M_NO_LABEL`](../../Reference/3dgra/M3dgraControl.md), the handle graphic is modified instead. You can specify several different handles that are all children of one root node and have their motion applied to that shared node. Note that the target is not required to be the parent of the handle graphics.

The following animation shows how the handle graphics are manipulated with the mouse and how their motion is transmitted to the graphics in the center. In this case, the handles are all children of one root node, so their motion is transmitted to all other graphics in the shared root node.

*[Image: 3dgra_handle_animation]*

You can specify that a handle graphic is clickable. This is useful, for example, to create interactive buttons in a display that perform a user defined task. For more information see[Hooking a function to a 3D graphics list event](S08_Interactive_3D_graphics.md).

## Hooking a function to a 3D graphics list event

The Aurora Imaging Library 3D Graphics module allows you to hook a function to an editable graphic event using [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md) with [`M_EDITABLE_GRAPHIC_MODIFIED`](../../Reference/3dgra/M3dgraHookFunction.md); the associated function will automatically be triggered whenever the interactive graphic is modified. For example, the hooked function can use the new position and size of a box graphic to reposition and resize a box geometry object used for cropping, using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_GEOMETRY`](../../Reference/3dgra/M3dgraCopy.md).

For an example of using [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md) in an application with an interactive box graphic, see:

> **Code example:** [m3dgraInteractive.cpp](m3dgraInteractive.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

You can also hook a function to a handle graphic event using [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md) with [`M_HANDLE_GRAPHIC_CLICKED`](../../Reference/3dgra/M3dgraHookFunction.md), [`M_HANDLE_GRAPHIC_ROTATED`](../../Reference/3dgra/M3dgraHookFunction.md), or [`M_HANDLE_GRAPHIC_TRANSLATED`](../../Reference/3dgra/M3dgraHookFunction.md); the associated function will automatically be triggered whenever the handle graphic is clicked, rotated, or translated, respectively. Note that if the handle graphic has a target, [`M3dgraGetHookInfo`](../../Reference/3dgra/M3dgraGetHookInfo.md) will return information about the target graphic rather than the handle graphic that was manipulated with the mouse.
