---
doctype: Reference
module: obj
function: MobjMessageRead
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjMessageRead"
---

# MobjMessageRead

> Reads data from a message sent to the message mailbox.

## Syntax

```c
AIL_INT64 MobjMessageRead(
    AIL_ID      MessageId,          //in
    void *      MessagePtr,         //out
    AIL_INT64   MessageInSize,      //in
    AIL_INT64 * MessageOutSizePtr,  //out
    AIL_INT64 * MessageTagPtr,      //out
    AIL_INT64 * StatusPtr,          //out
    AIL_INT64   OperationFlag       //in
)
```

## Description

This function reads the data from a message sent to the message mailbox.

## Parameters

### `MessageId` *(in, AIL_ID)*

Specifies the identifier of the message mailbox.

### `MessagePtr` *(out, *void)*

Specifies the address of the variable in which the message is to be written. If `M_NULL` is specified, this function will return the length of the message.

### `MessageInSize` *(in, AIL_INT64)*

Specifies the size of the array pointed to by [`MessagePtr`](../../Reference/obj/MobjMessageRead.md), in bytes. You must set this value to 0 if [`MessagePtr`](../../Reference/obj/MobjMessageRead.md) is set to `M_NULL`.

### `MessageOutSizePtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which the size of the message to be read will be written. If unused, set to`M_NULL`.

### `MessageTagPtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which the message tag associated with the message will be written. If unused, set to`M_NULL`.

### `StatusPtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which the status of the read operation will be written.

*For specifying the status of the operation*

| Value | Description |
| --- | --- |
| `M_BUFFER_TOO_SMALL` | Specifies the current message is not copied or deleted. The returned message length will be the required length in bytes. |
| `M_READ_TIMEOUT` | Specifies the read timeout has been attained. The returned message length will be 0. |
| `M_SUCCESS` | Specifies the message is copied to [`MessagePtr`](../../Reference/obj/MobjMessageRead.md). |

### `OperationFlag` *(in, AIL_INT64)*

Specifies additional options for the read operation.

*For specifying the operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |
| `M_KEEP_IN_QUEUE` | Specifies that if the mailbox is of type [`M_QUEUE`](../../Reference/obj/MobjAlloc.md), the message will not be deleted after the read operation. |
| `M_NO_TIMEOUT` | Specifies to not use the read operation timeout. This will return a status of [`M_SUCCESS`](../../Reference/obj/MobjMessageRead.md) and return 0 if no messages are ready to be read. |
| `M_REMOVE` | Specifies to remove the current message without reading it. |

## Return Value

**Type:** `AIL_INT64`

The returned value is the same as the value written in [`MessageOutSizePtr`](../../Reference/obj/MobjMessageRead.md).
