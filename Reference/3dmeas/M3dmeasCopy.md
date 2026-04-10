---
doctype: Reference
module: 3dmeas
function: M3dmeasCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasCopy"
---

# M3dmeasCopy

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

> Copy a path or template to a 3D geometry.

## Syntax

```c
void M3dmeasCopy(
    AIL_ID    SrcContext3dmeasId,  //in
    AIL_INT64 SrcIndex,            //in
    AIL_ID    DstAilObjectId,      //out
    AIL_INT64 DstIndex,            //in
    AIL_INT64 CopyType,            //in
    AIL_INT64 ControlFlag          //in
)
```

## Description

This function copies a path in a path 3D measurement context or a template in a template 3D measurement context to a 3D geometry.

## Parameters

### `SrcContext3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the source 3D measurement context from which to copy.

*For specifying the source 3D measurement context*

| Value | Description |
| --- | --- |
| `Path 3D measurement context identifier` | Specifies the identifier of the path 3D measurement context from which to copy the path. |
| `Template 3D measurement context identifier` | Specifies the identifier of the template 3D measurement context from which to copy the template. |

### `SrcIndex` *(in, AIL_INT64)*

Specifies the index of the path or template in the source 3D measurement context.

*For specifying a path or template*

| Value | Description |
| --- | --- |
| `M_PATH_INDEX` | Specifies the path in a path 3D measurement context, if one is specified. |
| `M_TEMPLATE_INDEX` | Specifies the template in a template 3D measurement context, if one is specified. |

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the 3D geometry object in which to copy the path or template.

*For specifying the destination 3D geometry object*

| Value | Description |
| --- | --- |
| `3D geometry object identifier` | Specifies the identifier of the 3D geometry object in which to copy the path or template.

The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_GEOMETRY_PATH` | Specifies to copy the path from the path 3D measurement context into the specified 3D geometry object, establishing a 3D line geometry. |
| `M_GEOMETRY_TEMPLATE` | Specifies to copy the template from the template 3D measurement context into the specified 3D geometry object, establishing a 3D line geometry. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
