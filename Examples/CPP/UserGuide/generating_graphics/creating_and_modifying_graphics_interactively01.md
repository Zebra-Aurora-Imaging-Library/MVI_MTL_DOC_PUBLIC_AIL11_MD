---
title: "creating_and_modifying_graphics_interactively01"
description: "Adding rectangle interactively"
ms.snippet: "userguide.generating_graphics.creating_and_modifying_graphics_interactively01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# creating_and_modifying_graphics_interactively01

> Adding rectangle interactively

**Language:** C++ | **Version:** 7.5+

```cpp
/* Structure used to hold information for the hook function. */
struct CreateGraphicInfo
   {
   AIL_ID  CreationDoneEventThrId;
   AIL_INT CreatedGraphicLabel;
   };

/* Function used to interactively define graphic for use as an ROI */
static AIL_INT DefineRectROI(AIL_ID SysId, AIL_ID ImageId, 
                      AIL_ID GraphicsContextId, AIL_ID GraphicsListId)
   {
   /* Create a synchronization event to indicate when graphic creation is done. */
   CreateGraphicInfo HookInfo;
   MthrAlloc(SysId, M_EVENT, M_NOT_SIGNALED+M_AUTO_RESET, M_NULL, M_NULL,
             &HookInfo.CreationDoneEventThrId);

   /* Hook a function to detect when the graphic's creation has ended,
      and pass an Aurora Imaging Library event to it, in order to notify the thread to resume. */
   MgraHookFunction(GraphicsListId, M_INTERACTIVE_GRAPHIC_STATE_MODIFIED,
                    &WaitUntilCreationEnds, &HookInfo);

   /* Set the graphics list's state to M_WAIT_FOR_CREATION. This allows the user 
      to add a graphic interactively to the list from the display. The graphic 
      is created by clicking in the display at the location you require one of 
      the rectangle's corners and dragging your cursor to the opposite corner. */
   MgraInteractive(GraphicsContextId, GraphicsListId, M_GRAPHIC_TYPE_RECT, 
                   M_DEFAULT, M_AXIS_ALIGNED_RECT);

   /* Wait for the graphic creation to end. The function hooked to the event will 
      set the event state to M_SIGNALED to indicate that graphic creation has ended. */
   MthrWait(HookInfo.CreationDoneEventThrId, M_EVENT_WAIT, M_NULL);

   /* Free allocated resources. */
   MthrFree(HookInfo.CreationDoneEventThrId);

   /* Get the label of the newly created rectangle. Note that MgraInquireList() with 
      M_LAST_LABEL might not work at this point, since the graphic might have been created
      in a separate thread. The call to retrieve the graphic's label is performed in the 
      hooked function, after the graphic is added to the list.*/
   AIL_INT RectangleLabel = HookInfo.CreatedGraphicLabel;

   /* Use the user-defined rectangle to define the region of interest for later processing.*/
   MbufSetRegion(ImageId, GraphicsListId, M_DEFAULT, M_RASTERIZE+M_FILL_REGION, M_DEFAULT);

   /* Return the label of the rectangle to the main function */
   return RectangleLabel;
   }
```
