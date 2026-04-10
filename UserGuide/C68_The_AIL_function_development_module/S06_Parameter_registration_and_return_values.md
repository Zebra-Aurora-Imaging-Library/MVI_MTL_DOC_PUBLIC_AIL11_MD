---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: Parameter_registration_and_return_values
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / Parameter registration and return values"
---

# Parameter registration and return values

Parameters must be registered in the caller function with the [`MfuncParam...`](../../Reference/func/MfuncParam.md) function that matches the data type of the parameter (for example, a parameter of type _AIL_INT_ should be registered using [`MfuncParamAilInt`](../../Reference/MfuncParamAilInt.md)); the values of the parameters passed to the caller function must then be retrieved from within the callee function using the corresponding [`MfuncParamValue...`](../../Reference/func/MfuncParamValue.md). You can alternatively use the general form of the above functions, [`MfuncParam`](../../Reference/func/MfuncParam.md) and [`MfuncParamValue`](../../Reference/func/MfuncParamValue.md), respectively.

Certain parameter registration functions require additional details about the parameter being registered:

- When registering parameters of type _AIL_ID_ (using [`MfuncParamAilId`](../../Reference/MfuncParamAilId.md), [`MfuncParamArrayAilId`](../../Reference/MfuncParamArrayAilId.md), or [`MfuncParamConstArrayAilId`](../../Reference/MfuncParamConstArrayAilId.md)), you must specify the type of Aurora Imaging Library object.
- When registering parameters that accept pointers (using [`MfuncParamDataPointer`](../../Reference/MfuncParamDataPointer.md) or [`MfuncParamConstDataPointer`](../../Reference/MfuncParamConstDataPointer.md)), you must specify the size of the data, in bytes.
- When registering parameters that accept strings (using [`MfuncParamAilText`](../../Reference/MfuncParamAilText.md), [`MfuncParamConstAilText`](../../Reference/MfuncParamConstAilText.md), or [`MfuncParamFilename`](../../Reference/MfuncParamFilename.md)), you must specify the number of characters in the string, including the null terminator.
- When registering parameters of type _AIL_ID_ (using [`MfuncParamAilId`](../../Reference/MfuncParamAilId.md), [`MfuncParamArrayAilId`](../../Reference/MfuncParamArrayAilId.md), or [`MfuncParamConstArrayAilId`](../../Reference/MfuncParamConstArrayAilId.md)) or parameters that accept strings or pointers, you must specify whether the parameter will serve as input ([`M_IN`](../../Reference/func/MfuncParam.md)), output ([`M_OUT`](../../Reference/func/MfuncParam.md)), or both ([`M_IN`](../../Reference/func/MfuncParam.md) `+` [`M_OUT`](../../Reference/func/MfuncParam.md)) for your function. Your callee function should use output parameters to return results (for example, the destination buffer of an operation should be passed as an output parameter).

The [`M_IN`](../../Reference/func/MfuncParam.md) and [`M_OUT`](../../Reference/func/MfuncParam.md) attributes of the [`MfuncParam...`](../../Reference/func/MfuncParam.md) functions do not serve to limit the callee function's ability to modify variables; rather, the [`M_OUT`](../../Reference/func/MfuncParam.md) attribute serves to indicate to Aurora Imaging Library that when the callee function finishes, Aurora Imaging Library must synchronize the callee function's data for that parameter with the rest of the application. Identifying output parameters is particularly important when the callee function is being executed by a remote processor which does not have direct access to the Host's address space. In this case, when the callee function terminates, Aurora Imaging Library will internally pass the new values for the output type parameters to the Host. If the content of a buffer is modified in a callee function executed by a remote processor, and the buffer was not registered as an output parameter, your changes will be lost when the callee function finishes.

