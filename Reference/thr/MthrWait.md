---
doctype: Reference
module: thr
function: MthrWait
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / thr / MthrWait"
---

# MthrWait

> Perform a wait operation on an Aurora Imaging Library thread or event.

## Syntax

```c
AIL_INT MthrWait(
    AIL_ID    ThreadOrEventId,  //in
    AIL_INT64 WaitOption,       //in
    AIL_INT * StatePtr          //out
)
```

## Description

This function allows you to synchronize the execution of threads by forcing the current thread to wait for the completion of the specified thread or the change of state of the specified event.

## Parameters

### `ThreadOrEventId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library thread context or event with which to be synchronized. This parameter can also be set to the Aurora Imaging Library identifier of a system.

*For specifying the thread context or event identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default Aurora Imaging Library thread context identifier associated with the current Host thread. |
| `Event identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library event ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |
| `System identifier` | Specifies the identifier of a valid system identifier ([`MsysAlloc`](../../Reference/sys/MsysAlloc.md)). |
| `Thread context identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library thread context ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |

### `WaitOption` *(in, AIL_INT64)*

Specifies the wait option.

*For specifying the wait option for threads*

| Value | Description |
| --- | --- |
| `M_THREAD_END_WAIT` | Forces the current thread to wait for the end (the death) of the specified thread. The thread currently running cannot be the thread for which you are waiting. This option can only be used for a thread allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) with [`M_THREAD`](../../Reference/thr/MthrAlloc.md).

If the [`ThreadOrEventId`](../../Reference/thr/MthrWait.md) parameter is set to an Aurora Imaging Library system identifier, using this wait option results in an error. |
| `M_THREAD_WAIT` | Forces the current thread to wait for the completion of all functions that are not asynchronous grab commands, in the specified thread's command queue.

If the [`ThreadOrEventId`](../../Reference/thr/MthrWait.md) parameter was set to an Aurora Imaging Library system identifier, the current thread waits for the completion of the current thread on the specified system. |

*For specifying the wait option for events*

| Value | Description |
| --- | --- |
| `M_EVENT_SYNCHRONIZE` | Allows the current thread to continue with execution while forcing its corresponding on-board thread, located on the same board as the specified event, to wait for the specified event to be in an [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state.

If the specified event is allocated on a board that supports asynchronous calls, Aurora Imaging Library issues the wait command to that board and allows the current thread to proceed executing while its corresponding thread on that board is waiting for the specified event to change to a signaled state. Any subsequent calls issued on that on-board thread are executed only after the event is signaled.

If the specified event is not allocated on a board that supports asynchronous calls, this value behaves like [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md) in that it forces the current thread to wait until the event is signaled or timed out. [`MthrWait`](../../Reference/thr/MthrWait.md) set to [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWait.md) always returns [`M_UNKNOWN`](../../Reference/thr/MthrWait.md).

If the event is of the type that is reset automatically ([`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md)), the state of the event is reset to [`M_NOT_SIGNALED`](../../Reference/thr/MthrControl.md), after the wait operation. |
| `M_EVENT_WAIT` | Forces the current thread to wait for the specified event to be in an [`M_SIGNALED`](../../Reference/thr/MthrControl.md) state or for the event to be timed out. The current thread will not proceed with execution until the specified event has changed to a signaled state or timed out.

If the event is of the type that is reset automatically ([`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md)), after the wait operation, the state of the event is reset to [`M_NOT_SIGNALED`](../../Reference/thr/MthrControl.md). |

*For threads*

| Value | Description |
| --- | --- |
| `M_THREAD_TIMEOUT` | Specifies the time interval after which a thread is considered to be timed out. |

*For events*

| Value | Description |
| --- | --- |
| `M_EVENT_TIMEOUT` | Specifies the time interval after which an event is considered to be timed out. |

### `StatePtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the state of the specified thread or event.

*For writing the state of the specified thread or event*

| Value | Description |
| --- | --- |
| `M_SIGNALED` | Specifies that the specified thread has successfully completed or the state of the specified event has changed. This state applies for [`M_THREAD_END_WAIT`](../../Reference/thr/MthrWait.md), [`M_THREAD_WAIT`](../../Reference/thr/MthrWait.md), and [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md). |
| `M_TIMEOUT` | Specifies that the thread or event on which the current thread was waiting timed out. This state applies for [`M_THREAD_END_WAIT`](../../Reference/thr/MthrWait.md) and [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md). |
| `M_UNKNOWN` | Specifies that the specified thread is set to [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWait.md). |

## Return Value

**Type:** `AIL_INT`

The returned value for [`M_THREAD_END_WAIT`](../../Reference/thr/MthrWait.md), [`M_THREAD_WAIT`](../../Reference/thr/MthrWait.md), and [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md) is [`M_SIGNALED`](../../Reference/thr/MthrControl.md) if the specified thread has successfully completed or if the state of the specified event has changed; it is `M_TIMEOUT` if the thread or event on which the current thread was waiting timed out. For [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWait.md), the returned value is `M_UNKNOWN`.
