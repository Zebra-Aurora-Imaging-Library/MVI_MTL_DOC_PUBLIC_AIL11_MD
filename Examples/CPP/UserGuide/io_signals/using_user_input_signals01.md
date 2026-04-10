---
title: "using_user_input_signals01"
description: "Using input signals as triggers"
ms.snippet: "userguide.io_signals.using_user_input_signals01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# using_user_input_signals01

> Using input signals as triggers

**Language:** C++ | **Version:** 7.5+

```cpp
   /* Hook function declaration. */
AIL_INT MFTYPE HookHandlerFnct(AIL_INT HookType, AIL_ID EventId, void* UserDataPtr)
   {
      AIL_INT IOSource = 0;
      MdigGetHookInfo(EventId, M_IO_INTERRUPT_SOURCE, &IOSource);
      // IOSource will be M_AUX_IO8 when interrupt is fired.
      if (IOSource == M_AUX_IO8)
      {
         /* Perform some action.*/
      }      
      return 0;
   }   


void UsingInputSignals(AIL_ID AilDigitizer, AIL_ID AilImage)
   {
   void* HookData = 0;

   /* Specify to generate an interrupt upon the low-to-high input signal state transition. */
   MdigControl(AilDigitizer, M_IO_INTERRUPT_ACTIVATION + M_AUX_IO8, M_EDGE_RISING);

   /* Specify to hook a function to the specified interrupt. */
   MdigHookFunction(AilDigitizer, M_IO_CHANGE,
                        HookHandlerFnct, (void*) &HookData);

   /* Enable the specified interrupt. */
   MdigControl(AilDigitizer, M_IO_INTERRUPT_STATE + M_AUX_IO8, M_ENABLE); 

   /* ... */
```
