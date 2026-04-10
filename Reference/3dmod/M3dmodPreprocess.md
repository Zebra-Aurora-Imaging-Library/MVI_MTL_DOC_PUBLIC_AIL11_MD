---
doctype: Reference
module: 3dmod
function: M3dmodPreprocess
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dmod / M3dmodPreprocess"
---

# M3dmodPreprocess

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

> Preprocess a 3D model finder context.

## Syntax

```c
void M3dmodPreprocess(
    AIL_ID    Context3dmodId,  //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function preprocesses the specified 3D model finder context. It sets internal search settings so that future searches will be optimized for speed and robustness.

This function must be called before performing the first [`M3dmodFind`](../../Reference/3dmod/M3dmodFind.md) search. Call this function after all search settings have been set. When you save the 3D model finder context, the model's preprocessing changes are not stored with the model. Upon restoration, the model must be preprocessed again.

Note that if some of the model's search settings are changed after a call to [`M3dmodPreprocess`](../../Reference/3dmod/M3dmodPreprocess.md), the model must be preprocessed again.

## Parameters

### `Context3dmodId` *(in, AIL_ID)*

Specifies the identifier of the 3D model finder context to preprocess. The context must have been previously allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md), or [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
