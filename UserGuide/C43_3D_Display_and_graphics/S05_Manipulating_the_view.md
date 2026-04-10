---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Display_and_graphics
section: Manipulating_the_view
module_tag: 3ddisp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Display_and_graphics / Manipulating the view"
---

# Manipulating the view

Aurora Imaging Library 3D graphics are 3D shapes that can be viewed from any angle (this includes 3D graphics associated with depth map image buffers/containers and 3D-displayable point cloud containers). A 3D graphics list is a collection of these 3D shapes. When a 3D display is shown, a specific view of the contents of the associated 3D graphics lists are rendered on the screen. You can manipulate the view of a 3D display, either interactively using the keyboard and mouse or within your application using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) and[`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md).

> **Note:** You can change the keyboard control bindings, using[`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_ACTION_KEY_...`](../../Reference/3ddisp/M3ddispControl.md). You can change the keyboard control bindings to increase or decrease the point size of all point clouds currently in the display, using [`M_FUNCTION_KEY_POINTSIZE_INCREMENT`](../../Reference/3ddisp/M3ddispControl.md) and [`M_FUNCTION_KEY_POINTSIZE_INCREMENT`](../../Reference/3ddisp/M3ddispControl.md) and change the keyboard control bindings to cycle the appearance of the point clouds using [`M_FUNCTION_KEY_CYCLE_APPEARANCES`](../../Reference/3ddisp/M3ddispControl.md). You can disable the ability to manipulate the view interactively with the keyboard or mouse, using [`M_KEYBOARD_USE`](../../Reference/3ddisp/M3ddispControl.md)/[`M_MOUSE_USE`](../../Reference/3ddisp/M3ddispControl.md), respectively. Additionally, you can partially disable mouse interactions with [`M_MOUSE_...`](../../Reference/3ddisp/M3ddispControl.md). For example, you can disable rotation with the mouse with [`M_MOUSE_ROTATION`](../../Reference/3ddisp/M3ddispControl.md) and disable translation in the Y-direction using [`M_MOUSE_TRANSLATION_Y`](../../Reference/3ddisp/M3ddispControl.md).

## View settings

The view is defined by 4 fundamental settings, set using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) and [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md). All other operations that you can perform using[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) are alternate methods for changing the fundamental settings. For example, the [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md)operation modifies the view's [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md)control type setting.

### Fundamental settings that control the view position and rotation

The following fundamental view settings are available with[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md).

- [`M_VIEWPOINT`](../../Reference/3ddisp/M3ddispSetView.md): The position from which the view is looking.
- [`M_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md): The position the view is looking at. This point is always at the center of the window.
- [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md): The vector which defines what is "up" for the view. This vector is normalized and perpendicular to the view's line of sight. If you specify an up-vector that is not perpendicular to the line of sight, an appropriate, valid vector will automatically be calculated.
  These settings define the position and rotation of your view, as shown in the following image:
  *[Image: 3ddisp_ViewDefinition.png]*
- [`M_FOV_HORIZONTAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md)/[`M_FOV_VERTICAL_ANGLE`](../../Reference/3ddisp/M3ddispControl.md): The angles which determine how wide the view is. Increasing one of these angles has an effect similar to using a wider angle lens on a camera. To prevent distortion, Aurora Imaging Library automatically updates these angles to have the same ratio as the horizontal and vertical resolution of the window. For example, if the user increases the horizontal window size, the horizontal FoV will automatically be increased proportionally.
  *[Image: 3ddisp_FoVAngleDefinition.png]*

## Alternative view settings

You can use operations in[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)to define the viewpoint, interest point, and up-vector based on alternative models. You can use any combination of these models to manipulate the view in your 3D display. For example, you can set the initial view using [`M_VIEW_BOX`](../../Reference/3ddisp/M3ddispSetView.md), and then use [`M_AZIMUTH`](../../Reference/3ddisp/M3ddispSetView.md)to rotate the viewpoint around the interest point.

If one of the operations described below could achieve the specified result by moving either the viewpoint or interest point (for example, [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md)), the viewpoint will be moved unless the combination value [`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md) is used.

