---
title: "associating_a_ail_identifier_with_a_user-define_object02"
description: "Using a pointer to modify the data in auser-defined object using MfuncInquire"
ms.snippet: "userguide.the_ail_function_development_module.associating_a_ail_identifier_with_a_user-define_object02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# associating_a_ail_identifier_with_a_user-define_object02

> Using a pointer to modify the data in auser-defined object using MfuncInquire

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_ID MyObjectId;
struct MyStruct MyObject = { 0 , 0 , 0 };

MyObjectId = MfuncAllocId( M_DEFAULT, M_USER_OBJECT_1+0x0001, &MyObject );
```
