---
doctype: Reference
module: buf
function: MbufFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufFree"
---

# MbufFree

> Free a data buffer or container.

## Syntax

```c
void MbufFree(
    AIL_ID BufOrContainerBufId  //in
)
```

## Description

This function deallocates a previously allocated data buffer or container. The memory reserved for the specified buffer or container is released.

> **Note:** All of a container's components are freed automatically when the container is freed. You can also free components manually using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md). [`MbufFree`](../../Reference/buf/MbufFree.md) cannot be used to free buffers that are components.

Before freeing a parent buffer or container, you must free all of its children.

Note that LUT buffers, once associated with another buffer, a digitizer, or a display, should either be disassociated before being freed, or freed after the associated buffer, digitizer, or display is freed.

To disassociate a digitizer or display from a LUT buffer, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LUT_ID`](../../Reference/dig/MdigControl.md) set to [`M_DEFAULT`](../../Reference/dig/MdigControl.md) or [`MdispLut`](../../Reference/disp/MdispLut.md) with [`M_DEFAULT`](../../Reference/disp/MdispLut.md) respectively. To disassociate another buffer from a LUT buffer, use [`MbufControl`](../../Reference/buf/MbufControl.md)with [`M_ASSOCIATED_LUT`](../../Reference/buf/MbufControl.md) set to [`M_DEFAULT`](../../Reference/buf/MbufControl.md).

All buffers and containers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocDefault.md) was specified during allocation.

## Parameters

### `BufOrContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the data buffer or container to deallocate.
