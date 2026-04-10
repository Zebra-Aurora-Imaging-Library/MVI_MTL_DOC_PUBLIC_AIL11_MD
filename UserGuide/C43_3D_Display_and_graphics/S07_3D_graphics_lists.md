---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: 3D_graphics_lists
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / 3D graphics lists"
---

# 3D graphics lists

You can annotate a 3D display by adding 3D graphics to its 3D graphics list, using functions from the [`M3dgra...`](../../Reference/3dgra/M3dgraBox.md)module.

> **Note:** You can also allocate a 3D graphics list that is not associated with a 3D display using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md).

## Structure of a 3D graphics list

A 3D graphics list has a node-based tree structure; each 3D graphic (except the root node) has a parent 3D graphic that partially defines its position and orientation. You can also use the tree structure to control multiple 3D graphics with a single call to [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md), copy multiple 3D graphics with a single call to[`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md), and remove multiple 3D graphics with a single call to [`M3dgraRemove`](../../Reference/3dgra/M3dgraRemove.md).

The top of every 3D graphics list tree structure is an [`M_GRAPHIC_TYPE_NODE`](../../Reference/3dgra/M3dgraInquire.md) 3D graphic called the root node. The root node always has the identity transformation matrix.

When you add a 3D graphic to a 3D graphics list, you must specify the label of the parent graphic. You can use [`M_ROOT_NODE`](../../Reference/3dgra/M3dgraBox.md) to specify the root node.

> **Note:** A point cloud 3D graphic added to the 3D graphics list using [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md)is always added as a child of the root node.

### Controlling settings for multiple 3D graphics

You can control the settings for a 3D graphic and all of its descendents using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with [`M_RECURSIVE`](../../Reference/3dgra/M3dgraCopy.md). This is useful for creating groups of 3D graphics that should have the same settings. For example, you can create a node type graphic using [`M3dgraNode`](../../Reference/3dgra/M3dgraNode.md), then label the displayed data by creating text 3D graphics (using [`M3dgraText`](../../Reference/3dgra/M3dgraText.md)) that are children of the node 3D graphic. You can enable or disable visibility for all of these labels at once, using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with the label of the node 3D graphic, [`M_VISIBLE`](../../Reference/3dgra/M3dgraControl.md), and [`M_RECURSIVE`](../../Reference/3dgra/M3dgraControl.md). Note that this works even though the node 3D graphic itself does not support the [`M_VISIBLE`](../../Reference/3dgra/M3dgraControl.md)setting.

### Copying multiple 3D graphics

You can copy a 3D graphic and/or all of its descendents using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md)with [`M_GRAPHIC`](../../Reference/3dgra/M3dgraCopy.md), the label of the source 3D graphic to copy, and the label of the destination 3D graphic (in the same 3D graphics list, or a different one) to be the parent of the copy.

*[Image: 3dgra_Copy_SingleGraphic.png]*

If you specify [`M_RECURSIVE`](../../Reference/3dgra/M3dgraCopy.md), all descendents of the source 3D graphic are also copied. The hierarchy of the source 3D graphics is maintained in the copies; the source 3D graphic is copied as a child of the destination 3D graphic, and the children of the source 3D graphic are copied as children of that copy.

*[Image: 3dgra_Copy_Recursive.png]*

If you specify [`M_CHILDREN_ONLY`](../../Reference/3dgra/M3dgraCopy.md), only the descendents of the source 3D graphic are copied. The children of the source 3D graphic are copied as children of the destination 3D graphic; the hierarchy is maintained for their descendents.

*[Image: 3dgra_Copy_ChildrenOnly.png]*

### Removing multiple 3D graphics

You can remove a 3D graphic using [`M3dgraRemove`](../../Reference/3dgra/M3dgraRemove.md). This also removes all of the 3D graphic's descendents.

### Changing the positions of multiple 3D graphics

You can change the transformation matrix of a 3D graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md). The position and orientation of a 3D graphic is determined by adding its transformation matrix to the transformation matrix of its ancestors. You can therefore change the position and/or orientation of multiple 3D graphics by altering the transformation matrix of their parent. For example, you can add the 3D graphics used to annotate a particular point cloud as children of the point cloud 3D graphic. If you change the transformation matrix of the point cloud 3D graphic, all of the annotations automatically move with it.

### Render layers

Normally, 3D objects are rendered with proper respect for depth; that is, if one object is in front of another, it will properly occlude the other object. However, sometimes it is necessary to ensure that a 3D object is always drawn on top of the others, regardless of the actual position of each object. This is useful, for example, to create label text that is always visible in the 3D display, regardless of the view of the scene. This is achieved by putting different objects on different "layers", where graphics on higher levels are always drawn completely in front of graphics on lower layers. You can specify which layer a graphic is rendered on, using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with[`M_RENDER_LAYER`](../../Reference/3dgra/M3dgraControl.md). Graphics on the same layer are rendered with proper respect for depth.

## Setting defaults for 3D graphics added to a 3D graphics list

You can control the default values for 3D graphics added to a 3D graphics list using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraControl.md). Any 3D graphics that you subsequently add to the 3D graphics list will be created with these settings.

You can set the default colormap LUT of the 3D graphics list using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_DEFAULT_SETTINGS`](../../Reference/3dgra/M3dgraCopy.md). For example, you can copy the [`M_COLORMAP_HOT`](../../Reference/3dgra/M3dgraCopy.md) colormap into the 3D graphics list so that subsequent point cloud graphics will, by default, use this colormap. If you change the default, it will not affect the 3D graphics that are already in the 3D graphics list.
