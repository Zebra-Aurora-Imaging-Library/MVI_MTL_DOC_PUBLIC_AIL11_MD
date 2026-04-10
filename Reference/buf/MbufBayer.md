---
doctype: Reference
module: buf
function: MbufBayer
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufBayer"
---

# MbufBayer

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Decode the color information of a single-band, Bayer color-encoded image.

## Syntax

```c
void MbufBayer(
    AIL_ID    SrcImageBufId,   //in
    AIL_ID    DestImageBufId,  //out
    AIL_ID    CoefOrExpId,     //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function converts a single-band, Bayer color-encoded image into a 1- or 3-band image. When using a single-band buffer as the destination, the destination will be filled with the Y-band obtained from the RGB to Y conversion when the Bayer image is decoded.

Optionally, you can specify to perform gamma correction and/or white balancing on the image if the appropriate exponents and coefficients are provided, respectively. First, gamma correction is applied to the raw RGB values of the Bayer color-encoded image. Second, white balancing is performed on the gamma corrected RGB values. Third, the demosaicing process converts the single-band, Bayer color-encoded image to a 1- or 3-band image. Finally, color correction is performed if [`M_COLOR_CORRECTION`](../../Reference/buf/MbufBayer.md)is specified. Color correction can only be used with the [`M_ADAPTIVE`](../../Reference/buf/MbufBayer.md)algorithm.

Note that you can use [`MbufBayer`](../../Reference/buf/MbufBayer.md) to automatically calculate the appropriate white balancing coefficients. To do so, specify a Bayer color-encoded source image that in reality should be a uniform shade of gray that is not too saturated, and add [`M_WHITE_BALANCE_CALCULATE`](../../Reference/buf/MbufBayer.md) to the [`ControlFlag`](../../Reference/buf/MbufBayer.md) parameter. After converting the image and calculating the appropriate coefficients, the coefficients are written to the specified Aurora Imaging Library array buffer and the converted image is then corrected using these automatically calculated coefficients. Alternatively, you could fill an array with custom values, and then load these values (in order) into the Aurora Imaging Library array using [`MbufPut1d`](../../Reference/buf/MbufPut1d.md) or [`MbufPut2d`](../../Reference/buf/MbufPut2d.md). For more information, see [White balancing your Bayer images](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md). Ensure that the image was grabbed under the same lighting conditions as subsequent source images.

> **Note:** Note that you cannot calculate the white balance coefficients and specify an adaptive demosaicing in the same call to [`MbufBayer`](../../Reference/buf/MbufBayer.md).

For more information on Bayer color-encoded images, the conversion, and the actual equations used to perform white balancing and gamma correction, see [Using images acquired with a Bayer color filter](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

Note that if the scale of the image is changed (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_SCALE...`](../../Reference/dig/MdigControl.md)) to a value other than 1 prior to grabbing a Bayer image, the Bayer image will not be converted properly; some of the Bayer pattern is lost during the scaling process, rendering color recovery impossible.

### System specific

| Board(s) | Note |
|---|---|
| Iris GTX | When using a digitizer that supports on-board Bayer conversion, the conversion of a single-band, Bayer color-encoded image into a 1- or 3-band image can be performed using either on-board buffers, or off-board (Host) buffers. When the operation is performed using both source and destination on-board image buffers, your digitizer must be able to perform the Bayer conversion on-board; otherwise, the operation will be performed on the Host.  In addition, the Bayer operation can only be performed on-board if there is sufficient memory for the operation to complete (for example, to hold internal buffers); otherwise, the operation will be performed on the Host, and the results copied to your on-board destination buffer. This will result in a speed-decrease of the Bayer operation.  To force the Bayer operation to be performed on-board, use [`MdigControl`](../../Reference/dig/MdigControl.md). For more information, refer to the _Hardware-specific Notes_ and the _Installation and Hardware Reference_ for your product. Note that, when using [`MdigControl`](../../Reference/dig/MdigControl.md), gamma correction is not available. |

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The image buffer must be a 1- or 3-band buffer. To achieve maximum conversion performance, the source image buffer should be an unsigned 1-band image buffer. If you specify a 3-band buffer, only the first band will be used.

### `DestImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. The results of the white balancing, gamma correction, and color correction are clipped, if necessary, to the maximum value of the source buffer.

*For the destination buffer*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies the destination buffer will not be used. Note that this can only be used when calculating white balance coefficients (using [`M_WHITE_BALANCE_CALCULATE`](../../Reference/buf/MbufBayer.md)). In this case, the white balance coefficients are calculated but the source image is not converted. |
| `Destination image buffer identifier` | Specifies the identifier of the destination image buffer.

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

When you convert the image using the adaptive algorithm (specified using [`M_ADAPTIVE`](../../Reference/buf/MbufBayer.md)), it is recommended that you use a planar RGB buffer (any RGB color buffer format + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)). The other destination formats mentioned below can be used, but require an extra conversion operation that increases processing time.

The RGB result is saturated according to the source buffer's maximum value, set using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MAX`](../../Reference/buf/MbufControl.md). With YUV or 1-band destination buffers, the saturation is applied to the intermediate RGB result, before doing the color conversion.

