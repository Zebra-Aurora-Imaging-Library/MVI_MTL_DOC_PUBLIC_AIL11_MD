---
doctype: Reference
module: im
function: MimBinarize
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimBinarize"
---

# MimBinarize

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

> Perform a point-to-point binary threshold operation.

## Syntax

```c
AIL_INT MimBinarize(
    AIL_ID     SrcImageBufId,           //in
    AIL_ID     DstImageBufId,           //out
    AIL_INT64  ConditionAndThreshMode,  //in
    AIL_DOUBLE LowParam,                //in
    AIL_DOUBLE HighParam                //in
)
```

## Description

This function performs a binary threshold operation on the specified image. Each pixel that meets the specified condition is set to the highest unsigned destination buffer value, while other pixels are set to 0. For example, the highest buffer value for an 8-bit buffer is 0xff (regardless if the source buffer is signed). If a floating-point destination buffer is specified, pixels that meet the condition are set to 1.

There are 6 ways to establish the threshold value(s): two that require you to explicitly specify the threshold value(s) ([`M_FIXED`](../../Reference/im/MimBinarize.md) and [`M_PERCENTILE_VALUE`](../../Reference/im/MimBinarize.md)) and four that automatically base the threshold value(s) on the histogram ([`M_BIMODAL`](../../Reference/im/MimBinarize.md), [`M_DOMINANT`](../../Reference/im/MimBinarize.md), [`M_TRIANGLE_BISECTION_DARK`](../../Reference/im/MimBinarize.md), and [`M_TRIANGLE_BISECTION_BRIGHT`](../../Reference/im/MimBinarize.md)).

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to write the results. In general, this parameter must be given an image buffer identifier.

### `ConditionAndThreshMode` *(in, AIL_INT64)*

Specifies the condition, as well as the threshold mode to use to establish the threshold value(s) of the condition.

### `LowParam` *(in, AIL_DOUBLE)*

Specifies either the lower limit of the selected condition or the minimum value to use when establishing the threshold.

### `HighParam` *(in, AIL_DOUBLE)*

Specifies either the upper limit of the selected condition or the maximum value to use when establishing the threshold.

## Parameter Associations

### For controlling the threshold determination mode

The following conditions allow you to control the threshold determination mode used.

---

### `M_BIMODAL`

Specifies to automatically establish the threshold value for the condition from the source image's histogram, using the bimodal threshold mode.  In this mode, if the source image's histogram contains only two peaks, the threshold value is set to the intensity value at the lowest point between these two peaks of the histogram, on the assumption that these peaks represent the object and the background. If the histogram contains more than two peaks, the threshold value will typically be set between the intensity values at the two principal peaks, although exceptions exist for the situations where the principal peaks occur are closest to the minimum intensity value (pure black) or the maximum intensity value (full saturation). Note that the bimodal threshold mode works best when the histogram contains two visible peaks.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`HighParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the minimum pixel value. The minimum value must be less than or equal to the maximum value. |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`LowParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the maximum pixel value. The maximum value must be greater than or equal to the minimum value. |

---

### `M_DOMINANT`

Specifies to automatically establish the threshold value for the condition from the source image's histogram, using the dominant threshold mode.  In this mode, the threshold value is set to isolate the dominant peak of the source image's histogram. If the histogram contains two peaks, the threshold is set to the intensity value at the lowest point between these two peaks.  If the histogram contains more than two peaks, the threshold value is set at the lowest point between the dominant peak and its nearest peak on the right or left, depending on the pixel distribution in the histogram. If there are more pixels represented on the left side of the dominant peak, then the threshold will be set to the left of the peak. If there are more pixels represented on the right side of the dominant peak, then the threshold will be set to the right of the peak. Note that the dominant threshold mode works best when the histogram contains clearly identifiable peaks.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`HighParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the minimum pixel value. The minimum value must be less than or equal to the maximum value. |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`LowParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the maximum pixel value. The maximum value must be greater than or equal to the minimum value. |

---

### `M_FIXED`

Specifies that the threshold value(s) for the condition are specified explicitly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use any value that meets the selected condition. You must use this setting for [`M_NON_FINITE`](../../Reference/im/MimBinarize.md). |
| `Value` | Sets the threshold value to use as the lower limit of the selected condition. The lower limit must be less than or equal to the upper limit. |
| `M_NULL` | Specifies not to use an upper limit. Use this setting when the condition uses only one limit. |
| `Value` | Specifies the threshold value to use as the upper limit of the selected condition. The upper limit must be greater than or equal to the lower limit. |

---

### `M_PERCENTILE_VALUE`

Specifies that the threshold value(s) for the condition are specified as a percentage of the source image's histogram data. In this mode, you specify a threshold value by specifying the percentage of pixels that must be equal to or less than the required threshold value.

| Value | Description |
| --- | --- |
| `0 <= Value <= 100` | Specifies the lower limit as a percentage of the source image's histogram data. The lower limit must be less than or equal to the upper limit. |
| `M_NULL` | Specifies not to use an upper limit. Use this setting when the condition uses only one limit. |
| `0 <= Value <= 100` | Specifies the upper limit as a percentage of the source image's histogram data. The upper limit must be greater than or equal to the lower limit. |

---

### `M_TRIANGLE_BISECTION_BRIGHT`

