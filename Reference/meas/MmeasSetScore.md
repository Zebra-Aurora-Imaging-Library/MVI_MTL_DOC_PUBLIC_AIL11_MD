---
doctype: Reference
module: meas
function: MmeasSetScore
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasSetScore"
---

# MmeasSetScore

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

> Set a score characteristic of a marker.

## Syntax

```c
void MmeasSetScore(
    AIL_ID     MarkerId,        //out
    AIL_INT64  Characteristic,  //in
    AIL_DOUBLE Min,             //in
    AIL_DOUBLE Low,             //in
    AIL_DOUBLE High,            //in
    AIL_DOUBLE Max,             //in
    AIL_DOUBLE ScoreOffset,     //in
    AIL_INT64  InputUnits,      //in
    AIL_INT64  ControlFlag      //in
)
```

## Description

This function sets a score characteristic of a marker. Aurora Imaging Library uses score characteristics to determine the best edge, stripe, or circle marker. This function only processes markers that have all the essential characteristics specified with [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md).

Unlike essential characteristics, which are either met or not, score characteristics can have a range of acceptable values; based on where the actual characteristic result falls within these specified limits, Aurora Imaging Library calculates the characteristic's score. To calculate the final score of the marker itself ([`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_SCORE_TOTAL`](../../Reference/meas/MmeasGetResult.md)), Aurora Imaging Library multiplies the score of all the marker's score characteristics. The marker with the highest final score is the one located by [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md).

To retrieve the characteristic's score, as well as other related information, such as the actual value of the characteristic (for example, the contrast of the marker in the image), you can use [`MmeasGetScore`](../../Reference/meas/MmeasGetScore.md) with [`M_RESULT`](../../Reference/meas/MmeasGetScore.md) (after calling [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)). To inquire the specified settings for a score characteristic, use [`MmeasGetScore`](../../Reference/meas/MmeasGetScore.md) with [`M_MARKER`](../../Reference/meas/MmeasGetScore.md).

To set the acceptable limits of a score characteristic, use the [`Min`](../../Reference/meas/MmeasSetScore.md), [`Low`](../../Reference/meas/MmeasSetScore.md), [`High`](../../Reference/meas/MmeasSetScore.md), and [`Max`](../../Reference/meas/MmeasSetScore.md) parameters. The values for these parameters must adhere to the following conditions: `[`Min`](../../Reference/meas/MmeasSetScore.md) &lt;= [`Low`](../../Reference/meas/MmeasSetScore.md) &lt;= [`High`](../../Reference/meas/MmeasSetScore.md) &lt;= [`Max`](../../Reference/meas/MmeasSetScore.md)`. If these conditions are not met, Aurora Imaging Library generates an error. Characteristics that are greater than or equal to [`Low`](../../Reference/meas/MmeasSetScore.md) and less than or equal to [`High`](../../Reference/meas/MmeasSetScore.md) receive a score of 100%, while those that are greater than or equal to [`Min`](../../Reference/meas/MmeasSetScore.md) and less than [`Low`](../../Reference/meas/MmeasSetScore.md), or greater than [`High`](../../Reference/meas/MmeasSetScore.md) and less than or equal to [`Max`](../../Reference/meas/MmeasSetScore.md), receive a score above 0% and below 100%. Characteristics that are less than [`Min`](../../Reference/meas/MmeasSetScore.md) or greater than [`Max`](../../Reference/meas/MmeasSetScore.md) receive a score of 0% and are discarded.

You can set all limits except [`Min`](../../Reference/meas/MmeasSetScore.md) to [`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md), which represents a predetermined maximum established by Aurora Imaging Library for that characteristic (the smallest possible value for any characteristic is always 0). Once a limit has been set to [`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md), all subsequent limits must also be set to [`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md). For example, if you are using [`M_EDGE_CONTRAST_SCORE`](../../Reference/meas/MmeasSetScore.md), and set the [`Low`](../../Reference/meas/MmeasSetScore.md) parameter to [`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md), you must also set the [`High`](../../Reference/meas/MmeasSetScore.md) and [`Max`](../../Reference/meas/MmeasSetScore.md) parameters to [`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md).

By default, Aurora Imaging Library only calculates the score of the marker's edge strength ([`M_STRENGTH_SCORE`](../../Reference/meas/MmeasSetScore.md)); therefore, the marker with the highest edgevalue (and all the essential characteristics) is found. When using another score characteristic to find the best marker, you might want to ignore the strength characteristic, to avoid its multiplicative influence. To do so, specify [`M_STRENGTH_SCORE`](../../Reference/meas/MmeasSetScore.md) and set [`Min`](../../Reference/meas/MmeasSetScore.md) and [`Low`](../../Reference/meas/MmeasSetScore.md) to 0, and set [`High`](../../Reference/meas/MmeasSetScore.md) and [`Max`](../../Reference/meas/MmeasSetScore.md) to [`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md).

