---
doctype: Reference
module: dig
function: MdigFocus
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dig / MdigFocus"
---

# MdigFocus

| Board | Supported |
| --- | --- |
| Host System | No |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Adjust a camera's lens motor to a position that produces optimum focus, or only calculate the focus quality of a specified image or an image grabbed at the current lens motor position.

## Syntax

```c
void MdigFocus(
    AIL_ID                      DigId,                  //in
    AIL_ID                      DestImageBufId,         //out
    AIL_ID                      FocusImageRegionBufId,  //out
    AIL_FOCUS_HOOK_FUNCTION_PTR FocusHookPtr,           //in
    void *                      UserDataPtr,            //in-out
    AIL_INT                     MinPosition,            //in
    AIL_INT                     StartPosition,          //in
    AIL_INT                     MaxPosition,            //in
    AIL_INT                     MaxPositionVariation,   //in
    AIL_INT64                   Operation,              //in
    AIL_INT *                   ResultPtr               //out
)
```

## Description

This function adjusts a camera's lens motor to a position that produces optimum focus, or only calculates an indicator of the focus quality (focus indicator) of a specified image or an image grabbed at the current lens motor position. The operation mode establishes which is performed.

To adjust a camera's lens motor to a position that produces optimum focus, this function calls a user-defined function to move the lens motor to a specified initial position, grabs an image, calculates the focus indicator of the grabbed image, and based on the focus indicator and the specified optimum focus search strategy, calculates a new position for the lens motor. The process repeats, calling the user-defined function to move the lens motor to the newly calculated position and comparing the new focus indicator with the old. The process continues repeating until the optimum focus position is found. You can optionally have the user-defined function perform the grab, instead of having [`MdigFocus`](../../Reference/dig/MdigFocus.md) directly perform the grab.

Regardless of the operation mode, the image is subsampled by a factor of 4, by default using a bilinear interpolation operation, before the focus indicator is calculated; you can override this default.

This function establishes the focus indicator of an image by analyzing the edges within a specified focus region (a child image of the grabbed or provided image). An image with a good focus indicator has a sharp intensity transition between its edges and their background. The resulting optimal focus position is an position between the specified minimum and maximum lens position.

Alternatively, this function can evaluate the current position and calculate the focus indicator (using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode). In this case, the focus indicator ranges from 0 to a very large number, based on the number of edges found in the grabbed image.

In either case, as the returned value increases, your camera's focus also increases, resulting in future grabbed image becoming more in focus.

The specified search strategy (set using the operation mode parameter) determines how each new position of the lens motor is calculated between grabs. For more information, see [Auto-focusing](../../UserGuide/C27_Grabbing_with_your_digitizer/S12_Autofocusing.md).

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer. If you want to provide the image (that is, either have the user-defined function perform the grab, or use an existing image) set this parameter to `M_NULL`. The provided image must be stored in the destination image buffer.

### `DestImageBufId` *(out, AIL_ID)*

Specifies the identifier of the buffer that contains the image to analyze or in which to grab. This buffer should be of an appropriate type to hold the image. This buffer must be a 1- or 3-band buffer, but cannot be a signed 3-band buffer, a 32-bit buffer, or a 1-bit buffer.

### `FocusImageRegionBufId` *(out, AIL_ID)*

Specifies the identifier of a child buffer of the destination buffer. Analysis of each grabbed image is limited to the region specified by this buffer. To analyze the entire image, set this parameter to `M_DEFAULT`.

### `FocusHookPtr` *(in, AIL_FOCUS_HOOK_FUNCTION_PTR)*

Specifies the address of the user-defined function to call before each grab. When only calculating the focus indicator (that is, using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode), this parameter is not used, and must be set to `M_NULL`.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the data that you want to make available to the user-defined function. When only calculating the focus indicator (that is, using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode), this parameter is not used, and must be set to [`M_NULL`](../../Reference/dig/MdigFocus.md).

*For specifying the address of the data*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the function does not require data. |
| `Value` | Specifies the address of the data. This address is passed to the user-defined function, through its UserDataPtr parameter, when the user-defined function is called. |

### `MinPosition` *(in, AIL_INT)*

