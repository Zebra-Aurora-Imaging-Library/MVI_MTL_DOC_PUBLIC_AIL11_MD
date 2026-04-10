---
title: "timers_and_coordinating_events02"
description: "Using a timer as a clock source for a timer."
ms.snippet: "userguide.io_signals.timers_and_coordinating_events02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# timers_and_coordinating_events02

> Using a timer as a clock source for a timer.

**Language:** C++ | **Version:** 7.5+

```cpp
/* In this example, timer 1 is using the default clock source
   to generate a continuous output signal with a frequency of 0.5 MHz. 
  Timer 2 will use timer 1 as a clock source to obtain a 
  timer output with a signal period of 32 seconds per cycle. */

/* TIMER 1 SET-UP.*/

/* Specify to output a continuous signal for timer 1. */   
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE + M_TIMER1, M_CONTINUOUS);

/* Timer 1 output delay and active portion (duration) set to 1000 nsec. */
MdigControl(AilDigitizer, M_TIMER_DELAY + M_TIMER1, 1000);
MdigControl(AilDigitizer, M_TIMER_DURATION + M_TIMER1, 1000);



/* TIMER 2 SET-UP.*/

/* Set the clock source of timer 2 to timer 1. */   
MdigControl(AilDigitizer, M_TIMER_CLOCK_SOURCE + M_TIMER2, M_TIMER1);

/* Specify to output a continuous signal for timer 2. */   
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE + M_TIMER2, M_CONTINUOUS);

/* Timer 2 output delay set to 12 seconds (12 billion nsec). */
MdigControl(AilDigitizer, M_TIMER_DELAY + M_TIMER2, 12000000000);

/* Timer 2 active portion (duration) set to 20 seconds (20 billion nsec). */
MdigControl(AilDigitizer, M_TIMER_DURATION + M_TIMER2, 20000000000);


/* Enable (start) both timers.*/
MdigControl(AilDigitizer, M_TIMER_STATE + M_TIMER1, M_ENABLE);
MdigControl(AilDigitizer, M_TIMER_STATE + M_TIMER2, M_ENABLE);
```
