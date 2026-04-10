---
doctype: Reference
module: 3dim
function: M3dimFillGaps
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimFillGaps"
---

# M3dimFillGaps

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

> Fill gaps in a depth map and, optionally, an intensity map, according to gap filling context controls.

## Syntax

```c
void M3dimFillGaps(
    AIL_ID    GapFillingContext3dimId,  //in
    AIL_ID    DepthMapImageBufId,       //out
    AIL_ID    IntensityMapImageBufId,   //out
    AIL_INT64 ControlFlag               //in
)
```

## Description

This function replaces missing or invalid data (gaps) in a depth map and/or intensity map. You can choose to fill gaps with interpolated values, or propagate a boundary value across the gap. The gap filling operation is performed according to the context specified with [`GapFillingContext3dimId`](../../Reference/3dim/M3dimFillGaps.md).

For some applications, you can reasonably approximate gap values with an interpolation of neighboring values. In other situations, such as when the gap spans an abrupt change in depth, or the gap is very wide, you can choose to propagate one of the gap's boundary values across the gap. To specify how wide a gap to fill and other gap filling settings, use [`M3dimControl`](../../Reference/3dim/M3dimControl.md).

Gaps are filled in a 2-step process: first along X, and then along Y (or vice versa). For example, if you choose to fill first along the X-direction, all gaps along rows in the depth or intensity map are evaluated and filled before any columns are examined. Therefore, gaps are examined not as an X-by-Y area to be filled, but on a per-row (or column) basis. To set the mode in which to fill gaps, use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_FILL_MODE`](../../Reference/3dim/M3dimControl.md) and [`M_X_THEN_Y`](../../Reference/3dim/M3dimControl.md) or [`M_Y_THEN_X`](../../Reference/3dim/M3dimControl.md).

## Parameters

### `GapFillingContext3dimId` *(in, AIL_ID)*

Specifies a fill gaps 3D image processing context.

*For specifying the fill gaps 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default fill gaps 3D image processing context of the current Aurora Imaging Library application.

> **Note:** The fill gaps operation will use default values for all gap filling control types listed in [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |
| `Fill gaps 3D image processing context identifier` | Specifies the identifier of a fill gaps 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILL_GAPS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the fill gaps control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `DepthMapImageBufId` *(out, AIL_ID)*

Specifies the depth map container or image buffer identifier of the depth map on which to fill gaps.

### `IntensityMapImageBufId` *(out, AIL_ID)*

Specifies the image buffer identifier of the intensity map on which to fill gaps. The image buffer must be an 8-bit or 16-bit unsigned, 1-band or 3-band buffer. The image buffer must have the same dimensions as the buffer specified for [`DepthMapImageBufId`](../../Reference/3dim/M3dimFillGaps.md) (or, if [`DepthMapImageBufId`](../../Reference/3dim/M3dimFillGaps.md) is a depth map container, the same dimensions as the range component).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
