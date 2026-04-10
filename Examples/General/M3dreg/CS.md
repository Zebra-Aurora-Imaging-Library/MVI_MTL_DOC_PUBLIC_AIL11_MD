---
title: "M3dreg"
description: "This program demonstrates how to use the 3D surface registration to register several partial point clouds of a 3D object and stitch them into a single complete point cloud."
ms.language: "C#"
---

# M3dreg

> This program demonstrates how to use the 3D surface registration to register several partial point clouds of a 3D object and stitch them into a single complete point cloud.

**Language:** C#

**Functions used:** `M3dregAlloc`, `M3dregAllocResult`, `M3dregControl`, `M3dregInquire`, `M3dregSetLocation`, `M3dregCalculate`, `M3dregMerge`, `M3dregFree`, `M3dimAlloc`, `M3dimAllocResult`, `M3dimControl`, `M3dimStat`, `M3dimGetResult`, `M3dimFree`, `M3ddispCopy`, `M3dgeoInquire`, `M3dgeoLine`, `MappAlloc`, `MappFree`, `MappTimer`, `MbufAllocContainer`, `MbufInquireContainer`, `MbufImport`, `MbufAllocColor`, `MbufAlloc2d`, `MbufClear`, `MbufFree`, `M3ddispAlloc`, `M3ddispControl`, `M3ddispSelect`, `M3ddispFree`, `MsysAlloc`, `MsysFree`, `M3ddispSetView`, `MappControl`, `MbufAllocComponent`, `MobjInquire`, `M3dimCalculateMapSize`, `M3dimCalibrateDepthMap`, `M3dimProject`, `M3dimRotate`, `MdispAlloc`, `MdispFree`, `MdispSelect`

**Categories:** Overview, General, Industries, Machining, Automation, Applications, Stitching, 3D profiling, Modules, 3D Registration, Buffer, 3D Display, 3D Graphics, Image Processing, 3D Image Processing, 3D Geometry, What's New, AIL 10.0 SP4

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dreg.cs
//
// Description: This example demonstrates how to use the 3D registration module
//              to stitch several partial point clouds of a 3D object together
//              into a single complete point cloud.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Threading;
using Zebra.AuroraImagingLibrary;

namespace M3dreg
{
    class Program
    {
        // ----------------------------------------------------------------------------
        // Example description.
        // ----------------------------------------------------------------------------
        private static void PrintHeader()
        {
            Console.WriteLine("[EXAMPLE NAME]");
            Console.WriteLine("M3dreg");
            Console.WriteLine();
            Console.WriteLine("[SYNOPSIS]");
            Console.WriteLine("This example demonstrates how to use the 3D Registration module ");
            Console.WriteLine("to stitch several partial point clouds of a 3D object together ");
            Console.WriteLine("into a single complete point cloud.");
            Console.WriteLine();

            Console.WriteLine("[MODULES USED]");
            Console.WriteLine("Modules used: 3D Registration, 3D Display, 3D Graphics, and 3D Image\nProcessing.");
            Console.WriteLine();
        }

        // Input scanned point cloud (PLY) files.
        private static readonly AIL_INT NUM_SCANS = 6;
        static readonly string[] FILE_POINT_CLOUD = new string[]
        {
               AIL.M_IMAGE_PATH + "Cloud1.ply",
               AIL.M_IMAGE_PATH + "Cloud2.ply",
               AIL.M_IMAGE_PATH + "Cloud3.ply",
               AIL.M_IMAGE_PATH + "Cloud4.ply",
               AIL.M_IMAGE_PATH + "Cloud5.ply",
               AIL.M_IMAGE_PATH + "Cloud6.ply",
        };

        // The colors assigned for each cloud.
        static readonly AIL_INT[] Color = new AIL_INT[]
        {
               AIL.M_RGB888(0,   159, 255),
               AIL.M_RGB888(154,  77,  66),
               AIL.M_RGB888(0,   255, 190),
               AIL.M_RGB888(120,  63, 193),
               AIL.M_RGB888(31,  150, 152),
               AIL.M_RGB888(255, 172, 253),
               AIL.M_RGB888(177, 204, 113)
        };

