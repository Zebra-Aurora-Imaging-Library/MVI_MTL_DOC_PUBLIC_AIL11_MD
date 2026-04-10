---
doctype: Reference
module: im
function: MimGetResult2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimGetResult2d"
---

# MimGetResult2d

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

> Get data from a statistics result buffer, and place it in a user-supplied array.

## Syntax

```c
void MimGetResult2d(
    AIL_ID    ResultImId,   //in
    AIL_INT   OffsetX,      //in
    AIL_INT   OffsetY,      //in
    AIL_INT   SizeX,        //in
    AIL_INT   SizeY,        //in
    AIL_INT64 ResultType,   //in
    AIL_INT64 ControlFlag,  //in
    void *    UserArrayPtr  //out
)
```

## Description

This function retrieves, from an [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md) result buffer, the results obtained for a specified region of the series of source buffer passed to [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) that uses an [`M_STATISTICS_CUMULATIVE_CONTEXT`](../../Reference/im/MimAlloc.md). One value is returned for every pixel in the specified region.

## Parameters

### `ResultImId` *(in, AIL_ID)*

Specifies the identifier of the statistics image processing result buffer from which to get results.

### `OffsetX` *(in, AIL_INT)*

Specifies the X-offset of the region for which to retrieve results. The specified value must be between 0 and the [`SizeX`](../../Reference/im/MimGetResult2d.md) parameter value.

### `OffsetY` *(in, AIL_INT)*

Specifies the Y-offset of the region for which to retrieve results. The specified value must be between 0 and the [`SizeY`](../../Reference/im/MimGetResult2d.md) parameter value.

### `SizeX` *(in, AIL_INT)*

Specifies the width (X-size) of the region for which to retrieve results.

### `SizeY` *(in, AIL_INT)*

Specifies the height (Y-size) of the region for which to retrieve results.

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result(s) to retrieve.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the user-specified array in which to write the results read from the multiple statistics result buffer.

## Parameter Associations

### For specifying the type of result to retrieve

---

### `M_STAT_MAX`

Retrieves the maximum value of each pixel, given the different source images.

---

### `M_STAT_MAX_ABS`

Retrieves the maximum absolute value of each pixel, given the different source images.

---

### `M_STAT_MEAN`

Retrieves the mean value of each pixel, given the different source images.

---

### `M_STAT_MIN`

Retrieves the minimum value of each pixel, given the different source images.

---

### `M_STAT_MIN_ABS`

Retrieves the minimum absolute value of each pixel, given the different source images.

---

### `M_STAT_STANDARD_DEVIATION`

Retrieves the standard deviation value of each pixel, given the different source images.

---

### `M_STAT_SUM`

Retrieves the sum of each pixel's different values, given the different source images.

---

### `M_STAT_SUM_ABS`

Retrieves the sum of the absolute value of each pixel's different value, given the different source images.

---

### `M_STAT_SUM_OF_SQUARES`

Retrieves the sum of the square of each pixel's different value, given the different source images.

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
