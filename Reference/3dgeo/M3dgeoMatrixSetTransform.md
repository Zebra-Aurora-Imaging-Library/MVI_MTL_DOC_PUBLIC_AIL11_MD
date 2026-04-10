---
doctype: Reference
module: 3dgeo
function: M3dgeoMatrixSetTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoMatrixSetTransform"
---

# M3dgeoMatrixSetTransform

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

> Set the transformation values of a transformation matrix object.

## Syntax

```c
void M3dgeoMatrixSetTransform(
    AIL_ID     Matrix3dgeoId,  //out
    AIL_INT64  TransformType,  //in
    AIL_DOUBLE Param1,         //in
    AIL_DOUBLE Param2,         //in
    AIL_DOUBLE Param3,         //in
    AIL_DOUBLE Param4,         //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function assigns a transformation to a transformation matrix object or composes a transformation with a transformation matrix object. The transformation can be defined as a translation, rotation, scale, or the result of a matrix composition.

The three distinct rotations about the axes of the working coordinate system ([`M_ROTATION_...`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)) are also known as roll, pitch, and yaw. You can determine the required rotation order by checking the convention used by your robot's manufacturer. Note that [`M_ROTATION_AXIS...`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) and [`M_ROTATION_QUATERNION`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) do not depend on the manufacturer.

Note that all angles should be given in degrees. However, unlike most other Aurora Imaging Library functions, angles are interpreted using the right-hand grip rule around the axis of rotation; if you wrap your right hand around the axis of rotation, pointing your thumb in the positive direction of the axis, your fingers wrap in the direction of rotation. For example, a positive rotation around the Z-axis corresponds to a rotation that turns the positive X-axis toward the positive Y-axis.

All coordinates are expressed in world units in the working coordinate system.

## Parameters

### `Matrix3dgeoId` *(out, AIL_ID)*

Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

### `TransformType` *(in, AIL_INT64)*

Specifies the type of transformation to apply.

### `Param1` *(in, AIL_DOUBLE)*

Specifies the first parameter used to define the transformation matrix.

### `Param2` *(in, AIL_DOUBLE)*

Specifies the second parameter used to define the transformation matrix.

### `Param3` *(in, AIL_DOUBLE)*

Specifies the third parameter used to define the transformation matrix.

### `Param4` *(in, AIL_DOUBLE)*

Specifies the fourth parameter used to define the transformation matrix.

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to apply the transformation.

*For specifying how to apply the transformation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ASSIGN`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md). |
| `M_ASSIGN` | Specifies to assign the specified transformation to the given transformation matrix object. |
| `M_COMPOSE_WITH_CURRENT` | Specifies to compose the specified transformation with the given matrix.

The matrix composition follows the equation `BA = C`, where _A_ is the transformation matrix specified by [`Matrix3dgeoId`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), _B_ is the transformation matrix defined by the parameter associations, and _C_ is the resulting transformation matrix. The values of the resulting transformation matrix are written to the transformation matrix object specified by [`Matrix3dgeoId`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md). |

## Parameter Associations

### For specifying the transformation type

Set unused parameters to [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md).

---

### `M_COMPOSE_TWO_MATRICES`

Specifies the transformation resulting from the composition of two matrices.  The matrix composition follows the equation `AB = C`, where _A_ is the transformation matrix specified by [`Param1`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), _B_ is the transformation matrix specified by [`Param2`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), and _C_ is the resulting transformation matrix.  > **Note:** Note that transformation matrix _B_ is applied before transformation matrix _A_. Therefore, transformation matrix _C_ describes the transformation obtained by applying transformation matrix _B_ followed by transformation matrix _A_.

| Value | Description |
| --- | --- |
| `M_IDENTITY_MATRIX` | Specifies the identity matrix. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object. |
| `M_IDENTITY_MATRIX` | Specifies the identity matrix. |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object. |

---

### `M_FIXTURE_TO_GEOMETRY`

Specifies a transformation that can move the working coordinate system to a 3D geometry object.

