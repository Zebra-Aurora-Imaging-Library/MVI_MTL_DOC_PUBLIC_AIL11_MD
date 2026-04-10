---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Scriptbased_userdefined_function
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Scriptbased userdefined function"
---

# Script-based user-defined Aurora Imaging Library function

Besides C-based user-defined functions, the Aurora Imaging Library Function Development module also allows you to create user-defined Aurora Imaging Library functions based on functions/scripts that require an interpreter (for example, written in Python) or the .NET framework (for example, written in C#) to be present at runtime. These are referred to as script-based user-defined functions.

*[Image: script_userdefined_function.png]*

Script-based and C-based user-defined Aurora Imaging Library functions have many similarities with regards to their implementation and manner of use. The following table gives a brief overview of the differences encountered:

|   |   |   |
| --- | --- | --- |
| Feature | C-based user-defined functions | Script-based user-defined functions |
| Parameter limitation | 16 parameters | 15 parameters |
| Compilation step | Compiled when the Aurora Imaging Library application is compiled | Compiled at runtime |
| Function grouping within Aurora Imaging Library | 7 modules 64 functions per module | 2 modules 64 functions per module |
| Supported languages | C, C++ | C#, Python 3.x (supported versions only) |
| Modifiable during runtime | No | Yes |

Much like the C-based user-defined functions, each script-based user-defined Aurora Imaging Library function has a caller and callee component to it. However, unlike a C-based user-defined function, you must allocate an Aurora Imaging Library function context using [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md) instead of [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md). The callee portion of the script-based user-defined Aurora Imaging Library function is the script you provide. Similar to C-based user-defined functions, you must register the parameters using [`MfuncParam`](../../Reference/func/MfuncParam.md). By passing the function context identifier to [`MfuncCall`](../../Reference/func/MfuncCall.md), you will execute the function specified in the call to [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md).

The script/function can be written in any of the languages (and language versions) listed below. Note that Aurora Imaging Library includes the interpreter interface DLLs for these languages (and language versions).

- CPython 3.x (supported versions only).
- C#.

If you are using a language (or language version) for which an interpreter interface DLL is not provided, you will be required to develop your own interpreter interface DLL from the provided code, along with ensuring the presence of an appropriate Aurora Imaging Library wrapper to unpack parameters in the callee script/function. This wrapper is required to associate objects in that language with Aurora Imaging Library, and make other Aurora Imaging Library calls such as unpacking your parameters.

Notable differences between the two types of user-defined functions are that you can register a maximum of 15 parameters for script-based user-defined functions, whereas you can register up to 16 parameters for C-based user-defined functions. Furthermore, you can organize your script-based user-defined functions into only two modules ([`M_SCRIPT_MODULE...`](../../Reference/func/MfuncAllocScript.md)); whereas, you can organize your C-based user-defined functions into 7 modules. However, like C-based user-defined Aurora Imaging Library functions, you can store up to 64 functions per module.

If your script/function requires other DLLs to run, such as C# dependencies, use the [`M_ADD_SCRIPT_REFERENCE`](../../Reference/func/MfuncControl.md) control type to specify a path to the file name of the DLL(s) that you need to include. Furthermore, using the [`M_COMPILE`](../../Reference/func/MfuncControl.md) control type, you can set when to recompile  .NET languages (into byte-code); this allows you to modify the script's code without having to stop the entire Aurora Imaging Library application. You can set whether your script should be compiled once ([`M_ONCE`](../../Reference/func/MfuncControl.md)) or if the script should be recompiled whenever the file on disk has been changed ([`M_MODIFIED`](../../Reference/func/MfuncControl.md)).

For languages within the .NET framework, it is possible to specify whether debug information should be generated, using the [`M_DEBUG_INFORMATION`](../../Reference/func/MfuncControl.md) control type. If you choose to have debug information generated, you need to set a path to the directory where the compiled version of the script (assembly file) will be created when the script/function is run. You must pass this path to the [`M_DEBUG_INFORMATION_PATH`](../../Reference/func/MfuncControl.md) control type.

> **Note:** Note that .NET Core is not supported by the Aurora Imaging Library Interpreter in a .NET program.

Much like with [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md), Aurora Imaging Library function contexts allocated using [`MfuncAllocScript`](../../Reference/func/MfuncAllocScript.md) can be set to have the callee function execute either locally or remotely. If the function can be executed remotely, it is important to include a copy of the DLL(s) and script file(s) on the remote computers in the cluster.

The _ScriptPreprocessing.cpp_ example shows how to create a script-based user-defined Aurora Imaging Library function. To run this and other examples, use Aurora Imaging Example Launcher.
