---
title: "MdigGrab"
description: "This program demonstrates how to grab from a camera in continuous and monoshot mode."
ms.language: "Python"
---

# MdigGrab

> This program demonstrates how to grab from a camera in continuous and monoshot mode.

**Language:** Python

**Functions used:** `MdispAlloc`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
# File name: MdigGrab.py
#
# Synopsis:  This program demonstrates how to grab from a camera in
#            continuous and monoshot mode.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#########################################################################################

import ail11 as AIL

# Main function.
def MdigGrabExample():
   
   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay, AilDigitizer, AilImage = AIL.MappAllocDefault(AIL.M_DEFAULT)

   # Grab continuously.
   AIL.MdigGrabContinuous(AilDigitizer, AilImage)

   # When a key is pressed, halt. 
   print("\nDIGITIZER ACQUISITION:")
   print("----------------------\n")
   print("Continuous image grab in progress.")
   print("Press any key to stop.\n")
   AIL.MosGetch()

   # Stop continuous grab
   AIL.MdigHalt(AilDigitizer)

   # Pause to show the result. 
   print("Continuous grab stopped.\n")
   print("Press any key to do a single image grab.\n")
   AIL.MosGetch()

   # Monoshot grab. 
   AIL.MdigGrab(AilDigitizer, AilImage)

   # Pause to show the result. 
   print("Displaying the grabbed image.")
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free defaults.
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AilImage)

   return 0

if __name__ == "__main__":
   MdigGrabExample()
```
