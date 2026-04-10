---
title: "M3dgraInteractive"
description: "This program contains an example on how to interactively edit a 3D box geometry and control its individual handlers. It also shows a brief ussage of LOD (level of detail) settings."
ms.language: "C++"
---

# M3dgraInteractive

> This program contains an example on how to interactively edit a 3D box geometry and control its individual handlers. It also shows a brief ussage of LOD (level of detail) settings.

**Language:** C++

**Functions used:** `M3ddispAlloc`, `M3ddispFree`, `M3ddispInquire`, `M3ddispSelect`, `M3ddispHookFunction`, `M3ddispGetHookInfo`, `M3dgeoAlloc`, `M3dgeoBox`, `M3dgeoDraw3d`, `M3dgeoFree`, `M3dgeoInquire`, `M3dgraAdd`, `M3dgraControl`, `M3dgraCopy`, `M3dgraHookFunction`, `M3dimCrop`, `M3dimStat`, `MappAlloc`, `MappControl`, `MappFileOperation`, `MappFree`, `MappHookFunction`, `MappInquire`, `MbufAllocContainer`, `MbufFree`, `MbufImport`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, 3D Display, 3D Geometry, 3D Graphics, 3D Image Processing, Buffer, What's New, AIL 11.0, AIL 10.0 SP7, AIL 10.0 SP5

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    M3dgraInteractive.cpp
//
// Description: This program contains an example on how to interactively edit a
//              3D box geometry and control its individual handlers. It also
//              shows a brief usage of LOD (level of detail) settings.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

//-----------------------------------------------------------------------------
// Constants.
//-----------------------------------------------------------------------------
static const AIL_INT    DEFAULT_EDITABLE_OPACITY = 30;
static const AIL_STRING PT_CLD_FILE              =
   M_IMAGE_PATH AIL_TEXT("M3dgra/MaskOrganized.mbufc");

static const AIL_STRING SAVE_PATH                = AIL_TEXT("");
static const AIL_STRING OUTPUT_PC_NAME           =
   SAVE_PATH + AIL_TEXT("CroppedPointCloud.ply");

//----------------------------------------------------------------------------
// Structure declarations.
//----------------------------------------------------------------------------
struct SPickStruct
   {
   AIL_INT64 BoxLabel;
   AIL_ID    Box;
   AIL_ID    Gralist;
   AIL_ID    OriginalContainer;
   AIL_ID    CroppedContainer;
   };

enum eParentControl
   {
   Translatable,
   Rotatable,
   Scalable
   };

//----------------------------------------------------------------------------
// Function Declaration.
//----------------------------------------------------------------------------
bool           CheckForRequiredAILFile(const AIL_STRING& FileName);
AIL_ID         Alloc3dDisplayId(AIL_ID AilSystem);
AIL_INT MFTYPE BoxModifiedHandler(AIL_INT HookType, AIL_ID EventId, void* UserDataPtr);
AIL_INT MFTYPE DispKeyHandler(AIL_INT HookType, AIL_ID EventId, void* UserDataPtr);
void           RetrieveBoxAndCrop(SPickStruct* PickStruct);
void           ChangeEditability(AIL_ID Ail3dGraList, AIL_ID BoxLabel,
                                 eParentControl ParentControl);
AIL_INT        AskMakeChoice(const std::vector<AIL_CONST_TEXT_PTR>& Choices,
                             const std::vector<bool>& Values, bool Print = true);
bool           AskYesNo(AIL_CONST_TEXT_PTR QuestionString);
AIL_INT        ObtainPointCloud(AIL_STRING& PointCloudFile);
AIL_INT        RestorePointCloud(AIL_CONST_TEXT_PTR PointCloudFilename);

