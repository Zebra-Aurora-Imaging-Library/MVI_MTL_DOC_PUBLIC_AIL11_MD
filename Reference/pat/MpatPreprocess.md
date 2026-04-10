---
doctype: Reference
module: pat
function: MpatPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatPreprocess"
---

# MpatPreprocess

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

> Preprocess a Pattern Matching context.

## Syntax

```c
void MpatPreprocess(
    AIL_ID    ContextPatId,      //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID    TypicalImageBufId  //in
)
```

## Description

This function preprocesses the specified Pattern Matching context. It trains the Aurora Imaging Library application to search for one or more models in the most efficient manner (optionally within a specified typical image). The procedure is potentially quite lengthy (up to several seconds).

Call this function after at least one search model is added to the context and all search settings have been adjusted. When you save, the model's preprocessing changes are not stored with the model. Upon restoration, the model needs to be preprocessed.

Note that if some of the model's search settings are changed after a call to [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md), the model must be preprocessed again. To inquire if your model is in a preprocessed state, use [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_PREPROCESSED`](../../Reference/pat/MpatInquire.md).

## Parameters

### `ContextPatId` *(in, AIL_ID)*

Specifies the identifier of the Pattern Matching context to preprocess.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. Set this parameter to `M_DEFAULT`.

### `TypicalImageBufId` *(in, AIL_ID)*

Specifies the identifier of a typical target image. The specified typical image will be used to refine and adapt the model to search on this typical background. You should only specify an image buffer if the models will always appear on such a background; otherwise, set this parameter to `M_NULL`.
