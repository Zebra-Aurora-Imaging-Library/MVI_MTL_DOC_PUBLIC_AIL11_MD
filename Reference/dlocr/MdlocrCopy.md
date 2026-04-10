---
doctype: Reference
module: dlocr
function: MdlocrCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrCopy"
---

# MdlocrCopy

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

> Copy settings between two Deep Learning OCR read contexts, or copy results from a Deep Learning OCR fine-tuning result buffer to a read context.

## Syntax

```c
void MdlocrCopy(
    AIL_ID    SrcDlOcrContextOrResult,  //in
    AIL_ID    DstDlOcrContext,          //out
    AIL_INT64 CopyType,                 //in
    AIL_INT64 ControlFlag               //in
)
```

## Description

This function copies settings from the source read context ([`SrcDlOcrContextOrResult`](../../Reference/dlocr/MdlocrCopy.md)) to the destination read context ([`DstDlOcrContext`](../../Reference/dlocr/MdlocrCopy.md)). This is useful, for example, if you want to allocate a new context using a different deep learning model ([`ContextType`](../../Reference/dlocr/MdlocrAlloc.md)) and keep the same settings.

Alternatively, this function copies results from a source Deep Learning OCR fine-tuning result buffer ([`SrcDlOcrContextOrResult`](../../Reference/dlocr/MdlocrCopy.md)) to a destination read context ([`DstDlOcrContext`](../../Reference/dlocr/MdlocrCopy.md)). You can then use this Deep Learning OCR read context in a [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operation.

You must preprocess the destination Deep Learning OCR context before calling [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) or [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md). To know if a context needs to be preprocessed, call [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_PREPROCESSED`](../../Reference/dlocr/MdlocrInquire.md).

## Parameters

### `SrcDlOcrContextOrResult` *(in, AIL_ID)*

Specifies the identifier of the source Deep Learning OCR read context or fine-tuning result buffer from which to copy.

### `DstDlOcrContext` *(out, AIL_ID)*

Specifies the identifier of the destination Deep Learning OCR context in which to copy.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*For specifying the copy operation*

| Value | Description |
| --- | --- |
| `M_ALL_COMMON_SETTINGS` | Specifies to copy all common settings from the source read context to the destination read context. The [`SrcDlOcrContextOrResult`](../../Reference/dlocr/MdlocrCopy.md) parameter must be set to a valid Deep Learning OCR read context identifier. |
| `M_FINETUNED_CONTEXT` | Specifies to copy the result of a fine-tuning operation from a source fine-tuning result buffer to a destination read context. The [`SrcDlOcrContextOrResult`](../../Reference/dlocr/MdlocrCopy.md) parameter must be set to a valid Deep Learning OCR fine-tuning result buffer identifier. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
