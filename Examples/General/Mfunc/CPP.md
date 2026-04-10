---
title: "Mfunc"
description: "This example shows the use of the Function Developer Toolkit and how AIL and custom code can be mixed to create a custom AIL function that accesses the data pointer of an AIL buffer directly in order to process it. The example creates a Main AIL function that registers all the parameters to AIL and calls the Worker function. The Worker function retrieves all the parameters, gets the pointers to the AIL image buffers, uses them to access the data directly, and adds a constant."
ms.language: "C++"
---

# Mfunc

> This example shows the use of the Function Developer Toolkit and how AIL and custom code can be mixed to create a custom AIL function that accesses the data pointer of an AIL buffer directly in order to process it. The example creates a Main AIL function that registers all the parameters to AIL and calls the Worker function. The Worker function retrieves all the parameters, gets the pointers to the AIL image buffers, uses them to access the data directly, and adds a constant.

**Language:** C++

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufControl`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MfuncAlloc`, `MfuncParamAilId`, `MfuncParamDataPointer`, `MfuncCall`, `MfuncErrorReport`, `MfuncFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MFunc.cpp 
//
// Description: This example shows the use of the function Developer Toolkit and how 
//              Aurora Imaging Library and custom code can be mixed to create a custom function that 
//              accesses the data pointer of a buffer directly in order to process it.
//              
//              The example creates a User function that registers all the parameters 
//              to Aurora Imaging Library and calls the Worker function. The Worker function
//              retrieves all the parameters, gets the pointers to the image buffers, uses them to
//              access the data directly and adds a constant.
//             
// Note:        The images must be 8-bit unsigned and have a valid Host pointer.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h>

// function specifications. 
#define FUNCTION_NB_PARAM                 4
#define FUNCTION_OPCODE_ADD_CONSTANT      1 
#define FUNCTION_PARAMETER_ERROR_CODE     1
#define FUNCTION_SUPPORTED_IMAGE_TYPE     (8+M_UNSIGNED)

// Target image file name. 
#define IMAGE_FILE M_IMAGE_PATH AIL_TEXT("BoltsNutsWashers.mim")

// Custom functions declarations. 
AIL_INT AddConstant(AIL_ID SrcImageId, AIL_ID DstImageId, AIL_INT ConstantToAdd);
void MFTYPE WorkerAddConstant(AIL_ID Func);


// Custom function definition. 
// ------------------------------- 

AIL_INT AddConstant(AIL_ID SrcImageId, AIL_ID DstImageId, AIL_INT ConstantToAdd)
{
   AIL_ID   Func;
   AIL_INT  WorkerReturnValue = 0;

   // Allocate a function context that will be used to call a target 
   // Worker function locally on the Host to do the processing.
   MfuncAlloc(AIL_TEXT("AddConstant"), 
              FUNCTION_NB_PARAM,
              WorkerAddConstant, M_NULL, M_NULL, 
              M_USER_MODULE_1+FUNCTION_OPCODE_ADD_CONSTANT, 
              M_LOCAL+M_SYNCHRONOUS_FUNCTION, 
              &Func
             );

   // Register the parameters. 
   MfuncParamAilId( Func, 1, SrcImageId, M_IMAGE, M_IN);
   MfuncParamAilId( Func, 2, DstImageId, M_IMAGE, M_OUT);
   MfuncParamAilInt(Func, 3, ConstantToAdd);
   MfuncParamDataPointer(Func, 4, &WorkerReturnValue, sizeof(AIL_INT), M_OUT);

   // Call the target Worker function. 
   MfuncCall(Func);

   // Free the function context. 
   MfuncFree(Func);
   
   // Return the value recieved from the Worker function. 
   return WorkerReturnValue;
}


// Worker function definition. 
// ------------------------------ 

void MFTYPE WorkerAddConstant(AIL_ID Func)
{
  AIL_ID    SrcImageId, DstImageId;
  AIL_INT   ConstantToAdd, TempValue;
  unsigned  char *SrcImageDataPtr, *DstImageDataPtr;
  AIL_INT   SrcImageSizeX, SrcImageSizeY, SrcImageType, SrcImagePitchByte;
  AIL_INT   DstImageSizeX, DstImageSizeY, DstImageType, DstImagePitchByte;
  AIL_INT   x, y;
  AIL_INT  *WorkerReturnValuePtr;

  // Read the parameters. 
  MfuncParamValue(Func, 1, &SrcImageId);
  MfuncParamValue(Func, 2, &DstImageId);
  MfuncParamValue(Func, 3, &ConstantToAdd); 
  MfuncParamValue(Func, 4, &WorkerReturnValuePtr); 

  // Lock buffers for direct access. 
  MbufControl(SrcImageId, M_LOCK, M_DEFAULT);
  MbufControl(DstImageId, M_LOCK, M_DEFAULT);

  // Read image information. 
  MbufInquire(SrcImageId, M_HOST_ADDRESS, &SrcImageDataPtr);
  MbufInquire(SrcImageId, M_SIZE_X,       &SrcImageSizeX);
  MbufInquire(SrcImageId, M_SIZE_Y,       &SrcImageSizeY);
  MbufInquire(SrcImageId, M_TYPE,         &SrcImageType);
  MbufInquire(SrcImageId, M_PITCH_BYTE,   &SrcImagePitchByte);
  MbufInquire(DstImageId, M_HOST_ADDRESS, &DstImageDataPtr);
  MbufInquire(DstImageId, M_SIZE_X,       &DstImageSizeX);
  MbufInquire(DstImageId, M_SIZE_Y,       &DstImageSizeY);
  MbufInquire(DstImageId, M_TYPE,         &DstImageType);
  MbufInquire(DstImageId, M_PITCH_BYTE,   &DstImagePitchByte);

  // Reduce the destination area to process if necessary. 
  if (SrcImageSizeX < DstImageSizeX)   DstImageSizeX = SrcImageSizeX;
  if (SrcImageSizeY < DstImageSizeY)   DstImageSizeY = SrcImageSizeY;

  // If images have the proper type and a valid host pointer,
  //   execute the operation using custom C code.

  if ((SrcImageType == DstImageType) && (SrcImageType == FUNCTION_SUPPORTED_IMAGE_TYPE) &&
      (SrcImageDataPtr != M_NULL) && (DstImageDataPtr != M_NULL)
     )
     {
     // Add the constant to the image. 
     for (y= 0; y < DstImageSizeY; y++)
        {
        for (x= 0; x < DstImageSizeX; x++) 
           {
           // Calculate the value to write. 
           TempValue = (AIL_INT)SrcImageDataPtr[x] + (AIL_INT)ConstantToAdd;

           // Write the value if no overflow, else saturate. 
           if (TempValue <= 0xff)
              DstImageDataPtr[x] = (unsigned char)TempValue;
           else 
              DstImageDataPtr[x] = 0xff;
           }

         // Move pointer to the next line taking into account the image's pitch. 
         SrcImageDataPtr += SrcImagePitchByte;
         DstImageDataPtr += DstImagePitchByte;
        }
     
     // Return a null error code to the User function. 
     *WorkerReturnValuePtr = M_NULL;
     }
  else 
     {
     // Buffer cannot be processed. Report an error.  
     MfuncErrorReport(Func,M_FUNC_ERROR+FUNCTION_PARAMETER_ERROR_CODE,
                      AIL_TEXT("Invalid parameter."),
                      AIL_TEXT("Image type not supported or invalid target system."),
                      AIL_TEXT("Image must be 8-bit and have a valid host address."),
                      M_NULL
                     );

     // Return an error code to the User function. 
     *WorkerReturnValuePtr = M_FUNC_ERROR+FUNCTION_PARAMETER_ERROR_CODE;
     }

  // Unlock buffers. 
  MbufControl(SrcImageId, M_UNLOCK, M_DEFAULT);
  MbufControl(DstImageId, M_UNLOCK, M_DEFAULT);

  // Signal that the destination buffer was modified. 
  MbufControl(DstImageId, M_MODIFIED, M_DEFAULT); 
}


// Main to test the custom function. 
// --------------------------------- 

int MosMain(void)
{
   AIL_ID  AilApplication,  // Application identifier.       
           AilSystem,       // System identifier.             
           AilDisplay,      // Display identifier.           
           AilImage;        // Image buffer identifier.      
   AIL_INT ReturnValue;     // Return value of the function. 

   // Allocate default application, system, display and image. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem, &AilDisplay, M_NULL, M_NULL);

   // Load source image into a Host memory image buffer. 
   MbufAlloc2d(AilSystem, 
               MbufDiskInquire(IMAGE_FILE, M_SIZE_X, M_NULL), 
               MbufDiskInquire(IMAGE_FILE, M_SIZE_Y, M_NULL), 
               8+M_UNSIGNED, 
               M_IMAGE+M_DISP+M_HOST_MEMORY,
               &AilImage);   
   MbufLoad(IMAGE_FILE, AilImage);
   
   // Display the image. 
   MdispSelect(AilDisplay, AilImage);
   
   // Pause. 
   MosPrintf(AIL_TEXT("\nFunction Developer Toolkit:\n"));
   MosPrintf(AIL_TEXT("---------------------------------\n\n"));
   MosPrintf(AIL_TEXT("This example creates a custom function that processes\n"));
   MosPrintf(AIL_TEXT("an image using its data pointer directly.\n\n"));
   MosPrintf(AIL_TEXT("Target image was loaded.\n"));
   MosPrintf(AIL_TEXT("Press a key to continue.\n\n"));
   MosGetch();

   // Run the custom function only if the target system's memory is local and accessible. 
   if (MsysInquire(AilSystem, M_LOCATION, M_NULL) == M_LOCAL)
      {
      // Process the image directly with the custom function. 
      ReturnValue = AddConstant(AilImage, AilImage, 0x40);
   
      // Print a conclusion message. 
      if (ReturnValue == M_NULL)
         MosPrintf(AIL_TEXT("The white level of the image was augmented.\n"));
      else
         MosPrintf(AIL_TEXT("An error was returned by the Worker function.\n"));
      }
   else
      {
      // Print that the example don't run remotely. 
      MosPrintf(AIL_TEXT("This example doesn't run with Distributed Aurora Imaging Library.\n"));
      }
   
   // Wait for a key to terminate. 
   MosPrintf(AIL_TEXT("Press a key to terminate.\n\n"));
   MosGetch();

   // Free all allocations. 
   MbufFree(AilImage);
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, M_NULL, M_NULL);

   return 0;
}

```
