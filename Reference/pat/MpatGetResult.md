---
doctype: Reference
module: pat
function: MpatGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / pat / MpatGetResult"
---

# MpatGetResult

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

> Get the specified type of result(s) from a Pattern Matching result buffer.

## Syntax

```c
void MpatGetResult(
    AIL_ID    ResultPatId,    //in
    AIL_INT   Index,          //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type from a specified Pattern Matching result buffer. Results are only available after calling [`MpatFind`](../../Reference/pat/MpatFind.md).

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MpatControl`](../../Reference/pat/MpatControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/pat/MpatControl.md) control type set to [`M_PIXEL`](../../Reference/pat/MpatControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/pat/MpatControl.md) to [`M_WORLD`](../../Reference/pat/MpatControl.md) without having specified a calibrated target image in which to calculate the results, [`MpatGetResult`](../../Reference/pat/MpatGetResult.md) will generate an error.

## Parameters

### `ResultPatId` *(in, AIL_ID)*

Specifies the identifier of the Pattern Matching result buffer from which to retrieve results.

### `Index` *(in, AIL_INT)*

Specifies where to get results. This parameter can be set to one of the following:

*For specifying where to get results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ALL`](../../Reference/pat/MpatGetResult.md) for result types that apply to model occurrences. Same as [`M_GENERAL`](../../Reference/pat/MpatGetResult.md) for result types that apply to the entire result buffer. |
| `M_ALL` | Retrieves the results of the specified type for all model occurrences found. |
| `M_GENERAL` | Retrieves a result relating to the entire result buffer. |
| `0 <= Value < M_NUMBER` | Specifies the occurrence index. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve for each model occurrence.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the one-dimensional array in which to write the specified results. The array must be big enough to hold the number of results indicated by [`M_NUMBER`](../../Reference/pat/MpatGetResult.md).

## Parameter Associations

### For general results

When retrieving a general result, the [`ResultType`](../../Reference/pat/MpatGetResult.md) parameter can be set to one of the following values.

---

### `M_CONTEXT_ID`

Retrieves the identifier of the Pattern Matching context used by [`MpatFind`](../../Reference/pat/MpatFind.md) to obtain the results in the result buffer. Note that, if the Pattern Matching context has already been freed, using [`MpatFree`](../../Reference/pat/MpatFree.md), the returned Pattern Matching context identifier might no longer be valid.

---

### `M_NUMBER`

Retrieves the number of occurrences found.

### For specifying the results to retrieve

To specify the type of result to retrieve for each model occurrence, the [`ResultType`](../../Reference/pat/MpatGetResult.md) parameter must be set to one of the following.

---

### `M_ANGLE`

Retrieves the angle of the occurrence (if found using [`MpatFind`](../../Reference/pat/MpatFind.md)), relative to the output coordinate system specified using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/pat/MpatControl.md).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

---

### `M_INDEX`

Retrieves the index of the model that was found.

---

### `M_NUMBER_OF_PIXELS`

Retrieves the number of pixels in the model used to compute the correlation function.

---

### `M_POSITION_X`

Retrieves the X-coordinate of the occurrence. This is the X-coordinate of the model's reference position transformed at the occurrence ([`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE_X`](../../Reference/pat/MpatControl.md)).

---

### `M_POSITION_Y`

Retrieves the Y-coordinate of the occurrence. This is the Y-coordinate of the model's reference position transformed at the occurrence ([`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE_Y`](../../Reference/pat/MpatControl.md).

---

### `M_SCORE`

Retrieves the match score of the occurrence, as a percentage.

---

### `M_SUM_I`

Retrieves the sum of the values of the pixels in the image over the `N` pixels corresponding to the model occurrence.  *[Image: MpatGetResult-M_SUM_I.png]*

---

### `M_SUM_II`

Retrieves the sum of the squares of the values of the pixels in the image over the `N` pixels corresponding to the model occurrence.  *[Image: MpatGetResult-M_SUM_II.png]*

---

### `M_SUM_IM`

Retrieves the sum of the products of the values of the pixels in the image with the pixels in the model over the `N` pixels corresponding to the model occurrence.  *[Image: MpatGetResult-M_SUM_IM.png]*

---

### `M_SUM_M`

Retrieves the sum of the values of the pixels in the model over the `N` pixels of the model.  *[Image: MpatGetResult-M_SUM_M.png]*

---

### `M_SUM_MM`

Retrieves the sum of the squares of the values of the pixels in the model over the `N` pixels of the model.  *[Image: MpatGetResult-M_SUM_MM.png]*

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

Note that to retrieve this result type, the sums must be saved using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SAVE_SUMS`](../../Reference/pat/MpatControl.md) set to [`M_ENABLE`](../../Reference/pat/MpatControl.md).