### View orientation and distance

You can define the view by specifying the orientation of the viewpoint relative to the interest point using the [`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispSetView.md) operation. Either choose from the preset orientations or specify your own orientation vector.

You can select a preset orientation using keyboard keys or using[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) (for example, with [`M_BOTTOM_TILTED`](../../Reference/3ddisp/M3ddispSetView.md)). The available preset orientations are as follows:

| Preset view | Keyboard key | Use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with |
| --- | --- | --- |
| Bottom tilted | `1` | [`M_BOTTOM_TILTED`](../../Reference/3ddisp/M3ddispSetView.md) |
| Bottom | `2` | [`M_BOTTOM_VIEW`](../../Reference/3ddisp/M3ddispSetView.md) |
| Front | `3` | [`M_FRONT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md) |
| Left | `4` | [`M_LEFT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md) |
| Rear | `5` | [`M_REAR_VIEW`](../../Reference/3ddisp/M3ddispSetView.md) |
| Right | `6` | [`M_RIGHT_VIEW`](../../Reference/3ddisp/M3ddispSetView.md) |
| Top tilted | `7` | [`M_TOP_TILTED`](../../Reference/3ddisp/M3ddispSetView.md) |
| Top | `8` | [`M_TOP_VIEW`](../../Reference/3ddisp/M3ddispSetView.md) |

> **Note:** You can enable or disable the keyboard controls using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md)with [`M_KEYBOARD_USE`](../../Reference/3ddisp/M3ddispControl.md).

Alternatively, you can specify your own orientation vector. The vector that you specify is normalized and multiplied by [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md), and the view is changed so that the viewpoint is offset from the interest point by the resulting vector. Similarly, you can use [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md) to change the distance between the viewpoint and interest point without changing the view orientation.

### Zooming

Zooming moves the viewpoint towards the interest point (zooming in) or moves the viewpoint away from the interest point (zooming out). The following is a list of default keyboard and mouse bindings that allow you to perform zooming or reset the view to the default position and orientation. The table also shows the constants that you can use with [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) to perform these actions programmatically.

| Action | Keyboard button | Mouse control | Use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with |
| --- | --- | --- | --- |
| Zoom in/out | `+` or `-` | Roll up/down with mouse wheel | [`M_ZOOM`](../../Reference/3ddisp/M3ddispSetView.md) or [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md) |
| Reset view | `Home` key | None | None |

> **Note:** The action of zooming will affect the value returned by [`M3ddispGetView`](../../Reference/3ddisp/M3ddispGetView.md) with [`M_DISTANCE`](../../Reference/3ddisp/M3ddispGetView.md).

### Azimuth, elevation, roll, and distance

You can define the view in a spherical model that is always aligned with the working coordinate system of the 3D display using operations such as [`M_AZIM_ELEV_ROLL`](../../Reference/3ddisp/M3ddispSetView.md). Using this model, the viewpoint is always a fixed distance (specified using [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)) from the interest point. [`M_AZIMUTH`](../../Reference/3ddisp/M3ddispSetView.md) is the rotation (between 0 and 360 degrees) around the interest point in the plane that intersects the interest point and is parallel to the XY-plane of the working coordinate system of the 3D display. [`M_ELEVATION`](../../Reference/3ddisp/M3ddispSetView.md) is the vertical rotation (between -90 and 90 degrees) around the interest point. [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md) is the rotation (between 0 and 360 degrees) of the view around pole described by the viewpoint and interest point. The up-vector is determined based on the elevation and roll.

*[Image: 3ddisp_AzimElevRoll_General.png]*

> **Note:** Note that for elevation settings that are close to 90 or -90, Aurora Imaging Library cannot deterministically calculate the azimuth and roll of the view. In this case, inquiring the azimuth or roll will return practically random values. Similarly, setting the azimuth or roll with the combination value[`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md)will not produce reliable results.

By moving the view point, you can roll the view left or right as well as rotate the view up, down, left or right. The following is a list of the default keyboard and mouse bindings that allow you to perform rolls and rotations. The table also shows the constants that you can use with [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) to perform these actions programmatically.

