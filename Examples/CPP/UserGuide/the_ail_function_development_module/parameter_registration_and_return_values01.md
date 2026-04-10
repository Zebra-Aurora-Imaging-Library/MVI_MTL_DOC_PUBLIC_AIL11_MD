---
title: "parameter_registration_and_return_values01"
description: "Registering a pointer to a return variable as a parameter"
ms.snippet: "userguide.the_AIL_function_development_module.parameter_registration_and_return_values01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# parameter_registration_and_return_values01

> Registering a pointer to a return variable as a parameter

**Language:** C++ | **Version:** 7.5+

```cpp
int MyFunc( AIL_ID SrcImage ) {
   AIL_ID MyFunctionContext;
   int ReturnValue = 0;

   MfuncAlloc( AIL_TEXT("MyFunc"), 2, MyFuncCallee, M_NULL, M_NULL,
               M_USER_MODULE_1+1, M_SYNCHRONOUS_FUNCTION+M_LOCAL,
               &MyFunctionContext );

   MfuncParamAilId   ( MyFunctionContext, 1, SrcImage, M_IMAGE, M_IN+M_PROC );
   MfuncParamDataPointer ( MyFunctionContext, 2, &ReturnValue, sizeof(ReturnValue), M_OUT );

   MfuncCall( MyFunctionContext );

   MfuncFree( MyFunctionContext );

   return ReturnValue;
}
```
