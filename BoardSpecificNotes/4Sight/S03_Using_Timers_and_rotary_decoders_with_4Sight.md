---
doctype: BoardSpecificNotes
part: ""
chapter: 4Sight
section: Using_Timers_and_rotary_decoders_with_4Sight
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / 4sight / Using Timers and rotary decoders with 4Sight"
---

# Using timers and rotary decoders with Zebra 4Sight EV7

Zebra 4Sight EV7 has 16 timers and 2 quadrature decoders that can be used to synchronize with external devices. Timers are used with I/O signals to coordinate events with external devices. Quadrature decoders are used to decode quadrature input (2-bit Gray code derived from two signals) from a quadrature encoder.

## Timers

The 16 timers on Zebra 4Sight EV7 can be configured using [`MsysControl`](../../Reference/sys/MsysControl.md). The timers are configured in a similar way to what is described in [Timers and coordinating events](../../UserGuide/C56_IO_signals/S06_Timers_and_coordinating_events.md). There are some differences, however, such as:

- A timer arming mechanism can be configured and enabled for each timer. If timer arming is enabled, the timer will ignore its trigger signal ([`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md)) until a signal transition, specified using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_ARM_ACTIVATION`](../../Reference/sys/MsysControl.md), occurs on the signal specified using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_ARM_SOURCE`](../../Reference/sys/MsysControl.md). To enable timer arming, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_ARM`](../../Reference/sys/MsysControl.md).
- The clock source can be specified for both the delay portion and duration portion of the timer output signal. For example, a rotary decoder output signal can be used to set the delay between the timer trigger and the active portion of the timer's output signal, and the unit's clock generator can be used to set the duration of the active portion of the timer's output signal.
- The timer can measure the duration of a pulse on a signal. To enable this functionality, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_USAGE`](../../Reference/sys/MsysControl.md) set to [`M_PULSE_MEASUREMENT`](../../Reference/sys/MsysControl.md). When a timer is set in this mode, the duration of pulses that occur on the timer's trigger source ([`M_TIMER_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md)) are measured with respect to the clock source for the active portion of the timer ([`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md)). To measure active-high pulses, set [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md) to [`M_LEVEL_HIGH`](../../Reference/sys/MsysControl.md); to measure active-low pulses, set [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/sys/MsysControl.md) to [`M_LEVEL_LOW`](../../Reference/sys/MsysControl.md). The measured value can be inquired using [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_TIMER_VALUE`](../../Reference/sys/MsysInquire.md). Note that when in this mode, the timer's output always remains low, and the values set using [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysControl.md), [`M_TIMER_DELAY`](../../Reference/sys/MsysControl.md), and [`M_TIMER_DURATION`](../../Reference/sys/MsysControl.md) are ignored.

## Quadrature decoders

The two quadrature decoders on Zebra 4Sight EV7 can be configured using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_ROTARY_ENCODER_...`](../../Reference/sys/MsysControl.md). The quadrature decoders are configured in a similar way to what is described in [Using quadrature input from a rotary encoder](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md). However, the following settings are unavailable when using Zebra 4Sight EV7: setting the direction of movement ([`M_ROTARY_ENCODER_DIRECTION`](../../Reference/dig/MdigControl.md)), setting a signal source that changes the rotary decoder counter value ([`M_ROTARY_ENCODER_FORCE_VALUE_SOURCE`](../../Reference/dig/MdigControl.md)), setting whether to enable the rotary decoder to store the counter value at the end of the last grab or frame interrupt ([`M_ROTARY_ENCODER_FRAME_END_READ`](../../Reference/dig/MdigControl.md)), and setting a multiplying factor to each increment/decrement of the rotary decoder counter ([`M_ROTARY_ENCODER_MULTIPLIER`](../../Reference/dig/MdigControl.md)).

There are two data latches associated with the rotary decoders on Zebra 4sight EV7. You can store the counter value of a rotary decoder in either of these data latches whenever a signal is received from a timer or auxiliary I/O, using [`MsysControl`](../../Reference/sys/MsysControl.md) with settings from the table [`DataLatch`](../../Reference/sys/MsysControl.md). You can retrieve the stored value within a hook function (hooked using [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md)) using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md)with [`M_SYS_DATA_LATCH_VALUE`](../../Reference/sys/MsysGetHookInfo.md).

> **Note:** Note that the function must be hooked to the same type of event that triggers the latch. For example, if the latch is triggered by a timer ([`M_SYS_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/sys/MsysControl.md)with[`M_TIMERn`](../../Reference/sys/MsysControl.md)), the hook function must be hooked to a timer event ([`M_TIMER_START`](../../Reference/sys/MsysHookFunction.md)or[`M_TIMER_END`](../../Reference/sys/MsysHookFunction.md)).
