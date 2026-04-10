---
doctype: Reference
module: func
function: MfuncAllocScript
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / func / MfuncAllocScript"
---

# MfuncAllocScript

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

> Allocate an Aurora Imaging Library function context for your script-based user-defined Aurora Imaging Library function.

## Syntax

```c
AIL_ID MfuncAllocScript(
    AIL_CONST_TEXT_PTR FunctionName,                //in
    AIL_INT            ParameterNum,                //in
    AIL_CONST_TEXT_PTR InterpreterLanguage,         //in
    AIL_CONST_TEXT_PTR ScriptFileName,              //in
    AIL_CONST_TEXT_PTR ScriptFunctionName,          //in
    AIL_INT            ScriptFunctionOpcode,        //in
    AIL_INT64          InitFlag,                    //in
    AIL_ID *           ScriptBasedContextFuncIdPtr  //out
)
```

## Description

This function allows you to allocate an Aurora Imaging Library function context for the current script-based user-defined Aurora Imaging Library function. Being script-based, the function will, instead of calling a C function, run a function in a script. Call [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md) in the caller function of the user-defined function. [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md) signals the creation of a user-defined Aurora Imaging Library function, and should be the first Aurora Imaging Library function called in the caller function. It will also allocate an Aurora Imaging Library function context identifier for the current function. You should use this Aurora Imaging Library function context identifier as an argument for other [`Mfunc...`](../../Reference/func/MfuncCall.md) functions used during the user-defined function's execution. Once the function context is allocated, your function is known as a script-based user-defined Aurora Imaging Library function. A script-based user-defined Aurora Imaging Library function is considered a standard Aurora Imaging Library function, respecting all Aurora Imaging Library environment controls, such as tracing and error handling. The script that you provided as the callee-portion of the function is compiled/interpreted at runtime.

When defining the function context, you must specify a unique opcode for the script-based user-defined function. The opcode is used to locate the script-based user-defined function when an application calls the script-based user-defined function. You must also specify the language in which the script is written.

Script-based user-defined Aurora Imaging Library functions can be grouped into user-defined script modules or remain ungrouped. To specify whether a user-defined function is grouped or not, and if grouped, assign it to a specific user-defined script module, pass an appropriate opcode to the [`ScriptFunctionOpcode`](../../Reference/func/MfuncAllocScript.md) parameter. Specify the opcode as a combination of two values: an offset from 0 to 63 and a label either identifying the module in which to group the function ([`M_SCRIPT_MODULE...`](../../Reference/func/MfuncAllocScript.md)) or identifying that the function is ungrouped ([`M_SCRIPT_FUNCTION`](../../Reference/func/MfuncAllocScript.md)). A user-defined script module can have up to 64 functions, and there can be up to 64 ungrouped functions.

## Parameters

### `FunctionName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the script-based user-defined Aurora Imaging Library function.

*For specifying the name of the user-defined function*

| Value | Description |
| --- | --- |
| `"FunctionName"` | Specifies the name of the script-based user-defined Aurora Imaging Library function.

This is the same as the name of the current caller function. This is also the name that you will use to call the user-defined function in your application. The name of the function must be a null-terminated string. |

### `ParameterNum` *(in, AIL_INT)*

Specifies the number of parameters to be registered for the current user-defined function.

*For specifying the number of parameters*

| Value | Description |
| --- | --- |
| `0 <= Value <= 15` | Specifies the number of parameters that you register for the function using [`MfuncParam`](../../Reference/func/MfuncParam.md) (or one of the type-specific versions of this function such as [`MfuncParamAILInt`](../../Reference/MfuncParamAILInt.md)). This should correspond to the number of parameters passed to the current user-defined function. |

### `InterpreterLanguage` *(in, AIL_CONST_TEXT_PTR)*

Specifies the language and language version in which the script is written.

*For specifying the path to the language interpreter.*

| Value | Description |
| --- | --- |
| `M_INTERPRETER_C_PYTHON3X` | Specifies that the first successfully loaded 3.X version of CPython is used.

Aurora Imaging Library will attempt to load the most recent 3.X version of CPython among the supported versions.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_C_PYTHON36` | Specifies that a CPython 3.6 based script is used. This is only supported if CPython 3.6 is the default Python version for the Linux distribution where Aurora Imaging Library is installed.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_C_PYTHON37` | Specifies that a CPython 3.7 based script is used.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_C_PYTHON38` | Specifies that a CPython 3.8 based script is used. This is only supported if CPython 3.8 is the default Python version for the Linux distribution where Aurora Imaging Library is installed.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_C_PYTHON39` | Specifies that a CPython 3.9 based script is used.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_C_PYTHON310` | Specifies that a CPython 3.10 based script is used. This is only supported if CPython 3.10 is the default Python version for the Linux distribution where Aurora Imaging Library is installed.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_C_PYTHON311` | Specifies that a CPython 3.11 based script is used.

