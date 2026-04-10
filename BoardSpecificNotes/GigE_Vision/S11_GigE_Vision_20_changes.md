---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: GigE_Vision_20_changes
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / GigE Vision 20 changes"
---

# Triggering simultaneous actions in multiple GigE Vision cameras

Triggering simultaneous actions across multiple GigE Vision devices (such as, cameras) on your network is a difficult and time-consuming process that, when dealing with unicast packets across a network, involves unavoidable delays and difficulties. The GigE Vision specification allows for synchronizing actions using broadcast packets that can trigger actions (such as, acquisitions) on all the cameras programmed to receive these broadcast packets; to trigger actions, these packets contain an action command.

*[Image: bsn_gige_action_first.png]*

## Introducing action commands

A broadcast packet that contains an action command contains the information to identify the cameras, on your network, that should receive and accept the broadcast packet and the information to trigger an associated action on the cameras. To configure and send out the broadcast packet from your Aurora Imaging Library application, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_ACTION...`](../../Reference/sys/MsysControl.md) + [`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md) where _n_ is the number of the action command in Aurora Imaging Library.

Your GigE Vision cameras must be configured to receive and act upon an action command sent by your Aurora Imaging Library application. To receive the action command, the action device key of the camera (`ActionDeviceKey` feature) must match that of the action command. To act upon the action command, you must select the internal action signal that should be generated upon reception of the command. The internal action signal will then trigger any associated action (such as, triggering acquisition). To associate an action (feature) with the reception of an action command, set the internal action signal as the trigger source of the action using either [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) or Aurora Imaging Capture Works' Feature Browser, with the associated SNFC trigger source feature name (for example, setting the`TriggerSource` feature of the Acquisition control category to`Action_n_`, where _n_ is the action number).

> **Note:** Note that all references in this section to GenICam SFNC feature names assume your camera is compliant with GenICam SFNC 2.0; check your camera's documentation or a listing of your camera's device description file to establish whether your camera uses a feature with a different name (or range of values) than those mentioned in this section.

The following table provides a quick overview of the steps required to use Aurora Imaging Library to send action commands to your GigE Vision cameras.

