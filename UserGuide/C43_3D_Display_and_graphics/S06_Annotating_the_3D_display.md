---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: Annotating_the_3D_display
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / Annotating the 3D display"
---

# Annotating the 3D display

You can annotate a 3D display by adding 3D graphics to its 3D graphics list or adding external 3D graphics lists, using functions from the [`M3dgra...`](../../Reference/3dgra/M3dgraBox.md)module. A 3D graphic is a fully 3D object that has a position and orientation in the scene, defined by its transformation matrix and the transformation matrix of its parent graphic.

When you create a 3D graphic, you must add it to a 3D graphics list as a child of another 3D graphic. For more information on 3D graphics lists and the 3D graphics hierarchy, see[3D graphics lists](S07_3D_graphics_lists.md).

You can remove an existing 3D graphic using [`M3dgraRemove`](../../Reference/3dgra/M3dgraRemove.md).

> **Note:** Note, the 3D graphics module will perform[`M3dimFix`](../../Reference/3dim/M3dimFix.md) with [`M_RANGE_FINITE`](../../Reference/3dim/M3dimFix.md), [`M_NORMALS_FINITE`](../../Reference/3dim/M3dimFix.md), [`M_MESH_VALID_POINTS`](../../Reference/3dim/M3dimFix.md) for point clouds. This does not modify the source point cloud.

## Types of graphics

You can add the following types of 3D graphics:

