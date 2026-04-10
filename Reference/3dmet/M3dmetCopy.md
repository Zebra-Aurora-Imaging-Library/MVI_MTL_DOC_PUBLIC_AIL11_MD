---
doctype: Reference
module: 3dmet
function: M3dmetCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetCopy"
---

# M3dmetCopy

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

> Copy a 3D geometry into, or out of, a fit 3D metrology context.

## Syntax

```c
void M3dmetCopy(
    AIL_ID    SrcAilObjectId,  //in
    AIL_ID    DstAilObjectId,  //out
    AIL_INT64 CopyType,        //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function copies a 3D geometry from a 3D geometry object into a fit 3D metrology context, or vice versa. The fit operation ([`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md)) will use this geometry to determine an initial fit estimate if [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_ESTIMATION_MODE`](../../Reference/3dmet/M3dmetControl.md) is set to [`M_FROM_GEOMETRY`](../../Reference/3dmet/M3dmetControl.md). Note that the fit operation does not support 3D box geometries.

## Parameters

### `SrcAilObjectId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library object from which to copy the 3D geometry.

*For specifying the source Aurora Imaging Library object*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies to copy the XY (Z=0) plane.

This requires that the destination Aurora Imaging Library object be a fit 3D metrology context. |
| `3D geometry object identifier` | Specifies the identifier of the 3D geometry object from which to copy the 3D geometry; 3D box geometries are not supported. |
| `Fit 3D metrology context identifier` | Specifies the identifier of the fit 3D metrology context from which to copy the 3D geometry. |

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library object in which to copy the 3D geometry.

*For specifying the destination Aurora Imaging Library object*

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of the 3D geometry object in which to copy the 3D geometry. |
| `Fit 3D metrology context identifier` | Specifies the identifier of the fit 3D metrology context in which to copy the 3D geometry. |

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_ESTIMATE_GEOMETRY` | Specifies to copy the 3D geometry from the source Aurora Imaging Library object into the destination Aurora Imaging Library object. The 3D geometry in the destination Aurora Imaging Library object will be overwritten. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
