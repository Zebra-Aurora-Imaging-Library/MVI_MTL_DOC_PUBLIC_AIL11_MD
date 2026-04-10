---
doctype: Reference
module: im
function: MimWatershed
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimWatershed"
---

# MimWatershed

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

> Perform a watershed transformation.

## Syntax

```c
void MimWatershed(
    AIL_ID    SrcImageBufId,     //in
    AIL_ID    MarkerImageBufId,  //in
    AIL_ID    DstImageBufId,     //out
    AIL_INT   MinVariation,      //in
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function performs a watershed transformation on the specified source buffer. The catchment basins of the source buffer can be determined from extrema (minima or maxima) in the source buffer and/or from a specified marker image. In the case when no marker image is specified, a valid catchment basin is determined from an extrema when the minimum variation in gray levels of an extremum's catchment basin is equal to or greater than the value specified by the [`MinVariation`](../../Reference/im/MimWatershed.md) parameter. In the case when a marker image is specified, each marker of a marker image produces catchment basin(s) by forcing an extremum in the corresponding area(s) of the source buffer.

There are two types of marker images: non-labeled and labeled. In a non-labeled marker image, each group of touching pixels with the value zero in the marker image (known as a marker) starts a catchment basin in the corresponding area of the source image. Specifically, each marker in the marker image forces a extremum in the corresponding area of the source image. Pixels in the marker image are considered touching if they are vertically, horizontally, or diagonally adjacent, that is, if they are "8-connected". Note that each catchment basin in the destination image is given a unique grayscale value, starting at 1.

In a labeled marker image, a marker is a set of pixels that do not have to necessarily touch and that have all the same label value (pixel intensity). Each marker starts a catchment basin in the corresponding area of the source image and each catchment basin of the destination image is assigned the label value of the marker that generated it. Note that, in a labeled marker image, a marker can touch other markers, allowing you to specify adjacent catchment basins. Valid marker label values are`1 to 2<sup>n</sup>- 2` (where _n_ refers to the number of bits per pixel in the buffer). Pixels with the label value of zero are interpreted as the region of the image that is not a marker. Finally, pixels with the label value of `2<sup>n</sup>- 1` are considered to be part of a "don't care" mask and are not processed, accelerating the watershed transformation.

You can specify that the destination buffer contain one of the following:

- Watershed lines. The watershed lines are given the value 0 and all other pixels are given the maximum value in the destination buffer.
- Labeled catchment basins, without watershed lines. Each catchment basin is assigned a grayscale value greater or equal to 1.
- Labeled catchment basins and watershed lines. Each catchment basin is assigned a grayscale value greater or equal to 1. Watershed lines are given the value 0.

For more information, see [Watershed transformations](../../UserGuide/C04_Advanced_image_processing/S05_Watershed_transformations.md).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the buffer on which to perform the transform. This buffer can be 8-bit or 16-bit, signed or unsigned.

### `MarkerImageBufId` *(in, AIL_ID)*

Specifies the buffer to use as a marker image. This buffer can be 8-bit or 16-bit, signed or unsigned. If you are not using a marker image, set this parameter to `M_NULL`.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the buffer in which to place the results of the transformation. This buffer can be 8-bit or 16-bit, signed or unsigned. The destination buffer can hold `2<sup>n</sup> - 5` catchment basins, where _n_ refers to the number of bits per pixel in the buffer. Therefore, an 8-bit destination buffer can hold 251 catchment basins and a 16-bit destination buffer can hold 65531 catchment basins.

### `MinVariation` *(in, AIL_INT)*

Specifies the minimum variation of gray-level of a catchment basin. This parameter must be set to one of the values below.

*For specifying the minimum variation of gray-level*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Produces a catchment basin from each extremum in the source buffer. Setting [`MinVariation`](../../Reference/im/MimWatershed.md) to 1 will have the same effect. |
| `M_OFF` | Specifies that catchment basins will not be determined from extrema in the source buffer. |
| `Value` | Sets the minimum variation. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to perform the transformation. It can be set to any combination of the following seven sets of values below. It can also be set to [`M_DEFAULT`](../../Reference/im/MimWatershed.md), in which case the default value from each set is used (without [`M_SKIP_LAST_LEVEL`](../../Reference/im/MimWatershed.md), [`M_LABELED_MARKER`](../../Reference/im/MimWatershed.md)and [`M_FILL_SOURCE`](../../Reference/im/MimWatershed.md)). If no value from a set is specified, its default value is used.

*For specifying how to perform the transformation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_WATERSHED`](../../Reference/im/MimWatershed.md) + [`M_MINIMA_FILL`](../../Reference/im/MimWatershed.md) + [`M_REGULAR`](../../Reference/im/MimWatershed.md) + [`M_4_CONNECTED`](../../Reference/im/MimWatershed.md). |
| `M_BASIN` | Labels catchment basins. |
| `M_WATERSHED` | Calculates watershed lines. |
| `M_WATERSHED + M_BASIN` | Labels catchment basin and calculates watershed lines. |

*For specifying whether to use the source buffer's minima or maxima*

| Value | Description |
| --- | --- |
| `M_MAXIMA_FILL` | Uses the source buffer's maxima. |
| `M_MINIMA_FILL` *(default)* | Uses the source buffer's minima. |

*For specifying how the watershed lines are drawn*

| Value | Description |
| --- | --- |
| `M_REGULAR` *(default)* | Traces the watershed lines exactly. |
| `M_STRAIGHT_WATERSHED` | Forces the watershed lines to be straight. Note that selecting [`M_STRAIGHT_WATERSHED`](../../Reference/im/MimWatershed.md) automatically selects the [`M_SKIP_LAST_LEVEL`](../../Reference/im/MimWatershed.md) setting. |

*For specifying 4 or 8-connected watershed lines*

| Value | Description |
| --- | --- |
| `M_4_CONNECTED` *(default)* | Uses 4-connected watershed lines. |
| `M_8_CONNECTED` | Uses 8-connected watershed lines. |

*For preventing an extremum's zone of influence from extending past the minimum and maximum gray level*

| Value | Description |
| --- | --- |
| `M_SKIP_LAST_LEVEL` | Prevents an extremum's zone of influence from extending beyond `L<sub>max</sub> - 1` (for a minimum) or `L<sub>min</sub> - 1` (for a maximum), where `L<sub>max</sub>` is the maximum gray level in the image and `L<sub>min</sub>` is the minimum gray level. |

*For denoting that the marker image is labeled*

| Value | Description |
| --- | --- |
| `M_LABELED_MARKER` | Specifies that the marker image is a labeled marker image. |

*For removing unwanted extrema*

| Value | Description |
| --- | --- |
| `M_FILL_SOURCE` | Fill all local basins of a valid catchment basin in the source image. |