        // --------------------------------------------------------------
        private static void Main(string[] args)
        {
            // Print example information in console.
            PrintHeader();

            AIL_ID AilApplication = AIL.M_NULL;    // Application identifier
            AIL_ID AilSystem = AIL.M_NULL;         // System identifier
            AIL_ID AilDisplay = AIL.M_NULL;        // Display identifier

            // Allocate objects.
            AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT, ref AilApplication);
            AIL.MsysAlloc(AIL.M_DEFAULT, AIL.M_SYSTEM_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilSystem);

            AIL_ID[] AilContainerIds = new AIL_ID[NUM_SCANS];
            Console.Write("Reading the PLY files of 6 partial point clouds");
            for (AIL_INT i = 0; i < NUM_SCANS; ++i)
            {
                Console.Write(".");
                AilContainerIds[i] = AIL.MbufImport(FILE_POINT_CLOUD[i], AIL.M_DEFAULT, AIL.M_RESTORE, AilSystem, AIL.M_NULL);
                ColorCloud(AilContainerIds[i], Color[i]);
            }
            Console.WriteLine();
            Console.WriteLine();

            AilDisplay = Alloc3dDisplayId(AilSystem);

            AIL_ID AilDisplayImage = AIL.M_NULL; // Used for 2D display if needed.
            AIL_ID AilDepthMap     = AIL.M_NULL; // Used for 2D display if needed.

            // Display the first point cloud container.
            DisplayContainer(AilSystem, AilDisplay, AilContainerIds[0], ref AilDepthMap, ref AilDisplayImage);
            AutoRotateDisplay(AilSystem, AilDisplay);

