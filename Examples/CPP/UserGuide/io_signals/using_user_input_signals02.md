---
title: "using_user_input_signals02"
description: "Using interrupts with input signals"
ms.snippet: "userguide.io_signals.using_user_input_signals02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# using_user_input_signals02

> Using interrupts with input signals

**Language:** C++ | **Version:** 7.5+

```cpp
/* Specify to use an input signal as the trigger source for grabbing an image.*/   
MdigControl(AilDigitizer, M_GRAB_TRIGGER_SOURCE, M_AUX_IO1);

/* Specify to trigger the grab when the signal transitions from low to high.*/
MdigControl(AilDigitizer, M_GRAB_TRIGGER_ACTIVATION, M_EDGE_RISING);

/* Enable digitizer to wait for a trigger to grab. */
MdigControl(AilDigitizer, M_GRAB_TRIGGER_STATE, M_ENABLE);

/* Execute grab (upon trigger). */
MdigGrab(AilDigitizer, AilImage);  
```
