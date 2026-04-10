---
doctype: Reference
module: dlocr
function: MdlocrPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrPreprocess"
---

# MdlocrPreprocess

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

> Preprocess a Deep Learning OCR context.

## Syntax

```c
void MdlocrPreprocess(
    AIL_ID    ContextDlocrId,  //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function prepares a Deep Learning OCR context for a read or fine-tuning operation.

For a read context, this allows Deep Learning OCR to make internal refinements so it can execute optimized and robust character recognition processes. You must call this function before the first call to [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

For a fine-tuning context, this allows Deep Learning OCR to prepare the context for training. You must call this function before the first call to [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md).

Changes to the context or to any of its content often require you to preprocess the context again. To inquire a context's preprocessing state, call [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_PREPROCESSED`](../../Reference/dlocr/MdlocrInquire.md).

Saving a context does not save preprocessing changes. Upon restoration, you must preprocess the context again.

## Parameters

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context to preprocess. The context must have been previously allocated on the system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to preprocess the Deep Learning OCR context. Set this parameter to one of the values below:

*For specifying whether to preprocess the context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Preprocesses the context. |
| `M_RESET` | Un-preprocesses the context.

Un-preprocessing the context can be useful if you want to conserve system memory within an application and preserve context settings. |
