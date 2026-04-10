---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: 3D_Display_and_graphics_overview
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / 3D Display and graphics overview"
---

# 3D display and graphics overview

You can display point cloud containers, depth map containers, and fully corrected depth map image buffers in a window on the desktop using an Aurora Imaging Library 3D display. You can also display 3D annotations (3D graphics such as cubes, cylinders, axes and text) in a 3D display.

> **Note:** There are additional hardware requirements to use Aurora Imaging Library 3D displays. For more information, see [Hardware requirements](../C02_Building_an_application/S01_Requirements.md).

3D displays are rendered in real-time, meaning that you can manipulate the view to show point clouds and 3D annotations from any angle. By default, the view in a 3D display can be changed interactively using the mouse and keyboard. Optionally, you can use [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)to manually change the view within your application.

A 3D display shows everything in its default 3D graphics list. To show a 3D-displayable point cloud container, depth map container, or fully-corrected depth map image buffer in a 3D display, you must add it to any of that 3D display's associated graphics lists (either using[`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md), [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md), or [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md)). By default, any subsequent changes to the 3D-displayable container or fully-corrected depth map image buffer will be shown in the 3D display; [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md) used with [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md) adds an unlinked 3D-displayable container or fully-corrected depth map. A 3D-displayable point cloud or depth map can be included in the 3D graphics list of any number of 3D displays.

Unlike 2D displays, each 3D display has a default 3D graphics list that is allocated and freed along with the 3D display. You can add or remove a 3D graphics list from a 3D display that you have allocated yourself. You can copy 3D graphics from any 3D graphics list to any of the 3D display's associated 3D graphics lists using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md). A 3D graphics list can be associated with multiple displays at the same time.

A 3D graphics list has a tree structure of 3D graphics, at the top of which is an invisible 3D graphic called the root node. The position and orientation of a 3D graphic is determined by its transformation matrix, which is applied relative to its parent in the tree structure. When a parent 3D graphic is moved, all of its descendents are moved accordingly. The root node always has the identity transformation matrix. You must specify the parent 3D graphic when you create 3D graphics or add 3D-displayable containers or fully corrected depth map image buffers, using functions from the [`M3dgra...`](../../Reference/3dgra/M3dgraBox.md)module.
