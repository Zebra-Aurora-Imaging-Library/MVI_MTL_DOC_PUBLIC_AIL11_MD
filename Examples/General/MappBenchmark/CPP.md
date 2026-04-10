---
title: "MappBenchmark"
description: "This program uses the MappTimer function to demonstrate the benchmarking of AIL functions."
ms.language: "C++"
---

# MappBenchmark

> This program uses the MappTimer function to demonstrate the benchmarking of AIL functions.

**Language:** C++

**Functions used:** `MappAlloc`, `MappControlMp`, `MappFree`, `MappTimer`, `MbufAllocColor`, `MbufCopy`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimRotate`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MthrInquireMp`, `MthrWait`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Image Processing, What's New, AIL 10.0 SP6, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MappBenchmark.cpp
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

#include <AIL.h>

// Target image specifications. 
#define IMAGE_FILE   M_IMAGE_PATH AIL_TEXT("LargeWafer.mim")
#define ROTATE_ANGLE -15

// Timing loop iterations setting. 
#define MINIMUM_BENCHMARK_TIME 2.0 // In seconds (1.0 and more recommended).
#define ESTIMATION_NB_LOOP      10
#define DEFAULT_NB_LOOP        100

// Processing function parameters structure. 
typedef struct 
   {
   AIL_ID AilSourceImage;        // Image buffer identifier.
   AIL_ID AilDestinationImage;   // Image buffer identifier.
   } PROC_PARAM;

// Declaration of the benchmarking function. 
void Benchmark(PROC_PARAM& ProcParamPtr, AIL_DOUBLE& Time, AIL_DOUBLE& FramesPerSecond);

// Declarations of the target processing functions. You can insert your 
// own intialization, execution and free operations in these functions. 
void ProcessingInit(AIL_ID AilSystem, PROC_PARAM& ProcParamPtr);
void ProcessingExecute(PROC_PARAM& ProcParamPtr);
void ProcessingFree(PROC_PARAM& ProcParamPtr);

