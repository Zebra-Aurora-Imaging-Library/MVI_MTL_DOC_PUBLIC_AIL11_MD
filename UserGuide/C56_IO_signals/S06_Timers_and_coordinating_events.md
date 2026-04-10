---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Timers_and_coordinating_events
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Timers and coordinating events"
---

# Timers and coordinating events

Some frame grabbers have on-board timers that can be used with I/O signals to coordinate events with external devices. For example, when grabbing an image (with camera reset), you can use an auxiliary input signal as a trigger source for a timer, and use an auxiliary output signal to route the timer's output to initiate and set the duration of the camera exposure. For information on which signals can be used to trigger a timer and which can be used to route a timer's output, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product.

## Timers in the advanced I/O engine

Some products also include a Zebra Advanced I/O engine, which is a series of hardware elements that allows you to communicate and coordinate events through I/O pins. The following diagram shows a functional diagram of the internal organization of a timer in the Zebra Advanced I/O engine.

*[Image: timer_aIO_setup_user_guide.png]*

The diagram below illustrates the logic of the timer's controller found in the advanced I/O engine.

*[Image: timer2.png]*

## Delay and duration

A timer can output a signal, typically with one pulse (one low and one high segment) per cycle. You can set-up a timer to output a signal with one pulse per cycle either using Aurora Imaging Library or in the DCF. For the few timers that can output a signal with one or two pulses per cycle, you can only set them up to output two pulses per cycle using the DCF. If you want to send the output of a timer to an auxiliary output signal, you should set up the timer before you call[`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_TIMERn`](../../Reference/dig/MdigControl.md).

