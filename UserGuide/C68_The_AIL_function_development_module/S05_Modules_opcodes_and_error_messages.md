---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Modules_opcodes_and_error_messages
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Modules opcodes and error messages"
---

# Modules, opcodes, and error handling

User-defined Aurora Imaging Library functions can be associated with a user-defined module; related functions are typically grouped into the same user-defined module. You can group your functions into a maximum of 7 user-defined modules, each of which can contain up to 128 user-defined Aurora Imaging Library functions. A function's opcode indicates both its module and its offset within the module. When allocating the function context using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md), the opcode is specified as [`M_USER_MODULE_n`](../../Reference/func/MfuncAlloc.md) +_m_, where _n_ specifies the user-defined module and _m_ specifies the function's offset within the module.

User-defined Aurora Imaging Library functions don't need to be grouped into a module. In this case, specify the function's opcode using [`M_USER_FUNCTION`](../../Reference/func/MfuncAlloc.md) + _m_, where m is a value between 0 to 127 that has not already been assigned to another ungrouped function. This means that to generate an opcode, you can have up to 128 ungrouped user-defined functions.

A function's opcode must be unique. The uniqueness of the function's opcode is particularly important when retrieving error codes since the function that returned an error is identified by its opcode. The uniqueness is also critical when executing the callee function remotely; see [Caller/callee dynamics on a remote system](S07_Controller_worker_dynamics_on_a_remote_system.md).

## Error handling in callee functions

An Aurora Imaging Library application has specific functionality and settings for error handling. Typically, the settings apply to the entire application. However, a callee function has its own error handling settings.

> **Note:** For general information about error handling in Aurora Imaging Library, see[Aurora Imaging Library errors](../C65_Development_and_Debugging_Tools_and_Techniques/S03_Errors.md).

### Current and global errors

An application has a place to store its current error and global error, if any exist. Any errors that come from a function within a callee function, however, are stored in a separate place reserved for the callee function's current and global error. To inquire or clear the callee function's errors, call [`MappGetError`](../../Reference/app/MappGetError.md) or [`MappControl`](../../Reference/app/MappControl.md) with [`M_CLEAR_ERROR`](../../Reference/app/MappControl.md), respectively, within the callee function itself. Calling these functions within the callee function will have no effect on the error settings of the application.

When a callee function ends, its global error, if any, is transferred to the current error of the main application. If the callee function's errors were cleared prior to it ending, nothing is transferred.

> **Note:** Each time your user-defined Aurora Imaging Library function is called, its callee function starts with cleared errors. Its current and global errors are not retained across multiple calls.

### Accessing errors in callee functions from the main thread

Within the thread that calls a callee function, you can retrieve information about the error reported by the function using[`MappGetError`](../../Reference/app/MappGetError.md) with [`M_CURRENT`](../../Reference/app/MappGetError.md) after the callee function has completed. The error reported is the first error that was generated in the callee function, unless it was cleared using [`MappControl`](../../Reference/app/MappControl.md) with [`M_CLEAR_ERROR`](../../Reference/app/MappControl.md)within the callee function (in which case the first error generated after the error was cleared is reported).

> **Note:** Note that if you have specified that your Aurora Imaging Library function executes asynchronously ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md)with[`M_ASYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md)) or calls an Aurora Imaging Library function that executes asynchronously (and therefore might complete after the callee function returns), there are additional considerations for error checking. For more information, see [Asynchronous Aurora Imaging Library functions](../C65_Development_and_Debugging_Tools_and_Techniques/S03_Errors.md).

Alternatively, within the thread that calls the main function, you can hook to Aurora Imaging Library errors generated within the callee function using [`MappHookFunction`](../../Reference/app/MappHookFunction.md)with [`M_CALLEE_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md). In this case, the hook function will be called immediately for every error that is generated (before the callee function has the opportunity to clear the error using [`MappControl`](../../Reference/app/MappControl.md) with [`M_CLEAR_ERROR`](../../Reference/app/MappControl.md)).

> **Note:** Note that, within a callee function, you can call [`MappHookFunction`](../../Reference/app/MappHookFunction.md) to hook a user-defined function to an event. However, the [`HookType`](../../Reference/app/MappHookFunction.md) cannot be [`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md). Specifying this event type will generate an error.

### Logging errors

When developing user-defined Aurora Imaging Library functions, [`MfuncErrorReport`](../../Reference/func/MfuncErrorReport.md) allows you to create custom error codes and error messages which are treated as normal Aurora Imaging Library errors. [`MfuncErrorReport`](../../Reference/func/MfuncErrorReport.md) also allows you to associate each error code with an error message; up to one hundred such custom error code associations can be defined within Aurora Imaging Library.

Any error logged by calling [`MfuncErrorReport`](../../Reference/func/MfuncErrorReport.md) from within a callee function will be stored as the callee function's new global error. If you set [`ErrorCode`](../../Reference/func/MfuncErrorReport.md) to [`M_NULL`](../../Reference/func/MfuncErrorReport.md), the callee function's global error is reset, resulting in no error being returned after the callee function has ended.

### Print error settings

Each callee function has its own print error setting, which controls whether an error is printed to the screen, independent of the caller function's setting. The default value for a callee function's print error setting is different from most other types of functions. The default for a callee function is [`M_PRINT_DISABLE`](../../Reference/app/MappControl.md), while it is [`M_PRINT_ENABLE`](../../Reference/app/MappControl.md) for the rest. This setting can be changed using [`MappControl`](../../Reference/app/MappControl.md) set to [`M_ERROR`](../../Reference/app/MappControl.md)+[`M_THREAD_CURRENT`](../../Reference/app/MappControl.md). Whatever the setting, you can always check for an error using [`MappGetError`](../../Reference/app/MappGetError.md).
