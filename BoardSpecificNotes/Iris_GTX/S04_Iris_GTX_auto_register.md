---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Iris_GTX_auto_register
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Iris GTX auto register"
---

# Automatically scheduling an output upon the occurrence of an event

Zebra Iris GTX supports I/O command lists. An I/O command list is an internal container for Aurora Imaging Library I/O commands. For information on I/O command lists, refer to [Using I/O command lists](../../UserGuide/C56_IO_signals/S08_Using_IO_command_lists.md).

Besides supporting scheduling I/O commands on an individual basis, Zebra Iris GTX supports automatically registering an I/O command every time a specified latch is triggered, and using that latch's value as a reference. To do so, use [`MsysIoCommandRegister`](../../Reference/sysIo/MsysIoCommandRegister.md) with an operation, [`M_AUTO_REGISTER`](../../Reference/sysIo/MsysIoCommandRegister.md), and [`M_LATCHn`](../../Reference/sysIo/MsysIoCommandRegister.md).

In this example, parts are moving along a conveyor belt, but are fixed in their position on the conveyor belt. The parts are always taking 100 rotary steps forward before reaching a camera, which will always acquire an image of the parts. For simplicity, the conveyor belt can only move forward and the camera is an areascan camera.

*[Image: IO_command_list_autoregister.png]*

An I/O command list is used to schedule the acquisition of the image. The rotary decoder output is used as the counter source of the I/O command list so that each time the rotary decoder counts 100 steps (distance that the part travels) after the sensor detects a part, an image is acquired.

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401)

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case402)

> **Code example:** [boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403](boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403)
