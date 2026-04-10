---
title: "Mdmr"
description: "This program uses the SureDotOCR® module to read a product expiry date and lot number printed using a CIJ printer."
ms.language: "Python"
---

# Mdmr

> This program uses the SureDotOCR® module to read a product expiry date and lot number printed using a CIJ printer.

**Language:** Python

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispInquire`, `MdispSelect`, `MdmrAlloc`, `MdmrAllocResult`, `MdmrControl`, `MdmrControlStringModel`, `MdmrDraw`, `MdmrFree`, `MdmrGetResult`, `MdmrImportFont`, `MdmrPreprocess`, `MdmrRead`, `MgraText`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Packaging, Applications, Reading, Modules, Buffer, Display, Graphics, SureDotOCR®, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: Mdmr.py
#
# Synopsis:  This program uses the Dot Matrix Reader (SureDotOCR®) module to
#            read a product expiry date and lot number printed using a
#            CIJ printer.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

 
import ail11 as AIL
import os

# File specifications. 
IMAGE_FILE_TO_READ   = os.path.join(AIL.M_IMAGE_PATH, "ExpiryDateAndLot.mim")
FONT_FILE_TO_IMPORT  = os.path.join(AIL.M_CONTEXT_PATH, "ExpiryDateAndLotFont5x7.mdmrf")

# Dot Matrix Reader settings. 
STRING_DOT_DIAMETER = 6.0
TEXT_BLOCK_WIDTH    = 400
TEXT_BLOCK_HEIGHT   = 100
EXPIRY_DATE_LENGTH  = 7
LOT_NUMBER_LENGTH   = 7

# Util. 
TEXT_MAX_SIZE = 128


def MdmrExample():
   # Print module name.
   print("\nDOT MATRIX READER (SureDotOCR) MODULE:")
   print("--------------------------------------\n")

   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Restore the font definition image. 
   AilImage = AIL.MbufRestore(IMAGE_FILE_TO_READ, AilSystem)

   # Display the image and prepare for overlay annotations. 
   AIL.MdispSelect(AilDisplay, AilImage)
   AIL.MdispControl(AilDisplay, AIL.M_OVERLAY, AIL.M_ENABLE)
   AilOverlay = AIL.MdispInquire(AilDisplay, AIL.M_OVERLAY_ID)

   # Allocate a new empty Dot Matrix Reader context. 
   AilDmrContext = AIL.MdmrAlloc(AilSystem, AIL.M_DOT_MATRIX, AIL.M_DEFAULT)

   # Allocate a new empty Dot Matrix Reader result buffer. 
   AilDmrResult = AIL.MdmrAllocResult(AilSystem, AIL.M_DOT_MATRIX, AIL.M_DEFAULT)

   # Import a Dot Matrix font. 
   AIL.MdmrImportFont(FONT_FILE_TO_IMPORT, AIL.M_DMR_FONT_FILE, AilDmrContext, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_DEFAULT)

   # Add a new string model definition for the product lot number. 
   # ------------------------------------------------------------- 
   AIL.MdmrControl(AilDmrContext, AIL.M_STRING_ADD, AIL.M_DEFAULT)

   # Set the string model rank and size. 
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(0), AIL.M_DEFAULT, AIL.M_STRING_RANK,      1, AIL.M_DEFAULT, AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(0), AIL.M_DEFAULT, AIL.M_STRING_SIZE_MIN_MAX, LOT_NUMBER_LENGTH, LOT_NUMBER_LENGTH, AIL.M_NULL)

   # Add a new string model definition for the expiry date (YYYY MM DD). 
   # ------------------------------------------------------------------- 
   AIL.MdmrControl(AilDmrContext, AIL.M_STRING_ADD, AIL.M_DEFAULT)

   # Set the string model rank and size. 
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_DEFAULT, AIL.M_STRING_RANK,     0, AIL.M_DEFAULT, AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_DEFAULT, AIL.M_STRING_SIZE_MIN_MAX, EXPIRY_DATE_LENGTH, EXPIRY_DATE_LENGTH, AIL.M_NULL)

   # Set the string model constraint for an expiry date (DDMMMYY). 
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(0), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS,            AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(1), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS,            AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(2), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_LETTERS_UPPERCASE, AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(3), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_LETTERS_UPPERCASE, AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(4), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_LETTERS_UPPERCASE, AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(5), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS,            AIL.M_NULL)
   AIL.MdmrControlStringModel(AilDmrContext, AIL.M_STRING_INDEX(1), AIL.M_POSITION_IN_STRING(6), AIL.M_ADD_PERMITTED_CHARS_ENTRY, AIL.M_FONT_LABEL(AIL.M_ANY), AIL.M_DIGITS,            AIL.M_NULL)

   print("A Dot Matrix Reader (SureDotOCR) context was set up to read")
   print("an expiry date and a lot number from a target image.\n")

   # Set the Dot Matrix dot diameter. 
   AIL.MdmrControl(AilDmrContext, AIL.M_DOT_DIAMETER, STRING_DOT_DIAMETER)

   # Set the maximum size of the string box. 
   AIL.MdmrControl(AilDmrContext, AIL.M_TEXT_BLOCK_WIDTH,  TEXT_BLOCK_WIDTH)
   AIL.MdmrControl(AilDmrContext, AIL.M_TEXT_BLOCK_HEIGHT, TEXT_BLOCK_HEIGHT)

   # Preprocess the context. 
   AIL.MdmrPreprocess(AilDmrContext, AIL.M_DEFAULT)

   # Reading the string from a target image. 
   AIL.MdmrRead(AilDmrContext, AilImage, AilDmrResult, AIL.M_DEFAULT)

   # Get number of strings read and show the result. 
   NumberOfStrings = AIL.MdmrGetResult(AilDmrResult, AIL.M_GENERAL, AIL.M_DEFAULT, AIL.M_STRING_NUMBER + AIL.M_TYPE_AIL_INT)

   # Draw the read result. 
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_GREEN)
   AIL.MdmrDraw(AIL.M_DEFAULT, AilDmrResult, AilOverlay, AIL.M_DRAW_STRING_CHAR_BOX, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)
   AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, AIL.M_COLOR_CYAN)
   AIL.MdmrDraw(AIL.M_DEFAULT, AilDmrResult, AilOverlay, AIL.M_DRAW_STRING_CHAR_POSITION, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT)

   if NumberOfStrings > 0:
      print("Result: {NumberOfStrings} strings have been read:\n".format(NumberOfStrings=NumberOfStrings))

      for Index in range(NumberOfStrings):
         # Print the read result. 
         StringModelIndex = AIL.MdmrGetResult(AilDmrResult, Index, AIL.M_GENERAL, AIL.M_STRING_MODEL_INDEX + AIL.M_TYPE_AIL_INT)
         StringResult = AIL.MdmrGetResult(AilDmrResult, Index, AIL.M_GENERAL, AIL.M_STRING)


         if StringModelIndex == 0:
            PrintText = " LOT# : " + StringResult + " "
            AIL.MgraText(AIL.M_DEFAULT, AilOverlay, 20, 20 + Index * 20, PrintText)
            print(" LOT# : {StringResult}".format(StringResult=StringResult))
         elif StringModelIndex == 1:
            PrintText = " EXP. : " + StringResult + " "
            AIL.MgraText(AIL.M_DEFAULT, AilOverlay, 20, 20 + Index * 20, PrintText)
            print(" EXPIRY DATE: {StringResult}".format(StringResult=StringResult))
         else:
            print("Unexpected model index")
   else:
      print("Error: no string found.")

   # Pause to show results. 
   print("\nPress any key to end.\n")
   AIL.MosGetch()

   # Free all allocations. 
   AIL.MdmrFree(AilDmrContext)
   AIL.MdmrFree(AilDmrResult)
   AIL.MbufFree(AilImage)

   # Free defaults. 
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)
   
   return 0


if __name__ == "__main__":
   MdmrExample()

```
