---
title: "MdigGrabSequence"
description: "This example shows how to grab a sequence and archive it in real-time."
ms.language: "Python"
---

# MdigGrabSequence

> This example shows how to grab a sequence and archive it in real-time.

**Language:** Python

**Functions used:** `MbufImportSequence`, `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufExportSequence`, `MbufFree`, `MbufControl`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
# File name: MDigGrabSequence.py
#
# Synopsis:  This example shows how to record a sequence and play it back in 
#            real time. the acquisition can be done in memory only or to 
#            an AVI file. 
#
# NOTE:      This example assumes that the hard disk is sufficiently fast 
#            to keep up with the grab. Also, removing the sequence display or 
#            the text annotation while grabbing will reduce the CPU usage and
#            might help if some frames are missed during acquisition. 
#            If the disk or system are not fast enough, choose the option to 
#            record uncompressed images to memory.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#########################################################################################

import ail11 as AIL

# Sequence file name.
SEQUENCE_FILE = AIL.M_TEMP_DIR + "AilSequence.avi"

# Quantization factor to use during the compression.
# Valid values are 1 to 99 (higher to lower quality). 
COMPRESSION_Q_FACTOR = 50

# Annotation flag. Set to M_YES to draw the frame number in the saved image. 
FRAME_NUMBER_ANNOTATION = AIL.M_YES

# Maximum number of images for the multiple buffering grab. 
NB_GRAB_IMAGE_MAX = 20

# User's record function hook data structure. 
class HookDataStruct:
   def __init__(self, AilSystem, AilDisplay, AilImageDisp, AilCompressedImage, SaveSequenceToDisk, NbGrabbedFrames):
      self.AilSystem = AilSystem
      self.AilDisplay = AilDisplay
      self.AilImageDisp = AilImageDisp
      self.AilCompressedImage = AilCompressedImage
      self.NbGrabbedFrames = NbGrabbedFrames
      self.SaveSequenceToDisk = SaveSequenceToDisk

