---
doctype: Reference
module: 3dmeas
function: M3dmeasDraw3d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasDraw3d"
---

# M3dmeasDraw3d

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

> Draw specific features of templates, paths, profiles, or markers into a 3D graphics list.

## Syntax

```c
AIL_INT64 M3dmeasDraw3d(
    AIL_ID    OperationDraw3dContext3dmeasId,  //in
    AIL_ID    SrcResult3dmeasId,               //in
    AIL_INT64 PathOrTemplateIndex,             //in
    AIL_INT64 ProfileIndex,                    //in
    AIL_INT64 MarkerIndex,                     //in
    AIL_ID    DstList3dgraId,                  //out
    AIL_INT64 DstParentLabel,                  //in
    AIL_INT64 ControlFlag                      //in
)
```

## Description

This function draws specific template, path, profile, or marker features, stored in the specified 3D measurement result buffer, into a 3D graphics list. Set the draw operations and options for the draw using [`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md).

## Parameters

### `OperationDraw3dContext3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the draw 3D measurement context that specifies the annotations to draw and how to draw them.

*For specifying the draw 3D measurement context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a predefined draw 3D measurement context.

The default context is predefined with all draw operations ([`M3dmeasControlDraw`](../../Reference/3dmeas/M3dmeasControlDraw.md)) set to use default settings. |
| `Draw 3D measurement context identifier` | Specifies a valid draw 3D measurement context identifier, which you have allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), [`M_DRAW_3D_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md), or [`M_DRAW_3D_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |

### `SrcResult3dmeasId` *(in, AIL_ID)*

Specifies the identifier of the profile, path, or template 3D measurement result buffer, previously allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_PROFILE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), [`M_FIND_MARKER_PATH_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), or [`M_FIND_MARKER_TEMPLATE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md), respectively.

### `PathOrTemplateIndex` *(in, AIL_INT64)*

Specifies the path or template to draw. Set this parameter to one of the following values:

*For specifying a template or path*

| Value | Description |
| --- | --- |
| `M_PATH_INDEX` | Specifies the path in the path 3D measurement result buffer to draw, if one is specified. |
| `M_TEMPLATE_INDEX` | Specifies the template in the template 3D measurement result buffer to draw, if one is specified. |
| `M_DEFAULT_TEMPLATE` | Specifies to draw the default template in the profile 3D measurement result buffer, if one is specified.

Note that the default template in a profile 3D measurement context is different from an explicitly defined template in a template 3D measurement context. The default template is inherent to the supplied profiles, such that it is the theoretical template that would result in these profiles. Note, unlike for the profiles perpendicular to the template in a template 3D measurement context, multiple markers can be found along the profiles perpendicular to the default template in a profile 3D measurement context. |

### `ProfileIndex` *(in, AIL_INT64)*

Specifies the profile to draw. Set this parameter to one of the following values:

*For specifying a profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as **M_PROFILE_INDEX()**. |
| `M_PROFILE_INDEX` | Specifies the profile in the specified 3D measurement result buffer to draw. |

### `MarkerIndex` *(in, AIL_INT64)*

Specifies the marker to draw. Set this parameter to one of the following values:

*For specifying a marker*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to draw features of all the markers. |
| `Value >= 0` | Specifies the index of the markers for which to draw features. |

### `DstList3dgraId` *(out, AIL_ID)*

Specifies the identifier of the 3D graphics list in which to draw. The 3D graphics list must have been previously allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `DstParentLabel` *(in, AIL_INT64)*

Specifies the label of the 3D graphic in the 3D graphics list to use as the annotation's parent.

*For specifying the parent label*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ROOT_NODE` *(default)* | Specifies the top-most node of the 3D graphics list. |
| `Value >= 0` | Specifies the label of the 3D graphic in the 3D graphics list. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Return Value

**Type:** `AIL_INT64`

Returns the label of the 3D graphic added to the 3D graphics list.