            Console.WriteLine("Showing the first partial point cloud of the object.");
            Console.WriteLine("Press any key to start.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Allocate context and result for 3D registration (stitching).
            AIL_ID AilContext = AIL.M3dregAlloc(AilSystem, AIL.M_PAIRWISE_REGISTRATION_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL_ID AilResult  = AIL.M3dregAllocResult(AilSystem, AIL.M_PAIRWISE_REGISTRATION_RESULT, AIL.M_DEFAULT, AIL.M_NULL);

            AIL.M3dregControl(AilContext, AIL.M_DEFAULT, AIL.M_NUMBER_OF_REGISTRATION_ELEMENTS, NUM_SCANS);
            AIL.M3dregControl(AilContext, AIL.M_DEFAULT, AIL.M_MAX_ITERATIONS, 40);

            // Pairwise registration context controls.
            // Use normal subsampling to preserve edges and yield faster registration.
            AIL_ID AilSubsampleContext = AIL.M_NULL;
            AIL.M3dregInquire(AilContext, AIL.M_DEFAULT, AIL.M_SUBSAMPLE_CONTEXT_ID, ref AilSubsampleContext);
            AIL.M3dregControl(AilContext, AIL.M_DEFAULT, AIL.M_SUBSAMPLE, AIL.M_ENABLE);

            // Keep edge points.
            AIL.M3dimControl(AilSubsampleContext, AIL.M_SUBSAMPLE_MODE, AIL.M_SUBSAMPLE_NORMAL);
            AIL.M3dimControl(AilSubsampleContext, AIL.M_NEIGHBORHOOD_DISTANCE, 10);

            // Chain of set location, i==0 is referencing to the GLOBAL.
            for (AIL_INT i = 1; i < NUM_SCANS; ++i)
            {
                AIL.M3dregSetLocation(AilContext, i, i - 1, AIL.M_IDENTITY_MATRIX, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
            }

            Console.WriteLine("The 3D stitching between partial point clouds has been performed with");
            Console.WriteLine("the help of the points within the expected common overlap regions.");
            Console.WriteLine();

            // Calculate the time to perform the registration.
            double ComputationTimeMS;
            AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_RESET, AIL.M_NULL);

            // Perform the registration (stitching).
            AIL.M3dregCalculate(AilContext, AilContainerIds, NUM_SCANS, AilResult, AIL.M_DEFAULT);
            ComputationTimeMS = AIL.MappTimer(AIL.M_DEFAULT, AIL.M_TIMER_READ, AIL.M_NULL) * 1000.0;

            Console.Write("The registration of the 6 partial point clouds ");
            Console.WriteLine("succeeded in {0,0:f2} ms .", ComputationTimeMS);
            Console.WriteLine();

            // Merging overlapping point clouds will result into unneeded large number of points at
            // the overlapping regions.                                                            
            // During merging, subsampling is used to ensure that the density of points
            // is fairly dense, but without replications.
            double GridSize = 0.0;
            AIL_ID StatResultId = AIL.M3dimAllocResult(AilSystem, AIL.M_STATISTICS_RESULT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.M3dimStat(AIL.M_STAT_CONTEXT_DISTANCE_TO_NEAREST_NEIGHBOR, AilContainerIds[0], StatResultId, AIL.M_DEFAULT);

            // Nearest neighbour distances gives a measure of the point cloud density.
            AIL.M3dimGetResult(StatResultId, AIL.M_DISTANCE_TO_NEAREST_NEIGHBOR_MIN, ref GridSize);

            // Use the measured point cloud density as guide for the subsampling.
            AIL_ID AilMergeSubsampleContext = AIL.M3dimAlloc(AilSystem, AIL.M_SUBSAMPLE_CONTEXT, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_SUBSAMPLE_MODE, AIL.M_SUBSAMPLE_GRID);
            AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_GRID_SIZE_X, GridSize);
            AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_GRID_SIZE_Y, GridSize);
            AIL.M3dimControl(AilMergeSubsampleContext, AIL.M_GRID_SIZE_Z, GridSize);

            // Allocate the point cloud for the final stitched clouds.
            AIL_ID AilStitchedId = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC + AIL.M_DISP, AIL.M_DEFAULT, AIL.M_NULL);

            Console.WriteLine("The merging of point clouds will be shown incrementally.");
            Console.WriteLine("Press any key to show 2 merged point clouds of 6.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Merge can merge all clouds at once, but here it is done incrementally for the display
            for (AIL_INT i = 2; i <= NUM_SCANS; ++i)
            {
                AIL.M3dregMerge(AilResult, AilContainerIds, i, AilStitchedId, AilMergeSubsampleContext, AIL.M_DEFAULT);

                if (i == 2)
                {
                    DisplayContainer(AilSystem, AilDisplay, AilStitchedId, ref AilDepthMap, ref AilDisplayImage);
                }
                else
                {
                    UpdateDisplay(AilSystem, AilStitchedId, AilDepthMap, AilDisplayImage);
                }

                if (i < NUM_SCANS)
                {
                    Console.WriteLine("Press any key to show {0} merged point clouds of 6.", i + 1);
                    Console.WriteLine();
                }
                else
                {
                    Console.WriteLine("Press any key to end.");
                    Console.WriteLine();
                }

                AutoRotateDisplay(AilSystem, AilDisplay);
                Console.ReadKey(true);
            }

            // Free Objects
            for (AIL_INT i = 0; i < NUM_SCANS; ++i)
            {
                AIL.MbufFree(AilContainerIds[i]);
            }

            AIL.MbufFree(AilStitchedId);
            AIL.M3dimFree(StatResultId);
            AIL.M3dimFree(AilMergeSubsampleContext);
            AIL.M3dregFree(AilContext);
            AIL.M3dregFree(AilResult);
            FreeDisplay(AilDisplay);

            if (AilDisplayImage != AIL.M_NULL)
            {
                AIL.MbufFree(AilDisplayImage);
            }

            if (AilDepthMap != AIL.M_NULL)
            {
                AIL.MbufFree(AilDepthMap);
            }

            AIL.MsysFree(AilSystem);
            AIL.MappFree(AilApplication);
        }

