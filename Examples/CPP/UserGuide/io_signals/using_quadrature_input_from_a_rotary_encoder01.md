---
title: "using_quadrature_input_from_a_rotary_encoder01"
description: "Rotary decoder decimation"
ms.snippet: "userguide.io_signals.using_quadrature_input_from_a_rotary_encoder01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# using_quadrature_input_from_a_rotary_encoder01

> Rotary decoder decimation

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set the rotary decoder to output a pulse at every 10 steps (incremented).*/
MdigControl(AilDigitizer, M_ROTARY_ENCODER_DIRECTION, M_FORWARD);
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION_TRIGGER, 10);
MdigControl(AilDigitizer, M_ROTARY_ENCODER_RESET_SOURCE, M_POSITION_TRIGGER);
MdigControl(AilDigitizer, M_ROTARY_ENCODER_OUTPUT_MODE, M_POSITION_TRIGGER);

/* Program the timer to trigger on the rotary decoder output.*/
MdigControl(AilDigitizer, M_TIMER_DELAY + M_TIMER1, 1000); // 1us delay
MdigControl(AilDigitizer, M_TIMER_DURATION + M_TIMER1, 1000000); // 1ms active time.
MdigControl(AilDigitizer, M_TIMER_STATE + M_TIMER1, M_ENABLE);
MdigControl(AilDigitizer, M_TIMER_TRIGGER_SOURCE + M_TIMER1, M_ROTARY_ENCODER);

/* Reset the rotary decoder's counter to 0 (currently its value is unknown).*/
MdigControl(AilDigitizer, M_ROTARY_ENCODER_POSITION, 0);
```
