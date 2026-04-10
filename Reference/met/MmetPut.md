---
doctype: Reference
module: met
function: MmetPut
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetPut"
---

# MmetPut

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

> Put edgels or points from user-supplied arrays into a constructed edgel feature built with external edgels/points.

## Syntax

```c
void MmetPut(
    AIL_ID             ContextMetId,        //out
    AIL_INT            LabelOrIndex,        //in
    AIL_INT            NumEdgelsOrPoints,   //in
    const AIL_INT *    ChainIndexArrayPtr,  //in
    const AIL_DOUBLE * PosXArrayPtr,        //in
    const AIL_DOUBLE * PosYArrayPtr,        //in
    const AIL_DOUBLE * AngleArrayPtr,       //in
    const AIL_DOUBLE * ValueArrayPtr,       //in
    AIL_INT64          ControlFlag          //in
)
```

## Description

This function adds edgels or points from user-supplied arrays to a constructed edgel feature built with external edgels/points. You can add a constructed edgel feature built with external edgels/points to the metrology context using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_EDGEL`](../../Reference/met/MmetAddFeature.md) and[`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md). Every time [`MmetPut`](../../Reference/met/MmetPut.md)is called, it adds the specified edgels/points to the constructed feature, without overwriting previously-added edgels/points with the same position. Points and edgels can be added to the same feature, but you must use separate calls.

[`MmetPut`](../../Reference/met/MmetPut.md) is useful if you want to construct an edgel feature with edgels established using the Edge Finder module or with the coordinates of chained pixels (points) established using the Blob Analysis module. You can also, for example, use[`MmetPut`](../../Reference/met/MmetPut.md) to add points from a cross-section of a 3D scanned object, delimited by an extraction box, so that you can analyze the profile of the cross-section. To inquire information about an external edgel feature ([`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md)), such as the total number of external points/edgels it contains ([`M_NUMBER`](../../Reference/met/MmetInquire.md)), call [`MmetInquire`](../../Reference/met/MmetInquire.md).

Information about a particular edgel/point must occupy the same index in each array. You specify points by specifying their position (X- and Y-position arrays). You specify edgels by specifying their position and their gradient angle (edgel direction). An edgel's angle must respect the convention:

- The angle is measured between the target image's horizontal axis and the edge's gradient (the direction of the edgel's most sudden change from black to white).
- The angle is measured counter-clockwise.
- You must map the angle to an 8-bit range; that is, 0° corresponds to 0, and 360° corresponds to 256.

A point/edgel's position must be relative to the reference frame of the constructed edgel feature ([`MmetControl`](../../Reference/met/MmetControl.md)with [`M_REFERENCE_FRAME`](../../Reference/met/MmetControl.md)), which can be set to either the global frame or a local frame.

When passing points that are along an edge, but you don't have their gradient angle information (as is the case when obtained using the Blob Analysis module), you can have[`MmetPut`](../../Reference/met/MmetPut.md) automatically calculate their approximate gradient angle by passing [`M_INTERPOLATE_ANGLE`](../../Reference/met/MmetPut.md) to the [`ControlFlag`](../../Reference/met/MmetPut.md) parameter. [`MmetPut`](../../Reference/met/MmetPut.md)interpolates their gradient angles assuming that points in the arrays are consecutive. As such, the points in the X- and Y-position arrays must be an ordered, connected chain of points. Note that if the points are not from an edge, you should not pass [`M_INTERPOLATE_ANGLE`](../../Reference/met/MmetPut.md).

Consecutive edgels in the arrays are considered to be consecutive in their associated edge.

For edgels or points along an edge, you can associate the edgels/points with the index of their edge chain/pixel chain using the [`ChainIndexArrayPtr`](../../Reference/met/MmetPut.md)parameter. You can add to the end of an edge chain/pixel chain that has already been added to the feature.

To remove all the edgels/points from the constructed edgel feature, set the [`ControlFlag`](../../Reference/met/MmetPut.md) parameter to [`M_RESET`](../../Reference/met/MmetPut.md).

Note, if a camera calibration context is associated with the expected target image, set values in real-world units (for example, positional values). Otherwise, use pixel units.

## Parameters

### `ContextMetId` *(out, AIL_ID)*

Specifies the identifier of the metrology context. The metrology context must have been previously allocated with [`MmetAlloc`](../../Reference/met/MmetAlloc.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the label or index of a constructed edgel feature with an [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md)build operation.

*For specifying the label or index value*

| Value | Description |
| --- | --- |
| `M_FEATURE_INDEX` | Specifies the index value of a constructed feature. |
| `M_FEATURE_LABEL` | Specifies the label value of a constructed feature. |

### `NumEdgelsOrPoints` *(in, AIL_INT)*

Specifies the number of edgels/points to add to the constructed edgel feature.

### `ChainIndexArrayPtr` *(in, *AIL_INT)*

Specifies the address of the array containing the index of the edge chain/pixel chain to which an edgel/point belongs. If specifying edgels/points from several separate chains, this index array identifies the connected edgels/points that are part of the same chain. The order of the edgels in the array determines the shape of the chain. This is especially useful if the gradient angle is not provided, and[`MmetPut`](../../Reference/met/MmetPut.md)is interpolating the angle value. If unused, set this parameter to `M_NULL`.

### `PosXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinates of the edgels/points. To remove all the edgels/points in the feature, set this parameter to `M_NULL`.

### `PosYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinates of the edgels/points. To remove all the edgels/points in the feature, set this parameter to `M_NULL`.

### `AngleArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the gradient angle (orientation) of the edgels. If adding points instead of edgels, or removing the edgels/points in the feature, set this parameter to `M_NULL`.

### `ValueArrayPtr` *(in, *AIL_DOUBLE)*

Reserved for future expansion and must be set to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to automatically calculate the expected gradient angle of the edge at the specified points, when a gradient angle array has not been specified. This parameter must be set to one of the following values:

*To specify the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to ignore this parameter, or in the case when points are passed, not to calculate the gradient angle. |
| `M_INTERPOLATE_ANGLE` | Specifies to determine the expected gradient angle of the edge at the specified points. This assumes that the specified points are a connected chain of points. Angles are calculated based on interpolation of consecutive points. This can be useful when limiting the angular range for fitted features. |
| `M_RESET` | Specifies to remove all the edgels/points from the specified constructed edgel feature. In this case, the [`NumEdgelsOrPoints`](../../Reference/met/MmetPut.md) parameter must be set to 0, and the array parameters to [`M_NULL`](../../Reference/met/MmetPut.md). |
