---
doctype: Reference
module: im
function: MimFlatField
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimFlatField"
---

# MimFlatField

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

> Applies a flat-field correction to remove artifacts from the specified source image.

## Syntax

```c
void MimFlatField(
    AIL_ID    FlatFieldContextImId,  //in
    AIL_ID    SrcImageBufId,         //in
    AIL_ID    DstImageBufId,         //out
    AIL_INT64 ControlFlag            //in
)
```

## Description

This function applies a per-pixel gain and offset correction to the source image and writes the results in the destination image. This process removes artifacts from grabbed images, such as electrical bias, thermal agitation, and sensitivity variations.

The following equation defines the relationship between the source and destination pixel values.

*[Image: IM-FlatFieldEquation.png]*

Before using this function, you must configure the specified image processing context with the gain to apply, and the electrical bias, thermal agitation, and CCD sensitivity to remove, using [`MimControl`](../../Reference/im/MimControl.md); otherwise, an error is reported. You can specify each of these on a per-pixel level using an image or on a global level using a constant.

Once the flat-field correction image processing context is configured, preprocess the context by calling [`MimFlatField`](../../Reference/im/MimFlatField.md) with [`M_PREPROCESS`](../../Reference/im/MimFlatField.md). If the preprocess operation is not done explicitly (using [`M_PREPROCESS`](../../Reference/im/MimFlatField.md)), it will be done when [`MimFlatField`](../../Reference/im/MimFlatField.md) is first called. When preprocessing explicitly, the source and destination image buffers can be set to [`M_NULL`](../../Reference/im/MimFlatField.md). If, however, the source or destination image buffer is provided, it will be used in the preprocess operation, to better optimize future calls.

## Parameters

### `FlatFieldContextImId` *(in, AIL_ID)*

Specifies the identifier of a flat-field image processing context, previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FLAT_FIELD_CONTEXT`](../../Reference/im/MimAlloc.md).

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer must be either an 8- or 16-bit unsigned image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. The buffer must be either an 8- or 16-bit unsigned image buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the operation to perform. This parameter must be set to one of the following values.

*For specifying the operation to perform.*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the flat-field correction operation. |
| `M_PREPROCESS` | Preprocesses the specified image processing context. Note that, if not called explicitly, this operation will be performed automatically upon the first call to [`MimFlatField`](../../Reference/im/MimFlatField.md).

If the identifier of the source and/or destination buffer is not set to [`M_NULL`](../../Reference/im/MimFlatField.md), information about the source and/or destination image buffer will be used in the preprocessing operation. |
