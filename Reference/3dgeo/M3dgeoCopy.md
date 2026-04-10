---
doctype: Reference
module: 3dgeo
function: M3dgeoCopy
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgeo / M3dgeoCopy"
---

# M3dgeoCopy

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

> Copy attributes or settings to or from a 3D geometry object or transformation matrix object.

## Syntax

```c
void M3dgeoCopy(
    AIL_ID    SrcAilObjectId,  //in
    AIL_ID    DstAilObjectId,  //out
    AIL_INT64 CopyType,        //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function copies attributes or settings to or from a 3D geometry object or transformation matrix object. You can copy the entire object or some attributes or settings of the object (for example, box orientation), depending on the source and destination objects and the specified copy type.

## Parameters

### `SrcAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the source object. Note that if the source object is not a predefined or user-defined 3D geometry object or transformation matrix object, the destination object must be.

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the destination object. Note that if the destination object is not a predefined or user-defined 3D geometry object or transformation matrix object, the source object must be.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type

---

### `M_BOX_ORIENTATION`

Specifies to set the orientation of a 3D box geometry object, or copy the orientation of a 3D box geometry object.  > **Note:** Note that changing the box orientation will not affect its center or its dimensions (that is, the box rotates around its center).

#### `M_IDENTITY_MATRIX`

Specifies to set the 3D box geometry object's box orientation to the identity matrix. This can be used to reset the box's orientation.

| Value | Description |
| --- | --- |
| `3D geometry object ID` | Specifies the identifier of the 3D box geometry object whose orientation to set. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box. |

#### `3D box geometry object ID from which to copy`

Specifies the identifier of a 3D box geometry object from which to copy the box orientation. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

| Value | Description |
| --- | --- |
| `3D box geometry object ID in which to copy` | Specifies the identifier of the 3D box geometry object whose orientation to set. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box. |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the transformation matrix object in which to copy the box orientation. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Transformation matrix object ID to use to define`

Specifies the identifier of the transformation matrix object used to set the 3D box geometry object's box orientation. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

| Value | Description |
| --- | --- |
| `3D geometry object ID` | Specifies the identifier of the 3D box geometry object whose orientation to set. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_GEOMETRY`

Specifies to copy a 3D geometry object into another 3D geometry object.

#### `M_XY_PLANE`

Specifies to copy the XY (Z=0) plane into a 3D geometry object.

| Value | Description |
| --- | --- |
| `3D geometry object ID` | Specifies the identifier of the 3D geometry object whose geometry to set to the XY (Z=0) plane. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `3D geometry object ID from which to copy`

Specifies the identifier of a 3D geometry object from which to copy the geometry. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined.

| Value | Description |
| --- | --- |
| `3D geometry object ID` | Specifies the identifier of the 3D geometry object whose geometry to set. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_ROTATION_AND_SCALE`

Specifies to set only the rotation and scale values of a transformation matrix object using an Aurora Imaging Library array buffer, or to copy only the rotation and scale values of a transformation matrix object.

#### `M_IDENTITY_MATRIX`

Specifies to copy the rotation and scale values of the identity matrix (equivalent to no rotation and uniform scale of 1).

| Value | Description |
| --- | --- |
| `Array buffer identifier in which to copy` | Specifies the identifier of the 4x4 Aurora Imaging Library array buffer in which to copy the rotation and scale values of the identity matrix. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the destination transformation matrix object in which to copy the rotation and scale values of the identity matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Array buffer identifier to use to define`

Specifies the identifier of a 4x4 Aurora Imaging Library array buffer used to set the rotation and scale values of the transformation matrix object. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md), and must have been initialized.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID` | Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Transformation matrix object ID from which to copy`

Specifies the identifier of a transformation matrix object from which to copy the rotation and scale values. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

| Value | Description |
| --- | --- |
| `Array buffer identifier in which to copy` | Specifies the identifier of the 4x4 Aurora Imaging Library array buffer in which to copy the rotation and scale values of the transformation matrix object. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the destination transformation matrix object in which to copy the rotation and scale values of the source transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_TRANSFORMATION_MATRIX`

Specifies to set the values of a transformation matrix object using an Aurora Imaging Library array buffer, or to copy a transformation matrix object.

#### `M_IDENTITY_MATRIX`

Specifies to copy the identity matrix.

| Value | Description |
| --- | --- |
| `Array buffer identifier in which to copy` | Specifies the identifier of the 4x4 Aurora Imaging Library array buffer in which to copy the identity matrix. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the destination transformation matrix object in which to copy the identity matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Array buffer ID to use to define`

Specifies the identifier of a 4x4 Aurora Imaging Library array buffer used to set the transformation matrix object. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md), and must have been initialized.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID` | Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Transformation matrix object ID from which to copy`

Specifies the identifier of a transformation matrix object to copy. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

| Value | Description |
| --- | --- |
| `Array buffer ID in which to copy` | Specifies the identifier of the 4x4 Aurora Imaging Library array buffer in which to copy the values of the transformation matrix object. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the destination transformation matrix object in which to copy the values of the source transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

---

### `M_TRANSLATION`

Specifies to set only the translation values of a transformation matrix object using an Aurora Imaging Library array buffer, or to copy only the translation values of a transformation matrix object.

#### `M_IDENTITY_MATRIX`

Specifies to copy the translation values of the identity matrix (equivalent to no translation).

| Value | Description |
| --- | --- |
| `Array buffer ID in which to copy` | Specifies the identifier of the 4x4 Aurora Imaging Library array buffer in which to copy the translation values of the identity matrix. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the destination transformation matrix object in which to copy the translation values of the identity matrix. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Array buffer ID to use to define`

Specifies the identifier of a 4x4 Aurora Imaging Library array buffer used to set the translation values of the transformation matrix object. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md), and must have been initialized.

| Value | Description |
| --- | --- |
| `Transformation matrix object ID` | Specifies the identifier of the transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |

#### `Transformation matrix object ID from which to copy`

Specifies the identifier of a transformation matrix object from which to copy the translation values. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).

| Value | Description |
| --- | --- |
| `Array buffer ID in which to copy` | Specifies the identifier of the 4x4 Aurora Imaging Library array buffer in which to copy the translation values of the transformation matrix object. The Aurora Imaging Library array buffer must have been previously allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) and [`32`](../../Reference/buf/MbufAlloc2d.md)+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md). |
| `Transformation matrix object ID in which to copy` | Specifies the identifier of the destination transformation matrix object in which to copy the translation values of the source transformation matrix object. The transformation matrix object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |
