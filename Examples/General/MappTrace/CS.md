---
title: "MappTrace"
description: "This example shows how to generate an AIL trace using the Zebra Profiler utility. To generate a trace, you must open Zebra Profiler (accessible from the Aurora Imaging Control Center) and select 'Generate New Trace' from the File menu."
ms.language: "C#"
---

# MappTrace

> This example shows how to generate an AIL trace using the Zebra Profiler utility. To generate a trace, you must open Zebra Profiler (accessible from the Aurora Imaging Control Center) and select 'Generate New Trace' from the File menu.

**Language:** C#

**Functions used:** `MappAlloc`, `MappControl`, `MappFree`, `MappInquire`, `MappTrace`, `MbufAlloc2d`, `MbufAllocColor`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGetHookInfo`, `MdigInquire`, `MdigProcess`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimArith`, `MimBinarize`, `MimConvert`, `MimHistogramEqualize`, `MsysAlloc`, `MsysFree`, `MthrAlloc`, `MthrControl`, `MthrFree`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MappTrace.cs
//
// Description: This example shows how to explicitly control and generate a trace for 
//              functions and how to visualize it using the Aurora Imaging Profiler utility. 
//              To generate a trace, you must open Aurora Imaging Profiler (accessible from the 
//              Aurora Imaging Control Center) and select 'Generate New Trace' from the 'File' menu 
//              before to run your application.
// 
// Note:        By default, all applications are traceable without code modifications.
//              You can try this using Aurora Imaging Profiler with any example (Ex: MappStart).
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Runtime.InteropServices;
using Zebra.AuroraImagingLibrary;

namespace MappTrace
{
    class HookDataStruct
    {
        public AIL_ID AilImageDisp;
        public AIL_ID AilImageTemp1;
        public AIL_ID AilImageTemp2;
        public AIL_INT ProcessedImageCount;
        public AIL_ID DoneEvent;
    }

    class MappTrace
    {
        // Trace related constants
        private const int TRACE_TAG_HOOK_START = 1;
        private const int TRACE_TAG_PROCESSING = 2;
        private const int TRACE_TAG_PREPROCESSING = 3;

        // General constants.
        private static readonly int COLOR_BROWN = AIL.M_RGB888(100, 65, 50);

        private const int BUFFERING_SIZE_MAX = 3;
        private const int NUMBER_OF_FRAMES_TO_PROCESS = 10;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;
            AIL_ID AilSystem = AIL.M_NULL;
            AIL_ID AilDisplay = AIL.M_NULL;
            AIL_ID AilDigitizer = AIL.M_NULL;
            AIL_ID[] AilGrabBuf = new AIL_ID[BUFFERING_SIZE_MAX];
            AIL_ID AilDummyBuffer = AIL.M_NULL;

            AIL_INT TracesActivated = AIL.M_NO;
            AIL_INT NbGrabBuf = 0;
            AIL_INT SizeX = 0, SizeY = 0;

            HookDataStruct UserHookData = new HookDataStruct();

            Console.WriteLine();
            Console.WriteLine("PROGRAM TRACING AND PROFILING:");
            Console.WriteLine("----------------------------------");
            Console.WriteLine();
            Console.WriteLine("This example shows how to generate a trace for the execution");
            Console.WriteLine("of the functions, and to visualize it using");
            Console.WriteLine("the Aurora Imaging Profiler utility.");
            Console.WriteLine();
            Console.WriteLine("ACTION REQUIRED:");
            Console.WriteLine();
            Console.WriteLine("Open 'Aurora Imaging Profiler' from the 'Aurora Imaging Control Center' and");
            Console.WriteLine("select 'Generate New Trace' from the 'File' menu.");
            Console.WriteLine();
            Console.WriteLine("Press any key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            //************** Untraceable code section ***************

            // The following code will not be visible in the trace.

            // Application allocation. 
            // At Application allocation time, M_TRACE_LOG_DISABLE can be used to ensures that 
            // an application will not be traceable regardless of Aurora Imaging Profiler or Aurora Imaging Configurator requests
            // unless traces are explicitly enabled in the program using an MappControl command.
            //

            AIL.MappAlloc("M_DEFAULT", AIL.M_TRACE_LOG_DISABLE, ref AilApplication);

            // Dummy calls that will be invisible in the trace.


            AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_HOST, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilSystem);
            AIL.MbufAllocColor(AilSystem, 3, 128, 128, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE, ref AilDummyBuffer);
            AIL.MbufClear(AilDummyBuffer, 0L);
            AIL.MbufFree(AilDummyBuffer);
            AIL.MsysFree(AilSystem);

            //*******************************************************

