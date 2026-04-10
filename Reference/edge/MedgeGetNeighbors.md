---
doctype: Reference
module: edge
function: MedgeGetNeighbors
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeGetNeighbors"
---

# MedgeGetNeighbors

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

> Get edgels from an Edge Finder result buffer that are the closest neighbors to a list of user-specified point coordinates.

## Syntax

```c
void MedgeGetNeighbors(
    AIL_ID             EdgeResultId,      //in
    AIL_INT            SizeOfArray,       //in
    const AIL_DOUBLE * SrcArrayXPtr,      //in
    const AIL_DOUBLE * SrcArrayYPtr,      //in
    const AIL_DOUBLE * SrcArrayAnglePtr,  //in
    AIL_DOUBLE *       DstArrayXPtr,      //out
    AIL_DOUBLE *       DstArrayYPtr,      //out
    AIL_INT *          DstArrayIndexPtr,  //out
    AIL_INT *          DstArrayLabelPtr,  //out
    AIL_INT64          ControlFlag        //in
)
```

## Description

This function retrieves the coordinates of edgels, from an Edge Finder result buffer, that correspond to the closest neighbors from a list of user-specified source point coordinates. You can also set a series of constraints that potential results must adhere to before being returned. To do so, use the appropriate settings in [`MedgeControl`](../../Reference/edge/MedgeControl.md).

If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) control type set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md). Note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) to [`M_WORLD`](../../Reference/edge/MedgeControl.md) and the results were not obtained from a calibrated image, [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md) will generate an error. Additionally, if results were obtained from a calibrated source image, and you specify to retrieve results in world units, user-specified coordinates and constraints must be specified in the same world units as the coordinate system used to calculate the results.

## Parameters

### `EdgeResultId` *(in, AIL_ID)*

Specifies the identifier of the Edge Finder result buffer from which to search for edgel candidates.

### `SizeOfArray` *(in, AIL_INT)*

Specifies the number of points provided. This value represents how many user-specified point coordinates will be searched for in the Edge Finder result buffer.

### `SrcArrayXPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinate(s) of the source point(s).

### `SrcArrayYPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinate(s) of the source point(s).

### `SrcArrayAnglePtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the constraint angle(s) used to refine the search for edgel candidates. Note that angle values (0° to 360°) are measured counter-clockwise and must be mapped between either 0 to 255 (for contour contexts) or 0 to 127 (for crest contexts).

### `DstArrayXPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which the X-coordinate(s) of the edgel(s) found in the Edge Finder result buffer are to be written. If no edgels were found, ignore the information written. Note that if no edgels were found, the index ([`DstArrayIndexPtr`](../../Reference/edge/MedgeGetNeighbors.md)) and label ([`DstArrayLabelPtr`](../../Reference/edge/MedgeGetNeighbors.md)) of the edgels is `M_NULL`.

### `DstArrayYPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which the Y-coordinate(s) of the edgel(s) found in the Edge Finder result buffer is to be written. If no edgels were found, ignore the information written. Note that if no edgels were found, the index ([`DstArrayIndexPtr`](../../Reference/edge/MedgeGetNeighbors.md)) and label ([`DstArrayLabelPtr`](../../Reference/edge/MedgeGetNeighbors.md)) of the edgels is `M_NULL`.

### `DstArrayIndexPtr` *(out, *AIL_INT)*

Specifies the address of the array in which the index of the edgel(s) found in the Edge Finder result buffer is to be written. If no edgels were found, `M_NULL` is written.

### `DstArrayLabelPtr` *(out, *AIL_INT)*

Specifies the address of the array in which the label of the edgel's edge found in the Edge Finder result buffer is to be written. If no edgels were found, `M_NULL` is written.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the accuracy with which to return edgels.

*For specifying the accuracy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GET_EDGELS` *(default)* | Specifies that edgels are returned with normal accuracy. In this case, only edgels explicitly located in the Edge Finder result buffer can be returned. |
| `M_GET_SUBEDGELS` | Specifies that edgels are returned with high accuracy. In this case, a point between two edgels can be returned.

When using [`M_GET_SUBEDGELS`](../../Reference/edge/MedgeGetNeighbors.md), the [`M_NEIGHBOR_MAXIMUM_NUMBER`](../../Reference/edge/MedgeControl.md) constraint in [`MedgeControl`](../../Reference/edge/MedgeControl.md) must be set to 1. |
