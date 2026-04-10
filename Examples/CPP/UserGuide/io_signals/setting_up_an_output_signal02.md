---
title: "setting_up_an_output_signal02"
description: "Using bits in main static-user-output register"
ms.snippet: "userguide.io_signals.setting_up_an_output_signal02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# setting_up_an_output_signal02

> Using bits in main static-user-output register

**Language:** C++ | **Version:** 7.5+

```cpp
/* In this example, M_AUX_IO3 is a bidirectional signal and M_AUX_IO12 is an output signal. */ 
/* For the board in question, only the state of user-bit 5 can be routed to M_AUX_IO3       */
/* (in output mode), while only the state of user-bit 0 can be routed to M_AUX_IO12.        */

/* Set the initial state of bit 5 and 0 of the main static-user-output register to off.*/
MsysControl(AilSystem, M_USER_BIT_STATE + M_USER_BIT5, M_OFF);
MsysControl(AilSystem, M_USER_BIT_STATE + M_USER_BIT0, M_OFF);

/* Set the source of the output signal to user-bit 5 and 0. */
MsysControl(AilSystem, M_IO_SOURCE + M_AUX_IO3, M_USER_BIT5); 
MsysControl(AilSystem, M_IO_SOURCE + M_AUX_IO12, M_USER_BIT0);

/* Set bidirectional signal M_AUX_IO3 to output mode. */
MsysControl(AilSystem, M_IO_MODE + M_AUX_IO3, M_OUTPUT);

/* Perform some operation that establishes whether user-bit 5 and/or 0 should be set to on. */
/* Store the result in ResultOfOperation.                                                   */
/* ... */


/* Set user-bits 5 and 0 according to the calculated results, while leaving all other      */
/* user-bits in their current state.                                                       */
MsysInquire(AilSystem, M_USER_BIT_STATE_ALL, &CurrentValueOfAllBits);
switch (ResultOfOperation) 
   {
   case 0: /* Set user-bit 5 to off and user-bit 0 to off. */
     MsysControl(AilSystem, M_USER_BIT_STATE_ALL, (CurrentValueOfAllBits & ~0x0021) | 0x0000);
     break;
   case 1: /* Set user-bit 5 to on and user-bit 0 to off. */
     MsysControl(AilSystem, M_USER_BIT_STATE_ALL, (CurrentValueOfAllBits & ~0x0021) | 0x0020);
     break;
   case 2: /* Set user-bit 5 to off and user-bit 0 to on. */
     MsysControl(AilSystem, M_USER_BIT_STATE_ALL, (CurrentValueOfAllBits & ~0x0021) | 0x0001);
     break;
   case 3: /* Set user-bit 5 to on and user-bit 0 to on. */
     MsysControl(AilSystem, M_USER_BIT_STATE_ALL, (CurrentValueOfAllBits & ~0x0021) | 0x0021);
     break;
}
```