| What to do | In your Aurora Imaging Library application (using [`MsysControl`](../../Reference/sys/MsysControl.md) and adding [`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md) to the control type) | On each of your GigE Vision cameras | Definition |
| --- | --- | --- | --- |
|  |
| 1. Specify the group of cameras on which to perform the action. | Set your Aurora Imaging Library device key (using[`M_GC_ACTION_DEVICE_KEY`](../../Reference/sys/MsysControl.md)). | Set each camera's action device key (using the `ActionDeviceKey` feature). | The camera's action device key is a 32-bit integer value that identifies the group of cameras on which the synchronized actions should occur. The Aurora Imaging Library device key and the camera's action device key must match; otherwise, the camera ignores the action command. |
| 2. Select the internal action signal to generate upon reception of a specific action command. | Select the internal action signal that you want to associate with an action group key (using the `ActionSelector` feature). | The action selector establishes which action signal to generate. |
| 3. Specify whether to include or exclude cameras already in the group. | Specify your action group mask (using [`M_GC_ACTION_GROUP_MASK`](../../Reference/sys/MsysControl.md)). | For the selected action signal, set the action group mask (using the`ActionGroupMask`feature). | The action group mask is a bit-encoded 32-bit integer value that allows you to exclude (mask out) cameras so they will not generate the selected action signal. See [Masking out one or more cameras](S11_GigE_Vision_20_changes.md). |
| 4. Specify the key corresponding to the action signal on the camera. | Set your Aurora Imaging Library action group key (using[`M_GC_ACTION_GROUP_KEY`](../../Reference/sys/MsysControl.md)). | For the selected action signal, set the camera's action group key (using the`ActionGroupKey`feature). | The action group key is a 32-bit integer value that identifies the Aurora Imaging Library action command to your GigE Vision camera. It associates the action command with the selected action signal. |
| 5. Set the internal action signal as the trigger source of an action(s) on your camera. | Set the trigger source of an action (feature) to the internal action signal (for example, set the `TriggerSource`feature of the acquisition control category to `Action0`). | The internal action signal is generated when your camera receives the associated Aurora Imaging Library action command. For something to happen (such as image acquisition), set the trigger source of an action (feature) on your camera to the internal action signal. |
| 6. Specify to perform the action at a predetermined time (scheduling) using a time offset. | Set your Aurora Imaging Library action time (using [`M_GC_ACTION_TIME`](../../Reference/sys/MsysControl.md)). | Set the camera to accept scheduled action commands (using the`GevIEEE1588`feature). | The scheduled action command uses a time offset that identifies when the action should occur on the camera. This optional value specifies when the action command should execute on your camera. See [Scheduling time](S11_GigE_Vision_20_changes.md). |
| 7. Add your GigE Vision camera(s) to your action command, in Aurora Imaging Library. | Identify the cameras to associate with the action command by specifying their Aurora Imaging Library digitizer identifier (using [`M_GC_ACTION_ADD_DEVICE`](../../Reference/sys/MsysControl.md)). | For Aurora Imaging Library to track and make use of the camera's action results, you must also add each camera explicitly to the action command using [`M_GC_ACTION_ADD_DEVICE`](../../Reference/sys/MsysControl.md) + [`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md). This is described later in [Acknowledgment of reception of an action command](S11_GigE_Vision_20_changes.md). |
| 8. Send the action command from your Aurora Imaging Library application to your GigE Vision camera. | Use [`M_GC_ACTION_EXECUTE`](../../Reference/sys/MsysControl.md). | Aurora Imaging Library sends the broadcast packet containing the action command. Typically, this control type is specified during the time-critical loop of your application. |

The following image illustrates the steps (above). The numbers refer to the steps on which they are set.

*[Image: bsn_gige_action_example.png]*

## Establish if your camera supports action commands

To send action commands to your GigE Vision cameras, each camera must support the following:

- **Action commands.** To verify that your camera supports action commands, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CONTROL_PROTOCOL_CAPABILITY`](../../Reference/dig/MdigInquire.md). It should return [`M_GC_ACTION_SUPPORT`](../../Reference/dig/MdigInquire.md), [`M_GC_UNCONDITIONAL_ACTION_SUPPORT`](../../Reference/dig/MdigInquire.md), or [`M_GC_SCHEDULED_ACTION_SUPPORT`](../../Reference/dig/MdigInquire.md).
- **The feature on your camera supports action signals as a trigger source.** The feature that you need to trigger (such as, acquisition) must support action signals as a trigger source. To verify this, refer to your camera's device description file. For example, the`TriggerSource` feature of the Acquisition control category should support the internal signal named`Action`_n_, where _n_ is the internal action signal number. Note that some cameras might only support a single action signal, while others might support several. The action command number ([`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md)) is an index specific to Aurora Imaging Library and does not necessarily match the number of the internal action signal set as the trigger source for the action on your camera. The association between Aurora Imaging Library and your camera's specific action signal is made using the action group key.

## Acknowledgment of reception of an action command

When an action command is sent to a camera, the camera will respond with an acknowledgment of reception (a special type of packet). This packet signifies that your camera successfully received Aurora Imaging Library's action command. When an action command is sent to several cameras, Aurora Imaging Library expects the number of acknowledgments to equal the number of cameras added to the action command using [`M_GC_ACTION_ADD_DEVICE`](../../Reference/sys/MsysControl.md)(minus those removed using [`M_GC_ACTION_REMOVE_DEVICE`](../../Reference/sys/MsysControl.md)).

For example, on a network of four cameras all added using [`M_GC_ACTION_ADD_DEVICE`](../../Reference/sys/MsysControl.md) and with the same action device key, if one action command is sent but is only received by two cameras, Aurora Imaging Library expects the number of acknowledgments to be 4, but only 2 would be returned. This generates an Aurora Imaging Library error.

## Masking out one or more cameras

In the case where you need one (or more) camera(s) to temporarily ignore an action command, you can mask out the action command by changing the action group mask of the command either in Aurora Imaging Library or on the camera. For the camera to accept and generate an internal action signal, the action group mask of the action command in Aurora Imaging Library and that on the camera, when combined in a bitwise AND operation, must result in a non-zero value. If the result is a zero, the camera ignores the action command.