The destination buffer should be either an 8- or 16-bit monochrome, or a 3-band color buffer in one of the following formats:

- [`M_BGR32`](../../Reference/buf/MbufAllocColor.md)+[`M_PACKED`](../../Reference/buf/MbufAllocColor.md).
- [`M_YUV16_YUYV`](../../Reference/buf/MbufAllocColor.md)(equivalent to [`M_YUV16`](../../Reference/buf/MbufAllocColor.md)+[`M_PACKED`](../../Reference/buf/MbufAllocColor.md)). |

### `CoefOrExpId` *(in, AIL_ID)*

Specifies whether white balancing and gamma correction are performed, and the coefficients and exponents used to perform these corrections, respectively.

*For white balancing and gamma correction*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs no white balancing or gamma correction on the source image; only the conversion is performed. |
| `Array buffer identifier` | Specifies the Aurora Imaging Library array buffer which contains the coefficients and exponents to perform the corrections. The buffer must be allocated with [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), as a single-band 32-bit floating-point buffer with an [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) attribute.

The first 3 elements in the Aurora Imaging Library array buffer specify the coefficients to perform the white balancing on the gamma corrected RGB values (if gamma correction is performed at all). If you add [`M_WHITE_BALANCE_CALCULATE`](../../Reference/buf/MbufBayer.md) to the [`ControlFlag`](../../Reference/buf/MbufBayer.md) parameter, these three elements are overwritten with the coefficients that are automatically calculated. Note that white balancing coefficients must be positive.

The next 3 elements, if any, specify the exponents to perform the gamma correction on the raw RGB values, respectively. Note that the gamma correction exponents must be positive, are typically in the range of 0.0 to 1.0, and are most commonly specified as 0.45. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the Bayer pattern to use in the conversion, and whether the white balancing coefficients will be calculated. This parameter can be set to one of the following:

*For the Bayer pattern*

| Value | Description |
| --- | --- |
| `M_BAYER_BG` | Specifies the Bayer pattern where the top-left pixel is band and the next pixel is green. That is, pixel [0,0] is blue, pixel [1,0] is green and pixel [1,1] is red in the source image.

*[Image: bayer_bg.png]* |
| `M_BAYER_GB` | Specifies the Bayer pattern where the top-left pixel is green and the next pixel is blue. That is, pixel [0,0] is green, pixel [1,0] is blue and pixel [0,1] is red in the source image.

*[Image: bayer_gb.png]* |
| `M_BAYER_GR` | Specifies the Bayer pattern where the top-left pixel is green and the next pixel is red. That is, pixel [0,0] is green, pixel [1,0] is red and pixel [0,1] is blue in the source image.

*[Image: bayer_gr.png]* |
| `M_BAYER_RG` | Specifies the Bayer pattern where the top-left pixel is red and the next pixel is green. That is, pixel [0,0] is red, pixel [1,0] is green and pixel [1,1] is blue in the source image.

*[Image: bayer_rg.png]* |

*For white balancing the source image*

| Value | Description |
| --- | --- |
| `M_WHITE_BALANCE_CALCULATE` | Determines the 3 coefficients required to white balance the source image, and writes these coefficients into the [`CoefOrExpId`](../../Reference/buf/MbufBayer.md) array. The image is then white balanced using the 3 calculated coefficients. If gamma exponents are specified, the image is first gamma corrected before the white balance coefficients are calculated. Note that to use [`M_WHITE_BALANCE_CALCULATE`](../../Reference/buf/MbufBayer.md), the source image must be a uniform shade of gray that is not too saturated. |

*For bit shifting the result*

| Value | Description |
| --- | --- |
| `M_BAYER_BIT_SHIFT` | Indicates to shift the result by the specified number of bits.

Note that this operation is applied to the RGB values resulting from the demosaicing process (after the color conversion); with YUV or 1-band destination buffers, the shift is applied to the intermediate RGB result, before doing the color conversion.

Signed buffers are considered as unsigned. |

*For overriding the bilinear interpolation demosaicing algorithm*

| Value | Description |
| --- | --- |
| `M_ADAPTIVE` | Specifies to use the adaptive demosaicing algorithm for the conversion process. Using this algorithm minimizes false colors and the zipper effect.

> **Note:** Note you cannot use this constant in combination with [`M_WHITE_BALANCE_CALCULATE`](../../Reference/buf/MbufBayer.md). |
| `M_ADAPTIVE_FAST` | Specifies to use a fast version of the adaptive demosaicing algorithm for the conversion process; using this algorithm gives you a good compromise between speed of the default bilinear algorithm and [`M_ADAPTIVE`](../../Reference/buf/MbufBayer.md) quality.

> **Note:** Note you cannot use this constant in combination with [`M_WHITE_BALANCE_CALCULATE`](../../Reference/buf/MbufBayer.md). |
| `M_AVERAGE_2X2` | Specifies to use a 2x2 neighborhood kernel to perform the demosaicing. By using this algorithm, the zipper effect is minimized, but the resulting image is shifted upwards and to the left by a small amount. |

*For*

| Value | Description |
| --- | --- |
| `M_COLOR_CORRECTION` | Specifies that a color correction is performed to further eliminate false colors. |
