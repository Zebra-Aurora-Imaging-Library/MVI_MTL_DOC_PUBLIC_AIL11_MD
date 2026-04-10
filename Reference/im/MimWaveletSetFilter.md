---
doctype: Reference
module: im
function: MimWaveletSetFilter
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimWaveletSetFilter"
---

# MimWaveletSetFilter

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

> Set wavelet filters for a custom wavelet context.

## Syntax

```c
void MimWaveletSetFilter(
    AIL_ID    WaveletContextImId,            //out
    AIL_ID    LowForwardRealFilterId,        //in
    AIL_ID    HighForwardRealFilterId,       //in
    AIL_ID    LowReverseRealFilterId,        //in
    AIL_ID    HighReverseRealFilterId,       //in
    AIL_ID    LowForwardImaginaryFilterId,   //in
    AIL_ID    HighForwardImaginaryFilterId,  //in
    AIL_ID    LowReverseImaginaryFilterId,   //in
    AIL_ID    HighReverseImaginaryFilterId,  //in
    AIL_INT64 ControlFlag                    //in
)
```

## Description

This function allows you to set wavelet filters for a custom wavelet context.

Filter parameters must specify the identifier of a kernel buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md) with [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md). Each kernel buffer type must be 32-bit float (32 + [`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md)). The height ([`SizeY`](../../Reference/buf/MbufAlloc2d.md)) of each kernel buffer must be 1. The width ([`SizeX`](../../Reference/buf/MbufAlloc2d.md)) of each kernel buffer must be identical. Kernels contain the filter values used by the wavelet transformation, when you call [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) with a custom wavelet context.

Each filter parameter can be set to [`M_NULL`](../../Reference/im/MimWaveletSetFilter.md) if you do not require its information. At least one filter parameter must specify the identifier of a kernel.

## Parameters

### `WaveletContextImId` *(out, AIL_ID)*

Specifies the identifier of a custom wavelet image processing context allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md).

### `LowForwardRealFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the real part of the complex numbers used for the low-pass filter of a forward wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `HighForwardRealFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the real part of the complex numbers used for the high-pass filter of a forward wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `LowReverseRealFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the real part of the complex numbers used for the low-pass filter of a reverse wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `HighReverseRealFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the real part of the complex numbers used for the high-pass filter of a reverse wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `LowForwardImaginaryFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the imaginary part of the complex numbers used for the low-pass filter of a forward wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `HighForwardImaginaryFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the imaginary part of the complex numbers used for the high-pass filter of a forward wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `LowReverseImaginaryFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the imaginary part of the complex numbers used for the low-pass filter of a reverse wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `HighReverseImaginaryFilterId` *(in, AIL_ID)*

Specifies the identifier of the kernel buffer containing the imaginary part of the complex numbers used for the high-pass filter of a reverse wavelet transformation. The kernel must be 32-bit float, have a height of 1, and a width that is identical to the other kernels. If you do not require the filter information related to this parameter, set it to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
