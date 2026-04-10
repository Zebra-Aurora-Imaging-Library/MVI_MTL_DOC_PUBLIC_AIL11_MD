---
title: "MdigGrabSequence"
description: "This example shows how to grab a sequence and archive it in real-time."
ms.language: "C#"
---

# MdigGrabSequence

> This example shows how to grab a sequence and archive it in real-time.

**Language:** C#

**Functions used:** `MbufImportSequence`, `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAllocColor`, `MbufClear`, `MbufCopy`, `MbufDiskInquire`, `MbufExportSequence`, `MbufFree`, `MbufControl`, `MdigAlloc`, `MdigControl`, `MdigFree`, `MdigGetHookInfo`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MsysAlloc`, `MsysFree`, `MsysInquire`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Digitizer, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigGrabSequence.cs
//
// Description: This example shows how to grab a sequence, archive it, and play 
//              it back in real time from an AVI file.
//
// Note:        This example assumes that the hard disk is sufficiently fast 
//              to keep up with the grab. Also, removing the sequence display or 
//              the text annotation while grabbing will reduce the CPU usage and
//              might help if some frames are missed during acquisition. 
//              If the disk or system are not fast enough, set
//              SAVE_SEQUENCE_TO_DISK to false.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates 
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;
using System.Runtime.InteropServices;

using Zebra.AuroraImagingLibrary;

namespace MDigGrabSequence
{
    class Program
    {
        // Sequence file name
        private const string SEQUENCE_FILE = AIL.M_TEMP_DIR + "AilSequence.avi";

        // Quantization factor to use during the compression.
        // Valid values are 1 to 99 (higher to lower quality).
        private const int COMPRESSION_Q_FACTOR = 50;

        // Annotation flag. Set to false to draw the frame number in the saved image.
        private static readonly AIL_INT FRAME_NUMBER_ANNOTATION = AIL.M_YES;

        // Maximum number of images for the multiple buffering grab.
        private const int NB_GRAB_IMAGE_MAX = 20;

        public class HookDataObject // User's archive function hook data structure.
        {
            public AIL_ID AilSystem;
            public AIL_ID AilDisplay;
            public AIL_ID AilImageDisp;
            public AIL_ID AilCompressedImage;
            public int NbGrabbedFrames;
            public int NbArchivedFrames;
            public bool SaveSequenceToDisk;
        };

        // Main function.
        // --------------
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;
            AIL_ID AilRemoteApplication = AIL.M_NULL;   // Remote Application identifier if running on a remote computer
            AIL_ID AilSystem = AIL.M_NULL;
            AIL_ID AilDigitizer = AIL.M_NULL;
            AIL_ID AilDisplay = AIL.M_NULL;
            AIL_ID AilImageDisp = AIL.M_NULL;
            AIL_ID[] AilGrabImages = new AIL_ID[NB_GRAB_IMAGE_MAX];
            AIL_ID AilCompressedImage = AIL.M_NULL;
            bool SaveSequenceToDisk = false;

            int NbFrames = 0;
            int n = 0;
            int NbFramesReplayed = 0;
            double FrameRate = 0;
            double TimeWait = 0;
            double TotalReplay = 0;
            HookDataObject UserHookData = new HookDataObject();
            AIL_INT LicenseModules = 0;
            AIL_INT FrameCount = 0;
            AIL_INT FrameMissed = 0;
            AIL_INT CompressAttribute = 0;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, ref AilDigitizer, AIL.M_NULL);

            // Allocate an image and display it.
            AIL.MbufAllocColor(AilSystem,
                                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND, AIL.M_NULL),
                                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X),
                                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y),
                                8 + AIL.M_UNSIGNED,
                                AIL.M_IMAGE + AIL.M_GRAB + AIL.M_DISP,
                                ref AilImageDisp);

            AIL.MbufClear(AilImageDisp, 0x0);
            AIL.MdispSelect(AilDisplay, AilImageDisp);

            // Grab continuously on display.
            AIL.MdigGrabContinuous(AilDigitizer, AilImageDisp);

            // Print a message
            Console.WriteLine();
            Console.WriteLine("SEQUENCE ACQUISITION:");
            Console.WriteLine("--------------------");
            Console.WriteLine();

            // Inquire licenses.
            AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION, ref AilRemoteApplication);
            AIL.MappInquire(AilRemoteApplication, AIL.M_LICENSE_MODULES, ref LicenseModules);

            Console.WriteLine("Choose the sequence format:");
            Console.WriteLine("1) Uncompressed images to memory (up to "+ (NB_GRAB_IMAGE_MAX) +" frames).");
            Console.WriteLine("2) Uncompressed images to an AVI file.");

            // If sequence is saved to disk, select between grabbing an 
            // uncompressed, JPEG or JPEG2000 sequence. 
            if (((LicenseModules & (AIL.M_LICENSE_JPEGSTD | AIL.M_LICENSE_JPEG2000)) != 0))
            {    
                if ((LicenseModules & AIL.M_LICENSE_JPEGSTD) != 0)
                {
                    Console.WriteLine("3) Compressed lossy JPEG images to an AVI file.");
                }
                if ((LicenseModules & AIL.M_LICENSE_JPEG2000) != 0)
                {
                    Console.WriteLine("4) Compressed lossy JPEG2000 images to an AVI file.");
                }
            }

            bool ValidSelection = false;
            while(!ValidSelection)
                {
                var Selection = Console.ReadKey(true);
                ValidSelection = true;

                // Set the buffer attribute.
                switch (Selection.Key)
                {
                case ConsoleKey.NumPad1:
                case ConsoleKey.D1:
                case ConsoleKey.Enter:
                    Console.WriteLine();
                    Console.WriteLine("Uncompressed images to memory selected.");
                    Console.WriteLine();
                    CompressAttribute = AIL.M_NULL;
                    SaveSequenceToDisk = false;
                    break;
                case ConsoleKey.NumPad2:
                case ConsoleKey.D2:
                    Console.WriteLine();
                    Console.WriteLine("Uncompressed images to file selected.");
                    Console.WriteLine();
                    CompressAttribute = AIL.M_NULL;
                    SaveSequenceToDisk = true;
                    break;

                case ConsoleKey.NumPad3:
                case ConsoleKey.D3:
                    Console.WriteLine();
                    Console.WriteLine("JPEG images to file selected.");
                    Console.WriteLine();
                    CompressAttribute = AIL.M_COMPRESS + AIL.M_JPEG_LOSSY;
                    SaveSequenceToDisk = true;
                    break;

                case ConsoleKey.NumPad4:
                case ConsoleKey.D4:
                    Console.WriteLine();
                    Console.WriteLine("JPEG 2000 images to file selected.");
                    Console.WriteLine();
                    CompressAttribute = AIL.M_COMPRESS + AIL.M_JPEG2000_LOSSY;
                    SaveSequenceToDisk = true;
                    break;

                default:
                    Console.WriteLine();
                    Console.WriteLine("Invalid selection !.");
                    ValidSelection = false;
                    break;
                }
            }

            // Allocate a compressed buffer if required.
            if (CompressAttribute != AIL.M_NULL)
            {
                AIL.MbufAllocColor(AilSystem,
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND, AIL.M_NULL),
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, AIL.M_NULL),
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, AIL.M_NULL),
                                    8 + AIL.M_UNSIGNED,
                                    AIL.M_IMAGE + CompressAttribute,
                                    ref AilCompressedImage);

                AIL.MbufControl(AilCompressedImage, AIL.M_Q_FACTOR, COMPRESSION_Q_FACTOR);
            }

            // Allocate the grab buffers to hold the sequence buffering.
            for (NbFrames = 0, n = 0; n < NB_GRAB_IMAGE_MAX; n++)
            {
                if (n == 2)
                {
                    AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
                }

                AIL.MbufAllocColor(AilSystem,
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_BAND, AIL.M_NULL),
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, AIL.M_NULL),
                                    AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, AIL.M_NULL),
                                    8 + AIL.M_UNSIGNED,
                                    AIL.M_IMAGE + AIL.M_GRAB,
                                    ref AilGrabImages[n]);

                if (AilGrabImages[n] != AIL.M_NULL)
                {
                    NbFrames++;
                    AIL.MbufClear(AilGrabImages[n], 0xFF);
                }
                else
                {
                    break;
                }
            }
           AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            // Halt continuous grab.
            AIL.MdigHalt(AilDigitizer);

            // Open the AVI file if required.
            if (SaveSequenceToDisk)
            {
                Console.WriteLine("Saving the sequence to an AVI file...");
                AIL.MbufExportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_OPEN);
            }
            else
            {
                Console.WriteLine("Saving the sequence to memory...");
                Console.WriteLine();
            }

            // Initialize User's archiving function hook data structure.
            UserHookData.AilSystem = AilSystem;
            UserHookData.AilDisplay = AilDisplay;
            UserHookData.AilImageDisp = AilImageDisp;
            UserHookData.AilCompressedImage = AilCompressedImage;
            UserHookData.SaveSequenceToDisk = SaveSequenceToDisk;
            UserHookData.NbGrabbedFrames = 0;
            UserHookData.NbArchivedFrames = 0;

            // get a handle to the DigHookUserData object in the managed heap, we will use this 
            // handle to get the object back in the callback function
            GCHandle UserHookDataHandle = GCHandle.Alloc(UserHookData);
            AIL_DIG_HOOK_FUNCTION_PTR UserHookFunctionDelegate = new AIL_DIG_HOOK_FUNCTION_PTR(RecordFunction);

            // Acquire the sequence. The processing hook function will
            // be called for each image grabbed to archive and display it. 
            // If sequence is not saved to disk, stop after NbFrames.
            AIL.MdigProcess(AilDigitizer, AilGrabImages, NbFrames, (SaveSequenceToDisk) ? AIL.M_START : AIL.M_SEQUENCE, AIL.M_DEFAULT, UserHookFunctionDelegate, GCHandle.ToIntPtr(UserHookDataHandle));

            Console.WriteLine();

            // Wait for a key press.
            if (SaveSequenceToDisk)
            {
                Console.WriteLine("Press any key to stop recording.");
                Console.WriteLine();
                Console.ReadKey(true);
            }

            // Wait until we have at least 2 frames to avoid an invalid frame rate.
            do
            {
                AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT, ref FrameCount);
            }
            while (FrameCount < 2);

            // Stop the sequence acquisition.
            AIL.MdigProcess(AilDigitizer, AilGrabImages, NbFrames, AIL.M_STOP, AIL.M_DEFAULT, UserHookFunctionDelegate, GCHandle.ToIntPtr(UserHookDataHandle));

            // Free the GCHandle when no longer used
            UserHookDataHandle.Free();

            // Read and print final statistics.
            AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT, ref FrameCount);
            AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_RATE, ref FrameRate);
            AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_MISSED, ref FrameMissed);

            Console.WriteLine();
            Console.WriteLine();
            Console.WriteLine("{0} frames recorded ({1} missed), at {2:0.0} frames/sec ({3:0.0} ms/frame).", UserHookData.NbGrabbedFrames, FrameMissed, FrameRate, 1000.0 / FrameRate);
            Console.WriteLine();

            // Sequence file closing if required.
            if (SaveSequenceToDisk)
            {
                AIL.MbufExportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, FrameRate, AIL.M_CLOSE);
            }

            Console.WriteLine("Press any key to start the sequence playback.");
            Console.ReadKey(true);
            Console.WriteLine();

            // Playback the sequence until a key is pressed.
            if (UserHookData.NbGrabbedFrames > 0)
            {
                do
                {
                    // If sequence must be loaded.
                    if (SaveSequenceToDisk)
                    {
                        // Inquire information about the sequence.
                        Console.WriteLine("Playing sequence from the AVI file...");
                        Console.WriteLine("Press any key to end playback.");
                        Console.WriteLine();
                        AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_NUMBER_OF_IMAGES, ref FrameCount);
                        AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_FRAME_RATE, ref FrameRate);
                        AIL.MbufDiskInquire(SEQUENCE_FILE, AIL.M_COMPRESSION_TYPE, ref CompressAttribute);

                        // Open the sequence file.
                        AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_OPEN);
                    }
                    else
                    {
                        Console.WriteLine("Playing sequence from memory...");
                        Console.WriteLine();
                    }
                    // Copy the images to the screen respecting the sequence frame rate.
                    TotalReplay = 0.0;
                    NbFramesReplayed = 0;
                    for (n = 0; n < FrameCount; n++)
                    {
                        // Reset the time.
                        AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET, AIL.M_NULL);

                        // If image was saved to disk.
                        if (SaveSequenceToDisk)
                        {
                            // Load image directly to the display.
                            AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_LOAD, AIL.M_NULL, ref AilImageDisp, n, 1, AIL.M_READ);
                            NbFramesReplayed++;
                            Console.Write("Frame #{0}             \r", NbFramesReplayed);
                        }
                        else
                        {
                            // Copy the grabbed image to the display.
                            AIL.MbufCopy(AilGrabImages[n], AilImageDisp);
                            NbFramesReplayed++;
                            Console.Write("Frame #{0}             \r", NbFramesReplayed);
                        }


                        // Check for a pressed key to exit.
                        if (Console.KeyAvailable && (n >= (NB_GRAB_IMAGE_MAX - 1)))
                        {
                            Console.ReadKey(true);
                            break;
                        }

                        // Wait to have a proper frame rate.
                        AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref TimeWait);
                        TotalReplay += TimeWait;
                        TimeWait = (1 / FrameRate) - TimeWait;
                        AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_WAIT, ref TimeWait);
                        TotalReplay += (TimeWait > 0) ? TimeWait : 0.0;
                    }

                    // Close the sequence file.
                    if (SaveSequenceToDisk)
                    {
                        AIL.MbufImportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AIL.M_CLOSE);
                    }

                    // Print statistics.
                    Console.WriteLine();
                    Console.WriteLine();
                    Console.WriteLine("{0} frames replayed, at a frame rate of {1:0.0} frames/sec ({2:0.0} ms/frame).", NbFramesReplayed, n / TotalReplay, 1000.0 * TotalReplay / n);
                    Console.WriteLine();
                    Console.WriteLine("Press <Enter> to end (or any other key to playback again).");
                }
                while (Console.ReadKey(true).Key != ConsoleKey.Enter);
            }

            // Free all allocated buffers.
            AIL.MbufFree(AilImageDisp);
            for (n = 0; n < NbFrames; n++)
            {
                AIL.MbufFree(AilGrabImages[n]);
            }

            if (AilCompressedImage != AIL.M_NULL)
            {
                AIL.MbufFree(AilCompressedImage);
            }

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AIL.M_NULL);
        }

        // User's archive function called each time a new buffer is grabbed.
        // -------------------------------------------------------------------*/

        // Local defines for the annotations.
        private const int STRING_LENGTH_MAX = 20;
        private const int STRING_POS_X = 20;
        private const int STRING_POS_Y = 20;

        static AIL_INT RecordFunction(AIL_INT HookType, AIL_ID HookId, IntPtr HookDataPtr)
        {
            GCHandle HookDataHandle = GCHandle.FromIntPtr(HookDataPtr);
            HookDataObject UserHookDataPtr = HookDataHandle.Target as HookDataObject;
            AIL_ID ModifiedImage = 0;

            // Retrieve the AIL_ID of the grabbed buffer.
            AIL.MdigGetHookInfo(HookId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID, ref ModifiedImage);

            // Increment the frame count.
            UserHookDataPtr.NbGrabbedFrames++;

            Console.Write("Frame #{0}\r", UserHookDataPtr.NbGrabbedFrames);

            // Draw the frame count in the image if enabled.
            if (FRAME_NUMBER_ANNOTATION == AIL.M_YES)
            {
                AIL.MgraText(AIL.M_DEFAULT, ModifiedImage, STRING_POS_X, STRING_POS_Y, UserHookDataPtr.NbGrabbedFrames.ToString());
            }

            // Compress the new image.
            if (UserHookDataPtr.AilCompressedImage != AIL.M_NULL)
            {
                AIL.MbufCopy(ModifiedImage, UserHookDataPtr.AilCompressedImage);
            }

            // Archive the new image.
            if (UserHookDataPtr.SaveSequenceToDisk)
            {
                AIL_ID ImageToExport;
                if (UserHookDataPtr.AilCompressedImage != AIL.M_NULL)
                {
                    ImageToExport = UserHookDataPtr.AilCompressedImage;
                }
                else
                {
                    ImageToExport = ModifiedImage;
                }

                AIL.MbufExportSequence(SEQUENCE_FILE, AIL.M_DEFAULT, ref ImageToExport, 1, AIL.M_DEFAULT, AIL.M_WRITE);
                UserHookDataPtr.NbArchivedFrames++;
            }



            // Copy the new grabbed image to the display.
            AIL.MbufCopy(ModifiedImage, UserHookDataPtr.AilImageDisp);

            return 0;
        }
    }
}

```