        // --------------------------------------------------------------
        // Color the container  
        // --------------------------------------------------------------
        private static void ColorCloud(AIL_ID AilPointCloud, AIL_INT Col)
        {
            AIL_INT SizeX = AIL.MbufInquireContainer(AilPointCloud, AIL.M_COMPONENT_RANGE, AIL.M_SIZE_X, AIL.M_NULL);
            AIL_INT SizeY = AIL.MbufInquireContainer(AilPointCloud, AIL.M_COMPONENT_RANGE, AIL.M_SIZE_Y, AIL.M_NULL);
            AIL_ID ReflectanceId = AIL.MbufAllocComponent(AilPointCloud, 3, SizeX, SizeY, 8 + AIL.M_UNSIGNED, AIL.M_IMAGE, AIL.M_COMPONENT_REFLECTANCE, AIL.M_NULL);
            AIL.MbufClear(ReflectanceId, (double)Col);
        }

        // --------------------------------------------------------------
        // Auto rotate the 3D object
        // --------------------------------------------------------------
        private static void AutoRotateDisplay(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            long DisplayType = 0;
            AIL.MobjInquire(AilDisplay, AIL.M_OBJECT_TYPE, ref DisplayType);

            // AutoRotate available only for the 3D display.
            if (DisplayType == AIL.M_DISPLAY)
            {
                return;
            }

            // By default the display rotates around the Z axis, but the robot is oriented along the Y axis. 
            AIL_ID Geometry = AIL.M3dgeoAlloc(AilSystem, AIL.M_GEOMETRY, AIL.M_DEFAULT, AIL.M_NULL);
            AIL.M3ddispCopy(AilDisplay, Geometry, AIL.M_ROTATION_AXIS, AIL.M_DEFAULT);
            double CenterX = AIL.M3dgeoInquire(Geometry, AIL.M_START_POINT_X, AIL.M_NULL);
            double CenterY = AIL.M3dgeoInquire(Geometry, AIL.M_START_POINT_Y, AIL.M_NULL);
            double CenterZ = AIL.M3dgeoInquire(Geometry, AIL.M_START_POINT_Z, AIL.M_NULL);
            AIL.M3dgeoLine(Geometry, AIL.M_POINT_AND_VECTOR, AIL.M_UNCHANGED, AIL.M_UNCHANGED, AIL.M_UNCHANGED, 0, 1, 0, AIL.M_UNCHANGED, AIL.M_DEFAULT);
            AIL.M3ddispCopy(Geometry, AilDisplay, AIL.M_ROTATION_AXIS, AIL.M_DEFAULT);
            AIL.M3ddispControl(AilDisplay, AIL.M_AUTO_ROTATE, AIL.M_ENABLE);
            AIL.M3dgeoFree(Geometry);

        }

        // --------------------------------------------------------------
        // Allocates a 3D display and returns its identifier.
        // --------------------------------------------------------------
        private static AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
        {
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
            AIL_ID AilDisplay = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            if (AilDisplay == AIL.M_NULL)
            {
                Console.WriteLine();

                Console.WriteLine("The current system does not support the 3D display.");
                Console.WriteLine("A 2D display will be used instead.");
                Console.WriteLine("Press any key to continue.");
                Console.ReadKey(true);

                // Allocate a 2D Display instead.
                AilDisplay = AIL.MdispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_WINDOWED, AIL.M_NULL);
            }
            else
            {
                // Adjust the viewpoint of the 3D display.
                AIL.M3ddispSetView(AilDisplay, AIL.M_AUTO, AIL.M_BOTTOM_VIEW, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
                Console.WriteLine("Press <R> on the display window to stop/start the rotation.");
                Console.WriteLine();
                }

            return AilDisplay;
        }