> **Note:** Note that the script must be written using this version of the language. |
| `M_INTERPRETER_CSHARP` | Specifies that C# based code is used.

> **Note:** Note that .NET Core is not supported by the Aurora Imaging Library Interpreter in a .NET program. |

### `ScriptFileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file containing the user-defined script. The script file must be accessible to the computer executing the script.

### `ScriptFunctionName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the function to export from the script file.

### `ScriptFunctionOpcode` *(in, AIL_INT)*

Specifies the opcode to use to locate the script-based user-defined Aurora Imaging Library function when an application calls the function. The opcode also serves to identify the module in which to group the function, if any.

*For setting the label identifying that the function is grouped or ungrouped*

| Value | Description |
| --- | --- |
| `M_SCRIPT_FUNCTION` | Specifies that the script-based user-defined Aurora Imaging Library function is an ungrouped user-defined Aurora Imaging Library function. |
| `M_SCRIPT_MODULE_1` | Specifies that the script-based user-defined Aurora Imaging Library function will be grouped in the first module. |
| `M_SCRIPT_MODULE_2` | Specifies that the script-based user-defined Aurora Imaging Library function will be grouped in the second module. |

*For setting the offset*

| Value | Description |
| --- | --- |
| `0 <= Value <= 63` | Sets the offset of the script-based user-defined Aurora Imaging Library function among the ungrouped user-defined Aurora Imaging Library functions or among those in the specified group.

When allocating multiple user-defined Aurora Imaging Library functions in a single user-defined module, you must set a different offset for each new user-defined Aurora Imaging Library function in the module (for example, [`M_SCRIPT_MODULE_1`](../../Reference/func/MfuncAllocScript.md)+ 12).

To allocate a script-based user-defined Aurora Imaging Library function that is not grouped in a user-defined module, use [`M_SCRIPT_FUNCTION`](../../Reference/func/MfuncAllocScript.md) + the offset; you must set a different offset for each new ungrouped user-defined Aurora Imaging Library function. |

### `InitFlag` *(in, AIL_INT64)*

Specifies more information about the script-based user-defined Aurora Imaging Library function.

*For specifying whether the script-based user-defined Aurora Imaging Library function is executed asynchronously or synchronously*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value.

If the system on which the function context is allocated has a remote processor, the default is the same as [`M_ASYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAllocScript.md)+[`M_REMOTE`](../../Reference/func/MfuncAllocScript.md), if the system does not have a remote processor, the default is the same as [`M_SYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAllocScript.md)+[`M_LOCAL`](../../Reference/func/MfuncAllocScript.md). |
| `M_ASYNCHRONOUS_FUNCTION` | Specifies that the script-based user-defined Aurora Imaging Library function will not wait for the callee script/function to finish executing before executing the next function (typically [`MfuncFree`](../../Reference/func/MfuncFree.md)) in the caller function. |
| `M_SYNCHRONOUS_FUNCTION` | Specifies that the script-based user-defined Aurora Imaging Library function will wait for the callee script/function to finish executing before executing the next function in the caller function. |

*For specifying whether the script-based user-defined Aurora Imaging Library function can be executed remotely*

| Value | Description |
| --- | --- |
| `M_LOCAL` | Specifies that the script-based user-defined Aurora Imaging Library function must be executed by the Host processor. Note that Aurora Imaging Library functions called from the script-based user-defined Aurora Imaging Library function will still be executed on their target system.

This value cannot be added to [`M_ASYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAllocScript.md). |
| `M_REMOTE` | Specifies that the script-based user-defined Aurora Imaging Library function will be executed remotely, if possible. For more information, see [Steps to create a user-defined Aurora Imaging Library function](../../UserGuide/C68_The_AIL_function_development_module/S02_Steps_to_create_a_userdefined_function.md). |

*For setting the type of script-based user-defined Aurora Imaging Library function*

| Value | Description |
| --- | --- |
| `M_ALLOC` | Specifies that the script-based user-defined Aurora Imaging Library function is an allocation function, used to allocate a user-defined Aurora Imaging Library object on a required system. |
| `M_FREE` | Specifies that the script-based user-defined Aurora Imaging Library function frees an object allocated using a user-defined Aurora Imaging Library allocation function. Note that when this value is used, the user-defined Aurora Imaging Library function must accept the Aurora Imaging Library identifier of the object to free as its only parameter. |

### `ScriptBasedContextFuncIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to store the Aurora Imaging Library identifier of the allocated Aurora Imaging Library function context. Since the [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md) function also returns the Aurora Imaging Library identifier, you can set this parameter to `M_NULL`.

## Return Value

**Type:** `AIL_ID`

The returned value is the Aurora Imaging Library function context identifier if the allocation is successful. If allocation fails, `M_NULL` is returned.