# Main function.
def MdigGrabSequenceExample():
   AilGrabImages = []
   AilCompressedImage = AIL.M_NULL
   TimeWait = 0.0

   # Allocate defaults.
   AilApplication, AilSystem, AilDisplay, AilDigitizer = AIL.MappAllocDefault(AIL.M_DEFAULT, ImageBufIdPtr=AIL.M_NULL)
   
   # Allocate an image and display it. 
   AilImageDisp = AIL.MbufAllocColor(AilSystem,
                                     AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND),
                                     AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X),
                                     AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y),
                                     8 + AIL.M_UNSIGNED,
                                     AIL.M_IMAGE + AIL.M_GRAB + AIL.M_DISP)
   AIL.MbufClear(AilImageDisp, 0)
   AIL.MdispSelect(AilDisplay, AilImageDisp)

   # Grab continuously on display.
   AIL.MdigGrabContinuous(AilDigitizer, AilImageDisp)

   # Print a message
   print("\nSEQUENCE ACQUISITION:")
   print("--------------------\n")

   # Inquire licenses.
   AilRemoteApplication = AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION)
   LicenseModules = AIL.MappInquire(AilRemoteApplication, AIL.M_LICENSE_MODULES)

   # Ask for the sequence format. 
   print("Choose the sequence format:")
   print("1) Uncompressed images to memory (up to {NB_GRAB_IMAGE_MAX} frames).".format(NB_GRAB_IMAGE_MAX=NB_GRAB_IMAGE_MAX))
   print("2) Uncompressed images to an AVI file.")
   if LicenseModules & (AIL.M_LICENSE_JPEGSTD | AIL.M_LICENSE_JPEG2000):
      if LicenseModules & AIL.M_LICENSE_JPEGSTD:
         print("3) Compressed lossy JPEG images to an AVI file.")
      if LicenseModules & AIL.M_LICENSE_JPEG2000:
         print("4) Compressed lossy JPEG2000 images to an AVI file.")

   ValidSelection = False
   while (not ValidSelection):
      Selection = chr(AIL.MosGetch())
      ValidSelection = True
      # Set the buffer attribute.
      if Selection == "1" or Selection == "\r":
         print("\nUncompressed images to memory selected.")
         CompressAttribute = AIL.M_NULL
         SaveSequenceToDisk = AIL.M_NO

      elif Selection == "2":
         print("\nUncompressed images to file selected.")
         CompressAttribute = AIL.M_NULL
         SaveSequenceToDisk = AIL.M_YES
   
      elif Selection == "3":
         print("\nJPEG images to file selected.")
         CompressAttribute = AIL.M_COMPRESS + AIL.M_JPEG_LOSSY
         SaveSequenceToDisk = AIL.M_YES

      elif Selection == "4":
         print("\nJPEG 2000 images to file selected.")
         CompressAttribute = AIL.M_COMPRESS + AIL.M_JPEG2000_LOSSY
         SaveSequenceToDisk = AIL.M_YES

      else:
         print("\nInvalid selection !.")
         ValidSelection = False
   
   # Allocate a compressed buffer if required.
   if CompressAttribute:
      AilCompressedImage = AIL.MbufAllocColor(AilSystem,
                                              AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND),
                                              AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X),
                                              AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y),
                                              8 + AIL.M_UNSIGNED, 
                                              AIL.M_IMAGE + CompressAttribute)
      AIL.MbufControl(AilCompressedImage, AIL.M_Q_FACTOR, COMPRESSION_Q_FACTOR)

   # Allocate the grab buffers to hold the sequence buffering.
   NbFrames = 0
   for n in range(0, NB_GRAB_IMAGE_MAX):
      if (n == 2):
         AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE)
      AilGrabImages.append(AIL.MbufAllocColor(AilSystem,
                                              AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND),
                                              AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X),
                                              AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y),
                                              8 + AIL.M_UNSIGNED, 
                                              AIL.M_IMAGE + AIL.M_GRAB))
      if AilGrabImages[n]:
         NbFrames += 1
         AIL.MbufClear(AilGrabImages[n], 0xFF)
      else:
         break
   
   AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE)

   # Halt continuous grab.
   AIL.MdigHalt(AilDigitizer)

   # If the sequence must be saved to disk.
   if SaveSequenceToDisk:
      # Open the AVI file if required. 
      print("\nSaving the sequence to an AVI file...")
      AIL.MbufExportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_OPEN)
   else:
      print("\nSaving the sequence to memory...\n")
   
   # Initialize User's archiving function hook data structure. 
   UserHookData = HookDataStruct(AilSystem, AilDisplay, AilImageDisp, AilCompressedImage, SaveSequenceToDisk, 0)

   # Acquire the sequence. The processing hook function will
   # be called for each image grabbed to record and display it. 
   # If sequence is not saved to disk, stop after NbFrames.
   RecordFunctionPtr = AIL.AIL_DIG_HOOK_FUNCTION_PTR(RecordFunction) 
   AIL.MdigProcess(AilDigitizer, AilGrabImages, NbFrames, AIL.M_START if SaveSequenceToDisk else AIL.M_SEQUENCE, AIL.M_DEFAULT, RecordFunctionPtr, UserHookData)

   # Wait for a key press.
   if SaveSequenceToDisk:
      print("\nPress any key to stop recording.\n")
      AIL.MosGetch()

   # Wait until we have at least 2 frames to avoid an invalid frame rate. 
   while True:
      FrameCount = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT)
      
      if FrameCount >= 2:
         break
   
   # Stop the sequence acquisition. 
   AIL.MdigProcess(AilDigitizer, AilGrabImages, NbFrames, AIL.M_STOP, AIL.M_DEFAULT, RecordFunctionPtr, UserHookData)
   
   # Read and print final statistics. 
   FrameCount = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT)
   FrameRate = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_RATE)
   FrameMissed = AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_MISSED)
   print("\n\n{NbFrames} frames recorded ({FrameMissed} missed), at {FrameRate} frames/sec ({FrameRateMS} ms/frame).\n"
         .format(NbFrames=UserHookData.NbGrabbedFrames, FrameMissed=FrameMissed, FrameRate=round(FrameRate, 1), FrameRateMS=round(1000.0/FrameRate, 1)))

   # Sequence file closing if required.
   if SaveSequenceToDisk:
      AIL.MbufExportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, FrameRate, AIL.M_CLOSE)

   # Wait for a key to playback. 
   print("Press any key to start the sequence playback.")
   AIL.MosGetch()

   # Playback the sequence until a key is pressed.
   if UserHookData.NbGrabbedFrames > 0:
      KeyPressed = 0

      while True:
         # If sequence must be loaded. 
         if SaveSequenceToDisk:
            # Inquire information about the sequence. 
            print("\nPlaying sequence from the AVI file...")
            print("Press any key to end playback.\n")
            FrameCount = AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES)
            FrameRate = AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_FRAME_RATE)
            CompressAttribute = AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_COMPRESSION_TYPE)

            # Open the sequence file.
            AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_OPEN)
         else:
            print("\nPlaying sequence from memory...\n")

         # Copy the images to the screen respecting the sequence frame rate.
         TotalReplay = 0.0
         NbFramesReplayed = 0
         for n in range(0, FrameCount):
            # Reset the time.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET, AIL.M_NULL)

            # If image was saved to disk.
            if SaveSequenceToDisk:
               # Load image directly to the display.
               AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, AilImageDisp, n, 1, AIL.M_READ)
            else:
               # Copy the grabbed image to the display.
               AIL.MbufCopy(AilGrabImages[n], AilImageDisp)
            
            NbFramesReplayed += 1
            print("Frame #{NbFramesReplayed}             \r".format(NbFramesReplayed=NbFramesReplayed), end='')

            # Check for a pressed key to exit.
            if AIL.MosKbhit() and (n >= (NB_GRAB_IMAGE_MAX - 1)):
               AIL.MosGetch()
               break

            # Wait to have a proper frame rate.
            TimeWait = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ)
            TotalReplay += TimeWait
            TimeWait = (1/FrameRate) - TimeWait
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, TimeWait)
            TotalReplay += TimeWait if (TimeWait > 0.0) else 0.0
         
         # Close the sequence file.
         if SaveSequenceToDisk:
            AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_CLOSE)

         # Print statistics.
         print("\n\n{NbFramesReplayed} frames replayed, at a frame rate of {FrameRate} frames/sec ({FrameRateMS} ms/frame).\n"
               .format(NbFramesReplayed=NbFramesReplayed, FrameRate=round(n/TotalReplay, 1), FrameRateMS=round(1000.0*TotalReplay/n, 1)))
         print("Press <Enter> to end (or any other key to playback again).")
         
         KeyPressed = chr(AIL.MosGetch())

         if KeyPressed == '\r' or KeyPressed == '\n':
            break
   
   # Free all allocated buffers.
   AIL.MbufFree(AilImageDisp)
   for n in range(0, NbFrames):
      AIL.MbufFree(AilGrabImages[n])
   if AilCompressedImage:
      AIL.MbufFree(AilCompressedImage)
   
   # Free defaults.
   AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AIL.M_NULL)

   return 0

