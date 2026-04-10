---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_multiprocessing_and_under_multithread_systems
section: Multithreading
module_tag: thr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / multiprocessing / Multithreading"
---

# Multi-threading

Aurora Imaging Library also supports multi-threading. Multi-threading is the ability to perform multiple operations simultaneously in the same process. This is done by creating different threads (execution queues) to ensure sequential execution of operations within the same thread, while allowing simultaneous yet independent execution of operations in other threads.

Threads within a process share the same data. Individual threads can communicate with each other and exchange data such as Aurora Imaging Library identifiers.

Multi-threading is most appropriate for applications where independent tasks can be done simultaneously but need to share data or to be controlled and synchronized within a main task.

Multi-threading does not always result in an increase of speed and efficiency. Threads running simultaneously on the same CPU share the same resources (such as memory). When using a machine with multiple CPUs under Windows, the threads generally run on separate CPUs and provide more processing power. However, since they share the same memory, operations that are I/O intensive and require only simple processing might not be accelerated.

For better performance, it is recommended to limit the interaction between multiple threads (for example, the synchronization between threads, and any shared resources). Furthermore, in a multi-threaded application, such as a multi-camera application, you will get better performance if the total number of threads, including the multi-processing threads, is equal to, or less than the number of cores. In such an application, you should either minimize core interaction or disable multi-core processing altogether. This can be done by using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) and limiting [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) for each thread, so that the sum of the value of [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) for all the threads is less than or equal to the number of cores on your computer. Alternatively, you can use [`MappControlMp`](../../Reference/app/MappControlMp.md) and disable multi-core use altogether, by setting [`M_MP_USE`](../../Reference/app/MappControlMp.md) to [`M_DISABLE`](../../Reference/app/MappControlMp.md).

Most applications do not require the use of multiple threads since there are other ways of multi-tasking. Mechanisms such as asynchronous grab and call-back functions can be used (see [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md)). Applications resolved by alternative means are often simpler to implement and easier to maintain than multi-threaded applications.

## Aurora Imaging Library and multi-threading

When your application contains several independent processing tasks that can be performed in parallel, you can design it so that each part is controlled by a separate thread (or task).

### Creating threads using Aurora Imaging Library

Under multi-thread operating systems, you can create as many threads as you require. You can create threads using commands provided by the operating system, or using the [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) function provided by Aurora Imaging Library. The [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) function is portable, which means that it can be called from user-defined Aurora Imaging Library functions that are executed on systems with multiple processors. For information on executing functions on systems with on-board processors, see [Caller/callee dynamics on a remote system](../C68_The_AIL_function_development_module/S07_Controller_worker_dynamics_on_a_remote_system.md).

There are two methods for creating threads using the [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) function:

- With the first method ([`M_THREAD`](../../Reference/thr/MthrAlloc.md)), the [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) function creates an Aurora Imaging Library thread context for the new thread, and allows you to specify a pointer to a function that will be executed by the thread. This user-created function must include all operations, and call all of the functions that you consider as being part of one thread. When a thread contains a function call whose target processor is an on-board processor that supports multi-threading, Aurora Imaging Library automatically creates a corresponding thread on that system's on-board processor. The functions of the Thread module allow you to synchronize threads running on the Host and/or various Aurora Imaging Library systems.
  *[Image: Threads_2.png]*
