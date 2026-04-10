---
title: "allocating_dail_remote_systems01"
description: "Allocation without specifying port"
ms.snippet: "userguide.distributed_ail.allocating_dail_remote_systems01"
ms.language: "C++"
ms.env: "vc"
ms.version: "9.0"
---

# allocating_dail_remote_systems01

> Allocation without specifying port

**Language:** C++ | **Version:** 9.0+

```cpp
/* Connection made without specifying port.   */ 
MsysAlloc(AIL_TEXT("dailtcp://192.168.53.4/M_SYSTEM_HOST"), M_DEFAULT, M_DEFAULT, 
         &RemoteSystemId);
```
