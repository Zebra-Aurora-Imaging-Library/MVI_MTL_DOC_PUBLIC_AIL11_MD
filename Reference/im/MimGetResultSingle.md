---
doctype: Reference
module: im
function: MimGetResultSingle
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimGetResultSingle"
---

# MimGetResultSingle

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

> Get a result from a wavelet transformation or 1D locate peak result buffer.

## Syntax

```c
void MimGetResultSingle(
    AIL_ID    ResultImId,   //in
    AIL_INT64 Index1,       //in
    AIL_INT64 Index2,       //in
    AIL_INT64 ResultType,   //in
    void *    UserArrayPtr  //out
)
```

## Description

This function retrieves a result of the specified type from a wavelet transformation or 1D locate peak result buffer. Aurora Imaging Library performs wavelet transformations with [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and 1D peak intensity detection with [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md).

## Parameters

### `ResultImId` *(in, AIL_ID)*

Specifies the identifier of the wavelet transformation or 1D locate peak result buffer from which to get results.

### `Index1` *(in, AIL_INT64)*

Specifies the first index with which to indicate the result to retrieve.

*For specifying the first index, for which to retrieve the wavelet transformation result*

| Value | Description |
| --- | --- |
| `M_DIAGONAL_LEVEL` | Retrieves results about the diagonal wavelet coefficient, at the specified transformation level. |
| `M_HORIZONTAL_LEVEL` | Retrieves results about the horizontal wavelet coefficient, at the specified transformation level. |
| `M_VERTICAL_LEVEL` | Retrieves results about the vertical wavelet coefficient, at the specified transformation level. |
| `M_APPROXIMATION` | Retrieves results about the approximation (the low frequency rendition) of the wavelet transformation at the last calculated level. |

*For specifying the index of the peak in the lane(s), for which to retrieve the 1D locate peak result*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no information is required for the [`Index1`](../../Reference/im/MimGetResultSingle.md) parameter. This setting is only valid if you are retrieving the number of scan lanes ([`M_NUMBER_OF_SCAN_LANES`](../../Reference/im/MimGetResultSingle.md)) or when retrieving the initialization state of the result buffer with [`M_STATUS`](../../Reference/im/MimGetResultSingle.md). |
| `M_SELECT_PEAK` | Specifies the frame(s) and rank of the peak(s) for which to return the requested result (for the lane(s) specified with the [`Index2`](../../Reference/im/MimGetResultSingle.md) parameter).

This macro is required if a [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) operation accumulated results from multiple frames in the result buffer ([`MimControl`](../../Reference/im/MimControl.md) with[`M_NUMBER_OF_FRAMES`](../../Reference/im/MimControl.md) set to a value greater than 1). |
| `M_ALL` | Retrieves the requested result for all peaks in the scan lane(s) specified using the [`Index2`](../../Reference/im/MimGetResultSingle.md) parameter. If peaks are absent from a lane, Aurora Imaging Library skips it unless [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) is specified. Ordering of the results is dependent on [`MimControl`](../../Reference/im/MimControl.md) with [`M_SORT_CRITERION`](../../Reference/im/MimControl.md).

Note that this setting is equivalent to using the **M_SELECT_PEAK()** macro above, with the Frame and Rank parameters set to 0 and **M_SELECT_PEAK()**, respectively. |
| `0 <= Value < M_NUMBER + M_INCLUDE_MISSING_DATA` | Specifies the rank of the peak for which to retrieve the requested result, for the scan lane(s) specified using the [`Index2`](../../Reference/im/MimGetResultSingle.md) parameter. Ordering of the results is dependent on [`MimControl`](../../Reference/im/MimControl.md) with [`M_SORT_CRITERION`](../../Reference/im/MimControl.md).

If the peak is absent from the lane, Aurora Imaging Library skips it, unless [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) is specified. |

### `Index2` *(in, AIL_INT64)*

Specifies the second index for which to retrieve results. For a wavelet transformation result, this information is not required; set this parameter to [`M_NULL`](../../Reference/im/MimGetResultSingle.md).

*For specifying the index of the lane for which to retrieve the 1D locate peak result*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no information is required for the [`Index2`](../../Reference/im/MimGetResultSingle.md) parameter. For a 1D locate peak result, this setting is only valid if you are retrieving the number of scan lanes ([`M_NUMBER_OF_SCAN_LANES`](../../Reference/im/MimGetResultSingle.md)) or when retrieving the initialization state of the result buffer with [`M_STATUS`](../../Reference/im/MimGetResultSingle.md). |
| `M_ALL` | Specifies to retrieve the requested result for all scan lanes. If the specified peak(s) are absent from a lane, Aurora Imaging Library skips it unless [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) is specified. Ordering of the results is dependent on [`MimControl`](../../Reference/im/MimControl.md) with [`M_SORT_CRITERION`](../../Reference/im/MimControl.md). |
| `0 <= Value < M_NUMBER_OF_SCAN_LANES` | Specifies the lane for which to retrieve the requested result. If the specified peak(s) are absent from a lane, Aurora Imaging Library skips it unless [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) is specified. Empty elements are added after the other elements, at the end of the returned array. Ordering of the results is dependent on [`MimControl`](../../Reference/im/MimControl.md) with [`M_SORT_CRITERION`](../../Reference/im/MimControl.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the one-dimensional array in which to write the results read from the Aurora Imaging Library result buffer.

## Parameter Associations

### For specifying the type of result to retrieve, when using a wavelet transformation result

To retrieve a result from the specified wavelet coefficient and detail level ([`Index1`](../../Reference/im/MimGetResultSingle.md)), set [`ResultType`](../../Reference/im/MimGetResultSingle.md) to one of the following:

---

### `M_WAVELET_COEFFICIENTS_IMAGE_ID`

Retrieves the identifier of the internal image buffer that holds the wavelet data specified by the [`Index1`](../../Reference/im/MimGetResultSingle.md) parameter. For example, if you use [`M_WAVELET_COEFFICIENTS_IMAGE_ID`](../../Reference/im/MimGetResultSingle.md) with the [`Index1`](../../Reference/im/MimGetResultSingle.md) parameter set to **M_DIAGONAL_LEVEL(3)**, you will retrieve the identifier of the internal image buffer that holds the diagonal wavelet coefficient at level three.  The type and depth (per band) of the data in each internal wavelet buffer is 32-bit floating-point.  There is a separate internal image buffer for every wavelet coefficient result, at every level, and for each numerical domain. You can specify the domain by combining [`M_WAVELET_COEFFICIENTS_IMAGE_ID`](../../Reference/im/MimGetResultSingle.md) with [`M_REAL_PART`](../../Reference/im/MimGetResultSingle.md) (the default) or [`M_IMAGINARY_PART`](../../Reference/im/MimGetResultSingle.md). To display an internal image buffer, you must inquire its system using [`MbufInquire`](../../Reference/buf/MbufInquire.md)with [`M_OWNER_SYSTEM`](../../Reference/buf/MbufInquire.md), and allocate a display on it using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md). You can then use this allocated display to visualize the content of [`M_WAVELET_COEFFICIENTS_IMAGE_ID`](../../Reference/im/MimGetResultSingle.md).

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the identifier of the internal image buffer. |

---

### `M_WAVELET_LEVEL_PADDING_OFFSET_X`

Retrieves the X-offset of the padding used to calculate the result.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the X-offset, in pixels. |

---

### `M_WAVELET_LEVEL_PADDING_OFFSET_Y`

Retrieves the Y-offset of the padding used to calculate the result.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the Y-offset, in pixels. |

---

### `M_WAVELET_LEVEL_SIZE_X`

Retrieves the width of the undistorted (non-padded) region that was used to calculate the result.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the width, in pixels. |

---

### `M_WAVELET_LEVEL_SIZE_Y`

Retrieves the height of the undistorted (non-padded) region that was used to calculate the result.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the height, in pixels. |

### Combination Constants — For specifying whether to retrieve information about the real or imaginary numbers in the wavelet result

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether to retrieve information about the real or imaginary numbers in the wavelet result.

| Value | Description |
| --- | --- |
| `M_IMAGINARY_PART` | Retrieves information about the imaginary part of the values in the wavelet result. Only available for complex wavelet transformations ([`MimGetResult`](../../Reference/im/MimGetResult.md) with [`M_TRANSFORMATION_DOMAIN`](../../Reference/im/MimGetResult.md)must return [`M_COMPLEX`](../../Reference/im/MimGetResult.md)). |
| `M_REAL_PART` *(default)* | Retrieves information about the real numbers in the result. Available for any type of wavelet transformation. |

### For specifying the type of result to retrieve, when using a 1D locate peak result

To retrieve a result from a 1D locate peak result buffer, [`ResultType`](../../Reference/im/MimGetResultSingle.md) can be set to one of the following:

---

### `M_NUMBER`

Retrieves the number of elements that would be returned, given the values specified for the [`Index1`](../../Reference/im/MimGetResultSingle.md) and [`Index2`](../../Reference/im/MimGetResultSingle.md) parameters.

---

### `M_NUMBER_OF_SCAN_LANES`

Retrieves the number of scan lanes.  When using [`M_NUMBER_OF_SCAN_LANES`](../../Reference/im/MimGetResultSingle.md), you must set the [`Index1`](../../Reference/im/MimGetResultSingle.md) and [`Index2`](../../Reference/im/MimGetResultSingle.md) parameters to [`M_NULL`](../../Reference/im/MimGetResultSingle.md).

---

### `M_PEAK_INTENSITY`

Retrieves the peak's intensity.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, empty elements are set to the value specified by [`M_PEAK_INTENSITY_INVALID_VALUE`](../../Reference/im/MimControl.md), and placed at the end of the returned array. The default intensity value for missing peaks is -1 (or an unsigned buffer's maximum value).

