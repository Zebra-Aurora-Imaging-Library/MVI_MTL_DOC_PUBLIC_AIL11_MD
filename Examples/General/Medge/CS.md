---
title: "Medge"
description: "This program uses the AIL Edge Finder module to find and measure the outer diameter of good seals in a target image."
ms.language: "C#"
---

# Medge

> This program uses the AIL Edge Finder module to find and measure the outer diameter of good seals in a target image.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufFree`, `MbufRestore`, `MdispAlloc`, `MdispControl`, `MdispFree`, `MdispSelect`, `MedgeAlloc`, `MedgeAllocResult`, `MedgeCalculate`, `MedgeControl`, `MedgeDraw`, `MedgeFree`, `MedgeGetResult`, `MedgeSelect`, `MgraAllocList`, `MgraClear`, `MgraFree`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Counting, Finding/Locating, Measuring, Modules, Buffer, Display, Graphics, Edge Finder, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    Medge.cs
//
// Description: This program uses the Edge Finder module to find and measure
//              the outer diameter of good seals in a target image. 
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MEdge
{
    class Program
    {
        // Source image file specifications.
        private const string CONTOUR_IMAGE = AIL.M_IMAGE_PATH + "Seals.mim";
        private const int CONTOUR_MAX_RESULTS = 100;
        private const double CONTOUR_MAXIMUM_ELONGATION = 0.8;
        private static readonly int CONTOUR_DRAW_COLOR = AIL.M_COLOR_GREEN;
        private static readonly int CONTOUR_LABEL_COLOR = AIL.M_COLOR_RED;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;                             // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;                                  // System Identifier.
            AIL_ID AilDisplay = AIL.M_NULL;                                 // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;                                   // Image buffer identifier.
            AIL_ID GraphicList = AIL.M_NULL;                                // Graphic list identifier.
            AIL_ID AilEdgeContext = AIL.M_NULL;                             // Edge context.
            AIL_ID AilEdgeResult = AIL.M_NULL;                              // Edge result identifier.
            double EdgeDrawColor = CONTOUR_DRAW_COLOR;                      // Edge draw color.
            double LabelDrawColor = CONTOUR_LABEL_COLOR;                    // Text draw color.
            AIL_INT NumEdgeFound = 0;                                       // Number of edges found.
            AIL_INT NumResults = 0;                                         // Number of results found.
            int i = 0;                                                      // Loop variable.
            double[] MeanFeretDiameter = new double[CONTOUR_MAX_RESULTS];   // Edge mean Feret diameter.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Restore the image and display it.
            AIL.MbufRestore(CONTOUR_IMAGE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Allocate a graphic list to hold the subpixel annotations to draw.
            AIL.MgraAllocList(AilSystem, AIL.M_DEFAULT, ref GraphicList);

            // Associate the graphic list to the display for annotations.
            AIL.MdispControl(AilDisplay, AIL.M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicList);

            // Pause to show the original image.
            Console.Write("\nEDGE MODULE:\n");
            Console.Write("------------\n\n");
            Console.Write("This program determines the outer seal diameters in the displayed image \n");
            Console.Write("by detecting and analyzing contours with the Edge Finder module.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Allocate a Edge Finder context.
            AIL.MedgeAlloc(AilSystem, AIL.M_CONTOUR, AIL.M_DEFAULT, ref AilEdgeContext);

            // Allocate a result buffer.
            AIL.MedgeAllocResult(AilSystem, AIL.M_DEFAULT, ref AilEdgeResult);

            // Enable features to compute.
            AIL.MedgeControl(AilEdgeContext, AIL.M_MOMENT_ELONGATION, AIL.M_ENABLE);
            AIL.MedgeControl(AilEdgeContext, AIL.M_FERET_MEAN_DIAMETER + AIL.M_SORT1_DOWN, AIL.M_ENABLE);

            // Calculate edges and features.
            AIL.MedgeCalculate(AilEdgeContext, AilImage, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL, AilEdgeResult, AIL.M_DEFAULT);

            // Get the number of edges found.
            AIL.MedgeGetResult(AilEdgeResult, AIL.M_DEFAULT, AIL.M_NUMBER_OF_CHAINS + AIL.M_TYPE_AIL_INT, ref NumEdgeFound);

            // Draw edges in the source image to show the result.
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, EdgeDrawColor);
            AIL.MedgeDraw(AIL.M_DEFAULT, AilEdgeResult, GraphicList, AIL.M_DRAW_EDGES, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Pause to show the edges.
            Console.Write("{0} edges were found in the image.\n", NumEdgeFound);
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Exclude elongated edges.
            AIL.MedgeSelect(AilEdgeResult, AIL.M_EXCLUDE, AIL.M_MOMENT_ELONGATION, AIL.M_LESS, CONTOUR_MAXIMUM_ELONGATION, AIL.M_NULL);

            // Exclude inner chains.
            AIL.MedgeSelect(AilEdgeResult, AIL.M_EXCLUDE, AIL.M_INCLUDED_EDGES, AIL.M_INSIDE_BOX, AIL.M_NULL, AIL.M_NULL);

            // Draw remaining edges and their index to show the result.
            AIL.MgraClear(AIL.M_DEFAULT, GraphicList);
            AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, EdgeDrawColor);
            AIL.MedgeDraw(AIL.M_DEFAULT, AilEdgeResult, GraphicList, AIL.M_DRAW_EDGES, AIL.M_DEFAULT, AIL.M_DEFAULT);

            // Pause to show the results.
            Console.Write("Elongated edges and inner edges of each seal were removed.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Get the number of edges found.
            AIL.MedgeGetResult(AilEdgeResult, AIL.M_DEFAULT, AIL.M_NUMBER_OF_CHAINS + AIL.M_TYPE_AIL_INT, ref NumResults);

            // If the right number of edges were found.
            if ((NumResults >= 1) && (NumResults <= CONTOUR_MAX_RESULTS))
            {
                // Draw the index of each edge.
                AIL.MgraControl(AIL.M_DEFAULT, AIL.M_COLOR, LabelDrawColor);
                AIL.MedgeDraw(AIL.M_DEFAULT, AilEdgeResult, GraphicList, AIL.M_DRAW_INDEX, AIL.M_DEFAULT, AIL.M_DEFAULT);

                // Get the mean Feret diameters.
                AIL.MedgeGetResult(AilEdgeResult, AIL.M_DEFAULT, AIL.M_FERET_MEAN_DIAMETER, MeanFeretDiameter);

                // Print the results.
                Console.Write("Mean diameter of the {0} outer edges are:\n\n", NumResults);
                Console.Write("Index   Mean diameter \n");
                for (i = 0; i < NumResults; i++)
                {
                    Console.Write("{0,-11}{1,-13:0.00}\n", i, MeanFeretDiameter[i]);
                }
            }
            else
            {
                Console.Write("Edges have not been found or the number of found edges is greater than\n");
                Console.Write("the specified maximum number of edges !\n\n");
            }

            // Wait for a key press.
            Console.Write("\nPress any key to end.\n");
            Console.ReadKey(true);

            // Free objects.
            AIL.MgraFree(GraphicList);
            AIL.MbufFree(AilImage);
            AIL.MedgeFree(AilEdgeContext);
            AIL.MedgeFree(AilEdgeResult);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
