---
title: "MdigGrabSequence"
description: "This example shows how to grab a sequence and archive it in real-time."
ms.language: "Python Object"
---

# MdigGrabSequence

> This example shows how to grab a sequence and archive it in real-time.

**Language:** Python Object

**Functions used:** `MbufImportSequence`, `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufExportSequence`, `MbufFree`, `MbufControl`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
#########################################################################################
#
# File name : MDigGrabSequence.py
#
# Content   :  This example shows how to record a sequence and play it back in
#              real time. the acquisition can be done in memory only or to
#              an AVI file.
#
# NOTE      :      This example assumes that the hard disk is sufficiently fast
#                  to keep up with the grab. Also, removing the sequence display or
#                  the text annotation while grabbing will reduce the CPU usage and
#                  might help if some frames are missed during acquisition.
#                  If the disk or system are not fast enough, choose the option to
#                  record uncompressed images to memory.
#
# (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
# All Rights Reserved
#
#########################################################################################

from ZebraAuroraImagingObjectLibrary11 import *

class MdigGrabSequenceExample(object):

   def __init__(self):
      self._application = App.Application(App.AllocInitFlags.Default)
      self._system = Sys.System(self._application)
      self._digitizer = Dig.Digitizer(self._system)
      self._imageDisp = Buf.Image(self._system, self._digitizer.SizeBand, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.ProcDispGrab)
      self._compressedImage = Buf.CompressedImage()
      self._display = Disp.Display(self._system)
      self._graphicContext = Gra.Context(self._system)
      self._images = []

      self._sequenceExporter = None
      self._sequenceImporter = None

      # Quantization factor to use during the compression.
      # Valid values are 1 to 99 (higher to lower quality).
      self._compressionQFactor = 50

      self._sequenceFile = Paths.Temp + "AilSequence.avi"
      self._nbGrabImageMax = 20
      self._nbGrabbedFrames = 0
      self._nbArchivedFrames = 0
      self._saveToDisk = True

   def __enter__(self):
      self._imageDisp.Clear(Color8.Black)
      self._display.Select(self._imageDisp)

      return self

   def ProcessingFunction(self, sender, e):
      try:
         STRING_POS_X = 20
         STRING_POS_Y = 20

         # Retrieve the AIL_ID of the grabbed buffer.
         modifiedImage = e.ModifiedBuffer

         # Increment the frame count.
         self._nbGrabbedFrames += 1
         print("Frame #{FrameNumber}             \r".format(FrameNumber=self._nbGrabbedFrames), end='')

         # Draw the frame count in the image if enabled.
         self._graphicContext.DrawString(modifiedImage, STRING_POS_X, STRING_POS_Y, str(self._nbGrabbedFrames))

         # Copy the new grabbed image to the display.
         Buf.Copy(modifiedImage, self._imageDisp)

         # Compress the new image if required.
         if self._compressedImage.IsAllocated:
            Buf.Copy(modifiedImage, self._compressedImage)

         # Record the new image.
         if self._saveToDisk:
            if self._compressedImage.IsAllocated:
               self._sequenceExporter.Write(self._compressedImage)
            else:
               self._sequenceExporter.Write(modifiedImage)
            self._nbArchivedFrames += 1

         return 0

      except:
         # User-defined event/processing functions should always handle their own exceptions
         return 1

   def Run(self):
      # Grab continuously on display.
      self._digitizer.GrabContinuous(self._imageDisp)

      # Print a message
      print("\nSEQUENCE ACQUISITION:")
      print("--------------------\n")

      licenseModules = self._system.OwnerApplication.LicenseModules

      print("Choose the sequence format:")
      print("1) Uncompressed images to memory (up to {numFrames} frames).".format(numFrames = self._nbGrabImageMax))
      print("2) Uncompressed images to an AVI file.")

      # If sequence is saved to disk, select between grabbing an
      # uncompressed, JPEG or JPEG2000 sequence.
      if(((licenseModules & (App.LicenseModules.Jpegstd | App.LicenseModules.Jpeg2000)) != 0)):
         if((licenseModules & App.LicenseModules.Jpegstd) != 0):
            print("3) Compressed lossy JPEG images to an AVI file.")
         if((licenseModules & App.LicenseModules.Jpeg2000) != 0):
            print("4) Compressed lossy JPEG2000 images to an AVI file.")

      compressAttribute = Buf.AllocAttributes.Null
      validSelection = False
      while not validSelection:
         selection = chr(Os.Getch())
         validSelection = True

         # Set the buffer attribute.
         if selection == '1' or selection == '\r':
            print("\nUncompressed images to memory selected.")
            compressAttribute = Buf.AllocAttributes.Null
            self._saveToDisk = False
         elif selection == '2':
            print("\nUncompressed images to file selected.")
            compressAttribute = Buf.AllocAttributes.Null
            self._saveToDisk = True
         elif selection == '3':
            print("\nJPEG images to file selected.")
            compressAttribute = Buf.AllocAttributes.Compress | Buf.AllocAttributes.JpegLossy
            self._saveToDisk = True
         elif selection == '4':
            print("\nJPEG 2000 images to file selected.")
            compressAttribute = Buf.AllocAttributes.Compress | Buf.AllocAttributes.Jpeg2000Lossy
            self._saveToDisk = True
         else:
            print("\nInvalid selection !.")
            validSelection = False

      if compressAttribute != Buf.AllocAttributes.Null:
         self._compressedImage.Alloc(self._system, self._digitizer.SizeBand, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, compressAttribute)
         self._compressedImage.QFactor = self._compressionQFactor

      # Allocate the grab buffers to hold the sequence buffering.
      self._application.ErrorGlobalMode = App.ErrorMode.PrintEnable;

      for n in range(self._nbGrabImageMax):
         if n == 2:
            self._application.ErrorGlobalMode = App.ErrorMode.PrintDisable;
         image = Buf.Image(self._system, self._digitizer.SizeBand, self._digitizer.SizeX, self._digitizer.SizeY, Buf.AllocType.Unsigned8, Buf.AllocAttributes.Grab)

         if image.IsAllocated:
            image.Clear(Color8.White)
            self._images.append(image)
         else:
            break

      self._application.ErrorGlobalMode = App.ErrorMode.Default;

      # Halt continuous grab.
      self._digitizer.Halt()

      # Open the AVI file if required.
      if self._saveToDisk:
         print("\nSaving the sequence to an AVI file...")
         self._sequenceExporter = Buf.SequenceExporter(self._sequenceFile, False)
      else:
         print("\nSaving the sequence to memory...\n")

      # Initialize User's archiving function hook data structure.
      self._digitizer.Process.Register(self.ProcessingFunction, self)
      if self._saveToDisk:
         self._digitizer.Process.Start(self._images)
         print("\nPress any key to stop recording.\n")
         Os.Getch()
      else:
         self._digitizer.Process.Sequence(self._images)

      # Wait until we have at least 2 frames to avoid an invalid frame rate.
      while(self._digitizer.ProcessFrameCount < 2):
         pass

      # Stop the sequence acquisition.
      self._digitizer.Process.Stop()
      self._digitizer.Process.UnRegister()

      # Read and print final statistics.
      frameCount = self._digitizer.ProcessFrameCount
      frameRate = self._digitizer.ProcessFrameRate
      frameMissed = self._digitizer.ProcessFrameMissed

      print("\n\n{processFrameCount} frames recorded ({frameMissed} missed), at {processFrameRate} frames/sec ({rate} ms/frame).\n"
            .format(processFrameCount=frameCount, frameMissed = frameMissed, processFrameRate=round(frameRate, 2), rate=round(1000.0 / frameRate, 2)))

      # Sequence file closing if required.
      if(self._saveToDisk):
         self._sequenceExporter.Close(frameRate)

      # Wait for a key to playback.
      print("Press any key to start the sequence playback.")
      Os.Getch()

      # Playback the sequence until a key is pressed.
      if self._nbGrabbedFrames > 0 :
         keyPressed = 0
         while (keyPressed != '\r') and (keyPressed != '\n'):
            # If sequence must be loaded.
            if self._saveToDisk:
               # Inquire information about the sequence.
               print("\nPlaying sequence from the AVI file...")
               print("Press any key to end playback.\n")

               fileInfo = App.FileInformation(self._sequenceFile)
               frameCount = fileInfo.NumberOfImages
               frameRate = fileInfo.FrameRate

               # Open the sequence file.
               self._sequenceImporter = Buf.SequenceImporter(self._sequenceFile)
            else:
               print("\nPlaying sequence from memory...\n")

            # Copy the images to the screen respecting the sequence frame rate.
            totalReplay = 0.0
            nbFramesReplayed = 0
            for n in range(frameCount):
               # Reset the time.
               self._application.Timer.Global.Reset()

               # If image was saved to disk.
               if self._saveToDisk:
                  # Load image directly to the display.
                  self._sequenceImporter.ReadLoad(self._imageDisp, n)
               else:
                  # Copy the grabbed image to the display.
                  Buf.Copy(self._images[n], self._imageDisp)

               nbFramesReplayed += 1
               print("Frame #{FrameNumber}             \r".format(FrameNumber=nbFramesReplayed), end='')

               # Check for a pressed key to exit.
               if(Os.Kbhit() and (n >= (self._nbGrabImageMax - 1))):
                  Os.Getch()
                  break

               # Wait to have a proper frame rate.
               timeWait = self._application.Timer.Global.Read()
               totalReplay += timeWait
               timeWait = (1 / frameRate) - timeWait
               self._application.Timer.Global.Wait(timeWait)
               totalReplay += timeWait if timeWait > 0 else 0

            # Close the sequence file.
            if(self._saveToDisk):
               self._sequenceImporter.Close()

            # Print statistics.
            print("\n\n{processFrameCount} frames replayed, at a frame rate of {processFrameRate} frames/sec ({rate} ms/frame).\n"
                  .format(processFrameCount=nbFramesReplayed, processFrameRate = round(n/totalReplay, 1), rate=round(1000.0*totalReplay/n, 1)))
            print("Press <Enter> to end (or any other key to playback again).\n")
            keyPressed = chr(Os.Getch())

   def __exit__(self, exc_type, exc_value, tb):
      for image in self._images:
         image.Free()

      self._imageDisp.Free() if self._imageDisp is not None else None
      self._compressedImage.Free() if self._compressedImage is not None else None
      self._display.Free() if self._display is not None else None
      self._graphicContext.Free() if self._graphicContext is not None else None
      self._digitizer.Free() if self._digitizer is not None else None
      self._system.Free() if self._system is not None else None
      self._application.Free() if self._application is not None else None

if __name__ == "__main__":
   try:
      with MdigGrabSequenceExample() as example:
         example.Run()
   except AIOLException as exception:
      print("Encountered an exception during the example.")
      print(exception)
      print("Press any key to exit.")
      Os.Getch()

```
