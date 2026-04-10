---
title: "Mfunc"
description: "This example shows the use of the Function Developer Toolkit and how AIL and custom code can be mixed to create a custom AIL function that accesses the data pointer of an AIL buffer directly in order to process it. The example creates a Main AIL function that registers all the parameters to AIL and calls the Worker function. The Worker function retrieves all the parameters, gets the pointers to the AIL image buffers, uses them to access the data directly, and adds a constant."
ms.language: "C#"
---

# Mfunc

> This example shows the use of the Function Developer Toolkit and how AIL and custom code can be mixed to create a custom AIL function that accesses the data pointer of an AIL buffer directly in order to process it. The example creates a Main AIL function that registers all the parameters to AIL and calls the Worker function. The Worker function retrieves all the parameters, gets the pointers to the AIL image buffers, uses them to access the data directly, and adds a constant.

**Language:** C#

**Functions used:** `MdispFree`, `MappAlloc`, `MappFree`, `MbufAlloc2d`, `MbufControl`, `MbufDiskInquire`, `MbufFree`, `MbufInquire`, `MbufLoad`, `MdispAlloc`, `MdispSelect`, `MsysAlloc`, `MsysFree`, `MsysInquire`, `MfuncAlloc`, `MfuncParamAilId`, `MfuncParamDataPointer`, `MfuncCall`, `MfuncErrorReport`, `MfuncFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, What's New, Older

```csharp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MFunc.cs 
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

