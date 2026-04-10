---
doctype: Reference
module: blob
function: MblobTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / blob / MblobTransform"
---

# MblobTransform

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

> Perform a transformation/modification on blob analysis results.

## Syntax

```c
void MblobTransform(
    AIL_ID     SrcResultBlobId,  //in
    AIL_ID     DstResultBlobId,  //out
    AIL_INT64  Operation,        //in
    AIL_DOUBLE Parameter1,       //in
    AIL_DOUBLE Parameter2,       //in
    AIL_DOUBLE Parameter3,       //in
    AIL_INT64  ControlFlag       //in
)
```

## Description

This function performs a transformation/modification on the results of a blob analysis operation performed using [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

## Parameters

### `SrcResultBlobId` *(in, AIL_ID)*

Specifies the identifier of the source result buffer.

### `DstResultBlobId` *(out, AIL_ID)*

Specifies the identifier of the destination result buffer. [`DstResultBlobId`](../../Reference/blob/MblobTransform.md) must be set to the same result buffer as [`SrcResultBlobId`](../../Reference/blob/MblobTransform.md)(that is, the transformation is done in-place).

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

*For specifying the operation*

| Value | Description |
| --- | --- |
| `M_RELABEL_CONSECUTIVE` | Specifies to relabel blobs to get a continuous sequence of labels for included and excluded blobs. New labels are assigned based on the order of the current labels of the blobs, and not their status.

For example, if you have 4 blobs, labeled 1,4,5,8, and exclude the second blob, after [`M_RELABEL_CONSECUTIVE`](../../Reference/blob/MblobTransform.md), the blobs will be labeled 1,3,4.

Results must have been calculated in [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md) processing mode.

Original blob labeling is not necessarily continuous, and the maximum label value ([`M_MAX_LABEL_VALUE`](../../Reference/blob/MblobGetResult.md)) can be large.[`M_RELABEL_CONSECUTIVE`](../../Reference/blob/MblobTransform.md) can reduce the maximum label value, which potentially shrinks the bit depth needed for a destination image buffer (for example, for an [`MblobLabel`](../../Reference/blob/MblobLabel.md) operation). |

### `Parameter1` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Parameter2` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Parameter3` *(in, AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
