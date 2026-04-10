---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Using_Iris_GTX_strobe
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Using Iris GTX strobe"
---

# Controlling your lighting device indirectly

The auxiliary output signals of your Zebra Iris GTX can be linked to the exposure signal to control a lighting device that illuminates an object when the camera takes a picture.

*[Image: Iris_GTX-Strobe-Overview.png]*

To indirectly control a lighting device with the strobe timer signal, perform the following:

1. Connect a lighting controller to an auxiliary output signal.
2. Set the source of the auxiliary output signal to the strobe timer, using [`MsysControl`](../../Reference/sys/MsysControl.md) with[`M_IO_SOURCE`](../../Reference/sys/MsysControl.md)set to[`M_TIMER_STROBE`](../../Reference/sys/MsysControl.md).
3. To control the strobe timer, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER...`](../../Reference/dig/MdigControl.md) + [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md).

For more information about setting up a strobe timer, refer to [Timers and coordinating events](../../UserGuide/C56_IO_signals/S06_Timers_and_coordinating_events.md).

## Using either a grab trigger or a continuous grab to start your lighting device

Whenever using a lighting device, it will always start cold (that is, completely discharged) and take a certain amount of time to reach the specified intensity.

How you account for this cold period when setting up the strobe timer, depends on how you are grabbing:

- **Continuous (or almost continuous) grabs**. If you are grabbing a sequence of images with a negligible amount of delay between one image and the next, you only have to deal with the lighting device being cold at the very beginning of the grab sequence. In this case, you can trigger the strobe timer when the camera starts exposing its sensor (using [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_EXPOSURE_START`](../../Reference/dig/MdigControl.md)).
- **Triggered grabs**. If you are grabbing images based on a trigger that might arrive after a non-negligible delay, you will have to account for the lighting device recharging before reaching the specified intensity for each grab. In this case, you should trigger the strobe timer upon the grab trigger signal (using [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_GRAB_TRIGGER`](../../Reference/dig/MdigControl.md)).

Typically, the lighting device should be illuminated for the duration of the camera's exposure time. Unlike with other products, when using the grab trigger signal to trigger the strobe timer, the specified duration of the strobe timer's output signal (set using [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md) + [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md)) specifies how long the timer should remain active during the exposure period (and therefore how long the lighting device should remain active during the exposure period). The active period of the strobe timer's output will actually start after the specified delay, but the count for how long it should remain active only starts when the camera starts exposing its image sensor. Once counting begins, the signal will typically be active for the specified duration. If the duration of the strobe timer is greater than the exposure time, the strobe timer's output signal will end when the exposure time ends (set using [`M_EXPOSURE_TIME`](../../Reference/dig/MdigControl.md)).

*[Image: IrisGTR-Strobe-timing.png]*

This allows for hardware-specific variances to occur between when the grab trigger signal arrives and when the exposure of the camera's sensor actually starts.

The maximum amount of time it takes for your lighting device to arrive at the required intensity must be probed. This is because the amount of time is hardware-specific, and can vary greatly based on the type of lighting device, the period of the lighting device's reduced activity (caused by significant delays between grab triggers, or long processing delays during [`MdigProcess`](../../Reference/dig/MdigProcess.md)) or inactivity (such as, between individual grabs with [`MdigGrab`](../../Reference/dig/MdigGrab.md)).

When the exposure ends and the strobe timer's stops outputting an active signal, the lighting device begins to discharge and grow cold.

## Controlling your lighting controller and/or the LED lighting device

The following is an example of how to configure a grab using the camera's exposure and the strobe timer.

> **Code example:** [boardspecific.irisgtr.Zebra_irisgtr_using_the_Zebra_iris_gtr_strobe](boardspecific.irisgtr.zebra_irisgtr_using_the_zebra_iris_gtr_strobe)
