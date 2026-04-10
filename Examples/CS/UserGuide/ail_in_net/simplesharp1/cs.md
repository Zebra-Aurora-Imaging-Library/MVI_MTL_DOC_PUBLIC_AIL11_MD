---
title: "cs"
description: ".NET chapter: Building a .NET app"
ms.snippet: "userguide.ail_in_net.simplesharp1.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "9.0"
---

# cs

> .NET chapter: Building a .NET app

**Language:** C# | **Version:** 9.0+

```csharp
// C# sample function call. 
// Note the 'AIL.' preface for the function name and constants
AIL.MbufAlloc2d(ownerSystem, 640, 480, 8 + AIL.M_UNSIGNED, 
                             AIL.M_IMAGE + AIL.M_PROC, ref _bufID);
```
