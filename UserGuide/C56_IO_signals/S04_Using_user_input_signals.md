---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Using_user_input_signals
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Using user input signals"
---

# Using input signals

You can route any external signal that meets electrical specifications, to an auxiliary input signal (or a bidirectional I/O signal set to input); your application can then use it for a specific purpose, as long as the functionality is supported by the auxiliary input signal. For example, if the input signal supports user input (user-defined input), you can poll (inquire) the state of the input signal to monitor or react to its state. Alternatively, if your hardware supports it, you can enable the generation of an interrupt when the user input signal changes state, and then hook a function to it. You can also use an input signal as a trigger for some operation or performing image acquisition. Some Zebra boards can also decode an input from a rotary encoder, which you can use to trigger an event; for more information on rotary decoders, refer to [Using quadrature input from a rotary encoder](S07_Using_quadrature_input_from_a_rotary_encoder.md).

> **Note:** Depending on your hardware, you will need to use either the [`Mdig...`](../../Reference/dig/MdigControl.md) functions or the [`Msys...`](../../Reference/sys/MsysControl.md) functions to use the input signals. For more information on which functions to use and which functionalities are supported by the signals for your hardware, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product.

## Polling the state of input signals

For input signals (or bidirectional I/O signals set to input) which support user input, you can use a continuous loop to poll (inquire) the input signal to determine if it is high or low, and then, react to its state or a change in its state. To inquire the current state of one specific I/O signal, use either [`MdigInquire`](../../Reference/dig/MdigInquire.md) or [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_IO_STATUS`](../../Reference/sys/MsysInquire.md).

To inquire the current state of all the I/O signals, use either [`MdigInquire`](../../Reference/dig/MdigInquire.md) or [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_IO_STATUS_ALL`](../../Reference/sys/MsysInquire.md). [`M_IO_STATUS_ALL`](../../Reference/sys/MsysInquire.md) returns a bit-encoded value that specifies the current state of each of the input and output signals of your hardware (if their state is inquirable), whereby [`M_AUX_IO0`](../../Reference/dig/MdigControl.md) is represented by the least-significant bit of the bit-encoded value. For example, inquiring [`M_IO_STATUS_ALL`](../../Reference/dig/MdigInquire.md) for auxiliary signals could return 0x44, equivalent to 101100 in binary, which would mean that I/O signals associated with Aurora Imaging Library constants [`M_AUX_IO2`](../../Reference/dig/MdigControl.md), [`M_AUX_IO3`](../../Reference/dig/MdigControl.md), and [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) are high, and all other I/O signals are low.

> **Note:** Note that the state of certain signals cannot be inquired (for example, signals exclusively dedicated for triggers). When using [`M_IO_STATUS_ALL`](../../Reference/sys/MsysInquire.md) to inquire the status of all the I/O signals, if there are I/O signals that cannot be inquired, the bits representing those signals, in the bit-encoded value returned, are not necessarily valid; these bits should be ignored.

## Using interrupts with input signals

If supported in hardware, instead of polling an input signal (that supports user input) to check its state and react to it, you can have a signal generate an interrupt when it changes state. You can then hook a function to this event to perform some operation or action upon this event.

To react to a change in the state of a specified input signal, based on an interrupt, perform the following steps:

1. Specify whether the input signal should generate an interrupt upon a rising edge, falling edge, or both, using either [`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/dig/MdigControl.md) + [`M_AUX_IOn`](../../Reference/dig/MdigControl.md).
2. Hook a function to the input signal's change of state, using either [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) or [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) with [`M_IO_CHANGE`](../../Reference/sys/MsysHookFunction.md).
   Within the scope of the hook function itself, inquire which signal(s) generated the interrupt, using either [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) or [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with[`M_IO_INTERRUPT_SOURCE`](../../Reference/sys/MsysGetHookInfo.md), and then, execute according to the result of the inquiry.
3. If the signal to be used is a bidirectional signal, set the signal to input mode, using either[`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_MODE`](../../Reference/dig/MdigControl.md) + [`M_AUX_IOn`](../../Reference/dig/MdigControl.md)set to [`M_INPUT`](../../Reference/dig/MdigControl.md).
4. Enable the interrupt state for the input signal, using [`M_IO_INTERRUPT_STATE`](../../Reference/dig/MdigControl.md) + [`M_AUX_IOn`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md).

The following code snippet demonstates how to declare your hook handler function and to use an auxiliary input signal ([`M_AUX_IO8`](../../Reference/dig/MdigControl.md)) to generate an interrupt when its state changes from low to high.

> **Code example:** [userguide.io_signals.using_user_input_signals01](userguide.io_signals.using_user_input_signals01)

## Using input signals as triggers

If supported in hardware, you can use an input signal (or a bidirectional signal set to input) as a trigger source. For example, you can specify to use a specific input signal to trigger a timer using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_AUX_IOn`](../../Reference/dig/MdigControl.md), or you can use a specific input signal to trigger image acquisition using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_AUX_IOn`](../../Reference/dig/MdigControl.md). You must establish how to use the signal to trigger, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) or [`M_GRAB_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md); you can specify whether to generate a trigger upon a signal state transition or to trigger continuously according to signal polarity (high or low state). To use the trigger signal, you must first enable the timer or enable grabbing upon a trigger, so that it is ready to receive a trigger signal; to do so, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_STATE`](../../Reference/dig/MdigControl.md) or [`M_GRAB_TRIGGER_STATE`](../../Reference/dig/MdigControl.md), respectively.

For information on this topic, see [Grabbing with triggers](../C27_Grabbing_with_your_digitizer/S11_Grabbing_with_triggers_and_exposures.md).

The following code snippet demonstrates how to use auxiliary input signal 1 ([`M_AUX_IO1`](../../Reference/dig/MdigControl.md)) to trigger the grab of an image sent from a connected camera.

> **Code example:** [userguide.io_signals.using_user_input_signals02](userguide.io_signals.using_user_input_signals02)
