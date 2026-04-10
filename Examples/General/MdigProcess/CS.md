---
title: "MdigProcess"
description: "This program shows the use of the MdigProcess() function to perform real-time processing."
ms.language: "C#"
---

# MdigProcess

> This program shows the use of the MdigProcess() function to perform real-time processing.

**Language:** C#

**Functions used:** `MdigGrabContinuous`, `MappAlloc`, `MappControl`, `MappFree`, `MbufAlloc2d`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigHalt`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MgraText`, `MimArith`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigProcess.cs
//
// Description: This program shows the use of the MdigProcess() function and its multiple
//              buffering acquisition to do robust real-time processing.
//
//              The user's processing code to execute is located in a callback function
//              that will be called for each frame acquired (see ProcessingFunction()).
//
// Note:        The average processing time must be shorter than the grab time or some
//              frames will be missed. Also, if the processing results are not displayed
//              and the frame count is not drawn or printed, the CPU usage is reduced
//              significantly.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;
using System.Runtime.InteropServices;

using Zebra.AuroraImagingLibrary;

namespace MDigProcess
{
    class Program
    {
        // Number of images in the buffering grab queue.
        // Generally, increasing this number gives a better real-time grab.
        private const int BUFFERING_SIZE_MAX = 20;

        // User's processing function hook data object.
        public class HookDataStruct
        {
            public AIL_ID AilDigitizer;
            public AIL_ID AilImageDisp;
            public int ProcessedImageCount;
        };

        // Main function.
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;
            AIL_ID AilSystem = AIL.M_NULL;
            AIL_ID AilDigitizer = AIL.M_NULL;
            AIL_ID AilDisplay = AIL.M_NULL;
            AIL_ID AilImageDisp = AIL.M_NULL;
            AIL_ID[] AilGrabBufferList = new AIL_ID[BUFFERING_SIZE_MAX];
            int AilGrabBufferListSize = 0;
            AIL_INT ProcessFrameCount = 0;
            double ProcessFrameRate = 0;

            HookDataStruct UserHookData = new HookDataStruct();

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay,
                                                        ref AilDigitizer, AIL.M_NULL);

