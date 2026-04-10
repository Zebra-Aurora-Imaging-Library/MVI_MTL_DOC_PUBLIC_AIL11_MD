---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: Color_and_display_settings_for_3D_data
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / Color and display settings for 3D data"
---

# Color and display settings for 3D data

When you select a 3D-displayable point cloud container, depth map container, or fully-corrected depth map image buffer to a 3D display (using[`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md), [`M3ddispSelectWindow`](../../Reference/3ddisp/M3ddispSelectWindow.md), or [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md)), a linked point cloud 3D graphic with the graphic type [`M_GRAPHIC_TYPE_POINT_CLOUD`](../../Reference/3dgra/M3dgraInquire.md) is added to the 3D graphics list.

> **Note:** Fully-corrected depth map image buffers and depth map containers are internally converted to 3D-displayable point cloud containers when they are selected to a 3D display. The word points in this section can therefore also refer to the pixels of a fully-corrected depth map image buffer/container.

Unlike other types of 3D graphics, a point cloud 3D graphic is continually updated to reflect changes in the linked 3D-displayable container or depth map image buffer. You can change how a point cloud 3D graphic is shown in the display using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with the label of the point cloud 3D graphic. If a single 3D-displayable container or fully-corrected depth map image buffer is shown in multiple 3D displays (or multiple times in the same 3D display), you can change these settings independently for each point cloud 3D graphic.

Note that when using [`M3dgraAdd`](../../Reference/3dgra/M3dgraAdd.md), you can specify to create an unlinked copy of the 3D-displayable container or depth map image buffer by using [`M_NO_LINK`](../../Reference/3dgra/M3dgraAdd.md).

You can specify the displayed size and opacity of each point, and how the color of each point is determined (optionally by applying a LUT). If you select a point cloud container to a 3D display and it has a valid mesh component, you can also specify how the mesh is shown in the 3D display.

## Point size

Points are represented as squares in a 3D display. You can specify the displayed size of the points (in number of pixels) using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with [`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md). Points are always shown with sides of this length, regardless of how close they are in the view.

## Opacity

You can specify the opacity of the points using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with [`M_OPACITY`](../../Reference/3dgra/M3dgraControl.md). An opacity of 100 means that the point cloud 3D graphic is completely opaque, while an opacity of 0 means that the point cloud 3D graphic is completely transparent.

## Coloring points

If you select a point cloud or depth map container to a 3D display, its points are displayed with the color of their corresponding intensity or reflectance component value. You can specify to color the points based on a different component of the container using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md), or to use only a single band of a component using [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md). For example, if you specify to use the third band (Z-coordinates) of the range component, points that have higher Z-coordinates will be shown brighter. This can be useful for emphasizing height differences.

Points converted from a depth map image buffer are colored based on their height.

You can display points with a single solid color using [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) to[`M_NULL`](../../Reference/3dgra/M3dgraControl.md). All points will then have the color specified by [`M_COLOR`](../../Reference/3dgra/M3dgraControl.md).

### Dynamic range

In some cases, the data that you use to color your points might not use all of the available dynamic range. For example, the intensity component might only have values between 20 and 30, even if it is an 8-bit buffer than can hold 256 possible values. The difference between these grayscale values would be difficult to discern when shown in the 3D display. You can improve visibility of this difference by specifying the dynamic range of the point color values using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md). The source color values of each point will be normalized to this scale when determining what color to show (for example, if the maximum value is set to 30 then any intensity component value of 30 or higher will be shown as pure white).

The default value for [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) is [`M_BUFFER_LIMITS`](../../Reference/3dgra/M3dgraControl.md), which uses the full available dynamic range of the component. You can specify that Aurora Imaging Library will automatically determine the dynamic range from the extremes of the data by setting [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) to [`M_DATA_EXTREMES_GLOBAL`](../../Reference/3dgra/M3dgraControl.md)or [`M_DATA_EXTREMES_PER_BAND`](../../Reference/3dgra/M3dgraControl.md). If you change [`M_COLOR_LIMITS`](../../Reference/3dgra/M3dgraControl.md) to[`M_USER_DEFINED`](../../Reference/3dgra/M3dgraControl.md), you can use [`M_COLOR_LIMITS_MIN`](../../Reference/3dgra/M3dgraControl.md)and [`M_COLOR_LIMITS_MAX`](../../Reference/3dgra/M3dgraControl.md)to specify values manually.

> **Note:** You cannot manually specify a different minimum/maximum per band. To use different values for each band, you must specify[`M_DATA_EXTREMES_PER_BAND`](../../Reference/3dgra/M3dgraControl.md).