---

### `M_PEAK_POSITION`

Retrieves the peak's position along the scan lane, in pixels.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, empty elements are set to [`M_INVALID`](../../Reference/im/MimGetResultSingle.md) and placed at the end of the returned array.

---

### `M_PEAK_POSITION_X`

Retrieves the X-coordinate of the peak's position along the image's X-axis, in pixels.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, and [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_HORIZONTAL`](../../Reference/im/MimControl.md), empty elements are set to [`M_INVALID`](../../Reference/im/MimGetResultSingle.md) and placed at the end of the returned array. If, however, [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_VERTICAL`](../../Reference/im/MimControl.md), the lane indices of the empty elements are placed at the end of the returned array.

---

### `M_PEAK_POSITION_Y`

Retrieves the Y-coordinate of the peak's position along the image's Y-axis, in pixels.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, and [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_VERTICAL`](../../Reference/im/MimControl.md), empty elements are set to [`M_INVALID`](../../Reference/im/MimGetResultSingle.md) and placed at the end of the returned array. If, however, [`M_SCAN_LANE_DIRECTION`](../../Reference/im/MimControl.md) is set to [`M_HORIZONTAL`](../../Reference/im/MimControl.md), the lane indices of the empty elements are placed at the end of the returned array.

---

### `M_PEAK_WIDTH`

Retrieves the peak's detected width.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, empty elements are set to [`M_INVALID`](../../Reference/im/MimGetResultSingle.md) and placed at the end of the returned array.

---

### `M_RANK_INDEX`

Retrieves the peak's index along the scan lane.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, the rank indices of the empty elements are placed at the end of the returned array.

---

### `M_SCAN_LANE_INDEX`

Retrieves the lane's index of the data.  Note that if the [`M_INCLUDE_MISSING_DATA`](../../Reference/im/MimGetResultSingle.md) combination value is specified, the lane indices of the empty elements are placed at the end of the returned array.

---

### `M_STATUS`

Retrieves the initialization state of a 1D locate peak result buffer.  When using [`M_STATUS`](../../Reference/im/MimGetResultSingle.md), you must set the [`Index1`](../../Reference/im/MimGetResultSingle.md) and [`Index2`](../../Reference/im/MimGetResultSingle.md) parameters to zero; otherwise, Aurora Imaging Library will generate an error.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the result buffer has been preprocessed and contains data from a previous operation.  Once data is accumulated using [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md), [`M_STATUS`](../../Reference/im/MimGetResultSingle.md) will return [`M_COMPLETE`](../../Reference/im/MimGetResultSingle.md). |
| `M_UNINITIALIZED` | Specifies that the result buffer has not been preprocessed. |
| `M_VALID` | Specifies that the result buffer has been properly preprocessed, but has not been used in any operation.  After preprocessing, but before accumulating any data, [`M_STATUS`](../../Reference/im/MimGetResultSingle.md) will return [`M_VALID`](../../Reference/im/MimGetResultSingle.md). To preprocess a result buffer without accumulating data, use [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) with [`M_PREPROCESS`](../../Reference/im/MimLocatePeak1d.md). |

### Combination Constants — For specifying how to handle missing data, when using a 1D locate peak result

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify how to handle missing data, when using a 1D locate peak result.

If you do not specify any of these combination constants, Aurora Imaging Library does not include missing data when returning information about the specified result type. That is, empty scan lanes and ranks are skipped.

#### `M_INCLUDE_MISSING_DATA`

Specifies that missing data is included as part of the information returned by the result type.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies an empty lane or rank. |

#### `M_ONLY_MISSING_DATA`

Specifies that only empty elements are returned by the result type. Alignment with other peaks is not checked. Empty lanes for rank _i_ are always those where at most `_i_-1` peaks were detected even when data inspection suggests that the last detected peak should be part of rank _i_.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies an empty lane or rank. |

### Combination Constants — For using fixed point with fractional bits, when using a 1D locate peak result

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify returned data using fixed point with fractional bits, when using a 1D locate peak result.

| Value | Description |
| --- | --- |
| `M_FIXED_POINT + n` | Expresses results using fixed point with _n_ fractional bits. Results must be an _integer_. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For the ResultType parameter to cast the required results to the requested data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

Specifies the transformation level.
