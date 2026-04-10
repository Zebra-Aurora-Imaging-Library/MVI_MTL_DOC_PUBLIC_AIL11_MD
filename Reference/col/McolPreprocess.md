---
doctype: Reference
module: col
function: McolPreprocess
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolPreprocess"
---

# McolPreprocess

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

> Preprocess a relative color calibration or color matching context.

## Syntax

```c
void McolPreprocess(
    AIL_ID    ContextId,   //in
    AIL_INT64 ControlFlag  //in
)
```

## Description

This function prepares the specified relative color calibration or color matching context for an [`McolTransform`](../../Reference/col/McolTransform.md) (relative color calibration) or [`McolMatch`](../../Reference/col/McolMatch.md) (color matching) operation. Preprocessing calculates the necessary data for each color-sample contained in its respective context and specifies internal settings to optimize future calculations for speed and robustness. For a relative color calibration context, preprocessing also establishes the color mapping between each color-sample and the reference color-sample.

You must preprocess the relative color calibration or color matching context before the first call to [`McolTransform`](../../Reference/col/McolTransform.md) or [`McolMatch`](../../Reference/col/McolMatch.md). Changes to the context, its settings, or one of its color-samples often require you to preprocess the context again. To inquire if this is the case, use [`McolInquire`](../../Reference/col/McolInquire.md) with [`M_PREPROCESSED`](../../Reference/col/McolInquire.md).

When you save the context, preprocessing changes are not saved. Upon restoration ([`McolRestore`](../../Reference/col/McolRestore.md)), you must preprocess the context.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the context to preprocess. The context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md) or [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
