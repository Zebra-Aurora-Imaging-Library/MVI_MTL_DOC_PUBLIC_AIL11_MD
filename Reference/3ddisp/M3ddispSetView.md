---
doctype: Reference
module: 3ddisp
function: M3ddispSetView
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispSetView"
---

# M3ddispSetView

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

> Sets the display's view.

## Syntax

```c
void M3ddispSetView(
    AIL_ID     Disp3dId,    //out
    AIL_INT64  Mode,        //in
    AIL_DOUBLE Param1,      //in
    AIL_DOUBLE Param2,      //in
    AIL_DOUBLE Param3,      //in
    AIL_INT64  ControlFlag  //in
)
```

## Description

This function changes the position and orientation of the view of the specified 3D display.

> **Note:** Note that, by default, you can also change the position of orientation of the view interactively using the keyboard and mouse. You can enable or disable the keyboard and mouse controls using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md)with [`M_KEYBOARD_USE`](../../Reference/3ddisp/M3ddispControl.md)and [`M_MOUSE_USE`](../../Reference/3ddisp/M3ddispControl.md).

This function changes the view of the 3D display by modifying the viewpoint, interest point, and/or up vector, as shown in the image below:

*[Image: 3ddisp_ViewDefinition.png]*

You can set the position of the viewpoint using [`M_VIEWPOINT`](../../Reference/3ddisp/M3ddispSetView.md), the position of the interest point using [`M_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md)and the direction of the up vector using [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md). All other modes are alternatives to adjusting these aspects of the view directly. For example, setting the [`M_ROLL`](../../Reference/3ddisp/M3ddispSetView.md) setting of the view is an alternate method to change the[`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md)setting.

Most modes are specified in absolute terms (for example, using [`M_AZIMUTH`](../../Reference/3ddisp/M3ddispSetView.md)with the value 90 sets the azimuth to 90, regardless of the current value). In some cases, you can instead specify to apply the operation relative to the current state of the view using [`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md). For example, if you use [`M_AZIMUTH`](../../Reference/3ddisp/M3ddispSetView.md) with[`M_COMPOSE_WITH_CURRENT`](../../Reference/3ddisp/M3ddispSetView.md)and the value 90, and the current azimuth is 30, the new azimuth is 120.

Modes that can be performed by moving either the viewpoint or the interest point will always move the viewpoint, unless you use the combination value [`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md). For example, if you use [`M_AZIMUTH`](../../Reference/3ddisp/M3ddispSetView.md) with the value 45, by default the viewpoint is moved to the position that is rotated 45 degrees around the interest point (relative to the plane that intersects the interest point and is parallel to the ZY-plane), effectively circling the view around the interest point. If you specify [`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md), instead the interest point is moved to the position in which the viewpoint is rotated 45 degrees around the interest point, effectively panning the view. Both modes result in the same orientation (the viewpoint is rotated 45 degrees around the interest point); however, in the first case the viewpoint has a new position, and in the second case the interest point has a new position.

Optionally, you can specify not to refresh the rendered image shown in the 3D display by setting the [`ControlFlag`](../../Reference/3ddisp/M3ddispSetView.md) parameter to[`M_NO_REFRESH`](../../Reference/3ddisp/M3ddispSetView.md). This [`ControlFlag`](../../Reference/3ddisp/M3ddispSetView.md) setting only guarantees that this operation will not trigger a refresh of the rendered image. It is possible for other events in the 3D display to trigger a refresh, including events which occur before or during the call to [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md). If you need to guarantee that there is no refresh between a sequence of operations, use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_UPDATE`](../../Reference/3ddisp/M3ddispControl.md) to disable and enable rendering.

## Parameters

### `Disp3dId` *(out, AIL_ID)*

Specifies the identifier of the 3D display.

### `Mode` *(in, AIL_INT64)*

Specifies the mode used for setting the view.

### `Param1` *(in, AIL_DOUBLE)*

Specifies the first parameter used to set the view.

### `Param2` *(in, AIL_DOUBLE)*

Specifies the second parameter used to set the view.

### `Param3` *(in, AIL_DOUBLE)*

Specifies the third parameter used to set the view.

### `ControlFlag` *(in, AIL_INT64)*

Specifies if the 3D display should be refreshed.

*For specifying if the display should be updated*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to trigger a refresh of the 3D display after the operation is complete. |
| `M_NO_HOOK` | Prevents generating an [`M_VIEW_CHANGE`](../../Reference/3ddisp/M3ddispHookFunction.md) event in response to the call to [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md).

