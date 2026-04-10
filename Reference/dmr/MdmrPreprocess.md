---
doctype: Reference
module: dmr
function: MdmrPreprocess
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrPreprocess"
---

# MdmrPreprocess

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

> Preprocess a SureDotOCR context.

## Syntax

```c
void MdmrPreprocess(
    AIL_ID    ContextDmrId,  //in
    AIL_INT64 ControlFlag    //in
)
```

## Description

This function prepares a SureDotOCR context for a read operation. This allows SureDotOCR to make internal refinements so it can execute optimized and robust dot-matrix character recognition processes. You must call this function before the first call to [`MdmrRead`](../../Reference/dmr/MdmrRead.md). To preprocess a context without error, it must contain at least one font, one character, and one string model.

Changes to a context or to any of its content often require you to preprocess the context again. To inquire a context's preprocessing state, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_PREPROCESSED`](../../Reference/dmr/MdmrInquire.md).

Saving a context does not save preprocessing changes. Upon restoration, you must preprocess the context again.

## Parameters

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context to preprocess. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to preprocess the SureDotOCR context. Set this parameter to one of the values below:

*For specifying whether to preprocess the context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Preprocesses the context. |
| `M_RESET` | Un-preprocesses the context.

Un-preprocessing the context can be useful if you want to conserve system memory within an application and preserve context settings. |
