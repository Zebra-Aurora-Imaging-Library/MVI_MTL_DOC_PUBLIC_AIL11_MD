---
doctype: Reference
module: meas
function: MmeasGetScore
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasGetScore"
---

# MmeasGetScore

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

> Retrieves information about the specified score characteristic.

## Syntax

```c
void MmeasGetScore(
    AIL_ID       MarkerId,        //in
    AIL_INT64    Characteristic,  //in
    AIL_INT      Index,           //in
    AIL_DOUBLE * Param1Ptr,       //out
    AIL_DOUBLE * Param2Ptr,       //out
    AIL_DOUBLE * Param3Ptr,       //out
    AIL_DOUBLE * Param4Ptr,       //out
    AIL_DOUBLE * Param5Ptr,       //out
    AIL_INT *    InputUnitsPtr,   //out
    AIL_INT64    ControlFlag      //in
)
```

## Description

This function retrieves information about the specified score characteristic of an edge, stripe, or circle marker. When using this function with [`M_RESULT`](../../Reference/meas/MmeasGetScore.md), you will get the results of the score characteristic that Aurora Imaging Library established in the target image after your last call to [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), as well as the value of the characteristic established by the find operation. When using this function with [`M_MARKER`](../../Reference/meas/MmeasGetScore.md), you will get the values that you specified for the score characteristic's settings with your last call to [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md), as well as the value of the character specified in the marker. Before using this function with [`M_RESULT`](../../Reference/meas/MmeasGetScore.md), you must first call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md).

The information that you can retrieve for each score characteristic is based on its settings in [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). If you try to get the results ([`M_RESULT`](../../Reference/meas/MmeasGetScore.md)) of a score characteristic that you did not set using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md), you will receive an error. However, if you inquire ([`M_MARKER`](../../Reference/meas/MmeasGetScore.md)) about a score characteristic that you did not set, you will receive the default (initial) values of that characteristic, as indicated in [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md).

If you have associated the target image with a camera calibration context ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)), you can specify that certain values be interpreted in world units. To do so, you must have used [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with [`M_WORLD`](../../Reference/meas/MmeasSetScore.md). Note that if you used [`M_WORLD`](../../Reference/meas/MmeasSetScore.md) but didn't use a calibrated target image, the function will generate an error.

## Parameters

### `MarkerId` *(in, AIL_ID)*

Specifies the identifier of the measurement marker buffer.

### `Characteristic` *(in, AIL_INT64)*

Specifies the score characteristic about which to get information.

*For retrieving information about a score characteristic for an edge, stripe, or circle marker*

| Value | Description |
| --- | --- |
| `M_EDGE_CONTRAST_SCORE` | Retrieves information about the grayscale difference in intensity between the start ([`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md)) and end ([`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md)) of the intensity transition from which the marker's edges are established. |
| `M_EDGEVALUE_PEAK_CONTRAST_SCORE` | Retrieves information about the grayscale difference of the intensity transition between the first zero edgevalues on both sides of the established edge peak (before [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md), and after [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md)).

Note that some images might never have a zero edgevalue, in which case you should use [`M_EDGE_CONTRAST_SCORE`](../../Reference/meas/MmeasGetScore.md) for a more meaningful result. |
| `M_STRENGTH_SCORE` | Retrieves information about the greatest edgevalue (strength) of the marker. |

*For retrieving information about a score characteristic for an edge or stripe marker*

| Value | Description |
| --- | --- |
| `M_DISTANCE_FROM_BOX_ORIGIN_SCORE` | Retrieves information about the distance between the position of the box search region's origin and the position of the marker. |
| `M_SPACING_SCORE` | Retrieves information about the typical distance (spacing) between consecutive edge or stripe occurrences of a multiple-occurrence marker. |

*For retrieving information about a score characteristic for a stripe marker*

| Value | Description |
| --- | --- |
| `M_EDGE_INSIDE_SCORE` | Retrieves information about the number of edges inside a stripe marker. |
| `M_STRIPE_WIDTH_SCORE` | Retrieves information about the distance (width) between the stripe's two outermost edges. |

*For retrieving information about a score characteristic for a circle marker*

| Value | Description |
| --- | --- |
| `M_RADIUS_SCORE` | Retrieves information about the linear distance between the circle's center and its circumference (radius). |

*For specifying the stripe's edge*

| Value | Description |
| --- | --- |
| `M_EDGE_FIRST` | Retrieves results for the first outermost edge of a stripe. This must be combined with [`M_RESULT`](../../Reference/meas/MmeasGetScore.md). |
| `M_EDGE_SECOND` | Retrieves results for the second outermost edge of a stripe. This must be combined with [`M_RESULT`](../../Reference/meas/MmeasGetScore.md). |

*For specifying whether to retrieve information about the results of, or the values set for, the specified score characteristic*

| Value | Description |
| --- | --- |
| `M_MARKER` | Retrieves the values that you specified for the score characteristic's settings with your last call to [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). |
| `M_RESULT` | Retrieves the results of the score characteristic after your last call to [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md). Aurora Imaging Library returns values using the input units specified using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). |

### `Index` *(in, AIL_INT)*

Specifies the index of the occurrence of a multiple-occurrence marker. This parameter must be set to one of the following values:

*For specifying the occurrence of a multiple-occurrence marker*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the index information is not applicable. Use this value when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md). |
| `Value >= 0` | Specifies the index of the occurrence of a multiple-occurrence marker ([`MarkerId`](../../Reference/meas/MmeasGetScore.md)), when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md).

If you are using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md), but did not specify a multiple-occurrence marker, the index must be zero. |

### `Param1Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write either the characteristic's resulting value (when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md)) or the characteristic's minimum acceptable limit, as indicated using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with the [`Min`](../../Reference/meas/MmeasSetScore.md) parameter (when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md)).

### `Param2Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write either the characteristic's resulting score (when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md)) or the characteristic's lowest acceptable limit, as indicated using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with the [`Low`](../../Reference/meas/MmeasSetScore.md) parameter (when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md)).

### `Param3Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write either the greatest value that Aurora Imaging Library considers theoretically possible for the characteristic (when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md)) or the characteristic's highest acceptable limit, as indicated using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with the [`High`](../../Reference/meas/MmeasSetScore.md) parameter (when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md)).

### `Param4Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the characteristic's maximum acceptable limit, as indicated using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with the [`Max`](../../Reference/meas/MmeasSetScore.md) parameter (when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md)); otherwise, you must specify `M_NULL`, indicating that Aurora Imaging Library does not return any information to this parameter (when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md)).

### `Param5Ptr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the characteristic's score-offset, as indicated using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with the [`ScoreOffset`](../../Reference/meas/MmeasSetScore.md) parameter (when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md)); otherwise, you must specify `M_NULL`, indicating that Aurora Imaging Library does not return any information to this parameter (when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md)).

### `InputUnitsPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the characteristic's input units, as indicated using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with the [`InputUnits`](../../Reference/meas/MmeasSetScore.md) parameter (when using [`M_MARKER`](../../Reference/meas/MmeasGetScore.md)); otherwise, you must specify `M_NULL`, indicating that Aurora Imaging Library does not return any information to this parameter (when using [`M_RESULT`](../../Reference/meas/MmeasGetScore.md)).

*For retrieving the input units*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_PIXEL`](../../Reference/meas/MmeasSetScore.md), if the score characteristic's settings are affected by input units. Otherwise, [`M_DEFAULT`](../../Reference/meas/MmeasSetScore.md) specifies that the input unit information should be ignored. |
| `M_PIXEL` | Specifies that the values are retrieved in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that the values are retrieved in world units, with respect to the relative coordinate system. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
