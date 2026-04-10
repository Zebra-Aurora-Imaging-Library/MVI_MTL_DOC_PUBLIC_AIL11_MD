---
title: "MimProcessing"
description: "This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample."
ms.language: "Python Object"
---

# MimProcessing

> This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample.

**Language:** Python Object

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispLut`, `MdispSelect`, `MimAllocResult`, `MimArith`, `MimBinarize`, `MimClose`, `MimFindExtreme`, `MimFree`, `MimGetResult`, `MimLabel`, `MimOpen`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Pharmaceutical/Medical, Applications, Counting, Finding/Locating, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
##########################################################################
#
# File name: MimProcessing.py
#
# Synopsis:  This program show the usage of image processing. Under Aurora Imaging Library lite,
#            it binarizes the image of a tissue sample to show dark particles.
#            Under Aurora Imaging Library full, it also uses different image processing primitives
#            to determine the number of cell nuclei that are larger than a
#            certain size and show them in pseudo-color.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
##########################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MimProcessingExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._graContext = Gra.Context(self._system)
      self._display = Disp.Display(self._system)
      self._extremeList = None
      self._imageDisp = None

      self._imageFile = Paths.Images + "Cell.mbufi"
      self._imageCup = Paths.Images + "PlasticCup.mim"
      self._imageSmallParticleRadius = 1

   def __enter__(self):
      return self

   def Run(self):
      # Show header.
      print("\nIMAGE PROCESSING:")
      print("-----------------\n")
      print("This program shows two image processing examples.")

      # Example about extracting particles in an image.
      self.ExtractParticlesExample()

      # Example about isolating objects from the background in an image.
      self.ExtractForegroundExample()

   def ExtractParticlesExample(self):
      # Restore source image and display it.
      self._imageDisp = Buf.Image(self._imageFile, self._system)
      self._display.Select(self._imageDisp)

      # Pause to show the original image.
      print("\n1) Particles extraction:")
      print("-----------------\n")
      print("This first example extracts the dark particles in an image.")
      print("Press any key to continue.\n")
      Os.Getch()

      # Binarize the image with an automatically calculated threshold so that
      # particles are represented in white and the background removed.
      Im.Binarize(self._imageDisp, self._imageDisp, Im.BinarizeConditionAndThreshModes.LessOrEqual | Im.BinarizeConditionAndThreshModes.Bimodal, 0, 0)

      # Print a message for the extracted particles.
      print("These particles were extracted from the original image.")

      # If IM module is available, count and label the larger particles.
      licenseModules = self._application.LicenseModules

      if ((licenseModules & App.LicenseModules.Im) != 0):
         # Pause to show the extracted particles.
         print("Press any key to continue.\n")
         Os.Getch()

         # Close small holes.
         Im.Close(self._imageDisp, self._imageDisp, self._imageSmallParticleRadius, Im.CloseProcMode.Binary)

         # Remove small particles.
         Im.Open(self._imageDisp, self._imageDisp, self._imageSmallParticleRadius, Im.OpenProcMode.Binary)

         # Label the image.
         Im.Label(self._imageDisp, self._imageDisp, Im.LabelProcModes.M8Connected | Im.LabelProcModes.Grayscale)

         # The largest label value corresponds to the number of particles in the image.
         self._extremeList = Im.ExtremeList(self._system, 1)
         Im.FindExtreme(self._imageDisp, self._extremeList, Im.FindExtremeExtremeTypes.MaxValue)
         maxLabelNumber = self._extremeList.Values[0]

         # Multiply the labeling result to augment the gray level of the particles.
         Im.Arith.Multiply(self._imageDisp, (256/maxLabelNumber), self._imageDisp, False)

         # Display the resulting particles in pseudo-color.
         self._display.Lut = Buf.Lut.PseudoLut

         # Print results.
         print("There were {MaxLabelNumber} large particles in the original image.".format(MaxLabelNumber = int(maxLabelNumber)))

      # Pause to show the result.
      print("Press any key to continue.\n")
      Os.Getch()

      # Reset display LUT to default.
      self._display.Lut = Buf.Lut.DefaultLut
      
      # Free all allocations.
      self._imageDisp.Free() if self._imageDisp is not None else None

   def ExtractForegroundExample(self):
      # Restore source image and display it.
      self._imageDisp = Buf.Image(self._imageCup, self._system)
      self._display.Select(self._imageDisp)

      # Pause to show the original image.
      print("\n2) Background removal:")
      print("-----------------\n")
      print("This second example separates a cup on a table from the background using MimBinarize() with an M_DOMINANT mode.")
      print("In this case, the dominant mode (black background) is separated from the rest. Note, using an M_BIMODAL mode")
      print("would give another result because the background and the cup would be considered as the same mode.")
      print("Press any key to continue.\n")
      Os.Getch()

      # Binarize the image with an automatically calculated threshold so that
      # particles are represented in white and the background removed.
      Im.Binarize(self._imageDisp, self._imageDisp, Im.BinarizeConditionAndThreshModes.Dominant | Im.BinarizeConditionAndThreshModes.LessOrEqual, 0, 0)

      # Print a message for the extracted cup and table.
      print("The cup and the table were separated from the background with M_DOMINANT mode of MimBinarize.")

      # Pause to show the result.
      print("Press any key to end.\n")
      Os.Getch()

      # Free all allocations.
      self._imageDisp.Free() if self._imageDisp is not None else None

   def __exit__(self, exc_type, exc_value, tb):
      self._extremeList.Free() if self._extremeList is not None else None
      self._display.Free() if self._display is not None else None
      self._graContext.Free() if self._graContext is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MimProcessingExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
