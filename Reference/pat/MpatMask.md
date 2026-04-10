---
doctype: Reference
module: pat
function: MpatMask
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatMask"
---

# MpatMask

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

> Mask areas of a model in a Pattern Matching context.

## Syntax

```c
void MpatMask(
    AIL_ID    ContextPatId,  //out
    AIL_INT   Index,         //in
    AIL_ID    MaskBufferId,  //in
    AIL_INT64 MaskType,      //in
    AIL_INT64 ControlFlag    //in
)
```

## Description

This function allows you to apply a "don't care" mask to a model in a Pattern Matching context. Essentially, the mask sets pixels in the specified model to a "don't care state. The "don't care" pixels of the model will not be considered when searching for occurrences of the model in the target image (using[`MpatFind`](../../Reference/pat/MpatFind.md)). If a pixel in the mask image is a non-zero value, the corresponding pixel in the model is set to a "don't care" state. Note that there must be at least one model to the context before calling this function in the mask image; otherwise an error will occur.

Note, each time this function is called, a new set of "don't care" pixels is assigned to the specified model, superseding any existing set. Therefore, multiple calls to this function do not have a cumulative effect. Also, whenever the "don't care" pixels in a model change, the effect of any preprocessing is undone. Therefore, if the "don't care" pixels are changed, call [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md) before searching for the model.

> **Note:** This function does not support an [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) + [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) type of model, except if [`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_DISABLE`](../../Reference/pat/MpatControl.md).

## Parameters

### `ContextPatId` *(out, AIL_ID)*

Specifies the identifier of the Pattern Matching context containing the model to mask.

### `Index` *(in, AIL_INT)*

Specifies the individual model on which to apply the mask.

*For specifying a model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the index of the model for which the mask is being created. |

### `MaskBufferId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to use as a mask. Non-zero-pixel values identify the "don't care" pixels in the model.

### `MaskType` *(in, AIL_INT64)*

Specifies which type of mask to employ.

*For specifying the value of the don't care pixels*

| Value | Description |
| --- | --- |
| `M_DONT_CARE` | Specifies to apply (or remove) a don't care mask. The features present in the masked area (non-zero pixels) are ignored during the search. Features present in this area of the occurrence will not affect the scores. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
