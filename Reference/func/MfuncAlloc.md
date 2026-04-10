---
doctype: Reference
module: func
function: MfuncAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncAlloc"
---

# MfuncAlloc

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Allocate an Aurora Imaging Library function context for your user-defined function.

## Syntax

```c
AIL_ID MfuncAlloc(
    AIL_CONST_TEXT_PTR    FunctionName,           //in
    AIL_INT               ParameterNum,           //in
    AIL_FUNC_FUNCTION_PTR CalleeFunctionPtr,      //in
    AIL_CONST_TEXT_PTR    CalleeFunctionDLLName,  //in
    AIL_CONST_TEXT_PTR    CalleeFunctionName,     //in
    AIL_INT               UserFunctionOpcode,     //in
    AIL_INT64             InitFlag,               //in
    AIL_ID *              CBasedContextFuncIdPtr  //out
)
```

## Description

This function allows you to allocate an Aurora Imaging Library function context for the current user-defined function. Call [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) in the caller function of the user-defined function. [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) signals the creation of a user-defined Aurora Imaging Library function context, and should be the first Aurora Imaging Library function called in the caller function. Once the function context is allocated, your function is known as a user-defined Aurora Imaging Library function. A user-defined Aurora Imaging Library function is considered a standard Aurora Imaging Library function, respecting all Aurora Imaging Library environment controls, such as tracing and error handling.

When defining the function context, you must specify a unique opcode for the user-defined function. The opcode is used to locate the user-defined function.

Aurora Imaging Library user-defined functions can be grouped into user-defined modules or remain ungrouped. To specify whether a user-defined function is grouped or not, and if grouped, assign it to a specific user-defined module, pass an appropriate opcode to the [`UserFunctionOpcode`](../../Reference/func/MfuncAlloc.md) parameter. Specify the opcode as a combination of two values: an offset from 0 to 63 and a label either identifying the module in which to group the function ([`M_USER_MODULE_n`](../../Reference/func/MfuncAlloc.md), where n is a number between 1 and 7) or identifying that the function is ungrouped ([`M_USER_FUNCTION`](../../Reference/func/MfuncAlloc.md)). A user-defined module can have up to 64 functions, and there can be up to 64 ungrouped functions.

## Parameters

### `FunctionName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the user-defined function.

*For specifying the name of the user-defined function*

| Value | Description |
| --- | --- |
| `"FunctionName"` | Specifies the name of the user-defined function.

This is the same as the name of the current caller function. This is also the name that you will use to call the user-defined Aurora Imaging Library function in your application. The name of the function must be a null-terminated string. |

### `ParameterNum` *(in, AIL_INT)*

Specifies the number of parameters passed to the current user-defined function. Note that the number of parameters should not exceed 16.

### `CalleeFunctionPtr` *(in, AIL_FUNC_FUNCTION_PTR)*

Specifies the address of the callee function.

### `CalleeFunctionDLLName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the library file which contains the callee function's code. The library file must be visible to the system executing the callee function.

### `CalleeFunctionName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the callee function to export from the library file.

### `UserFunctionOpcode` *(in, AIL_INT)*

Specifies the opcode to use when an application executes the user-defined Aurora Imaging Library function. The opcode is used to locate the user-defined function and identifies which user-defined module, if any, the function belongs to.

*For specifying the label identifying that the function is grouped or ungrouped*

| Value | Description |
| --- | --- |
| `M_USER_FUNCTION` | Specifies that the user-defined function is an ungrouped user-defined function. |
| `M_USER_MODULE_n` | Specifies the module in which to group the user-defined function, where _n_ is a value between 1 and 7, inclusive. |

*For specifying the offset*

| Value | Description |
| --- | --- |
| `0 <= Value <= 63` | Specifies the offset of the user-defined function among the ungrouped functions or among the functions in a specified group.

When allocating multiple user-defined functions in a single user-defined module, you must set a different offset for each new function in the module. For example, [`M_USER_MODULE_3`](../../Reference/func/MfuncAlloc.md)+ 12.

To allocate a user-defined function that is not grouped in a user-defined module, use [`M_USER_FUNCTION`](../../Reference/func/MfuncAlloc.md) + the offset; you must set a different offset for each new ungrouped function. |

### `InitFlag` *(in, AIL_INT64)*

Specifies more information about the user-defined Aurora Imaging Library function.

*For specifying whether the callee function is executed asynchronously or synchronously*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value.

If the system on which the function context is allocated has a remote processor, the default is the same as [`M_ASYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md)+[`M_REMOTE`](../../Reference/func/MfuncAlloc.md), if the system does not have a remote processor, the default is the same as [`M_SYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md)+[`M_LOCAL`](../../Reference/func/MfuncAlloc.md). |
| `M_ASYNCHRONOUS_FUNCTION` | Specifies that the user-defined function will not wait for the callee function to finish executing before executing the next function (typically [`MfuncFree`](../../Reference/func/MfuncFree.md)) in the caller function. |
| `M_SYNCHRONOUS_FUNCTION` | Specifies that the user-defined function will wait for the callee function to finish executing before executing the next function in the caller function. |

*For specifying whether the callee function can be executed remotely*

| Value | Description |
| --- | --- |
| `M_LOCAL` | Specifies that the callee function must be executed by the Host processor. Note that Aurora Imaging Library functions called from the callee function will still be executed on their target system.

This value cannot be added to [`M_ASYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md). |
| `M_REMOTE` | Specifies that the callee function will be executed remotely, if possible. If you select [`M_REMOTE`](../../Reference/func/MfuncAlloc.md), but do not compile your callee function with a cross-compiler specific to the target processor, an error will be generated. For more information, see [Steps to create a user-defined Aurora Imaging Library function](../../UserGuide/C68_The_AIL_function_development_module/S02_Steps_to_create_a_userdefined_function.md). |

*For setting the type of user-defined Aurora Imaging Library function*

| Value | Description |
| --- | --- |
| `M_ALLOC` | Specifies that the user-defined function is an allocation function, used to allocate a user-defined Aurora Imaging Library object on a required system. |
| `M_FREE` | Specifies that the user-defined function frees an object allocated using a user-defined Aurora Imaging Library allocation function. Note that when this value is used, the user-defined Aurora Imaging Library function must accept the Aurora Imaging Library identifier of the object to free as its only parameter. |

### `CBasedContextFuncIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to store the Aurora Imaging Library identifier provided for the user-defined function. Since the [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) function also returns the Aurora Imaging Library identifier, you can set this parameter to `M_NULL`.

## Return Value

**Type:** `AIL_ID`

The returned value is the function context identifier if the allocation is successful. If allocation fails, `M_NULL` is returned.