//-----------------------------------------------------------------------------
// Main.
//-----------------------------------------------------------------------------
int MosMain()
   {
   MosPrintf(
      AIL_TEXT("[EXAMPLE NAME]\n")
      AIL_TEXT("M3dgraInteractive\n\n")
      AIL_TEXT("[SYNOPSIS]\n")
      AIL_TEXT("This example demonstrates how to interactively edit a 3D box\n")\
      AIL_TEXT("geometry as well as setting up and using the LOD settings\n")
      AIL_TEXT("(level of detail) and changing the editable controls to enable\n")
      AIL_TEXT("and disable individual editable handles.\n\n")
      AIL_TEXT("[MODULES USED]\n")
      AIL_TEXT("Modules used: application, system, buffer, 3D display, ")
      AIL_TEXT("3D graphics, 3D image processing.\n\n"));

   AIL_ID AilApplication,  // Application identifier.
          AilSystem;       // System identifier.

   // Allocate defaults.
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, M_NULL, M_NULL, M_NULL);

   // Allocate the display.
   AIL_ID Ail3dDisplay = Alloc3dDisplayId(AilSystem);
   if(!Ail3dDisplay)
      {
      MappFreeDefault(AilApplication, AilSystem, M_NULL, M_NULL, M_NULL);
      return EXIT_FAILURE;
      }
   AIL_ID Ail3dGraList = (AIL_ID)M3ddispInquire(Ail3dDisplay, M_3D_GRAPHIC_LIST_ID, M_NULL);

   // Allocate and restore a point cloud.
   AIL_STRING PointCloudFile = AIL_TEXT("");
   AIL_ID PointCloud = ObtainPointCloud(PointCloudFile);
   if(!PointCloud)
      {
      M3ddispFree(Ail3dDisplay);
      MappFreeDefault(AilApplication, AilSystem, M_NULL, M_NULL, M_NULL);
      return EXIT_FAILURE;
      }
   AIL_ID OriginalContainer = MbufAllocContainer(AilSystem, M_PROC + M_DISP,
                                                 M_DEFAULT, M_NULL);
   MbufConvert3d(PointCloud, OriginalContainer, M_NULL, M_REMOVE_NON_FINITE, M_COMPENSATE);
   if(!OriginalContainer) return EXIT_FAILURE;

   // Create a cropped copy of the point cloud and add it to the graphics list.
   AIL_ID CroppedContainer = MbufAllocContainer(AilSystem, M_PROC + M_DISP, M_DEFAULT, M_NULL);
   AIL_ID PointCloudLabel  = M3dgraAdd(Ail3dGraList, M_ROOT_NODE, CroppedContainer, M_DEFAULT);

   // Create an editable box in the graphics list.
   // Initialize the size of the box to a fraction of the original point cloud's size.
   AIL_ID BoundingBox = M3dgeoAlloc(AilSystem, M_GEOMETRY, M_DEFAULT, M_NULL);
   M3dimStat(M_STAT_CONTEXT_BOUNDING_BOX, OriginalContainer, BoundingBox, M_DEFAULT);

   M3dgeoBox(BoundingBox, M_CENTER_AND_DIMENSION, M_UNCHANGED, M_UNCHANGED, M_UNCHANGED,
             M3dgeoInquire(BoundingBox, M_SIZE_X, M_NULL),
             M3dgeoInquire(BoundingBox, M_SIZE_Y, M_NULL),
             M_UNCHANGED, M_DEFAULT);

   AIL_INT64 BoxLabel = M3dgeoDraw3d(M_DEFAULT, BoundingBox, Ail3dGraList,
                                     M_ROOT_NODE, M_DEFAULT);

   M3dgraControl(Ail3dGraList, BoxLabel, M_EDITABLE, M_ENABLE);

   // This sets the boxes properties to be the same as editable.
   M3dgraControl(Ail3dGraList, BoxLabel, M_OPACITY, DEFAULT_EDITABLE_OPACITY);

   // This sets the LOD controls to make apparent the changes if active.
   M3dgraControl(Ail3dGraList, PointCloudLabel, M_VIEW_BASED_LOD, M_ENABLE);

   AIL_ID CroppingBox = M3dgeoAlloc(AilSystem, M_GEOMETRY, M_DEFAULT, M_NULL);

   // Create a hook to crop the container when the box is modified in the graphics list.
   SPickStruct PickStruct;
   PickStruct.Box               = CroppingBox;
   PickStruct.BoxLabel          = BoxLabel;
   PickStruct.Gralist           = Ail3dGraList;
   PickStruct.OriginalContainer = OriginalContainer;
   PickStruct.CroppedContainer  = CroppedContainer;

   M3dgraHookFunction(
      Ail3dGraList,
      M_EDITABLE_GRAPHIC_MODIFIED,
      BoxModifiedHandler,
      &PickStruct);

   // Crop a first time before starting the interactivity.
   RetrieveBoxAndCrop(&PickStruct);

   // Open the 3d display.
   M3ddispSelect(Ail3dDisplay, M_NULL, M_OPEN, M_DEFAULT);

   MosPrintf(AIL_TEXT("A 3D point cloud is restored from a ply file and displayed.\n"));
   MosPrintf(AIL_TEXT("The box is editable.\n"));
   MosPrintf(AIL_TEXT("Only the points inside the interactive box are shown.\n\n"));
   MosPrintf(AIL_TEXT("- Use side box handles to resize.\n"));
   MosPrintf(AIL_TEXT("- Use axis arrow tips to translate.\n"));
   MosPrintf(AIL_TEXT("- Use axis arcs to rotate.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosGetch();

   // Enable LOD degradation on action.
   M3ddispControl(Ail3dDisplay, M_LOD_DEGRADE_MODE, M_ENABLE_ON_ACTION);
   M3ddispHookFunction(
      Ail3dDisplay,
      M_KEY_DOWN,
      DispKeyHandler,
      (void*)Ail3dDisplay);
   M3ddispControl(Ail3dDisplay, M_AUTO_ROTATE, M_ENABLE);

   MosPrintf(AIL_TEXT("You can control the LOD (level of detail) of a point cloud\n"));
   MosPrintf(AIL_TEXT("and have this always be shown or when interacting with the display.\n"));
   MosPrintf(AIL_TEXT("This is used to speedup rendering during its use at the cost\n"));
   MosPrintf(AIL_TEXT("of hidding points and needing to calculate these LODs.\n"));
   MosPrintf(AIL_TEXT("The degradation setting can be toggled using the \"L\" key in\n"));
   MosPrintf(AIL_TEXT("the display window. The usage of LODs must be enabled on the point\n"));
   MosPrintf(AIL_TEXT("cloud to see a change in the display when changing this setting.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to continue.\n\n"));
   MosPrintf(AIL_TEXT("Level of Detail (lod) status : degrade on action \r"));

   int i = 0;
   const int MaxIterations = 100;
   for(; (i < MaxIterations) && !MosKbhit(); i++)
      {
      MosSleep(50);
      }
   M3ddispControl(Ail3dDisplay, M_AUTO_ROTATE, M_DISABLE);

   // Set the view of the 3d display.
   if(i == MaxIterations)
      {
      M3ddispSetView(Ail3dDisplay, M_INTEREST_POINT, 83, 83, 29, M_DEFAULT);
      M3ddispSetView(Ail3dDisplay, M_AZIM_ELEV_ROLL, 170.12, 35.26, 0.0, M_DEFAULT);
      }
   MosGetch();

   M3ddispHookFunction(
      Ail3dDisplay,
      M_KEY_DOWN + M_UNHOOK,
      DispKeyHandler,
      (void*)Ail3dDisplay);
   MosPrintf(AIL_TEXT("\n\n"));

   ChangeEditability(Ail3dGraList, BoxLabel, Translatable);
   ChangeEditability(Ail3dGraList, BoxLabel, Rotatable   );
   ChangeEditability(Ail3dGraList, BoxLabel, Scalable    );

   if(AskYesNo(AIL_TEXT("Do you want to save the point cloud?")))
      {
      MbufSave(OUTPUT_PC_NAME, CroppedContainer);
      }

   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   M3dgeoFree(CroppingBox);
   M3dgeoFree(BoundingBox);
   MbufFree(PointCloud);
   MbufFree(CroppedContainer);
   MbufFree(OriginalContainer);
   M3ddispFree(Ail3dDisplay);
   MappFreeDefault(AilApplication, AilSystem, M_NULL, M_NULL, M_NULL);

   return 0;
   }

//-----------------------------------------------------------------------------
// Handler call for modifying the editable box.
//-----------------------------------------------------------------------------
AIL_INT MFTYPE BoxModifiedHandler(AIL_INT /*HookType*/, AIL_ID /*EventId*/, void* UserDataPtr)
   {
   SPickStruct* PickStruct = static_cast<SPickStruct*>(UserDataPtr);
   RetrieveBoxAndCrop(PickStruct);

   return 0;
   }

//-----------------------------------------------------------------------------
// Handler call when key hit in disp.
//-----------------------------------------------------------------------------
AIL_INT MFTYPE DispKeyHandler(AIL_INT /*HookType*/, AIL_ID EventId, void* UserDataPtr)
   {
   // Not LOD key (M_KEY_L) so we ignore it.
   AIL_INT HitKey = M_NULL;
   M3ddispGetHookInfo(EventId, M_AIL_KEY_VALUE, &HitKey);
   if(HitKey != M_KEY_L)
      {
      return 0;
      }

   AIL_INT DisplayId          = reinterpret_cast<AIL_INT>(UserDataPtr);
   AIL_INT LodDegradeOnAction = M3ddispInquire(DisplayId, M_LOD_DEGRADE_MODE, M_NULL);

   // The hook is called before the setting is changed, therefore we write what it will be.
   MosPrintf(AIL_TEXT("Level of Detail (lod) status : %s           \r"),
             LodDegradeOnAction == M_ENABLE_ALWAYS ? AIL_TEXT("disabled") :
             LodDegradeOnAction == M_ENABLE_ON_ACTION ? AIL_TEXT("always degrade") : AIL_TEXT("degrade on action"));

   return 0;
   }

//-----------------------------------------------------------------------------
// Crops container based on associated box.
//-----------------------------------------------------------------------------
void RetrieveBoxAndCrop(SPickStruct* PickStruct)
   {
   // Retrieve the edited box from the graphics list.
   M3dgraCopy(PickStruct->Gralist,
              PickStruct->BoxLabel,
              PickStruct->Box,
              M_DEFAULT,
              M_GEOMETRY,
              M_DEFAULT);

   // Crop the point cloud using the retrieved box.
   M3dimCrop(PickStruct->OriginalContainer,
             PickStruct->CroppedContainer,
             PickStruct->Box,
             M_NULL,
             M_SAME,
             M_DEFAULT);
   }

//----------------------------------------------------------------------------
// Check for required files to run the example.
//----------------------------------------------------------------------------
bool CheckForRequiredAILFile(const AIL_STRING& FileName)
   {
   AIL_INT FilePresent = M_NO;

   MappFileOperation(M_DEFAULT,
                     FileName,
                     M_NULL,
                     M_NULL,
                     M_FILE_EXISTS,
                     M_DEFAULT,
                     &FilePresent);

   if(FilePresent == M_NO)
      {
      MosPrintf(AIL_TEXT("The footage needed to run this example is missing. You need \n")
                AIL_TEXT("to obtain and apply a separate specific update to have it.\n\n"));

      MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
      MosGetch();
      }

   return (FilePresent == M_YES);
   }

//-----------------------------------------------------------------------------
// Allocates a 3D display and returns its identifier.
//-----------------------------------------------------------------------------
AIL_ID Alloc3dDisplayId(AIL_ID AilSystem)
   {
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);
   AIL_ID AilDisplay3D =
      M3ddispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

   if(!AilDisplay3D)
      {
      MosPrintf(AIL_TEXT("\n")
                AIL_TEXT("The current system does not support the 3D display.\n")
                AIL_TEXT("Press any key to exit.\n"));
      MosGetch();
      }

   return AilDisplay3D;
   }