Specifies the minimum focus position of the search. When only calculating the focus indicator (that is, using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode), this parameter is not used, and must be set to [`M_DEFAULT`](../../Reference/dig/MdigFocus.md).

*For specifying the minimum focus position of the search*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the position, in lens motor steps. |

### `StartPosition` *(in, AIL_INT)*

Specifies the starting focus position of the search. When only calculating the focus indicator (that is, using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode), this parameter is not used, and must be set to [`M_DEFAULT`](../../Reference/dig/MdigFocus.md).

*For specifying the starting focus position of the search*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is [`MinPosition`](../../Reference/dig/MdigFocus.md). |
| `MinPosition <= Value <= MaxPosition` | Specifies the position, in lens motor steps. |

### `MaxPosition` *(in, AIL_INT)*

Specifies the maximum focus position of the search. When only calculating the focus indicator (that is, using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode), this parameter is not used, and must be set to [`M_DEFAULT`](../../Reference/dig/MdigFocus.md).

*For specifying the maximum focus position of the search*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > MinPosition` *(default)* | Specifies the position, in lens motor steps. |

### `MaxPositionVariation` *(in, AIL_INT)*

Specifies the positional increment to use in the search. When only calculating the focus indicator (that is, using the [`M_EVALUATE`](../../Reference/dig/MdigFocus.md) operation mode), or when performing an [`M_SCAN_ALL`](../../Reference/dig/MdigFocus.md) or [`M_REFOCUS`](../../Reference/dig/MdigFocus.md) operation, this parameter is not used, and must be set to [`M_DEFAULT`](../../Reference/dig/MdigFocus.md).

*For specifying the positional increment*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is equal to 1/16 of the value specified by [`MaxPosition`](../../Reference/dig/MdigFocus.md). |
| `Value > 0` | Specifies the position, in lens motor steps. |

### `Operation` *(in, AIL_INT64)*

Specifies the operation mode used by this function.

*For specifying the optimum focus position search strategy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BISECTION` | Specifies that the bisection search strategy will be used. This strategy breaks down the given positional range, step-by-step, until it finds the optimum focus position. In general, the bisection strategy processes the fewest amount of images. However, it is most sensitive to noise and requires that the lens motor travel the greatest distance. |
| `M_REFOCUS` | Specifies that the refocus search strategy will be used. This strategy scans upward or downward until it finds the optimum focus position or until it reaches the minimum or maximum position. The refocus strategy is the best strategy to use when the current focus position is close to optimum. |
| `M_SCAN_ALL` | Specifies that the scan-all search strategy will be used. This strategy scans, by 1, all positions between the minimum and maximum. The scan-all strategy is the slowest but most accurate. |
| `M_SMART_SCAN` *(default)* | Specifies that the smart-scan search strategy will be used. This strategy performs three refocus searches, each with a smaller positional increment. The smart-scan strategy is a compromise between the speed of a bisection and the accuracy of a scan-all. |

*For specifying that only the focus indicator is calculated*

| Value | Description |
| --- | --- |
| `M_EVALUATE` | Specifies to only calculate the focus indicator. The user-defined function and all position parameters are unused (but must be set to the specific values mentioned previously). Instead, this operation mode grabs an image and uses it to calculate the focus indicator value. Note that, if you don't specify a digitizer identifier, the image contained in the specified destination buffer is used. |

*For specifying the number of focus positions at which to compare the focus indicator*

| Value | Description |
| --- | --- |
| `1 <= Value <= 255` | Specifies the required number of focus positions. By default, the value is 2.

Note that this number cannot be greater than the difference between the maximum and minimum positions specified by [`MaxPosition`](../../Reference/dig/MdigFocus.md) and [`MinPosition`](../../Reference/dig/MdigFocus.md), respectively. |

*For specifying additional information about the mode of operation*

| Value | Description |
| --- | --- |
| `M_NO_FILTER` | Specifies to not perform a smoothing convolution. Instead, a subsampling operation is performed (using a nearest neighbor interpolation), with a scaling factor of 4. |
| `M_NO_SUBSAMPLING` | Specifies to not perform a subsampling operation (using a nearest neighbor interpolation), with a scaling factor of 4. Instead, a smoothing convolution is performed. |

### `ResultPtr` *(out, *AIL_INT)*

Specifies the address in which to return the resulting value from the focusing operation.
