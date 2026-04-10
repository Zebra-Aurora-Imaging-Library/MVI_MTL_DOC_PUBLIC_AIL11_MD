---
doctype: Reference
module: buf
function: MbufChild1d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufChild1d"
---

# MbufChild1d

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

> Allocate a 1D child data buffer within a specific region of the first row of a parent data buffer.

## Syntax

```c
AIL_ID MbufChild1d(
    AIL_ID   ParentBufId,  //in
    AIL_INT  OffX,         //in
    AIL_INT  SizeX,        //in
    AIL_ID * BufIdPtr      //out
)
```

## Description

This function allocates a one-dimensional child data buffer within a region of the first row of the specified, previously allocated, parent data buffer. If the parent buffer is multi-band, this function allocates a multi-band child buffer; the child is allocated within the specified one-dimensional region of the first row in each band. To allocate a child in one specific band, use [`MbufChildColor2d`](../../Reference/buf/MbufChildColor2d.md) instead of [`MbufChild1d`](../../Reference/buf/MbufChild1d.md).

The child buffer is not allocated in its own memory space; it remains part of the parent buffer. Therefore, any modification to the child buffer affects the parent and vice versa. Note, a parent buffer can have several child buffers.

A child buffer is considered a data buffer in its own right, and can be used in the same circumstances as its parent buffer. A child buffer inherits its type and attributes from the parent buffer.

If a child buffer is allocated using a parent buffer with an ROI, set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the child buffer inherits the parent buffer's ROI. You can also set an ROI in a child buffer directly. Note that the ROI of the child buffer is only accessible using the child buffer's Aurora Imaging Library identifier.

After allocating the child buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the child buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufChild1d.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufChild1d.md) was specified).

When the child buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufChild1d.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufChild1d.md) was specified, the smart identifier manages the child buffer's lifetime and you must not manually free it.

## Parameters

### `ParentBufId` *(in, AIL_ID)*

Specifies the identifier of the parent buffer.

### `OffX` *(in, AIL_INT)*

Specifies the offset of the child buffer relative to the parent buffer's top-left pixel. The offset must be within the width of the parent buffer.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the child buffer.

### `BufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the child buffer identifier or specifies the data type that the function should use to return the child buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated child buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated child buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the child buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated child buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufChild1d.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the child buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufChild1d.md) was specified).