| Action | Keyboard button | Mouse control | Use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with |
| --- | --- | --- | --- |
| Roll left/right | `Left` or `Right` arrow `+Alt` | Hold mouse wheel and drag | [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md) |
| Rotate up/down | `Up` or `Down` arrow `+Alt``+Shift` | None | [`M_ELEVATION`](../../Reference/3ddisp/M3ddispSetView.md) |
| Rotate left/right | `Left` or `Right` arrow `+Alt``+Shift` | None | [`M_AZIMUTH`](../../Reference/3ddisp/M3ddispSetView.md) |

### Orbit, up-vector, and distance

You can define the view in a spherical model that is relative to the current position and orientation of the view using[`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md)and [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md). Using this model, the viewpoint is always a fixed distance (specified using [`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)) from the interest point. When you use [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md), the viewpoint orbits around the interest point in the direction of the up-vector by the specified number of degrees. When you use [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md), the viewpoint orbits around the interest point in the direction perpendicular the up-vector by the specified number of degrees.

Unlike azimuth and elevation, vertical and horizontal orbit are always specified relative to the current position and orientation of the view. For example, repeatedly using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md)and the value 90 will repeatedly orbit the viewpoint around the interest point, in 90 degree increments.

The following animation shows the effect of setting the horizontal and vertical orbit of the view, in relation to the up-vector and the working coordinate system of the 3D display.

*[Image: 3ddisp_Orbit_General]*

The following is a list of the default keyboard and mouse bindings that allow you to perform orbits. The table also shows the constants that you can use with [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) to perform these actions programmatically.

| Action | Keyboard button | Mouse control | Use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with |
| --- | --- | --- | --- |
| Orbit up/down | `Up` or `Down` arrow keys | Left-click and drag vertically | [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md) |
| Orbit left/right | `Left` or `Right` arrow keys | Left-click and drag horizontally | [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md) |

### Scrolling

Scrolling works by moving both the viewpoint and the interest point in a certain direction. The following is a list of scrolls that can be performed in the display and their corresponding default keyboard and mouse bindings. The table also shows the constants that you can use with [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) to perform these actions programmatically.

