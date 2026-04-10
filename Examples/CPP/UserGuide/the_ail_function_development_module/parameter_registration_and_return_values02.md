---
title: "parameter_registration_and_return_values02"
description: "Returning a value in a user defined function using a pointer"
ms.snippet: "userguide.the_AIL_function_development_module.parameter_registration_and_return_values02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# parameter_registration_and_return_values02

> Returning a value in a user defined function using a pointer

**Language:** C++ | **Version:** 7.5+

```cpp
void MFTYPE MyFuncCallee( AIL_ID Func ){
   AIL_ID SrcImage;

   int *ReturnValuePtr;
   int ValueToReturn = 0;

   MfuncParamValue( Func, 1, &SrcImage );
   MfuncParamValue( Func, 2, &ReturnValuePtr );

   /* Calculate the return value */
   // ValueToReturn = ...

   *ReturnValuePtr = ValueToReturn;
}
```