- With the second method, [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) creates a selectable thread ([`M_SELECTABLE_THREAD`](../../Reference/thr/MthrAlloc.md)). Selectable threads are threads executed on an on-board processor that supports multi-threading and can be controlled from a single corresponding thread on the Host. Use [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_THREAD_SELECT`](../../Reference/thr/MthrControl.md) to send Aurora Imaging Library functions to be executed by a selectable thread.
  *[Image: Threads_1.png]*

Every thread in an Aurora Imaging Library application, including the main thread which initially called [`MappAlloc`](../../Reference/app/MappAlloc.md), shares the same application context settings. Calling [`MappControl`](../../Reference/app/MappControl.md) in any thread will affect the application context settings for every thread, unless [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) is added to the [`ControlType`](../../Reference/app/MappControl.md). When [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) is added to the [`ControlType`](../../Reference/app/MappControl.md), the specified application context setting applies only to the thread in which the call was made.

When a thread changes an application context setting using [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md), only the specified [`ControlType`](../../Reference/app/MappControl.md) in the thread will be unique, and it will remain unique. For example, in an Aurora Imaging Library application with three threads (Thread A, Thread B, and the main thread), all threads share the same application context settings, by default. The following calls to [`MappAlloc`](../../Reference/app/MappAlloc.md) demonstrate the shared and unique application context settings of this example.

1. If Thread A calls [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md)+[`M_THREAD_CURRENT`](../../Reference/app/MappControl.md), then Thread A will have the new error setting, while Thread B and the main thread will still share the original default error setting.
2. After that call, if either Thread B or the main thread calls [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md), but without adding [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md), Thread B and the main thread will change their error setting, while Thread A will remain with its unique setting.
3. Finally, if after the first two calls any thread calls [`MappControl`](../../Reference/app/MappControl.md) with [`M_PARAMETER`](../../Reference/app/MappControl.md), but without [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md), all three threads will share the new setting.

While application context settings are generally shared between all threads (except when made unique with [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md)), thread context settings, controlled using [`MthrControl`](../../Reference/thr/MthrControl.md), are unique to each thread.

### Thread execution

Aurora Imaging Library functions in any thread are executed as follows:

- If the target processor is the Host CPU, processing in each thread is determined by the operating system.
- If the target processor is an on-board processor of a system that supports multi-threading, Aurora Imaging Library automatically creates, and eventually terminates, an on-board thread for each thread that sends commands to the board.

Since the creation of on-board threads is done automatically, you do not have to specify the system on which to create a specific thread. However, you can do so by creating selectable threads on a particular system.

### Synchronization and mutex

Thread synchronization is generally done using the Host synchronization services (such as Windows event objects). However, when using a system with an on-board processor, the activities of this processor cannot be synchronized by the Host.

This means that threads continue execution without waiting for the execution of the on-board functions to complete. In most cases, this behavior is acceptable, since it leaves the Host available for other tasks. However, for operations that require sequential execution of functions to return valid results (for example, [`MbufGet`](../../Reference/buf/MbufGet.md) after an [`MdigGrab`](../../Reference/dig/MdigGrab.md)), Aurora Imaging Library automatically synchronizes the threads, forcing the Host to wait for completion of the earlier function(s).

Explicit synchronization of threads is necessary if functions sharing a common resource might conflict with each other. For example, if two threads sharing the same image buffer are not synchronized, and each thread tries to clear the buffer to a different value, these functions might execute at the same time and the buffer could be cleared to either value or even to a combination of both. Use the synchronization features of the [`MthrControl`](../../Reference/thr/MthrControl.md), [`MthrWait`](../../Reference/thr/MthrWait.md), and [`MthrWaitMultiple`](../../Reference/thr/MthrWaitMultiple.md) functions to synchronize the flow of threads. [`MthrWait`](../../Reference/thr/MthrWait.md) forces the current thread to wait for the completion of the specified thread or the change of state of the specified Aurora Imaging Library event. [`MthrWaitMultiple`](../../Reference/thr/MthrWaitMultiple.md) forces the current thread to wait for a change in state in one or all of the Aurora Imaging Library events identified in a user-supplied array of events.

[`MthrControl`](../../Reference/thr/MthrControl.md) allows you to use mutexes to synchronize the flow of threads. A mutual exclusion object allows threads to synchronize access to shared resources. Once a mutex is allocated on the specified system, you can lock and unlock critical sections of code. Locking a critical section of code ensures that no two threads can access the same data at the same time. To lock an Aurora Imaging Library mutex, you must call [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_LOCK`](../../Reference/thr/MthrControl.md) or [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md) immediately preceding the section of code to lock. If you lock a mutex, you must unlock it at the end of the critical section of code using [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_UNLOCK`](../../Reference/thr/MthrControl.md). Once you are finished using the mutex, free it using [`MthrFree`](../../Reference/thr/MthrFree.md). The use of a mutex is illustrated in the following example with MimArith and MimFlip accessing the same data.

*[Image: mutex.png]*

[`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_LOCK`](../../Reference/thr/MthrControl.md) and [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md) are very similar; the difference occurs when one attempts to lock a mutex that is already locked. [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_LOCK`](../../Reference/thr/MthrControl.md) will block the thread, forcing it to wait for the mutex to become unlocked before executing the critical section of code. [`MthrControl`](../../Reference/thr/MthrControl.md) with [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md) will not wait for the mutex to unlock. If the mutex is currently locked, the thread will proceed to execute, skipping the critical section of code. Note that once you call [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md), you should immediately call [`MthrInquire`](../../Reference/thr/MthrInquire.md) with [`M_LOCK_TRY`](../../Reference/thr/MthrControl.md) to inquire whether the mutex was locked.

### Thread control

Windows operating systems are multi-process and multi-thread operating systems. They provide various thread control services, including events (used to synchronize threads).