# User's record function called each time a new buffer is grabbed. #
# ---------------------------------------------------------------- #

# Local defines for the annotations.
STRING_LENGTH_MAX = 20
STRING_POS_X      = 20
STRING_POS_Y      = 20

def RecordFunction(HookType, HookId, HookDataPtr):
   UserHookDataPtr = HookDataPtr
   ModifiedImage = 0
   n = 0
   
   # Retrieve the AIL_ID of the grabbed buffer.
   ModifiedImage = AIL.MdigGetHookInfo(HookId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID)

   # Increment the frame count.
   UserHookDataPtr.NbGrabbedFrames += 1
   print("Frame #{FrameNumber}\r".format(FrameNumber=UserHookDataPtr.NbGrabbedFrames), end='')

   # Draw the frame count in the image if enabled.
   if FRAME_NUMBER_ANNOTATION == AIL.M_YES:
      AIL.MgraText(AIL.M_DEFAULT, ModifiedImage, STRING_POS_X, STRING_POS_Y, str(UserHookDataPtr.NbGrabbedFrames))

   # Copy the new grabbed image to the display.
   AIL.MbufCopy(ModifiedImage, UserHookDataPtr.AilImageDisp)
   
   # Compress the new image if required. 
   if UserHookDataPtr.AilCompressedImage != AIL.M_NULL:
      AIL.MbufCopy(ModifiedImage, UserHookDataPtr.AilCompressedImage)
   
   # Record the new image. 
   ImageToExport = UserHookDataPtr.AilCompressedImage if UserHookDataPtr.AilCompressedImage != AIL.M_NULL else ModifiedImage
   if UserHookDataPtr.SaveSequenceToDisk:
      AIL.MbufExportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, [ImageToExport],
                             1, AIL.M_DEFAULT, AIL.M_WRITE)
   
   return 0
      

if __name__ == "__main__":
   MdigGrabSequenceExample()

```
