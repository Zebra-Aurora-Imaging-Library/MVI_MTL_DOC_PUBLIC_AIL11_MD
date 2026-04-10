---
title: "MgraInteractive"
description: "This program uses the capabilities of AIL's interactive graphics and extracts the blobs within a user defined region."
ms.language: "C#"
---

# MgraInteractive

> This program uses the capabilities of AIL's interactive graphics and extracts the blobs within a user defined region.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MblobAlloc`, `MblobAllocResult`, `MblobCalculate`, `MblobControl`, `MblobDraw`, `MblobFree`, `MblobGetResult`, `MbufAlloc2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MbufSetRegion`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MgraAlloc`, `MgraAllocList`, `MgraControl`, `MgraControlList`, `MgraFree`, `MgraGetHookInfo`, `MgraHookFunction`, `MgraInquireList`, `MgraRectAngle`, `MgraText`, `MimBinarize`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Finding/Locating, Counting, Modules, Buffer, Display, Graphics, Image Processing, Blob Analysis, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MGraInteractive.cpp
//
// Description: This program uses the capabilities of AIL's interactive
//              graphics, along with the Blob analysis module, to count 
//              objects within a user-defined region.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;
using System.Runtime.InteropServices;

namespace MGraInteractive
{
    public class STestParameters
    {
        public AIL_ID AilDisplay;
        public AIL_ID AilGraphicsList;
        public AIL_ID AilGraphicsContext;
        public AIL_ID AilBinImage;
        public AIL_ID AilBlobContext;
        public AIL_ID AilBlobResult;
        public AIL_INT RegionLabel;
    }

    public class Program
    {
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System Identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Print example name.
            Console.WriteLine();
            Console.WriteLine("INTERACTIVE REGIONS AND SUBPIXEL ANNOTATIONS:");
            Console.WriteLine("---------------------------------------------");
            Console.WriteLine();
            Console.WriteLine("This program determines the number of blobs in a region");
            Console.WriteLine("defined interactively by the user. The extracted blob's");
            Console.WriteLine("features are drawn with subpixel accuracy in a zoomable");
            Console.WriteLine("display.");
            Console.WriteLine();

            // Run Interactivity Example.
            InteractivityExample(AilSystem, AilDisplay);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }

        //*****************************************************************************
        // Interactivity example.

        // Source image file specification.
        private static readonly string IMAGE_FILE = AIL.M_IMAGE_PATH + "Seals.mim";

        // Threshold for image binarization.
        private const int IMAGE_THRESHOLD_VALUE = 110;

        // Initial region parameters.
        private const int RECTANGLE_POSITION_X = 160;
        private const int RECTANGLE_POSITION_Y = 310;
        private const int RECTANGLE_WIDTH = 200;
        private const int RECTANGLE_HEIGHT = 175;
        private const int RECTANGLE_ANGLE = 0;
        // Value of enter key in Ascii table
        private const int ASCII_ENTER = 13; 

