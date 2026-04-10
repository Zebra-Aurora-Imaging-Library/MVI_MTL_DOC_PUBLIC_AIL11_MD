---
doctype: Reference
module: dig
function: MdigHookFunction
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dig / MdigHookFunction"
---

# MdigHookFunction

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Hook a function to a digitizer event.

## Syntax

```c
void MdigHookFunction(
    AIL_ID                    DigId,           //in
    AIL_INT                   HookType,        //in
    AIL_DIG_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified digitizer event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain more information about the event from within the hook-handler function using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md).

Note that functions hooked to an event execute on a distinct thread. This permits the functions to run concurrently from the operation that fired the event and from functions hooked to other events. Although there is a small queue to permit a certain amount of overlap, hooked functions should not take longer to execute than the period in which two of their associated events can occur.

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer.

### `HookType` *(in, AIL_INT)*

Specifies the digitizer event to which to hook the function. This parameter can be set to one of the following values:

*For specifying the digitizer event to which to hook the function*

| Value | Description |
| --- | --- |
| `M_CAMERA_LOCK` | Hooks the function to the event that occurs when the camera is locked. Use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_CAMERA_LOCKED`](../../Reference/dig/MdigInquire.md) to inquire if the camera is currently locked. |
| `M_CAMERA_PRESENT` | Hooks the function to the presence of the camera. Use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_CAMERA_PRESENT`](../../Reference/dig/MdigInquire.md) to inquire if the camera is currently present. |
| `M_EXPOSURE_END` | Hooks the function to the event that occurs at the end of each exposure period of your camera. |
| `M_EXPOSURE_START` | Hooks the function to the event that occurs at the start of each exposure period of your camera. |
| `M_FEATURE_CHANGE` | Hooks the function to the event that occurs when the value of a GenICam feature changes.

To enable an event to occur when the value of a specific feature changes, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with [`M_FEATURE_CHANGE_HOOK`](../../Reference/dig/MdigControlFeature.md) set to [`M_ENABLE`](../../Reference/dig/MdigControlFeature.md). Repeat for each feature for which you want to enable a feature change event. To enable an event to occur when the value of any feature changes, use [`M_FEATURE_CHANGE`](../../Reference/dig/MdigHookFunction.md) + [`M_ALL`](../../Reference/dig/MdigHookFunction.md). This setting is only available for hooking to an image buffer.

To retrieve the name of the feature that caused the event, use [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_GC_FEATURE_CHANGE_NAME`](../../Reference/dig/MdigGetHookInfo.md). |
| `M_FRAME_START` | Hooks the function to the event that occurs at the start of each frame of the incoming signal. |
| `M_GC_EVENT` | Hooks the function to the message sent from your GenICam SFNC-compliant camera at the time of the specified event. Your camera sends specific messages using a dedicated message channel to inform your computer of specific events (such as the start and end of specific events in the process through which your camera acquires an image). When dealing with the messages sent from your camera, the exact timing and order of the messages is specific to your camera's manufacturer.

Note that, if you don't add a combination constant to [`M_GC_EVENT`](../../Reference/dig/MdigHookFunction.md), the function is hooked to any GenICam event that has been enabled using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md). In addition, the message channel must be supported by your camera. |
| `M_GRAB_END` | Hooks the function to the event that occurs at the end of each grab.

When grabbing continuously, the [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md) event is fired only when the last frame is grabbed. To be notified at the end of each frame, use the [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md) event.

A function hooked to an [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md) event is called when all the data of the grab is transferred to the Host. This means that this hooked function can be called after the [`M_GRAB_START`](../../Reference/dig/MdigHookFunction.md) or [`M_GRAB_FRAME_START`](../../Reference/dig/MdigHookFunction.md) event of the next frame. |
| `M_GRAB_FRAME_END` | Hooks the function to the event that occurs at the end of grabbed frames. Note that, in this case, the grabbed data might not be completely transferred to the Host. |
| `M_GRAB_FRAME_START` | Hooks the function to the event that occurs at the start of grabbed frames. |
| `M_GRAB_LINE_END + n` | Hooks the function to the event that occurs when the data of the specified line (row) is in the buffer and ready to be processed, where _n_ is the specified line. For example, [`M_GRAB_LINE_END + 200`](../../Reference/dig/MdigHookFunction.md) would call the hook-handler function when the data of line number 200 is in the buffer.