| Value | Description |
| --- | --- |
| `3D box geometry object identifier` | Specifies a 3D box geometry object identifier previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a box.  The result is the transformation that can move the working coordinate system such that its origin moves to the box's center, and the box's orientation is applied. |
| `3D cylinder geometry object identifier` | Specifies a 3D cylinder geometry object identifier previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a cylinder.  The result is the transformation that can move the working coordinate system such that its origin moves to the cylinder's start point, and the cylinder's central axis becomes the positive Z-axis. The rotation of the X- and Y-axis is determined by the minimal rotation that must be applied to align the positive Z-axis with the cylinder's central axis. |
| `3D plane geometry object identifier` | Specifies a 3D plane geometry object identifier previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a plane.  The result is the transformation that can move the working coordinate system such that its XY (Z=0) plane is moved to the specified plane, and the plane's normal becomes the positive Z-axis. The working coordinate system's origin is moved to the closest point on the plane (the projection). |
| `3D point geometry object identifier` | Specifies a 3D point geometry object identifier previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a point.  The result is the transformation that can move the working coordinate system such that its origin is moved to the point. |
| `3D sphere geometry object identifier` | Specifies a 3D sphere geometry object identifier previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a sphere.  The result is the transformation that can move the working coordinate system such that its origin is moved to the sphere's center. No rotation is applied. |

---

### `M_FIXTURE_TO_PLANE`

Specifies a transformation that can move the XY (Z=0) plane of the working coordinate system to a plane specified by Param1. By default, the origin is moved using the same transformation. You can optionally specify a point on the plane where the origin should be moved, using [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), and [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md). If the point is not on the plane, or if you set [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), and [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) to [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), the closest point on the plane (the projection) is chosen instead.

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane.  The result is the transformation that can move the origin of the working coordinate system to a point on its XY (Z=0) plane, specified by [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), and [`M_FIXTURE_TO_PLANE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md). If these parameters are set to [`M_DEFAULT`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md), the result is the identity transformation. |
| `3D geometry object identifier` | Specifies a 3D plane geometry object identifier previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md) and defined as a plane. |

---

### `M_IDENTITY`

Specifies the identity transformation. This resets the matrix to the identity matrix, removing all transformations from the transformation matrix object, when [`ControlFlag`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) is [`M_ASSIGN`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md).

---

### `M_INVERSE`

Specifies the inverse transformation.

| Value | Description |
| --- | --- |
| `Transformation matrix object identifier` | Specifies the identifier of a transformation matrix object previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_ROTATION_AXIS_ANGLE`

Specifies a rotation described by an axis and angle of rotation. The axis of rotation is defined by a vector. The angle of rotation is measured in the counter-clockwise direction around the axis of rotation.  *[Image: calAngleOfRotation.png]*

---

### `M_ROTATION_AXIS_X`

Specifies the minimal rotation that can align the working coordinate system's X-axis with the specified vector.

---

### `M_ROTATION_AXIS_Y`

Specifies the minimal rotation that can align the working coordinate system's Y-axis with the specified vector.

---

### `M_ROTATION_AXIS_Z`

Specifies the minimal rotation that can align the working coordinate system's Z-axis with the specified vector.

---

### `M_ROTATION_QUATERNION`

Specifies a rotation operation that is described by a rotation quaternion.

---

### `M_ROTATION_X`

Specifies a rotation operation around the X-axis.

---

### `M_ROTATION_XYZ`

Specifies a rotation that is described by three distinct rotations about the axes of the working coordinate system in the following order: a rotation about the X-axis, a rotation about the Y-axis, and a rotation about the Z-axis rotation.

---

### `M_ROTATION_XZY`

Specifies a rotation that is described by three distinct rotations about the axes of the working coordinate system in the following order: a rotation about the X-axis, a rotation about the Z-axis, and a rotation about the Y-axis rotation.

---

### `M_ROTATION_Y`

Specifies a rotation operation around the Y-axis.

---

### `M_ROTATION_YXZ`

Specifies a rotation that is described by three distinct rotations about the axes of the working coordinate system in the following order: a rotation about the Y-axis, a rotation about the X-axis, and a rotation about the Z-axis rotation.

---

### `M_ROTATION_YZX`

Specifies a rotation that is described by three distinct rotations about the axes of the working coordinate system in the following order: a rotation about the Y-axis, a rotation about the Z-axis, and a rotation about the X-axis rotation.

---

### `M_ROTATION_Z`

Specifies a rotation operation around the Z-axis.

---

### `M_ROTATION_ZXY`

Specifies a rotation that is described by three distinct rotations about the axes of the working coordinate system in the following order: a rotation about the Z-axis, a rotation about the X-axis, and a rotation about the Y-axis rotation.

---

### `M_ROTATION_ZYX`

Specifies a rotation that is described by three distinct rotations about the axes of the working coordinate system in the following order: a rotation about the Z-axis, a rotation about the Y-axis, and a rotation about the X-axis rotation.

---

### `M_SCALE`

Specifies a scaling operation.

---

### `M_TRANSLATION`

Specifies a translation along each of the axes of the working coordinate system.
