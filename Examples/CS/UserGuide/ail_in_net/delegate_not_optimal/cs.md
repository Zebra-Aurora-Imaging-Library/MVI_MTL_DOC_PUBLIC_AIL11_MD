---
title: "cs"
description: "userguide.mil_in_net.delegate_not_optimal"
ms.snippet: "userguide.ail_in_net.delegate_not_optimal.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "7.5"
---

# cs

> userguide.mil_in_net.delegate_not_optimal

**Language:** C# | **Version:** 7.5+

```csharp
using System;
using Zebra.AuroraImagingLibrary;

namespace Delegates.Incorrect
{
    class MyDigitizer
    {
        private AIL_ID _digitizerId = AIL.M_NULL;
        public MyDigitizer(AIL_ID sysId)
        {
            AIL.MdigAlloc(sysId, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref _digitizerId);
            AIL_DIG_HOOK_FUNCTION_PTR myDel = new AIL_DIG_HOOK_FUNCTION_PTR(MyHookHandler);
            AIL.MdigHookFunction(_digitizerId, AIL.M_GRAB_START, myDel, AIL.M_NULL);
        }

        public AIL_INT MyHookHandler(AIL_INT hookType, AIL_ID eventId, IntPtr userDataPtr)
        {
            return AIL.M_NULL;
        }
    }
}
```
