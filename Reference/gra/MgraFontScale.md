---
doctype: Reference
module: gra
function: MgraFontScale
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraFontScale"
---

# MgraFontScale

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

> Set the font scale of a 2D graphics context.

## Syntax

```c
void MgraFontScale(
    AIL_ID     ContextGraId,  //out
    AIL_DOUBLE XFontScale,    //in
    AIL_DOUBLE YFontScale     //in
)
```

## Description

This function sets both the vertical and horizontal scaling factor settings of the specified 2D graphics context for use with subsequent [`MgraText`](../../Reference/gra/MgraText.md) function calls. These settings only apply to bitmap fonts. When using TrueType fonts, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_FONT_SIZE`](../../Reference/gra/MgraControl.md) to set the font size.

To change the font scale for graphics already added to a 2D graphics list, use [`MgraControlList`](../../Reference/gra/MgraControlList.md).

## Parameters

### `ContextGraId` *(out, AIL_ID)*

Specifies the identifier of the 2D graphics context for which to set the font scale. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `XFontScale` *(in, AIL_DOUBLE)*

Specifies the X-scale-factor by which to multiply the width of the font characters. This parameter can be independently set to any positive floating-point value. The default scale factor is 1.0.

### `YFontScale` *(in, AIL_DOUBLE)*

Specifies the Y-scale-factor by which to multiply the height of the font characters. This parameter can be independently set to any positive floating-point value. The default scale factor is 1.0.
