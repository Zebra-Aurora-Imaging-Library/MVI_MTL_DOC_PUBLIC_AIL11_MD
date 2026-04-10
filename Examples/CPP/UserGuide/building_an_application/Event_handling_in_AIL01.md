---
title: "Event_handling_in_AIL01"
description: "User defined data structure."
ms.snippet: "userguide.building_an_application.Event_handling_in_AIL01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Event_handling_in_AIL01

> User defined data structure.

**Language:** C++ | **Version:** 7.5+

```cpp
/* Example of a data structure used to pass data to the hook-handler function. */

typedef struct
{
    AIL_ID  AilDigitizer;
    AIL_ID  AilImageDisp;
    AIL_INT ProcessedImageCount;
} HookDataStruct;
```
