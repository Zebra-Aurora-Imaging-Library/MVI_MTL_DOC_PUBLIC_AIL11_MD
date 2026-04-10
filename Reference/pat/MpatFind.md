---
doctype: Reference
module: pat
function: MpatFind
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / pat / MpatFind"
---

# MpatFind

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

> Find a pattern matching model(s) in the target image buffer.

## Syntax

```c
void MpatFind(
    AIL_ID ContextPatId,      //in
    AIL_ID TargetImageBufId,  //in
    AIL_ID ResultPatId        //out
)
```

## Description

This function searches for the model(s) defined in the specified Pattern Matching context and writes the results in the provided result buffer. Note, the Pattern Matching context must be preprocessed (using [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md)) before calling this function.

Use [`MpatControl`](../../Reference/pat/MpatControl.md) to configure the find operation before calling this function.

When there is more than one model, select the appropriate search mode using [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md). When the search mode is set to [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md), the search settings of the first model apply for the search of all models; otherwise, when using [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the find operation searches for all models using their own search settings. When using [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md), all models must be of the same size.

To specify the maximum number of occurrences for which to look in the target image, use [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_NUMBER`](../../Reference/pat/MpatControl.md). Note that this control type can also be used to temporarily ignore a model during a find operation, by setting [`M_NUMBER`](../../Reference/pat/MpatControl.md)to zero. When using the [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md) search mode, if [`M_NUMBER`](../../Reference/pat/MpatControl.md) is set to the zero for the first model, the subsequent model that does not have its [`M_NUMBER`](../../Reference/pat/MpatControl.md) set to zero will be used instead.

When the model's reference position is expected to be found in a certain region, you can specify this region using [`MpatControl`](../../Reference/pat/MpatControl.md) with[`M_SEARCH_OFFSET_X`](../../Reference/pat/MpatControl.md),[`M_SEARCH_OFFSET_Y`](../../Reference/pat/MpatControl.md), [`M_SEARCH_SIZE_X`](../../Reference/pat/MpatControl.md), and [`M_SEARCH_SIZE_Y`](../../Reference/pat/MpatControl.md). Note that, since you are setting the area in which to find the model's reference position, the search region can be smaller than the model. An alternative method to limiting your search region is to use a rectangular region of interest (ROI), set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). This might cause an increase in processing time, but it is more flexible than setting a search region using [`MpatControl`](../../Reference/pat/MpatControl.md); this is because you can define a search region in world units and set it at angle. If using an ROI, the values of [`M_SEARCH_OFFSET...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE...`](../../Reference/pat/MpatControl.md) are ignored and the ROI settings take precedence.

Do not use a child buffer to restrict the search region. Using a child buffer could result in edge effects from overscan pixels or incomplete neighborhoods. If the search region is as small as or smaller than the model, using a child buffer could produce invalid results.

To reset the search region size to the full image when using the control types to define the region, set [`M_SEARCH_OFFSET_X`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_OFFSET_Y`](../../Reference/pat/MpatControl.md) to zero (0), and [`M_SEARCH_SIZE_X`](../../Reference/pat/MpatControl.md)and [`M_SEARCH_SIZE_Y`](../../Reference/pat/MpatControl.md) to [`M_ALL`](../../Reference/pat/MpatControl.md).

This function assumes that the model was extracted from a source image with the same scaling as the target image. In addition, if angular search is disabled ([`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_DISABLE`](../../Reference/pat/MpatControl.md)), the model's source image and the target image must have the same orientation (with approximately 5° tolerance). To perform an angular search, use [`MpatControl`](../../Reference/pat/MpatControl.md) to enable the angular search and then use [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md)to set the nominal search angle and the angular search range of the model.

This function returns results in decreasing match-score order. This means that the most-likely occurrence is always returned first.

If a correlation has a match score greater than or equal to the certainty level ([`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_CERTAINTY`](../../Reference/pat/MpatControl.md)), it is automatically considered an occurrence (default 80%). The remaining occurrences will be the best of those greater than or equal to the acceptance level ([`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_ACCEPTANCE`](../../Reference/pat/MpatControl.md)).

Results are written to the specified result buffer. Use the [`MpatGetResult`](../../Reference/pat/MpatGetResult.md) to read the results.

## Parameters

### `ContextPatId` *(in, AIL_ID)*

Specifies the Pattern Matching context identifier to use when finding the search model in the target image.

### `TargetImageBufId` *(in, AIL_ID)*

Specifies the identifier of the target image. Note that this function only supports 8-bit unsigned grayscale images.

### `ResultPatId` *(out, AIL_ID)*

Specifies the identifier of the pattern matching result buffer in which to store results. The result buffer must have been previously allocated using [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md).
