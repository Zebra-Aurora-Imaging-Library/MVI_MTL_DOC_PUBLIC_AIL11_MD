---
doctype: Reference
module: gen
function: MgenLutRamp
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gen / MgenLutRamp"
---

# MgenLutRamp

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

> Generate ramp data into a LUT buffer.

## Syntax

```c
void MgenLutRamp(
    AIL_ID     LutBufId,    //out
    AIL_INT    StartIndex,  //in
    AIL_DOUBLE StartValue,  //in
    AIL_INT    EndIndex,    //in
    AIL_DOUBLE EndValue     //in
)
```

## Description

This function generates a ramp, inverse ramp, or a constant in the specified LUT buffer region ([`StartIndex`](../../Reference/gen/MgenLutRamp.md) to [`EndIndex`](../../Reference/gen/MgenLutRamp.md)). The increment between LUT entries is the difference between [`StartValue`](../../Reference/gen/MgenLutRamp.md) and [`EndValue`](../../Reference/gen/MgenLutRamp.md), divided by (the number of entries - 1).

If you need to generate a more complex LUT, use [`MgenLutFunction`](../../Reference/gen/MgenLutFunction.md) or generate the values with your Host system and load them into an Aurora Imaging Library LUT buffer, using [`MbufPut1d`](../../Reference/buf/MbufPut1d.md) or [`MbufPutColor`](../../Reference/buf/MbufPutColor.md).

## Parameters

### `LutBufId` *(out, AIL_ID)*

Specifies the identifier of the LUT in which to generate values. This parameter must be given a valid LUT buffer identifier. Allocate a LUT buffer, using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md).

### `StartIndex` *(in, AIL_INT)*

Specifies the first LUT index for which to generate a value.

### `StartValue` *(in, AIL_DOUBLE)*

Specifies the starting value from which the increment is calculated; that is, the first LUT entry.

### `EndIndex` *(in, AIL_INT)*

Specifies the last LUT index for which to generate a value.

### `EndValue` *(in, AIL_DOUBLE)*

Specifies the ending value from which the increment is calculated; that is, the last LUT entry. If both [`StartValue`](../../Reference/gen/MgenLutRamp.md) and [`EndValue`](../../Reference/gen/MgenLutRamp.md) are the same, the entire LUT range is filled with this value. If [`EndValue`](../../Reference/gen/MgenLutRamp.md) is smaller than [`StartValue`](../../Reference/gen/MgenLutRamp.md), an inverse ramp is generated. This parameter accepts only an integer value, except when using a floating-point LUT buffer.
