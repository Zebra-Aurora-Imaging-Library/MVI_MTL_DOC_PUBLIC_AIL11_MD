---
doctype: BoardSpecificNotes
part: ""
chapter: Indio
section: Additional_Indio_notes_Command_reference
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / indio-u53 / Additional Indio notes Command reference"
---

# Miscellaneous reference information for Zebra Indio

This section includes additional command reference information for Zebra Indio. The information found in this section might be a reiteration of content previously documented.

The following modules are mentioned in this section:

- [Mcom](S05_Additional_Indio_notes_Command_reference.md).
- [Msys](S05_Additional_Indio_notes_Command_reference.md).
- [MsysIo](S05_Additional_Indio_notes_Command_reference.md).

## Mcom

This section presents information about Zebra Indio and the communication module.

To initiate communication on Zebra Indio using the Mcom module, the Aurora Imaging Library system identifier should be of **M_SYSTEM_INDIO** that was allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). For more information, refer to Host system column in the MsysIo command reference.

## Msys

This section presents information about Zebra Indio and the system module.

To configure the auxiliary signals, use the **M_IO_...** control types.

To use an auxiliary input signal as a trigger source, use [`MsysControl`](../../Reference/sys/MsysControl.md) or [`MsysIoControl`](../../Reference/sysIo/MsysIoControl.md) with an **M_..._TRIGGER_SOURCE** control type. Your application can also act upon and interpret the state of an auxiliary input signal.

To poll the state of an auxiliary input signal, use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with **M_IO_STATUS**. The state of an auxiliary input signal can also generate an interrupt. To do so, use [`MsysControl`](../../Reference/sys/MsysControl.md) with **M_IO_INTERRUPT_STATE** and then use [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with **M_IO_CHANGE** to hook a function to this event (that is, to set up an event handler).

There are 16 timers available. Use [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with **M_TIMER_START** or **M_TIMER_END** to hook a function to the respective start or end of a timer.

### MsysInquire

The following lists additional Indio support when using [`MsysInquire`](../../Reference/sys/MsysInquire.md).

- Support for inquiring the current encoder position:
  **MsysInquire(Sys, M_ROTARY_ENCODER_POSITION+M_ROTARY_ENCODERn, &Position)**.
  Select _n_ as 1 or 2 to choose the target encoder.
  Position is an AIL_INT.
- Support for inquiring the encoder position based on a signal event. Possible events include:
  - The following associates a latch with a rotary encoder:
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

## MsysIo

This section presents information about Zebra Indio and the I/O sub-module of the system module.

To initiate an Aurora Imaging Library I/O command list on Zebra Indio using the MsysIo module, the Aurora Imaging Library system identifier should be of **M_SYSTEM_INDIO** that was allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). For more information, refer to Host system column in the MsysIo command reference.
