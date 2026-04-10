---
doctype: BoardSpecificNotes
part: ""
chapter: 4Sight
section: Additional_4Sight_notes_Command_reference
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / 4sight / Additional 4Sight notes Command reference"
---

# Miscellaneous reference information for Zebra 4Sight

This section includes additional command reference information for Zebra 4Sight. The information found in this section might be a reiteration of content previously documented.

The following modules are mentioned in this section:

- [Msys](S06_Additional_4Sight_notes_Command_reference.md).

## Msys

This section presents information about Zebra 4Sight and the system module.

### MsysControl/Inquire

The following lists additional 4Sight support when using [`MsysControl`](../../Reference/sys/MsysControl.md)/[`MsysInquire`](../../Reference/sys/MsysInquire.md).

- Support for inquire of an output: **M_USER_BIT_STATE + M_USER_BIT[0...15]**. Possible inquire values include:
  - **M_OFF**.
  - **M_ON**.
- Support for inquire of an output: **M_USER_BIT_STATE_ALL**. Possible inquire values include:
  - **M_OFF**.
  - **M_ON**.
- Support for inquire of an output:**M_IO_STATUS + M_AUX_IO[0...15]**.
  Support for inquire of an input: **M_IO_STATUS + M_AUX_IO[16...31]**.
- Support for**M_IO_INTERRUPT_ACTIVATION + M_AUX_IO[16...31]**. Possible control/inquire values include:
  - **M_DEFAULT** or **M_EDGE_RISING**.
  - **M_EDGE_FALLING**.
- Support for**M_IO_INTERRUPT_STATE + M_AUX_IO[16...31]**. Possible control/inquire values include:
  - **M_DEFAULT** or **M_DISABLE**.
  - **M_ENABLE**.
- Support for**M_IO_MODE + M_AUX_IO[n]**. Possible control/inquire values include:
  - **M_OUTPUT** for n = 0...15.
  - **M_INPUT** for n = 15...31.
- Support for reading the current encoder position with [`MsysInquire`](../../Reference/sys/MsysInquire.md).
  **MsysInquire(Sys, M_ROTARY_ENCODER_POSITION+M_ROTARY_ENCODERn, &Position)**.
  Select _n_ as 1 or 2 to choose the target encoder.
  Position is an **AIL_INT**.
- Support for reading the current encoder position based on a signal event with [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md). Possible events include:
  - The following associates a latch with a specific rotary encoder:
    **MsysControl(Sys, M_SYS_DATA_LATCH_TYPE+M_LATCHm, M_ROTARY_ENCODERn)**.
    Select _m_ as 1 or 2 to choose the target latch storage.
    Select _n_ as 1 or 2 to choose the target encoder.
  - The following determines which signal will latch. In this case, the rotary encoder position:
    **MsysControl(Sys, M_SYS_DATA_LATCH_TRIGGER_SOURCE+M_LATCHm, LatchTriggerSource)**.
    Select _m_ as 1 or 2 to choose the target latch storage.
    LatchTriggerSource is an **AIL_INT** initialized with **M_AUX_IOi**, where i is from 8 to 15 or **M_TIMERj**, where j is from 1 to 16, on Zebra Indio.
  - The following determines which signal edge will latch the encoder position:
    **MsysControl(Sys, M_SYS_DATA_LATCH_TRIGGER_ACTIVATION+M_LATCHm, LatchTriggerActivation)**.
    Select _m_ as 1 or 2 to choose the target latch storage.
    LatchTriggerActivation is an **AIL_INT** initialized with **M_EDGE_RISING**, **M_EDGE_FALLING**, or **M_ANY_EDGE**.
  - The following activates (or deactivates) the given latch mechanism:
    **MsysControl(AilSystem, M_SYS_DATA_LATCH_STATE+M_LATCHm, LatchState)**.
    Select _m_ as 1 or 2 to choose the target latch storage.
    LatchState is an **AIL_INT** initialized with **M_ENABLE** or **M_DISABLE**.
  - The following allows you to retrieve the latched value in the context of a hook-handler function registered with [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md):
    **MsysGetHookInfo(EventId, M_SYS_DATA_LATCH_VALUE+M_LATCHm, LatchValue)**.
    Select _m_ as 1 or 2 to choose the target latch storage.
    **LatchValue** is an **AIL_UINT64** that returns the rotary encoder position that is latched.
    Note that if **M_SYS_DATA_LATCH_TRIGGER_SOURCE** was specified as **M_AUX_IOi**, the [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) call is done in the scope of a hook function to **M_IO_CHANGE**. If **M_SYS_DATA_LATCH_TRIGGER_SOURCE** was specified as **M_TIMERj**, the [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) call is done in the scope of a hook function to **M_TIMER_START** or **M_TIMER_END**.
