---
doctype: Reference
module: thr
function: MthrInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / thr / MthrInquire"
---

# MthrInquire

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | Partial |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | Partial |
| Iris GTX | Partial |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Partial |

> Inquire about an Aurora Imaging Library thread context, event, or mutex setting.

## Syntax

```c
AIL_INT MthrInquire(
    AIL_ID    ThreadEventOrMutexId,  //in
    AIL_INT64 InquireType,           //in
    void *    UserVarPtr             //out
)
```

## Description

This function inquires about the specified Aurora Imaging Library thread context, event setting, or mutex setting.

## Parameters

### `ThreadEventOrMutexId` *(in, AIL_ID)*

Specifies the identifier of a user-allocated Aurora Imaging Library thread context, event, or mutex, allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md). This parameter should be set to one of the following values:

*For a user-allocated thread context or event*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default Aurora Imaging Library thread context identifier associated with the current Host thread. |
| `Event identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library event ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |
| `Mutex identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library mutex ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |
| `System identifier` | Specifies the identifier of a valid system identifier ([`MsysAlloc`](../../Reference/sys/MsysAlloc.md)).

When the parameter is set to the Aurora Imaging Library identifier of a system, the function inquires about the current thread on the particular system. |
| `Thread context identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library thread context ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire. This parameter can be set to one of the values below. See [`MthrControl`](../../Reference/thr/MthrControl.md) for more information about these values.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MthrInquire`](../../Reference/thr/MthrInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For thread contexts

The following [`InquireType`](../../Reference/thr/MthrInquire.md) settings can be specified to inquire about Aurora Imaging Library thread contexts.

---

### `M_ACCELERATOR`

Inquires whether the thread uses hardware acceleration.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to use hardware acceleration. |
| `M_ENABLE` *(default)* | Specifies to use hardware acceleration. |

---

### `M_DMA_COPY_MODE`

**Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the synchronization mode for copy operations when they are driven by your imaging board DMA controller.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASYNCHRONOUS` | Specifies that when possible, control will be returned to the application immediately after a copy operation is launched. |
| `M_SYNCHRONOUS` *(default)* | Specifies that the execution of a copy operation must be completed before returning control to the application. |

---

### `M_THREAD_MODE`

Inquires the thread's execution mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASYNCHRONOUS` *(default)* | Specifies that the thread will execute in asynchronous mode, if possible. |
| `M_SYNCHRONOUS` | Specifies that the thread will execute in synchronous mode. |

---

### `M_THREAD_PRIORITY`

Inquires the priority setting of the thread.

| Value | Description |
| --- | --- |
| `M_ABOVE_NORMAL` | Specifies that the thread is above normal priority. |
| `M_BELOW_NORMAL` | Specifies that the thread is below normal priority. |
| `M_IDLE` | Specifies that the thread is idle. |
| `M_LOWEST` | Specifies that the thread is of the lowest priority. |
| `M_NORMAL` *(default)* | Specifies that the thread is of normal priority. |
| `M_TIME_CRITICAL` | Specifies that the thread is time critical. |

### For Aurora Imaging Library events

The following [`InquireType`](../../Reference/thr/MthrInquire.md) settings can be specified to inquire about Aurora Imaging Library events.

---

### `M_EVENT_MODE`

Inquires the reset mode of the event.

| Value | Description |
| --- | --- |
| `M_AUTO_RESET` | Specifies that the event is reset automatically. |
| `M_MANUAL_RESET` | Specifies that the event is reset manually. |

---

### `M_EVENT_STATE`

Inquires the state of the event.

| Value | Description |
| --- | --- |
| `M_NOT_SIGNALED` | Specifies the not-signaled state. |
| `M_SIGNALED` | Specifies the signaled state. |

### For mutexes

The following [`InquireType`](../../Reference/thr/MthrInquire.md) settings can be specified to inquire about Aurora Imaging Library mutexes.

---

### `M_LOCK_TRY`

Inquires whether the specified Aurora Imaging Library mutex is locked by the current thread.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the current thread has not locked the specified mutex. |
| `M_YES` | Specifies that the current thread has locked the specified mutex. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