You can call this function with this value up to 32 times during one grab. |
| `M_GRAB_LINE + n` | Hooks the function to the event that occurs when the specified line (row) is reached, where _n_ is the specified line. For example, [`M_GRAB_LINE + 200`](../../Reference/dig/MdigHookFunction.md) would call the hook-handler function when line number 200 is reached.

> **Note:** Note that the data of the specified line number might not be in the buffer.

You can call this function with this value up to 32 times during one grab. |
| `M_GRAB_START` | Hooks the function to the event that occurs at the start of each grab.

When grabbing continuously, the [`M_GRAB_START`](../../Reference/dig/MdigHookFunction.md) event is fired only when the first frame is grabbed. To be notified at the start of each frame, use the [`M_GRAB_FRAME_START`](../../Reference/dig/MdigHookFunction.md) event. |
| `M_IO_CHANGE` | Hooks the function to the event that occurs when an I/O signal changes in accordance with its specified interrupt activation mode, set using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigControl.md). Within the scope of the hook-handler function, it is prudent to verify that the appropriate I/O signal triggered the hook-handler function, using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_IO_INTERRUPT_SOURCE`](../../Reference/dig/MdigGetHookInfo.md).

If more than one input signal triggers an interrupt at the same time, the hook-handler function (or chain of hook-handler functions) is called for each input signal that generated an interrupt. As a result, any and all user-specified function(s) hooked using this hook type will be executed for each interrupt. |
| `M_ROTARY_ENCODER` | Hooks the function to the event that occurs when the rotary decoder's counter reaches its trigger position (specified using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/dig/MdigControl.md)). |
| `M_TIMER_END` | Hooks the function to the event that occurs when the active period of the specified timer ends (specified using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md)). |
| `M_TIMER_START` | Hooks the function to the event that occurs when the specified timer is triggered. |

*For specifying to hook to all feature change events*

| Value | Description |
| --- | --- |
| `M_GENTL_STREAM_NUMBER` | Specifies to display the GenTL stream configuration information. |
| `M_ALL` | Specifies to hook the specified function to all feature change events. |
| `M_GENTL_DEVICE` | Specifies to display the GenTL device configuration information. |
| `M_GENTL_REMOTE_DEVICE` | Specifies to display the GenTL remote device (camera) configuration information. |

*For specifying that the function should be unhooked*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/dig/MdigHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

*For GenICam camera events*

| Value | Description |
| --- | --- |
| `M_ACQUISITION_END` | Hooks the function to the event that occurs when acquisition ends on your camera. When the camera is in single frame acquisition mode, this event occurs at (or before) the end of each frame. When the camera is in multiframe or continuous acquisition mode, this event occurs at (or before) the end of multiple frames.

Note that, whether the acquisition end includes the acquisition transfer end is specific to your camera's manufacturer. |
| `M_ACQUISITION_ERROR` | Hooks the function to the event that occurs if an error is generated at some point while the camera exposes an image, stores it in an internal buffer, and transfers it from the camera to your computer. |
| `M_ACQUISITION_START` | Hooks the function to the event that occurs when acquisition begins on your camera. When the camera is in single frame acquisition mode, this event occurs at (or before) the beginning of each frame. When the camera is in multiframe or continuous acquisition mode, this event occurs at (or before) the beginning of multiple frames.

Note that, whether the acquisition start includes the first frame start and the exposure start is specific to your camera's manufacturer. |
| `M_ACQUISITION_TRANSFER_END` | Hooks the function to the event that occurs after transferring the image from your camera to your computer. When the camera is in single frame acquisition mode, this event occurs at the end of transferring each frame. When the camera is in multiframe or continuous acquisition mode, this event occurs at the end of transferring multiple frames.

Note that, whether the acquisition transfer end includes the last frame transfer end is specific to your camera's manufacturer. |
| `M_ACQUISITION_TRANSFER_START` | Hooks the function to the event that occurs before transferring the image from your camera to your computer. When the camera is in single frame acquisition mode, this event occurs at the beginning of transferring each frame. When the camera is in multiframe or continuous acquisition mode, this event occurs at the beginning of transferring multiple frames.

Note that, whether the acquisition transfer start includes the first frame transfer start is specific to your camera's manufacturer. |
| `M_ACQUISITION_TRIGGER` | Hooks the function to the event that occurs when acquisition is triggered on the camera. When the camera is in continuous acquisition mode, the [`M_ACQUISITION_TRIGGER`](../../Reference/dig/MdigHookFunction.md) event is fired only once, when your camera receives the initial acquisition trigger. |
| `M_COUNTER_END` | Hooks the function to the event that occurs when the specified camera counter stops.

Counters are unique to GenICam SFNC-cameras. To configure your GenICam SFNC-counters, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md). |
| `M_COUNTER_START` | Hooks the function to the event that occurs when the specified camera counter starts.

Counters are unique to GenICam SFNC-cameras. To configure your GenICam SFNC-counters, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md). |
| `M_EXPOSURE_END` | Hooks the function to the event that occurs at the end of each exposure period of your camera. |
| `M_EXPOSURE_START` | Hooks the function to the event that occurs at the start of each exposure period of your camera. |
| `M_FRAME_END` | Hooks the function to the event that occurs when the frame ends on your camera. Regardless of the acquisition mode of your camera, this event occurs at the end of each frame. |
| `M_FRAME_START` | Hooks the function to the event that occurs when the frame begins on your camera. Regardless of the acquisition mode of your camera, this event occurs at the beginning of each frame. |
| `M_FRAME_TRANSFER_END` | Hooks the function to the event that occurs after transferring the frame from your camera to your computer. Regardless of the acquisition mode of your camera, this event occurs at the end of each frame.

Instead of this event, it is more efficient to use [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md) with a hook-handler function. This reduces overhead both on the camera and the Host. |
| `M_FRAME_TRANSFER_START` | Hooks the function to the event that occurs before transferring the frame from your camera to your computer. Regardless of the acquisition mode of your camera, this event occurs at the beginning of each frame.

Instead of this event, it is more efficient to use [`M_GRAB_FRAME_START`](../../Reference/dig/MdigHookFunction.md). This reduces overhead both on the camera and the Host. |
| `M_FRAME_TRIGGER` | Hooks the function to the event that occurs for each image when the acquisition of each image is triggered on the camera. |
| `M_LINE_ANY_EDGE` | Hooks the function to the event that occurs when the specified I/O signal changes on the camera. |
| `M_LINE_FALLING_EDGE` | Hooks the function to the event that occurs on the falling edge of the specified I/O signal on the camera. |
| `M_LINE_RISING_EDGE` | Hooks the function to the event that occurs on the rising edge of the specified I/O signal on the camera. |
| `M_TIMER_END` | Hooks the function to the event that occurs when the specified timer ends. |
| `M_TIMER_START` | Hooks the function to the event that occurs when the specified timer is triggered. |

*For specifying the GenICam SFNC-specific counter or I/O signal to which to hook the function*

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies to hook to counter _n_ or I/O signal _n_.

When dealing with counters, _n_ is a value from 0 to 8.

When dealing with an I/O signal, _n_ is a value from 0 to 31. |

*For specifying the timer to which to hook the function*

| Value | Description |
| --- | --- |
| `M_TIMERn` | Specifies the timer to hook. |

*For specifying the rotary decoder to which to hook the function*

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies to hook to rotary decoder n, where _n_ is a number between 1 and 4.

Note that if you do not use this combination constant to specify a rotary decoder, the rotary decoder corresponding to 1+ the device number of the specified digitizer ([`DigId`](../../Reference/dig/MdigHookFunction.md)) will be used. For example, when the device number of the specified digitizer is [`M_DEV2`](../../Reference/dig/MdigAlloc.md), rotary decoder [`M_ROTARY_ENCODER3`](../../Reference/dig/MdigHookFunction.md) is used. |

### `HookHandlerPtr` *(in, AIL_DIG_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/dig/MdigHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.

Note that Aurora Imaging Library need not be grabbing the incoming signal for this event to occur. The event is only dependent on the presence of a camera.
