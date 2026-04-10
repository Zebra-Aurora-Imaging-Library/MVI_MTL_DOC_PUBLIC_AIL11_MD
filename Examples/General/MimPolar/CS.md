---
title: "MimPolar"
description: "This program uses the polar-to-rectangular transformation to unroll a string."
ms.language: "C#"
---

# MimPolar

> This program uses the polar-to-rectangular transformation to unroll a string.

**Language:** C#

**Functions used:** `MappAlloc`, `MappFree`, `MbufChild2d`, `MbufFree`, `MbufInquire`, `MbufRestore`, `MdispAlloc`, `MdispFree`, `MdispSelect`, `MimPolarTransform`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Machining, Applications, Reading, Preprocessing, Modules, Buffer, Display, Image Processing, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MImPolar.cs 
//
// Description: This program uses the polar-to-rectangular transformation
//              to unroll a string.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MImPolar
{
    class Program
    {
        // Target image specifications.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "Polar.mim";

        // Points used to compute the parameters of the circle.
        private const int POINT1_X = 147;
        private const int POINT1_Y = 359;
        private const int POINT2_X = 246;
        private const int POINT2_Y = 404;
        private const int POINT3_X = 354;
        private const int POINT3_Y = 368;

        // Polar stripe features.
        private const int DELTA_RADIUS = 25;
        private const int START_ANGLE = 210;
        private const int END_ANGLE = 330;

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier.
            AIL_ID AilPolarImage = AIL.M_NULL;      // Destination buffer identifier.
            double SizeRadius = 0.0;
            double SizeAngle = 0.0;
            double CenterX = 0.0;
            double CenterY = 0.0;
            double Radius = 0.0;
            int OffsetX = 0;
            int OffsetY = 0;
            int SizeX = 0;
            int SizeY = 0;

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Load the source image and display it.
            AIL.MbufRestore(IMAGE_FILE, AilSystem, ref AilImage);
            AIL.MdispSelect(AilDisplay, AilImage);

            // Calculate the parameters of the circle.
            GenerateCircle(POINT1_X, POINT1_Y, POINT2_X, POINT2_Y, POINT3_X, POINT3_Y, ref CenterX, ref CenterY, ref Radius);

            // Get the size of the destination buffer.
            AIL.MimPolarTransform(AilImage, AIL.M_NULL, CenterX, CenterY, Radius + DELTA_RADIUS, Radius - DELTA_RADIUS, START_ANGLE, END_ANGLE, AIL.M_RECTANGULAR_TO_POLAR, AIL.M_NEAREST_NEIGHBOR + AIL.M_OVERSCAN_ENABLE, ref SizeAngle, ref SizeRadius);

            // Allocate the destination buffer.
            OffsetX = (int)((AIL.MbufInquire(AilImage, AIL.M_SIZE_X, AIL.M_NULL) / 2) - (SizeAngle / 2));
            OffsetY = 20;
            SizeX = (int)Math.Ceiling(SizeAngle);
            SizeY = (int)Math.Ceiling(SizeRadius);
            AIL.MbufChild2d(AilImage, OffsetX, OffsetY, SizeX, SizeY, ref AilPolarImage);

            // Print a message.
            Console.Write("\nPOLAR TRANSFORMATION:\n");
            Console.Write("---------------------\n\n");
            Console.Write("A string will be unrolled using a polar-to-rectangular transformation.\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Unroll the string.
            AIL.MimPolarTransform(AilImage, AilPolarImage, CenterX, CenterY, Radius + DELTA_RADIUS, Radius - DELTA_RADIUS, START_ANGLE, END_ANGLE, AIL.M_RECTANGULAR_TO_POLAR, AIL.M_NEAREST_NEIGHBOR + AIL.M_OVERSCAN_ENABLE, ref SizeAngle, ref SizeRadius);

            // Print a message on the Host screen.
            Console.Write("Press any key to end.\n");
            Console.ReadKey(true);

            // Free buffers.
            AIL.MbufFree(AilPolarImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AilImage);
        }

        // GenerateCircle() function returns the center and the radius of a circle 
        // defined by 3 non-collinear points.
        static void GenerateCircle(double X1, double Y1, double X2, double Y2, double X3, double Y3, ref double CenterX, ref double CenterY, ref double Radius)
        {
            double Slope1 = 0.0;
            double Slope2 = 0.0;
            double MidPoint1X = 0.0;
            double MidPoint1Y = 0.0;
            double MidPoint2X = 0.0;
            double MidPoint2Y = 0.0;
            double Offset1 = 0.0;
            double Offset2 = 0.0;

            // Compute the middle points of the two segments.
            MidPoint1X = (X1 + X2) / 2;
            MidPoint1Y = (Y1 + Y2) / 2;
            MidPoint2X = (X2 + X3) / 2;
            MidPoint2Y = (Y2 + Y3) / 2;

            // Compute the slope between points 1 and 2, and between 
            // points 2 and 3.
            if (((Y2 - Y1) != 0.0) && ((Y3 - Y2) != 0.0))
            {
                Slope1 = -(X2 - X1) / (Y2 - Y1);
                Slope2 = -(X3 - X2) / (Y3 - Y2);

                Offset1 = MidPoint1Y - Slope1 * MidPoint1X;
                Offset2 = MidPoint2Y - Slope2 * MidPoint2X;

                CenterX = (Offset2 - Offset1) / (Slope1 - Slope2);
                CenterY = Slope1 * (CenterX) + Offset1;
            }
            else if (((Y2 - Y1) == 0.0) && ((Y3 - Y2) != 0.0))
            {
                Slope2 = -(X3 - X2) / (Y3 - Y2);
                Offset2 = MidPoint2Y - Slope2 * MidPoint2X;

                CenterX = MidPoint1X;
                CenterY = Slope2 * (CenterX) + Offset2;
            }
            else if (((Y2 - Y1) != 0.0) && ((Y3 - Y2) == 0.0))
            {
                Slope1 = -(X2 - X1) / (Y2 - Y1);
                Offset1 = MidPoint1Y - Slope1 * MidPoint1X;

                CenterX = MidPoint2X;
                CenterY = Slope1 * (CenterX) + Offset1;
            }

            // Compute the radius.
            Radius = Math.Sqrt(Math.Pow(CenterX - X1, 2) + Math.Pow(CenterY - Y1, 2));
        }
    }
}

```