int MosMain(void)
   {
   AIL_ID AilApplication,              // Application identifier.  
          AilSystem,                   // System identifier.       
          AilDisplay,                  // Display identifier.      
          AilDisplayImage,             // Image buffer identifier.    
          AilSystemOwnerApplication,   // System's owner application.          
          AilSystemCurrentThreadId;    // System's current thread identifier.  
   PROC_PARAM ProcessingParam;         // Processing parameters.               
   AIL_DOUBLE TimeAllCores, TimeAllCoresNoCS, TimeOneCore; // Timer variables. 
   AIL_DOUBLE FPSAllCores, FPSAllCoresNoCS, FPSOneCore;    // FPS variables.   
   AIL_INT    NbCoresUsed, NbCoresUsedNoCS;                // Number of CPU Core used.
   AIL_INT    NbPerformanceLevel;      // Number of performance level.
   AIL_INT    CurrentMaxPerfLevel;

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem,
                               &AilDisplay, M_NULL, M_NULL);

   // Get the system's owner application.
   MsysInquire(AilSystem, M_OWNER_APPLICATION, &AilSystemOwnerApplication);

   // Get the system's current thread identifier. 
   MsysInquire(AilSystem, M_CURRENT_THREAD_ID, &AilSystemCurrentThreadId);

   // Restore image into an automatically allocated image buffer. 
   MbufRestore(IMAGE_FILE, AilSystem, &AilDisplayImage);

   // Display the image buffer. 
   MdispSelect(AilDisplay, AilDisplayImage);

   // Allocate the processing objects. 
   ProcessingInit(AilSystem, ProcessingParam);

   // Pause to show the original image. 
   MosPrintf(AIL_TEXT("\nPROCESSING FUNCTION BENCHMARKING:\n"));
   MosPrintf(AIL_TEXT("---------------------------------\n\n"));
   MosPrintf(AIL_TEXT("This program times a processing function under "));
   MosPrintf(AIL_TEXT("different conditions.\n"));
   MosPrintf(AIL_TEXT("Press any key to start.\n\n"));
   MosGetch();
   MosPrintf(AIL_TEXT("PROCESSING TIME FOR %lldx%lld:\n" ),
             (long long)MbufInquire(ProcessingParam.AilDestinationImage, M_SIZE_X, M_NULL),
             (long long)MbufInquire(ProcessingParam.AilDestinationImage, M_SIZE_Y, M_NULL));
   MosPrintf(AIL_TEXT("------------------------------\n\n"));

   // Benchmark the processing function without multi-processing. 
   // ------------------------------------------------------------
   MappControlMp(AilSystemOwnerApplication, M_MP_USE, M_DEFAULT, M_DISABLE, M_NULL);   
   Benchmark(ProcessingParam, TimeOneCore, FPSOneCore);

   // Show the resulting image and the timing results. 
   MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage);
   MosPrintf(AIL_TEXT("Without multi-processing (  1 CPU core ): %5.3f ms (%6.1f fps)\n\n"),
               TimeOneCore, FPSOneCore);               
   MappControlMp(AilSystemOwnerApplication, M_MP_USE, M_DEFAULT, M_DEFAULT, M_NULL);   


   // Benchmark the processing function with multi-processing. 
   // ---------------------------------------------------------
   MappControlMp(AilSystemOwnerApplication, M_MP_USE, M_DEFAULT, M_ENABLE, M_NULL);

   // Enable all performance level 
   MappControlMp(AilSystemOwnerApplication, M_MP_USE_PERFORMANCE_LEVEL, M_ALL, M_ENABLE, M_NULL);

   // Inquire the number of performance level. This will return 1 on non-hybrid processor. 
   MappInquireMp(AilSystemOwnerApplication, M_MP_NB_PERFORMANCE_LEVEL, M_DEFAULT, M_DEFAULT, &NbPerformanceLevel);

   for(CurrentMaxPerfLevel = NbPerformanceLevel; CurrentMaxPerfLevel > 0; CurrentMaxPerfLevel--)
      {
      if(CurrentMaxPerfLevel > 1)
         MosPrintf(AIL_TEXT("Benchmark result with core performance level 1 to %d.\n\n"), CurrentMaxPerfLevel);
      else
         MosPrintf(AIL_TEXT("Benchmark result with core performance level 1.\n\n"), CurrentMaxPerfLevel);

      MappControlMp(AilSystemOwnerApplication, M_CORE_SHARING, M_DEFAULT, M_ENABLE, M_NULL); 
      MthrInquireMp(AilSystemCurrentThreadId, M_CORE_NUM_EFFECTIVE, M_DEFAULT, M_DEFAULT, &NbCoresUsed);
      if (NbCoresUsed > 1)
         {
         Benchmark(ProcessingParam, TimeAllCores, FPSAllCores);

         // Show the resulting image and the timing results. 
         MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage);
   
         MosPrintf(AIL_TEXT("Using multi-processing   (%3d CPU cores): %5.3f ms (%6.1f fps)\n"), 
                     (int)NbCoresUsed, TimeAllCores, FPSAllCores);
         }
      MappControlMp(AilSystemOwnerApplication, M_MP_USE, M_DEFAULT, M_DEFAULT, M_NULL);   
      MappControlMp(AilSystemOwnerApplication, M_CORE_SHARING, M_DEFAULT, M_DEFAULT, M_NULL); 

      // Benchmark the processing function with multi-processing but no hyper-threading. 
      // --------------------------------------------------------------------------------

      // If Hyper-threading is available on the PC, test if the performance is better 
      // with it disabled. Sometimes, too many cores sharing the same CPU resources that
      // are already fully occupied can reduce the overall performance.
      MappControlMp(AilSystemOwnerApplication, M_MP_USE, M_DEFAULT, M_ENABLE, M_NULL);   
      MappControlMp(AilSystemOwnerApplication, M_CORE_SHARING, M_DEFAULT, M_DISABLE, M_NULL); 
      MthrInquireMp(AilSystemCurrentThreadId, M_CORE_NUM_EFFECTIVE, M_DEFAULT, M_DEFAULT, &NbCoresUsedNoCS);
      if (NbCoresUsedNoCS != NbCoresUsed)
         {
         Benchmark(ProcessingParam, TimeAllCoresNoCS, FPSAllCoresNoCS);

         // Show the resulting image and the timing results. 
         MbufCopy(ProcessingParam.AilDestinationImage, AilDisplayImage);
         MosPrintf(AIL_TEXT("Using multi-processing   (%3d CPU cores): %5.3f ms (%6.1f fps), no Hyper-Thread.\n"), 
                     (int)NbCoresUsedNoCS, TimeAllCoresNoCS, FPSAllCoresNoCS);
         }
      MappControlMp(AilSystemOwnerApplication, M_MP_USE, M_DEFAULT, M_DEFAULT, M_NULL);   
      MappControlMp(AilSystemOwnerApplication, M_CORE_SHARING, M_DEFAULT, M_DEFAULT, M_NULL);

      // Disable the last performance level. 
      MappControlMp(AilSystemOwnerApplication, M_MP_USE_PERFORMANCE_LEVEL, CurrentMaxPerfLevel, M_DISABLE, M_NULL);

      // Show a comparative summary of the timing results. 
      if (NbCoresUsed > 1)
         {
         MosPrintf(AIL_TEXT("Benchmark is %.1f times faster with multi-processing.\n"), 
                     TimeOneCore/TimeAllCores);
         }
      if (NbCoresUsedNoCS != NbCoresUsed)
         {
         MosPrintf(AIL_TEXT("Benchmark is %.1f times faster with multi-processing and no Hyper-Thread.\n\n"), 
                     TimeOneCore/TimeAllCoresNoCS);
         }
      }
     
   // Wait for a key press. 
   MosPrintf(AIL_TEXT("Press any key to end.\n"));
   MosGetch();

   // Free all allocations. 
   ProcessingFree(ProcessingParam);
   MdispSelect(AilDisplay, M_NULL);
   MbufFree(AilDisplayImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);   
   return 0;
   }

