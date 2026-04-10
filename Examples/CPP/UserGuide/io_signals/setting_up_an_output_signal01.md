---
title: "setting_up_an_output_signal01"
description: "setting mode, format and source"
ms.snippet: "userguide.io_signals.setting_up_an_output_signal01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# setting_up_an_output_signal01

> setting mode, format and source

**Language:** C++ | **Version:** 7.5+

```cpp
/* M_AUX_IO9 can be transmitted in TTL or LVDS format. Specify the TTL format. */   
MdigControl(AilDigitizer, M_IO_FORMAT + M_AUX_IO9, M_TTL);

/* Set the source of the signal to timer 1. */   
MdigControl(AilDigitizer, M_IO_SOURCE + M_AUX_IO9, M_TIMER1);

/* If the signal is a bidirectional signal, specify the mode. */
MdigControl(AilDigitizer, M_IO_MODE + M_AUX_IO9, M_OUTPUT);
```
