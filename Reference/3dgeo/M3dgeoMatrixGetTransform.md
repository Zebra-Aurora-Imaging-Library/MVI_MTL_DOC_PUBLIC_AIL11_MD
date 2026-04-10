---
doctype: Reference
module: 3dgeo
function: M3dgeoMatrixGetTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoMatrixGetTransform"
---

# M3dgeoMatrixGetTransform

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

> Get the translation, rotation, or scale defined by a given transformation matrix object.

## Syntax

```c
void M3dgeoMatrixGetTransform(
    AIL_ID       Matrix3dgeoId,  //in
    AIL_INT64    InquireType,    //in
    AIL_DOUBLE * Param1Ptr,      //out
    AIL_DOUBLE * Param2Ptr,      //out
    AIL_DOUBLE * Param3Ptr,      //out
    AIL_DOUBLE * Param4Ptr,      //out
    AIL_INT64    ControlFlag     //in
)
```

## Description

This function retrieves the translation, rotation, or scale defined by a given transformation matrix object.

The three distinct rotations about the axes of the working coordinate system ([`M_ROTATION_...`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md)) are also known as roll, pitch, and yaw. You can determine the required rotation order by checking the convention used by your robot's manufacturer. Note that [`M_ROTATION_AXIS...`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) and [`M_ROTATION_QUATERNION`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) do not depend on the manufacturer.

Note that [`M3dgeoMatrixGetTransform`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) returns angles in degrees. However, unlike most other Aurora Imaging Library functions, this function returns angles following the right-hand grip rule around the axis of rotation; if you wrap your right hand around the axis of rotation, pointing your thumb in the positive direction of the axis, your fingers wrap in the direction of rotation. For example, a positive rotation around the Z-axis corresponds to a rotation that turns the positive X-axis toward the positive Y-axis.

All coordinates are expressed in world units in the working coordinate system.

## Parameters

### `Matrix3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

*For specifying the transformation matrix object identifier*

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of a 3D geometry object. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of transformation to retrieve from the given transformation matrix object.

### `Param1Ptr` *(out, *AIL_DOUBLE)*

Specifies the first address in which to return information about the transformation required.

### `Param2Ptr` *(out, *AIL_DOUBLE)*

Specifies the second address in which to return information about the transformation required.

### `Param3Ptr` *(out, *AIL_DOUBLE)*

Specifies the third address in which to return information about the transformation required.

### `Param4Ptr` *(out, *AIL_DOUBLE)*

Specifies the fourth address in which to return information about the transformation required.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the transformation type

Any parameters for which a value is not returned must be set to [`M_NULL`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md).

---

### `M_ROTATION_AXIS_ANGLE`

Retrieves the axis and angle of rotation. The axis of rotation is defined by a vector. The rotation angle is measured in the counter-clockwise direction around the axis of rotation, as per the right-hand rule.  *[Image: calAngleOfRotation.png]*  The axis of rotation is always normalized.

| Value | Description |
| --- | --- |
| `0 <= Value < 360` | Specifies the rotation angle, in degrees. |

---

### `M_ROTATION_AXIS_X`

Retrieves the normalized vector of the X-axis which results from applying the given transformation matrix to the working coordinate system's X-axis.

---

### `M_ROTATION_AXIS_Y`

Retrieves the normalized vector of the Y-axis which results from applying the given transformation matrix to the working coordinate system's Y-axis.

---

### `M_ROTATION_AXIS_Z`

Retrieves the normalized vector of the Z-axis which results from applying the given transformation matrix to the working coordinate system's Z-axis.

---

### `M_ROTATION_QUATERNION`

Retrieves the components of the rotation quaternion.  The quaternion is always normalized.

---

### `M_ROTATION_XYZ`

Retrieves the three distinct rotations about the axes of the working coordinate system in the following order: the X-axis, the Y-axis, and the Z-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the positive X-axis. |
| `270.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the negative X-axis. |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |

---

### `M_ROTATION_XZY`

Retrieves the three distinct rotations about the axes of the working coordinate system in the following order: the X-axis, the Z-axis, and the Y-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the positive Y-axis. |
| `270.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the negative Y-axis. |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |

---

### `M_ROTATION_YXZ`

Retrieves the three distinct rotations about the axes of the working coordinate system in the following order: the Y-axis, the X-axis, and the Z-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the positive Z-axis. |
| `270.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the negative Z-axis. |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |

---

### `M_ROTATION_YZX`

Retrieves the three distinct rotations about the axes of the working coordinate system in the following order: the Y-axis, the Z-axis, and the X-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the positive Y-axis. |
| `270.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees, when located between the positive X-axis and the negative Y-axis. |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |

---

### `M_ROTATION_ZXY`

Retrieves the three distinct rotations about the axes of the working coordinate system in the following order: the Z-axis, the X-axis, and the Y-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the positive Z-axis. |
| `270.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees, when located between the positive Y-axis and the negative Z-axis. |
| `0.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees. |

---

### `M_ROTATION_ZYX`

Retrieves the three distinct rotations about the axes of the working coordinate system in the following order: the Z-axis, the Y-axis, and the X-axis rotation.

| Value | Description |
| --- | --- |
| `0.0 <= Value < 360.0` | Specifies the Z-axis rotation, in degrees. |
| `0.0 <= Value <= 90.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the positive X-axis. |
| `270.0 <= Value < 360.0` | Specifies the Y-axis rotation, in degrees, when located between the positive Z-axis and the negative X-axis. |
| `0.0 <= Value < 360.0` | Specifies the X-axis rotation, in degrees. |

---

### `M_SCALE`

Retrieves the scale factor along each axis.

---

### `M_TRANSLATION`

Retrieves the translation along each of the axes of the working coordinate system.
