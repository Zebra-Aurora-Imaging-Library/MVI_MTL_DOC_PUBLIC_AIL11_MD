---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Delegates_and_hook_functions
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Delegates and hook functions"
---

# Delegates and Aurora Imaging Library hook-type functions

Delegates are objects that contain a reference to functions, and as such, they are similar to function pointers in C++. The AIL.NET wrapper declares a several delegate types, and each one for use with a specific Aurora Imaging Library hook-type function; Aurora Imaging Library hook-type functions are those Aurora Imaging Library functions that take a pointer to a user-defined (hook-handler or event-handler) function. When using an Aurora Imaging Library hook-type function in .NET, use a delegate instead of a pointer to a user-defined function. The following table lists the Aurora Imaging Library hook-type functions and their associated delegate type.

| Aurora Imaging Library hook-type function | Associated delegate type |
| --- | --- |
|  |
| [`MappHookFunction`](../../Reference/app/MappHookFunction.md) | _M_APP_HOOK_FUNCTION_PTR_ |
| [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) | _M_BUF_HOOK_FUNCTION_PTR_ |
| [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md), [`MdigProcess`](../../Reference/dig/MdigProcess.md) | _M_DIG_HOOK_FUNCTION_PTR_ |
| [`MdispHookFunction`](../../Reference/disp/MdispHookFunction.md) | _M_DISP_HOOK_FUNCTION_PTR_ |
| [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) | _MFUNCFCTPTR_ |
| [`MocrHookFunction`](../../Reference/ocr/MocrHookFunction.md) | _M_OCR_HOOK_FUNCTION_PTR_ |
| [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) | _M_SYS_HOOK_FUNCTION_PTR_ |
| [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) | _M_THREAD_FUNCTION_PTR_ |
| [`MfpgaHookFunction`](../../Reference/fpga/MfpgaHookFunction.md) | _M_FPGA_HOOK_FUNCTION_PTR_ |
| [`MdigFocus`](../../Reference/dig/MdigFocus.md) | _M_FOCUS_HOOK_FUNCTION_PTR_ |

To create a new instance of a delegate, you must pass a user-defined function, with the proper signature and return type, to the constructor of the delegate object. The following table outlines the proper signature and return type for each type of delegate.

| Delegate type | Return type | Parameter types |
| --- | --- | --- |
| _M_APP_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _ M_ID_, _IntPtr_ |
| _M_BUF_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _ M_ID_, _IntPtr_ |
| _M_DIG_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _ M_ID_, _IntPtr_ |
| _M_DISP_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _ M_ID_, _IntPtr_ |
| _MFUNCFCTPTR_ | void | _M_ID_ |
| _M_OCR_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _ string_, _IntPtr_ |
| _M_SYS_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _M_ID_, _IntPtr_ |
| _M_THREAD_FUNCTION_PTR_ | _M_INT_ | _IntPtr_ |
| _M_FPGA_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _M_ID_, _IntPtr_ |
| _M_FOCUS_HOOK_FUNCTION_PTR_ | _M_INT_ | _M_INT_, _ M_INT_, _IntPtr_ |

For more information on the requirements of the user-defined function, please refer to the description of the corresponding hook-type function.

## Lifetime of delegates passed to Aurora Imaging Library hook-type functions

Delegate objects passed to an Aurora Imaging Library hook-type function must stay alive as long as the user-defined function is hooked in Aurora Imaging Library. This is because Aurora Imaging Library needs to keep a pointer to the user-defined function to call it when the specified event occurs. Since out of scope delegates are subject to be collected by the common language runtime's (CLR's) garbage collector, you must ensure that the delegate will be in scope long enough; otherwise, it could result in an Null reference exception when the specified event occurs. Typically, delegates should be declared as members of the class that calls the Aurora Imaging Library hook-type function, as in the following example:

> **Code example:** [userguide.mil_in_net.delegate_optimal.cs](userguide.ail_in_net.delegate_optimal.cs)

The following example demonstrates an easy-to-make error; the delegate object _myDel_ is declared in the scope of the class constructor, as opposed to being declared as a member of the class that calls the Aurora Imaging Library hook-type function. When the constructor exits, _myDel_ goes out of scope and will eventually be reclaimed by the garbage collector. When this happens, any further attempts from Aurora Imaging Library to call the user-defined function will result in an exception.

> **Code example:** [userguide.mil_in_net.delegate_not_optimal.cs](userguide.ail_in_net.delegate_not_optimal.cs)

## Passing user data to a user-defined function

Some applications might require user data to be available to the user-defined (hook-handler) function. To pass this user data to the user-defined function, and to protect this user data from the CLR's garbage collector, you should encapsulate the user data in an object, and encapsulate this new user data object in a `GCHandle` object. Note that a pinned `GCHandle` object is not needed because Aurora Imaging Library does not directly reference the memory address of the user data object. The following example demonstrates how to pass user data to a user-defined function through the creation of a user data object encapsulated by a `GCHandle` object.

> **Code example:** [userguide.mil_in_net.delegate_userdata.cs](userguide.ail_in_net.delegate_userdata.cs)

It is not only important to inform the garbage collector not to move an object, it is also important to let the garbage collector know when it can move an object. This is done by releasing the `GCHandle` object with its `Free` function. This is shown in the `Unhook` function of the above example.