| Types of Aurora Imaging Library 3D graphics |
| --- |
| A point cloud graphic, generated from a 3D-displayable point cloud container, depth map container, or fully corrected depth map image buffer, using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md), [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md), or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md). By default, it is linked to the container or image buffer. *[Image: 3dgra_ExampleObject_PointCloud.png]* | A meshed point cloud graphic, generated from a meshed 3D-displayable point cloud container, using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md), [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md), or [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md). By default, it is linked to the container. *[Image: 3dgra_ExampleObject_PointCloudMeshed.png]* | A dots graphic, generated from coordinate arrays, using [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md). Note that a dots graphic is not generated from a container or image buffer. *[Image: 3dgra_ExampleObject_Dots.png]* |
| A box, using [`M3dgraBox`](../../Reference/3dgra/M3dgraBox.md). *[Image: 3dgra_ExampleObject_Box.png]* | A cylinder, using [`M3dgraCylinder`](../../Reference/3dgra/M3dgraCylinder.md). *[Image: 3dgra_ExampleObject_Cylinder.png]* | A sphere, using [`M3dgraSphere`](../../Reference/3dgra/M3dgraSphere.md). *[Image: 3dgra_ExampleObject_Sphere.png]* |
| An axis, using [`M3dgraAxis`](../../Reference/3dgra/M3dgraAxis.md). *[Image: 3dgra_ExampleObject_Axis.png]* | A line, using [`M3dgraLine`](../../Reference/3dgra/M3dgraLine.md). *[Image: 3dgra_ExampleObject_Line.png]* | One or more lines using [`M3dgraLines`](../../Reference/3dgra/M3dgraLines.md) *[Image: 3dgra_ExampleObject_Lines.png]* |
| An arc, using [`M3dgraArc`](../../Reference/3dgra/M3dgraArc.md). *[Image: graphicArc.png]* | A filled arc, using [`M3dgraArcFill`](../../Reference/3dgra/M3dgraArcFill.md). *[Image: graphicArcFill.png]* | A plane, using [`M3dgraPlane`](../../Reference/3dgra/M3dgraPlane.md). *[Image: 3dgra_ExampleObject_Plane.png]* |
| A grid, using [`M3dgraGrid`](../../Reference/3dgra/M3dgraGrid.md), with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md)set to [`M_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md). *[Image: 3dgra_ExampleObject_EmptyGrid.png]* | A filled grid, using [`M3dgraGrid`](../../Reference/3dgra/M3dgraGrid.md), with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md)set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md). *[Image: 3dgra_ExampleObject_FilledGrid.png]* | A polygon, using [`M3dgraPolygon`](../../Reference/3dgra/M3dgraPolygon.md). *[Image: 3dgra_ExampleObject_Polygon.png]* |
| A textured polygon, using [`M3dgraPolygon`](../../Reference/3dgra/M3dgraPolygon.md). *[Image: 3dgra_ExampleObject_PolygonTextured.png]* | A textured polygon with portions cut-out, using [`M3dgraPolygon`](../../Reference/3dgra/M3dgraPolygon.md)with an [`M_KEYING_COLOR`](../../Reference/3dgra/M3dgraControl.md)specified. *[Image: 3dgra_ExampleObject_PolygonTexturedCutout.png]* | 3D text, using [`M3dgraText`](../../Reference/3dgra/M3dgraText.md). *[Image: 3dgra_ExampleObject_Text.png]* |
| 3D fixed rotation text, using [`M3dgraText`](../../Reference/3dgra/M3dgraText.md) with [`M_FIXED_ROTATION`](../../Reference/3dgra/M3dgraText.md). An example of this behavior can be seen in the animation below. *[Image: 3dgra_3DTextFixedRotation]* | 3D fixed scale text, using [`M3dgraText`](../../Reference/3dgra/M3dgraText.md) with [`M_FIXED_SCALE`](../../Reference/3dgra/M3dgraText.md). An example of this behavior can be seen in the animation below. *[Image: 3dgra_3DTextFixedScale]* | A rectangle, using [`M3dgraRect`](../../Reference/3dgra/M3dgraRect.md) *[Image: 3dgra_ExampleObject_Plane.png]* |

You can also add a node, an invisible 3D graphic that you can use to organize other 3D graphics in the 3D graphics list, using [`M3dgraNode`](../../Reference/3dgra/M3dgraNode.md).

## Positions of 3D graphics

Every 3D graphic has a transformation matrix that determines the position and rotation of the 3D graphic in the 3D display. Some functions that create 3D graphics require you to specify the transformation matrix, while others determine the transformation matrix from another property that you must specify (such as the start position and end position of a line 3D graphic). Some 3D graphics (such as point clouds) are always created with the identity transformation matrix.

You can change the transformation matrix of an existing 3D graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgra/M3dgraCopy.md).

Each 3D graphic has a local coordinate system, with a local origin. For example, the local origin of a box 3D graphic is always at the center of the box, and its coordinate system is rotated to match the box's orientation. When you create a new 3D graphic, you must specify its position and rotation relative to the parent. If the new 3D graphic's position and rotation are specified by a transformation matrix, the transformation matrix that you specify will be applied relative to the parent's transformation matrix. If the new 3D graphic's position and rotation are specified by other properties (such as X, Y, and Z-coordinates), those properties are interpreted within the coordinate system of the parent 3D graphics object. Any subsequent changes to the transformation matrix of the parent 3D graphic also change the position and orientation of the child 3D graphic.

When you set the transformation matrix of a 3D graphic using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md), you can specify to use the passed transformation matrix to transform the 3D graphic within the absolute coordinate system, instead of within the coordinate system of the parent 3D graphic. To do this, specify [`M_RELATIVE_TO_ROOT`](../../Reference/3dgra/M3dgraCopy.md). Aurora Imaging Library calculates the correct transformation matrix within the coordinate system of the parent so that the 3D graphic has the specified transformation matrix in the absolute coordinate system.

> **Note:** The local origin and orientation of a 3D graphic's coordinate system is always determined by its transformation matrix. Changing the transformation matrix of the 3D graphic also alters the local origin and orientation of its coordinate system.

To determine the local origin and orientation of a particular type of 3D graphic, refer to the function used to create it.

## Picking 3D graphics

You might want to obtain information about 3D graphics at a particular position or within a particular area of the 3D display. In these cases, you can use [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) or [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md), respectively, to detect (pick) the graphics. To do so, first allocate a picking context, using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) with [`M_PICKING_CONTEXT`](../../Reference/3ddisp/M3ddispAlloc.md) (for an [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) operation) or [`M_PICKING_AREA_CONTEXT`](../../Reference/3ddisp/M3ddispAlloc.md) (for an [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) operation). Then, allocate the appropriate 3D picking result buffer using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md). After picking the 3D graphic(s), you can retrieve results (using [`M3ddispGetResult`](../../Reference/3ddisp/M3ddispGetResult.md)), such as the number of 3D graphics picked ([`M_NUMBER`](../../Reference/3ddisp/M3ddispGetResult.md)), a picked 3D graphic's type ([`M_GRAPHIC_TYPE`](../../Reference/3ddisp/M3ddispGetResult.md)), or its label ([`M_LABEL`](../../Reference/3ddisp/M3ddispGetResult.md)).

For [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) results, you can also retrieve:

- The pixel or 3D position ([`M_PICKED_POINT_PIXEL_...`](../../Reference/3ddisp/M3ddispGetResult.md) or[`M_PICKED_POSITION_3D_...`](../../Reference/3ddisp/M3ddispGetResult.md), respectively).
- The distance from the view point ([`M_DISTANCE`](../../Reference/3ddisp/M3ddispGetResult.md)).
- The index of the picked face, if a box or meshed point cloud graphic was picked ([`M_FACE`](../../Reference/3ddisp/M3ddispGetResult.md)).
- The index of the picked sub-element ([`M_GRAPHIC_SUB_INDEX`](../../Reference/3ddisp/M3ddispGetResult.md)), such as the index of the picked dot in a detected dots 3D graphic.

For [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) results, you can also retrieve information about the sub-elements of a picked 3D graphic, if a dots, lines, or point cloud graphic was detected. Sub-elements are the parts of the 3D graphic that were located within the rectangular picking region during the call to [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md). For example, if the picking region covered only a portion of a point cloud graphic, the whole 3D graphic is picked, but only points within the picking region are considered sub-elements. You can retrieve the number of picked sub-elements ([`M_GRAPHIC_SUB_INDEX_NUMBER`](../../Reference/3ddisp/M3ddispGetResult.md)) or an array of sub-element indices ([`M_GRAPHIC_SUB_INDICES`](../../Reference/3ddisp/M3ddispGetResult.md)).

Note that, when using [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) to detect 3D graphics, you can set a region that encapsulates the 3D display in its entirety, a portion of it, or none of it at all. Any 3D graphics located within (or partly within) the picking region will be picked, including those outside the visible portion of the 3D display.

*[Image: 3ddisp_Picking_Region_OutOfBounds.png]*

You can interactively pick 3D graphics, using [`M3ddispGetHookInfo`](../../Reference/3ddisp/M3ddispGetHookInfo.md) to get the position of the mouse when an [`M_MOUSE_LEFT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md) event is generated (left mouse button clicked). You can, for instance, pick two points in the display and measure the distance between them. For more information, see the [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) and [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) function descriptions, or see [Examples](S06_Annotating_the_3D_display.md).

