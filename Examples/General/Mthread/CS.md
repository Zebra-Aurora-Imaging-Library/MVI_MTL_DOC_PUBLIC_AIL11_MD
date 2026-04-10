---
title: "Mthread"
description: "This program shows how to use threads in an AIL application and synchronize them with events. It creates 4 processing threads that are used to work in 4 different regions of a display buffer."
ms.language: "C#"
---

# Mthread

> This program shows how to use threads in an AIL application and synchronize them with events. It creates 4 processing threads that are used to work in 4 different regions of a display buffer.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MappInquire`, `MappTimer`, `MbufAlloc2d`, `MbufClear`, `MbufCopy`, `MbufCopyColor2d`, `MbufFree`, `MbufLoad`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MdispInquire`, `MgraArcFill`, `MgraRectFill`, `MgraText`, `MimArith`, `MimConvolve`, `MimRotate`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MthrAlloc`, `MthrControl`, `MthrFree`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Graphics, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MThread.cs 
//
// Description: This program shows how to use threads in an application and
//              synchronize them with event. It creates 4 processing threads that
//              are used to work in 4 different regions of a display buffer.
//      
//              Thread usage:
//              - The main thread starts a processing thread in each of the 4 different
//                quarters of a display buffer. The main thread then waits for a key to
//                be pressed to stop them.
//              - The top-left and bottom-left threads work in a loop, as follows: the
//                top-left thread adds a constant to its buffer, then sends an event to
//                the bottom-left thread. The bottom-left thread waits for the event
//                from the top-left thread, rotates the top-left buffer image, then sends an
//                event to the top-left thread to start a new loop.
//              - The top-right and bottom-right threads work the same way as the
//                top-left and bottom-left threads, except that the bottom-right thread
//                performs an edge detection operation, rather than a rotation.
//
// Note:        - Under AIL-Lite, the threads will do graphic annotations instead.
//              - Comment out the MdispSelect() if you wish to avoid benchmarking
//                the display update overhead on CPU usage and processing rate.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;
using System.Threading;
using System.Runtime.InteropServices;

using Zebra.AuroraImagingLibrary;

namespace MThread
{
    class Program
    {
        // Local constants.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Bird.mim";
        private const int IMAGE_WIDTH = 256;
        private const int IMAGE_HEIGHT = 240;
        private const int STRING_LENGTH_MAX = 40;
        private const int STRING_POS_X = 10;
        private const int STRING_POS_Y = 220;
        private const int DRAW_RADIUS_NUMBER = 5;
        private const int DRAW_RADIUS_STEP = 10;
        private const int DRAW_CENTER_POSX = 196;
        private const int DRAW_CENTER_POSY = 180;

        // Thread parameters object.
        public class THREAD_PARAM
        {
            public AIL_ID Id;
            public AIL_ID System;
            public AIL_ID OrgImage;
            public AIL_ID SrcImage;
            public AIL_ID DstImage;
            public AIL_ID DispImage;
            public AIL_INT DispOffsetX;
            public AIL_INT DispOffsetY;
            public AIL_ID ReadyEvent;
            public AIL_ID DoneEvent;
            public AIL_INT NumberOfIteration;
            public AIL_INT Radius;
            public AIL_INT Exit;
            public AIL_INT LicenseModules;
            public THREAD_PARAM LinkThreadParam;
        }

        // Main function:
        // --------------
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;                 // Application identifier.
            AIL_ID AilRemoteApplication = AIL.M_NULL;           // Remote Application identifier if running on a remote computer
            AIL_ID AilSystem = AIL.M_NULL;                      // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                     // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                       // Image buffer identifiers.
            AIL_ID AilOrgImage = AIL.M_NULL;
            THREAD_PARAM TParTopLeft = new THREAD_PARAM();      // Parameters passed to top-left thread.
            THREAD_PARAM TParBotLeft = new THREAD_PARAM();      // Parameters passed to bottom-left thread.
            THREAD_PARAM TParTopRight = new THREAD_PARAM();     // Parameters passed to top-right thread.
            THREAD_PARAM TParBotRight = new THREAD_PARAM();     // Parameters passed to bottom-right thread.
            double Time = 0.0;                                  // Timer variables.
            double FramesPerSecond = 0.0;
            AIL_INT LicenseModules = 0;                         // List of available modules.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Allocate and display the main image buffer.
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH * 2, IMAGE_HEIGHT * 2, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC + AIL.M_DISP, ref AilImage);
            AIL.MbufClear(AilImage, 0);
            AIL.MdispSelect(AilDisplay, AilImage);
            AIL.MdispInquire(AilDisplay, AIL.M_SELECTED, ref TParTopLeft.DispImage);

            // Allocate an image buffer to keep the original.
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilOrgImage);

            // Allocate a processing buffer for each thread.
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref TParTopLeft.SrcImage);
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref TParBotLeft.DstImage);
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref TParTopRight.SrcImage);
            AIL.MbufAlloc2d(AilSystem, IMAGE_WIDTH, IMAGE_HEIGHT, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref TParBotRight.DstImage);

            // Allocate synchronization events.
            AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, ref TParTopLeft.DoneEvent);
            AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, ref TParBotLeft.DoneEvent);
            AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, ref TParTopRight.DoneEvent);
            AIL.MthrAlloc(AilSystem, AIL.M_EVENT, AIL.M_DEFAULT, AIL.M_NULL, AIL.M_NULL, ref TParBotRight.DoneEvent);

            // Inquire licenses.
            AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION, ref AilRemoteApplication);
            AIL.MappInquire(AilRemoteApplication, AIL.M_LICENSE_MODULES, ref LicenseModules);

            // Initialize remaining thread parameters.
            TParTopLeft.System = AilSystem;
            TParTopLeft.OrgImage = AilOrgImage;
            TParTopLeft.DstImage = TParTopLeft.SrcImage;
            TParTopLeft.DispOffsetX = 0;
            TParTopLeft.DispOffsetY = 0;
            TParTopLeft.ReadyEvent = TParBotLeft.DoneEvent;
            TParTopLeft.NumberOfIteration = 0;
            TParTopLeft.Radius = 0;
            TParTopLeft.Exit = 0;
            TParTopLeft.LicenseModules = LicenseModules;
            TParTopLeft.LinkThreadParam = TParBotLeft;

            TParBotLeft.System = AilSystem;
            TParBotLeft.OrgImage = 0;
            TParBotLeft.SrcImage = TParTopLeft.DstImage;
            TParBotLeft.DispImage = TParTopLeft.DispImage;
            TParBotLeft.DispOffsetX = 0;
            TParBotLeft.DispOffsetY = IMAGE_HEIGHT;
            TParBotLeft.ReadyEvent = TParTopLeft.DoneEvent;
            TParBotLeft.NumberOfIteration = 0;
            TParBotLeft.Radius = 0;
            TParBotLeft.Exit = 0;
            TParBotLeft.LicenseModules = LicenseModules;
            TParBotLeft.LinkThreadParam = null;

            TParTopRight.System = AilSystem;
            TParTopRight.OrgImage = AilOrgImage;
            TParTopRight.DstImage = TParTopRight.SrcImage;
            TParTopRight.DispImage = TParTopLeft.DispImage;
            TParTopRight.DispOffsetX = IMAGE_WIDTH;
            TParTopRight.DispOffsetY = 0;
            TParTopRight.ReadyEvent = TParBotRight.DoneEvent;
            TParTopRight.NumberOfIteration = 0;
            TParTopRight.Radius = 0;
            TParTopRight.Exit = 0;
            TParTopRight.LicenseModules = LicenseModules;
            TParTopRight.LinkThreadParam = TParBotRight;

            TParBotRight.System = AilSystem;
            TParBotRight.OrgImage = 0;
            TParBotRight.SrcImage = TParTopRight.DstImage;
            TParBotRight.DispImage = TParTopLeft.DispImage;
            TParBotRight.DispOffsetX = IMAGE_WIDTH;
            TParBotRight.DispOffsetY = IMAGE_HEIGHT;
            TParBotRight.ReadyEvent = TParTopRight.DoneEvent;
            TParBotRight.NumberOfIteration = 0;
            TParBotRight.Radius = 0;
            TParBotRight.Exit = 0;
            TParBotRight.LicenseModules = LicenseModules;
            TParBotRight.LinkThreadParam = null;

            // Initialize the original image to process.
            AIL.MbufLoad(IMAGE_FILE, AilOrgImage);

            // Start the 4 threads.
            AIL_THREAD_FUNCTION_PTR TopThreadDelegate = new AIL_THREAD_FUNCTION_PTR(TopThread);
            AIL_THREAD_FUNCTION_PTR BotLeftThreadDelegate = new AIL_THREAD_FUNCTION_PTR(BotLeftThread);
            AIL_THREAD_FUNCTION_PTR BotRightThreadDelegate = new AIL_THREAD_FUNCTION_PTR(BotRightThread);

            GCHandle TParTopLeftHandle = GCHandle.Alloc(TParTopLeft);
            GCHandle TParBotLeftHandle = GCHandle.Alloc(TParBotLeft);
            GCHandle TParTopRightHandle = GCHandle.Alloc(TParTopRight);
            GCHandle TParBotRightHandle = GCHandle.Alloc(TParBotRight);

            AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, TopThreadDelegate, GCHandle.ToIntPtr(TParTopLeftHandle), ref TParTopLeft.Id);
            AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, BotLeftThreadDelegate, GCHandle.ToIntPtr(TParBotLeftHandle), ref TParBotLeft.Id);
            AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, TopThreadDelegate, GCHandle.ToIntPtr(TParTopRightHandle), ref TParTopRight.Id);
            AIL.MthrAlloc(AilSystem, AIL.M_THREAD, AIL.M_DEFAULT, BotRightThreadDelegate, GCHandle.ToIntPtr(TParBotRightHandle), ref TParBotRight.Id);

            // Start the timer.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET + AIL.M_SYNCHRONOUS, AIL.M_NULL);

            // Set events to start operation of top-left and top-right threads.
            AIL.MthrControl(TParTopLeft.ReadyEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);
            AIL.MthrControl(TParTopRight.ReadyEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);

            // Report that the threads are started and wait for a key press to stop them.
            Console.Write("\nMULTI-THREADING:\n");
            Console.Write("----------------\n\n");
            Console.Write("4 threads running...\n");
            Console.Write("Press any key to stop.\n\n");
            Console.ReadKey(true);

            // Signal the threads to exit.
            TParTopLeft.Exit = 1;
            TParTopRight.Exit = 1;

            // Wait for all threads to terminate.
            AIL.MthrWait(TParTopLeft.Id, AIL.M_THREAD_END_WAIT);
            AIL.MthrWait(TParBotLeft.Id, AIL.M_THREAD_END_WAIT);
            AIL.MthrWait(TParTopRight.Id, AIL.M_THREAD_END_WAIT);
            AIL.MthrWait(TParBotRight.Id, AIL.M_THREAD_END_WAIT);

            // Stop the timer and calculate the number of frames per second processed.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ + AIL.M_SYNCHRONOUS, ref Time);
            FramesPerSecond = (TParTopLeft.NumberOfIteration + TParBotLeft.NumberOfIteration + TParTopRight.NumberOfIteration + TParBotRight.NumberOfIteration) / Time;

            // Print statistics.
            Console.Write("Top-left iterations done:     {0,4}.\n", TParTopLeft.NumberOfIteration);
            Console.Write("Bottom-left iterations done:  {0,4}.\n", TParBotLeft.NumberOfIteration);
            Console.Write("Top-right iterations done:    {0,4}.\n", TParTopRight.NumberOfIteration);
            Console.Write("Bottom-right iterations done: {0,4}.\n\n", TParBotRight.NumberOfIteration);
            Console.Write("Processing speed for the 4 threads: {0:0.0} Images/Sec.\n\n", FramesPerSecond);
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free threads.
            AIL.MthrFree(TParTopLeft.Id);
            AIL.MthrFree(TParBotLeft.Id);
            AIL.MthrFree(TParTopRight.Id);
            AIL.MthrFree(TParBotRight.Id);

            // Free events.
            AIL.MthrFree(TParTopLeft.DoneEvent);
            AIL.MthrFree(TParBotLeft.DoneEvent);
            AIL.MthrFree(TParTopRight.DoneEvent);
            AIL.MthrFree(TParBotRight.DoneEvent);

            // Free buffers.
            AIL.MbufFree(TParTopLeft.SrcImage);
            AIL.MbufFree(TParTopRight.SrcImage);
            AIL.MbufFree(TParBotLeft.DstImage);
            AIL.MbufFree(TParBotRight.DstImage);
            AIL.MbufFree(AilOrgImage);
            AIL.MbufFree(AilImage);

            // Free the GCHandles
            TParTopLeftHandle.Free();
            TParBotLeftHandle.Free();
            TParTopRightHandle.Free();
            TParBotRightHandle.Free();

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        // Top-left and top-right threads' function (Add an offset):
        // ---------------------------------------------------------
        static uint TopThread(IntPtr ThreadParameters)
        {
            GCHandle threadParamHandle = GCHandle.FromIntPtr(ThreadParameters);
            THREAD_PARAM TPar = threadParamHandle.Target as THREAD_PARAM;

            while (TPar.Exit == 0)
            {
                // Wait for bottom ready event before proceeding.
                AIL.MthrWait(TPar.ReadyEvent, AIL.M_EVENT_WAIT);

                // For better visual effect, reset SrcImage to the original image regularly.
                if ((TPar.NumberOfIteration % 192) == 0)
                {
                    AIL.MbufCopy(TPar.OrgImage, TPar.SrcImage);
                }

                if ((TPar.LicenseModules & AIL.M_LICENSE_IM) != 0)
                {
                    // Add a constant to the image.
                    AIL.MimArith(TPar.SrcImage, 1L, TPar.DstImage, AIL.M_ADD_CONST + AIL.M_SATURATION);
                }
                else
                {
                    // Under AIL-Lite draw a variable size rectangle in the image.
                    TPar.Radius = TPar.LinkThreadParam.Radius = (TPar.NumberOfIteration % DRAW_RADIUS_NUMBER) * DRAW_RADIUS_STEP;
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xff);
                    AIL.MgraRectFill(AIL.M_DEFAULT, TPar.DstImage, DRAW_CENTER_POSX - TPar.Radius, DRAW_CENTER_POSY - TPar.Radius, DRAW_CENTER_POSX + TPar.Radius, DRAW_CENTER_POSY + TPar.Radius);
                }

                // Increment iteration count and draw text.
                TPar.NumberOfIteration++;
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF);
                AIL.MgraText(AIL.M_DEFAULT, TPar.DstImage, STRING_POS_X, STRING_POS_Y, String.Format("{0}", TPar.NumberOfIteration));

                // Update the display.
                if (TPar.DispImage != AIL.M_NULL)
                {
                    AIL.MbufCopyColor2d(TPar.DstImage, TPar.DispImage, AIL.M_ALL_BANDS, 0, 0, AIL.M_ALL_BANDS, TPar.DispOffsetX, TPar.DispOffsetY, IMAGE_WIDTH, IMAGE_HEIGHT);
                }

                // Signal to the bottom thread that the first part of the processing is completed.
                AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);
            }

            // Require the bottom thread to exit.
            TPar.LinkThreadParam.Exit = 1;

            // Signal the bottom thread to wake up.
            AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);

            // Before exiting the thread, make sure that all the commands are executed.
            AIL.MthrWait(TPar.System, AIL.M_THREAD_WAIT, AIL.M_NULL);

            return 1;
        }


        // Bottom-left thread function (Rotate):
        // -------------------------------------
        static uint BotLeftThread(IntPtr ThreadParameters)
        {
            GCHandle threadParamHandle = GCHandle.FromIntPtr(ThreadParameters);
            THREAD_PARAM TPar = threadParamHandle.Target as THREAD_PARAM;
            double Angle = 0.0;
            double AngleIncrement = 0.5;

            while (TPar.Exit == 0)
            {
                // Wait for the event in top-left function to be ready before proceeding.
                AIL.MthrWait(TPar.ReadyEvent, AIL.M_EVENT_WAIT);

                if ((TPar.LicenseModules & AIL.M_LICENSE_IM) != 0)
                {
                    // Rotate the image.
                    AIL.MimRotate(TPar.SrcImage, TPar.DstImage, Angle, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NEAREST_NEIGHBOR + AIL.M_OVERSCAN_CLEAR);
                    Angle += AngleIncrement;
                    if (Angle >= 360)
                    {
                        Angle -= 360;
                    }
                }
                else
                {
                    // Under AIL-Lite copy the top-left image and draw 
                    // a variable size filled circle in the image.
                    AIL.MbufCopy(TPar.SrcImage, TPar.DstImage);
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x80);
                    AIL.MgraArcFill(AIL.M_DEFAULT, TPar.DstImage, DRAW_CENTER_POSX, DRAW_CENTER_POSY, TPar.Radius, TPar.Radius, 0, 360);
                }

                // Increment iteration count and draw text.
                TPar.NumberOfIteration++;
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF);
                AIL.MgraText(AIL.M_DEFAULT, TPar.DstImage, STRING_POS_X, STRING_POS_Y, String.Format("{0}", TPar.NumberOfIteration));

                // Update the display.
                if (TPar.DispImage != AIL.M_NULL)
                {
                    AIL.MbufCopyColor2d(TPar.DstImage, TPar.DispImage, AIL.M_ALL_BANDS, 0, 0, AIL.M_ALL_BANDS, TPar.DispOffsetX, TPar.DispOffsetY, IMAGE_WIDTH, IMAGE_HEIGHT);
                }

                // Signal to the top-left thread that the last part of the processing is completed.
                AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);
            }

            // Before exiting the thread, make sure that all the commands are executed.
            AIL.MthrWait(TPar.System, AIL.M_THREAD_WAIT, AIL.M_NULL);

            return 1;
        }

        // Bottom-right thread function (Edge Detect):
        // -------------------------------------------
        static uint BotRightThread(IntPtr ThreadParameters)
        {
            GCHandle threadParamHandle = GCHandle.FromIntPtr(ThreadParameters);
            THREAD_PARAM TPar = threadParamHandle.Target as THREAD_PARAM;

            while (TPar.Exit == 0)
            {
                // Wait for the event in top-right function to be ready before proceeding.
                AIL.MthrWait(TPar.ReadyEvent, AIL.M_EVENT_WAIT);

                if ((TPar.LicenseModules & AIL.M_LICENSE_IM) != 0)
                {
                    // Perform an edge detection operation on the image.
                    AIL.MimConvolve(TPar.SrcImage, TPar.DstImage, AIL.M_EDGE_DETECT_SOBEL_FAST);
                }
                else
                {
                    // Under AIL-Lite copy the top-right image and draw
                    // a variable size filled circle in the image.
                    AIL.MbufCopy(TPar.SrcImage, TPar.DstImage);
                    AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0x40);
                    AIL.MgraArcFill(AIL.M_DEFAULT, TPar.DstImage, DRAW_CENTER_POSX, DRAW_CENTER_POSY, TPar.Radius / 2, TPar.Radius / 2, 0, 360);
                }

                // Increment iteration count and draw text.
                TPar.NumberOfIteration++;
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, 0xFF);
                AIL.MgraText(AIL.M_DEFAULT, TPar.DstImage, STRING_POS_X, STRING_POS_Y, String.Format("{0}", TPar.NumberOfIteration));

                // Update the display.
                if (TPar.DispImage != AIL.M_NULL)
                {
                    AIL.MbufCopyColor2d(TPar.DstImage, TPar.DispImage, AIL.M_ALL_BANDS, 0, 0, AIL.M_ALL_BANDS, TPar.DispOffsetX, TPar.DispOffsetY, IMAGE_WIDTH, IMAGE_HEIGHT);
                }

                // Signal to the top-right thread that the last part of the processing is completed.
                AIL.MthrControl(TPar.DoneEvent, AIL.M_EVENT_SET, AIL.M_SIGNALED);
            }

            // Before exiting the thread, make sure that all the commands are executed.
            AIL.MthrWait(TPar.System, AIL.M_THREAD_WAIT, AIL.M_NULL);

            return 1;
        }
    }
}

```
