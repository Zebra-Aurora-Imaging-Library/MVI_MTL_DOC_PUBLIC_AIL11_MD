---
doctype: BoardSpecificNotes
part: ""
chapter: Concord_PoE
section: Sending_a_ToE_trigger_packet
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / Concord_PoE / Sending a ToE trigger packet"
---

# Sending a Trigger-over-Ethernet packet without Host intervention

Although both models of Zebra Concord PoE support sending Trigger-over-Ethernet packets as described in [Introducing action commands](../GigE_Vision/S11_GigE_Vision_20_changes.md), the Zebra Concord PoE with ToE model can also do so without Host intervention.

Zebra Concord PoE with ToE includes a Trigger-over-Ethernet (ToE) module that can send a Trigger-over-Ethernet packet (action command or GigE Vision software trigger) over Ethernet, upon reception of an internal or external event, without Host intervention. The ToE module reduces latency, jitter, and is useful for applications that require a high degree of trigger accuracy. Since most GigE Vision cameras include a hardware implementation for receiving action commands, sending an action command is likely to provide a better result than sending a GigE Vision software trigger. For an explanation on action commands, see[Introducing action commands](../GigE_Vision/S11_GigE_Vision_20_changes.md). If your camera does not support action commands, or does not have hardware implemented for receiving them, then using a GigE Vision software trigger is an alternative option.

## Sending a Trigger-over-Ethernet packet as an action command

To configure the ToE module to send a Trigger-over-Ethernet packet as an action command, you follow similar steps as when sending an action command in software. However, you set up the action command on an Aurora Imaging Library Concord PoE system instead of an Aurora Imaging Library GigE system; you still set up the cameras using digitizers allocated on an Aurora Imaging Library GigE system. Specifically:

1. Allocate an Aurora Imaging Library Concord PoE system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md).
2. Allocate an Aurora Imaging Library GigE system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md). Then, allocate a digitizer on this system for each GigE camera that is physically connected to the Zebra Concord PoE and will receive the action command, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).
3. Set up the action command on the Aurora Imaging Library Concord PoE system. To do so, make successive calls to [`MsysControl`](../../Reference/sys/MsysControl.md) with the Aurora Imaging Library Concord PoE system identifier and each of the following:
   • [`M_GC_ACTION_DEVICE_KEY`](../../Reference/sys/MsysControl.md) to identify the group of cameras to receive the action command.
   • [`M_GC_ACTION_GROUP_MASK`](../../Reference/sys/MsysControl.md) to limit which cameras in the group should generate the action signal upon receiving the command. Set this to 0xFFFFFFFF if all cameras in the group should generate the action signal.
   • [`M_GC_ACTION_GROUP_KEY`](../../Reference/sys/MsysControl.md) to specify the key corresponding to the internal action signal on the camera.
   • [`M_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md) to specify the event that should cause the action command to be sent (for example, an auxiliary input signal).
4. Set up the GigE cameras to receive the action command, using successive calls to [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with their digitizer identifiers (allocated on the Aurora Imaging Library GigE system) and each of the following features: ActionDeviceKey, ActionGroupMask, and ActionGroupKey.
5. Set up the cameras to acquire the next frame upon receiving the action command. To do so, make successive calls to [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the camera's corresponding digitizer and each of the following features:
   • The TriggerSelector feature to select the type of trigger to configure (for example, FrameStart).
   • The ActionSelector feature to associate an internal action signal (for example, Action0) with the action group key.
   • The TriggerSource feature to set the trigger source of the frame start to the internal action signal (for example, Action0).
   • The TriggerMode feature to activate triggering the frame start upon the reception of the action command.
6. Register with Aurora Imaging Library the cameras associated with the action command. To do so, make successive calls to [`MsysControl`](../../Reference/sys/MsysControl.md) with the Aurora Imaging Library Concord PoE system, [`M_ADD_DESTINATION`](../../Reference/sys/MsysControl.md), and each of the Aurora Imaging Library digitizers. Aurora Imaging Library uses this information to know on which port to send the packet and to track and make use of the camera's action results.
7. Activate sending a Trigger-over-Ethernet packet as an action command upon the reception of the specified trigger event (for example, an auxiliary input signal). Use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TRIGGER_STATE`](../../Reference/sys/MsysControl.md) set to [`M_ENABLE`](../../Reference/sys/MsysControl.md).

The following code snippet demonstrate how to set up the ToE module on Zebra Concord PoE with ToE to send an action command, and how to set the camera to grab a frame upon receiving the action command:

> **Code example:** [boardspecific.Concord_PoE.ToE_action_setup](boardspecific.Concord_PoE.ToE_action_setup)

## Sending a Trigger-over-Ethernet packet as a GigE Vision software trigger

To configure the ToE module to send a Trigger-over-Ethernet packet as a GigE Vision software trigger, do the following:

1. Allocate an Aurora Imaging Library Concord PoE system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md).
2. Allocate an Aurora Imaging Library GigE Vision system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md). Then, allocate a digitizer on this system for each GigE Vision camera that should receive the GigE Vision software trigger, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).
3. Set up the ToE module to send a Trigger-over-Ethernet packet, as a GigE Vision software trigger, upon a specified event. To do so, make successive calls to [`MsysControl`](../../Reference/sys/MsysControl.md) with the Aurora Imaging Library Concord PoE system identifier and each of the following:
   • [`M_GC_TRIGGER_SOFTWARE0`](../../Reference/sys/MsysControl.md) + [`M_GC_TRIGGER_SELECTOR`](../../Reference/sys/MsysControl.md) to specify the type of trigger that should take place on the cameras upon receiving the Trigger-over-Ethernet packet (for example, frame start).
   • [`M_GC_TRIGGER_SOFTWARE0`](../../Reference/sys/MsysControl.md) + [`M_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md) to specify the event that should cause the Trigger-over-Ethernet packet (GigE Vision software trigger) to be sent (for example, an auxiliary input signal).
4. Set up the cameras to receive the Trigger-over-Ethernet packets, using successive calls to [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with their digitizer identifiers (allocated on the Aurora Imaging Library GigE Vision system) and each of the following features:
   • The TriggerSelector feature to select the type of trigger to configure (for example, FrameStart).
   • The TriggerSource feature to set the trigger source of the frame start to Software.
   • The TriggerMode feature to activate triggering the frame start upon the reception of the GigE Vision software trigger.
5. Specify the cameras that should receive the GigE Vision software trigger. To do so, make successive calls to [`MsysControl`](../../Reference/sys/MsysControl.md) with the Aurora Imaging Library Concord PoE system, [`M_ADD_DESTINATION`](../../Reference/sys/MsysControl.md), and each of the Aurora Imaging Library digitizers.
6. Activate sending a Trigger-over-Ethernet packet as a GigE Vision software trigger upon the reception of the specified trigger event (for example, an auxiliary input signal). Use MsysControl with [`M_TRIGGER_STATE`](../../Reference/sys/MsysControl.md) set to [`M_ENABLE`](../../Reference/sys/MsysControl.md).

The following code snippet demonstrate how to set up the ToE module on Zebra Concord PoE with ToE to send a GigE Vision software trigger, and how to set the camera to grab a frame upon receiving the GigE Vision software trigger:

> **Code example:** [boardspecific.Concord_PoE.ToE_software_setup](boardspecific.Concord_PoE.ToE_software_setup)