In Aurora Imaging Library, to specify the length of the segments (one low and one high) of the timer output signal, use[`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md) and [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md). The timer delay ([`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md)) is the length of time before the active portion of the signal ([`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md)); the length of the active portion is often referred to as the timer duration.

Typically, the specified delay segment is low and the specified duration is high (active-high signal). However, you can invert the signal, such that the specified delay segment is high and the specified duration is low (active-low signal), using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_OUTPUT_INVERTER`](../../Reference/dig/MdigControl.md). The following illustration demonstrates two timers with the same settings, except that timer 1 has [`M_TIMER_OUTPUT_INVERTER`](../../Reference/dig/MdigControl.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md) and timer 2 has [`M_TIMER_OUTPUT_INVERTER`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). *[Image: timer_simple_inverter.png]*

## Timer modes: triggered or continuous

A timer can operate in one of two modes: triggered or continuous.

In triggered mode, an enabled timer waits to receive a trigger signal, before it begins to output a signal with the specified delay and duration. To specify the trigger signal, use[`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md). To specify when, upon receiving the trigger signal, to activate the timer to output a signal, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md); you can typically activate the timer upon a state transition (low-to-high or high-to-low) of the specified trigger signal.

If supported by your digitizer, you can also trigger a timer with a software trigger. To do so, set the trigger source to software using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_SOFTWARE`](../../Reference/dig/MdigControl.md). Then, to issue the software trigger for the timer, call [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOFTWARE`](../../Reference/dig/MdigControl.md)+ [`M_TIMERn`](../../Reference/dig/MdigControl.md).

In continuous mode, the timer starts to output a signal when it is enabled, and repeats the same cycle until the timer is disabled. You can use this type of signal, for example, to perform a task at fixed time intervals or to act as a clock. To set up your timer to output a signal continuously, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md)set to [`M_CONTINUOUS`](../../Reference/dig/MdigControl.md).

To enable or disable a timer, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_STATE`](../../Reference/dig/MdigControl.md).

## Steps to set up timers

The illustration below is a visual summary of the group of Aurora Imaging Library constants used to set up timers. *[Image: timer_setup_user_guide.png]*

|  |
||
| 1 | [`M_TIMER_STATE`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) | 5 | [`M_TIMER_OUTPUT_INVERTER`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) |
| 2 | [`M_TIMER_CLOCK_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) | 6 | [`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) |
| 3 | [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) | 7 | [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) |
| 4 | [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) + [`M_TIMER1`](../../Reference/dig/MdigControl.md) |

Perform the following steps to set up timer_n_ (for example, timer 1):

1. If the default is not appropriate, specify the clock source for the timer using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_CLOCK_SOURCE`](../../Reference/dig/MdigControl.md)+ [`M_TIMERn`](../../Reference/dig/MdigControl.md). For more information, see [Timer clock source](S06_Timers_and_coordinating_events.md).
2. Specify the mode in which you will be using the timer. To use the timer in triggered mode, specify the signal to use as the trigger, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_TIMERn`](../../Reference/dig/MdigControl.md). To use the timer in continous mode, set [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_TIMERn`](../../Reference/dig/MdigControl.md) to [`M_CONTINUOUS`](../../Reference/dig/MdigControl.md).
   Note that, the output of a timer cannot be used as a trigger source for itself.
3. Specify the timer delay (in nsec) using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md). Note that the delay must be greater than 0; otherwise, an error will be generated.
4. Specify the duration of the active portion (in nsec) using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md). Note that this value must be greater than 0; otherwise, an error will be generated.
5. In triggered mode, specify the trigger signal transition upon which to trigger the timer to begin transmitting, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) + [`M_TIMERn`](../../Reference/dig/MdigControl.md).
6. If you want the timer to output an active-low signal, invert the output of the timer using [`MdigControl`](../../Reference/dig/MdigControl.md) with[`M_TIMER_OUTPUT_INVERTER`](../../Reference/dig/MdigControl.md)+ [`M_TIMERn`](../../Reference/dig/MdigControl.md).
7. Finally, use[`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_STATE`](../../Reference/dig/MdigControl.md)+ [`M_TIMERn`](../../Reference/dig/MdigControl.md) to enable the timer. If in triggered mode, the timer will wait for a trigger signal before transmitting its output signal, with its specified delay and active time (duration).

In the following code snippet, an auxiliary input signal ( [`M_AUX_IO1`](../../Reference/dig/MdigControl.md)) is used as a trigger for timer 1; the timer output signal is sent to a connected camera.

> **Code example:** [userguide.io_signals.timers_and_coordinating_events01](userguide.io_signals.timers_and_coordinating_events01)

## Timer clock source

Although you set a timer in nanoseconds, a timer actually counts clock ticks; the numbers you enter are internally converted to clock ticks. The duration of each clock tick depends on the clock source. For some Zebra products that have timers, the clock that drives the specified timer is selectable using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_CLOCK_SOURCE`](../../Reference/dig/MdigControl.md). Typically, the default clock source is the pixel clock from the camera ([`M_PIXCLK`](../../Reference/dig/MdigControl.md)). The timer output delay and active portion ([`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md) and [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md), respectively) must each be at least equivalent to one clock tick and be set within the hardware limitations of the timer, otherwise an error will be generated. The maximum time lapse (in seconds) of a timer before it resets is the maximum number of ticks that the timer can count up to divided by the frequency (_f_) of the clock source. For example, if a timer resets after a maximum of 16,777,215 clock ticks, and the clock frequency is 85 MHz, the summed value of [`M_TIMER_DELAY`](../../Reference/dig/MdigControl.md) and [`M_TIMER_DURATION`](../../Reference/dig/MdigControl.md) cannot be greater than 197 msec (16,777,215 clock ticks/85 MHz). Note that in some cases, you can adjust the clock source frequency in the DCF, within a limited range, to permit the required number of clock ticks per timer output cycle.

To inquire the frequency of the clock that drives a specific timer, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_TIMER_CLOCK_FREQUENCY`](../../Reference/dig/MdigInquire.md) + [`M_TIMERn`](../../Reference/dig/MdigInquire.md).

If your hardware supports at least two timers and if the available timer clock sources are not appropriate for your application, you can create your own clock using a timer. For example, if the clock frequency of your timer is very high, the maximum number of clock ticks that your timer supports might not be enough to generate a signal with the required period (delay + duration). In the following illustration, the first signal has a frequency of 4 Hz and represents the default clock source of timer 1. The second signal is timer 1's output (in continuous mode) with a cycle (timer delay + active portion), equivalent to 4 cycles of the default clock source (1 Hz). If timer 1's output is used as the clock source for another timer (for example, timer 2), then timer 2's output can be up to 4 times longer than when using the default clock source, but will lose 4 times the granularity. Note that a timer output cannot be a clock source for itself.

*[Image: clock_frequency_user_guide.png]*

The following code snippet demonstrates how to use one timer as a clock source for another; timer 2 uses timer 1 as a clock source to generate a signal with a long period (32 seconds), which would not be possible if timer 2 were using its default clock source.

> **Code example:** [userguide.io_signals.timers_and_coordinating_events02](userguide.io_signals.timers_and_coordinating_events02)
