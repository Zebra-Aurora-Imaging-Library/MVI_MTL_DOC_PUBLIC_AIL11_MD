---
doctype: Reference
module: 3dreg
function: M3dregSetLocation
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dreg / M3dregSetLocation"
---

# M3dregSetLocation

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

> Sets the rough location of the working coordinate system of a point cloud with respect to either the working coordinate system of another point cloud, or the global coordinate system.

## Syntax

```c
void M3dregSetLocation(
    AIL_ID    Context3dregId,  //out
    AIL_INT   Index,           //in
    AIL_INT   Reference,       //in
    AIL_ID    ParamId,         //in
    AIL_INT   ParamIndex,      //in
    AIL_INT   ParamReference,  //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function sets the rough location of the working coordinate system of a point cloud with respect to a specified reference. A point cloud's reference is either the working coordinate system of another point cloud, or the global coordinate system. The rough location is stored in the point cloud's corresponding registration element, in the pairwise 3D registration context. Each registration element contains the registration information for a single point cloud.

The first iteration of the registration operation is called the preregistration. Specify the type of preregistration to perform using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md) with [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md). If [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md) is set to [`M_USER_DEFINED`](../../Reference/3dreg/M3dregControl.md), [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) will preregister a point cloud using the preregistration transformation matrix specified by this function. If [`M_PREREGISTRATION_MODE`](../../Reference/3dreg/M3dregControl.md) is set to [`M_CENTROID`](../../Reference/3dreg/M3dregControl.md), the point cloud will be rotated according to the preregistration transformation matrix specified by this function, and translated so that the centroids of the two points clouds share the same coordinates. The translation component of the specified preregistration transformation matrix is ignored.

This function can copy the rough location from another registration element, or from the results of a previous registration operation.

You can set the rough location of several point clouds through repeated calls to [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md), and perform all registrations with one call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md). Some restrictions apply when selecting references for point clouds. One point cloud must use the global coordinate system as its reference. Additionally, the references for points clouds cannot be circularly dependent. If you take a specific point cloud and look at its reference, and then at its reference's reference, and continue through the series of point clouds linked in this manner, the initial point cloud must never be another's reference.

By default, in a pairwise 3D registration context, the first registration element has the global coordinate system as its reference, and the first registration element is the reference of all other registration elements.

## Parameters

### `Context3dregId` *(out, AIL_ID)*

Specifies the identifier of the pairwise 3D registration context in which to store the rough location.

### `Index` *(in, AIL_INT)*

Specifies the index of the registration element in the pairwise 3D registration context to set.

*For specifying the index*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to set all registration elements in the pairwise 3D registration context.

> **Note:** If [`ParamId`](../../Reference/3dreg/M3dregSetLocation.md) is a pairwise 3D registration context or pairwise 3D registration result buffer, then the number of registration elements in the pairwise 3D registration context must be less than or equal to the number of registration elements in the object passed to [`ParamId`](../../Reference/3dreg/M3dregSetLocation.md). |
| `0 <= Value < M3dregInquire(Context3dregId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration element in the pairwise 3D registration context. |

### `Reference` *(in, AIL_INT)*

Specifies the reference of the registration element in [`Context3dregId`](../../Reference/3dreg/M3dregSetLocation.md).

*For specifying the reference*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COPY` | Specifies to copy the rough location of [`ParamReference`](../../Reference/3dreg/M3dregSetLocation.md).

> **Note:** This setting is only available if [`ParamId`](../../Reference/3dreg/M3dregSetLocation.md) specifies a pairwise 3D registration context or a pairwise 3D registration result buffer. |
| `M_LAST` | Specifies the index of the last registration element in the pairwise 3D registration context passed to [`Context3dregId`](../../Reference/3dreg/M3dregSetLocation.md). |
| `M_NEXT` | Specifies the index of the registration element after the one specified in [`Index`](../../Reference/3dreg/M3dregSetLocation.md), unless [`Index`](../../Reference/3dreg/M3dregSetLocation.md)refers to the last registration element in the context. In this case, [`M_NEXT`](../../Reference/3dreg/M3dregSetLocation.md) will specify the global coordinate system. |
| `M_PREVIOUS` *(default)* | Specifies the index of the registration element before the one specified in [`Index`](../../Reference/3dreg/M3dregSetLocation.md), unless [`Index`](../../Reference/3dreg/M3dregSetLocation.md)refers to the first registration element in the context. In this case, [`M_PREVIOUS`](../../Reference/3dreg/M3dregSetLocation.md) will specify the global coordinate system. |
| `M_REGISTRATION_GLOBAL` | Specifies the global coordinate system. |
| `M_UNCHANGED` | Specifies not to modify the current reference of the registration element. |
| `0 <= Value < M3dregInquire(Context3dregId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the reference registration element.

This value should not equal the value specified for [`Index`](../../Reference/3dreg/M3dregSetLocation.md). |

### `ParamId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object from which to copy the rough location.

### `ParamIndex` *(in, AIL_INT)*

Specifies the index of the registration element or registration result element in the Aurora Imaging Library object passed to the [`ParamId`](../../Reference/3dreg/M3dregSetLocation.md) parameter.

### `ParamReference` *(in, AIL_INT)*

Specifies an attribute of the transformation that specifies a rough location.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the reference point cloud

---

### `M_IDENTITY_MATRIX`

Specifies to use an identity matrix for the preregistration iteration. This means the source point cloud will not be transformed or rotated in any way during the preregistration iteration.

---

### `Pairwise 3D registration context identifier`

Specifies the identifier of a pairwise 3D registration context from which to copy the rough location. The context must have been previously allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_PAIRWISE_REGISTRATION_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the same value passed to the [`Index`](../../Reference/3dreg/M3dregSetLocation.md) parameter. |
| `M_ALL` | Specifies to copy all registration elements. [`ParamIndex`](../../Reference/3dreg/M3dregSetLocation.md) must be set to [`M_ALL`](../../Reference/3dreg/M3dregSetLocation.md) if [`Index`](../../Reference/3dreg/M3dregSetLocation.md) is set to [`M_ALL`](../../Reference/3dreg/M3dregSetLocation.md). |
| `0 <= Value < M3dregInquire(ParamId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration element. |

---

### `Pairwise 3D registration result buffer identifier`

Specifies the identifier of a pairwise 3D registration result buffer from which to copy the rough location. The result buffer must have been previously allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md) with [`M_PAIRWISE_REGISTRATION_RESULT`](../../Reference/3dreg/M3dregAllocResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the same value passed to the [`Index`](../../Reference/3dreg/M3dregSetLocation.md) parameter. |
| `M_ALL` | Specifies to copy all registration result elements. [`ParamIndex`](../../Reference/3dreg/M3dregSetLocation.md) must be set to [`M_ALL`](../../Reference/3dreg/M3dregSetLocation.md) if [`Index`](../../Reference/3dreg/M3dregSetLocation.md) is set to [`M_ALL`](../../Reference/3dreg/M3dregSetLocation.md). |
| `0 <= Value < M3dregInquire(ParamId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration result element. |
| `M_DEFAULT` | Specifies the same value passed to the [`Reference`](../../Reference/3dreg/M3dregSetLocation.md) parameter.  This parameter cannot be set to [`M_DEFAULT`](../../Reference/3dreg/M3dregSetLocation.md) when the [`Reference`](../../Reference/3dreg/M3dregSetLocation.md) parameter is set to [`M_COPY`](../../Reference/3dreg/M3dregSetLocation.md). |
| `M_REGISTRATION_GLOBAL` | Specifies the global coordinate system. |
| `0 <= Value < M3dregInquire(ParamId, M_NUMBER_OF_REGISTRATION_ELEMENTS)` | Specifies the index of the registration result element. |

---

### `Transformation matrix object identifier`

Specifies the identifier of a transformation matrix object from which to copy the rough location. The transformation matrix object must have been previously allocated with [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md).