You can reduce the influence of a characteristic on the total score by using a score offset. Aurora Imaging Library calculates the lowest-possible nonzero score by subtracting the specified score-offset from 100% `(100% - [`ScoreOffset`](../../Reference/meas/MmeasSetScore.md) = _LowestPossibleScore_).` Since characteristics can, by default, score above 0%, the default score-offset is equivalent to 100% (calculated as, `100% - 100% = 0%`). Unless you are developing an advanced application, the default is typically appropriate. Values below [`Min`](../../Reference/meas/MmeasSetScore.md) and above [`Max`](../../Reference/meas/MmeasSetScore.md) are set to 0, whereas values between [`Min`](../../Reference/meas/MmeasSetScore.md) and [`Low`](../../Reference/meas/MmeasSetScore.md) are mapped to scores between this _ LowestPossibleScore_ and 100%, and values between [`High`](../../Reference/meas/MmeasSetScore.md) and [`Max`](../../Reference/meas/MmeasSetScore.md) are mapped to scores between 100% and this _ LowestPossibleScore_.

## Parameters

### `MarkerId` *(out, AIL_ID)*

Specifies the identifier of the measurement marker for which to set the score characteristic.

### `Characteristic` *(in, AIL_INT64)*

Specifies the score characteristic to set.

*For specifying a score characteristic for an edge, stripe, or circle marker*

| Value | Description |
| --- | --- |
| `M_EDGE_CONTRAST_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the grayscale difference between the start and end of the intensity transition from which the marker's edge is established. To retrieve the start and end of the intensity transition, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md) and [`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md).

The theoretical limit of the greatest possible contrast value ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) depends on the target image. For example, for an 8-bit image, the greatest possible contrast is 255. |
| `M_EDGEVALUE_PEAK_CONTRAST_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the grayscale difference of the intensity transition between the first zero edgevalues on both sides of the established edge peak. To retrieve the minimum and maximum position of the edge peak, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md) and [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md).

Note that some search regions might never have a zero edgevalue; in which case, you should use [`M_EDGE_CONTRAST_SCORE`](../../Reference/meas/MmeasSetScore.md) for a more meaningful result.

The theoretical limit of the greatest possible contrast value ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) depends on the target image. For example, for an 8-bit image, the greatest possible contrast is 255; this corresponds to an edgevalue of 100. |
| `M_STRENGTH_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the mean edgevalue (strength) of the marker.

The theoretical limit of the greatest possible edgevalue ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) is 100. |

*For specifying a score characteristic for an edge or stripe marker*

| Value | Description |
| --- | --- |
| `M_DISTANCE_FROM_BOX_ORIGIN_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the distance between the position of the box search region's origin and the position of the marker.

The theoretical limit of the greatest possible position-offset ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) depends on the relevant dimension of the box search region (either the box's width or height, depending on the orientation). For a multiple-occurrence marker, each occurrence must respect the specified limits; otherwise Aurora Imaging Library discards the marker. |
| `M_SPACING_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the typical distance (spacing) between consecutive edge or stripe occurrences of a multiple-occurrence marker.

