---
title: "ail_system_incorrectly_passed_to_dig_free"
description: "Aurora Imaging Library system incorrectly passed to MdigFree"
ms.snippet: "userguide.ail_system_incorrectly_passed_to_dig_free"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# ail_system_incorrectly_passed_to_dig_free

> Aurora Imaging Library system incorrectly passed to MdigFree

**Language:** C++ | **Version:** 7.5+

```cpp
/* This code will generate a runtime error */
AIL_ID AilApplication = MappAlloc(M_NULL, M_DEFAULT, M_NULL);
AIL_ID AilSystem = MsysAlloc(AilApplication, M_SYSTEM_DEFAULT, M_DEFAULT, M_DEFAULT, M_NULL);
MdigFree(AilSystem);
```
