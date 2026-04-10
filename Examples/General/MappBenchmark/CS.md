---
title: "MappBenchmark"
description: "This program uses the MappTimer function to demonstrate the benchmarking of AIL functions."
ms.language: "C#"
---

# MappBenchmark

> This program uses the MappTimer function to demonstrate the benchmarking of AIL functions.

**Language:** C#

**Functions used:** `MappAlloc`, `MappControlMp`, `MappFree`, `MappTimer`, `MbufAllocColor`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimRotate`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MthrInquireMp`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MappBenchmark.cs
//
// Description: This program uses the MappTimer function to demonstrate the 
//              benchmarking of functions. It can be used as a template 
//              to benchmark any Aurora Imaging Library or custom processing function accurately.
//       
//              It takes into account different factors that can influence 
//              the timing such as dll load time and OS inaccuracy when 
//              measuring a very short time.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;

using Zebra.AuroraImagingLibrary;

namespace MAppBenchmark
{
    // Processing function parameters structure.
    public struct PROC_PARAM
    {
        public AIL_ID AilSourceImage;        // Image buffer identifier.
        public AIL_ID AilDestinationImage;   // Image buffer identifier.
    }

    public class Program
    {
        // Target image specifications.
        private static readonly string IMAGE_FILE = AIL.M_IMAGE_PATH + "LargeWafer.mim";
        private const int ROTATE_ANGLE = -15;

        // Timing loop iterations setting.
        private const double MINIMUM_BENCHMARK_TIME = 1.0; // In seconds (1.0 and more recommended).
        private const int ESTIMATION_NB_LOOP = 5;
        private const int DEFAULT_NB_LOOP = 100;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;             // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                  // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                 // Display identifier.
            AIL_ID AilDisplayImage = AIL.M_NULL;            // Image buffer identifier.
            AIL_ID AilSystemOwnerApplication = AIL.M_NULL;  // System's owner application.
            AIL_ID AilSystemCurrentThreadId = AIL.M_NULL;   // System's current thread identifier.
            PROC_PARAM ProcessingParam = new PROC_PARAM();  // Processing parameters.
            double TimeAllCores = 0.0;                      // Timer variables.
            double TimeAllCoresNoCS = 0.0;
            double TimeOneCore = 0.0;
            double FPSAllCores = 0.0;                       // FPS variables.
            double FPSAllCoresNoCS = 0.0;
            double FPSOneCore = 0.0;
            AIL_INT NbCoresUsed = 0;                        // Number of CPU Core used.
            AIL_INT NbCoresUsedNoCS = 0;
            AIL_INT NbPerformanceLevel = 0;                 // Number of performance level.
            AIL_INT CurrentMaxPerfLevel = 0;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Get the system's owner application.
            AIL.MsysInquire(AilSystem, AIL.M_OWNER_APPLICATION, ref AilSystemOwnerApplication);

            // Get the system's current thread identifier.
            AIL.MsysInquire(AilSystem, AIL.M_CURRENT_THREAD_ID, ref AilSystemCurrentThreadId);

            // Restore image into an automatically allocated image buffer.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilDisplayImage);

            // Display the image buffer.
            AIL.MdispSelect(AilDisplay, AilDisplayImage);

            // Allocate the processing objects.
            ProcessingInit(AilSystem, ref ProcessingParam);

            // Pause to show the original image.
            Console.WriteLine();
            Console.WriteLine("PROCESSING FUNCTION BENCHMARKING:");
            Console.WriteLine("---------------------------------");
            Console.WriteLine();
            Console.WriteLine("This program times a processing function under different conditions.");
            Console.WriteLine("Press any key to start.");
            Console.WriteLine();
            Console.ReadKey(true);
            Console.WriteLine("PROCESSING TIME FOR {0}x{1}:",
                AIL.MbufInquire(ProcessingParam.AilDestinationImage, AIL.M_SIZE_X, AIL.M_NULL),
                AIL.MbufInquire(ProcessingParam.AilDestinationImage, AIL.M_SIZE_Y, AIL.M_NULL));
            Console.WriteLine("------------------------------");
            Console.WriteLine();