Specifies to automatically establish the threshold value for the condition from the source image's histogram, using the bright triangle bisection threshold mode.  In bright triangular bisection threshold mode, Aurora Imaging Library establishes the tallest peak in the source image's histogram (the most commonly-occurring value) and the lowest point (the least commonly-occurring value) when heading from the peak toward its maximum intensity value. It then connects these two points by a line. The threshold value is the intensity value at the point furthest below that line (the inflection point). Note that all points above the line are ignored.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`HighParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the minimum pixel value. The minimum value must be less than or equal to the maximum value. |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`LowParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the maximum pixel value. The maximum value must be greater than or equal to the minimum value. |

---

### `M_TRIANGLE_BISECTION_DARK`

Specifies to automatically establish the threshold value for the condition from the source image's histogram, using the dark triangle bisection threshold mode.  In dark triangular bisection threshold mode, Aurora Imaging Library establishes the tallest peak in the source image's histogram (the most commonly-occurring value) and the lowest point (the least commonly-occurring value) when heading from the peak toward its minimum intensity value. It then connects these two points by a line. The threshold value is the intensify value at the point furthest below that line (the inflection point). Note that all points above the line are ignored.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`HighParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the minimum pixel value. The minimum value must be less than or equal to the maximum value. |
| `M_NULL` | Specifies to use the full range of pixel values in the histogram. This setting is only valid when [`LowParam`](../../Reference/im/MimBinarize.md) is also set to [`M_NULL`](../../Reference/im/MimBinarize.md). |
| `Value` | Specifies the maximum pixel value. The maximum value must be greater than or equal to the minimum value. |

### Combination Constants — For specifying a threshold condition

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify a threshold condition.

| Value | Description |
| --- | --- |
| `M_EQUAL` | Sets pixel values equal to the threshold value to the highest buffer value possible, while other pixels are set to zero. |
| `M_GREATER` | Sets pixel values greater than the threshold value to the highest buffer value possible, while other pixels are set to zero. |
| `M_GREATER_OR_EQUAL` | Sets pixel values greater than or equal to the threshold value to the highest buffer value possible, while other pixels are set to zero. |
| `M_IN_RANGE` | Sets pixels with values between the lower threshold value and the higher threshold value, inclusive, to the highest buffer value. Other pixels are set to 0.  > **Note:** Note that this is only applicable to constants that can use two limits ([`M_FIXED`](../../Reference/im/MimBinarize.md) and [`M_PERCENTILE_VALUE`](../../Reference/im/MimBinarize.md)). |
| `M_LESS` | Sets pixel values less than the threshold value to the highest buffer value possible, while other pixels are set to zero. |
| `M_LESS_OR_EQUAL` | Sets pixel values less than or equal to the threshold value to the highest buffer value possible, while other pixels are set to zero. |
| `M_NON_FINITE` | Sets floating-point pixel values that are non-finite to the highest buffer value possible, while other pixels are set to zero.  This condition can only be used with [`M_FIXED`](../../Reference/im/MimBinarize.md). [`HighParam`](../../Reference/im/MimBinarize.md) must be set to [`M_NULL`](../../Reference/im/MimBinarize.md). The source must be a 32-bit floating-point buffer. |
| `M_NOT_EQUAL` | Sets pixel values not equal to the threshold value to the highest buffer value possible, while other pixels are set to zero. |
| `M_OUT_RANGE` | Sets pixels with values less than the lower threshold value or greater than the higher threshold value, to the highest buffer value. Other pixels are set to 0.  > **Note:** Note that this is only applicable to constants that can use two limits ([`M_FIXED`](../../Reference/im/MimBinarize.md) and [`M_PERCENTILE_VALUE`](../../Reference/im/MimBinarize.md)). |

## Return Value

**Type:** `AIL_INT`

When [`DstImageBufId`](../../Reference/im/MimBinarize.md) is given an image buffer identifier, this function returns the specified value of the [`LowParam`](../../Reference/im/MimBinarize.md) parameter. When [`DstImageBufId`](../../Reference/im/MimBinarize.md) is set to `M_NULL` and the [`ConditionAndThreshMode`](../../Reference/im/MimBinarize.md) parameter is set to one of the automatic threshold modes ([`M_BIMODAL`](../../Reference/im/MimBinarize.md), [`M_TRIANGLE_BISECTION_DARK`](../../Reference/im/MimBinarize.md), or [`M_TRIANGLE_BISECTION_BRIGHT`](../../Reference/im/MimBinarize.md)), this function returns the automatically calculated threshold value. If the [`ConditionAndThreshMode`](../../Reference/im/MimBinarize.md) parameter is set to [`M_PERCENTILE_VALUE`](../../Reference/im/MimBinarize.md), this function returns the threshold value established from the specified lower limit. When dealing with floating-point source buffers, the returned threshold is truncated to an integer value. Therefore, the returned threshold and the actual threshold used to binarize can differ.

## Remarks

> Optimized in-place processing is supported (source equals destination), but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).

This threshold mode can only be used if the condition uses one limit.

Note that if using a floating-point destination buffer, pixels that meet the condition are set to 1.

You can limit the intensity value range of the histogram from which the threshold value is established. To do so, use the [`LowParam`](../../Reference/im/MimBinarize.md) and [`HighParam`](../../Reference/im/MimBinarize.md) parameters.
