---
title: "Send_trigger"
description: "Rapixo CoF send trigger"
ms.snippet: "boardspecific.Rapixo-CoF.Send_trigger"
ms.language: "C++"
ms.env: "vc"
ms.version: "11"
---

# Send_trigger

> Rapixo CoF send trigger

**Language:** C++ | **Version:** 11+

```cpp
/* Use CoaXPress trigger signal to trigger the camera. */
MdigControl(AilDigitizer, M_IO_SOURCE + M_TL_TRIGGER, M_AUX_IO4);
```
