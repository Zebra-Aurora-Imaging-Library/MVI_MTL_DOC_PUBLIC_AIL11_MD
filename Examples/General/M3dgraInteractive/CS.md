---
title: "M3dgraInteractive"
description: "This program contains an example on how to interactively edit a 3D box geometry and control its individual handlers. It also shows a brief ussage of LOD (level of detail) settings."
ms.language: "C#"
---

# M3dgraInteractive

> This program contains an example on how to interactively edit a 3D box geometry and control its individual handlers. It also shows a brief ussage of LOD (level of detail) settings.

**Language:** C#

**Functions used:** `M3ddispAlloc`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3ddispHookFunction`, `M3ddispGetHookInfo`, `M3dgeoAlloc`, `M3dgeoBox`, `M3dgeoDraw3d`, `M3dgeoFree`, `M3dgeoInquire`, `M3dgraAdd`, `M3dgraControl`, `M3dgraCopy`, `M3dgraHookFunction`, `M3dimCrop`, `M3dimStat`, `MappAlloc`, `MappControl`, `MappFileOperation`, `MappFree`, `MappHookFunction`, `MappInquire`, `MbufAllocContainer`, `MbufFree`, `MbufImport`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, 3D Display, 3D Geometry, 3D Graphics, 3D Image Processing, Buffer, What's New, AIL 11.0, AIL 10.0 SP7, AIL 10.0 SP5

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dgraInteractive.cs
//
// Description: This program contains an example on how to interactively edit a
//              3D box geometry and control its individual handlers. It also
//              shows a brief usage of LOD (level of detail) settings.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Threading;
using System.Collections.Generic;
using System.Text;
using System.Runtime.InteropServices;

using Zebra.AuroraImagingLibrary;
using System.Data;

namespace M3dgraInteractive
    {
    class Program
        {
        // Constants.
        private static readonly AIL_INT DEFAULT_EDITABLE_OPACITY = 30;
        private static readonly string  PT_CLD_FILE              =
            AIL.M_IMAGE_PATH + "M3dgra/MaskOrganized.mbufc";

        private static readonly string  SAVE_PATH                = "";
        private static readonly string  OUTPUT_PC_NAME           =
            SAVE_PATH + "CroppedPointCloud.ply";

        //----------------------------------------------------------------------------
        public class SPickStruct
            {
            public AIL_INT BoxLabel;
            public AIL_ID  Box;
            public AIL_ID  Gralist;
            public AIL_ID  OriginalContainer;
            public AIL_ID  CroppedContainer;
            };

        public class SKeyStruct
            {
            public AIL_ID  Disp;
            };

        enum EParentControl
            {
            Translatable,
            Rotatable,
            Scalable
            };

        //----------------------------------------------------------------------------*
        // Main.
        //----------------------------------------------------------------------------*
        static void Main(string[] args)
            {
            Console.Write("[EXAMPLE NAME]\n");
            Console.Write("M3dgraInteractive\n\n");

            Console.Write("[SYNOPSIS]\n");
            Console.Write("This example demonstrates how to interactively edit a 3D box\n");
            Console.Write("geometry as well as setting up and using the LOD settings\n");
            Console.Write("(level of detail) and changing the editable controls to enable\n");
            Console.Write("and disable individual editable handles.\n\n");

            Console.Write("[MODULES USED]\n");
            Console.Write("Modules used: application, system, buffer, 3D display, ");
            Console.Write("3D graphics, 3D image processing.\n\n");

            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem      = AIL.M_NULL;     // System identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(
                AIL.M_DEFAULT,
                ref AilApplication,
                ref AilSystem,
                AIL.M_NULL,
                AIL.M_NULL,
                AIL.M_NULL);

            // Check for required example files.
            if (!CheckForRequiredAILFile(PT_CLD_FILE))
                {
                AIL.MappFreeDefault(
                    AilApplication,
                    AilSystem,
                    AIL.M_NULL,
                    AIL.M_NULL,
                    AIL.M_NULL);
                return;
                }

            // Allocate the display.
            AIL_ID Ail3dDisplay = Alloc3dDisplayId(AilSystem);
            if (Ail3dDisplay == AIL.M_NULL)
                {
                AIL.MappFreeDefault(
                    AilApplication,
                    AilSystem,
                    AIL.M_NULL,
                    AIL.M_NULL,
                    AIL.M_NULL);
                return;
                }

            AIL_ID Ail3dGraList = AIL.M3ddispInquire(
                Ail3dDisplay,
                AIL.M_3D_GRAPHIC_LIST_ID,
                AIL.M_NULL);

            // Allocate and restore a point cloud.
            string PointCloudFile = "";
            AIL_ID PointCloud = ObtainPointCloud(PointCloudFile);
            AIL_ID OriginalContainer = AIL.MbufAllocContainer(AilSystem,
                                                              AIL.M_PROC + AIL.M_DISP,
                                                              AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MbufConvert3d(PointCloud, OriginalContainer, AIL.M_NULL,
                              AIL.M_REMOVE_NON_FINITE, AIL.M_COMPENSATE);
            if (OriginalContainer == AIL.M_NULL)
                {
                return;
                }

            // Create a cropped copy of the point cloud and add it to the graphics list.
            AIL_ID CroppedContainer = AIL.MbufAllocContainer(
                AilSystem,
                AIL.M_PROC + AIL.M_DISP,
                AIL.M_DEFAULT,
                AIL.M_NULL);

            AIL_ID PtCldLabel  = AIL.M3dgraAdd(
                Ail3dGraList,
                AIL.M_ROOT_NODE,
                CroppedContainer,
                AIL.M_DEFAULT);

            // Create an editable box in the graphics list.
            AIL_ID BoundingBox = AIL.M3dgeoAlloc(
                AilSystem,
                AIL.M_GEOMETRY,
                AIL.M_DEFAULT,
                AIL.M_NULL);

            // Initialize the size of the box to a fraction of the original point cloud's size.
            AIL.M3dimStat(
                AIL.M_STAT_CONTEXT_BOUNDING_BOX,
                OriginalContainer,
                BoundingBox,
                AIL.M_DEFAULT);

            AIL.M3dgeoBox(BoundingBox, AIL.M_CENTER_AND_DIMENSION,
                          AIL.M_UNCHANGED, AIL.M_UNCHANGED, AIL.M_UNCHANGED,
                          AIL.M3dgeoInquire(BoundingBox, AIL.M_SIZE_X, AIL.M_NULL),
                          AIL.M3dgeoInquire(BoundingBox, AIL.M_SIZE_Y, AIL.M_NULL),
                          AIL.M_UNCHANGED, AIL.M_DEFAULT);

            AIL_ID BoxLabel = AIL.M3dgeoDraw3d(
                AIL.M_DEFAULT,
                BoundingBox,
                Ail3dGraList,
                AIL.M_ROOT_NODE,
                AIL.M_DEFAULT);

            AIL.M3dgraControl(Ail3dGraList, BoxLabel, AIL.M_EDITABLE, AIL.M_ENABLE);

            // This sets the boxes properties to be the same as editable.
            AIL.M3dgraControl(Ail3dGraList, BoxLabel, AIL.M_OPACITY, DEFAULT_EDITABLE_OPACITY);

            // This enables the LOD on the pointclod.
            AIL.M3dgraControl(Ail3dGraList, PtCldLabel, AIL.M_VIEW_BASED_LOD, AIL.M_ENABLE);

            AIL_ID CroppingBox = AIL.M3dgeoAlloc(AilSystem, AIL.M_GEOMETRY,
                                                 AIL.M_DEFAULT, AIL.M_NULL);

            // Create a hook to crop the container when the box is modified
            // in the graphics list.
            SPickStruct PickStruct       = new SPickStruct();
            PickStruct.Box = CroppingBox;
            PickStruct.BoxLabel = BoxLabel;
            PickStruct.Gralist = Ail3dGraList;
            PickStruct.OriginalContainer = OriginalContainer;
            PickStruct.CroppedContainer = CroppedContainer;

            GCHandle hUserData = GCHandle.Alloc(PickStruct);
            AIL_3DGRA_HOOK_FUNCTION_PTR HandlerFunctionPtr =
                new AIL_3DGRA_HOOK_FUNCTION_PTR(BoxModifiedHandler);

            AIL.M3dgraHookFunction(Ail3dGraList, AIL.M_EDITABLE_GRAPHIC_MODIFIED,
                                   HandlerFunctionPtr, GCHandle.ToIntPtr(hUserData));

            // Crop a first time before starting the interactivity.
            RetrieveBoxAndCrop(ref PickStruct);

            // Open the 3d display.
            AIL.M3ddispSelect(Ail3dDisplay, AIL.M_NULL, AIL.M_OPEN, AIL.M_DEFAULT);

            Console.Write("A 3D point cloud is restored from a ply file and displayed.\n");
            Console.Write("The box is editable.\n");
            Console.Write("Only the points inside the interactive box are shown.\n\n");
            Console.Write("- Use side box handles to resize.\n");
            Console.Write("- Use axis arrow tips to translate.\n");
            Console.Write("- Use axis arcs to rotate.\n\n");
            Console.Write("Press any key to continue.\n\n");
            Console.ReadKey(true);

            // Enable LOD degradation on action.
            AIL.M3ddispControl(Ail3dDisplay, AIL.M_LOD_DEGRADE_MODE, AIL.M_ENABLE_ON_ACTION);

            SKeyStruct KeyStruct = new SKeyStruct();
            KeyStruct.Disp = Ail3dDisplay;

            GCHandle hDispPointer = GCHandle.Alloc(KeyStruct);
            AIL_3DDISP_HOOK_FUNCTION_PTR KeyHandlerFunctionPtr =
                new AIL_3DDISP_HOOK_FUNCTION_PTR(DispKeyHandler);

            AIL.M3ddispHookFunction(Ail3dDisplay, AIL.M_KEY_DOWN, KeyHandlerFunctionPtr,
                                    GCHandle.ToIntPtr(hDispPointer));
            AIL.M3ddispControl(Ail3dDisplay, AIL.M_AUTO_ROTATE, AIL.M_ENABLE);

            Console.Write("You can control the LOD (level of detail) of a point cloud\n");
            Console.Write("and have this always be shown or when interacting with the display.\n");
            Console.Write("This is used to speedup rendering during its use at the cost\n");
            Console.Write("of hidding points and needing to calculate these LODs.\n");
            Console.Write("The degradation setting can be toggled using the \"L\" key in\n");
            Console.Write("the display window. The usage of LODs must be enabled on the point\n");
            Console.Write("cloud to see a change in the display when changing this setting.\n\n");
            Console.Write("Press any key to continue.\n\n");
            Console.Write("Level of Detail (lod) status : degrade on action \r");

            for (int i = 0; (i <= 100) && !Console.KeyAvailable; i++)
                {
                Thread.Sleep(50);
                }
            AIL.M3ddispControl(Ail3dDisplay, AIL.M_AUTO_ROTATE, AIL.M_DISABLE);
            Console.ReadKey(true);

            AIL.M3ddispHookFunction(Ail3dDisplay, AIL.M_KEY_DOWN + AIL.M_UNHOOK,
                                    KeyHandlerFunctionPtr, GCHandle.ToIntPtr(hDispPointer));
            Console.Write("\n\n");

            ChangeEditability(Ail3dGraList, BoxLabel, EParentControl.Translatable);
            ChangeEditability(Ail3dGraList, BoxLabel, EParentControl.Rotatable);
            ChangeEditability(Ail3dGraList, BoxLabel, EParentControl.Scalable);

            if (AskYesNo("Do you want to save the point cloud?"))
                {
                AIL.MbufSave(OUTPUT_PC_NAME, CroppedContainer);
                }

            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            AIL.M3dgeoFree(CroppingBox);
            AIL.M3dgeoFree(BoundingBox);
            AIL.MbufFree(PointCloud);
            AIL.MbufFree(CroppedContainer);
            AIL.MbufFree(OriginalContainer);
            AIL.M3ddispFree(Ail3dDisplay);
            AIL.MappFreeDefault(AilApplication, AilSystem, AIL.M_NULL, AIL.M_NULL, AIL.M_NULL);
            }

        //----------------------------------------------------------------------------
        // Handler call for modifying the editable box.
        //----------------------------------------------------------------------------
        static public AIL_INT BoxModifiedHandler(AIL_INT HookType, AIL_ID EventId,
                                                 IntPtr UserDataPtr)
            {
            GCHandle hUserData = GCHandle.FromIntPtr(UserDataPtr);
            SPickStruct PickStruct = hUserData.Target as SPickStruct;

            RetrieveBoxAndCrop(ref PickStruct);

            return 0;
            }

        //-----------------------------------------------------------------------------
        // Handler call when key hit in disp.
        //-----------------------------------------------------------------------------
        static public AIL_INT DispKeyHandler(AIL_INT HookType, AIL_ID EventId,
                                             IntPtr UserDataPtr)
            {
            // Not LOD key (M_KEY_L) so we ignore it.
            AIL_INT HitKey = AIL.M_NULL;
            AIL.M3ddispGetHookInfo(EventId, AIL.M_AIL_KEY_VALUE, ref HitKey);
            if (HitKey != AIL.M_KEY_L)
                {
                return 0;
                }

            GCHandle UserDataHandle = GCHandle.FromIntPtr(UserDataPtr);
            SKeyStruct KeyStruct = UserDataHandle.Target as SKeyStruct;

            AIL_INT LodDegrade = AIL.M3ddispInquire(KeyStruct.Disp,
                                                    AIL.M_LOD_DEGRADE_MODE, AIL.M_NULL);

            // The hook is called before the setting is changed,
            // therefore we write what it will be.
            Console.Write("Level of Detail (lod) status : {0}           \r",
                          LodDegrade == AIL.M_ENABLE_ALWAYS ? "disabled" :
                          LodDegrade == AIL.M_ENABLE_ON_ACTION ? "always degrade" : "degrade on action");

            return 0;
            }

        //----------------------------------------------------------------------------
        // Crops container based on associated box.
        //----------------------------------------------------------------------------
        static public void RetrieveBoxAndCrop(ref SPickStruct PickStruct)
            {
            // Retrieve the edited box from the graphics list.
            AIL.M3dgraCopy(PickStruct.Gralist, PickStruct.BoxLabel, PickStruct.Box,
                           AIL.M_DEFAULT, AIL.M_GEOMETRY, AIL.M_DEFAULT);

            // Crop the point cloud using the retrieved box.
            AIL.M3dimCrop(PickStruct.OriginalContainer, PickStruct.CroppedContainer,
                          PickStruct.Box, AIL.M_NULL, AIL.M_SAME, AIL.M_DEFAULT);
            }

        //----------------------------------------------------------------------------
        // Check for required files to run the example.
        //----------------------------------------------------------------------------
        static bool CheckForRequiredAILFile(string FileName)
            {
            AIL_INT FilePresent = AIL.M_NO;

            AIL.MappFileOperation(AIL.M_DEFAULT, FileName, AIL.M_NULL, AIL.M_NULL,
                                  AIL.M_FILE_EXISTS, AIL.M_DEFAULT, ref FilePresent);

            if (FilePresent == AIL.M_NO)
                {
                Console.Write("The footage needed to run this example is missing. You need\n");
                Console.Write("to obtain and apply a separate specific update " +
                              "to have it.\n\n");

                Console.Write("Press any key to end.\n\n");
                Console.ReadKey(true);
                }

            return (FilePresent == AIL.M_YES);
            }

        //----------------------------------------------------------------------------
        // Allocates a 3D display and returns its identifier.
        //----------------------------------------------------------------------------
        static AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
            {
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
            AIL_ID AilDisplay3D = AIL.M3ddispAlloc(AilSystem, AIL.M_DEFAULT, "M_DEFAULT",
                                                   AIL.M_DEFAULT, AIL.M_NULL);
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            if (AilDisplay3D == AIL.M_NULL)
                {
                Console.Write("\n");
                Console.Write("The current system does not support the 3D display.\n");
                Console.Write("Press any key to exit.\n");
                Console.ReadKey(true);
                }

            return AilDisplay3D;
            }

        //----------------------------------------------------------------------------
        // Modify editable settings.
        //----------------------------------------------------------------------------
        static void ChangeEditability(AIL_ID Ail3dGraList, AIL_ID BoxLabel,
                                      EParentControl ParentControl)
            {
            List<AIL_INT> Controls;
            List<string>  Choices;
            List<bool>    Values = new List<bool>();

            switch (ParentControl)
                {
                case EParentControl.Translatable:
                    Controls = new List<AIL_INT>
                        { AIL.M_EDITABLE, AIL.M_TRANSLATABLE, AIL.M_TRANSLATABLE_X,
                          AIL.M_TRANSLATABLE_Y, AIL.M_TRANSLATABLE_Z };

                    Choices = new List<string>
                        { "M_EDITABLE", "M_TRANSLATABLE", "M_TRANSLATABLE_X",
                          "M_TRANSLATABLE_Y", "M_TRANSLATABLE_Z", "continue"};
                    break;
                case EParentControl.Rotatable:
                    Controls = new List<AIL_INT>
                        { AIL.M_EDITABLE, AIL.M_ROTATABLE, AIL.M_ROTATABLE_X,
                          AIL.M_ROTATABLE_Y, AIL.M_ROTATABLE_Z };

                    Choices = new List<string>
                        { "M_EDITABLE", "M_ROTATABLE", "M_ROTATABLE_X",
                        "M_ROTATABLE_Y", "M_ROTATABLE_Z", "continue" };
                    break;
                case EParentControl.Scalable:
                    Controls = new List<AIL_INT>
                        { AIL.M_EDITABLE, AIL.M_SCALABLE, AIL.M_SCALABLE_X, AIL.M_SCALABLE_Y,
                          AIL.M_SCALABLE_Z, AIL.M_SCALABLE_X0, AIL.M_SCALABLE_X1 };

                    Choices = new List<string>
                        { "M_EDITABLE", "M_SCALABLE", "M_SCALABLE_X", "M_SCALABLE_Y",
                          "M_SCALABLE_Z", "M_SCALABLE_X0", "M_SCALABLE_X1", "continue" };
                    break;
                default:
                    Controls = new List<AIL_INT>();
                    Choices = new List<string>();
                    break;
                }

            for (int c = 0; c < Controls.Count; c++)
                {
                Values.Add(AIL.M3dgraInquire(Ail3dGraList, BoxLabel, Controls[c], AIL.M_NULL)
                           == AIL.M_ENABLE);
                }

            // Get user choice
            Console.Write("Select a control to swap (between M_ENABLE and " +
                          "M_DISABLE)\n");
            Console.Write("by pressing its respective key in the console.\n");
            Console.Write("{0} takes precedence when disabled followed by {1}",
                          Choices[0], Choices[1]);

            if (ParentControl == EParentControl.Scalable)
                {
                Console.Write(", {0} and {1}.\n", Choices[2], Choices[5]);
                }
            else
                {
                Console.Write(" and {0}.\n", Choices[2], Choices[5]);
                }

            int Choice  = AskMakeChoice(Choices, Values);
            AIL_INT Control = AIL.M_NULL;
            if (Choice >= 0)
                {
                Control = Controls[Choice];
                }

            while (Control != AIL.M_NULL)
                {
                // Swap control value.
                AIL_INT OldValue = AIL.M3dgraInquire(Ail3dGraList, BoxLabel,
                                                     Control, AIL.M_NULL);
                AIL.M3dgraControl(Ail3dGraList, BoxLabel, Control,
                                  OldValue == AIL.M_ENABLE ? AIL.M_DISABLE : AIL.M_ENABLE);
                Values[Choice] = !Values[Choice];

                // Get user choice.
                Choice = AskMakeChoice(Choices, Values, false);
                Control = AIL.M_NULL;
                if (Choice >= 0)
                    {
                    Control = Controls[Choice];
                    }
                }
            Console.Write("\n\n");
            }

        //----------------------------------------------------------------------------
        // Helper function for getting user choices.
        //----------------------------------------------------------------------------
        static int AskMakeChoice(List<string> Choices, List<bool> Values, bool Print = true)
            {
            int Choice;

            // Print the choices.
            if (Print)
                {
                for (int c = 0; c < Choices.Count - 1; c++)
                    {
                    Console.Write("\t{0}. {1}\n", c, Choices[c]);
                    }
                Console.Write("\tPress \"Enter\" to {0}. \n\n", Choices[Choices.Count - 1]);
                }

            // Print the choice values.
            for (int c = 0; c < Values.Count; c++)
                {
                if (c != 0)
                    {
                    Console.Write(", ");
                    }
                Console.Write("{0}: {1}", c, Values[c] ? "Enabled" : "Disabled");
                }
            Console.Write("     \r");

            // Get the choice.
            do
                {
                Choice = Console.ReadKey(true).KeyChar;
                if (Choice == '\r')
                    {
                    return -1;
                    }
                Choice -= ('0' + 0);
                } while (Choice < 0 || Choice >= (AIL_INT)Choices.Count - 1);
            return Choice;
            }


        //----------------------------------------------------------------------------
        // Prompts user for yes/no.
        //----------------------------------------------------------------------------
        static bool AskYesNo(string QuestionString)
            {
            Console.Write("{0} (y/n)?\n", QuestionString);
            while (true)
                {
                switch (Console.ReadKey(true).KeyChar)
                    {
                    case 'y':
                    case 'Y':
                        return true;

                    case 'n':
                    case 'N':
                    case '\r':
                        return false;
                    }
                }
            }

        //----------------------------------------------------------------------------
        // Obtains a point cloud either from file or by calculating with example data.
        //----------------------------------------------------------------------------
        static AIL_ID ObtainPointCloud(string PointCloudFile)
            {
            AIL_ID AilPointCloud;
            do
                {
                if (PointCloudFile != "")
                    {
                    AilPointCloud = RestorePointCloud(PointCloudFile);
                    }
                else if (AskYesNo("Do you want to load a user point cloud"))
                    {
                    Console.Write("Please select a .mbufc or .ply point cloud file.\n\n");
                    AilPointCloud = RestorePointCloud("");
                    }
                else
                    {
                    Console.Write("The example will run using a point cloud from " +
                                  "example source data.\n\n");
                    if (!CheckForRequiredAILFile(PT_CLD_FILE))
                        {
                        return AIL.M_NULL;
                        }
                    AilPointCloud = RestorePointCloud(PT_CLD_FILE);
                    }
                } while (AilPointCloud == AIL.M_NULL);

            return AilPointCloud;
            }

        //----------------------------------------------------------------------------
        // Restores the registration result from file.
        //----------------------------------------------------------------------------
        static AIL_ID RestorePointCloud(string PointCloudFilename)
            {
            // Restore the 3dreg result.
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_DISABLE);
            AIL_ID AilPointCloud = AIL.M_NULL;
            if (PointCloudFilename == String.Empty)
                {
                AilPointCloud = AIL.MbufImport(AIL.M_NULL, AIL.M_DEFAULT, AIL.M_RESTORE,
                                               AIL.M_DEFAULT_HOST, AIL.M_NULL);
                }
            else
                {
                AilPointCloud = AIL.MbufImport(PointCloudFilename, AIL.M_DEFAULT,
                                               AIL.M_RESTORE, AIL.M_DEFAULT_HOST, AIL.M_NULL);
                }
            AIL.MappControl(AIL.M_DEFAULT, AIL.M_ERROR, AIL.M_PRINT_ENABLE);

            // The restored file must be a convertible container.
            if (AilPointCloud != AIL.M_NULL &&
               (AIL.MobjInquire(AilPointCloud, AIL.M_OBJECT_TYPE, AIL.M_NULL) !=
                AIL.M_CONTAINER ||
                AIL.MbufInquireContainer(AilPointCloud, AIL.M_CONTAINER,
                                         AIL.M_3D_CONVERTIBLE, AIL.M_NULL) ==
                AIL.M_NOT_CONVERTIBLE))
                {
                AIL.MbufFree(AilPointCloud);
                AilPointCloud = AIL.M_NULL;
                }

            // Verify that the result is valid.
            if (AilPointCloud == AIL.M_NULL)
                {
                Console.Write("No valid .mbufc file restored.\n\n");
                }

            return AilPointCloud;
            }
        }
    }

```