//----------------------------------------------------------------------------
// Modify editable settings.
//----------------------------------------------------------------------------
void ChangeEditability(AIL_ID Ail3dGraList, AIL_ID BoxLabel, eParentControl ParentControl)
   {
   std::vector<AIL_INT>            Controls;
   std::vector<AIL_CONST_TEXT_PTR> Choices;
   std::vector<bool>               Values;

   switch(ParentControl)
      {
      case Translatable:
         Controls = { M_EDITABLE, M_TRANSLATABLE, M_TRANSLATABLE_X,
                      M_TRANSLATABLE_Y, M_TRANSLATABLE_Z };

         Choices  = { AIL_TEXT("M_EDITABLE"), AIL_TEXT("M_TRANSLATABLE"),
                      AIL_TEXT("M_TRANSLATABLE_X"), AIL_TEXT("M_TRANSLATABLE_Y"),
                      AIL_TEXT("M_TRANSLATABLE_Z"), AIL_TEXT("continue") };
         break;
      case Rotatable:
         Controls = { M_EDITABLE, M_ROTATABLE, M_ROTATABLE_X,
                      M_ROTATABLE_Y, M_ROTATABLE_Z};

         Choices  = { AIL_TEXT("M_EDITABLE"), AIL_TEXT("M_ROTATABLE"),
                      AIL_TEXT("M_ROTATABLE_X"), AIL_TEXT("M_ROTATABLE_Y"),
                      AIL_TEXT("M_ROTATABLE_Z"), AIL_TEXT("continue") };
         break;
      case Scalable:
         Controls = { M_EDITABLE, M_SCALABLE, M_SCALABLE_X, M_SCALABLE_Y,
                      M_SCALABLE_Z, M_SCALABLE_X0, M_SCALABLE_X1 };

         Choices  = { AIL_TEXT("M_EDITABLE"), AIL_TEXT("M_SCALABLE"),
                      AIL_TEXT("M_SCALABLE_X"), AIL_TEXT("M_SCALABLE_Y"),
                      AIL_TEXT("M_SCALABLE_Z"), AIL_TEXT("M_SCALABLE_X0"),
                      AIL_TEXT("M_SCALABLE_X1"), AIL_TEXT("continue") };
         break;
      }

   for(int c = 0; c < Controls.size(); c++)
      {
      Values.emplace_back(
         M3dgraInquire(Ail3dGraList, BoxLabel, Controls[c], M_NULL) == M_ENABLE);
      }

   // Get user choice
   MosPrintf(AIL_TEXT("Select a control to swap (between M_ENABLE and M_DISABLE)\n"));
   MosPrintf(AIL_TEXT("by pressing its respective key in the console.\n"));
   MosPrintf(AIL_TEXT("%s takes precedence when disabled followed by %s"),
             Choices[0], Choices[1]);

   if(ParentControl == Scalable)
      {
      MosPrintf(AIL_TEXT(", %s and %s.\n"), Choices[2], Choices[5]);
      }
   else
      {
      MosPrintf(AIL_TEXT(" and %s.\n"), Choices[2]);
      }

   AIL_INT Choice = AskMakeChoice(Choices, Values);
   AIL_INT Control = M_NULL;
   if(Choice >= 0)
      {
      Control = Controls[Choice];
      }

   while(Control != M_NULL)
      {
      // Swap control value.
      M3dgraControl(Ail3dGraList, BoxLabel, Control, Values[Choice] ? M_DISABLE : M_ENABLE);
      Values[Choice] = !Values[Choice];

      // Get user choice.
      Choice = AskMakeChoice(Choices, Values, false);
      Control = M_NULL;
      if(Choice >= 0)
         {
         Control = Controls[Choice];
         }
      }
   MosPrintf(AIL_TEXT("\n\n"));
   }