From within the callee function, you must declare a new variable for each parameter registered in the caller function; the data type of each new variable must match the data type of its associated parameter. For example, a parameter of type _AIL_ID_ must be registered using [`MfuncParamAilId`](../../Reference/MfuncParamAilId.md) in the caller function; then, in the callee function, you must declare a new variable of type _AIL_ID_ and use [`MfuncParamValueAilId`](../../Reference/MfuncParamValueAilId.md) to associate the new variable with the value passed as a parameter to the caller function.

> **Note:** Note that, if you are using an output pointer parameter, your function context must be allocated to run synchronously.

## MfuncParam and MfuncParamValue

When registering a parameter, you can call the general function [`MfuncParam`](../../Reference/func/MfuncParam.md), which you can use to register any type of parameter, or you can call a data-type specific [`MfuncParam...`](../../Reference/func/MfuncParam.md) function, such as [`MfuncParamAilId`](../../Reference/MfuncParamAilId.md) and [`MfuncParamAilInt`](../../Reference/MfuncParamAilInt.md). [`MfuncParam`](../../Reference/func/MfuncParam.md) will internally call the appropriate data-type specific version based on the values you specify in the function.

Similarly to [`MfuncParam`](../../Reference/func/MfuncParam.md), when reading the value of a parameter registered in the caller function, you can call the general function [`MfuncParamValue`](../../Reference/func/MfuncParamValue.md), or you can call a data-type specific [`MfuncParamValue...`](../../Reference/func/MfuncParamValue.md) function, such as [`MfuncParamValueAilId`](../../Reference/MfuncParamValueAilId.md) and [`MfuncParamValueAilInt`](../../Reference/MfuncParamValueAilInt.md).

## Return values

Only synchronous user-defined Aurora Imaging Library functions ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) with [`M_SYNCHRONOUS_FUNCTION`](../../Reference/func/MfuncAlloc.md)) can have a return value. To have a user-defined Aurora Imaging Library function return a value, the return value must be generated in the callee function and returned by the caller function. However, the callee function of a user-defined Aurora Imaging Library function must be declared as being of type `void MFTYPE`; therefore, callee functions can't directly return a value. To return a value, the caller function must declare a variable (the return variable) and register a pointer to it as if it was a real [`M_OUT`](../../Reference/func/MfuncParam.md) parameter of the caller function. The callee function should treat the return variable as a regular [`M_OUT`](../../Reference/func/MfuncParam.md) parameter.

Note that the number of parameters specified during context allocation in a user-defined Aurora Imaging Library function ([`MfuncAlloc`](../../Reference/func/MfuncAlloc.md)) limits the number of parameters that the callee function can retrieve. If a user-defined Aurora Imaging Library function has a return value, the number of parameters must be set to one greater than is actually passed to the caller function. The address of the return variable must be registered as a pointer parameter in the caller function ([`MfuncParamDataPointer`](../../Reference/MfuncParamDataPointer.md) or [`MfuncParamArrayAilId`](../../Reference/MfuncParamArrayAilId.md) with [`Attribute`](../../Reference/func/MfuncParam.md) set to [`M_OUT`](../../Reference/func/MfuncParam.md)). Then, in the callee function, you must declare a pointer variable of the same type as the return variable and associate the pointer with the address of the return variable using [`MfuncParamValue`](../../Reference/func/MfuncParamValue.md). Recall that, the callee function does not actually store the return value at the memory address provided by the caller function; it provides the caller function with the new value to be written to memory.

The following portion of Aurora Imaging Library code shows the caller function of a user-defined Aurora Imaging Library function with a return value:

> **Code example:** [userguide.The_AIL_function_development_module.parameter_registration_and_return_values01](userguide.The_AIL_function_development_module.parameter_registration_and_return_values01)

The following portion of Aurora Imaging Library code shows how to retrieve the address of the return variable and use it to store the return value (in the callee function):

> **Code example:** [userguide.The_AIL_function_development_module.parameter_registration_and_return_values02](userguide.The_AIL_function_development_module.parameter_registration_and_return_values02)