| Action command 1 | The action group mask's 32-bit value |
| --- | --- |
| Action group mask in Aurora Imaging Library | 00000000 00000000 00000000 00000001 |
| Action group mask on camera 1 | 00000000 00000000 00000000 00000001 |
| Result: Camera 1 triggers the feature | 00000000 00000000 00000000 00000001 |

The best way to use the action group mask is to treat each bit as an identifier of the camera on which the action command should be accepted. Set the action group mask of all action commands on each camera to a value where all bits are set to 0 except the identifier bit. Then, in Aurora Imaging Library, set the action group mask such that only the bits identifying the cameras to receive the command are set to 1. For example:

| Action command 1 | The action group mask's 32-bit value |
| --- | --- |
| Action group mask in Aurora Imaging Library | 00000000 00000000 00000000 00001110 |
| Action group mask on camera 1 | 00000000 00000000 00000000 00000001 |
| Action group mask on camera 2 | 00000000 00000000 00000000 00000010 |
| Action group mask on camera 3 | 00000000 00000000 00000000 00000100 |
| Action group mask on camera 4 | 00000000 00000000 00000000 00001000 |
| Action group mask on camera 5 Only camera 2, 3, and 4 trigger the action. | 00000000 00000000 00000000 00010000 |

For a further example, refer to [Example of using group masks to ignore an action command on one camera](S11_GigE_Vision_20_changes.md).

When masking out one or more action commands on one or more cameras, you must also modify the number of acknowledgments that Aurora Imaging Library expects to receive, using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_ACTION_ACKNOWLEDGE_NUMBER`](../../Reference/sys/MsysControl.md)+[`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md), where _n_ is the number of the action command in Aurora Imaging Library.

## Scheduling time

To schedule an action command so that it will execute at the same time across multiple cameras, the following features must be available on your camera:

- **Scheduled actions**. To verify that scheduled actions are available on your camera, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CONTROL_PROTOCOL_CAPABILITY`](../../Reference/dig/MdigInquire.md). It should return [`M_GC_SCHEDULED_ACTION_SUPPORT`](../../Reference/dig/MdigInquire.md).
- **IEEE 1588**. IEEE 1588 is a standard that defines the use of PTP (the precision time protocol). PTP synchronizes clocks across multiple network devices (such as, multiple GigE Vision cameras). To verify that IEEE 1588 is available on your camera, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CONTROL_PROTOCOL_CAPABILITY`](../../Reference/dig/MdigInquire.md). It should return [`M_GC_IEEE_1588_SUPPORT`](../../Reference/dig/MdigInquire.md).

If the above are available, you must then enable IEEE 1588 on each of your cameras, for example, using `GevIEEE1588 ` with Aurora Imaging Intellicam (or using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)).

Once enabled, IEEE 1588 will synchronize the clocks of all of your cameras. Note that this can take some time. When the synchronization is complete, querying the `Gev1588status` feature for each camera should return one camera as timeTransmitter, and all the rest as timeReceivers. You can then set up the action command in your Aurora Imaging Library application to generate the internal action signal at a specific time and know that the action will occur at the exact moment on all cameras receiving the command at the required time. With all your cameras clocks synchronized, you only have to account for the longest network delay possible between sending a packet to your camera, and the camera receiving it, when selecting a time.

> **Note:** Note that, when you enable IEEE1588, the timestamp frequency of the camera will change. Therefore, your application should read the timestamp after you enable IEEE1588. In addition, all the 1588 devices must be connected to the same switch on your network. The switch must have a multicast service and it must be enabled to use 1588.

To select a time and provide that time to the action command, use[`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_GC_ACTION_TIME`](../../Reference/sys/MsysControl.md). To determine the required time, perform the following:

1. After 1588 is enabled, read the timestamp from any target camera using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CAMERA_TIME_STAMP`](../../Reference/dig/MdigInquire.md).
   If you want your action to occur relative to the timing of another action (for example, an exposure start), especially when the action should reoccur periodically, read the timestamp from within the callback function hooked to the other action. In this case, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_GC_CAMERA_TIME_STAMP`](../../Reference/dig/MdigInquire.md) to read the time at which the event occurred on the camera.
