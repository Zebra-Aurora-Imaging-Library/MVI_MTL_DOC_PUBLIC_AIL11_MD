---
title: "porting_64"
description: "Two calls to the same function where different data types are expected."
ms.snippet: "userguide.building_an_application.porting_64"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# porting_64

> Two calls to the same function where different data types are expected.

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_INT SizeX;
MbufInquire(BufId, M_SIZE_X, &SizeX);
```