### Display settings for a picking context

Picking contexts have settings that determine what is picked in the display. You can change these settings using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md). You can specify the following settings to limit what can be picked:

- What type of graphics can be picked, using [`M_GRAPHIC_TYPE`](../../Reference/3ddisp/M3ddispControl.md). For example, if you only want to pick rectangle graphics, set [`M_GRAPHIC_TYPE`](../../Reference/3ddisp/M3ddispControl.md) to [`M_GRAPHIC_TYPE_RECT`](../../Reference/3ddisp/M3ddispControl.md).
- Whether points can be picked, using [`M_POINTS`](../../Reference/3ddisp/M3ddispControl.md). This only applies to point cloud graphics, dots graphics, and graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md).
- Whether faces and lines can be picked, using [`M_POINTS`](../../Reference/3ddisp/M3ddispControl.md). This only applies to point cloud graphics, dots graphics, and graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) or [`M_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).
- The required label for a 3D graphic to be detected, using [`M_LABEL`](../../Reference/3ddisp/M3ddispControl.md). All other graphics are filtered out and are not pickable.
- On which render layers a 3D graphic can be detected by the pick operation, using [`M_RENDER_LAYER_MASK`](../../Reference/3ddisp/M3ddispControl.md).
- The minimum opacity required for a 3D graphic to be detected, using [`M_OPACITY_THRESHOLD`](../../Reference/3ddisp/M3ddispControl.md).

For [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) operations only, you can specify the following:

- Whether the 3D graphics are blocked from being picked if the closest 3D graphic is filtered out, using [`M_BLOCKABLE`](../../Reference/3ddisp/M3ddispControl.md).
- The radius around the specified position for the pick operation to search, using [`M_SELECTION_RADIUS`](../../Reference/3ddisp/M3ddispControl.md).

### Examples

For an example of using [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) in an application, see _Multi3dDisplaySharedGraphicsList.cpp_.

For examples of interactively picking 3D graphics using [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md), see _M3ddispPicking.cpp_ and _M3ddispFacePicking.cpp_.

For an example of interactively picking 3D graphics using [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md), see _M3ddispAreaPicking.cpp_.

To run these and other examples, use Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

## Using a 2D graphics list with the 3D display

You can associate a 2D graphics list with the 3D display. To do so, allocate the list using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), then use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_ASSOCIATED_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispControl.md) to specify the 2D graphics list's identifier. You can use the 2D graphics list for display annotations, such as a heads-up display (HUD), or to create and add 2D graphics interactively. For example, you can interactively draw a rectangular picking region for detecting 3D graphics using [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md).

When annotating a display using a 2D graphics list, you can set the opacity of the annotations. To do so, use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_GRAPHIC_LIST_OPACITY`](../../Reference/3ddisp/M3ddispControl.md).

