---
doctype: Reference
module: im
function: MimDistance
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimDistance"
---

# MimDistance

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

> Perform a distance transformation.

## Syntax

```c
void MimDistance(
    AIL_ID    SrcImageBufId,     //in
    AIL_ID    DstImageBufId,     //out
    AIL_INT64 DistanceTransform  //in
)
```

## Description

This function determines the shortest distance between each blob pixel and the blob's background, and assigns this distance to the pixel. It produces a type of contour mapping of a blob.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the resulting image. This parameter must be given an image buffer identifier.

### `DistanceTransform` *(in, AIL_INT64)*

Specifies the way in which the minimum distance from blob pixel to background pixel is calculated. This parameter can be set to one of the following:

*For specifying how to calculate the minimum distance*

| Value | Description |
| --- | --- |
| `M_CHAMFER_3_4` | Approximates the minimum distance using horizontal, vertical or diagonal pixel steps. Horizontal and vertical steps are counted as 3; diagonal steps are counted as 4. Then, the resulting distance is normalized by a factor of 3. This transform provides the best approximation to Euclidean distance.

This transform requires that the destination buffer be deep enough to hold a number at least three times the maximum distance from a blob pixel to its edge. For example, an 8-bit buffer (255 max) can be used for a maximum distance of 85 pixels and a 16-bit buffer (65535 max) for a maximum distance of 21845 pixels.

3x3 Distance Matrix:

*[Image: Chamfer.png]* |
| `M_CHESSBOARD` | Approximates the minimum distance using horizontal, vertical, or diagonal pixel steps. All steps count as 1.

3x3 Distance Matrix:

*[Image: Chessboard.png]* |
| `M_CITY_BLOCK` | Approximates the minimum distance using only horizontal or vertical pixel steps. Horizontal and vertical steps count as 1.

3x3 Distance Matrix:

*[Image: Cityblock.png]* |
| `M_EUCLIDEAN_DIST` | Calculates the true minimum Euclidean distance using a straight-line path.

The Euclidean distance is calculated using the following formula, where:

||
||
|  |
| *[Image: Euclidean_distance.png]* | - _x<sub>1</sub> _ and _y<sub>1</sub> _ represent the coordinates of the blob pixel.<br/>- _x<sub>2</sub> _ and _y<sub>2</sub> _ represent the coordinates of the nearest background pixel. |

Note, the source buffer must be a 1-band, 8-bit image buffer of type [`M_UNSIGNED`](../../Reference/buf/MbufInquire.md) and the destination buffer must be a 1-band, 32-bit image buffer of type [`M_FLOAT`](../../Reference/buf/MbufInquire.md). |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
