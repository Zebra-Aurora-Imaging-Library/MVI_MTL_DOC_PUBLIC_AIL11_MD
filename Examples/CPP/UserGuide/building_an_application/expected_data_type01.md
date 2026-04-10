---
title: "expected_data_type01"
description: "Two calls to the same function where different data types are expected."
ms.snippet: "userguide.building_an_application.expected_data_type01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# expected_data_type01

> Two calls to the same function where different data types are expected.

**Language:** C++ | **Version:** 7.5+

```cpp
// Two calls to the same function where different data types are expected.
//
// Value1 should point to an AIL_INT
/*Pointer for last parameter of MbufInquire should reference an AIL_INT*/
AIL_INT     Value1;
MbufInquire(BufId, M_ALLOCATION_OVERSCAN_SIZE, &Value1);
//
/*Pointer for last parameter of MbufInquire should reference an AIL_ID*/
AIL_ID      Value2;
MbufInquire(BufId, M_ANCESTOR_ID, &Value2);
```
