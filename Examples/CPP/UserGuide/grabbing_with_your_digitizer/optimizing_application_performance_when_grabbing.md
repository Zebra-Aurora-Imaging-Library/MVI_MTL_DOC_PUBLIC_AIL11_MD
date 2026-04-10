---
title: "optimizing_application_performance_when_grabbing"
description: "Optimizing application performance when grabbing"
ms.snippet: "userguide.grabbing_with_your_digitizer.optimizing_application_performance_when_grabbing"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# optimizing_application_performance_when_grabbing

> Optimizing application performance when grabbing

**Language:** C++ | **Version:** 7.5+

```cpp
/* Put digitizer in asynchronous mode */
MdigControl(AilDigitizer, M_GRAB_MODE, M_ASYNCHRONOUS);
/* Grab the sequence. */
for (n=0; n<NbFrames; n++){
      /* Grab one buffer at a time. */
      MdigGrab(AilDigitizer, AilImage[n]);
   }
```