//----------------------------------------------------------------------------
// Helper function for getting user choices.
//----------------------------------------------------------------------------
AIL_INT AskMakeChoice(const std::vector<AIL_CONST_TEXT_PTR>& Choices,
                      const std::vector<bool>& Values, bool Print)
   {
   AIL_INT Choice;

   // Print the choices.
   if(Print)
      {
      for(AIL_INT c = 0; c < (AIL_INT)Choices.size() - 1; c++)
         {
         AIL_STRING_STREAM ChoiceStream;
         ChoiceStream << c + 0 << ". ";
         ChoiceStream << Choices[c];
         MosPrintf(AIL_TEXT("\t%s\n"), ChoiceStream.str().c_str());
         }
      MosPrintf(AIL_TEXT("\tPress \"Enter\" to %s. \n\n"), Choices[Choices.size() - 1]);
      }

   // Print the choice values.
   for(int c = 0; c < Values.size(); c++)
      {
      if(c != 0)
         {
         MosPrintf(AIL_TEXT(", "));
         }
      AIL_CONST_TEXT_PTR Value = Values[c] ? AIL_TEXT("Enabled") : AIL_TEXT("Disabled");
      MosPrintf(AIL_TEXT("%d: %s"), c, Value);
      }
   MosPrintf(AIL_TEXT("     \r"));

   // Get the choice.
   do
      {
      Choice = MosGetch();
      if(Choice == '\r')
         {
         return -1;
         }
      Choice -= ('0' + 0);
      } while(Choice < 0 || Choice >= (AIL_INT)Choices.size() - 1);
   return Choice;
   }

