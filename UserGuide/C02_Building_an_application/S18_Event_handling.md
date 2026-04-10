---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Event_handling
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Event handling"
---

# Event handling in Aurora Imaging Library with hook-handler functions

In Aurora Imaging Library, there are functions that take a callback function and set it up as the event handler for a specific type of event (hooks it to the event). When the event then happens, the callback function is executed. The Aurora Imaging Library functions capable of attaching (hooking) a callback function to an event are referred to as hook-type functions (for example, the [`MappHookFunction`](../../Reference/app/MappHookFunction.md) function), and the callback functions are referred to as hook-handler functions.

## Aurora Imaging Library hook-type functions

Aurora Imaging Library hook-type functions allow you to execute a user-defined hook-handler function upon the occurrence of a specified event. These hook-type functions can associate a variety of events with specified hook-handler functions. The supported types of events differ based on the module and the particular hook-type function. For instance, the [`MappHookFunction`](../../Reference/app/MappHookFunction.md) function allows you to, among other things, trap errors ([`M_ERROR_...`](../../Reference/app/MappHookFunction.md)) that occur in an application and execute a hook-handler function whenever one of these errors occur; whereas, the [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) allows you to execute a hook-handler function, for example, at the end of a grab.

Within a hook-handler function, you can retrieve information about the event that caused the hook-handler function to be called, using a hook-information ([`M...GetHookInfo`](../../Reference/dig/MdigGetHookInfo.md)) function.

The various Aurora Imaging Library hook-type functions and their corresponding hook-information functions typically conform to the following naming scheme:

- [`M...HookFunction`](../../Reference/buf/MbufHookFunction.md).
- [`M...GetHookInfo`](../../Reference/dig/MdigGetHookInfo.md).

There are, however, a few hook-type functions that do not conform to this standard (for example, [`MdigFocus`](../../Reference/dig/MdigFocus.md) and [`MdigProcess`](../../Reference/dig/MdigProcess.md)).

Depending on the hook-type function, you can attach multiple hook-handler functions to an event. To do so, call the hook-type function multiple times, once for each hook-handler function that you want to hook to the event. Aurora Imaging Library automatically chains and keeps an internal list of all the associated hook-handling functions. When a function is hooked, this new function is added to the end of the list. The hook-handler functions will be executed in the order in which they were attached to the event. Note that [`MdigFocus`](../../Reference/dig/MdigFocus.md) and [`MdigProcess`](../../Reference/dig/MdigProcess.md) cannot attach multiple hook-handler functions.

Each Aurora Imaging Library hook-type function requires that the hook-handler function that you specify has a particular signature (or prototype); it must have the expected parameters and a particular return type. If the hook-handler function does not conform to the signature that the hook-type function expects, your application won't successfully compile. Refer to the hook-type function's reference for a description of the particular signature required.

Hook-handler functions are typically executed asynchronously on a separate thread when the event deals with hardware (for example, [`Mdig...`](../../Reference/dig/MdigHookFunction.md) and [`Msys...`](../../Reference/sys/MsysHookFunction.md) events), or a remote computer. If the event is software related, the hook-handler function will generally be executed synchronously on the same thread as the one on which the event was triggered. The exception to this is the [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) function, which will have its hook-handler function executed on a separate thread if the buffer causing the event ([`M_MODIFIED_BUFFER`](../../Reference/buf/MbufHookFunction.md)) is modified by a function that executes asynchronously (for example, [`MimArith`](../../Reference/im/MimArith.md) executed on a processing FPGA); otherwise, the hook-handler function is executed on the same thread as the one on which the event was triggered.

*[Image: Hook-Function_box_diagram.png]*

Note that although there is a small queue to permit a certain amount of overlap, hook-handler functions executed asynchronously should limit their execution length such that the function is executed faster than the time it takes to generate another instance of the associated event. As a general guideline, hook-handler functions should have a shorter execution period than the average period required to generate an event. Typically, the hook-handler function performs the minimum number of operations required, and if necessary, performs longer processes by launching a utility function in another thread. This is more important when dealing with hardware-related event handling but should be held as a general rule for all event handling.

### Events and user data

Since hook-handler functions must have a very specific signature (prototype), and you might need information from your Aurora Imaging Library application within your hook-handler function, all hook-handler functions have a parameter which takes a pointer to a variable or data structure. You can use the reference to access and/or modify the data from within the hook-handler function, upon its execution. This also allows the main application block to receive user-data from the hook-handler functions.

