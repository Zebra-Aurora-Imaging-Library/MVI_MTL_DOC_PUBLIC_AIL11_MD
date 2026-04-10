---
doctype: Reference
module: 3dmeas
function: M3dmeasDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmeas / M3dmeasDraw"
---

# M3dmeasDraw

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

> Draw specific features of templates, paths, profiles, or markers in an image buffer or 2D graphics list.

## Syntax

```c
void M3dmeasDraw(
    AIL_ID    ContextGraId,             //in
    AIL_ID    ContextOrResult3dmeasId,  //in
    AIL_ID    DstImageBufOrListGraId,   //out
    AIL_INT64 Operation,                //in
    AIL_INT64 PathOrTemplateIndex,      //in
    AIL_INT64 ProfileIndex,             //in
    AIL_INT64 MarkerIndex,              //in
    AIL_INT64 ControlFlag               //in
)
```

## Description

This function draws specific template, path, profile, or marker features in the destination image buffer or 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResult3dmeasId` *(in, AIL_ID)*

Specifies the 3D measurement context or result buffer from which to extract the features to draw. The 3D measurement context or result buffer must have been previously allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) or [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md), respectively.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. Operations can be added together to draw multiple features at a time. For example, to draw both the marker's profile region and the marker, you would set the [`Operation`](../../Reference/3dmeas/M3dmeasDraw.md) parameter to [`M_DRAW_PROFILE_REGION`](../../Reference/3dmeas/M3dmeasDraw.md)+[`M_DRAW_MARKERS`](../../Reference/3dmeas/M3dmeasDraw.md). The [`Operation`](../../Reference/3dmeas/M3dmeasDraw.md) parameter can be set to a combination of the following values.

*For extracting path features from a path 3D measurement context*

| Value | Description |
| --- | --- |
| `M_DRAW_PATH` | Draws the path as a line with an arrow head. |

*For extracting template features from a template 3D measurement context*

| Value | Description |
| --- | --- |
| `M_DRAW_TEMPLATE` | Draws the template as a line with an arrow head. |

*For extracting profile features from a path or template 3D measurement context or any 3D measurement result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_PROFILE_REGION` | Draws a rectangle around the profile. |
| `M_DRAW_PROFILE_REGION_DIRECTION` | Draws a line with an arrow pointing in the direction of the profile. |

*For extracting marker features from a 3D measurement result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_MARKERS` | Draws a cross-like symbol at the marker position and, for pair markers, draws an H-type line (|-|) connecting the two transitions. Note that this is equivalent to [`M_DRAW_MARKERS_POSITION`](../../Reference/3dmeas/M3dmeasDraw.md) and [`M_DRAW_TRANSITIONS`](../../Reference/3dmeas/M3dmeasDraw.md) for single markers and [`M_DRAW_MARKERS_POSITION`](../../Reference/3dmeas/M3dmeasDraw.md) + [`M_DRAW_MARKERS_WIDTH`](../../Reference/3dmeas/M3dmeasDraw.md) for pair markers. |
| `M_DRAW_MARKERS_POSITION` | Draws a cross-like symbol at the marker position. Note that this is equivalent to [`M_DRAW_MARKERS`](../../Reference/3dmeas/M3dmeasDraw.md) and [`M_DRAW_TRANSITIONS`](../../Reference/3dmeas/M3dmeasDraw.md) for single markers. |
| `M_DRAW_MARKERS_WIDTH` | Draws an H-type line (|-|) connecting the two transitions to indicate the width of the pair marker.

This operation is only supported for pair markers. |
| `M_DRAW_TRANSITIONS` | Draws a cross-like symbol at the marker's transition positions. Note that this is equivalent to [`M_DRAW_MARKERS`](../../Reference/3dmeas/M3dmeasDraw.md) and [`M_DRAW_MARKERS_POSITION`](../../Reference/3dmeas/M3dmeasDraw.md) for single markers. |

*For extracting fit features from a template 3D measurement result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_FIT_GEOMETRY` | Draws the fitted 3D line geometry. |
| `M_DRAW_FIT_MARKERS` | Draws a cross-like symbol at the marker positions that were used for the fit. |
| `M_DRAW_FIT_MARKERS_OUTLIERS` | Draws a cross-like symbol at the marker positions that were not used for the fit. |

### `PathOrTemplateIndex` *(in, AIL_INT64)*

Specifies the index of the path or template in the specified 3D measurement context or result buffer.

*For specifying the template or path*

| Value | Description |
| --- | --- |
| `M_PATH_INDEX` | Specifies that the drawing operation is for a path in the path 3D measurement context or result buffer, if one is specified. |
| `M_TEMPLATE_INDEX` | Specifies that the drawing operation is for a template in the template 3D measurement context or result buffer, if one is specified. |
| `M_DEFAULT_TEMPLATE` | Specifies that the drawing operation is for the default template in the profile 3D measurement result buffer, if one is specified.

Note that the default template in a profile 3D measurement context is different from an explicitly defined template in a template 3D measurement context. The default template is inherent to the supplied profiles, such that it is the theoretical template that would result in these profiles. Note, unlike for the profiles perpendicular to the template in a template 3D measurement context, multiple markers can be found along the profiles perpendicular to the default template in a profile 3D measurement context. |

### `ProfileIndex` *(in, AIL_INT64)*

Specifies the index of the profile in the specified 3D measurement context or result buffer.

*For specifying the profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as **M_PROFILE_INDEX()**. |
| `M_PROFILE_INDEX` | Specifies that the drawing operation is for a profile in the specified 3D measurement context or result buffer. |

### `MarkerIndex` *(in, AIL_INT64)*

Specifies the index of the marker in the specified 3D measurement result buffer.

*For specifying the marker*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies that the drawing operation is for all markers in the specified 3D measurement result buffer. |
| `Value >= 0` | Specifies the index of the marker in the specified 3D measurement result buffer. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. this parameter must be set to `M_DEFAULT`.
