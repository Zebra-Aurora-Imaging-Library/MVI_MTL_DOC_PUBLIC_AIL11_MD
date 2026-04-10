---
title: "expected_data_type02"
description: "Two calls to the same function where different data types are expected."
ms.snippet: "userguide.building_an_application.expected_data_type02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# expected_data_type02

> Two calls to the same function where different data types are expected.

**Language:** C++ | **Version:** 7.5+

```cpp
// Two calls to the same function where different data types are expected.
//
/*Pointer for last parameter of MgraInquire should reference an AIL_DOUBLE*/
AIL_DOUBLE     Value3;
MgraInquire(ContextGraId, M_DRAW_OFFSET_X, &Value3);
//
/*Pointer for last parameter of MgraInquire should reference an AIL_INT*/
AIL_INT    Value4;
MgraInquire(ContextGraId, M_DRAW_OFFSET_X + M_TYPE_AIL_INT, &Value4);
```