By default, modifications to the graphics within the associated 2D graphics list (added, deleted, or altered graphics) are immediately reflected in the 3D display. To change this behavior, use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_UPDATE_GRAPHIC_LIST`](../../Reference/3ddisp/M3ddispControl.md). To release the 2D graphics list from memory, use [`MgraFree`](../../Reference/gra/MgraFree.md). If the associated 2D graphics list is freed, it is automatically disassociated from the 3D display.

When working interactively, use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_INTERACTIVE_INPUT_MODE`](../../Reference/3ddisp/M3ddispControl.md) to set how the 3D display handles user inputs. The default mode is [`M_NORMAL_3D`](../../Reference/3ddisp/M3ddispControl.md), which is the standard 3D mode in which mouse movements and keystrokes manipulate the 3D view. To send mouse and keyboard inputs to the associated 2D graphics list, set the mode to [`M_ASSOCIATED_GRAPHIC_LIST_2D`](../../Reference/3ddisp/M3ddispControl.md). In this mode, you can use (for example) [`MgraInteractive`](../../Reference/gra/MgraInteractive.md) to add a rectangle to the 3D display without disturbing underlying 3D graphics.

See [Creating and modifying graphics interactively](../C26_Generating_graphics/S07_Creating_and_modifying_graphics_interactively.md) for more information on how to use a 2D graphics list interactively. Also see [Examples](S06_Annotating_the_3D_display.md).

## Display settings for 3D graphics

Each 3D graphic has settings that determine how it is shown in the 3D display. You can change these settings using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md). You can specify the following for most 3D graphics:

- How the polygons of the 3D graphic are displayed (if the 3D graphic is polygonal), using [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md). You can specify to show filled polygons, a wireframe, filled polygons with a wireframe on top, or only the vertices that define the polygons.
- The color of the points/dots and lines of the 3D graphic, using[`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).
- The thickness (in pixels) of the points/dots and lines, using[`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md). Points/dots are shown as squares; this setting determines the length of each side.
- The color of the faces (polygons) of the 3D graphic, using [`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md). This setting applies to most 3D graphics when [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) is set to [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md)or [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).
- How complex the automatically generated mesh of the 3D graphic is, using [`M_GRAPHIC_RESOLUTION`](../../Reference/3dgra/M3dgraControl.md). A higher resolution 3D graphic is displayed using more polygons. This makes the surface of the 3D graphic appear smoother. Displaying a large number of high-resolution 3D graphics might require a more powerful GPU to maintain acceptable performance.
- The opacity of the 3D graphic, using[`M_OPACITY`](../../Reference/3dgra/M3dgraControl.md). An opacity of 100 means that the 3D graphic is completely opaque, while an opacity of 0 means that the 3D graphic is completely transparent.
- What type of shading (if any) is used to render the 3D graphic, using[`M_SHADING`](../../Reference/3dgra/M3dgraControl.md)(or [`M_TEXT_SHADING`](../../Reference/3dgra/M3dgraControl.md)for text 3D graphics). Shaded 3D graphics are shown brighter or darker depending on the viewing angle, giving a visual indication of depth relative to the view in the 3D display. Different types of shading use different models to determine the brightness of each position on the surface of the 3D graphic. In general, this changes how smooth the surface of the 3D graphic appears to be.
- Whether the 3D graphic is shown in the 3D display, using[`M_VISIBLE`](../../Reference/3dgra/M3dgraControl.md).

## Clipping 3D graphics that extend infinitely

Some types of 3D graphics (such as planes and cylinders) can be added with settings such that they extend infinitely. Every 3D graphics list has a clipping box that limits the size of these 3D graphics. For example, if you add a plane 3D graphic with [`Size`](../../Reference/3dgra/M3dgraPlane.md) set to [`M_INFINITE`](../../Reference/3dgra/M3dgraPlane.md) the created plane is clipped to the area that is within the current clipping box of the 3D graphics list.

> **Note:** Changes to the clipping box of a 3D graphics list are not applied retroactively to existing 3D graphics, only to 3D graphics added subsequently.

By default the clipping box of a 3D graphics list is equal to the bounding box of all 3D graphics currently in the list. You can set the clipping box of a 3D graphics list manually using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md)with [`SrcAilObjectId`](../../Reference/3dgra/M3dgraCopy.md)set to the Aurora Imaging Library identifier of a box 3D geometry object (created using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md)and defined using [`M3dgeoBox`](../../Reference/3dgeo/M3dgeoBox.md)). You can restore the default behavior using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md)with [`SrcAilObjectId`](../../Reference/3dgra/M3dgraCopy.md)set to [`M_WHOLE_SCENE`](../../Reference/3dgra/M3dgraCopy.md).

### Clipping options

For an infinite cylinder, line, or plane, you can set how the 3D graphic is clipped with respect to the clipping box, using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_CLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md). The default setting is [`M_CLIP_TO_BOX`](../../Reference/3dgra/M3dgraControl.md), which truncates the graphic to fit within the clipping box. Alternatively, you can specify [`M_CLIP_TO_PROJECTION`](../../Reference/3dgra/M3dgraControl.md). In this mode, the infinite graphic is clipped to a projection of the clipping box.

For example, if an infinite plane intersects the clipping box at an angle and the clipping mode is set to [`M_CLIP_TO_BOX`](../../Reference/3dgra/M3dgraControl.md), the plane is clipped to a polygon with 3 to 6 sides, depending on where it intersects the box. When the clipping mode is set to [`M_CLIP_TO_PROJECTION`](../../Reference/3dgra/M3dgraControl.md), the plane is clipped to a rectangle, with bounds that encapsulate a projection of the clipping box onto the plane. To visualize the operation, imagine dropping verticals to the plane from each box corner (that is, orthographically projecting the box's corner points to the plane). These locations establish the region to which the plane is clipped, resulting in a clipped plane whose edges run parallel to the plane's local coordinate axes.

The following animation depicts a clipping box with an infinite plane graphic, displayed successively in [`M_CLIP_TO_PROJECTION`](../../Reference/3dgra/M3dgraControl.md) and [`M_CLIP_TO_BOX`](../../Reference/3dgra/M3dgraControl.md) modes.

*[Image: 3ddisp_ClippingMode_Plane]*

When interactively manipulating (moving or rotating) an infinite plane graphic, its orientation with respect to the clipping box can change. Use [`M_RECLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md) to configure whether to clip the plane again (reclip) after each manipulation ([`M_RECLIP_ON_HANDLE_RELEASE`](../../Reference/3dgra/M3dgraControl.md), the default mode) or to never automatically reclip the plane ([`M_NEVER_AUTO_RECLIP`](../../Reference/3dgra/M3dgraControl.md)).

