---
title: "displays_and_dail_remote_systems01"
description: "Display on the local computer"
ms.snippet: "userguide.distributed_ail.displays_and_dail_remote_systems01"
ms.language: "C++"
ms.env: "vc"
ms.version: "9.0"
---

# displays_and_dail_remote_systems01

> Display on the local computer

**Language:** C++ | **Version:** 9.0+

```cpp
/* Allocate a display on a DAIL remote system, which displays image */ 
/* buffers on the local computer.                           */
MdispAlloc(RemoteSystemId, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_WINDOWED, &DisplayId);
MdispSelect(DisplayId, RemoteImageBufId);
```
