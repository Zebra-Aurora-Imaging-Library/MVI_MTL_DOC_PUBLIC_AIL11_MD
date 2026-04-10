---
doctype: Reference
module: thr
function: MthrAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / thr / MthrAlloc"
---

# MthrAlloc

> Allocate an Aurora Imaging Library thread context, event, or mutex.

## Syntax

```c
AIL_ID MthrAlloc(
    AIL_ID                  SystemId,                //in
    AIL_INT64               ObjectType,              //in
    AIL_INT64               ControlFlag,             //in
    AIL_THREAD_FUNCTION_PTR ThreadFctPtr,            //in
    void *                  UserDataPtr,             //in-out
    AIL_ID *                ThreadEventOrMutexIdPtr  //out
)
```

## Description

This function allocates an Aurora Imaging Library thread context, event, or mutex.

Threads are function streams, used to ensure sequential execution of operations within the same thread, while allowing simultaneous yet independent execution of operations in other threads. [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) can allocate threads using two different methods:

- With the first method ([`M_THREAD`](../../Reference/thr/MthrAlloc.md)), [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) creates an Aurora Imaging Library thread context for the new thread, and allows you to specify a pointer to a function that will be executed by the thread. When a thread contains a function call whose target processor is an on-board processor that supports multi-threading, Aurora Imaging Library automatically creates a corresponding thread on that system's on-board processor. The functions of the Thread module allow you to synchronize threads running on the Host and/or various Aurora Imaging Library systems.
- With the second method, [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) creates a selectable thread ([`M_SELECTABLE_THREAD`](../../Reference/thr/MthrAlloc.md)). Selectable threads are threads executed on an on-board processor that supports multi-threading but can be controlled from a single corresponding thread on the Host. Use [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_THREAD_SELECT`](../../Reference/thr/MthrControl.md) to send Aurora Imaging Library functions to be executed by a selectable thread.

For more information on these two types of threads, see [Multi-threading](../../UserGuide/C66_Using_AIL_with_multiprocessing_and_under_multithread_systems/S03_Multithreading.md).

An Aurora Imaging Library event is a synchronization marker that can signal the completion of a required set of functions in one thread to other threads. [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) can allocate Aurora Imaging Library events, or map a new Aurora Imaging Library event to an existing Aurora Imaging Library event.

A mutex is a mutual exclusion object that allows threads to synchronize access to shared resources. Once a mutex is allocated on the specified system, you can lock and unlock critical sections of code. Locking a critical section of code ensures that no two threads can access the same data at the same time. To lock an Aurora Imaging Library mutex, you must call [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_LOCK`](../../Reference/thr/MthrControl.md) or [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md) immediately preceding the section of code to lock. If you lock a mutex, you must unlock it at the end of the critical section of code using [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_UNLOCK`](../../Reference/thr/MthrControl.md).

To control or inquire about an Aurora Imaging Library thread context, event, or mutex, use the [`MthrControl`](../../Reference/thr/MthrControl.md) or [`MthrInquire`](../../Reference/thr/MthrInquire.md) function, respectively. To synchronize the execution of threads, use [`MthrWait`](../../Reference/thr/MthrWait.md).

After allocating the thread context, event, or mutex you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the thread context, event, or mutex identifier returned is not [`M_NULL`](../../Reference/thr/MthrAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/thr/MthrAlloc.md) was specified).

When the thread context, event, or mutex is no longer required, release it using[`MthrFree`](../../Reference/thr/MthrFree.md)unless [`M_UNIQUE_ID`](../../Reference/thr/MthrAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/thr/MthrAlloc.md) was specified, the smart identifier manages the thread context, event, or mutex's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate an Aurora Imaging Library thread context, event, or mutex.

*For specifying the system*

| Value | Description |
| --- | --- |
| `System identifier` | Specifies the Aurora Imaging Library identifier of the required system, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ObjectType` *(in, AIL_INT64)*

Specifies the type of object to allocate. This parameter should be set to one of the following values:

### `ControlFlag` *(in, AIL_INT64)*

Specifies the initialization state of the Aurora Imaging Library thread context, event, mutex, or the identifier of an existing Aurora Imaging Library event. This parameter should be set to one of the following values:

### `ThreadFctPtr` *(in, AIL_THREAD_FUNCTION_PTR)*

Specifies the address of the function that will be executed by the new thread ([`M_THREAD`](../../Reference/thr/MthrAlloc.md)). For all other objects, this parameter should be set to `M_NULL`.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of user data necessary to execute the **FunctionToCall** function of the new thread ([`M_THREAD`](../../Reference/thr/MthrAlloc.md)). For all other objects, this parameter should be set to `M_NULL`.

### `ThreadEventOrMutexIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the thread context, event or mutex identifier or specifies the data type that the function should use to return the context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated thread context, event, or mutex; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated thread context, event, or mutex; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_THR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the thread context, event, or mutex (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the event identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated event.

If allocation fails, [`M_NULL`](../../Reference/thr/MthrAlloc.md) is written as the identifier. |
| `Address in which to write the mutex identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated mutex.

If allocation fails, [`M_NULL`](../../Reference/thr/MthrAlloc.md) is written as the identifier. |
| `Address in which to write the thread context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated thread context.

If allocation fails, [`M_NULL`](../../Reference/thr/MthrAlloc.md) is written as the identifier. |

## Parameter Associations

### For specifying the type of object to allocate and its initiation state

---

### `M_EVENT`

Allocates a new Aurora Imaging Library synchronization event on the specified system.

| Value | Description |
| --- | --- |
| `M_NOT_SIGNALED` | Initializes the event as not signaled. |
| `M_SIGNALED` | Initializes the event as signaled. |

---

### `M_MUTEX`

Allocates an Aurora Imaging Library mutex on the specified system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default initialization state. This is the only setting available for threads and mutexes. For events, [`M_DEFAULT`](../../Reference/thr/MthrAlloc.md) is the same as [`M_NOT_SIGNALED`](../../Reference/thr/MthrAlloc.md) + [`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md). |

---

### `M_SELECTABLE_THREAD`

Allocates a selectable thread on the specified multi-threaded system. This allows you to synchronize the execution of functions on an on-board processor without creating a Host thread.  Use [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_THREAD_SELECT`](../../Reference/thr/MthrControl.md) to select the on-board thread to which to send the functions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default initialization state. This is the only setting available for threads and mutexes. For events, [`M_DEFAULT`](../../Reference/thr/MthrAlloc.md) is the same as [`M_NOT_SIGNALED`](../../Reference/thr/MthrAlloc.md) + [`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md). |
| `M_TRACE_LOG_DISABLE` | Specifies to disable the creation of trace logs on this thread by any external program, such as Aurora Imaging Profiler or the Aurora Imaging Configurator utility Interactive Troubleshooting.  A trace-disabled thread can still log trace information when explicitly enabled with Aurora Imaging Library code. To enable a trace log on a trace-disabled thread, use [`MappControl`](../../Reference/app/MappControl.md) with [`M_TRACE`](../../Reference/app/MappControl.md) + [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) set to [`M_LOG_ENABLE`](../../Reference/app/MappControl.md). The trace log will generate trace information starting from the point in the code that the function appears. You can disable generating further trace information in this thread with [`M_LOG_DISABLE`](../../Reference/app/MappControl.md). This creates a trace-enabled block of code in this thread, which is otherwise not generating any trace information.  Disabling trace logs with [`M_TRACE_LOG_DISABLE`](../../Reference/thr/MthrAlloc.md) helps to protect intellectual property because no external program will be able to generate a trace for this application. A trace-enabled block will protect sensitive intellectual property while still examining a specific section of code. |

---

### `M_THREAD`

Allocates a thread and associates it with an Aurora Imaging Library thread context. If the target processor is an on-board processor of a system that supports multi-threading, Aurora Imaging Library automatically creates, and eventually terminates, an on-board thread for each thread that sends commands to the board.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default initialization state. This is the only setting available for threads and mutexes. For events, [`M_DEFAULT`](../../Reference/thr/MthrAlloc.md) is the same as [`M_NOT_SIGNALED`](../../Reference/thr/MthrAlloc.md) + [`M_AUTO_RESET`](../../Reference/thr/MthrAlloc.md). |
| `M_TRACE_LOG_DISABLE` | Specifies to disable the creation of trace logs on this thread by any external program, such as Aurora Imaging Profiler or the Aurora Imaging Configurator utility Interactive Troubleshooting.  A trace-disabled thread can still log trace information when explicitly enabled with Aurora Imaging Library code. To enable a trace log on a trace-disabled thread, use [`MappControl`](../../Reference/app/MappControl.md) with [`M_TRACE`](../../Reference/app/MappControl.md) + [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) set to [`M_LOG_ENABLE`](../../Reference/app/MappControl.md). The trace log will generate trace information starting from the point in the code that the function appears. You can disable generating further trace information in this thread with [`M_LOG_DISABLE`](../../Reference/app/MappControl.md). This creates a trace-enabled block of code in this thread, which is otherwise not generating any trace information.  Disabling trace logs with [`M_TRACE_LOG_DISABLE`](../../Reference/thr/MthrAlloc.md) helps to protect intellectual property because no external program will be able to generate a trace for this application. A trace-enabled block will protect sensitive intellectual property while still examining a specific section of code. |

### Combination Constants — For

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set how the event is reset.

| Value | Description |
| --- | --- |
| `M_AUTO_RESET` | Specifies that the event is reset automatically.  The state of this type of event is automatically reset to [`M_NOT_SIGNALED`](../../Reference/thr/MthrAlloc.md) upon the return of a call to [`MthrWait`](../../Reference/thr/MthrWait.md) with [`M_EVENT_SYNCHRONIZE`](../../Reference/thr/MthrWait.md) or [`M_EVENT_WAIT`](../../Reference/thr/MthrWait.md).  Events that are reset automatically are useful in applications where only one thread waits on a specific event. |
| `M_MANUAL_RESET` | Specifies that the event is reset manually.  The state of this type of event remains unchanged until a call to [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_EVENT_SET`](../../Reference/thr/MthrControl.md) is issued.  Events that are reset manually are useful when multiple threads wait on a specific event. |

## Return Value

**Type:** `AIL_ID`

The returned value is the thread context, event, or mutex identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_THR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/thr/MthrAlloc.md) was specified).
