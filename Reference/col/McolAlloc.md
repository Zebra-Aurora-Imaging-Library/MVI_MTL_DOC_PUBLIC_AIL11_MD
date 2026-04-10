---
doctype: Reference
module: col
function: McolAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolAlloc"
---

# McolAlloc

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

> Allocate a color matching context or a relative color calibration context.

## Syntax

```c
AIL_ID McolAlloc(
    AIL_ID    SysId,                //in
    AIL_INT64 ContextType,          //in
    AIL_INT64 SourceColorSpace,     //in
    AIL_ID    ColorSpaceProfileId,  //in
    AIL_INT64 ControlFlag,          //in
    AIL_ID *  ContextIdPtr          //out
)
```

## Description

The context contains all the information necessary to perform [`McolMatch`](../../Reference/col/McolMatch.md) (for color matching) or [`McolTransform`](../../Reference/col/McolTransform.md) (for relative color calibration), including global context settings and color-samples. To add color-samples to a context, use [`McolDefine`](../../Reference/col/McolDefine.md). To adjust context and color-sample settings, use [`McolControl`](../../Reference/col/McolControl.md) and [`McolSetMethod`](../../Reference/col/McolSetMethod.md).

Aurora Imaging Library considers all color data to be the same as the context's color space, regardless of the color attributes of the data's buffer ([`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md)). For relative color calibration, color must be RGB (8-bit).

When the color matching context or relative color calibration context is no longer required, release it using[`McolFree`](../../Reference/col/McolFree.md)unless [`M_UNIQUE_ID`](../../Reference/col/McolAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/col/McolAlloc.md) was specified, the smart identifier manages the context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of context to allocate. This parameter must be set to one of the following values:

*For specifying the context*

| Value | Description |
| --- | --- |
| `M_COLOR_CALIBRATION_RELATIVE` | Allocates a relative color calibration context for use with [`McolTransform`](../../Reference/col/McolTransform.md). |
| `M_COLOR_MATCHING` | Allocates a color matching context for use with [`McolMatch`](../../Reference/col/McolMatch.md). |

### `SourceColorSpace` *(in, AIL_INT64)*

Specifies the source color space of the context. In general, all color data required for the color operation should be the same as the source color space. For example, if you specify an [`M_RGB`](../../Reference/col/McolAlloc.md) source color space, color-samples ([`McolDefine`](../../Reference/col/McolDefine.md)) and target or source images ([`McolMatch`](../../Reference/col/McolMatch.md) or [`McolTransform`](../../Reference/col/McolTransform.md)) should also typically be RGB.

*For specifying the color space*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RGB`](../../Reference/col/McolAlloc.md). |
| `M_CIELAB` | Specifies a CIELAB (or LAB) color space. This is only for color matching contexts.

[`M_CIELAB`](../../Reference/col/McolAlloc.md) is based on the color's luminance (L), its position between red and green (A), and its position between yellow and blue (B). Store the components of CIELAB data in color buffers as follows: the L component in band 0, the A component in band 1, and the B component in band 2. |
| `M_HSL` | Specifies an HSL color space. This is only for color matching contexts.

[`M_HSL`](../../Reference/col/McolAlloc.md) is based on the color's hue (H), saturation (S), and luminance (L). Store the components of HSL data in color buffers as follows: the H component in band 0, the S component in band 1, and the L component in band 2. |
| `M_RGB` | Specifies an RGB color space.

[`M_RGB`](../../Reference/col/McolAlloc.md) is based on the color's red (R), green (G), and blue (B) components. Store the components of RGB data in color buffers as follows: the R component in band 0, the G component in band 1, and the B component in band 2.

For color matching, when setting the color operation strategy with [`McolSetMethod`](../../Reference/col/McolSetMethod.md), you can also specify that, for internal calculations, you want to convert your RGB data to CIELAB before the match operation ([`McolMatch`](../../Reference/col/McolMatch.md)). |

### `ColorSpaceProfileId` *(in, AIL_ID)*

Specifies the context's color space profile. This is the standard Aurora Imaging Library uses to interpret the source color space data when performing an internal conversion between two distinct color spaces before the match operation (for example, specifying a CIELAB conversion mode with [`McolSetMethod`](../../Reference/col/McolSetMethod.md)prior to calling [`McolMatch`](../../Reference/col/McolMatch.md)). Unless you are performing such a conversion for the match, the color space profile standard has no effect.

*For specifying the context's color space profile*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use standard RGB specifications (sRGB), as defined by the International Electrotechnical Commission (IEC) Project Team 61966-2-1. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the color matching context or relative calibration context identifier or specifies the data type that the function should use to return the color matching context or relative calibration context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated color matching context or relative
                              color calibration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated color matching context or relative
                              color calibration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_COL_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the color matching context or relative
                              color calibration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the calibration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated relative color calibration context.

If allocation fails, [`M_NULL`](../../Reference/col/McolAlloc.md) is written as the identifier. |
| `Address in which to write the color matching context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated color matching context.

If allocation fails, [`M_NULL`](../../Reference/col/McolAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the color matching context or relative calibration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_COL_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/col/McolAlloc.md) was specified).
