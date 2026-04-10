---
doctype: Reference
module: mod
function: MmodMask
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodMask"
---

# MmodMask

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

> Mask regions of a model.

## Syntax

```c
void MmodMask(
    AIL_ID    ContextId,     //out
    AIL_INT   Index,         //in
    AIL_ID    MaskBufferId,  //in
    AIL_INT64 MaskType,      //in
    AIL_INT64 ControlFlag    //in
)
```

## Description

This function allows you to apply masks to a model. Each mask can set pixels in the specified model to one of three states: a "don't care" state, a "flat region" state, or a "weighted region" state. You can apply different types of masks to the same model by making multiple calls to this function. Note that "don't care" and "flat region" masks are not supported for synthetic models. In addition, this function does not support models in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

"Don't care" pixels in a model will not be considered when searching for occurrences of the model in the target.

"Flat region" pixels denote a region where no features are expected, other than consistent pixel values with no edges present; any edge found in a "flat region" reduces the target score.

"Weighted region" pixels denote that the extracted model features will have weights to indicate their importance in the occurrence. The weight of the features will affect the calculation of the score; although the weights themselves don't affect the target score, features corresponding to negative weights will be ignored when calculating the target score.

Note that if you modify the margin of a synthetic model's bounding box after a mask has been applied, the mask will be discarded since it is no longer the same size as the model.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the Model Finder context of the model to mask. For this parameter, you can only specify [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder contexts. The Model Finder context must have been previously allocated on the system using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md).

### `Index` *(in, AIL_INT)*

Specifies the individual model for which to create a mask.

*For specifying a model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the index of the model for which the mask is being created.

User labels cannot be used as indices; to retrieve the index of a model from its user label, use [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_INDEX_FROM_LABEL`](../../Reference/mod/MmodInquire.md). |

### `MaskBufferId` *(in, AIL_ID)*

Specifies the identifier of the image buffer used to identify the masked pixels in the model. This buffer must be a 1-band 8-bit unsigned buffer for a "don't care" or "flat region" mask. For a "weighted region" mask, the buffer must be a 1-band 8-bit signed buffer. For [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) and [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) types of contexts, the buffer must be of the same size as the model. For [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) types of models, the buffer must be the same size as the smallest buffer in which you can draw the model box. You can inquire the model buffer size using [`MmodInquire`](../../Reference/mod/MmodInquire.md) with [`M_ALLOC_SIZE_X`](../../Reference/mod/MmodInquire.md) and [`M_ALLOC_SIZE_Y`](../../Reference/mod/MmodInquire.md). This buffer can be calibrated; however, even if the model is calibrated, the mask is applied on a pixel basis. Setting the [`MaskBufferId`](../../Reference/mod/MmodMask.md) parameter to `M_NULL` when the [`MaskType`](../../Reference/mod/MmodMask.md) parameter is set to [`M_DONT_CARE`](../../Reference/mod/MmodMask.md), [`M_FLAT_REGIONS`](../../Reference/mod/MmodMask.md), or [`M_WEIGHT_REGIONS`](../../Reference/mod/MmodMask.md) removes the mask.

### `MaskType` *(in, AIL_INT64)*

Specifies which type of mask to employ. Set this parameter to one of the following values:

*For specifying the type of mask*

| Value | Description |
| --- | --- |
| `M_DONT_CARE` | Specifies that features present in the masked region (non-zero pixels) are ignored during the search. Features present in this region of the occurrence will not affect the scores. |
| `M_FLAT_REGIONS` | Specifies that no features are expected in the masked region (non-zero pixels). If features are present in this region of the occurrence, the target score will decrease. |
| `M_WEIGHT_REGIONS` | Specifies that the pixels corresponding to the model's active edges are weighted according to the values of the weighted region mask. The weights affect the calculation of the score; although the weights themselves don't affect the target score, edges corresponding to negative weights will be ignored when calculating the target score. The valid mask weights range from -127 to +127.

Positive weights indicate how significant it is for a given pixel to be part of an edge in the occurrence; the more positive the weight, the greater the influence on the score. The absence of a pixel with a very positive weight will reduce the score more than the absence of one with a lower weight.

The longer the edges with a positive weight, the more significant the edges to the target coverage and to the target score.

Negative weights indicate how significant it is for a pixel not to be part of an edge in the occurrence; the more negative the weight, the greater the influence on the score. The presence of a pixel with a very negative weight will reduce the score more than the presence of one with a less negative weight.

Note that weights that have no correspondence to edges in the model have no effect on the score. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