The reference to the variable/user data structure is received in the hook-handler function as a void pointer. As such, to access the data in the hook-handler function, the pointer to the data must be cast back to the appropriate data type. If you need to pass the hook-handler function more than one piece of data, define a structure or class that can store the set of required data. For example, you can use a data structure to pass the identifier of both the source and destination image buffers on which you want the hook function to operate.

> **Code example:** [userguide.building_an_application.event_handling_in_mil01](userguide.building_an_application.event_handling_in_ail01)

Since you are passing the user data with a pointer, the user data is accessible and modifiable in both the primary application block (in which the hook-type function was called), and in the hook-handler function. Note that if the hook-handler function is executed asynchronously, care must be taken to properly synchronize the primary application block and hook-handler functions so that the data is not being modified by both functions simultaneously. For more information regarding synchronization, see [](../C66_Using_AIL_with_multiprocessing_and_under_multithread_systems/S03_Multithreading.md).

### Unhooking hook-handler functions

If the hook-handler function was hooked using a [`M...HookFunction`](../../Reference/buf/MbufHookFunction.md) function, you can unhook the hook-handler function from its associated event. To do so, you must call the appropriate [`M...HookFunction`](../../Reference/buf/MbufHookFunction.md) with the same parameter values that were passed in the previous call, but you must combine the hook-type (event) with [`M_UNHOOK`](../../Reference/buf/MbufHookFunction.md) so that the event is detached (unhooked) from the hook-handler function as shown below:

> **Code example:** [userguide.building_an_application.event_handling_in_mil02](userguide.building_an_application.event_handling_in_ail02)

## Aurora Imaging Library hook information functions

The hook information functions ([`M...GetHookInfo`](../../Reference/dig/MdigGetHookInfo.md)) allow you to retrieve information about the event that caused the hook-handler function to be called. You can use hook information functions, for example, to determine the identifier of the buffer that triggered the event ([`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/buf/MbufGetHookInfo.md) + [`M_BUFFER_ID`](../../Reference/buf/MbufGetHookInfo.md)) so that you can perform some operation on that buffer, or to determine if a grabbed frame is corrupt ([`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_CORRUPTED_FRAME`](../../Reference/dig/MdigGetHookInfo.md)). The [`M...GetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) types of functions can only be called within the scope of a hook-handler function. Each hook information function can return information related to the event to which the hook-handler function was hooked; although, some types of information are available regardless of the event triggering the hook-handler function.

Some events occur under multiple circumstances. For instance, if you hook a function to an I/O change event using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_IO_CHANGE`](../../Reference/sys/MsysHookFunction.md), the hook-handler function will be invoked when any I/O signal changes state. To determine the particular I/O signal that caused the event, you can call [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) using [`M_IO_INTERRUPT_SOURCE`](../../Reference/sys/MsysGetHookInfo.md). Based on the result, you can implement a switch statement to execute different code based on the particular I/O signal that caused the event. Another example of an event occurring under multiple circumstances is an [`M_GRAPHIC_MODIFIED`](../../Reference/gra/MgraHookFunction.md) type of event. This type of event is triggered whenever any graphic is modified in a 2D graphics list ([`GraListId`](../../Reference/gra/MgraHookFunction.md)). You could determine the modification performed on the graphic using [`MgraGetHookInfo`](../../Reference/gra/MgraGetHookInfo.md) with [`M_GRAPHIC_CONTROL_TYPE`](../../Reference/gra/MgraGetHookInfo.md), and implement a switch statement to execute different code based on the particular modification made to the 2D graphics list.

The following code snippet is an example of a function hooked to an [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md) event using [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md). The hook-handler function receives user data and makes use of a hook information function:

> **Code example:** [userguide.building_an_application.event_handling_in_mil03](userguide.building_an_application.event_handling_in_ail03)

Since the hook-handler function receives the reference to the user-data as a void pointer, the pointer is cast back to the appropriate data type (in this case, the _HookDataStruct_ structure defined earlier in this section) so that the structure's various elements can be accessed. To identify the buffer that was modified (written into by the grab), causing the hook-handler function to be executed, a call to [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/dig/MdigGetHookInfo.md) + [`M_BUFFER_ID`](../../Reference/dig/MdigGetHookInfo.md) is made. The hook-handler function then performs an operation on the buffer and stores the results in the image buffer specified by the user-defined structure. Since the main application block also has access to the buffer, the results of the operation are accessible in the main application block.
