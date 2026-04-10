---
doctype: Reference
module: agm
function: MagmPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / agm / MagmPreprocess"
---

# MagmPreprocess

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

> Preprocess an AGM context.

## Syntax

```c
void MagmPreprocess(
    AIL_ID    ContextAgmId,  //in
    AIL_INT64 ControlFlag    //in
)
```

## Description

This function preprocesses the specified AGM context. It sets internal settings so that future search or train operations will be optimized for speed and robustness. The procedure is potentially quite lengthy (up to a few seconds) and increases the size of the AGM context significantly.

This function must be called before performing an [`MagmFind`](../../Reference/agm/MagmFind.md) or [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation. Call this function after all the required settings have been set; if some of the model's settings are changed after a call to [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md), the model must be preprocessed again. Note that you must preprocess the context after copying a composite-definition model to a find AGM context, even if no settings are changed. When you save the AGM context, the model's preprocessing changes are not stored with the model. Upon restoration, the model must be preprocessed again.

## Parameters

### `ContextAgmId` *(in, AIL_ID)*

Specifies the AGM context to preprocess. The AGM context must have been previously allocated on the system using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md) or [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to preprocess the AGM context or remove preprocessing information from the AGM context. Set this parameter to one of the following values:

*For specifying whether to perform preprocessing*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the preprocessing of the AGM context. |
| `M_RESET` | Removes preprocessing information from the AGM context.

Removing this information can be useful if you want to conserve computer memory within an application and preserve AGM context settings. |
