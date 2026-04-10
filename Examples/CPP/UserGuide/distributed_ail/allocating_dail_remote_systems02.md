---
title: "allocating_dail_remote_systems02"
description: "Allocation and specifying port"
ms.snippet: "userguide.distributed_ail.allocating_dail_remote_systems02"
ms.language: "C++"
ms.env: "vc"
ms.version: "9.0"
---

# allocating_dail_remote_systems02

> Allocation and specifying port

**Language:** C++ | **Version:** 9.0+

```cpp
/* In this case, the connection cannot be made without specifying the port - an error   */
/* is generated with the following call.                                    */
 MsysAlloc(AIL_TEXT("dailtcp://192.168.53.4/M_SYSTEM_HOST"), M_DEFAULT, M_DEFAULT, 
         &RemoteSystemId);

/* Since the port is specified and it matches the listening port on the remote computer,*/
/* the connection is made with the following call.                              */
MsysAlloc(AIL_TEXT("dailtcp://192.168.53.4:58000/M_SYSTEM_HOST"), M_DEFAULT, M_DEFAULT, 
         &RemoteSystemId);
```
