---
doctype: Reference
module: obj
function: MobjAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjAlloc"
---

# MobjAlloc

> Allocate an Aurora Imaging Library utility object for network communications using Distributed Aurora Imaging Library, Aurora Imaging Web Library, or HTTP.

## Syntax

```c
AIL_ID MobjAlloc(
    AIL_ID    SysId,       //in
    AIL_INT64 ObjectType,  //in
    AIL_INT64 InitFlag,    //in
    AIL_ID *  ObjIdPtr     //out
)
```

## Description

This function allocates an Aurora Imaging Library utility object (for example, a message mailbox or HTTP server) for network communication with remote computers using Distributed Aurora Imaging Library, Aurora Imaging Web Library, or HTTP. You can control and inquire an Aurora Imaging Library utility object using [`MobjControl`](../../Reference/obj/MobjControl.md) and [`MobjInquire`](../../Reference/obj/MobjInquire.md), respectively.

> **Note:** Note, to allocate all other Aurora Imaging Library objects that this module can manipulate, use the allocation function of the object's corresponding Aurora Imaging Library module (for example, to allocate an image buffer, use [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md)).

After allocating the Aurora Imaging Library utility object, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the Aurora Imaging Library utility object identifier returned is not [`M_NULL`](../../Reference/obj/MobjAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/obj/MobjAlloc.md) was specified).

When the Aurora Imaging Library utility object is no longer required, release it using[`MobjFree`](../../Reference/obj/MobjFree.md)unless [`M_UNIQUE_ID`](../../Reference/obj/MobjAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/obj/MobjAlloc.md) was specified, the smart identifier manages the Aurora Imaging Library utility object's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the Aurora Imaging Library utility object. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ObjectType` *(in, AIL_INT64)*

Specifies the type of Aurora Imaging Library utility object to allocate.

### `InitFlag` *(in, AIL_INT64)*

Specifies the initialization of the Aurora Imaging Library utility object.

### `ObjIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the Aurora Imaging Library utility object identifier or specifies the data type that the function should use to return the Aurora Imaging Library utility object identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated Aurora Imaging Library utility object; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated Aurora Imaging Library utility object; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_OBJ_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the Aurora Imaging Library utility object (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the HTTP server identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated HTTP server.

If allocation fails, [`M_NULL`](../../Reference/obj/MobjAlloc.md) is written as the identifier. |
| `Address in which to write the message mailbox identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated message mailbox.

If allocation fails, [`M_NULL`](../../Reference/obj/MobjAlloc.md) is written as the identifier. |

## Parameter Associations

### For specifying the type of Aurora Imaging Library utility object to allocate

---

### `M_HTTP_SERVER`

Creates an HTTP server.  Once configured, the HTTP server hosts files on the local computer, for read-only access by remote computers. You configure the server using [`MobjControl`](../../Reference/obj/MobjControl.md). You must set the directory on the local computer that contains the files to host (using [`M_HTTP_ROOT_DIRECTORY`](../../Reference/obj/MobjControl.md)) and enable the HTTP server (using [`M_HTTP_START`](../../Reference/obj/MobjControl.md)).  > **Note:** This setting requires Aurora Imaging Library driver update 47 (or later).

---

### `M_MESSAGE_MAILBOX`

Creates a message mailbox.  You can read from a message mailbox using [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) and write to a message mailbox using [`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_OVERWRITE` | Specifies that only one message can be kept in the mailbox and is replaced at each write. The message is kept after a read operation. |
| `M_QUEUE` *(default)* | Specifies that multiple messages can be queued in the mailbox. Messages are read on a FIFO basis. Upon a read operation, a message is removed from the queued message mailbox by default. To keep a message in the queued message mailbox after a read operation, call [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) with [`M_KEEP_IN_QUEUE`](../../Reference/obj/MobjMessageRead.md). |

## Return Value

**Type:** `AIL_ID`

The returned value is the Aurora Imaging Library utility object identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_OBJ_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/obj/MobjAlloc.md) was specified).
