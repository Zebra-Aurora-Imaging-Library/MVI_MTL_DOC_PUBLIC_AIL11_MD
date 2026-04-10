---
doctype: Reference
module: buf
function: MbufGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGetHookInfo"
---

# MbufGetHookInfo

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

> Get information about a hook event.

## Syntax

```c
AIL_INT MbufGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the event that caused the hook-handler function to be called. [`MbufGetHookInfo`](../../Reference/buf/MbufGetHookInfo.md) should only be called within the scope of a buffer or container hook-handler function (see [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the buffer or container event identifier received by the hook-handler function (see [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md)).

### `InfoType` *(in, AIL_INT64)*

Specifies the event type and the type of information about the event to return.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For the event type

---

### `M_GC_FEATURE_CHANGE_NAME`

**Board availability:** GenTL, V4L2

Retrieves the name of the GenICam feature that changed. This information type is only available if[`MbufGetHookInfo`](../../Reference/buf/MbufGetHookInfo.md) is used from a function hooked to GenTL feature change events using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_FEATURE_CHANGE`](../../Reference/buf/MbufHookFunction.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the feature name. |

---

### `M_MODIFIED_BUFFER`

Retrieves information about a modified buffer or container type of event. This event occurs if the hook-handler function was hooked using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_MODIFIED_BUFFER`](../../Reference/buf/MbufHookFunction.md).

---

### `M_OBJECT_ID`

Retrieves the identifier of the container that was modified. This event occurs if the hook-handler function was hooked using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_LAYOUT_MODIFIED`](../../Reference/buf/MbufHookFunction.md).

| Value | Description |
| --- | --- |
| `Container identifier` | Specifies the identifier of the container that was modified. |

### Combination Constants — For the modified buffer type of event

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set the type of information to return.

#### `M_BUFFER_ID`

Retrieves the Aurora Imaging Library identifier of the modified buffer or container.

#### `M_GRAB_TIME_STAMP`

**Board availability:** Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the time when Aurora Imaging Library finished grabbing into the buffer, expressed in seconds. Typically, this will be in seconds since the Host computer was started. This can be used to see if a frame has been missed since the last time stamp.

#### `M_MOVED`

Returns whether the buffer modification event was triggered by a move operation performed by [`MbufChildMove`](../../Reference/buf/MbufChildMove.md); that is, the buffer is a child buffer and its X/Y offset was changed.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the buffer modification event was not triggered by a move operation. |
| `M_YES` | Specifies that the buffer modification event was triggered by a move operation. |

#### `M_REGION_OFFSET_X`

Retrieves the X-offset of the modified region (of the buffer).

#### `M_REGION_OFFSET_Y`

Retrieves the Y-offset of the modified region (of the buffer).

#### `M_REGION_SIZE_X`

Retrieves the width of the modified region (of the buffer).

#### `M_REGION_SIZE_Y`

Retrieves the height of the modified region (of the buffer).

#### `M_RESIZED`

Returns whether the buffer modification event was triggered by a resize operation performed by [`MbufChildMove`](../../Reference/buf/MbufChildMove.md); that is; the buffer is a child buffer and its XY-size was changed.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the buffer modification event was not triggered by a resize operation. |
| `M_YES` | Specifies that the buffer modification event was triggered by a resize operation. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

> **Board availability:** GenTL, V4L2

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (\![`M_NULL`](../../Reference/buf/MbufGetHookInfo.md)) value is returned.