To manually reclip an infinite cylinder, line, or plane, call [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_RECLIP`](../../Reference/3dgra/M3dgraControl.md).

When reclipping (either through [`M_RECLIP_ON_HANDLE_RELEASE`](../../Reference/3dgra/M3dgraControl.md) or [`M_RECLIP`](../../Reference/3dgra/M3dgraControl.md)), the specified clipping mode ([`M_CLIPPING_MODE`](../../Reference/3dgra/M3dgraControl.md)) defines the clipping behavior.

## Working with texture images of polygon 3D graphics

You can choose to give a polygon 3D graphic an image buffer as a texture when you add it to a 3D graphics list using [`M3dgraPolygon`](../../Reference/3dgra/M3dgraPolygon.md), or using [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md)with [`M_COLOR_TEXTURE`](../../Reference/3dgra/M3dgraCopy.md). If you set[`M_COLOR_USE_TEXTURE`](../../Reference/3dgra/M3dgraControl.md)is set to [`M_TRUE`](../../Reference/3dgra/M3dgraControl.md), the polygon is shown with its texture instead of a solid color. Optionally, you can use [`M_KEYING_COLOR`](../../Reference/3dgra/M3dgraControl.md)to specify that one color value in the texture is instead shown as transparent.

### Texture mapping

When you add a polygon graphic with a texture to a 3D graphics list using [`M3dgraPolygon`](../../Reference/3dgra/M3dgraPolygon.md), you can optionally specify the texture coordinates used to map the texture onto the polygon. If you do not specify texture coordinates, Aurora Imaging Library will determine the texture mapping automatically. Typically, you should only use automatic texture mapping for polygons that are rectangles or right-angle triangles with the same aspect ratio as the texture. Otherwise, the texture will be displayed with distortion, as shown in this image.

*[Image: 3dgra_ExamplesOfAutomaticTextureMapping.png]*

For cases where automatic texture mapping produces warping, you can use texture coordinates to manually specify what position in the texture image buffer each vertex is mapped to. For example, if the first texture coordinate is (5,12), the first vertex is mapped to the center of the pixel at position (5,12) in the texture image buffer, and so on. The faces of the polygon are colored by interpolating between the specified positions. This animation shows how different texture coordinates affect how the texture is shown on an isosceles triangle polygon.

*[Image: 3dgra_TextureMappingAnimation]*