| Action | Keyboard button | Mouse control | Use[`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with |
| --- | --- | --- | --- |
| Scroll left/right | `Left` or `Right` arrow `+Shift` | Right-click and drag horizontally | [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md)(Param 1) |
| Scroll in/out | `Up` or `Down` arrow `+Alt` | None | [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md) (Param 2) |
| Scroll up/down | `Up` or `Down` arrow `+Shift` | Right-click and drag vertically | [`M_TRANSLATE`](../../Reference/3ddisp/M3ddispSetView.md) (Param 3) |

### View box

You can configure the view to show either the whole scene or only a specified area of it. To view the whole scene, use [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) with [`M_VIEW_BOX`](../../Reference/3ddisp/M3ddispSetView.md) set to[`M_WHOLE_SCENE`](../../Reference/3ddisp/M3ddispSetView.md). This establishes a hypothetical box that encompasses everything in the 3D display's 3D graphics list, and ensures that it is all visible. Alternatively, you can define the view to show only things within a specified area using the [`M_VIEW_BOX`](../../Reference/3ddisp/M3ddispSetView.md)operation and specifying a 3D box geometry object (previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) and, for example, defined using [`M3dgeoBox`](../../Reference/3dgeo/M3dgeoBox.md)).

The [`M_VIEW_BOX`](../../Reference/3ddisp/M3ddispSetView.md) operation keeps the viewpoint and interest point in the same orientation relative to each other ([`M_VIEW_ORIENTATION`](../../Reference/3ddisp/M3ddispGetView.md) does not change), but moves the viewpoint and interest point such that the interest point is at the center of the scene/box geometry object, and the viewpoint is the correct distance away to show the entire region defined by the box.

*[Image: 3ddisp_viewBox_General.png]*

### View matrix

You can define the view by specifying a transformation matrix using [`M_VIEW_MATRIX`](../../Reference/3ddisp/M3ddispSetView.md). If the transformation matrix is the identity matrix, the viewpoint will be at (0,0,1), with the interest point at the origin (0,0,0) and an up-vector of (0,1,0). This can also be thought of as a direct top-down view, with the up-vector aligned with the Y-axis.

You can store the current transformation matrix of the view using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md), and then restore the same view later using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md)with[`M_VIEW_MATRIX`](../../Reference/3ddisp/M3ddispSetView.md).

## Other 3D display controls

You can define other aspects of the 3D display, including the background, rotation indicator, whether the view rotates automatically, and the depth sorting mode.

### Changing the background

By default, the background of a 3D display is a vertical gradient with colors suitable for viewing most objects. Optionally, you can change the background settings using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md).

You can change the background mode with [`M_BACKGROUND_MODE`](../../Reference/3ddisp/M3ddispControl.md). You can specify to show the background as a solid color with the [`M_SINGLE_COLOR`](../../Reference/3ddisp/M3ddispControl.md)setting, or to show a background image using the [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispControl.md)setting.

You can change the colors of the background with the [`M_BACKGROUND_COLOR`](../../Reference/3ddisp/M3ddispControl.md) and [`M_BACKGROUND_COLOR_GRADIENT`](../../Reference/3ddisp/M3ddispControl.md)settings. When the background is a gradient, [`M_BACKGROUND_COLOR`](../../Reference/3ddisp/M3ddispControl.md)is the color at the top of the 3D display and[`M_BACKGROUND_COLOR_GRADIENT`](../../Reference/3ddisp/M3ddispControl.md) is the color at the bottom of the 3D display.

To set the background image, copy an 8-bit unsigned image buffer to the 3D display using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_BACKGROUND_IMAGE`](../../Reference/3ddisp/M3ddispCopy.md). If the background image has a different aspect ratio than the window, the image will be stretched in the 3D display. You can determine the size and number of bands of the current background image buffer using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md)with [`M_BACKGROUND_IMAGE_SIZE_...`](../../Reference/3ddisp/M3ddispInquire.md).

### Rotation indicator

