---
title: "wrapper_function"
description: "Safe type should not be used when wrapper functions are used and data types are ambiguous"
ms.snippet: "userguide.building_an_application.wrapper_function"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# wrapper_function

> Safe type should not be used when wrapper functions are used and data types are ambiguous

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_INT UserWrapperAroundMbufInquire(AIL_ID BufId, AIL_INT64 InquireType, void *UserVarPtr){
return MbufInquire(BufId, InquireType, UserVarPtr);
}
```
