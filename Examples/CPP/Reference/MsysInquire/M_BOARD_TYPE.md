---
title: "M_BOARD_TYPE"
description: "MsysInquire()"
ms.snippet: "reference.MsysInquire.M_BOARD_TYPE"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# M_BOARD_TYPE

> MsysInquire()

**Language:** C++ | **Version:** 8.0+

```cpp
/* To return only the main board type, and not the sub-board types (for example, 
M_CXP or M_CL), mask the return value with M_BOARD_TYPE_MASK.
*/

/* Call MsysInquire with M_BOARD_TYPE to inquire the full board type. */
MsysInquire(AilSystem, M_BOARD_TYPE, &BoardType);

/* Use M_BOARD_TYPE_MASK to verify the main board type. */
if ((BoardType & M_BOARD_TYPE_MASK) == M_RAPIXO)
{
/* Perform a Zebra Rapixo-specific task. */
}
```
