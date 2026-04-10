---
doctype: Reference
module: col
function: McolDistance
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolDistance"
---

# McolDistance

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

> Calculate the distance between colors.

## Syntax

```c
void McolDistance(
    AIL_ID     Src1Id,                     //in
    AIL_ID     Src2Id,                     //in
    AIL_ID     DestId,                     //out
    AIL_ID     MaskId,                     //in
    AIL_ID     DistanceAttributesArrayId,  //in
    AIL_INT64  DistType,                   //in
    AIL_DOUBLE NormalizeValue,             //in
    AIL_INT64  ControlFlag                 //in
)
```

## Description

This function calculates the point-to-point distance between colors in two images. You can also calculate the point-to-point distance between the color in one image and: a color constant, a covariance matrix, or the covariance of a specified image. The calculated distance (for each pixel) is written in the specified destination image ([`DestId`](../../Reference/col/McolDistance.md) parameter). To ignore unwanted pixels in the distance calculation, you can apply a mask.

The color distance is typically calculated only for pixels within the intersection of the source ([`Src1Id`](../../Reference/col/McolDistance.md) and [`Src2Id`](../../Reference/col/McolDistance.md)), destination ([`DestId`](../../Reference/col/McolDistance.md)), and mask ([`MaskId`](../../Reference/col/McolDistance.md)) images, with the assumption of a common origin at the top-left corner.

The Color Analysis module assumes that the color of the images you use with [`McolDistance`](../../Reference/col/McolDistance.md) is consistent. For example, you will not receive an error if you try to calculate the distance between an RGB and an HSL image, or a 16-bit and an 8-bit image. Even when color data is inconsistent, it is still processed as is, which can produce misleading results.

You can also obtain distance results after calling [`McolMatch`](../../Reference/col/McolMatch.md), which unlike [`McolDistance`](../../Reference/col/McolDistance.md), considers the color matching context's color space. For more information on how distances differ between these two functions, see [Distance between colors](../../UserGuide/C22_Color/S06_Distance_between_colors.md).

