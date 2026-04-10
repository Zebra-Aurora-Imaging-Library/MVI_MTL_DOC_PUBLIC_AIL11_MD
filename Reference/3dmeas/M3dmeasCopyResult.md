---
doctype: Reference
module: 3dmeas
function: M3dmeasCopyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasCopyResult"
---

# M3dmeasCopyResult

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

> Copy a group of results from a 3D measurement result buffer into a 3D geometry object, container, or image buffer.

## Syntax

```c
void M3dmeasCopyResult(
    AIL_ID    SrcResult3dmeasId,       //in
    AIL_INT64 SrcPathOrTemplateIndex,  //in
    AIL_INT64 SrcProfileIndex,         //in
    AIL_ID    DstAilObjectId,          //out
    AIL_INT64 DstIndex,                //in
    AIL_INT64 CopyType,                //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function copies a group of results from a 3D measurement result buffer into a 3D geometry object, container, or image buffer.

## Parameters

### `SrcResult3dmeasId` *(in, AIL_ID)*

Specifies the identifier of a 3D measurement result buffer from which to copy results. The 3D measurement result buffer must have been allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md).

### `SrcPathOrTemplateIndex` *(in, AIL_INT64)*

Specifies the index of the source template or path to copy.

### `SrcProfileIndex` *(in, AIL_INT64)*

Specifies the index of the source profile to copy.

### `DstAilObjectId` *(out, AIL_ID)*

Specifies the identifier of the image buffer, 3D geometry object, or container in which to store the copied results. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the copy type

---

### `M_GEOMETRY_FIT`

Specifies to copy the fitted geometry from the specified template 3D measurement result buffer into the specified 3D geometry object.  Note that this is only available after an [`M3dmeasFit`](../../Reference/3dmeas/M3dmeasFit.md) operation.

| Value | Description |
| --- | --- |
| `M_TEMPLATE_INDEX` | Specifies the index of the template. |

---

### `M_GEOMETRY_PATH`

Specifies to copy the path used to calculate the results in the specified path 3D measurement result buffer, into the specified 3D geometry object.

| Value | Description |
| --- | --- |
| `M_PATH_INDEX` | Specifies the index of the path. |

---

### `M_GEOMETRY_TEMPLATE`

Specifies to copy the template used to calculate the results in the specified template 3D measurement result buffer, into the specified 3D geometry object.

| Value | Description |
| --- | --- |
| `M_TEMPLATE_INDEX` | Specifies the index of the template. |

---

### `M_PROFILE_EXTRACTED`

Specifies to copy the extracted profile from the specified 3D measurement result buffer into the first row of the specified image buffer or container.

| Value | Description |
| --- | --- |
| `M_PATH_INDEX` | Specifies the index of the path in the path 3D measurement result buffer. |
| `M_TEMPLATE_INDEX` | Specifies the index of the template in the template 3D measurement result buffer. |
| `Value >= 0` | Specifies the index of a profile. Note that for a path 3D measurement result buffer, only index 0 is supported because only 1 profile is extracted along the path. |
| `Container identifier` | Specifies the identifier of a container. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.  Note that after the profile is copied, the specified container will be a 3D-processable depth map container with a single row. |
| `Image buffer identifier` | Specifies the identifier of an image buffer. The image buffer must be an 8-bit, 16-bit, or 32-bit unsigned, 1-band buffer.  Note that the profile is copied into the first row of the specified image buffer. |