The theoretical limit of the greatest possible spacing value ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) depends on the relevant dimension of the box search region (either the box's width or height, depending on the orientation). |

*For specifying a score characteristic for a stripe marker*

| Value | Description |
| --- | --- |
| `M_EDGE_INSIDE_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the number of edges inside a stripe marker.

The theoretical limit of the greatest possible number of edges ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) is equivalent to half the relevant dimension of the box search region (either `0.5 x _BoxWidth_` or `0.5 x _BoxHeight_,` depending on the orientation). |
| `M_STRIPE_WIDTH_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the distance (width) between the stripe's two outermost edges.

The theoretical limit of the greatest possible width value for the stripe ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) depends on the relevant dimension of the box search region (either the box's width or height, depending on the orientation). |

*For specifying a score characteristic for a circle marker*

| Value | Description |
| --- | --- |
| `M_RADIUS_SCORE` | Specifies that Aurora Imaging Library must calculate a score, as a percentage, based on the linear distance between the circle's center and its circumference (radius).

The theoretical limit of the greatest possible radius value for the circle ([`M_MAX_POSSIBLE_VALUE`](../../Reference/meas/MmeasSetScore.md)) is the length of the outer radius specified for the ring search region using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_RING_RADII`](../../Reference/meas/MmeasSetMarker.md). |

### `Min` *(in, AIL_DOUBLE)*

Specifies the minimum limit, which Aurora Imaging Library uses to calculate the characteristic's score. Any value less than the minimum limit receives a score of 0%.

*For specifying the minimum acceptable limit*

| Value | Description |
| --- | --- |
| `0 <= Value < Low` *(default)* | Specifies the minimum acceptable limit. |

### `Low` *(in, AIL_DOUBLE)*

Specifies the low limit, which Aurora Imaging Library uses to calculate the characteristic's score. Any value less than the lowest limit and greater than or equal to the minimum limit receives a score above 0% and below 100%, while any value greater than or equal to the lowest limit and less than or equal to the highest limit receives a score of 100%.

*For specifying the low acceptable limit*

| Value | Description |
| --- | --- |
| `M_MAX_POSSIBLE_VALUE` | Specifies that the low acceptable limit is equivalent to the greatest possible value. |
| `Min <= Value <= High` *(default)* | Specifies the low acceptable limit. |

### `High` *(in, AIL_DOUBLE)*

Specifies the high limit, which Aurora Imaging Library uses to calculate the characteristic's score. Any value greater than the highest limit and less than or equal to the maximum limit receives a score above 0% and below 100%.

*For specifying the high acceptable limit*

| Value | Description |
| --- | --- |
| `M_MAX_POSSIBLE_VALUE` *(default)* | Specifies that the high acceptable limit is equivalent to the greatest possible value. |
| `Low <= Value <= Max` | Specifies the high acceptable limit. |

### `Max` *(in, AIL_DOUBLE)*

Specifies the maximum limit, which Aurora Imaging Library uses to calculate the characteristic's score. Any value greater than the maximum limit receives a score of 0%.

*For specifying the maximum acceptable limit*

| Value | Description |
| --- | --- |
| `M_MAX_POSSIBLE_VALUE` *(default)* | Specifies that the maximum acceptable limit is equivalent to the greatest possible value. |
| `High <= Value <= M_MAX_POSSIBLE_VALUE` | Specifies the maximum acceptable limit. |

### `ScoreOffset` *(in, AIL_DOUBLE)*

Specifies the characteristic's score-offset. This allows you to indirectly specify the lowest non-zero score that a characteristic can receive. The measurement module subtracts the score-offset from 100% to establish the lowest possible score assigned to values between [`Min`](../../Reference/meas/MmeasSetScore.md) and above [`Max`](../../Reference/meas/MmeasSetScore.md) and values between [`Min`](../../Reference/meas/MmeasSetScore.md) and [`Low`](../../Reference/meas/MmeasSetScore.md).

*For specifying the score-offset*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 100.0%. This means that the lowest possible score is 0 (100%-100%=0). |
| `0.0 <= Value <= 100.0` | Specifies the score-offset, as a percentage. |

### `InputUnits` *(in, AIL_INT64)*

Specifies the units with which to interpret the [`Min`](../../Reference/meas/MmeasSetScore.md), [`Low`](../../Reference/meas/MmeasSetScore.md), [`High`](../../Reference/meas/MmeasSetScore.md), and [`Max`](../../Reference/meas/MmeasSetScore.md) parameter values, when applicable. This essentially sets the input coordinate system to use.

*For specifying the input units*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_PIXEL`](../../Reference/meas/MmeasSetScore.md), if the score characteristic's settings are affected by input units. Otherwise, [`M_DEFAULT`](../../Reference/meas/MmeasSetScore.md) specifies that the input unit information should be ignored. |
| `M_PIXEL` | Specifies that the values will be interpreted in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that the values will be interpreted in world units, with respect to the relative coordinate system. If world units are specified, calling [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) generates an error if the operation is not performed on a calibrated image. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