        private static void InteractivityExample(AIL_ID AilSystem, AIL_ID AilDisplay)
        {
            AIL_ID AilImage = AIL.M_NULL;                           // Image buffer identifier.
            AIL_ID AilGraphicsList = AIL.M_NULL;                    // Graphics list identifier.
            AIL_ID AilGraphicsContext = AIL.M_NULL;                 // Graphics context identifier.
            AIL_ID AilBinImage = AIL.M_NULL;                        // Binary image buffer identifier.
            AIL_ID AilBlobContext = AIL.M_NULL;                     // Context identifier.
            AIL_ID AilBlobResult = AIL.M_NULL;                      // Blob result buffer identifier.

            AIL_INT SizeX = 0;                                      // Size X of the source buffer.
            AIL_INT SizeY = 0;                                      // Size Y of the source buffer.
            AIL_INT RegionLabel = 0;                                // Label value of the region.

            STestParameters DataStructure = new STestParameters();  // Hook function data structure.

            // Restore the source image.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);

            // Display the buffer.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphics list to hold the subpixel annotations.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref AilGraphicsList);

            // Associate the graphics list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, AilGraphicsList);

            // Allocate a graphics context for the draw operations.
            AIL.MgraAlloc(AilSystem, ref AilGraphicsContext);

            // Enable the interactive mode.
            AIL.MdispControl(AilDisplay, AIL.M_GRAPHIC_LIST_INTERACTIVE, AIL.M_ENABLE);

            // Enable the use of action keys.
            AIL.MgraControlList(AilGraphicsList, AIL.M_LIST, AIL.M_DEFAULT, AIL.M_ACTION_KEYS, AIL.M_ENABLE);

            // Add a selectable rectangular region.
            AIL.MgraRectAngle(AilGraphicsContext, AilGraphicsList, RECTANGLE_POSITION_X, RECTANGLE_POSITION_Y, RECTANGLE_WIDTH, RECTANGLE_HEIGHT, RECTANGLE_ANGLE, AIL.M_CENTER_AND_DIMENSION);

            // Retrieve the label of the rectangle graphic.
            AIL.MgraInquireList(AilGraphicsList, AIL.M_LIST, AIL.M_DEFAULT, AIL.M_LAST_LABEL, ref RegionLabel);

            // Modify the line thickness of the rectangle.
            AIL.MgraControlList(AilGraphicsList, AIL.M_GRAPHIC_LABEL(RegionLabel),AIL.M_DEFAULT,AIL.M_LINE_THICKNESS, 5.0);

            // Disable the selectable mode for the next annotations to the graphics list.
            AIL.MgraControl(AilGraphicsContext, AIL.M_SELECTABLE, AIL.M_DISABLE);

            // Allocate a binary image buffer for fast processing.
            AIL.MbufInquire(AilImage, AIL.M_SIZE_X, ref SizeX);
            AIL.MbufInquire(AilImage, AIL.M_SIZE_Y, ref SizeY);
            AIL.MbufAlloc2d(AilSystem, SizeX, SizeY, 1 + AIL.M_UNSIGNED, AIL.M_IMAGE + AIL.M_PROC, ref AilBinImage);

            // Binarize the source image.
            AIL.MimBinarize(AilImage, AilBinImage, AIL.M_FIXED + AIL.M_LESS, IMAGE_THRESHOLD_VALUE, AIL.M_NULL);

            // Allocate a blob context and a blob result.
            AIL.MblobAlloc(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBlobContext);
            AIL.MblobAllocResult(AilSystem, AIL.M_DEFAULT, AIL.M_DEFAULT, ref AilBlobResult);

            // Select the blob features to calculate (Center Of Gravity and Box).
            AIL.MblobControl(AilBlobContext, AIL.M_CENTER_OF_GRAVITY + AIL.M_BINARY, AIL.M_ENABLE);
            AIL.MblobControl(AilBlobContext, AIL.M_BOX, AIL.M_ENABLE);

            // Programmatically initialize the selected state of the rectangle region.
            AIL.MgraControlList(AilGraphicsList, AIL.M_GRAPHIC_LABEL(RegionLabel), AIL.M_DEFAULT, AIL.M_GRAPHIC_SELECTED, AIL.M_TRUE);

            // Perform and display a first count of the number of objects
            // within the initial region.                                
            CountObjects(AilDisplay, AilGraphicsList, AilGraphicsContext, AilBinImage, AilBlobContext, AilBlobResult);

            // Initialize the hook data structure, then associate the hook function to
            // the "AIL.M_GRAPHIC_MODIFIED" event. The hook function will be called
            // with any region interaction by the user.
            DataStructure.AilDisplay = AilDisplay;
            DataStructure.AilGraphicsList = AilGraphicsList;
            DataStructure.AilGraphicsContext = AilGraphicsContext;
            DataStructure.AilBinImage = AilBinImage;
            DataStructure.RegionLabel = RegionLabel;
            DataStructure.AilBlobContext = AilBlobContext;
            DataStructure.AilBlobResult = AilBlobResult;

            GCHandle DataStructureHandle = GCHandle.Alloc(DataStructure);
            AIL_GRA_HOOK_FUNCTION_PTR HookHandlerDelegate = new AIL_GRA_HOOK_FUNCTION_PTR(HookHandler);
            AIL.MgraHookFunction(AilGraphicsList, AIL.M_GRAPHIC_MODIFIED, HookHandlerDelegate, GCHandle.ToIntPtr(DataStructureHandle));

            Console.WriteLine("You can try using your mouse or your keyboard to interactively");
            Console.WriteLine("modify the displayed region, such as moving, resizing, or");
            Console.WriteLine("rotating the region. If you do so, the results and annotations");
            Console.WriteLine("will be immediately updated.");
            Console.WriteLine();

            Console.WriteLine("Press Enter to exit.");

            int PressedKey = 0;
            while (PressedKey != ASCII_ENTER)
               PressedKey = (int)Console.ReadKey(true).Key;

            AIL.MgraHookFunction(AilGraphicsList, AIL.M_GRAPHIC_MODIFIED + AIL.M_UNHOOK, HookHandlerDelegate, GCHandle.ToIntPtr(DataStructureHandle));
            DataStructureHandle.Free();

            // Free all allocations.
            AIL.MblobFree(AilBlobResult);
            AIL.MblobFree(AilBlobContext);
            AIL.MbufFree(AilBinImage);
            AIL.MgraFree(AilGraphicsContext);
            AIL.MgraFree(AilGraphicsList);
            AIL.MbufFree(AilImage);
        }

