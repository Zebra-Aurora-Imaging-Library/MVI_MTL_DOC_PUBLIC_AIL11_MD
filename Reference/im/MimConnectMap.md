---
doctype: Reference
module: im
function: MimConnectMap
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimConnectMap"
---

# MimConnectMap

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

> Perform a 3x3 binary connectivity mapping.

## Syntax

```c
void MimConnectMap(
    AIL_ID SrcImageBufId,  //in
    AIL_ID DstImageBufId,  //out
    AIL_ID LutBufId        //in
)
```

## Description

This function performs a 3x3 binary connectivity mapping. It calculates a connectivity code for each pixel in the source image, treating the source image as if it were binary (that is, all non-zero pixels are treated as 1); it then maps the codes through the specified LUT buffer. Specifically, for each source pixel, this function concatenates the binary values of the pixels in a pixel's 3x3 neighborhood, and then uses this 9-bit number to address the specified LUT. The value at this LUT address is then written in the specified destination buffer at the pixel's corresponding position.

Pixel connectivity codes are determined in the following order:

*[Image: connectivitycode.png]*

*[Image: ConnectivityCodeFormula.png]*

`_Result_ = LUTMAP[_Connectivity code_]`

The overscan pixels are automatically set to 0 (zero), which will produce the most accurate possible results for the image border pixels.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer identifier.

### `LutBufId` *(in, AIL_ID)*

Specifies the identifier of the LUT buffer. Since each connectivity code has 9 bits, you should supply a LUT buffer with at least 512 (2_ <sup>9</sup> _) entries; otherwise, unpredictable results can occur.
