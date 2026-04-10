---
title: "user_defined_data_type"
description: "Safe type should not be used when using custom data types"
ms.snippet: "userguide.building_an_application.user_defined_data_type"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# user_defined_data_type

> Safe type should not be used when using custom data types

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_INT8 *Data = (AIL_INT8 *)malloc(sizeof(AIL_DOUBLE) / sizeof(AIL_INT8));
MmodGetResult(ResultId, M_GENERAL, M_NUMBER, Data);
Number = *((double*)Data);
free(Data);
```
