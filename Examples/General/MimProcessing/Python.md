---
title: "MimProcessing"
description: "This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample."
ms.language: "Python"
---

# MimProcessing

> This program uses different image processing primitives to automatically binarize an image and to determine the number of cell nuclei that are larger than a certain size in an image of a tissue sample.

**Language:** Python

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
#            it binarizes two images to isolate specific zones.
#            Under Aurora Imaging Library full, it also uses different image processing primitives 
#            to determine the number of cell nuclei that are larger than a 
#            certain size and show them in pseudo-color.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
 
import ail11 as AIL

# Target image file specifications.
IMAGE_FILE = AIL.M_IMAGE_PATH + "Cell.mbufi"
IMAGE_CUP  = AIL.M_IMAGE_PATH + "PlasticCup.mim"
IMAGE_SMALL_PARTICLE_RADIUS = 1

def ExtractParticlesExample(AilApplication, AilSystem, AilDisplay):
   # Restore source image and display it.
   AilImage = AIL.MbufRestore(IMAGE_FILE, AilSystem)
   AIL.MdispSelect(AilDisplay, AilImage)
   
   # Pause to show the original image..
   print("\n1) Particles extraction:")
   print("-----------------\n")
   print("This first example extracts the dark particles in an image.")
   print("Press any key to continue.\n")
   AIL.MosGetch()
   
   # Binarize the image with an automatically calculated threshold so that 
   # particles are represented in white and the background removed.
   AIL.MimBinarize(AilImage, AilImage, AIL.M_BIMODAL + AIL.M_LESS_OR_EQUAL, AIL.M_NULL, AIL.M_NULL)
   
   # Print a message for the extracted particles.
   print("These particles were extracted from the original image.")
   
   # If IM module is available, count and label the larger particles.
   AilRemoteApplication = AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION)
   LicenseModules = AIL.MappInquire(AilRemoteApplication, AIL.M_LICENSE_MODULES)
   
   if (LicenseModules & AIL.M_LICENSE_IM) != 0:
      # Pause to show the extracted particles.
      print("Press any key to continue.\n")
      AIL.MosGetch()
      
      # Close small holes
      AIL.MimClose(AilImage, AilImage, IMAGE_SMALL_PARTICLE_RADIUS, AIL.M_BINARY)
      
      # Remove small particles.
      AIL.MimOpen(AilImage, AilImage, IMAGE_SMALL_PARTICLE_RADIUS, AIL.M_BINARY)
      
      # Label the image.
      AIL.MimLabel(AilImage, AilImage, AIL.M_DEFAULT)
      
      # The largest label value corresponds to the number of particles in the image.
      AilExtremeResult = AIL.MimAllocResult(AilSystem, 1, AIL.M_EXTREME_LIST)
      AIL.MimFindExtreme(AilImage, AilExtremeResult, AIL.M_MAX_VALUE)

      MaxLabelNumber = AIL.MimGetResult(AilExtremeResult, AIL.M_VALUE)[0]

      AIL.MimFree(AilExtremeResult)
      
      # Multiply the labeling result to augment the gray level of the particles.
      AIL.MimArith(AilImage, int(256 / float(MaxLabelNumber)), AilImage, AIL.M_MULT_CONST)

      # Display the resulting particles in pseudo-color.
      AIL.MdispLut(AilDisplay, AIL.M_PSEUDO)
      
      # Print results.
      print("There were {MaxLabelNumber} large particles in the original image.".format(MaxLabelNumber = int(MaxLabelNumber)))
      
   
   # Pause to show the result
   print("Press any key to continue.\n")
   AIL.MosGetch()
   
   AIL.MdispLut(AilDisplay, AIL.M_DEFAULT)

   # Free all allocations.
   AIL.MbufFree(AilImage)
   
def ExtractForegroundExample(AilApplication, AilSystem, AilDisplay):
   # Restore source image and display it.
   AilImage = AIL.MbufRestore(IMAGE_CUP, AilSystem)
   AIL.MdispSelect(AilDisplay, AilImage)
   
   # Pause to show the original image..
   print("\n2) Background removal:")
   print("-----------------\n")
   print("This second example separates a cup on a table from the background using MimBinarize() with an M_DOMINANT mode.")
   print("In this case, the dominant mode (black background) is separated from the rest. Note, using an M_BIMODAL mode")
   print("would give another result because the background and the cup would be considered as the same mode.")
   print("Press any key to continue.\n")
   AIL.MosGetch()
   
   # Binarize the image with an automatically calculated threshold so that
   # cup and table are represented in white and the background removed
   AIL.MimBinarize(AilImage, AilImage, AIL.M_DOMINANT + AIL.M_LESS_OR_EQUAL, AIL.M_NULL, AIL.M_NULL)
   
   # Print a message for the extracted cup and table
   print("The cup and the table were separated from the background with M_DOMINANT mode of MimBinarize.")
   
   # Pause to show the result
   print("Press any key to end.\n")
   AIL.MosGetch()

   # Free all allocations.
   AIL.MbufFree(AilImage)

def MimProcessingExample():
   # Allocate a default application, system, display and image.
   AilApplication, AilSystem, AilDisplay = AIL.MappAllocDefault(AIL.M_DEFAULT, DigIdPtr=AIL.M_NULL, ImageBufIdPtr=AIL.M_NULL)

   # Show header
   print("\nIMAGE PROCESSING:")
   print("-----------------\n")
   print("This program shows two image processing examples.")
   
   # Example about extracting particles in an image
   ExtractParticlesExample(AilApplication, AilSystem, AilDisplay)
   
   # Example about isolating objects from the background in an image
   ExtractForegroundExample(AilApplication, AilSystem, AilDisplay)
   
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL)
   
   return 0

if __name__ == "__main__":
   MimProcessingExample()

```
