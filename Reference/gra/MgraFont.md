---
doctype: Reference
module: gra
function: MgraFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraFont"
---

# MgraFont

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

> Associate a text font with a 2D graphics context.

## Syntax

```c
void MgraFont(
    AIL_ID    ContextGraId,  //out
    AIL_INT64 FontName       //in
)
```

## Description

This function associates a character font with the specified 2D graphics context for use with subsequent [`MgraText`](../../Reference/gra/MgraText.md) function calls.

To change the font for graphics already added to a 2D graphics list, use [`MgraControlList`](../../Reference/gra/MgraControlList.md).

## Parameters

### `ContextGraId` *(out, AIL_ID)*

Specifies the identifier of the 2D graphics context with which to associate the character font. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `FontName` *(in, AIL_INT64)*

Specifies the font with which to write text. This parameter can be set to one of the following:

*For specifying the font*

| Value | Description |
| --- | --- |
| `M_FONT_DEFAULT` | Same as [`M_FONT_DEFAULT_SMALL`](../../Reference/gra/MgraFont.md). |
| `M_FONT_DEFAULT_LARGE` | Specifies a large bitmap font, where each character is drawn in a 16x32 pixel area. |
| `M_FONT_DEFAULT_MEDIUM` | Specifies a medium bitmap font, where each character is drawn in a 12x24 pixel area. |
| `M_FONT_DEFAULT_SMALL` | Specifies a small bitmap font, where each character is drawn in a 8x16 pixel area. |
| `M_FONT_DEFAULT_TTF` | Specifies the default TrueType font of your operating system. |
| `"FontFamily:Weight:Slant"` | Specifies the font according to the following format, _Family_:_Weight_:_Slant_.

_Family_ must be set to the name of the font's family, such as Arial, Times New Roman, and Wingdings.

_Weight_ can be set to one of the following: Book, Thin, ExtraLight, UltraLight, Light, Normal, Regular, Medium, SemiBold, DemiBold, Bold, ExtraBold, UltraBold, Heavy, Black, ExtraBlack, or UltraBlack.

_Slant_ can be set to one of the following: Italic, Oblique, or Roman.

You can omit _Weight_ and _Slant_; also, you can provide the _Weight_ and _Slant_ in any order. |
| `"FontFile"` | Specifies a full path to a TrueType file name. |
