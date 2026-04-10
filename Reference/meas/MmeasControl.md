---
doctype: Reference
module: meas
function: MmeasControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasControl"
---

# MmeasControl

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

> Control a measurement context or result buffer setting.

## Syntax

```c
void MmeasControl(
    AIL_ID     ContextOrResultId,  //out
    AIL_INT64  ControlType,        //in
    AIL_DOUBLE ControlValue        //in
)
```

## Description

This function allows you to control a measurement context or result buffer setting. These settings control the behavior of measurement operations ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) and [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)), which subsequently affects the results that you obtain ([`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md)) or draw ([`MmeasDraw`](../../Reference/meas/MmeasDraw.md)).

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies the identifier of the measurement context or result buffer.

*For specifying the measurement context or result buffer identifier*

| Value | Description |
| --- | --- |
| `Measurement context ID` | Specifies a valid measurement context, previously allocated using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). |
| `Measurement result buffer ID` | Specifies a valid measurement result buffer, previously allocated using [`MmeasAllocResult`](../../Reference/meas/MmeasAllocResult.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For a measurement context

The following [`ControlType`](../../Reference/meas/MmeasControl.md) and corresponding [`ControlValue`](../../Reference/meas/MmeasControl.md) parameter settings can be specified for a measurement context. In this case, you must set the [`ContextOrResultId`](../../Reference/meas/MmeasControl.md) parameter to a measurement context.

---

### `M_PIXEL_ASPECT_RATIO`

Sets the pixel width to pixel height ratio.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the ratio. |

---

### `M_PIXEL_ASPECT_RATIO_INPUT`

Sets whether to use [`M_PIXEL_ASPECT_RATIO`](../../Reference/meas/MmeasControl.md) to correct measurement inputs.

| Value | Description |
| --- | --- |
| `M_CORRECTED` *(default)* | Specifies to correct measurement inputs with the specified aspect ratio. |
| `M_NORMAL` | Specifies to not correct measurement inputs (ratio of 1.0). |

---

### `M_PIXEL_ASPECT_RATIO_OUTPUT`

Sets whether to use [`M_PIXEL_ASPECT_RATIO`](../../Reference/meas/MmeasControl.md) to correct measurement results (output data).

| Value | Description |
| --- | --- |
| `M_CORRECTED` *(default)* | Specifies to correct measurement results with the specified aspect ratio. |
| `M_NORMAL` | Specifies to not correct measurement results (ratio of 1.0). |

### For a measurement result buffer

The following [`ControlType`](../../Reference/meas/MmeasControl.md) and corresponding [`ControlValue`](../../Reference/meas/MmeasControl.md) parameter settings can be specified for a measurement result buffer. In this case, you must set the [`ContextOrResultId`](../../Reference/meas/MmeasControl.md) parameter to a measurement result buffer.

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results of [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.  Use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) to control the units in which results found using [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) are returned.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) generates an error if the result was not calculated on a calibrated image. |