If you want to prevent more hooks from being generated when calling [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) with [`M_VIEW_CHANGE`](../../Reference/3ddisp/M3ddispHookFunction.md) for another 3D display (to which you are hooked). |
| `M_NO_REFRESH` | Specifies not to trigger a refresh of the 3D display after the operation is complete. If you need to guarantee that there is no refresh between a sequence of operations, use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_UPDATE`](../../Reference/3ddisp/M3ddispControl.md) to disable and enable rendering. |

## Parameter Associations

### For setting the view

---

### `M_DEFAULT`

Same as [`M_AUTO`](../../Reference/3ddisp/M3ddispSetView.md).

---

### `M_AUTO`

Sets the viewpoint and interest point position so that a specific 3D graphic, and all of its descendants, are completely within the view. This is referred to as focusing the view on the 3D graphic. By default, the view is focused on the root node (this guarantees that everything in the 3D display's 3D graphics list is completely within the view).  Optionally, you can also specify to set the orientation of the view to a preset.

| Value | Description |
| --- | --- |
| *(see [`M_VIEW_ORIENTATION`](Reference/3ddisp/M3ddispSetView.md))* |  |
| `M_DEFAULT` | Specifies to preserve the current orientation of the view. |
| `M_DEFAULT` | Specifies to focus the view on the 3D graphic that you set using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md)with[`M_VIEW_BOX_NODE`](../../Reference/3ddisp/M3ddispControl.md).  By default, this is set to 0 (the root node). |
| `Value >= 0` | Specifies the label of the 3D graphic (in the 3D display's 3D graphics list) on which to focus the view. |

---

### `M_AZIM_ELEV_ROLL`

Sets the azimuth and elevation of the viewpoint relative to the interest point, and the roll of the view. These are the viewpoint position and up vector, described in a spherical coordinate system (additionally described by[`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)).  Azimuth is the angle of the viewpoint relative to the interest point on the XY-plane. An azimuth of 0 corresponds to a view where the viewpoint and interest point are parallel to the X-axis on the XY-plane, with the viewpoint on the positive side. Increasing values for the azimuth correspond to rotating the view counter-clockwise around the interest point.  Elevation is the angle of the viewpoint relative to the interest point on the XZ-plane. An elevation of 0 corresponds to a view where the viewpoint and interest point are parallel to the X-axis on the XZ-plane, with the viewpoint on the positive side. Increasing values for the elevation correspond to rotating the view counter-clockwise around the interest point in the XZ-plane.  Roll is the angle that describes how much the view is rotated to the right. A roll of 0 corresponds to a view for which up is positive on the Z-axis.  *[Image: 3ddisp_AzimElevRoll_General.png]*

---

### `M_AZIMUTH`

Sets the azimuth of the view.

---

### `M_DISTANCE`

Sets the distance between the viewpoint and the interest point.

---

### `M_ELEVATION`

Sets the elevation of the view relative to the interest point.

---

### `M_FLIP`

Sets the view to a position and orientation that is flipped around the interest point in the specified axis.

| Value | Description |
| --- | --- |
| `M_AXIS_X` | Specifies to flip the viewpoint around the interest point in the specified axis. |
| `M_AXIS_Y` | Specifies to flip the viewpoint around the interest point in the specified axis. |
| `M_AXIS_Z` | Specifies to flip the viewpoint around the interest point in the specified axis. |

---

### `M_INTEREST_POINT`

Sets the position of the interest point.

---

### `M_ORBIT_HORIZONTAL`

Sets the angle of the viewpoint's horizontal orbit around the interest point. This angle is used relative to the [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md) of the view. For more information, see [Orbit, up-vector, and distance](../../UserGuide/C43_3D_Display_and_graphics/S05_Manipulating_the_view.md).  *[Image: 3ddisp_Orbit_Horizontal.png]*  > **Note:** Note that orbit is always specified relative to the current position of the view. For example, repeatedly using this function with [`M_ORBIT_HORIZONTAL`](../../Reference/3ddisp/M3ddispSetView.md) and the value 90 will repeatedly orbit the viewpoint horizontally around the interest point, in 90 degree increments.

---

### `M_ORBIT_VERTICAL`

Sets the angle of the viewpoint's vertical orbit around the interest point. This angle is used relative to the [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md) of the view. For more information, see [Orbit, up-vector, and distance](../../UserGuide/C43_3D_Display_and_graphics/S05_Manipulating_the_view.md).  *[Image: 3ddisp_Orbit_Vertical.png]*  > **Note:** Note that orbit is always specified relative to the current position of the view. For example, repeatedly using this function with [`M_ORBIT_VERTICAL`](../../Reference/3ddisp/M3ddispSetView.md) and the value 90 will repeatedly orbit the viewpoint vertically around the interest point, in 90 degree increments.

---