            // Benchmark the processing function without multi-processing.
            // -----------------------------------------------------------
            AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DISABLE, AIL.M_NULL);
            Benchmark(ref ProcessingParam, ref TimeOneCore, ref FPSOneCore);

            // Show the resulting image and the timing results.
            AIL.MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage);

            Console.WriteLine("Without multi-processing (  1 CPU core ): {0,5:0.000} ms ({1,6:0.0} fps)", TimeOneCore, FPSOneCore);
            Console.WriteLine();

            AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL);


            // Benchmark the processing function with multi-processing.
            // --------------------------------------------------------
            AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_ENABLE, AIL.M_NULL);

            // Enable all performance level.
            AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE_PERFORMANCE_LEVEL, AIL.M_ALL, AIL.M_ENABLE, AIL.M_NULL);

            // Inquire the number of performance level. This will return 1 on non-hybrid processor.
            AIL.MappInquireMp(AilSystemOwnerApplication, AIL.M_MP_NB_PERFORMANCE_LEVEL, AIL.M_DEFAULT, AIL.M_DEFAULT, ref NbPerformanceLevel);

            for (CurrentMaxPerfLevel = NbPerformanceLevel; CurrentMaxPerfLevel > 0; CurrentMaxPerfLevel--)
            {
                if (CurrentMaxPerfLevel > 1)
                {
                    Console.WriteLine("Benchmark result with core performance level 1 to {0,3}.\n", CurrentMaxPerfLevel);
                }
                else
                {
                    Console.WriteLine("Benchmark result with core performance level 1.\n");
                }

                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_ENABLE, AIL.M_NULL);
                AIL.MthrInquireMp(AilSystemCurrentThreadId, AIL.M_CORE_NUM_EFFECTIVE, AIL.M_DEFAULT, AIL.M_DEFAULT, ref NbCoresUsed);
                if (NbCoresUsed > 1)
                {
                    Benchmark(ref ProcessingParam, ref TimeAllCores, ref FPSAllCores);

                    // Show the resulting image and the timing results.
                    AIL.MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage);

                    Console.WriteLine("Using multi-processing   ({0,3} CPU cores): {1,5:0.000} ms ({2,6:0.0} fps)", NbCoresUsed, TimeAllCores, FPSAllCores);
                }
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL);
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL);

                // Benchmark the processing function with multi-processing but no hyper-threading.
                // --------------------------------------------------------------------------------

                // If Hyper-threading is available on the PC, test if the performance is better 
                // with it disabled. Sometimes, too many cores sharing the same CPU resources that
                // are already fully occupied can reduce the overall performance.
                //
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_ENABLE, AIL.M_NULL);
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_DISABLE, AIL.M_NULL);
                AIL.MthrInquireMp(AilSystemCurrentThreadId, AIL.M_CORE_NUM_EFFECTIVE, AIL.M_DEFAULT, AIL.M_DEFAULT, ref NbCoresUsedNoCS);
                if (NbCoresUsedNoCS != NbCoresUsed)
                {
                    Benchmark(ref ProcessingParam, ref TimeAllCoresNoCS, ref FPSAllCoresNoCS);

                    // Show the resulting image and the timing results.
                    AIL.MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage);

                    Console.WriteLine("Using multi-processing   ({0,3} CPU cores): {1,5:0.000} ms ({2,6:0.0} fps), no Hyper-Thread.", NbCoresUsedNoCS, TimeAllCoresNoCS, FPSAllCoresNoCS);
                }
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL);
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_CORE_SHARING, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_NULL);

                // Disable the last performance level.
                AIL.MappControlMp(AilSystemOwnerApplication, AIL.M_MP_USE_PERFORMANCE_LEVEL, CurrentMaxPerfLevel, AIL.M_DISABLE, AIL.M_NULL);

                // Show a comparative summary of the timing results.
                if (NbCoresUsed > 1)
                {
                    Console.WriteLine("Benchmark is {0:0.0} times faster with multi-processing.", TimeOneCore / TimeAllCores);
                }

                if (NbCoresUsedNoCS != NbCoresUsed)
                {
                    Console.WriteLine("Benchmark is {0:0.0} times faster with multi-processing and no Hyper-Thread.", TimeOneCore / TimeAllCoresNoCS);
                }

                Console.WriteLine();
            }
            // Wait for a key press.
            Console.WriteLine("Press any key to end.");
            Console.ReadKey(true);

            // Free all allocations.
            ProcessingFree(ref ProcessingParam);
            AIL.MdispSelect(AilDisplay, AIL.M_NULL);
            AIL.MbufFree(AilDisplayImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //*****************************************************************************
        // Benchmark function.
        //*****************************************************************************
        private static void Benchmark(ref PROC_PARAM ProcParamPtr, ref double Time, ref double FramesPerSecond)
        {
            AIL_INT EstimatedNbLoop = DEFAULT_NB_LOOP;
            double StartTime = 0.0;
            double EndTime = 0.0;
            double MinTime = 0;
            AIL_INT n;

            // Wait for the completion of all functions in this thread.
            AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL);

            // Call the processing once before benchmarking for a more accurate time.
            // This compensates for Dll load time, etc.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref StartTime);
            ProcessingExecute(ref ProcParamPtr);
            AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL);
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref EndTime);

            MinTime = EndTime - StartTime;

            // Estimate the number of loops required to benchmark the processing for 
            // the specified minimum time.
            for (n = 0; n < ESTIMATION_NB_LOOP; n++)
            {
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref StartTime);
                ProcessingExecute(ref ProcParamPtr);
                AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL);
                AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref EndTime);

                Time = EndTime - StartTime;
                MinTime = (Time < MinTime) ? Time : MinTime;
            }

            if (MinTime > 0)
            {
                EstimatedNbLoop = (AIL_INT)(MINIMUM_BENCHMARK_TIME / MinTime) + 1;
            }

            // Benchmark the processing according to the estimated number of loops.
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref StartTime);
            for (n = 0; n < EstimatedNbLoop; n++)
            {
                ProcessingExecute(ref ProcParamPtr);
            }
            AIL.MthrWait(AIL.M_DEFAULT, AIL.M_THREAD_WAIT, AIL.M_NULL);
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, ref EndTime);

            Time = EndTime - StartTime;

            FramesPerSecond = EstimatedNbLoop / Time;
            Time = Time * 1000 / EstimatedNbLoop;
        }

        //*****************************************************************************
        // Processing initialization function.
        //*****************************************************************************
        private static void ProcessingInit(AIL_ID AilSystem, ref PROC_PARAM ProcParamPtr)
        {
            // Allocate a source buffer.
            AIL.MbufAllocColor(AilSystem,
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BAND, AIL.M_NULL),
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X, AIL.M_NULL),
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y, AIL.M_NULL),
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BIT, AIL.M_NULL) + AIL.M_UNSIGNED,
                     AIL.M_IMAGE + AIL.M_PROC, ref ProcParamPtr.AilSourceImage );

            // Load the image into the source image.
            AIL.MbufLoad(IMAGE_FILE, ProcParamPtr.AilSourceImage);

            // Allocate a destination buffer.
            AIL.MbufAllocColor(AilSystem,
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BAND, AIL.M_NULL),
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X, AIL.M_NULL),
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y, AIL.M_NULL),
                     AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_BIT, AIL.M_NULL) + AIL.M_UNSIGNED,
                     AIL.M_IMAGE + AIL.M_PROC, ref ProcParamPtr.AilDestinationImage);
        }

        //*****************************************************************************
        // Processing execution function.
        //*****************************************************************************
        private static void ProcessingExecute(ref PROC_PARAM ProcParamPtr)
        {
            AIL.MimRotate(ProcParamPtr.AilSourceImage, ProcParamPtr.AilDestinationImage, ROTATE_ANGLE,
                      AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_BILINEAR + AIL.M_OVERSCAN_CLEAR);
        }

        //*****************************************************************************
        // Processing free function.
        //*****************************************************************************
        private static void ProcessingFree(ref PROC_PARAM ProcParamPtr)
        {
            // Free all processing allocations.
            AIL.MbufFree(ProcParamPtr.AilSourceImage);
            AIL.MbufFree(ProcParamPtr.AilDestinationImage);
        }
    }
}

```