//*****************************************************************************
// Benchmark function. 
//*****************************************************************************
void Benchmark(PROC_PARAM& ProcParamPtr, AIL_DOUBLE& Time, AIL_DOUBLE& FramesPerSecond)
   {
   AIL_INT EstimatedNbLoop = DEFAULT_NB_LOOP;
   AIL_DOUBLE StartTime, EndTime;
   AIL_DOUBLE MinTime = 0;
   AIL_INT n;

   // Wait for the completion of all functions in this thread. 
   MthrWait(M_DEFAULT, M_THREAD_WAIT, M_NULL);

   // Call the processing once before benchmarking for a more accurate time.
   // This compensates for Dll load time, etc.
   MappTimer(M_DEFAULT, M_TIMER_READ, &StartTime);
   ProcessingExecute(ProcParamPtr);  
   MthrWait(M_DEFAULT, M_THREAD_WAIT, M_NULL);
   MappTimer(M_DEFAULT, M_TIMER_READ, &EndTime);

   MinTime = EndTime-StartTime;

   // Estimate the number of loops required to benchmark the processing for 
   // the specified minimum time.
   for (n = 0; n < ESTIMATION_NB_LOOP; n++)
      {
      MappTimer(M_DEFAULT, M_TIMER_READ, &StartTime);
      ProcessingExecute(ProcParamPtr);
      MthrWait(M_DEFAULT, M_THREAD_WAIT, M_NULL);
      MappTimer(M_DEFAULT, M_TIMER_READ, &EndTime);

      Time = EndTime-StartTime;
      MinTime = (Time<MinTime)?Time:MinTime;
      }
   if (MinTime > 0) 
      EstimatedNbLoop = (AIL_INT)(MINIMUM_BENCHMARK_TIME/MinTime)+1;

   // Benchmark the processing according to the estimated number of loops. 
   MappTimer(M_DEFAULT, M_TIMER_READ, &StartTime);
   for (n = 0; n < EstimatedNbLoop; n++)
      ProcessingExecute(ProcParamPtr);
   MthrWait(M_DEFAULT, M_THREAD_WAIT, M_NULL);
   MappTimer(M_DEFAULT, M_TIMER_READ, &EndTime);

   Time = EndTime-StartTime;

   FramesPerSecond = EstimatedNbLoop/Time;
   Time = Time*1000/EstimatedNbLoop;
   }

//*****************************************************************************
// Processing initialization function.
//*****************************************************************************
void ProcessingInit(AIL_ID AilSystem, PROC_PARAM& ProcParamPtr)
   {
   // Allocate a source buffer. 
   MbufAllocColor(AilSystem, 
            MbufDiskInquire(IMAGE_FILE, M_SIZE_BAND, M_NULL),
            MbufDiskInquire(IMAGE_FILE, M_SIZE_X, M_NULL),
            MbufDiskInquire(IMAGE_FILE, M_SIZE_Y, M_NULL),
            MbufDiskInquire(IMAGE_FILE, M_SIZE_BIT, M_NULL)+M_UNSIGNED,
            M_IMAGE+M_PROC, &ProcParamPtr.AilSourceImage);

   // Load the image into the source image.  
   MbufLoad(IMAGE_FILE, ProcParamPtr.AilSourceImage);

   // Allocate a destination buffer. 
   MbufAllocColor(AilSystem, 
            MbufDiskInquire(IMAGE_FILE, M_SIZE_BAND, M_NULL),
            MbufDiskInquire(IMAGE_FILE, M_SIZE_X, M_NULL),
            MbufDiskInquire(IMAGE_FILE, M_SIZE_Y, M_NULL),
            MbufDiskInquire(IMAGE_FILE, M_SIZE_BIT, M_NULL)+M_UNSIGNED,
            M_IMAGE+M_PROC, &ProcParamPtr.AilDestinationImage);
   }

//*****************************************************************************
// Processing execution function.
//*****************************************************************************
void ProcessingExecute(PROC_PARAM& ProcParamPtr)
   {
   MimRotate(ProcParamPtr.AilSourceImage, ProcParamPtr.AilDestinationImage, ROTATE_ANGLE,
             M_DEFAULT, M_DEFAULT, M_DEFAULT, M_DEFAULT, M_BILINEAR+M_OVERSCAN_CLEAR);
   }

//*****************************************************************************
// Processing free function.
//*****************************************************************************
void ProcessingFree(PROC_PARAM& ProcParamPtr)
   {
   // Free all processing allocations.  
   MbufFree(ProcParamPtr.AilSourceImage);
   MbufFree(ProcParamPtr.AilDestinationImage);
   }

```
