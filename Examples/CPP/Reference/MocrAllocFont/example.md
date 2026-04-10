---
title: "example"
description: "Example in MocrAllocFont()"
ms.snippet: "reference.MocrAllocFont.example"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# example

> Example in MocrAllocFont()

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_ID FontID = M_NULL;
MocrAllocFont(M_DEFAULT, M_USER_DEFINED+M_GENERAL,
              26, 8, 11, 1, 1, 6, 9, 1, 26, M_FOREGROUND_BLACK, &FontID);
```
