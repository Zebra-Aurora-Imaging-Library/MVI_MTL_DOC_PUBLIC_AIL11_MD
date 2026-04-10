---
title: "timers_and_coordinating_events01"
description: "Setting up a timer."
ms.snippet: "userguide.io_signals.timers_and_coordinating_events01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# timers_and_coordinating_events01

> Setting up a timer.

**Language:** C++ | **Version:** 7.5+

```cpp
/* Specify to use an input signal as the trigger source for a timer. */   
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE + M_TIMER1, M_AUX_IO1);

/* Set the delay between the trigger and the active portion of the timer to 1000 nsec. */
MdigControl(AilDigitizer, M_TIMER_DELAY + M_TIMER1, 1000);

/* Set the duration of the active portion for the timer to 1 msec (1 million nsec). */
MdigControl(AilDigitizer, M_TIMER_DURATION + M_TIMER1, 1000000);

/* Specify to trigger the timer when the trigger signal changes from low to high.*/
MdigControl(AilDigitizer, M_TIMER_TRIGGER_ACTIVATION + M_TIMER1, M_EDGE_RISING);

/* Route the output of the timer onto a camera control output signal. */
MdigControl(AilDigitizer, M_IO_SOURCE + M_CC_IO1, M_TIMER1);

/* Enable the timer to wait for a trigger.*/
MdigControl(AilDigitizer, M_TIMER_STATE + M_TIMER1, M_ENABLE);
```
