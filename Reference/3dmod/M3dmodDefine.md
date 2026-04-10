---
doctype: Reference
module: 3dmod
function: M3dmodDefine
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodDefine"
---

# M3dmodDefine

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

> Add a model to, or delete a model from, a 3D model finder context.

## Syntax

```c
AIL_INT64 M3dmodDefine(
    AIL_ID     Context3dmodId,  //out
    AIL_INT64  Operation,       //in
    AIL_INT64  ModelType,       //in
    AIL_DOUBLE Param1,          //in
    AIL_DOUBLE Param2,          //in
    AIL_DOUBLE Param3,          //in
    AIL_DOUBLE Param4,          //in
    AIL_DOUBLE Param5,          //in
    AIL_DOUBLE Param6,          //in
    AIL_INT64  ControlFlag      //in
)
```

## Description

This function allows you to add a model to, or remove a model from, a 3D model finder context. Note, when a model is added or deleted, the 3D model finder context must be preprocessed again, using [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md). You can define one model per model finder context.

A geometric model can be defined either nominally or using a range of accepted values. Nominal models should be used when the expected occurrences are all approximately the same size; define models as ranges when you want to accept a variety of occurrences at different scales. Note that for range-type models, the proportions of the occurrences don't have to be the same as that of the model, as long as they are in the valid range.

Models are defined from specified numeric constraints, from one or two specified 3D geometries, or from a point cloud.

The search is performed according to the general search settings specified in the 3D model finder context ([`M_DEFAULT`](../../Reference/3dmod/M3dmodControl.md)), as well as the individual model search settings. Both the context and individual model search settings can be specified using [`M3dmodControl`](../../Reference/3dmod/M3dmodControl.md).

## Parameters

### `Context3dmodId` *(out, AIL_ID)*

Specifies the 3D model finder context to which to add, or from which to delete, the model. The 3D model finder context must have been previously allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_..._CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies whether to add or remove a model.

### `ModelType` *(in, AIL_INT64)*

Specifies the type of model to define when adding models to the context. Note that the type of model specified must match the specified 3D model finder context.

### `Param1` *(in, AIL_DOUBLE)*

Specifies the first parameter. Its definition is dependent on the model type chosen.

### `Param2` *(in, AIL_DOUBLE)*

Specifies the second parameter. Its definition is dependent on the model type chosen.

### `Param3` *(in, AIL_DOUBLE)*

Specifies the third parameter. Its definition is dependent on the model type chosen.

### `Param4` *(in, AIL_DOUBLE)*

Specifies the fourth parameter. Its definition is dependent on the model type chosen.

### `Param5` *(in, AIL_DOUBLE)*

Specifies the fifth parameter. Its definition is dependent on the model type chosen.

### `Param6` *(in, AIL_DOUBLE)*

Specifies the sixth parameter. Its definition is dependent on the model type chosen.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For adding or removing a model from the context

To add a model to the 3D model finder context, the [`Operation`](../../Reference/3dmod/M3dmodDefine.md), [`ModelType`](../../Reference/3dmod/M3dmodDefine.md), [`Param1`](../../Reference/3dmod/M3dmodDefine.md), [`Param2`](../../Reference/3dmod/M3dmodDefine.md), [`Param3`](../../Reference/3dmod/M3dmodDefine.md), [`Param4`](../../Reference/3dmod/M3dmodDefine.md), [`Param5`](../../Reference/3dmod/M3dmodDefine.md), and [`Param6`](../../Reference/3dmod/M3dmodDefine.md) parameters can be set to the following values. Note that any unused parameters should be set to [`M_DEFAULT`](../../Reference/3dmod/M3dmodDefine.md).

---

### `M_ADD`

Adds a model to the 3D model finder context.

#### `M_BOX`

Specifies to add a nominal box as the model.

#### `M_BOX_RANGE`

Specifies to add a box, defined with a range of possible values, as the model.

#### `M_CYLINDER`

Specifies to add a nominal cylinder as the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_WITH_BASES` *(default)* | Specifies that the cylinder model includes bases. |
| `M_WITHOUT_BASES` | Specifies that the cylinder model does not include bases. |

#### `M_CYLINDER_RANGE`

Specifies to add a cylinder, defined with a range of possible values, as the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_WITH_BASES` *(default)* | Specifies that the cylinder model includes bases. |
| `M_WITHOUT_BASES` | Specifies that the cylinder model does not include bases. |

#### `M_RECTANGLE`

Specifies to add a nominal rectangular plane as the model.

#### `M_RECTANGLE_RANGE`

Specifies to add a rectangular plane, defined with a range of possible values, as the model.

#### `M_SPHERE`

Specifies to add a nominal sphere as the model.

#### `M_SPHERE_RANGE`

Specifies to add a sphere, defined with a range of possible values, as the model.

---

### `M_ADD_FROM_GEOMETRY`

Adds a model to the 3D model finder context using predefined 3D geometries.

#### `M_BOX`

Specifies to add a nominal box as the model, using a predefined 3D geometry.

#### `M_BOX_RANGE`

Specifies to add a box, defined with a range of possible sizes, as the model, using predefined 3D geometries.

#### `M_CYLINDER`

Specifies to add a nominal cylinder as the model, using a predefined 3D geometry.

#### `M_CYLINDER_RANGE`

Specifies to add a cylinder, defined with a range of possible sizes, as the model, using predefined 3D geometries.

#### `M_SPHERE`

Specifies to add a nominal sphere as the model, using predefined 3D geometries.

#### `M_SPHERE_RANGE`

Specifies to add a sphere, defined with a range of possible sizes, as the model, using predefined 3D geometries.

---

### `M_ADD_FROM_POINT_CLOUD`

Adds a model to the 3D model finder context using the points in a point cloud.

#### `M_SURFACE`

Specifies to add an arbitrary surface, defined by a point cloud, as the model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to perform outlier removal on the model. |
| `M_ENABLE` | Specifies to perform outlier removal on the model. |
| `M_DEFAULT` | Specifies the default behavior; the origin of the model's reference axis is at the origin of the working coordinate system of the original point cloud from which the model is defined. |
| `M_BOX_CENTER` | Specifies to move the origin of the working coordinate system to the center of the point cloud's bounding box, setting the model's reference axis origin to this position. |
| `M_CENTROID` | Specifies to move the origin of the working coordinate system to the point cloud's centroid (center of mass), setting the model's reference axis origin to this position. |
| `M_GEOMETRY_CENTER` | Specifies to move the origin of the working coordinate system to the center of the specified 3D geometry, setting the model's reference axis origin to this position. |
| `M_TRANSFORMATION_MATRIX` | Specifies to transform the point cloud according to the specified rigid transformation matrix. |
| `M_DEFAULT` | Specifies the default behavior. |
| `3D geometry object ID` | Specifies the identifier of a 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined as a box or a sphere. |
| `Transformation matrix object ID` | Specifies the identifier of a transformation matrix object. The transformation object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). The transformation matrix must be of type [`M_RIGID`](../../Reference/3dgeo/M3dgeoInquire.md). |

---

### `M_DELETE`

Deletes a model from the 3D model finder context.

#### `M_DEFAULT`

Specifies the default behavior.

## Return Value

**Type:** `AIL_INT64`

Reserved for future expansion.

Note that if you expect occurrences to have a significantly different size than the model, you should use a range-type model instead, as the algorithm used for this type of model is better suited.