2. Determine the longest possible transmission and processing latency of sending a packet from your computer to your camera. Add this amount of time (in seconds) to the timestamp retrieved previously.
   Experimentation is required to accurately determine how long it takes for your action command to reach the camera and the action to occur. If the action command reaches the camera after the scheduled time, an error is returned, but the action will still occur.

## Examples of action commands

This subsection details a set of examples showing how action commands trigger actions (features) on the camera, based on the configuration of the action command in your Aurora Imaging Library application.

### Example of sending a single action command

In the following example, three GigE Vision cameras on the network are configured to receive action commands from an Aurora Imaging Library application. When you send an action command (using [`M_GC_ACTION_EXECUTE`](../../Reference/sys/MsysControl.md)+[`M_GC_ACTION0`](../../Reference/sys/MsysControl.md)), Aurora Imaging Library sends a broadcast packet containing the action command.

In this case, action command 0 generates internal action signal 1 so it triggers the feature whose trigger source is set to action 1 on Cameras 1 and 2. Camera 3 ignores action command 0 because Camera 3 has a different action device key than the other cameras. This configuration is designed so that Camera 3 will always ignore action command 0.

*[Image: bsn_gige_action_sending_one.png]*

### Example of sending multiple action commands

In the following example, three GigE Vision cameras on the network are set up with the same action device keys (Cameras 1, 2, and 4). When you send the two action commands to your cameras (using [`M_GC_ACTION_EXECUTE`](../../Reference/sys/MsysControl.md)+[`M_GC_ACTIONn`](../../Reference/sys/MsysControl.md), where _n_ is the number of the action command in Aurora Imaging Library), Aurora Imaging Library sends two broadcast packets (one per action command). The network copies the packets to all four GigE Vision cameras. On the cameras that accept the broadcast packets, the camera generates an internal action signal (either action signal 1 or action signal 2) that will trigger one or more features (depending on their trigger source).

In this case, action command 0 triggers the feature whose trigger source is set to action signal 1 on Cameras 1 and 2. Camera 4 ignores action command 0 because no action signal is associated with the action group of action command 0. To allow Aurora Imaging Library's action command 0 to trigger a feature on Camera 4, the action group key (0x24) would have to be associated with an action signal on Camera 4.

Action command 1 triggers the feature whose trigger source is set to action signal 2 only on Camera 3, since it is the only camera with both an action device key and action group key associated with action command 1. To allow Aurora Imaging Library's action command 1 to trigger a feature on Camera 4, the action device key of Camera 4 would have to be set to 0x45670123.

*[Image: bsn_gige_action_sending_many.png]*

### Example of using group masks to ignore an action command on one camera

In the following example, an action group mask is used to have Camera 2 temporarily ignore action command 0. It sets the group mask in Aurora Imaging Library's action command 0 so that the bit representing Camera 2 is 0. When ANDed in a bitwise fashion with the camera's group mask for action signal 0, the result is a 0 instead of a 1.

| Action command 0 | The action group mask's 32-bit value |
| --- | --- |
| Action group mask in Aurora Imaging Library | 00000000 00000000 00000000 00000001 |
| Action group mask on Camera 2 | 00000000 00000000 00000000 00000010 |
| Result: Camera 2 ignores the action. | 00000000 00000000 00000000 00000000 |

This forces Camera 2 to ignore action command 0, even though the device key and group key match between action command 0 in your Aurora Imaging Library application and the associated action signal on your camera.

*[Image: bsn_gige_action_masking_out.png]*

## Example distributed with Aurora Imaging Library

The Aurora Imaging Library hardware system specific example _ActionTrigger.CPP_ illustrates how to send an action command to multiple cameras, configuring both the action command in Aurora Imaging Library and on each of the GigE Vision cameras on your network. In addition, the example tries to use scheduled actions, testing first that IEEE 1588 (Precision Time Protocol) is available and then enabling it on the cameras. If IEEE 1588 is not available, the example reverts to sending the action command without scheduling. To run this and other examples, use Aurora Imaging Example Launcher.
