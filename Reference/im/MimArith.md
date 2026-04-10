---
doctype: Reference
module: im
function: MimArith
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimArith"
---

# MimArith

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

> Perform a point-to-point arithmetic operation.

## Syntax

```c
void MimArith(
    AIL_DOUBLE Src1ImageBufIdOrConst,  //in
    AIL_DOUBLE Src2ImageBufIdOrConst,  //in
    AIL_ID     DstImageBufId,          //out
    AIL_INT64  Operation               //in
)
```

## Description

This function performs the specified point-to-point operation on two images, an image and a constant, an image, or a constant, storing results in the specified destination image buffer.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `Src1ImageBufIdOrConst` *(in, AIL_DOUBLE)*

Specifies the data source of the first operand. This parameter can be given an image buffer identifier or a constant. When using a constant, it will be considered to have the same type as the destination buffer unless [`M_FLOAT_PROC`](../../Reference/im/MimArith.md) or [`M_FIXED_POINT`](../../Reference/im/MimArith.md) has been added to the operation type. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Src2ImageBufIdOrConst` *(in, AIL_DOUBLE)*

Specifies the data source of the second operand. This parameter can be given an image buffer identifier or a constant. If the selected operation uses only one operand, set this parameter to `M_NULL`. When using a constant, it will be considered to have the same type as the destination buffer unless [`M_FLOAT_PROC`](../../Reference/im/MimArith.md) or [`M_FIXED_POINT`](../../Reference/im/MimArith.md) has been added to the operation type. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer identifier. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform. This parameter should be set in accordance to the operands.

*For specifying the type of operation*

| Value | Description |
| --- | --- |
| `M_ABS_SUB` | Subtracts the absolute values of the second image from the corresponding absolute values of the first image. |
| `M_ADD` | Adds the values of the first image to the corresponding values of the second image. |
| `M_AND` | Performs a bitwise AND operation between the values of the first and second images. |
| `M_ATAN2` | Performs the two-argument arctangent function, `atan2(_x_, _y_)`, using the first image for the X-coordinates and the second image for the Y-coordinates. This results in the counter-clockwise angle between the positive X-axis and each X- and Y-coordinate.

> **Note:** Note that [`M_ATAN2`](../../Reference/im/MimArith.md) uses the standard mathematical convention, where the Y-axis is oriented upwards. If you want to use the Aurora Imaging Library angle convention, negate the Y-coordinate buffer using [`MimArith`](../../Reference/im/MimArith.md) with [`M_NEG`](../../Reference/im/MimArith.md) before you use this operation.

Aurora Imaging Library establishes the resulting angle according to the type of the source image buffer, and its minimum and maximum values. For unsigned buffers, Aurora Imaging Library maps 0° to the minimum value of the two images, and 360° to the maximum value plus 1 (for 8-bit buffers, a maximum possible value of 256 maps to 360°). For signed buffers, Aurora Imaging Library maps -180° to the minimum value of the two images, and +180° to the maximum value plus 1. For float buffers, Aurora Imaging Library expects values between 0 and 1, and maps them to angles between 0° and 360°. |
| `M_AVG` | Averages the values of the first and second images. |
| `M_DIV` | Divides the values of the first image by the values of the second image.

Note that dividing a value by 0 will generate a destination pixel with an unpredictable value. This will not generate an error. |
| `M_EXP` | Raises the values of the first image to the power of the second image's values. |
| `M_LOG` | Takes the logarithm of the values of the second image, to the base of the values of the first image. |
| `M_MAX` | Compares the values of the first image with the values of the second image, and takes the maximum of the two. |
| `M_MAX_ABS` | Compares the absolute values of the first image with the absolute values of the second image, and takes the maximum of the two. |
| `M_MIN` | Compares the values of the first image with the values of the second image, and takes the minimum of the two. |
| `M_MIN_ABS` | Compares the absolute values of the first image with the absolute values of the second image, and takes the minimum of the two. |
| `M_MULT` | Multiplies the values of the first image with the values of the second image. |
| `M_NAND` | Performs a bitwise NAND operation between the values of the first and second images. |
| `M_NOR` | Performs a bitwise NOR operation between the values of the first and second images. |
| `M_OR` | Performs a bitwise OR operation between the values of the first and second images. |
| `M_SUB` | Subtracts the values of the second image from the values of the first image (for example, image 1 - image 2). |
| `M_SUB_ABS` | Takes the absolute value of the result of subtracting the values of the second image from the values of the first image. |
| `M_XNOR` | Performs a bitwise exclusive NOR operation between the values of the first and second images. |
| `M_XOR` | Performs a bitwise exclusive OR operation between the first and second images. |

*For logical operations using two image buffers*

| Value | Description |
| --- | --- |
| `M_EQUAL` | Determines if the values of the first and second images are equal. |
| `M_GREATER` | Determines if the values of the first image are greater than the second image. |
| `M_GREATER_OR_EQUAL` | Determines if the values of the first image are greater than or equal to the second image. |
| `M_LESS` | Determines if the values of the first image are less than the second image. |
| `M_LESS_OR_EQUAL` | Determines if the values of the first image are less than or equal to the second image. |
| `M_NOT_EQUAL` | Determines if the values of the first and second images are not equal. |

*For operations using one image buffer and a constant*

| Value | Description |
| --- | --- |
| `M_ADD_CONST` | Adds a constant to the values of an image. |
| `M_AND_CONST` | Performs a bitwise AND operation on the image and a constant. |
| `M_CONST_DIV` | Divides a constant by the values of an image (for example, constant / image).

Note that dividing the constant by 0 will generate a destination pixel with an unpredictable value. This will not generate an error. |
| `M_CONST_EXP` | Raises a constant to the power specified by the values of an image. |
| `M_CONST_LOG` | Takes the logarithm of the values of an image, to the base of a constant (for example LOG<sub>constant</sub>Image). |
| `M_CONST_PASS` | Copies the constant to each destination pixel. |
| `M_CONST_SUB` | Subtracts the image from the constant (for example, constant - image). |
| `M_DIV_CONST` | Divides the values of an image by a constant (for example, image / constant).

Note that dividing a value by 0 will generate an error. |
| `M_EXP_CONST` | Raises the values of the image to the power specified by the constant. |
| `M_LOG_CONST` | Takes the logarithm of a constant, to the base of the values of an image (for example LOG<sub>image</sub>Constant). |
| `M_MAX_CONST` | Compares the values of one image with a constant, and takes the maximum of the two. |
| `M_MIN_CONST` | Compares the values of one image with a constant, and takes the minimum of the two. |
| `M_MULT_CONST` | Multiplies the values of the image by the constant. |
| `M_NAND_CONST` | Performs a bitwise NAND operation with an image and a constant. |
| `M_NOR_CONST` | Performs a bitwise NOR operation with an image and a constant. |
| `M_OR_CONST` | Performs a bitwise OR operation with an image and a constant. |
| `M_SUB_CONST` | Subtracts a constant from the values of an image (for example, image - constant). |
| `M_SUB_CONST_ABS` | Takes the absolute value of the subtraction of a constant value from the values of the first image. |
| `M_XNOR_CONST` | Performs a bitwise exclusive NOR operation with an image and a constant. |
| `M_XOR_CONST` | Performs a bitwise exclusive OR operation with an image and a constant. |

*For logical operations using one image buffer and a constant*

| Value | Description |
| --- | --- |
| `M_EQUAL_CONST` | Determines if the values of an image are equal to the specified constant. |
| `M_GREATER_CONST` | Determines if the values of an image are greater than the specified constant. |
| `M_GREATER_OR_EQUAL_CONST` | Determines if the values of an image are greater than or equal to the specified constant. |
| `M_LESS_CONST` | Determines if the values of an image are less than the specified constant. |
| `M_LESS_OR_EQUAL_CONST` | Determines if the values of an image are less than or equal to the specified constant. |
| `M_NOT_EQUAL_CONST` | Determines if the values of an image are not equal to the specified constant. |

*For operations using one image buffer*

| Value | Description |
| --- | --- |
| `M_ABS` | Takes the absolute value of the image values. |
| `M_CEIL` | Rounds the image values up to the next integer.

> **Note:** This operation is only supported with float source buffers. |
| `M_CUBE` | Raises the image values to the power of 3 (`_x_ <sup>3</sup>`). |
| `M_FLOOR` | Rounds the image values down to the previous integer.

> **Note:** This operation is only supported with float source buffers. |
| `M_INTEGRAL` | Takes the integral of the image. This computes the cumulative sum of the source image at the corresponding pixels in the destination. Each pixel in the destination buffer contains the total sum of the pixels in the rectangular sections delimited by the pixel at (0, 0) and itself, inclusively. |
| `M_LN` | Performs a natural logarithm on the image values. |
| `M_LOG2` | Performs a base-2 logarithm on the image values. |
| `M_LOG10` | Performs a base-10 logarithm on the image values. |
| `M_NEG` | Performs a unary negation operation (2's complement) on an image. For example, if a pixel has a binary value of 0x01000000 (corresponding to a decimal value of 64 in a signed buffer) and you perform an [`M_NEG`](../../Reference/im/MimArith.md) operation on it, the resulting value will be 0x11000000 (corresponding to a decimal value of -64).

> **Note:** This operation is buffer dependent. That is, a signed buffer with a value of 64 will be converted to -64, whereas an unsigned buffer with a value of 64 will be converted to 192. |
| `M_NOT` | Performs a bitwise NOT operation (1's complement) on an image. For example, if a pixel has a binary value of 0x01000000 (corresponding to a decimal value of 64 in an unsigned buffer) and you perform an [`M_NOT`](../../Reference/im/MimArith.md) operation on it, the resulting value will be 0x10111111 (corresponding to a decimal value of 191).

> **Note:** This operation is buffer dependent. That is, a signed buffer with a value of 64 will be converted to -63, whereas an unsigned buffer with a value of 64 will be converted to 191. |
| `M_PASS` | Copies the source buffer to the destination image buffer. |
| `M_ROUND` | Rounds image values to the nearest integer value, and values with decimal portion 0.5 are rounded to the nearest integer value, away from 0. For example, 1.5 is rounded to 2, while -1.5 is rounded to -2.

> **Note:** This operation is only supported with float source buffers. |
| `M_ROUND_HALF_UP` | Rounds image values to the nearest integer value, and values with decimal portion 0.5 are rounded up to the next highest integer value. For example, 1.5 is rounded to 2, while -1.5 is rounded to -1.

> **Note:** This operation is only supported with float source buffers. |
| `M_SQUARE` | Raises the image values to the power of 2 (`_x_ <sup>2</sup>`). |
| `M_SQUARE_ROOT` | Performs a square root operation on the image values. |
| `M_TRUNC` | Removes the fractional portion of the image values, such that only the integer portion remains. For example, -1.3 truncates to -1, and 2.9 truncates to 2.

> **Note:** This operation is only supported with float source buffers. |

*For specifying to save the source value*

| Value | Description |
| --- | --- |
| `M_SOURCE_VALUE` | Specifies to write the source value of the selected pixel into the destination buffer. When corresponding pixels from the two source images are compared based on the specified operation, [`MimArith`](../../Reference/im/MimArith.md)writes the source value of the pixel from the selected image into the destination buffer instead of the value used during the comparison.

For example, if combined with[`M_MAX_ABS`](../../Reference/im/MimArith.md), after finding the maximum absolute value between the corresponding pixels in the two source images,[`MimArith`](../../Reference/im/MimArith.md)writes the source value of the bigger absolute value in the destination buffer. |

*For specifying to perform operations in fixed-point or floating-point format*

| Value | Description |
| --- | --- |
| `M_FIXED_POINT` | Forces the operation to be performed in a fixed point format.

Operations using this attribute result in an 0.8 fixed-point format for an 8-bit destination. That is, the 8 bits comprise only the fractional portion of the value, and no integer portion. When the destination image buffer is 16-bit, operations that use the [`M_FIXED_POINT`](../../Reference/im/MimArith.md) attribute result in an 8.8 fixed-point format (where the eight most-significant bits are the integer portion and the eight least-significant bits are the fractional portion). For a 32-bit destination, the result is in a 16.16 fixed-point format.

Note that this attribute cannot be used when using a floating-point destination buffer and it can only be used with [`M_CONST_DIV`](../../Reference/im/MimArith.md), [`M_DIV`](../../Reference/im/MimArith.md)and [`M_DIV_CONST`](../../Reference/im/MimArith.md). In addition, this attribute cannot be used in combination with [`M_FLOAT_PROC`](../../Reference/im/MimArith.md). |
| `M_FLOAT_PROC` | Forces the operation to be performed in floating-point format.

Note that, if the destination buffer is not a floating-point buffer, the resulting data of the operation will be truncated accordingly. This attribute cannot be used with [`M_DIV`](../../Reference/im/MimArith.md). In addition, this attribute cannot be used in combination with [`M_FIXED_POINT`](../../Reference/im/MimArith.md). |

*For specifying to perform logical operations*

| Value | Description |
| --- | --- |
| `M_LOGICAL` | Specifies to use the logical operation rather than the bitwise operation.

The bitwise operation performs the specified operation on each binary digit. The logical operation determines the boolean state of the image pixel or constant based on whether it is equal to 0, and then performs the specified operation on that boolean state. In the destination buffer, a result of TRUE or FALSE is represented as 1 or 0, respectively.

*[Image: IM_MimArith_Logical.png]* |

*For specifying to saturate results that overflow or underflow*

| Value | Description |
| --- | --- |
| `M_SATURATION` | Forces the operation to saturate any resulting pixel values that overflow or underflow the possible range of the destination buffer. The pixel values are clipped to fit within the buffer's range, rather than wrapped around the range.

If you do not specify [`M_SATURATION`](../../Reference/im/MimArith.md) and there is an overflow or underflow, the resulting pixel values are either clipped (saturated) or wrapped around the buffer's range (not saturated), depending on which is faster for your hardware. |

## Remarks

> Optimized in-place processing is supported (source equals destination), but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
