---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Steps_to_use_IO_signals
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Steps to use IO signals"
---

# Steps to use I/O signals

The following steps provide a typical methodology to use I/O signals. Note that, depending on your hardware, you will need to use either the [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md) functions, or the [`MsysControl`](../../Reference/sys/MsysControl.md) and [`MsysInquire`](../../Reference/sys/MsysInquire.md) functions, to control or inquire about the I/O signal. For more information on which function to use, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product.

1. If the I/O signal can be transmitted using one of several formats and the default setting is not appropriate, specify the format using [`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_FORMAT`](../../Reference/dig/MdigControl.md).
2. For an output signal, set-up the source of the signal. For example, if you want to route the output of a timer to an I/O signal, you can use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_...`](../../Reference/dig/MdigControl.md) control types to set-up the timer.
3. Configure the I/O signal according to its purpose.
   For output signals (or bidirectional signals that you will set to output mode), specify the type of signal to be routed to it, using [`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md).
   For input signals (or bidirectional signals that you will set to input mode), specify the input signal ([`M_AUX_IOn`](../../Reference/dig/MdigControl.md)) as the source of the required event. For example, specify auxiliary input signal [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) as the signal to use to trigger a grab operation ([`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER_SOURCE`](../../Reference/dig/MdigControl.md) set to [`M_AUX_IO5`](../../Reference/dig/MdigControl.md)). You can also poll the state of an input signal using [`MdigInquire`](../../Reference/dig/MdigInquire.md) or [`MsysInquire`](../../Reference/sys/MsysInquire.md)with [`M_IO_STATUS`](../../Reference/sys/MsysInquire.md), and act upon it.
   > **Note:** Note that step #2 does not necessarily need to be performed before step #3; however, it ensures that you know what is being routed on the signal initially.
4. If the I/O signal is a bidirectional signal, specify its mode (direction) using [`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_MODE`](../../Reference/dig/MdigControl.md) set to either [`M_INPUT`](../../Reference/dig/MdigControl.md) or [`M_OUTPUT`](../../Reference/dig/MdigControl.md).
   Note that in some cases, the bidirectional signal is full-duplex, which can receive input signals and transmit output signals simultaneously (for example, transport layer (TL) trigger signals). For full-duplex signals, you typically do not have to set the mode of the signal unless you are inquiring its state; that is, call [`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_MODE`](../../Reference/dig/MdigControl.md), followed by a call to [`MdigInquire`](../../Reference/dig/MdigInquire.md) or [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_IO_STATUS`](../../Reference/dig/MdigInquire.md).

## Using Aurora Imaging Intellicam's Feature Browser

You can test settings for I/O signals and other hardware-specific features interactively in real-time with Aurora Imaging Intellicam's Feature Browser. That is, you can use this interactive user interface to set any of the [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MsysControl`](../../Reference/sys/MsysControl.md) (and [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md)) control types available for your Zebra product, and then inquire the results immediately. For example, you can fine-tune the duration of a timer output signal's delay and active portion by adjusting the values in the Feature Browser, and verify the result in real-time; otherwise, you would have to re-compile your application program each time you made an adjustment to see the effects of the change.

To help build your application program, Intellicam's Feature Browser provides code snippets with the Aurora Imaging Library functions and Aurora Imaging Library constants associated with your selected settings. You can copy and paste these code snippets into your application's code.

For information on how to use Intellicam's Feature Browser, refer to the Intellicam Help. To verify if you can save the settings you selected using the Feature Browser directly to the DCF for your camera, refer to the Using your Zebra product with Aurora Imaging Library section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product.