The [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) function serves as a link between Aurora Imaging Library and the operating system; the function allows you to create an operating-system-independent Aurora Imaging Library version of these services. Threads and events created using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) can be used in addition to, or instead of, the events created using commands of the operating system.

[`MthrControl`](../../Reference/thr/MthrControl.md) controls and coordinates both threads and events. The [`MthrWait`](../../Reference/thr/MthrWait.md) function synchronizes thread processing by forcing a "wait" state. The [`MthrInquire`](../../Reference/thr/MthrInquire.md) function inquires about both the settings of an Aurora Imaging Library thread context and the state of an event allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md). [`MthrFree`](../../Reference/thr/MthrFree.md) frees the allocated Aurora Imaging Library thread context or event.

The [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) function allows you to specify a particular system on which to allocate Aurora Imaging Library thread contexts or events. This permits you to synchronize the execution of functions on a specific Aurora Imaging Library system.

When creating multiple threads on a multi-core computer, use [`MappControlMp`](../../Reference/app/MappControlMp.md) and [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) to restrict the number of cores used by each thread. Restricting the number of cores used by each thread might be more efficient; see [Transparent multi-core use](S02_Transparent_MultiCore_Use.md) for more information.

### Using image processing and analysis contexts in multiple threads

Multiple threads cannot share an image processing or analysis context, but you can duplicate the context for use across more than one thread. For all the image processing and analysis modules, use their [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) function to duplicate their context. Using [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) is a faster and more convenient way of duplicating contexts than with [`M...Save`](../../Reference/3dmap/M3dmapSave.md) and [`M...Restore`](../../Reference/3dmap/M3dmapRestore.md). The basic steps to duplicate a context are:

1. Call [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) with [`M_INQUIRE_SIZE_BYTE`](../../Reference/3dmap/M3dmapStream.md) to return the number of bytes needed to store the context.
2. Allocate some memory of the same size as was established in step 1.
3. Call [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) with [`M_SAVE`](../../Reference/3dmap/M3dmapStream.md) and [`M_MEMORY`](../../Reference/3dmap/M3dmapStream.md) to save the context to the allocated memory.
4. Call [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) with [`M_LOAD`](../../Reference/3dmap/M3dmapStream.md) or [`M_RESTORE`](../../Reference/3dmap/M3dmapStream.md) to create a duplicate context.

At the end of each application, all the duplicated contexts, including the original context, must be freed using [`M...Free`](../../Reference/3dmap/M3dmapFree.md).

### Thread-safety in Aurora Imaging Library

To avoid race conditions, it is important to know the guarantees that Aurora Imaging Library provides with regards to thread-safety. There are multiple approaches to thread-safety and Aurora Imaging Library implements a few of these mechanisms through support for re-entrancy and the mutual exclusion ([`M_MUTEX`](../../Reference/thr/MthrAlloc.md)) mechanism.

Re-entrancy is an approach to thread-safety which focuses on avoiding shared state conditions. With regards to Aurora Imaging Library, it means that an Aurora Imaging Library function can be safely called concurrently from multiple threads, as long as the data passed to the function's parameters (for example, image buffers) is not shared between threads. Unless otherwise specified, all Aurora Imaging Library functions are considered to be re-entrant.

You can use the second approach to thread-safety which deals with synchronization in situations where shared states cannot be avoided. The mutual exclusion mechanism allows you to serialize access to shared data such as an Aurora Imaging Library object. This means that only one thread can read from and/or write to the shared data at a time. The mutual exclusion mechanism is implemented with a platform independent mechanism using the [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object. In Aurora Imaging Library, unless otherwise stated, functions that access or modify an Aurora Imaging Library object cannot be called concurrently on the same object in multiple threads without using the mutual exclusion mechanism ([`M_MUTEX`](../../Reference/thr/MthrAlloc.md)). For more information, see [](S03_Multithreading.md).

### Error reporting

To check for errors, use the [`MappGetError`](../../Reference/app/MappGetError.md) function. In multi-thread environments, a call to [`MappGetError`](../../Reference/app/MappGetError.md) returns the last error that occurred in the current thread.

Some functions in Aurora Imaging Library are asynchronous, that is, they queue their command to the hardware and then immediately return control to the Host. Errors are logged once the function is executed on the processor of the specified system, and are reported the next time an Aurora Imaging Library function is executed on the same system. By default, [`MappGetError`](../../Reference/app/MappGetError.md)is asynchronous; it will therefore not report the most recent error in this situation. you can specify that [`MappGetError`](../../Reference/app/MappGetError.md) should wait for all pending function calls to complete before returning by specifying [`M_SYNCHRONOUS`](../../Reference/app/MappGetError.md).
