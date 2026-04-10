---
doctype: Reference
module: im
function: MimGetResult1d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimGetResult1d"
---

# MimGetResult1d

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

> Get the specified number of result entries from an image processing result buffer.

## Syntax

```c
void MimGetResult1d(
    AIL_ID    ResultImId,   //in
    AIL_INT   OffEntry,     //in
    AIL_INT   NbEntries,    //in
    AIL_INT64 ResultType,   //in
    void *    UserArrayPtr  //out
)
```

## Description

This function retrieves the specified number of result entries from the specified result buffer and stores them in the specified one-dimensional destination user array.

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context, but you want to retrieve positional and dimensional results in pixel units, use [`MimControl`](../../Reference/im/MimControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/im/MimControl.md) control type set to [`M_PIXEL`](../../Reference/im/MimControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/im/MimControl.md) to [`M_WORLD`](../../Reference/im/MimControl.md) without specifying a calibrated image in which to calculate the results, [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) will generate an error.

## Parameters

### `ResultImId` *(in, AIL_ID)*

Specifies the identifier of the image processing result buffer from which to get results.

### `OffEntry` *(in, AIL_INT)*

Specifies the offset of the first result to read. The given value must be within the number of allocated result entries.

### `NbEntries` *(in, AIL_INT)*

Specifies the number of result entries to get, starting from the entry specified by the [`OffEntry`](../../Reference/im/MimGetResult1d.md) parameter.

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result(s) to retrieve.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the one-dimensional array in which to write the results read from the Aurora Imaging Library result buffer.

## Parameter Associations

### For specifying the type of result to retrieve

---

### `M_ANGLE`

Retrieves the best orientations (angle values) calculated by [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) and stored in an [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md) result buffer. Aurora Imaging Library sorts the resulting angles according to their associated score ([`M_SCORE`](../../Reference/im/MimGetResult1d.md)); angles with higher scores are listed first.

---

### `M_CUMULATIVE_VALUE`

Retrieves the cumulative histogram values from an[`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_NB_EVENT`

Retrieves the number of events from an[`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) result buffer. Note that if the result buffer did not have the required number of entries to hold the total number of events that satisfied the condition, the number returned is limited to the number of entries in the result buffer. In this case, results from a multi-core processing application might differ from run to run as results are added globally from the multiple threads.

---

### `M_NUMBER`

Retrieves the number of differences from an [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer. Note that if the result buffer did not have the required number of entries to hold the total number of differences that satisfied the condition, the number returned is limited to the number of entries in the result buffer. In this case, results from a multi-core processing application might differ from run to run as results are added globally from the multiple threads.

---

### `M_PERCENTILE_VALUE`

Retrieves the inverse cumulative histogram from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md) result buffer. Each value retrieved is the pixel value whereby _n_% of the pixels have a value less than or equal to that value, where _n_ is the index of the user array.

---

### `M_POSITION_X`

Retrieves the image X-coordinate of each event from an[`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) or [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_POSITION_Y`

Retrieves the image Y-coordinate of each event from an[`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) or [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_SCORE`

Retrieves the score attributed to each orientation (angle) value calculated by [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) and stored in an [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md) result buffer. The scores are represented as percentages relative to the other orientations stored in the result buffer. Aurora Imaging Library lists higher scores first.  The first orientation in the buffer will always be the most dominant orientation with a score of 100.

---

### `M_SRC1_VALUE`

Retrieves source 1 values for each differences from [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_SRC2_VALUE`

Retrieves source 2 values for each differences from [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) result buffer.

---

### `M_VALID`

Retrieves the validity of projection data, from an [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md)result buffer. This is only useful when a region of interest is associated with the source image buffer of the projection operation.  A lane of pixels will contain no valid pixels if it is excluded from the region of interest associated with the source image; this means that projection data cannot be calculated for the lane. However, the [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md)result buffer contains an entry for each lane of pixels found in the source image, regardless of the use of a region of interest. So when your source image buffer has a region of interest, you should use [`M_VALID`](../../Reference/im/MimGetResult1d.md)to check the validity of the data for the lane in the result buffer.  Note that for an [`M_SUM`](../../Reference/im/MimProjection.md)projection operation, if a lane of pixels does not contain valid projection data, a neutral pixel value of 0 is used for that lane. In this case, [`M_VALID`](../../Reference/im/MimGetResult1d.md)will indicate valid projection data for all lanes of pixels in the image.

| Value | Description |
| --- | --- |
| `0` | Specifies invalid projection data for the lane due to no valid pixels residing in the lane of pixels in the source image corresponding to the [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md) entry. |
| `Value != 0` | Specifies valid projection data for the lane. |

---

### `M_VALUE`

Retrieves values from an [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md), [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md), [`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md), [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md), or [`M_COUNT_LIST`](../../Reference/im/MimAllocResult.md) result buffer.  If an [`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md) type result buffer contains both the minimum and maximum value of an image, the minimum is written first, followed by the maximum.

### Combination Constants — For retrieving results as a percentage

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the requested results as a percentage.

#### `M_PERCENTAGE`

Retrieves the histogram values or cumulative histogram values, as a percentage. The values are multiplied by 100 and then divided by the total number of elements in the histogram.

### Combination Constants — For the ResultType parameter to cast the required results to the requested data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

## Remarks

> Note that some of the values listed above are not available in Aurora Imaging Library Lite. See the result's corresponding operation function for Aurora Imaging Library Lite availability.
