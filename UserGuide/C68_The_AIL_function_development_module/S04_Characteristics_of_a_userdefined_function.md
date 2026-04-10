---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Characteristics_of_a_userdefined_function
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Characteristics of a userdefined function"
---

# Characteristics of a user-defined Aurora Imaging Library function

Each user-defined Aurora Imaging Library function consists of a caller and a callee function. The caller function provides the user interface and sets up the Aurora Imaging Library function context for the new function. In the caller function, you must allocate the function context for the new function, register the parameters declared by the caller function as parameters of the user-defined Aurora Imaging Library function, call the callee function, and, in the end, free the allocated Aurora Imaging Library identifier of the function.

The callee function is a separate function that actually performs the required operations when the user-defined Aurora Imaging Library function is called. The callee function must be implemented as a separate function because it can be executed remotely by a callee node in a Distributed Aurora Imaging Library application. The only parameter that is passed to the callee function is the Aurora Imaging Library identifier of the function context. The callee function uses this identifier to access the Aurora Imaging Library function context and retrieve the parameters from the context using the [`MfuncParamValue`](../../Reference/func/MfuncParamValue.md) function. You can also make calls to other Aurora Imaging Library functions from the callee function.

*[Image: userdefined_function.png]*

> **Note:** Note that to increase efficiency, parameter checking is not performed for user-defined Aurora Imaging Library functions.

## Remote and local functions

When allocating your function context using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) in the caller function, you must specify whether your user-defined Aurora Imaging Library function will be executed on the local computer or the remote computer with [`M_LOCAL`](../../Reference/func/MfuncAlloc.md) or [`M_REMOTE`](../../Reference/func/MfuncAlloc.md).

User-defined Aurora Imaging Library functions allocated as [`M_LOCAL`](../../Reference/func/MfuncAlloc.md) will be executed by the Host processor, whereas user-defined Aurora Imaging Library functions allocated as [`M_REMOTE`](../../Reference/func/MfuncAlloc.md) will be executed by a remote processor, if one is available.

## Asynchronous and synchronous functions

When allocating your function context using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) in the caller function, you must specify whether your user-defined Aurora Imaging Library function will run asynchronously or synchronously ([`M_ASYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md) or [`M_SYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md)) with respect to the calling thread.

User-defined Aurora Imaging Library functions allocated to run asynchronously will return control to the calling thread immediately after being called. One advantage of running functions asynchronously is that they allow the caller function to immediately proceed to the next statement, while the callee processor executes the callee function. Note that an asynchronous function can modify Aurora Imaging Library buffers and parameters passed by reference, but cannot actually return a value. For more information on return values, see [Return values](S06_Parameter_registration_and_return_values.md).

> **Note:** Note that user-defined Aurora Imaging Library functions allocated to run on the Host processor must be run synchronously and user-defined Aurora Imaging Library functions allocated to run on the remote processor can be run asynchronously or synchronously.
