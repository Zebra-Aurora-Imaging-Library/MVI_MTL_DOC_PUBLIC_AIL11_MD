---
title: "creating_and_modifying_graphics_interactively02"
description: "Hook function"
ms.snippet: "userguide.generating_graphics.creating_and_modifying_graphics_interactively02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# creating_and_modifying_graphics_interactively02

> Hook function

**Language:** C++ | **Version:** 7.5+

```cpp
/* Hook function used to notify a thread that interactive graphic creation has ended. */
static AIL_INT MFTYPE WaitUntilCreationEnds(AIL_INT HookType, AIL_ID EventId, void *UserDataPtr)
   {
   /* Casts UserDataPtr to the data type of the hook information structure. */
   CreateGraphicInfo* HookInfoPtr = (CreateGraphicInfo*)UserDataPtr;

   /* Get the previous and current interactive states of the graphics list. */
   AIL_INT PreviousInteractiveState, CurrentInteractiveState;
   MgraGetHookInfo(EventId, M_INTERACTIVE_GRAPHIC_PREVIOUS_STATE, &PreviousInteractiveState);
   MgraGetHookInfo(EventId, M_INTERACTIVE_GRAPHIC_STATE, &CurrentInteractiveState);

   if (CurrentInteractiveState == M_STATE_BEING_CREATED)
      {
      /* If the current state is M_STATE_BEING_CREATED, the graphic being 
         created has been assigned a label value and position in the graphics list.
         Fetch its label and store it in the hook information structure. */
      MgraGetHookInfo(EventId, M_GRAPHIC_LABEL_VALUE, &HookInfoPtr->CreatedGraphicLabel);
      }
   else if (PreviousInteractiveState == M_STATE_BEING_CREATED)
      {
      /* If the previous state was M_STATE_BEING_CREATED, graphic creation is done. Set
         the event state to M_SIGNALED so the waiting thread can continue execution. */
      MthrControl(HookInfoPtr->CreationDoneEventThrId, M_EVENT_SET, M_SIGNALED);
      }

   return M_NULL;
   }
```