using System;
using System.Collections.Generic;
using System.Runtime.InteropServices;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MFunc
{
    class Program
    {
        // Function specifications.
        private const int FUNCTION_NB_PARAM = 4;
        private const int FUNCTION_OPCODE_ADD_CONSTANT = 1;
        private const int FUNCTION_PARAMETER_ERROR_CODE = 1;
        private const int FUNCTION_SUPPORTED_IMAGE_TYPE = (8 + AIL.M_UNSIGNED);

        // Target image file name.
        private const string IMAGE_FILE = AIL.M_IMAGE_PATH + "BoltsNutsWashers.mim";

        // User function definition.
        // -------------------------------

        static AIL_INT AddConstant(AIL_ID SrcImageId, AIL_ID DstImageId, AIL_INT ConstantToAdd)
        {
            AIL_ID Func = AIL.M_NULL;
            AIL_INT WorkerReturnValue = 0;

            // Allocate a function context that will be used to call a target 
            // Worker function locally on the Host to do the processing.
            MFUNCFCTPTR WorkerAddConstantDelegate = new MFUNCFCTPTR(WorkerAddConstant);
            AIL.MfuncAlloc("AddConstant",
                       FUNCTION_NB_PARAM,
                       WorkerAddConstantDelegate, AIL.M_NULL, AIL.M_NULL,
                       AIL.M_USER_MODULE_1 + FUNCTION_OPCODE_ADD_CONSTANT,
                       AIL.M_LOCAL + AIL.M_SYNCHRONOUS_FUNCTION,
                       ref Func);

            // Register the parameters.
            AIL.MfuncParamAilId(Func, 1, SrcImageId, AIL.M_IMAGE, AIL.M_IN);
            AIL.MfuncParamAilId(Func, 2, DstImageId, AIL.M_IMAGE, AIL.M_OUT);
            AIL.MfuncParamAilInt(Func, 3, ConstantToAdd);

            // To pass a pointer to AIL, we need to use a reference type such as an array
            // to be able to pin the object and prevent the garbage collector from moving the object.
            AIL_INT[] workerReturnValueArray = new AIL_INT[1] { -1 };
            GCHandle workerReturnValueArrayHandle = GCHandle.Alloc(workerReturnValueArray, GCHandleType.Pinned);

            try
            {
                // Get the address of the pinned object, for an array, the address is pointing to the first element.
                IntPtr workerReturnValuePtr = workerReturnValueArrayHandle.AddrOfPinnedObject();
                AIL.MfuncParamDataPointer(Func, 4, workerReturnValuePtr, AIL_INT.Size * 2, AIL.M_OUT);

                // Call the target Worker function.
                AIL.MfuncCall(Func);

                WorkerReturnValue = workerReturnValueArray[0];

                // Make sure that the delegate survives garbage collection until the worker function is executed.
                GC.KeepAlive(WorkerAddConstantDelegate);
            }
            finally
            {
                // Free the allocated GCHandle to allow the array object to be garbage collected.
                if (workerReturnValueArrayHandle.IsAllocated)
                {
                    workerReturnValueArrayHandle.Free();
                }
            }

            // Free the function context.
            AIL.MfuncFree(Func);

            return WorkerReturnValue;
        }

        // Worker function definition.
        // ------------------------------

        static void WorkerAddConstant(AIL_ID Func)
        {
            AIL_ID SrcImageId = AIL.M_NULL;
            AIL_ID DstImageId = AIL.M_NULL;
            AIL_INT ConstantToAdd = 0;
            AIL_INT TempValue = 0;
            AIL_INT SrcImageDataPtr = AIL.M_NULL;
            AIL_INT DstImageDataPtr = AIL.M_NULL;
            AIL_INT SrcImageSizeX = 0;
            AIL_INT SrcImageSizeY = 0;
            AIL_INT SrcImageType = 0;
            AIL_INT SrcImagePitchByte = 0;
            AIL_INT DstImageSizeX = 0;
            AIL_INT DstImageSizeY = 0;
            AIL_INT DstImageType = 0;
            AIL_INT DstImagePitchByte = 0;
            int x = 0;
            int y = 0;
            IntPtr WorkerReturnValuePtr = IntPtr.Zero;

            // Read the parameters.
            AIL.MfuncParamValue(Func, 1, ref SrcImageId);
            AIL.MfuncParamValue(Func, 2, ref DstImageId);
            AIL.MfuncParamValue(Func, 3, ref ConstantToAdd);
            AIL.MfuncParamValue(Func, 4, ref WorkerReturnValuePtr);

            // Lock buffers for direct access.
            AIL.MbufControl(SrcImageId, AIL.M_LOCK, AIL.M_DEFAULT);
            AIL.MbufControl(DstImageId, AIL.M_LOCK, AIL.M_DEFAULT);

            // Read image information.
            AIL.MbufInquire(SrcImageId, AIL.M_HOST_ADDRESS, ref SrcImageDataPtr);
            AIL.MbufInquire(SrcImageId, AIL.M_SIZE_X, ref SrcImageSizeX);
            AIL.MbufInquire(SrcImageId, AIL.M_SIZE_Y, ref SrcImageSizeY);
            AIL.MbufInquire(SrcImageId, AIL.M_TYPE, ref SrcImageType);
            AIL.MbufInquire(SrcImageId, AIL.M_PITCH_BYTE, ref SrcImagePitchByte);
            AIL.MbufInquire(DstImageId, AIL.M_HOST_ADDRESS, ref DstImageDataPtr);
            AIL.MbufInquire(DstImageId, AIL.M_SIZE_X, ref DstImageSizeX);
            AIL.MbufInquire(DstImageId, AIL.M_SIZE_Y, ref DstImageSizeY);
            AIL.MbufInquire(DstImageId, AIL.M_TYPE, ref DstImageType);
            AIL.MbufInquire(DstImageId, AIL.M_PITCH_BYTE, ref DstImagePitchByte);

            // Reduce the destination area to process if necessary.
            if (SrcImageSizeX < DstImageSizeX) DstImageSizeX = SrcImageSizeX;
            if (SrcImageSizeY < DstImageSizeY) DstImageSizeY = SrcImageSizeY;

            unsafe // Unsafe code block to allow manipulating memory addresses
            {
                // If images have the proper depth, execute the operation using a custom code.
                if ((SrcImageType == DstImageType) && (SrcImageType == FUNCTION_SUPPORTED_IMAGE_TYPE) &&
                    (SrcImageDataPtr != AIL.M_NULL) && (DstImageDataPtr != AIL.M_NULL))
                {
                    // Convert the address inquired from Aurora Imaging Library to a fixed size pointer.
                    // The AIL_INT type cannot be converted to a byte * so use the
                    // IntPtr portable type 
                    // 
                    IntPtr SrcImageDataPtrIntPtr = SrcImageDataPtr;
                    IntPtr DstImageDataPtrIntPtr = DstImageDataPtr;
                    byte* SrcImageDataAddr = (byte*)SrcImageDataPtrIntPtr;
                    byte* DstImageDataAddr = (byte*)DstImageDataPtrIntPtr;

                    // Add the constant to the image.
                    for (y = 0; y < DstImageSizeY; y++)
                    {
                        for (x = 0; x < DstImageSizeX; x++)
                        {
                            // Calculate the value to write.
                            TempValue = (AIL_INT)SrcImageDataAddr[x] + (AIL_INT)ConstantToAdd;

                            // Write the value if no overflow, else saturate.
                            if (TempValue <= 0xff)
                            {
                                DstImageDataAddr[x] = (byte)TempValue;
                            }
                            else
                            {
                                DstImageDataAddr[x] = 0xff;
                            }
                        }

                        // Move pointer to the next line taking into account the image's pitch.
                        SrcImageDataAddr += SrcImagePitchByte;
                        DstImageDataAddr += DstImagePitchByte;
                    }

                    // Return a null error code to the User function.
                    AIL_INT.WriteMilInt(WorkerReturnValuePtr, AIL.M_NULL);
                }
                else
                {
                    // Buffer cannot be processed. Report an error.
                    AIL.MfuncErrorReport(Func, AIL.M_FUNC_ERROR + FUNCTION_PARAMETER_ERROR_CODE,
                                     "Invalid parameter.",
                                     "Image type not supported or invalid target system.",
                                     "Image depth must be 8-bit and have a valid host address.",
                                     AIL.M_NULL);

                    // Return an error code to the User function.
                    AIL_INT.WriteMilInt(WorkerReturnValuePtr, AIL.M_FUNC_ERROR + FUNCTION_PARAMETER_ERROR_CODE);
                }
            }

            // Unlock buffers.
            AIL.MbufControl(SrcImageId, AIL.M_UNLOCK, AIL.M_DEFAULT);
            AIL.MbufControl(DstImageId, AIL.M_UNLOCK, AIL.M_DEFAULT);

            // Signal that the destination buffer was modified.
            AIL.MbufControl(DstImageId, AIL.M_MODIFIED, AIL.M_DEFAULT);
        }

        // Main to test the custom function. //
        // --------------------------------- //

        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;     // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;          // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;         // Display identifier.
            AIL_ID AilImage = AIL.M_NULL;           // Image buffer identifier.
            AIL_INT ReturnValue = 0;                // Return value of the function.

            // Allocate default application, system, display and image.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, AIL.M_NULL, AIL.M_NULL);

            // Load source image into a Host memory image buffer.
            AIL.MbufAlloc2d(AilSystem,
                        AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_X),
                        AIL.MbufDiskInquire(IMAGE_FILE, AIL.M_SIZE_Y),
                        8 + AIL.M_UNSIGNED,
                        AIL.M_IMAGE + AIL.M_DISP + AIL.M_HOST_MEMORY,
                        ref AilImage);
            AIL.MbufLoad(IMAGE_FILE, AilImage);

            // Display the image.
            AIL.MdispSelect(AilDisplay, AilImage);

            // Pause.
            Console.WriteLine();
            Console.WriteLine("Function Developer Toolkit:");
            Console.WriteLine("---------------------------------");
            Console.WriteLine();
            Console.WriteLine("This example creates a custom function that processes");
            Console.WriteLine("an image using its data pointer directly.");
            Console.WriteLine();
            Console.WriteLine("Target image was loaded.");
            Console.WriteLine("Press a key to continue.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Run the custom function only if the target system's memory is local and accessible.
            if (AIL.MsysInquire(AilSystem, AIL.M_LOCATION, AIL.M_NULL) == AIL.M_LOCAL)
            {
                // Process the image directly with the custom function.
                ReturnValue = AddConstant(AilImage, AilImage, 0x40);

                // Print a conclusion message.
                if (ReturnValue == AIL.M_NULL)
                {
                    Console.WriteLine("The white level of the image was augmented.");
                }
                else
                {
                    Console.WriteLine("An error was returned by the Worker function.");
                }
            }
            else
            {
                // Print that the example don't run remotely.
                Console.WriteLine("This example doesn't run with Distributed AIL.");
            }

            // Wait for a key to terminate.
            Console.WriteLine("Press a key to terminate.");
            Console.WriteLine();
            Console.ReadKey(true);

            // Free all allocations.
            AIL.MbufFree(AilImage);
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AIL.M_NULL, AIL.M_NULL);
        }
    }
}

```
