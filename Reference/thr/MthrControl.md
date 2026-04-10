---
doctype: Reference
module: thr
function: MthrControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / thr / MthrControl"
---

# MthrControl

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Yes |
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

> Control an Aurora Imaging Library thread context, Aurora Imaging Library event, or Aurora Imaging Library mutex setting.

## Syntax

```c
void MthrControl(
    AIL_ID     ThreadEventOrMutexId,  //out
    AIL_INT64  ControlType,           //in
    AIL_DOUBLE ControlValue           //in
)
```

## Description

This function controls an Aurora Imaging Library thread context, Aurora Imaging Library event, or Aurora Imaging Library mutex setting. Most of these control type settings can be inquired using [`MthrInquire`](../../Reference/thr/MthrInquire.md).

## Parameters

### `ThreadEventOrMutexId` *(out, AIL_ID)*

Specifies the identifier of a user-allocated Aurora Imaging Library thread context, event, or mutex, allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md). This parameter can also be set to the Aurora Imaging Library identifier of a system.

*For specifying the identifier of a user-allocated Aurora Imaging Library thread context, event, or mutex*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default Aurora Imaging Library thread context identifier associated with the current Host thread. |
| `Event identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library event ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |
| `Mutex identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library mutex ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |
| `System identifier` | Specifies the identifier of a valid system identifier ([`MsysAlloc`](../../Reference/sys/MsysAlloc.md)).

When the parameter is set to the Aurora Imaging Library identifier of a system, the function inquires about the current thread on the particular system. |
| `Thread context identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library thread context ([`MthrAlloc`](../../Reference/thr/MthrAlloc.md)). |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the new value to assign to the setting specified by the [`ControlType`](../../Reference/thr/MthrControl.md) parameter.

## Parameter Associations

### For thread contexts

The following [`ControlType`](../../Reference/thr/MthrControl.md) and corresponding [`ControlValue`](../../Reference/thr/MthrControl.md) settings can be specified to control Aurora Imaging Library thread contexts:

---

### `M_ACCELERATOR`

Sets whether the thread uses hardware acceleration. Hardware acceleration speeds up the execution of certain functions in a thread. Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_ACCELERATOR_PRESENT`](../../Reference/sys/MsysInquire.md) to learn if your Zebra imaging board has an accelerator.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to use hardware acceleration. |
| `M_ENABLE` *(default)* | Specifies to use hardware acceleration. |

---

### `M_DMA_COPY_MODE`

**Board availability:** Clarity UHD, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Sets the synchronization mode for copy operations when they are driven by your imaging board DMA controller. In this case, the current setting for [`M_THREAD_MODE`](../../Reference/thr/MthrControl.md) does not effect the synchronization mode.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASYNCHRONOUS` | Specifies that when possible, control will be returned to the application immediately after a copy operation is launched. |
| `M_SYNCHRONOUS` *(default)* | Specifies that the execution of a copy operation must be completed before returning control to the application. |

---

### `M_NODE_SELECT`

Sets the Aurora Imaging Library system where Aurora Imaging Library functions will run in the thread, when running multiple systems in Distributed Aurora Imaging Library.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the Aurora Imaging Library system on which the Aurora Imaging Library functions will run is selected automatically. |
| `System identifier` | Specifies a valid system identifier, which forces all Aurora Imaging Library functions to run on this system.  > **Note:** When running a monitoring model Distributed Aurora Imaging Library setup, to specify that Aurora Imaging Library functions must run on a monitored computer, the monitored computer must first publish its system identifier, using [`MobjControl`](../../Reference/obj/MobjControl.md). Once the system identifier is published, the monitoring computer must inquire it, using [`MobjInquire`](../../Reference/obj/MobjInquire.md), before using it with this control value. |

---

### `M_THREAD_COMMANDS_ABORT`

Cancels all calls queued in the thread. The remainder of the application is executed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

---

### `M_THREAD_MODE`

Sets the execution mode of the thread.  Note that a thread can only work in asynchronous mode if threads allocated on the same system can execute in asynchronous mode ([`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_THREAD_MODE`](../../Reference/sys/MsysControl.md)). If they can only execute in synchronous mode, the specified thread will work in synchronous mode regardless of this setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ASYNCHRONOUS` *(default)* | Specifies that the thread will execute in asynchronous mode, if possible. In this mode, control is returned to the Host immediately after an Aurora Imaging Library function is sent to the processor of the thread's associated system (when the system and function allow an immediate return). |
| `M_SYNCHRONOUS` | Specifies that the thread will execute in synchronous mode. In this mode, the execution of an Aurora Imaging Library function sent to the processor of the thread's associated system must be completed (execution terminated) before returning control to the Host. |

