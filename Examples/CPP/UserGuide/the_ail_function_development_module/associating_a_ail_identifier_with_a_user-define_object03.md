---
title: "associating_a_ail_identifier_with_a_user-define_object03"
description: "Using a pointer to modify the data in a user-defined object using MfuncInquire"
ms.snippet: "userguide.the_ail_function_development_module.associating_a_ail_identifier_with_a_user-define_object03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# associating_a_ail_identifier_with_a_user-define_object03

> Using a pointer to modify the data in a user-defined object using MfuncInquire

**Language:** C++ | **Version:** 7.5+

```cpp
struct MyStruct *MyObjectPtr = M_NULL;
int Result1 = 0;
int Result2 = 0;
int Result3 = 0;

MfuncInquire( MyObjectId, M_OBJECT_PTR, &MyObjectPtr );

/* Perform the calculations */
/* Result1 = ... */
/* Result2 = ... */
/* Result3 = ... */

/* The user-defined object's data can now be accessed using the pointer
   retrieved by MfuncInquire */
MyObjectPtr->MyData1 = Result1 ;
MyObjectPtr->MyData2 = Result2 ;
MyObjectPtr->MyData3 = Result3 ;
```