        // --------------------------------------------------------------
        // Display the received container.
        // --------------------------------------------------------------
        private static void DisplayContainer(AIL_ID AilSystem, AIL_ID AilDisplay, AIL_ID AilContainer, ref AIL_ID AilDepthMap, ref AIL_ID AilIntensityMap)
        {
            long DisplayType = 0;
            AIL.MobjInquire(AilDisplay, AIL.M_OBJECT_TYPE, ref DisplayType);

            bool Use3D = (DisplayType == AIL.M_3D_DISPLAY);

            if (Use3D)
            {
                AIL.M3ddispSelect(AilDisplay, AilContainer, AIL.M_ADD, AIL.M_DEFAULT);
                AIL.M3ddispSelect(AilDisplay, AIL.M_NULL, AIL.M_OPEN, AIL.M_DEFAULT);
            }
            else
            {
                if (AilDepthMap == AIL.M_NULL)
                {
                    AIL_INT SizeX = 0;         // Image size X for 2d display
                    AIL_INT SizeY = 0;         // Image size Y for 2d display

                    AIL.M3dimCalculateMapSize(AIL.M_DEFAULT, AilContainer, AIL.M_NULL, AIL.M_DEFAULT, ref SizeX, ref SizeY);

                    AilIntensityMap = AIL.MbufAllocColor(AilSystem, 3, SizeX, SizeY, AIL.M_UNSIGNED + 8, AIL.M_IMAGE | AIL.M_PROC | AIL.M_DISP, AIL.M_NULL);
                    AilDepthMap = AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, AIL.M_UNSIGNED + 8, AIL.M_IMAGE | AIL.M_PROC | AIL.M_DISP, AIL.M_NULL);

                    AIL.M3dimCalibrateDepthMap(AilContainer, AilDepthMap, AilIntensityMap, AIL.M_NULL, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_CENTER);
                }

                // Rotate the point cloud container to be in the xy plane before projecting.
                AIL_ID RotatedContainer = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC, AIL.M_DEFAULT, AIL.M_NULL);

                AIL.M3dimRotate(AilContainer, RotatedContainer, AIL.M_ROTATION_XYZ, 90, 60, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
                AIL.M3dimProject(AilContainer, AilDepthMap, AilIntensityMap, AIL.M_DEFAULT, AIL.M_MIN_Z, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Display the projected point cloud container.
                AIL.MdispSelect(AilDisplay, AilIntensityMap);
                AIL.MbufFree(RotatedContainer);
            }
        }

        // --------------------------------------------------------------
        // Updated the displayed image.
        // --------------------------------------------------------------
        private static void UpdateDisplay(AIL_ID AilSystem, AIL_ID AilContainer, AIL_ID AilDepthMap, AIL_ID AilIntensityMap)
        {
            if (AilDepthMap == AIL.M_NULL)
            {
                return;
            }

            AIL_ID RotatedContainer = AIL.MbufAllocContainer(AilSystem, AIL.M_PROC, AIL.M_DEFAULT, AIL.M_NULL);

            AIL.M3dimRotate(AilContainer, RotatedContainer, AIL.M_ROTATION_XYZ, 90, 70, 0, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT, AIL.M_DEFAULT);
            AIL.M3dimProject(AilContainer, AilDepthMap, AilIntensityMap, AIL.M_DEFAULT, AIL.M_MIN_Z, AIL.M_DEFAULT, AIL.M_DEFAULT);

            AIL.MbufFree(RotatedContainer);
        }

        // --------------------------------------------------------------
        // Free the display.
        // --------------------------------------------------------------
        private static void FreeDisplay(AIL_ID AilDisplay)
        {
            long DisplayType = 0;
            AIL.MobjInquire(AilDisplay, AIL.M_OBJECT_TYPE, ref DisplayType);

            if (DisplayType == AIL.M_DISPLAY)
            {
                AIL.MdispFree(AilDisplay);
            }
            else
            {
                AIL.M3ddispFree(AilDisplay);
            }
        }
    }
}

```
