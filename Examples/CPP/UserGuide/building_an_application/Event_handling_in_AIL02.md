---
title: "Event_handling_in_AIL02"
description: "Unhooking a function."
ms.snippet: "userguide.building_an_application.Event_handling_in_AIL02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Event_handling_in_AIL02

> Unhooking a function.

**Language:** C++ | **Version:** 7.5+

```cpp
/*Unhooking the hook-handler function*/

MbufHookFunction(AilImage, M_MODIFIED_BUFFER + M_UNHOOK,
    UserHookFunction, &UserHookData);
```
