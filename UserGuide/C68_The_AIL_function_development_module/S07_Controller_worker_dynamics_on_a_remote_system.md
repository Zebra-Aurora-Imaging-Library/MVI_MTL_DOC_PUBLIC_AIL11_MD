---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Controller_worker_dynamics_on_a_remote_system
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Controller worker dynamics on a remote system"
---

# Caller/callee dynamics on a remote system

When you allocate your function context in the caller function ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md)), you must provide an opcode for the user-defined function. If the callee function will be executed on the local computer, you must provide a pointer to the function using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`CalleeFunctionPtr`](../../Reference/func/MfuncAlloc.md), but if your callee function will be executed by the remote system in a Distributed Aurora Imaging Library application you must provide the name of the library file and the name of the function within the library file using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`CalleeFunctionDLLName`](../../Reference/func/MfuncAlloc.md) and [`CalleeFunctionName`](../../Reference/func/MfuncAlloc.md), respectively. You must also specify, using the [`InitFlag`](../../Reference/func/MfuncAlloc.md) parameter, if the callee function must be run on the local computer ([`M_LOCAL`](../../Reference/func/MfuncAlloc.md)) or, when possible, on the remote computer ([`M_REMOTE`](../../Reference/func/MfuncAlloc.md)). [`MfuncCall`](../../Reference/func/MfuncCall.md) uses the pointer to call the callee function when it is to be executed on the local computer, and uses either the opcode or the library file to call the callee function when it is to be executed on the remote computer. Even if the callee function will be executed on the local computer, the opcode is still required for error reporting purposes.

For more information about Distributed Aurora Imaging Library and its applications, see [Distributed Aurora Imaging Library](../C63_Distributed_AIL/ChapterInformation.md).

## Compilation

Depending on the scope requirements of your user-defined function, its caller function and its callee function will need to be compiled differently, according to how they will be called and how you want them to be executed. Every user-defined Aurora Imaging Library function will have different scope requirements depending on the application in which it is being used. There are 5 possible execution scenarios for a user-defined Aurora Imaging Library function:

- The user-defined Aurora Imaging Library function will be called and executed by the Host.
- The user-defined Aurora Imaging Library function will be called by the Host and executed by the remote system.
  In this scenario, the caller function should be compiled for the Host and the callee function should be compiled for the callee processor or into a library file, depending on the type of remote system executing the callee function.
- The user-defined Aurora Imaging Library function will be called and executed by the remote system.
  In this scenario, the caller and callee functions should be compiled for the callee processor or into a library file, depending on the type of remote system executing the callee function.
- The user-defined Aurora Imaging Library function might be called from either the Host or remote system, and is executed by the remote system.
  In this scenario, the caller function should be compiled for both the Host and remote system, and the callee function should be compiled for the callee processor or into a library file, depending on the type of remote system executing the callee function.

The recommended way to compile the caller and callee functions of a user-defined Aurora Imaging Library function is to compile both caller and callee functions for both the Host and remote systems. Compiling the function in this way ensures that your Aurora Imaging Library application will be portable to computers that might or might not have access to remote systems. Function contexts allocated to run on the remote computer ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`InitFlag`](../../Reference/func/MfuncAlloc.md) set to [`M_REMOTE`](../../Reference/func/MfuncAlloc.md)) will, in the absence of a remote processor, be executed on the local computer and function contexts allocated using the default execution type ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`InitFlag`](../../Reference/func/MfuncAlloc.md) set to [`M_DEFAULT`](../../Reference/func/MfuncAlloc.md)) will make use of the remote system when one is available.

Compiling a caller and/or callee function for the Host processor is typically the same as compiling any other function of your application.

## Executing the callee function on a remote system

Typically, Aurora Imaging Library automatically determines whether to have the callee function executed by the Host processor or by the remote processor. To determine where to execute the callee function, Aurora Imaging Library looks to the values of the user-defined Aurora Imaging Library function parameters, registered in the caller function. When all of the Aurora Imaging Library objects passed as parameters to the function are allocated on the same system, or if one of the parameters is an Aurora Imaging Library system identifier, the callee function is executed by the processor associated with that system. If the caller function has multiple Aurora Imaging Library object parameters, and the Aurora Imaging Library objects passed to these parameters are allocated on different systems, an error message is returned.

When a user-defined Aurora Imaging Library function has a user-defined Aurora Imaging Library object as one of its parameters, and you want the callee function to be executed by a remote processor, you must allocate this object on the system with the remote processor. For more information, see [Associating an Aurora Imaging Library identifier with a user-defined object](S08_Associating_an_identifier_with_a_userdefined_object.md).

> **Note:** Note that when the callee function is executed by the remote processor, operations that use other systems cannot be executed. For example, when a callee function is executed on a remote system using a Zebra Radient eV-CL, and there is a call in the callee function to an [`MdigGrab`](../../Reference/dig/MdigGrab.md) function that performs a grab on Zebra Rapixo CXP, this grab operation will cause an error.

### Using a Library file in a Distributed Aurora Imaging Library cluster

When compiling a new user-defined Aurora Imaging Library function for a remote system in a Distributed Aurora Imaging Library cluster, you must compile your user-defined function into a library file.

In the caller function, located in your application running on the Host-type system, you must provide the name of the library file containing the callee function and the name of the callee function, using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`CalleeFunctionDLLName`](../../Reference/func/MfuncAlloc.md) and [`CalleeFunctionName`](../../Reference/func/MfuncAlloc.md), respectively.

If your function will be called from a remote computer, the library file should also contain the caller function of your user-defined function. In the caller function, located in the library file, you can provide a pointer to your callee function using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) using [`CalleeFunctionPtr`](../../Reference/func/MfuncAlloc.md), or provide the name of the library file and the name of the callee function, as above. From within the library file, it is faster to provide a pointer to the callee function than it is to provide the name of the library file and the name of the function.

*[Image: mfunc_worker_on_remote.png]*
