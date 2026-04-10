---
doctype: Reference
module: app
function: MappTimer
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappTimer"
---

# MappTimer

> Control or inquire a timer's setting or value.

## Syntax

```c
AIL_DOUBLE MappTimer(
    AIL_ID       ContextAppId,  //out
    AIL_INT64    ControlType,   //in
    AIL_DOUBLE * TimePtr        //in-out
)
```

## Description

This function controls or inquires an Aurora Imaging Library timer's setting or value. There is a different timer for each thread of the application, and one global timer for the application. There is also a separate non-resettable trace timer used by Aurora Imaging Profiler. By default, the timer that is accessed is the timer of the current thread.

This function is useful for benchmarking operations in an application. The timer resolution varies according to the hardware and operating system used.

> **Note:** Note that functions run at a slower speed during debug mode. Therefore, once you are done debugging your application, you should compile in release mode to achieve maximum processing speed.

## Parameters

### `ContextAppId` *(out, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappTimer.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the control to exert on the timer. It can be set to one of the following:

*For timers*

| Value | Description |
| --- | --- |
| `M_TIMER_READ` | Reads the time, in sec, of the timer since the last reset. Note that the time read takes into account (subtracts) the μsec (microseconds) needed to execute [`M_TIMER_READ`](../../Reference/app/MappTimer.md).

In this case, the function is always called synchronously. |
| `M_TIMER_RESET` | Resets the timer to zero. |
| `M_TIMER_RESOLUTION` | Reads the timer resolution, in seconds. |
| `M_TIMER_WAIT` | Waits for the specified period of time, in sec, before returning.

In this case, the function is always called synchronously. |

*For all ControlType parameters*

| Value | Description |
| --- | --- |
| `M_SYNCHRONOUS` | Forces the function to wait for all pending calls of the current thread to complete before continuing. |

*For controlling and accessing the global or trace timer*

| Value | Description |
| --- | --- |
| `M_GLOBAL` | Executes the requested timer operation using the global timer. In an application, there is only one global timer that is accessible by all threads.

If used with [`M_SYNCHRONOUS`](../../Reference/app/MappTimer.md), the global timer will wait for all pending calls of the current thread to complete before continuing. |
| `M_TRACE` | Executes the requested timer operation using the trace timer. This timer is accessible by all threads in the current application.

Note that this value can only be combined with [`M_TIMER_READ`](../../Reference/app/MappTimer.md) or [`M_TIMER_RESOLUTION`](../../Reference/app/MappTimer.md). |

### `TimePtr` *(in-out, *AIL_DOUBLE)*

Specifies the address of the variable in which to store the timer information produced by [`M_TIMER_READ`](../../Reference/app/MappTimer.md) or [`M_TIMER_RESOLUTION`](../../Reference/app/MappTimer.md). Since [`MappTimer`](../../Reference/app/MappTimer.md) also returns the requested information when [`M_TIMER_READ`](../../Reference/app/MappTimer.md) or [`M_TIMER_RESOLUTION`](../../Reference/app/MappTimer.md) are selected, you can set this parameter to `M_NULL`.

## Return Value

**Type:** `AIL_DOUBLE`

The returned value, for [`M_TIMER_READ`](../../Reference/app/MappTimer.md) and [`M_TIMER_RESOLUTION`](../../Reference/app/MappTimer.md), is the requested information cast to an _AIL_DOUBLE_. For [`M_TIMER_RESET`](../../Reference/app/MappTimer.md) and [`M_TIMER_WAIT`](../../Reference/app/MappTimer.md), this function returns 0.0.