---

### `M_THREAD_PRIORITY`

Sets the priority status of the thread.

| Value | Description |
| --- | --- |
| `M_ABOVE_NORMAL` | Specifies that the thread is above normal priority. Only time critical threads will be executed before it. |
| `M_BELOW_NORMAL` | Specifies that the thread is below normal priority. Threads of normal, above normal, and time critical priority will be executed before it. |
| `M_IDLE` | Specifies that the thread is idle. The thread will remain idle until its priority status is changed. |
| `M_LOWEST` | Specifies that the thread is of the lowest priority. All other non-idle threads will be executed before it. |
| `M_NORMAL` *(default)* | Specifies that the thread is of normal priority. Threads of above normal and time critical priority will be executed before it. |
| `M_TIME_CRITICAL` | Specifies that the thread is time critical. Time critical threads will be executed before all other threads that are not time critical. |

---

### `M_THREAD_SELECT`

Sets the selectable thread, specified by the [`ThreadEventOrMutexId`](../../Reference/thr/MthrControl.md) parameter, as the destination for subsequent Aurora Imaging Library functions. This allows Aurora Imaging Library functions to be executed on systems with on-board processors.  Before calling [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_THREAD_SELECT`](../../Reference/thr/MthrControl.md) you should inquire and save the identifier of the on-board thread that is by default associated with the Host thread, using [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_CURRENT_THREAD_ID`](../../Reference/sys/MsysInquire.md). This allows you to return control to this on-board thread.  If the [`ThreadEventOrMutexId`](../../Reference/thr/MthrControl.md) parameter is set to an identifier of a thread that is not selectable, changing the settings of this control will generate an error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

### For controlling Aurora Imaging Library events

The following [`ControlType`](../../Reference/thr/MthrControl.md) and corresponding [`ControlValue`](../../Reference/thr/MthrControl.md) setting can be specified to control Aurora Imaging Library events:

---

### `M_EVENT_SET`

Sets an event to the specified state.

| Value | Description |
| --- | --- |
| `M_NOT_SIGNALED` | Sets the event to the not-signaled state. |
| `M_SIGNALED` | Sets the event to the signaled state. |

### For controlling an Aurora Imaging Library mutex

The following [`ControlType`](../../Reference/thr/MthrControl.md) and corresponding [`ControlValue`](../../Reference/thr/MthrControl.md) setting can be specified to control an Aurora Imaging Library mutex:

---

### `M_LOCK`

Forces the current thread to wait until the specified Aurora Imaging Library mutex is available and then locks it. Locking the mutex blocks all other threads from accessing the current critical section of code.  Note that the current thread can lock the same mutex several times without an error occurring. However, the current thread must unlock ([`M_UNLOCK`](../../Reference/thr/MthrControl.md)) the mutex as many times as it was locked. For example, if the current thread previously locked the mutex twice, the mutex must be unlocked twice after the critical section of code has completed.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_LOCK_TRY`

Locks the specified Aurora Imaging Library mutex if it is currently unlocked. Locking the mutex blocks all other threads from accessing the current critical section of code.  If the mutex is locked by another thread, [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md) does not force the thread to wait for the mutex to become unlocked; the thread continues executing without executing the critical section of code protected by the mutex.  To determine whether the current thread has successfully locked the mutex, use [`MthrInquire`](../../Reference/thr/MthrInquire.md) with [`M_LOCK_TRY`](../../Reference/thr/MthrInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_UNLOCK`

Unlocks the specified mutex.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |
