---
doctype: Reference
module: buf
function: MbufLink
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufLink"
---

# MbufLink

> Link one buffer or container to another.

## Syntax

```c
void MbufLink(
    AIL_ID    SrcContainerOrBufId,     //in
    AIL_ID    TargetContainerOrBufId,  //out
    AIL_INT64 LinkOperation,           //in
    AIL_INT   ControlFlag              //in
)
```

## Description

This function links two buffers or containers so that subsequent modifications in the source are copied to the target until the buffers/containers are unlinked. For instance, when using the Distributed Aurora Imaging Library monitoring configuration, you can link a publishing application's image buffer to the monitoring application's image buffer. Any modifications to the published buffer will be copied to the local buffer.

## Parameters

### `SrcContainerOrBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer or container. This buffer or container remains independent and can be modified as per the application's standard mechanisms.

### `TargetContainerOrBufId` *(out, AIL_ID)*

Specifies the identifier of the target buffer or container. The contents of this buffer or container will have the same modifications applied to it as are applied to [`SrcContainerOrBufId`](../../Reference/buf/MbufLink.md).

### `LinkOperation` *(in, AIL_INT64)*

Specifies the link operation to perform.

*To specify the link operation*

| Value | Description |
| --- | --- |
| `M_LINK` | Specifies to link the two specified buffers. |
| `M_UNLINK` | Specifies to unlink the two specified buffers. |

### `ControlFlag` *(in, AIL_INT)*

Specifies whether to convert 3D information copied from the source container. If unused, this parameter must be set to `M_DEFAULT`.

*For specifying to compensate for missing information in the source container*

| Value | Description |
| --- | --- |
| `M_COMPENSATE` | Specifies to automatically convert the information copied from the source container to the destination container so that the destination container is 3D-processable (if the destination container has the attribute [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)) and/or 3D-displayable (if the destination container has the attribute [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)). This is equivalent to using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)with [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md).

> **Note:** An error will be generated any time the source container is modified and its contents cannot be copied because it cannot be converted.

> **Note:** Note that this setting is only available when ail3d&lt;version number>.dll is present. |