//----------------------------------------------------------------------------
// Prompts user for yes/no.
//----------------------------------------------------------------------------
bool AskYesNo(AIL_CONST_TEXT_PTR QuestionString)
   {
   MosPrintf(AIL_TEXT("%s (y/n)?\n"), QuestionString);
   while(true)
      {
      switch(MosGetch())
         {
         case AIL_TEXT('y'):
         case AIL_TEXT('Y'):
            return true;

         case AIL_TEXT('n'):
         case AIL_TEXT('N'):
         case AIL_TEXT('\r'):
            return false;
         }
      }
   }

//----------------------------------------------------------------------------
// Obtains a point cloud either from file or by calculating with example data.
//----------------------------------------------------------------------------
AIL_ID ObtainPointCloud(AIL_STRING& PointCloudFile)
   {
   AIL_ID AilPointCloud;
   do
      {
      if(PointCloudFile != AIL_TEXT(""))
         AilPointCloud = RestorePointCloud(PointCloudFile.c_str());
      else if(AskYesNo(AIL_TEXT("Do you want to load a user point cloud")))
         {
         MosPrintf(AIL_TEXT("Select a .mbufc or .ply point cloud file.\n\n"));
         AilPointCloud = RestorePointCloud(M_NULL);
         }
      else
         {
         MosPrintf(AIL_TEXT("The example will run using a point cloud ")
                   AIL_TEXT("from example source data.\n\n"));
         if(!CheckForRequiredAILFile(PT_CLD_FILE))
            return M_NULL;
         AilPointCloud = RestorePointCloud(PT_CLD_FILE.c_str());
         }
      } while(!AilPointCloud);

      return AilPointCloud;
   }

//----------------------------------------------------------------------------
// Restores the registration result from file.
//----------------------------------------------------------------------------
AIL_ID RestorePointCloud(AIL_CONST_TEXT_PTR PointCloudFilename)
   {
   // Restore the 3dreg result.
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_DISABLE);
   auto AilPointCloud = MbufImport(PointCloudFilename, M_DEFAULT, M_RESTORE,
                                   M_DEFAULT_HOST, M_NULL);
   MappControl(M_DEFAULT, M_ERROR, M_PRINT_ENABLE);

   // The restored file must be a convertible container.
   if(AilPointCloud &&
      (MobjInquire(AilPointCloud, M_OBJECT_TYPE, M_NULL) != M_CONTAINER ||
       MbufInquireContainer(AilPointCloud, M_CONTAINER, M_3D_CONVERTIBLE, M_NULL) ==
       M_NOT_CONVERTIBLE))
      {
      MbufFree(AilPointCloud);
      AilPointCloud = M_NULL;
      }

   // Verify that the result is valid.
   if(!AilPointCloud)
      MosPrintf(AIL_TEXT("No valid .mbufc file restored.\n\n"));

   return AilPointCloud;
   }

```
