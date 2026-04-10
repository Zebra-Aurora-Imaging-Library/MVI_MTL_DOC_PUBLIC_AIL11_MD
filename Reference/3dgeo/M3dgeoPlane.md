---
doctype: Reference
module: 3dgeo
function: M3dgeoPlane
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoPlane"
---

# M3dgeoPlane

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

> Define a 3D plane geometry object.

## Syntax

```c
void M3dgeoPlane(
    AIL_ID     Geometry3dgeoId,  //out
    AIL_INT64  CreationMode,     //in
    AIL_DOUBLE X1,               //in
    AIL_DOUBLE Y1,               //in
    AIL_DOUBLE Z1,               //in
    AIL_DOUBLE X2OrD,            //in
    AIL_DOUBLE Y2,               //in
    AIL_DOUBLE Z2,               //in
    AIL_DOUBLE X3,               //in
    AIL_DOUBLE Y3,               //in
    AIL_DOUBLE Z3,               //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function defines a 3D plane geometry object. You can translate, rotate, or transform the resulting plane, using the 3D image processing module.

You can use the resulting plane to, for example, crop a point cloud or perform an arithmetic operation on a depth map using the 3D image processing module, or calculate its distance from each point in a point cloud using the 3D metrology module.

If you want to define a plane geometry object from results obtained in a different module, you can use the copy function of that module.

All coordinates are expressed in world units in the working coordinate system.

> **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoPlane.md) specifies a previously-defined plane, and [`CreationMode`](../../Reference/3dgeo/M3dgeoPlane.md) is set to [`M_POINT_AND_NORMAL`](../../Reference/3dgeo/M3dgeoPlane.md), you can leave the components of the plane's normal vector unchanged, even if the plane was modified using the 3D image processing module.

## Parameters

### `Geometry3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object, previously allocated on the required system using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `CreationMode` *(in, AIL_INT64)*

Specifies how the plane is defined.

### `X1` *(in, AIL_DOUBLE)*

Specifies the first parameter used to define the plane.

### `Y1` *(in, AIL_DOUBLE)*

Specifies the second parameter used to define the plane.

### `Z1` *(in, AIL_DOUBLE)*

Specifies the third parameter used to define the plane.

### `X2OrD` *(in, AIL_DOUBLE)*

Specifies the fourth parameter used to define the plane.

### `Y2` *(in, AIL_DOUBLE)*

Specifies the fifth parameter used to define the plane.

### `Z2` *(in, AIL_DOUBLE)*

Specifies the sixth parameter used to define the plane.

### `X3` *(in, AIL_DOUBLE)*

Specifies the seventh parameter used to define the plane.

### `Y3` *(in, AIL_DOUBLE)*

Specifies the eighth parameter used to define the plane.

### `Z3` *(in, AIL_DOUBLE)*

Specifies the ninth parameter used to define the plane.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the plane

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoPlane.md).

---

### `M_COEFFICIENTS`

Specifies to define the plane using coefficients from the plane equation (`Ax + By + Cz + D = 0`).  Coefficients _A_, _B_, and _C_ cannot all be zero.  > **Note:** Note that the coefficients will be internally normalized such that `A^2 + B^2 + C^2 = 1`.

---

### `M_POINT_AND_NORMAL`

Specifies to define the plane using a point on the plane and the plane's normal.  The plane's normal must have a non-zero magnitude.  > **Note:** Note that the plane's normal will be internally converted to a unit vector.  > **Note:** Note that if [`Geometry3dgeoId`](../../Reference/3dgeo/M3dgeoPlane.md) specifies a previously-defined plane, you can leave the components of its normal vector unchanged, even if that attribute was set using a different creation mode or was modified using the 3D image processing module. To do so, set the corresponding parameter to [`M_UNCHANGED`](../../Reference/3dgeo/M3dgeoPlane.md).

| Value | Description |
| --- | --- |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the X-component of the normal. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the Y-component of the normal. |
| `M_UNCHANGED` | Specifies to use the previously-defined value. |
| `Value` | Specifies the Z-component of the normal. |

---

### `M_POINT_AND_TWO_VECTORS`

Specifies to define the plane using a point on the plane and two vectors parallel to the plane.  The two vectors must have non-zero magnitudes and cannot be co-linear.

---

### `M_THREE_POINTS`

Specifies to define the plane using three points on the plane.
