---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
    // Setup triggered grab.
    MdigControl(AilDigitizer, M_GRAB_TRIGGER_SOURCE, M_AUX_IO0);
    MdigControl(AilDigitizer, M_GRAB_TRIGGER_ACTIVATION, M_EDGE_RISING);
    MdigControl(AilDigitizer, M_GRAB_TRIGGER_STATE, M_ENABLE);

    // Setup the rorary decoder; for simplicty, we assume the conveyor cannot move backwards.
    MsysControl(SysId, M_ROTARY_ENCODER_BIT0_SOURCE + M_ROTARY_ENCODER1, M_AUX_IO14);
    MsysControl(SysId, M_ROTARY_ENCODER_BIT1_SOURCE + M_ROTARY_ENCODER1, M_AUX_IO15);
    MsysControl(SysId, M_ROTARY_ENCODER_OUTPUT_MODE + M_ROTARY_ENCODER1, M_STEP_FORWARD);
    MsysControl(SysId, M_ROTARY_ENCODER_STATE + M_ROTARY_ENCODER1, M_ENABLE);

    // Link the grab trigger to an I/O command register bit.
    MsysControl(SysId, M_IO_SOURCE + M_AUX_IO0, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT0);

    // Allocate an I/O command list where I/O commands are scheduled based on rotary decoder 1's output signal.
    CmdListId = MsysIoAlloc(SysId, M_IO_COMMAND_LIST1, M_IO_COMMAND_LIST, M_ROTARY_ENCODER1, M_NULL);
    if (CmdListId!= M_NULL)
    {

        // Latch the counter upon detection of the object
        MsysIoControl(CmdListId, M_REFERENCE_LATCH_TRIGGER_SOURCE+M_LATCH1, M_AUX_IO8);
        MsysIoControl(CmdListId, M_REFERENCE_LATCH_ACTIVATION+M_LATCH1, M_EDGE_RISING);
        MsysIoControl(CmdListId, M_REFERENCE_LATCH_STATE+M_LATCH1, M_ENABLE);

        // Every one hundred decoder steps after latch 1 is triggered, two I/O commands 
        // are automatically added to the I/O command list to change register bit 0, 
        // such that M_AUX_IO0 outputs an active high pulse.
        // Since M_AUX_IO0 is the grab trigger source, a frame will always be grabbed 100 
        // decoder steps after latch 1 is triggered. 
        MsysIoCommandRegister(CmdListId, M_PULSE_HIGH+M_AUTO_REGISTER, M_LATCH1, CAMERA_TRIGGER_OFFSET, 1, M_IO_COMMAND_BIT0, M_NULL);

        // Start the grabbing and processing. 
        // The processing function is called each time a frame is grabbed. 
        MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize,
            M_START, M_DEFAULT, ProcessingFunction, &HookParam);

        // Print a message and wait for a key press before stopping the processing.
        MosPrintf(AIL_TEXT("Press <Enter> to stop.                    \n\n"));
        MosGetch();

        HookParam.AilSystem = SysId;
        HookParam.CmdListId = CmdListId;

        // Stop the grabbing and processing.
        MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize,
            M_STOP, M_DEFAULT, ProcessingFunction, &HookParam);

        // Release the auto-register, so that, if latch 1 is triggered, it will not cause
        // an I/O command to be added.
        MsysIoCommandRegister(CmdListId, M_PULSE_HIGH + M_AUTO_REGISTER_CANCEL, M_LATCH1, CAMERA_TRIGGER_OFFSET, 1, M_IO_COMMAND_BIT0, M_NULL);

    }

	// Free the I/O command list.
    MsysIoFree(CmdListId);
```
