---
doctype: Reference
module: mod
function: MmodPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodPreprocess"
---

# MmodPreprocess

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

> Preprocess a Model Finder context.

## Syntax

```c
void MmodPreprocess(
    AIL_ID    ContextId,   //in-out
    AIL_INT64 ControlFlag  //in
)
```

## Description

This function preprocesses the specified Model Finder context. It extracts the active edges of the models contained within the Model Finder context and sets internal search settings so that future searches will be optimized for speed and robustness. The procedure is potentially quite lengthy (up to a few seconds) and increases the size of the Model Finder context significantly.

This function must be called before performing the first [`MmodFind`](../../Reference/mod/MmodFind.md) search. Call this function after all search settings have been set. When you save the Model Finder context, the model's preprocessing changes are not stored with the model. Upon restoration, the model must be preprocessed again.

Note that if some of the model's search settings are changed after a call to [`MmodPreprocess`](../../Reference/mod/MmodPreprocess.md), the model must be preprocessed again. To inquire if your model is in a preprocessed state, use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_PREPROCESSED`](../../Reference/mod/MmodInquire.md) (this value must be true to perform an [`MmodFind`](../../Reference/mod/MmodFind.md) search).

## Parameters

### `ContextId` *(in-out, AIL_ID)*

Specifies the Model Finder context of the model to preprocess. The Model Finder context must have been previously allocated on the system using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to perform or undo the preprocessing of the model context. Set this parameter to one of the following values:

*For specifying whether to perform preprocessing*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the preprocessing of the model context. |
| `M_RESET` | Undoes preprocessing.

Undoing preprocessing can be useful if you want to conserve computer memory within an application and preserve Model Finder context settings. |
