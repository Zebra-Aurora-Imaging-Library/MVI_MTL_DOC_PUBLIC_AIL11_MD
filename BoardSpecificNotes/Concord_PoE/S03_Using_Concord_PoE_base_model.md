---
doctype: BoardSpecificNotes
part: ""
chapter: Concord_PoE
section: Using_Concord_PoE_base_model
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / Concord_PoE / Using Concord PoE base model"
---

# Using an Aurora Imaging Library Concord PoE system with the Zebra Concord PoE base model

To use Zebra Concord PoE for acquisition, you must allocate and use an Aurora Imaging Library GigE Vision system ([`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)); refer to information denoted for a GigE Vision system. You only need to allocate and use an Aurora Imaging Library Concord PoE system ([`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md)) to use the other functionality on the board and to inquire about the board itself. This means that you rarely use an Aurora Imaging Library Concord PoE system with the Zebra Concord PoE base model.

For this reason, in the reference, information pertinent to using an Aurora Imaging Library Concord PoE system is identified with a column for the Zebra Concord PoE with ToE; most of the functions and constants are typically only available to this model. If the information is applicable to the Zebra Concord PoE base model, there will be a note included to specify this.

The following are the only functions and constants that can be used with an Aurora Imaging Library Concord PoE system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md)) when allocated for a Zebra Concord PoE base model:

| Function | Constants supported |
| --- | --- |
| [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) | [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md) with [`M_DEVn`](../../Reference/sys/MsysAlloc.md) or user-defined board name. |
| [`MsysFree`](../../Reference/sys/MsysFree.md) | All functionality is available. |
| [`MsysControl`](../../Reference/sys/MsysControl.md) | [`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md), [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysControl.md). |
| [`MsysInquire`](../../Reference/sys/MsysInquire.md) | [`M_BOARD_TYPE`](../../Reference/sys/MsysInquire.md), [`M_DEVICE_NAME`](../../Reference/sys/MsysInquire.md), [`M_DEVICE_NAME_MAX_SIZE`](../../Reference/sys/MsysInquire.md), [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysInquire.md), [`M_SERIAL_NUMBER`](../../Reference/sys/MsysInquire.md), [`M_SERIAL_NUMBER`](../../Reference/sys/MsysInquire.md), [`M_SYSTEM_DESCRIPTOR`](../../Reference/sys/MsysInquire.md), [`M_SYSTEM_DESCRIPTOR`](../../Reference/sys/MsysInquire.md), [`M_SYSTEM_TYPE`](../../Reference/sys/MsysInquire.md). |

Action commands are also supported on Zebra Concord PoE base model; however, you need to use an Aurora Imaging Library GigE Vision system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)).