> **Note:** This function is optimized for planar buffers or for packed 32-bit BGR buffers with a width (X-size) greater than seven pixels ([`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md)). It is recommended to avoid providing both planar and packed buffers, since this requires some internal compensation.

## Parameters

### `Src1Id` *(in, AIL_ID)*

Specifies the identifier of the first source image buffer. The buffer must be an 8- or 16-bit color (3-band) unsigned image buffer.

### `Src2Id` *(in, AIL_ID)*

Specifies the identifier of the second source buffer, which can be either an image or an Aurora Imaging Library array buffer.

### `DestId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to write the results of the distance calculation. The destination buffer must be a 1-band buffer; its type is not restricted.

### `MaskId` *(in, AIL_ID)*

Specifies the identifier of the image buffer used to identify the masked (non-zero) pixels. This must be a 1- or 8-bit (1-band) unsigned buffer. To not apply a mask, set this parameter to `M_NULL`.

### `DistanceAttributesArrayId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DistType` *(in, AIL_INT64)*

Specifies the type of distance operation to perform. This parameter must be set to one of the following values:

*For specifying the distance type*

| Value | Description |
| --- | --- |
| `M_DELTA_E` | Specifies to use a Delta-E color distance, as defined by the International Commission on Illumination (CIE). A Delta-E color distance is the same as a Euclidean color distance ([`M_EUCLIDEAN`](../../Reference/col/McolDistance.md)), but specialized for the LAB (CIELAB) color space. This function assumes that the data is in the LAB color space; it does not convert the data before computing the distance. To convert the data, use [`MimConvert`](../../Reference/im/MimConvert.md)with [`M_SRGB_LINEAR_TO_LAB`](../../Reference/im/MimConvert.md) or [`M_SRGB_TO_LAB`](../../Reference/im/MimConvert.md). |
| `M_EUCLIDEAN` | Specifies to use a Euclidean color distance.

A Euclidean distance is the square root of the sum of the squared differences between the color of each pixel in the first source image buffer ([`Src1Id`](../../Reference/col/McolDistance.md)) and either the color of each pixel in the second source image buffer, or a color constant ([`Src2Id`](../../Reference/col/McolDistance.md)).

For a color constant, you must use a 3x1, one-dimensional Aurora Imaging Library array buffer. This buffer's type is unrestricted, and must contain three color band values (for example, RGB). |
| `M_MAHALANOBIS` | Specifies to use a Mahalanobis color distance.

A Mahalanobis distance is calculated between the color of each pixel in the image specified in the first source image buffer ([`Src1Id`](../../Reference/col/McolDistance.md)) and, typically, either the covariance of the image specified in the second source image buffer, or a covariance matrix ([`Src2Id`](../../Reference/col/McolDistance.md)). This distance, between a color and a distribution of colors, is similar to a Euclidean distance between the mean of the two colors, but weighted by the inverse of the covariance of the distribution. This implies that the more a color distribution varies in a direction within the color space, the less important is the distance in that direction.

Since the covariance matrix of the second source is used, the second source should usually be a distribution of colors, such as an image, and not a single solid color (a color constant). However, if you provide a color constant as the second source, Mahalanobis behaves very much like Euclidean and will yield similar results.

For a covariance matrix, you must use a 4x3, two-dimensional Aurora Imaging Library array buffer. |
| `M_MANHATTAN` | Specifies to use a Manhattan color distance.

A Manhattan distance is the sum of the absolute value of the differences between the color of each pixel in the image specified in the first source image buffer ([`Src1Id`](../../Reference/col/McolDistance.md)) and either the color of each pixel in the second source image buffer, or a color constant ([`Src2Id`](../../Reference/col/McolDistance.md)).

Note that in HSL the color's hue is stored as a separate band (H) represented as an angular position on a circular color disk. Therefore the distance between colors is equal to the smallest angular difference, rather than the absolute value of the difference.

For a color constant, you must use a 3x1, one-dimensional Aurora Imaging Library array buffer. The buffer's type is unrestricted, and must contain three color band values (for example, RGB). |

### `NormalizeValue` *(in, AIL_DOUBLE)*

Specifies the factor with which to normalize the resulting distances. The normalized distance that is written to the destination buffer must respect this buffer's limits ([`DestId`](../../Reference/col/McolDistance.md)).

*For specifying the normalization of distances*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NO_NORMALIZE`](../../Reference/col/McolDistance.md). |
| `M_MAX_NORMALIZE` | Specifies to normalize distance values according to the greatest calculated distance. This value corresponds to the result [`M_MAX_DISTANCE`](../../Reference/col/McolGetResult.md) ([`McolGetResult`](../../Reference/col/McolGetResult.md)). The normalization performed is:

- `(_Distance_/_GreatestCalculatedDistance_) x _MaximumPossibleValueOfDestBuf_` (for destination buffer's of type _integer_).
- `(_Distance_/_GreatestCalculatedDistance_)` (for destination buffer's of type _floating-point_).
  Note that if the destination buffer is of type _floating-point_, you would typically not normalize the distance results, since all this will do is return a value between 0.0 and 1.0. |
| `M_NO_NORMALIZE` | Specifies no normalization. For destination buffers of type _integer_, distance values are truncated (no decimals). For destination buffers of type _floating-point_, distance values are complete (with decimals). |
| `Value > 0.0` | Specifies to normalize distance values according to the specified normalization value. The normalization performed is:

- `(_Distance_/_NormalizationValue_) x _MaximumPossibleValueOfDestBuf_` (for destination buffer's of type _integer_).
- `(_Distance_/_NormalizationValue_)` (for destination buffer's of type _floating-point_).
  Note that if the destination buffer is of type _floating-point_, you would typically not normalize the distance results, since all this will do is return a value between 0.0 and 1.0. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