            // Explicitly allow trace logging after a certain point if Aurora Imaging Profiler has
            // requested a trace. Note that M_TRACE = M_ENABLE can be used to force the log 
            // of a trace even if Profiler is not opened; M_TRACE = M_DISABLE can prevent 
            // logging of code section.
            //

            AIL.MappControl(AIL.M_DEFAULT, AIL.M_TRACE, AIL.M_DEFAULT);

            // Inquire if the traces are active (i.e. Profiler is open and waiting for a trace).

            AIL.MappInquire(AIL.M_DEFAULT, AIL.M_TRACE_ACTIVE, ref TracesActivated);

            if (TracesActivated == AIL.M_YES)
            {
                // Create custom trace markers: setting custom names and colors.

                // Initialize a custom Tag for the grab callback function with a unique color (blue).
                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_SET_TAG_INFORMATION,
                          TRACE_TAG_HOOK_START,
                          AIL.M_COLOR_BLUE, "Grab Callback Marker");

                // Initialize the custom Tag for the processing section.
                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_SET_TAG_INFORMATION,
                          TRACE_TAG_PROCESSING,
                          AIL.M_DEFAULT, "Processing Section");

                // Initialize the custom Tag for the preprocessing with a unique color (brown).
                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_SET_TAG_INFORMATION,
                          TRACE_TAG_PREPROCESSING,
                          COLOR_BROWN, "Preprocessing Marker");
            }


            // Allocate objects.
            AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_HOST, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilSystem);
            AIL.MdigAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref AilDigitizer);
            AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref AilDisplay);

            SizeX = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_X, AIL.M_NULL);
            SizeY = AIL.MdigInquire(AilDigitizer, AIL.M_SIZE_Y, AIL.M_NULL);

            AIL.MbufAllocColor(AilSystem, 3, SizeX, SizeY, 8 + AIL.M_UNSIGNED,
                           AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC + AIL.M_DISP, ref UserHookData.AilImageDisp);

            AIL.MdispSelect(AilDisplay, UserHookData.AilImageDisp);

            // Allocate the processing temporary buffers.
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_PROC + AIL.M_IMAGE, ref UserHookData.AilImageTemp1);
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_PROC + AIL.M_IMAGE, ref UserHookData.AilImageTemp2);

            // Allocate the grab buffers.
            for (NbGrabBuf = 0; NbGrabBuf < BUFFERING_SIZE_MAX; NbGrabBuf++)
            {
                AIL.MbufAllocColor(AilSystem, 3, SizeX, SizeY, 8 + AIL.M_UNSIGNED,
                               AIL.M_IMAGE + AIL.M_GRAB + AIL.M_PROC, ref AilGrabBuf[NbGrabBuf]);
            }

            // Initialize the user's processing function data structure.
            UserHookData.ProcessedImageCount = 0;
            AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_NOT_SIGNALED + AIL.M_AUTO_RESET, AIL.M_NULL,
                      AIL.M_NULL, ref UserHookData.DoneEvent);

            GCHandle UserHookDataPtr = GCHandle.Alloc(UserHookData);
            AIL_DIG_HOOK_FUNCTION_PTR HookFunctionPtr = new AIL_DIG_HOOK_FUNCTION_PTR(HookFunction);

            // Start the processing. The processing function is called with every frame grabbed.
            AIL.MdigProcess(AilDigitizer, AilGrabBuf, BUFFERING_SIZE_MAX, AIL.M_START,
                        AIL.M_DEFAULT, HookFunctionPtr, GCHandle.ToIntPtr(UserHookDataPtr));

            // Stop the processing when the event is triggered.
            AIL.MthrWait(UserHookData.DoneEvent, AIL.M_EVENT_WAIT + AIL.M_EVENT_TIMEOUT(2000), AIL.M_NULL);

            // Stop the processing.
            AIL.MdigProcess(AilDigitizer, AilGrabBuf, BUFFERING_SIZE_MAX, AIL.M_STOP, AIL.M_DEFAULT,
                        HookFunctionPtr, GCHandle.ToIntPtr(UserHookDataPtr));
            UserHookDataPtr.Free();

            // Free the grab and temporary buffers.
            for (NbGrabBuf = 0; NbGrabBuf < BUFFERING_SIZE_MAX; NbGrabBuf++)
                AIL.MbufFree(AilGrabBuf[NbGrabBuf]);
            AIL.MbufFree(UserHookData.AilImageTemp1);
            AIL.MbufFree(UserHookData.AilImageTemp2);


            // Free defaults.
            AIL.MthrFree(UserHookData.DoneEvent);
            AIL.MappFreeDefault(AilApplication,
                            AilSystem,
                            AilDisplay,
                            AilDigitizer,
                            UserHookData.AilImageDisp);

            // If Aurora Imaging Profiler activated the traces, the trace file is now ready.
            if (TracesActivated == AIL.M_YES)
            {
                Console.WriteLine("A PROCESSING SEQUENCE WAS EXECUTED AND LOGGED A NEW TRACE:");
                Console.WriteLine();
                Console.WriteLine("The trace can now be loaded in Aurora Imaging Profiler by selecting the");
                Console.WriteLine("corresponding file listed in the 'Trace Generation' dialog.");
                Console.WriteLine();
                Console.WriteLine("Once loaded, Aurora Imaging Profiler's main window displays the 'Main'");
                Console.WriteLine("and the 'MdigProcess' threads of the application.");
                Console.WriteLine();
                Console.WriteLine("- This main window can now be used to select a section");
                Console.WriteLine("  of a thread and to zoom or pan in it.");
                Console.WriteLine();
                Console.WriteLine("- The right pane shows detailed statistics as well as a");
                Console.WriteLine("  'Quick Access' list displaying all function calls.");
                Console.WriteLine();
                Console.WriteLine("- The 'User Markers' tab lists the markers and sections logged");
                Console.WriteLine("  during the execution. For example, selecting 'Tag:Processing'");
                Console.WriteLine("  allows double-clicking to refocus the display on the related");
                Console.WriteLine("  calls.");
                Console.WriteLine();
                Console.WriteLine("- By clicking a particular function call, either in the");
                Console.WriteLine("  'main view' or in the 'Quick Access', additional details");
                Console.WriteLine("  are displayed, such as its parameters and execution time.");
                Console.WriteLine();
            }
            else
            {
                Console.WriteLine("ERROR: No active tracing detected in Profiler!");
                Console.WriteLine();
            }
            Console.WriteLine("Press any key to end.");
            Console.ReadKey(true);
        }

        static AIL_INT HookFunction(AIL_INT HookType, AIL_ID HookId, IntPtr HookDataPtr)
        {
            AIL_ID CurrentImage = AIL.M_NULL;

            if (HookDataPtr != IntPtr.Zero)
            {
                HookDataStruct UserDataPtr = GCHandle.FromIntPtr(HookDataPtr).Target as HookDataStruct;

                // Add a marker to indicate the reception of a new grabbed image.
                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_MARKER,
                          TRACE_TAG_HOOK_START,
                          AIL.M_NULL,
                          "New Image grabbed");

                // Retrieve the AIL_ID of the grabbed buffer.
                AIL.MdigGetHookInfo(HookId, AIL.M_MODIFIED_BUFFER + AIL.M_BUFFER_ID, ref CurrentImage);

                // Start a Section to highlight the processing calls on the image.

                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_SECTION_START,
                          TRACE_TAG_PROCESSING,
                          UserDataPtr.ProcessedImageCount,
                          "Processing Image");

                // Add a Marker to indicate the start of the preprocessing section.
                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_MARKER,
                          TRACE_TAG_PREPROCESSING,
                          UserDataPtr.ProcessedImageCount,
                          "Start Preprocessing");

                // Do the preprocessing.
                AIL.MimConvert(CurrentImage, UserDataPtr.AilImageTemp1, AIL.M_RGB_TO_L);
                AIL.MimHistogramEqualize(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp1, AIL.M_UNIFORM, AIL.M_NULL, 55, 200);

                // Add a Marker to indicate the end of the preprocessing section.

                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_MARKER,
                          TRACE_TAG_PREPROCESSING,
                          UserDataPtr.ProcessedImageCount,
                          "End Preprocessing");

                // Do the main processing.
                AIL.MimBinarize(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp2, AIL.M_IN_RANGE, 120, 140);
                AIL.MimBinarize(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp1, AIL.M_IN_RANGE, 220, 255);
                AIL.MimArith(UserDataPtr.AilImageTemp1, UserDataPtr.AilImageTemp2, UserDataPtr.AilImageDisp, AIL.M_OR);

                // End the Section that highlights the processing.
                AIL.MappTrace(AIL.M_DEFAULT,
                          AIL.M_TRACE_SECTION_END,
                          TRACE_TAG_PROCESSING,
                          UserDataPtr.ProcessedImageCount,
                          "Processing Image End");

                // Signal that we have done enough processing.
                if (++(UserDataPtr.ProcessedImageCount) >= NUMBER_OF_FRAMES_TO_PROCESS)
                    AIL.MthrControl(UserDataPtr.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);
            }

            return 0;
        }
    }
}

```