### `M_ROLL`

Sets the roll of the view. This is an angle which defines what direction the top of the view will be pointing, with 0 degrees specifying that the view will be perfectly level. As this value increases, the view rolls to the right.

---

### `M_TRANSLATE`

Applies a translation to the viewpoint and interest point without altering their relative position and orientation.

---

### `M_UP_VECTOR`

Sets the vector which defines what direction the top of the view is pointing.  > **Note:** Note that this setting is normalized and adjusted to a unit vector that is orthogonal to the current orientation of the view. If you use[`M3ddispGetView`](../../Reference/3ddisp/M3ddispGetView.md) with [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispGetView.md) after setting the up vector, the returned value will therefore typically be different from the vector you specified.

---

### `M_VIEW_BOX`

Sets the viewpoint and interest point (without altering the orientation) such that the interest point is at the center of the box and the entire region defined by the box is within view.  *[Image: 3ddisp_viewBox_General.png]*

| Value | Description |
| --- | --- |
| `M_WHOLE_SCENE` | Specifies to set the viewpoint and interest point so that everything in the scene is within the view. |
| `3D box geometry object ID` | Specifies the identifier of the 3D box geometry object used to set the 3D display's view. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the scale factor. The position for the viewpoint is calculated as though the box is scaled by this factor. |

---

### `M_VIEW_MATRIX`

Sets the view to match a transformation matrix. If the transformation matrix is the identity matrix, the viewpoint will be at (0,0,1), with the interest point at the origin (0,0,0) and an up-vector of (0,1,0). This can also be thought of as a direct top-down view, with the up-vector aligned with the Y-axis.  > **Note:** You must specify the Aurora Imaging Library identifier of a similarity transformation matrix. You can determine whether a transformation matrix is a similarity transformation matrix using[`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_SIMILARITY`](../../Reference/3dgeo/M3dgeoInquire.md).

| Value | Description |
| --- | --- |
| `MatrixId` | Specifies the Aurora Imaging Library identifier of a similarity transformation matrix (previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md)). |

---

### `M_VIEW_ORIENTATION`

Sets the direction in which the view is looking. This moves the viewpoint (or interest point if combined with [`M_MOVE_INTEREST_POINT`](../../Reference/3ddisp/M3ddispSetView.md)) so that line of sight matches a given orientation, specified using either a predefined orientation or a vector that defines the direction from the viewpoint to the interest point.

| Value | Description |
| --- | --- |
| `M_BOTTOM_TILTED` | Specifies to set the orientation of the view to an interpolation between the bottom, front, and left views. |
| `M_BOTTOM_VIEW` | Specifies to set the orientation of the view so that the viewpoint is directly below the interest point (parallel to the Z-axis of the working coordinate system of the 3D display). |
| `M_FRONT_VIEW` | Specifies to set the orientation of the view so that the viewpoint is directly in front of the interest point (parallel to the Y-axis of the working coordinate system of the 3D display). |
| `M_LEFT_VIEW` | Specifies to set the orientation of the view so that the viewpoint is directly to the left of the interest point (parallel to the X-axis of the working coordinate system of the 3D display). |
| `M_REAR_VIEW` | Specifies to set the orientation of the view so that the viewpoint is directly behind the interest point (parallel to the Y-axis of the working coordinate system of the 3D display). |
| `M_RIGHT_VIEW` | Specifies to set the orientation of the view so that the viewpoint is directly to the right of the interest point (parallel to the X-axis of the working coordinate system of the 3D display). |
| `M_TOP_TILTED` | Specifies to set the orientation of the view to an interpolation between the top, front, and left views. |
| `M_TOP_VIEW` | Specifies to set the orientation of the view so that the viewpoint is directly above the interest point (parallel to the Z-axis of the working coordinate system of the 3D display). |
| `Value` | Specifies the X-component of the vector that defines the orientation of the view. |

---

### `M_VIEWPOINT`

Sets the position of the view.

---

### `M_ZOOM`

Zooms the view in or out by a factor. This is equivalent to dividing[`M_DISTANCE`](../../Reference/3ddisp/M3ddispSetView.md)by the specified factor.

### Combination Constants — Specifies whether the motion is absolute or relative.

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the viewpoint is moved relative to the current position and orientation of the viewpoint..

| Value | Description |
| --- | --- |
| `M_COMPOSE_WITH_CURRENT` | Specifies that the view is altered relative to the current position and orientation of the view. |

### Combination Constants — Specifies whether the interest point should move

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the interest point is moved instead of the viewpoint..

| Value | Description |
| --- | --- |
| `M_MOVE_INTEREST_POINT` | Specifies that the interest point is moved instead of the viewpoint. |
