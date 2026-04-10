---
doctype: Reference
module: 3dmeas
function: M3dmeasFit
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasFit"
---

# M3dmeasFit

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

> Fits a 3D line geometry to markers found in a template's perpendicular profiles.

## Syntax

```c
void M3dmeasFit(
    AIL_ID    FitContext3dmeasId,        //in
    AIL_ID    TemplateContext3dmeasId,   //in
    AIL_ID    DepthMapOrContainerBufId,  //in
    AIL_ID    TemplateResult3dmeasId,    //out
    AIL_INT64 ControlFlag                //in
)
```

## Description

This function fits a 3D line geometry to markers found in a template's perpendicular profiles. You can find the markers before calling this function, using [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md). This function can also find the markers and perform the fit simultaneously, in which case, all possible markers are considered and the ones that provide the best fit are kept.

If finding the markers before calling this function, pass the template 3D measurement result buffer containing the markers to which to fit the line geometry.

If finding the markers and performing the fit simultaneously, you must provide the template 3D measurement context and the depth map image buffer or container in which to search for markers.

You can use [`M3dmeasGetResult`](../../Reference/3dmeas/M3dmeasGetResult.md) with [`M_ANGULARITY`](../../Reference/3dmeas/M3dmeasGetResult.md), [`M_CENTER_DISTANCE`](../../Reference/3dmeas/M3dmeasGetResult.md), and [`M_DISTANCE_MAX`](../../Reference/3dmeas/M3dmeasGetResult.md) to compare the resulting fitted geometry to the defined template. Note that these comparison results are also available when the markers were previously found (because the template 3D measurement result buffer also holds the original template).

## Parameters

### `FitContext3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the fit 3D measurement context. The context must have been allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIT_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md).

### `TemplateContext3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the template 3D measurement context, when searching for the markers using this function. If [`TemplateResult3dmeasId`](../../Reference/3dmeas/M3dmeasFit.md) already contains found markers and you only want to perform the fit operation, set this parameter to `M_NULL`.

### `DepthMapOrContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the depth map container or depth map image buffer, when searching for the markers using this function. If [`TemplateResult3dmeasId`](../../Reference/3dmeas/M3dmeasFit.md) already contains found markers and you only want to perform the fit operation, set this parameter to `M_NULL`.

### `TemplateResult3dmeasId` *(out, AIL_ID)*

Specifies the identifier of the template 3D measurement result buffer. The result buffer must have been allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_TEMPLATE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), and, if [`TemplateContext3dmeasId`](../../Reference/3dmeas/M3dmeasFit.md) and [`DepthMapOrContainerBufId`](../../Reference/3dmeas/M3dmeasFit.md) are set to [`M_NULL`](../../Reference/3dmeas/M3dmeasFit.md), must contain the results of a call to [`M3dmeasFindMarker`](../../Reference/3dmeas/M3dmeasFindMarker.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