        private static AIL_INT HookHandler(AIL_INT HookType, AIL_ID EventId, IntPtr UserDataPtr)
        {
            // this is how to check if the user data is null, the IntPtr class
            // contains a member, Zero, which exists solely for this purpose
            if (!IntPtr.Zero.Equals(UserDataPtr))
            {
                // get the handle to the DigHookUserData object back from the IntPtr
                GCHandle hUserData = GCHandle.FromIntPtr(UserDataPtr);

                // get a reference to the DigHookUserData object
                STestParameters DataStructure = hUserData.Target as STestParameters;

                // Check that the modified graphic is the rectangular region.
                AIL_INT ModifiedGraphicLabel = 0;
                AIL.MgraGetHookInfo(EventId, AIL.M_GRAPHIC_LABEL_VALUE, ref ModifiedGraphicLabel);

                if (ModifiedGraphicLabel == DataStructure.RegionLabel)
                {

                    // Count objects and draw the corresponding annotations.
                    CountObjects(DataStructure.AilDisplay,
                                 DataStructure.AilGraphicsList,
                                 DataStructure.AilGraphicsContext,
                                 DataStructure.AilBinImage,
                                 DataStructure.AilBlobContext,
                                 DataStructure.AilBlobResult);
                }
            }

            return AIL.M_NULL;
        }


        private const int MAX_TEXT_SIZE = 100;

        private static void CountObjects(AIL_ID AilDisplay, AIL_ID AilGraphicsList, AIL_ID AilGraphicsContext, AIL_ID AilBinImage, AIL_ID AilBlobContext, AIL_ID AilBlobResult)
        {
            AIL_INT NumberOfBlobs = 0;
            AIL_INT NumberOfPrimitives = 0;
            AIL_INT Index;

            string TextLabel;

            // Disable the display update for better performance.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_DISABLE);

            // Remove all elements from the graphics list, except the rectangle
            // region primitive at index 0.
            AIL.MgraInquireList(AilGraphicsList, AIL.M_LIST, AIL.M_DEFAULT, AIL.M_NUMBER_OF_GRAPHICS, ref NumberOfPrimitives);
            for (Index = NumberOfPrimitives-1; Index > 0; Index--)
            {
                AIL.MgraControlList(AilGraphicsList, AIL.M_GRAPHIC_INDEX(Index), AIL.M_DEFAULT, AIL.M_DELETE, AIL.M_DEFAULT);
            }

            // Set the input region. The blob analysis will be done
            // from the (filled) interactive rectangle.
            AIL.MbufSetRegion(AilBinImage, AilGraphicsList, AIL.M_DEFAULT, AIL.M_RASTERIZE + AIL.M_FILL_REGION + AIL.M_USE_LINE_THICKNESS_1, AIL.M_DEFAULT);

            // Calculate the blobs and their features.
            AIL.MblobCalculate(AilBlobContext, AilBinImage, AIL.M_NULL, AilBlobResult);

            // Get the total number of blobs.
            AIL.MblobGetResult(AilBlobResult, AIL.M_GENERAL, AIL.M_NUMBER + AIL.M_TYPE_AIL_INT, ref NumberOfBlobs);

            // Set the input units to display unit for the count annotations.
            AIL.MgraControl(AilGraphicsContext, AIL.M_INPUT_UNITS, AIL.M_DISPLAY);
            TextLabel = string.Format(" Number of blobs found: {0,2} ", NumberOfBlobs);

            AIL.MgraControl(AilGraphicsContext, AIL.M_COLOR, AIL.M_COLOR_WHITE);
            AIL.MgraText(AilGraphicsContext, AilGraphicsList, 10, 10, TextLabel);

            // Restore the input units to pixel units for result annotations.
            AIL.MgraControl(AilGraphicsContext, AIL.M_INPUT_UNITS, AIL.M_PIXEL);

            // Draw blob center of gravity annotation.
            AIL.MgraControl(AilGraphicsContext, AIL.M_LINE_THICKNESS, 3.0);
            AIL.MgraControl(AilGraphicsContext, AIL.M_COLOR, AIL.M_COLOR_RED);
            AIL.MblobDraw(AilGraphicsContext, AilBlobResult, AilGraphicsList, AIL.M_DRAW_CENTER_OF_GRAVITY, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT);

            // Draw blob bounding box annotations.
            AIL.MgraControl(AilGraphicsContext, AIL.M_COLOR, AIL.M_COLOR_GREEN);
            AIL.MgraControl(AilGraphicsContext, AIL.M_LINE_THICKNESS, 1.0);
            AIL.MblobDraw(AilGraphicsContext, AilBlobResult, AilGraphicsList, AIL.M_DRAW_BOX, AIL.M_INCLUDED_BLOBS, AIL.M_DEFAULT);

            // Enable the display to update the drawings.
            AIL.MdispControl(AilDisplay, AIL.M_UPDATE, AIL.M_ENABLE);
        }
    }
}

```