### Applying LUTs

You can use a LUT to color the points of a point cloud graphic. To quickly apply a LUT that maps the point cloud range component's Z-band to establish the color of the corresponding point, use [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md). Alternatively, enable [`M_COLOR_USE_LUT`](../../Reference/3dgra/M3dgraControl.md) using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md), and set the required component and band with [`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md) and [`M_COLOR_COMPONENT_BAND`](../../Reference/3dgra/M3dgraControl.md), respectively. The default LUT is [`M_COLORMAP_TURBO`](../../Reference/3dgra/M3dgraCopy.md). Other predefined LUTs are available, or you can specify a user-defined LUT buffer. To change the LUT used for a point cloud graphic, use either [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) or [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) with the point cloud graphic's label.

Note that [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) is useful for the most common scenario, that of applying color to the third band (Z-band) of the point cloud's range component. You cannot change the component or band that [`M3ddispLut`](../../Reference/3ddisp/M3ddispLut.md) uses; you can only specify which LUT and to which point cloud graphic(s) it applies. For any other settings, use [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) and [`M3dgraCopy`](../../Reference/3dgra/M3dgraCopy.md) as described above.

### Visualizing small local intensity variations

To visualize small local intensity variations and details in a point cloud or depth map, you can use a LUT that repeats its colormap pattern. In this case, it is best to use a cyclic colormap for a seamless transition between the repeating pattern. To generate the LUT, use [`MgenLutFunction`](../../Reference/gen/MgenLutFunction.md) (for example, with [`M_COLORMAP_WHEEL`](../../Reference/gen/MgenLutFunction.md)), and specify the number of times the basic pattern is to be repeated using the [`c`](../../Reference/gen/MgenLutFunction.md) parameter. You can then apply the cyclic LUT to the point cloud's range component or depth map.

The example _cyclicallut.cpp_ demonstrates how to use a cyclic LUT to enhance a low contrast depth map image where the label and bar code are initially imperceptible. This example uses a **rule of thumb** for selecting the number of times the basic pattern is to be repeated when generating the cyclic LUT (taking 32 divided by the local intensity difference between the foreground and background of the image). Note that some experimentation is often required to obtain the best results.

*[Image: colormap_dimm.png]*

It can also be beneficial to exclude invalid pixels when remapping the depth map. This can be done by specifying a neutral value when generating the cyclic LUT, using [`MgenLutFunction`](../../Reference/gen/MgenLutFunction.md) with [`M_COLORMAP_WHEEL`](../../Reference/gen/MgenLutFunction.md) +[`M_LAST_GRAY`](../../Reference/gen/MgenLutFunction.md).

To run the _cyclicallut.cpp_ example, and many others, use the Aurora Imaging Example Launcher.

## Meshed point cloud containers

You can display a 3D-displayable meshed point cloud container as a full 3D model, or as a standard point cloud. By default, triangles in the mesh are shown as filled polygons in the 3D display. If there are points that are not part of the mesh, those points are still shown as squares.

You can change how mesh data in the point cloud container is shown using[`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md)with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md):

- You can specify that triangles in the mesh are shown as filled polygons in the 3D display, using [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md).
  If a color component is set for the 3D graphic (using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with[`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md)) the triangles of the mesh are colored by interpolating between the colors of their vertices. Otherwise, the color of all of the triangles is set using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with[`M_FILL_COLOR`](../../Reference/3dgra/M3dgraControl.md).
- You can specify that edges of the mesh are shown as lines in the 3D display, using [`M_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).
  If a color component is set for the 3D graphic (using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with[`M_COLOR_COMPONENT`](../../Reference/3dgra/M3dgraControl.md)) the lines of the wireframe are colored by interpolating between the colors of their vertices. Otherwise, the color of all of the lines is set using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with[`M_COLOR`](../../Reference/3dgra/M3dgraControl.md). The lines are always shown with the thickness (in pixels) indicated by[`M_THICKNESS`](../../Reference/3dgra/M3dgraControl.md), regardless of how close they are in the view.
- You can specify that triangles and edges of the mesh are shown as filled polygons and wireframes simultaneously in the 3D display, using [`M_SOLID_WITH_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md).
  The filled polygons are shown with the visible edges of the wireframe on top. The color and thickness settings used with [`M_SOLID`](../../Reference/3dgra/M3dgraControl.md) and [`M_WIREFRAME`](../../Reference/3dgra/M3dgraControl.md)are also used with this mode.
- You can specify that the mesh is ignored, using [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md). The point cloud is shown as though it has no associated mesh information.
