---
doctype: Reference
module: 3dgeo
function: M3dgeoBox
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoBox"
---

# M3dgeoBox

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

> Define a 3D box geometry object.

## Syntax

```c
void M3dgeoBox(
    AIL_ID     Geometry3dgeoId,  //out
    AIL_INT64  CreationMode,     //in
    AIL_DOUBLE XPos1,            //in
    AIL_DOUBLE YPos1,            //in
    AIL_DOUBLE ZPos1,            //in
    AIL_DOUBLE XPos2OrLength,    //in
    AIL_DOUBLE YPos2OrLength,    //in
    AIL_DOUBLE ZPos2OrLength,    //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function defines an axis-aligned, 3D box geometry object. You can get or set the 3D box orientation using [`M3dgeoCopy`](../../Reference/3dgeo/M3dgeoCopy.md). You can translate, rotate, scale, or transform the resulting box, using the 3D image processing module.

You can use the resulting box to, for example, crop a point cloud or perform an arithmetic operation on a depth map using the 3D image processing module, or calculate its distance from each point in a point cloud using the 3D metrology module.

If you want to define a box geometry object from results obtained in a different module, you can use the copy function of that module.

All coordinates are expressed in world units in the working coordinate system.

> **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoBox.md) specifies a previously-defined box, and [`CreationMode`](../../Reference/3dgeo/M3dgeoBox.md) is set to [`M_CENTER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md), you can leave some of the box's attributes unchanged, even if that attribute was set using a different creation mode or was modified using the 3D image processing module. Unless you specify [`M_ORIENTATION_UNCHANGED`](../../Reference/3dgeo/M3dgeoBox.md), its orientation will be reset to the identity matrix.

## Parameters

### `Geometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `CreationMode` *(in, AIL_INT64)*

Specifies how the box is defined.

### `XPos1` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the first point used to define the box.

### `YPos1` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the first point used to define the box.

### `ZPos1` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the first point used to define the box.

### `XPos2OrLength` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the second point used to define the box, or the length of the box along the X-axis.

### `YPos2OrLength` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the second point used to define the box, or the length of the box along the Y-axis.

### `ZPos2OrLength` *(in, AIL_DOUBLE)*

Specifies the Z-coordinate of the second point used to define the box, or the length of the box along the Z-axis.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the box

---

### `M_BOTH_CORNERS`

Defines the box using any two opposite corners.

---

### `M_CENTER_AND_DIMENSION`

Defines the box by its center point and its length along each axis.  You can determine the coordinates of the corners of the box by taking a coordinate from the center point and adding or subtracting half the box's length, along the coordinate's axis. For example, the Y-coordinates of the corners of the box are [`YPos1`](../../Reference/3dgeo/M3dgeoBox.md) ± ([`YPos2OrLength`](../../Reference/3dgeo/M3dgeoBox.md)/2). You can also use [`M3dgeoInquire`](../../Reference/3dgeo/M3dgeoInquire.md) with [`M_CORNER_...`](../../Reference/3dgeo/M3dgeoInquire.md).  > **Note:** Note that the box's size along each axis, [`M_SIZE_X`](../../Reference/3dgeo/M3dgeoInquire.md), [`M_SIZE_Y`](../../Reference/3dgeo/M3dgeoInquire.md), and [`M_SIZE_Z`](../../Reference/3dgeo/M3dgeoInquire.md), will be the absolute values of the corresponding lengths specified by [`M_CORNER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md), [`M_CORNER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md), and [`M_CORNER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md).  > **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoBox.md) specifies a previously-defined box, you can leave some of its attributes unchanged, even if that attribute was set using a different creation mode or was modified using the 3D image processing module. To do so, set the corresponding parameter to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoBox.md).

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the box center's X-coordinate. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the box center's Y-coordinate. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the box center's Z-coordinate. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the box's length along the X-axis. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the box's length along the Y-axis. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the box's length along the Z-axis. |

---

### `M_CORNER_AND_DIMENSION`

Defines the box by one of its corners and its length (or negative length) along each axis.  A positive value for length extends the box in the direction of the positive axis, starting from the specified corner point. A negative value extends the box in the direction of the negative axis, starting at the specified corner point. For example, if the specified corner is (1, 1, 1) and the value of the X, Y, and Z lengths are all positive 3 (a cube), the coordinates of the corner opposite the specified corner would be (4, 4, 4). If the X length was -3, while the other two lengths remained positive 3 (still a cube), the coordinates of the corner opposite the specified corner would be (-2, 4, 4).  > **Note:** Note that the box's size along each axis, [`M_SIZE_X`](../../Reference/3dgeo/M3dgeoInquire.md), [`M_SIZE_Y`](../../Reference/3dgeo/M3dgeoInquire.md), and [`M_SIZE_Z`](../../Reference/3dgeo/M3dgeoInquire.md), will be the absolute values of the corresponding lengths specified by [`M_CORNER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md), [`M_CORNER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md), and [`M_CORNER_AND_DIMENSION`](../../Reference/3dgeo/M3dgeoBox.md).

### Combination Constants — For specifying to leave the orientation unchanged

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify to leave the orientation of the previously-defined box unchanged.

| Value | Description |
| --- | --- |
| `M_ORIENTATION_UNCHANGED` | Specifies to leave the orientation of the previously-defined box unchanged. |