            // Allocate a monochrome display buffer. 
            AIL.MbufAlloc2d(AilSystem,
                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, AIL.M_NULL),
                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, AIL.M_NULL),
                8 + AIL.M_UNSIGNED,
                AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC + AIL.M_DISP,
                ref AilImageDisp);
            AIL.MbufClear(AilImageDisp, AIL.M_COLOR_BLACK);

            // Display the image buffer. 
            AIL.MdispSelect(AilDisplay, AilImageDisp);

            // Print a message.
            Console.WriteLine();
            Console.WriteLine("MULTIPLE BUFFERED PROCESSING.");
            Console.WriteLine("-----------------------------");
            Console.WriteLine();
            Console.WriteLine("Press any key to start processing.");
			Console.WriteLine();

            // Grab continuously on the display and wait for a key press.
            AIL.MdigGrabContinuous(AilDigitizer, AilImageDisp);
            Console.ReadKey(true);

            // Halt continuous grab.
            AIL.MdigHalt(AilDigitizer);

            // Allocate the grab buffers and clear them.
            for (AilGrabBufferListSize = 0; AilGrabBufferListSize < BUFFERING_SIZE_MAX; AilGrabBufferListSize++)
            {
                if (AilGrabBufferListSize == 2)
                {
                    AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
                }

                AIL.MbufAlloc2d(AilSystem,
                                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, AIL.M_NULL),
                                AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, AIL.M_NULL),
                                8 + AIL.M_UNSIGNED,
                                AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC,
                                ref AilGrabBufferList[AilGrabBufferListSize]);

                if (AilGrabBufferList[AilGrabBufferListSize] != AIL.M_NULL)
                {
                    AIL.MbufClear(AilGrabBufferList[AilGrabBufferListSize], 0xFF);
                }
                else
                {
                    break;
                }
            }
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            // Initialize the user's processing function data structure.
            UserHookData.AilDigitizer = AilDigitizer;
            UserHookData.AilImageDisp = AilImageDisp;
            UserHookData.ProcessedImageCount = 0;

            // get a handle to the HookDataStruct object in the managed heap, we will use this 
            // handle to get the object back in the callback function
            GCHandle hUserData = GCHandle.Alloc(UserHookData);
            AIL_DIG_HOOK_FUNCTION_PTR ProcessingFunctionPtr = new AIL_DIG_HOOK_FUNCTION_PTR(ProcessingFunction);

            // Start the processing. The processing function is called with every frame grabbed.
            AIL.MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize, AIL.M_START, AIL.M_DEFAULT, ProcessingFunctionPtr, GCHandle.ToIntPtr(hUserData));

            // Here the main() is free to perform other tasks while the processing is executing.
            // ---------------------------------------------------------------------------------

            // Print a message and wait for a key press after a minimum number of frames.
            Console.WriteLine("Press any key to stop.                    ");
            Console.WriteLine();
            Console.ReadKey(true);

            // Stop the processing.
            AIL.MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize, AIL.M_STOP, AIL.M_DEFAULT, ProcessingFunctionPtr, GCHandle.ToIntPtr(hUserData));

            // Free the GCHandle when no longer used
            hUserData.Free();

            // Print statistics.
            AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_COUNT, ref ProcessFrameCount);
            AIL.MdigInquire(AilDigitizer, AIL.M_PROCESS_FRAME_RATE, ref ProcessFrameRate);
            Console.WriteLine();
            Console.WriteLine();
            Console.WriteLine("{0} frames grabbed at {1:0.0} frames/sec ({2:0.0} ms/frame).", ProcessFrameCount, ProcessFrameRate, 1000.0 / ProcessFrameRate);
            Console.WriteLine("Press any key to end.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Free the grab buffers.
            while (AilGrabBufferListSize > 0)
            {
                AIL.MbufFree(AilGrabBufferList[--AilGrabBufferListSize]);
            }

            // Free display buffer.
            AIL.MbufFree(AilImageDisp);

            // Release defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AIL.M_NULL);
        }

        // User's processing function called every time a grab buffer is ready.
        // -----------------------------------------------------------------------

        // Local defines.
        private const int STRING_LENGTH_MAX = 20;
        private const int STRING_POS_X = 20;
        private const int STRING_POS_Y = 20;
        static AIL_INT ProcessingFunction(AIL_INT HookType, AIL_ID HookId, IntPtr HookDataPtr)
        {
            AIL_ID ModifiedBufferId = AIL.M_NULL;

            // this is how to check if the user data is null, the IntPtr class
            // contains a member, Zero, which exists solely for this purpose
            if (!IntPtr.Zero.Equals(HookDataPtr))
            {
                // get the handle to the DigHookUserData object back from the IntPtr
                GCHandle hUserData = GCHandle.FromIntPtr(HookDataPtr);

                // get a reference to the DigHookUserData object
                HookDataStruct UserData = hUserData.Target as HookDataStruct;

                // Retrieve the AIL_ID of the grabbed buffer.
                AIL.MdigGetHookInfo(HookId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID, ref ModifiedBufferId);

                // Increment the frame counter.
                UserData.ProcessedImageCount++;

                // Print and draw the frame count (remove to reduce CPU usage).
                Console.Write("Processing frame #{0}.\r", UserData.ProcessedImageCount);
                AIL.MgraText(AIL.M_DEFAULT, ModifiedBufferId, STRING_POS_X, STRING_POS_Y, String.Format("{0}", UserData.ProcessedImageCount));

                // Execute the processing and update the display.
                AIL.MimArith(ModifiedBufferId, AIL.M_NULL, UserData.AilImageDisp, AIL.M_NOT);
            }

            return 0;
        }
    }
}

```
