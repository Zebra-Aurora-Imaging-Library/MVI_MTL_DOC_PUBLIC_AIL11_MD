---
title: "displays_and_dail_remote_systems02"
description: "Display on the remote computer"
ms.snippet: "userguide.distributed_ail.displays_and_dail_remote_systems02"
ms.language: "C++"
ms.env: "vc"
ms.version: "9.0"
---

# displays_and_dail_remote_systems02

> Display on the remote computer

**Language:** C++ | **Version:** 9.0+

```cpp
/* Allocate a display on a DAIL remote system, which displays image buffers */
/* on a remote computer.                                       */
MdispAlloc(RemoteSystemId, M_DEFAULT, AIL_TEXT("M_DEFAULT"), 
                          M_WINDOWED+M_REMOTE_DISPLAY, &DisplayId);
MdispSelect(DisplayId, RemoteImageBufId);
```
