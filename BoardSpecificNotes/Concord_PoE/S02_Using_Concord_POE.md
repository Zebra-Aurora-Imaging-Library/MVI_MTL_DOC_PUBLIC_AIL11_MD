---
doctype: BoardSpecificNotes
part: ""
chapter: Concord_PoE
section: Using_Concord_POE
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / Concord_PoE / Using Concord POE"
---

# Using Zebra Concord PoE

To use Zebra Concord PoE with Aurora Imaging Library for acquisition, you must allocate an Aurora Imaging Library GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens communication with GigE Vision compliant cameras (through one or more Gigabit Ethernet network adapters in your computer). You can then access and grab from multiple connected GigE Vision cameras by allocating a digitizer for each connected camera (using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEVn`](../../Reference/dig/MdigAlloc.md)). Each process (executable) can allocate only one Aurora Imaging Library GigE Vision system. Each camera can be used by one process (executable) at a time. For additional information, refer to [Zebra GigE Vision driver](../GigE_Vision/ChapterInformation.md).

To use other functionality available on Zebra Concord PoE with ToE or inquire about the status (for example, BoardType) of either model of the board, you must allocate an Aurora Imaging Library Concord PoE system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md).

> **Note:** Be aware that to access the Aurora Imaging Library license that is included on the Zebra Concord PoE board, you must install the Zebra Concord PoE driver.

## Using the Advanced I/O Engine of Zebra Concord PoE with ToE

Zebra Concord PoE with ToE has an Advanced I/O Engine that controls the auxiliary I/O interface. The engine includes 16 timers, 2 rotary decoders, 2 I/O command lists, a user-output register, 2 auxiliary output signals, 6 auxiliary input signals and a Trigger-over-Ethernet (ToE) module. To route I/O signals, configure interrupts, and set the mode of an auxiliary signal, use [`MsysControl`](../../Reference/sys/MsysControl.md). To inquire this information, use [`MsysInquire`](../../Reference/sys/MsysInquire.md) For more information on some of these features, refer to [I/O signals and communicating with external devices](../../UserGuide/C56_IO_signals/ChapterInformation.md). For information on how to use the ToE module to send a Trigger-over-Ethernet packet (as an action command or GigE Vision software trigger) to the camera, refer to [Sending a Trigger-over-Ethernet packet without Host intervention](S04_Sending_a_ToE_trigger_packet.md).
