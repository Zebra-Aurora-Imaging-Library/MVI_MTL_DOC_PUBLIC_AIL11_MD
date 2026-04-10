---
doctype: Reference
module: thr
function: MthrWaitMultiple
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / thr / MthrWaitMultiple"
---

# MthrWaitMultiple

> Perform a wait operation on multiple Aurora Imaging Library events.

## Syntax

```c
AIL_INT MthrWaitMultiple(
    const AIL_ID * EventArrayIdPtr,  //in
    AIL_INT        EventArraySize,   //in
    AIL_INT64      WaitOption,       //in
    AIL_INT *      StatePtr          //out
)
```

## Description

This function allows you to synchronize the execution of threads by forcing the current thread to wait for one of the Aurora Imaging Library events identified in a user-supplied array to change state. To force the current thread to wait for all events identified in the user-supplied array to change state, add [`M_ALL_OBJECTS`](../../Reference/thr/MthrWaitMultiple.md) to [`M_EVENT_WAIT`](../../Reference/thr/MthrWaitMultiple.md) or [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWaitMultiple.md) when setting the [`WaitOption`](../../Reference/thr/MthrWaitMultiple.md) parameter.

## Parameters

### `EventArrayIdPtr` *(in, *AIL_ID)*

Specifies the address of a user-supplied array that contains the identifiers of the Aurora Imaging Library events for which to wait. All Aurora Imaging Library event identifiers must be allocated, using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md), on the same system.

### `EventArraySize` *(in, AIL_INT)*

Specifies the number of events in the user-supplied array.

### `WaitOption` *(in, AIL_INT64)*

Specifies the wait option.

### `StatePtr` *(out, *AIL_INT)*

Specifies the address in which to write the returned value.

## Parameter Associations

### For specifying the wait option

---

### `M_EVENT_SYNCHRONIZE`

Allows the current thread to continue executing while forcing its corresponding on-board thread, located on the same board as the events identified in the user-supplied array, to wait for one of the events to be in an [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state or for a time out to occur.  If the events are allocated on a board that supports asynchronous calls, Aurora Imaging Library issues the wait command to that board and allows the current thread to continue executing. Its corresponding thread, on that board, waits for an event identified in the array to change to a signaled state. Any subsequent calls issued on that on-board thread are executed only after an event is signaled.  If the specified event is not allocated on a board that supports asynchronous calls, this value behaves like [`M_EVENT_WAIT`](../../Reference/thr/MthrWaitMultiple.md) in that it forces the current thread to wait until the event is signaled or timed out. [`MthrWaitMultiple`](../../Reference/thr/MthrWaitMultiple.md) set to [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWaitMultiple.md) always returns [`M_UNKNOWN`](../../Reference/thr/MthrWaitMultiple.md).  If the event that changes state is of the type that is reset automatically ([`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md)), the state of the event is reset to [`M_NOT_SIGNALED`](../../Reference/thr/MthrControl.md), after the wait operation.

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that [`WaitOption`](../../Reference/thr/MthrWaitMultiple.md) parameter is set to [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWaitMultiple.md). |

---

### `M_EVENT_WAIT`

Forces the current thread to wait for an event, identified in the user-supplier array, to change to the [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state or for the events to timeout. The current thread will not continue until one of the specified events has changed to a signaled state or timed out.  If the event that changed state is of the type that is reset automatically ([`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md)), after the wait operation, the state of the event is reset to [`M_NOT_SIGNALED`](../../Reference/thr/MthrControl.md).

| Value | Description |
| --- | --- |
| `M_TIMEOUT` | Specifies that none of the events in the array have changed state before the time out interval is reached. |
| `Value` | Specifies the index of the event that changed to the [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state. |

### Combination Constants — For specifying that the thread must wait for all the events to change state

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the current thread must wait for all events to change state or for a timeout to occur.

#### `M_ALL_OBJECTS`

Waits for all the events identified in the user-supplied array to be in an [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state or for the events to time out.

| Value | Description |
| --- | --- |
| `M_SIGNALED` | Specifies that all the events have changed state. |
| `M_TIMEOUT` | Specifies that the time out interval has been reached before all the events in the array have changed state. |
| `M_UNKNOWN` | Specifies that [`WaitOption`](../../Reference/thr/MthrWaitMultiple.md) parameter is set to [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWaitMultiple.md). |

### Combination Constants — For events

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the time interval after which an event is considered to be timed out.

| Value | Description |
| --- | --- |
| `M_EVENT_TIMEOUT` | Specifies the time interval after which an event is considered to be timed out. |

## Return Value

**Type:** `AIL_INT`

The returned value for [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md) is the index of the event that changed to the [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state, or `M_TIMEOUT` if none of the events in the array changed state before the time out interval is reached. For [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md) + [`M_ALL_OBJECTS`](../../Reference/thr/MthrWaitMultiple.md), the returned value is [`M_SIGNALED`](../../Reference/thr/MthrControl.md) if all the events specified in the array changed state, or [`M_TIMEOUT`](../../Reference/thr/MthrWaitMultiple.md) if the time out interval is reached before all the events in the array have changed state. For [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWait.md), the returned value is `M_UNKNOWN`.
