---
doctype: Reference
module: 3ddisp
function: M3ddispGetView
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispGetView"
---

# M3ddispGetView

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

> Get information about the 3D display's view.

## Syntax

```c
void M3ddispGetView(
    AIL_ID       Disp3dId,    //in
    AIL_INT64    Mode,        //in
    AIL_DOUBLE * Param1Ptr,   //out
    AIL_DOUBLE * Param2Ptr,   //out
    AIL_DOUBLE * Param3Ptr,   //out
    AIL_INT64    ControlFlag  //in
)
```

## Description

This function gets information about the current view of the specified 3D display.

All settings of the view of a 3D display are based on the viewpoint, interest point, and/or up vector, as shown in the image below:

*[Image: 3ddisp_ViewDefinition.png]*

You can get the position of the viewpoint using [`M_VIEWPOINT`](../../Reference/3ddisp/M3ddispGetView.md), the position of the interest point using [`M_INTEREST_POINT`](../../Reference/3ddisp/M3ddispGetView.md)and the direction of the up-vector using [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispGetView.md). The results of all other modes are derived from these modes. For example, the [`M_ROLL`](../../Reference/3ddisp/M3ddispGetView.md)setting of the view is derived from the [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispGetView.md)setting.

All results are returned with respect to the working coordinate system of the 3D display.

## Parameters

### `Disp3dId` *(in, AIL_ID)*

Specifies the identifier of the 3D display.

### `Mode` *(in, AIL_INT64)*

Specifies which aspect of the view to inquire.

### `Param1Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write either the X-coordinate, distance, azimuth, elevation, or roll.

### `Param2Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write either the Y-coordinate or elevation.

### `Param3Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write either the Z-coordinate or roll.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For determining the position and orientation of the viewpoint.

---

### `M_AZIM_ELEV_ROLL`

Gets the azimuth and elevation of the viewpoint relative to the interest point, and the roll of the view. These are the viewpoint position and up vector, described in a spherical coordinate system (additionally described by[`M_DISTANCE`](../../Reference/3ddisp/M3ddispGetView.md)).  Azimuth is the angle of the viewpoint relative to the interest point on the XY-plane. An azimuth of 0 corresponds to a view where the viewpoint and interest point are parallel to the X-axis on the XY-plane, with the viewpoint on the positive side. Increasing values for the azimuth correspond to rotating the view counter-clockwise around the interest point.  Elevation is the angle of the viewpoint relative to the interest point on the XZ-plane. An elevation of 0 corresponds to a view where the viewpoint and interest point are parallel to the X-axis on the XZ-plane, with the viewpoint on the positive side. Increasing values for the elevation correspond to rotating the view counter-clockwise around the interest point in the XZ-plane.  Roll is the angle that describes how much the view is rotated to the right. A roll of 0 corresponds to a view for which up is positive on the Z-axis.  *[Image: 3ddisp_AzimElevRoll_General.png]*

---

### `M_AZIMUTH`

Gets the azimuth of the viewpoint relative to the interest point.  Azimuth is the angle of the viewpoint relative to the interest point on the XY-plane. An azimuth of 0 corresponds to a view where the viewpoint and interest point are parallel to the X-axis on the XY-plane, with the viewpoint on the positive side. Increasing values for the azimuth correspond to rotating the view counter-clockwise around the interest point.

---

### `M_DISTANCE`

Gets the distance of the viewpoint from the interest point.

---

### `M_ELEVATION`

Gets the elevation of the viewpoint relative to the interest point.  Elevation is the angle of the viewpoint relative to the interest point on the XZ-plane. An elevation of 0 corresponds to a view where the viewpoint and interest point are parallel to the X-axis on the XZ-plane, with the viewpoint on the positive side. Increasing values for the elevation correspond to rotating the view counter-clockwise around the interest point in the XZ-plane.

---

### `M_INTEREST_POINT`

Gets the position of the interest point.

---

### `M_ROLL`

Gets the roll of the view. This is an angle which defines what direction the top of the view will be pointing, with 0 degrees specifying that the view will be perfectly level. As this value increases, the view rolls to the right.

---

### `M_UP_VECTOR`

Gets the vector which defines what direction the top of the view is pointing.  > **Note:** Note that this setting will always return a unit vector that is orthogonal to the current orientation of the view. The value returned by this function will therefore typically be different from the up vector that you might have previously specified using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) with [`M_UP_VECTOR`](../../Reference/3ddisp/M3ddispSetView.md).

---

### `M_VIEW_ORIENTATION`

Gets the unit vector that describes the direction from the viewpoint to the interest point.

---

### `M_VIEWPOINT`

Gets the position of the viewpoint.