You can enable a visual indicator of the orientation of the view, using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_ROTATION_INDICATOR`](../../Reference/3ddisp/M3ddispControl.md). The rotation indicator is always positioned at the interest point and is always rotated so that it is aligned with the working coordinate system of the 3D display.

### Auto-rotation

You can have the view automatically rotate around a specified axis, using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md). This is useful for showing a whole 3D scene without user input.

By default, the view auto-rotates around the first 3D graphic added to the internal 3D graphics list of the 3D display. You can change the auto-rotation axis using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md)with [`M_ROTATION_AXIS`](../../Reference/3ddisp/M3ddispCopy.md)and the Aurora Imaging Library identifier of a 3D line geometry object.

> **Note:** Note that both the viewpoint and the interest point move during auto-rotation. Some users might be disoriented by the view rotating around an axis that does not intersect the interest point, especially when the rotation indicator is enabled.

The following table shows the default keyboard binding that allows you to perform auto-rotation. It also shows the function and constant to use if you want to perform this action programmatically.

| Action | Keyboard button | Use[`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md)with |
| --- | --- | --- |
| Auto-rotate | `R` key | [`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md) |

### Level of detail

You can use levels of detail (LoDs) to change the number of points displayed in the 3D display. You can enable LoDs using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md) with [`M_VIEW_BASED_LOD`](../../Reference/3dgra/M3dgraControl.md). You can specify the number of degraded LoDs to generate with [`M_VIEW_BASED_LOD_LEVELS`](../../Reference/3dgra/M3dgraControl.md) and the maximum sample scale factor with [`M_VIEW_BASED_LOD_SAMPLE_FACTOR_MAX`](../../Reference/3dgra/M3dgraControl.md). For example, with 3 LoD levels and a maximum sample factor of 8, the display will generate 2 LoDs (in addition to the full LoD); one with approximately 1/8 the number of points as the source, and one with approximately 1/4 the number of points as the source. LoDs are shown when the 3D display is zoomed out to maintain a similar point density. LoDs are only used for 3D graphics with [`M_APPEARANCE`](../../Reference/3dgra/M3dgraControl.md) set to [`M_POINTS`](../../Reference/3dgra/M3dgraControl.md).

When using graphics with many points, manipulating the display can become unusably slow. Using LoDs can increase performance while interacting with a 3D display. You can use lower LoDs by setting the [`M_LOD_DEGRADE_MODE`](../../Reference/3ddisp/M3ddispControl.md). This also degrades the LoD while using automatic rotation ([`M_AUTO_ROTATE`](../../Reference/3ddisp/M3ddispControl.md)). LoDs are generated once per graphic, per display. If you show the same 3D graphic twice in one display, the LoDs are generated and stored twice. LoDs are regenerated any time the graphic, or one of the LoD related controls is modified. You can set how long after interacting with a display to show the full resolution LoD with [`M_LOD_TIME_TO_SHOW_DEGRADED`](../../Reference/3ddisp/M3ddispControl.md). Opacity is automatically adjusted to maintain a similar appearance. For example, if the opacity is 20 and the currently used LoD has half as many points as the native LoD, an opacity of 40 is used instead.

For an example of using levels of detail in an application, see:

> **Code example:** [m3dgraInteractive.cpp](m3dgraInteractive.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.

### Depth sorting

The depth sorting mode determines how Aurora Imaging Library resolves transparency errors related to semi-opaque (semi-transparent) 3D graphics that are shown in the 3D display.

You can set the depth sorting mode using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md), with [`M_TRANSPARENCY_SORT_MODE`](../../Reference/3ddisp/M3ddispControl.md). The default depth sorting mode is depth peeling ([`M_DEPTH_PEELING`](../../Reference/3ddisp/M3ddispControl.md)), which resolves most transparency sorting errors. GPU usage might increase when depth peeling is enabled and there is a single complex semi-opaque 3D graphic or multiple semi-opaque 3D graphics in the 3D graphics list. However, depth peeling generally does not increase GPU usage unless there are visible transparency errors to be resolved.

If enabling depth peeling does not provide acceptable performance, you can set [`M_TRANSPARENCY_SORT_MODE`](../../Reference/3ddisp/M3ddispControl.md) to [`M_FAST`](../../Reference/3ddisp/M3ddispControl.md) for fast depth sorting. Fast depth sorting offers maximum performance, but can exhibit significant visual errors when showing semi-opaque 3D graphics, particularly if they intersect with other graphics.

> **Note:** Note that depth sorting can alter the appearance of both opaque and semi-opaque co-planar surfaces (for example, if you create two plane graphics with the same position and normal).

## Hooking a function to a 3D display event

The 3D Display module allows you to hook a function to a 3D display event using [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md). Generally, 3D display events happen when the display is manipulated ([`M_VIEW_CHANGE`](../../Reference/3ddisp/M3ddispHookFunction.md)), the mouse is manipulated ([`M_MOUSE_...`](../../Reference/3ddisp/M3ddispHookFunction.md)), or when a key is pressed ([`M_KEY_...`](../../Reference/3ddisp/M3ddispHookFunction.md)) while the mouse cursor is on the display; the associated function will automatically be triggered whenever the 3D display event occurs.

### Safe mode

Occasionally, errors are generated by the graphics driver. In many cases, it is not possible for the application to progress if such an error is produced. In such cases, an unrecoverable error event is generated. You can use [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md) with [`M_ENTER_SAFE_MODE`](../../Reference/3ddisp/M3ddispHookFunction.md) to hook a function to this error event and enter safe mode. When in safe mode, all calls to the 3D Display module will generate an error (except [`M3ddispGetHookInfo`](../../Reference/3ddisp/M3ddispGetHookInfo.md) and [`M3ddispFree`](../../Reference/3ddisp/M3ddispFree.md)). When this event occurs, you should save your work and restart the application. While all 3D displays will enter safe mode, only the 3D display that encountered the error will generate this event.
