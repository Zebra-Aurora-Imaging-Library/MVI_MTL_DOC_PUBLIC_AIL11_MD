---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Steps_to_create_a_userdefined_function
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Steps to create a userdefined function"
---

# Steps to create a user-defined Aurora Imaging Library function

The following steps provide a basic methodology for using the Aurora Imaging Library Function Development module to create a C-based user-defined function:

1. Create a caller function. The name that you will use to call the user-defined Aurora Imaging Library function from an application is the same as the name of the caller function.
2. Allocate a function context for the new function using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md). The [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) function should be the first Aurora Imaging Library function called in the caller function. The allocated function context provides an Aurora Imaging Library identifier for your user-defined Aurora Imaging Library function, and allows this function to be treated as a regular Aurora Imaging Library function (for example, error checking and tracing will be performed).
3. Register the parameters of the caller function in the function context using [`MfuncParam`](../../Reference/func/MfuncParam.md) with the appropriate data type or using one of the type-specific versions of this function (for example, [`MfuncParamAILInt`](../../Reference/MfuncParamAILInt.md)). Registering the parameters gives the callee function access to the parameter values.
4. Call the callee function from the caller function, using [`MfuncCall`](../../Reference/func/MfuncCall.md). The callee function must be created as a separate function. Note that you must call [`MfuncCall`](../../Reference/func/MfuncCall.md) from the same thread as [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md).
5. Free the function context. At the end of the caller function, the created function context must be freed, using the [`MfuncFree`](../../Reference/func/MfuncFree.md) function. This should be the last Aurora Imaging Library function called in the caller function. Note that you must call [`MfuncFree`](../../Reference/func/MfuncFree.md) from the same thread as [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) and [`MfuncCall`](../../Reference/func/MfuncCall.md).
6. Create the callee function. This function must accept the Aurora Imaging Library identifier of the function context as its only parameter. See the description of [`MfuncCall`](../../Reference/func/MfuncCall.md) for a prototype of a callee function. In the callee function, you must retrieve the values of the parameters registered in the caller function, using [`MfuncParamValue`](../../Reference/func/MfuncParamValue.md), and develop the code that will produce the required results. You can make calls to other Aurora Imaging Library functions from the callee function.

Once you have performed the steps listed above, the user-defined Aurora Imaging Library function is created, and you can use it in an application.

The steps required to create a script-based user-defined function are similar to creating a C-based user-defined function. So with the exception of one section, the remainder of this chapter will discuss creating a C-based Aurora Imaging Library user-defined function. Then, the differences between the two sets of steps are examined in [Script-based user-defined Aurora Imaging Library function](S09_Scriptbased_userdefined_function.md).

The following example shows how to create a C-based user-defined Aurora Imaging Library function.

> **Code example:** [MFunc.cpp](MFunc.cpp)
