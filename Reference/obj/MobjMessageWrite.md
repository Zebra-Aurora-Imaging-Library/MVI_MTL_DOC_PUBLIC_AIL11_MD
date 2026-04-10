---
doctype: Reference
module: obj
function: MobjMessageWrite
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjMessageWrite"
---

# MobjMessageWrite

> Writes a message to the message mailbox.

## Syntax

```c
void MobjMessageWrite(
    AIL_ID       MessageId,     //out
    const void * MessagePtr,    //in
    AIL_INT64    MessageSize,   //in
    AIL_INT64    MessageTag,    //in
    AIL_INT64    OperationFlag  //in
)
```

## Description

This function writes a message that will be added to the message mailbox.

## Parameters

### `MessageId` *(out, AIL_ID)*

Specifies the identifier of the message mailbox.

### `MessagePtr` *(in, *void)*

Specifies the address of the variable of the user message to write. An error will be generated if this parameter is set to`M_NULL`.

### `MessageSize` *(in, AIL_INT64)*

Specifies the length of the message to write, in Bytes.

### `MessageTag` *(in, AIL_INT64)*

Specifies a user-defined tag to send with the message.

### `OperationFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
