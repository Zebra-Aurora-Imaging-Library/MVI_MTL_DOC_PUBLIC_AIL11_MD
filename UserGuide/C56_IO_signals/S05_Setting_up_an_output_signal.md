---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Setting_up_an_output_signal
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Setting up an output signal"
---

# Setting up an output signal

Before using an output signal, you have to specify its purpose; that is, you must establish the type of signal that will be routed to it. For example, you can route the output of a timer to a specific output signal, to control or synchronize with an external device. Most output signals can also output a user signal to start or stop an event; in this case, you essentially route the state (on or off) of a bit of a static-user-output register to the output signal. In addition, certain auxiliary input signals can be rerouted to certain output signals.

> **Note:** Depending on your hardware, you will need to use either the [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md) functions, or the [`MsysControl`](../../Reference/sys/MsysControl.md) and [`MsysInquire`](../../Reference/sys/MsysInquire.md) functions to use the output signals. For more information on which function to use, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product.

## Routing a source to an output signal

The source of an output signal (or a bidirectional signal set to output) is typically specified by the DCF. Otherwise, by default, the source is set to the signal's associated bit in a static-user-output register; for more information, see [User-bits and controlling the state of output signals](S05_Setting_up_an_output_signal.md).

Use [`MdigControl`](../../Reference/dig/MdigControl.md) or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md) to specify the source of an output signal. For information on which type of signal can be routed onto a specific pin/signal, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product. For example, the camera control (CC) output signals of Zebra Rapixo support timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. You can set the source of these CC signals to any one of these types of signals. For instance, using [`MdigControl`](../../Reference/dig/MdigControl.md), you can set the source of [`M_CC_IO1`](../../Reference/dig/MdigControl.md) to [`M_TIMER1`](../../Reference/dig/MdigControl.md)to control when a camera will expose its CCD, and set the source of [`M_CC_IO2`](../../Reference/dig/MdigControl.md) to [`M_TIMER2`](../../Reference/dig/MdigControl.md) to control a strobe light, such that the strobe light is only on when the camera's CCD is exposed.

You can verify the state (on or off) of a specific signal or of all signals using [`MdigInquire`](../../Reference/dig/MdigInquire.md)or [`MsysInquire`](../../Reference/sys/MsysInquire.md) (depending on your hardware), with [`M_IO_STATUS`](../../Reference/sys/MsysInquire.md) or [`M_IO_STATUS_ALL`](../../Reference/sys/MsysInquire.md), respectively. Note that the state of certain signals cannot be inquired (for example, most auxiliary output signals). When using [`M_IO_STATUS_ALL`](../../Reference/sys/MsysInquire.md) to inquire the status of all the I/O signals, if there are I/O signals that cannot be inquired, the bits representing those signals, in the bit-encoded value returned, are not necessarily valid; these bits should be ignored. For details on how to use [`M_IO_STATUS`](../../Reference/sys/MsysInquire.md) or [`M_IO_STATUS_ALL`](../../Reference/sys/MsysInquire.md) to inquire the state of a signal, refer to [Polling the state of input signals](S04_Using_user_input_signals.md).

Note that, if you set the signal mode (direction) before setting up the source of the signal, the signal will begin transmitting the default source. If the state of the default source for the signal is not appropriate, you must set up the source of the signal before setting its mode.

In the following code snippet, a bidirectional signal ([`M_AUX_IO9`](../../Reference/dig/MdigControl.md)), which can be transmitted in one of two formats (TTL or LVDS), is set.

> **Code example:** [userguide.io_signals.setting_up_an_output_signal01](userguide.io_signals.setting_up_an_output_signal01)

## User-bits and controlling the state of output signals

If you have results that you would like to use to control an external device (for example, to start or stop a process), you can do so by controlling the bits of a static-user-output register. These bits, often referred to as user-bits, can be used to control the state of output signals (or bidirectional signals set to output).

> **Note:** Note that most output signals can only output a specific bit. This bit is denoted in the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product. The Aurora Imaging Library I/O signal number is not necessarily the bit number.

For all hardware supporting I/O signals that can transmit user output, there is a static-user-output register for the auxiliary signals, referred to as the main static-user-output register. If your hardware has a Camera Link connector, there will be a 2-bit register whose bits can be routed to the camera control (CC) signals ([`M_USER_BIT_CC_IO`](../../Reference/dig/MdigControl.md)). If your hardware supports transport layer (TL) signals, there will be a 1-bit register whose bit can be routed to the TL trigger signal ([`M_USER_BIT_TL_TRIGGER0`](../../Reference/dig/MdigControl.md)). Only the bits in the static-user-output registers associated with the output signals are meaningful; the other bits are ignored.

To route the state of a user-bit to an output signal, use [`MdigControl`](../../Reference/dig/MdigControl.md)or [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md) and [`M_USER_BITn`](../../Reference/dig/MdigControl.md). To set the state of a specific bit in any static-user-output register, use [`MdigControl`](../../Reference/dig/MdigControl.md)or [`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_USER_BIT_STATE`](../../Reference/sys/MsysControl.md) and combine it with [`M_USER_BITn`](../../Reference/dig/MdigControl.md), [`M_USER_BIT_CC_IOn`](../../Reference/dig/MdigControl.md), or [`M_USER_BIT_TL_TRIGGER`](../../Reference/dig/MdigControl.md), depending on the bit to affect.

To simultaneously set or inquire the state of all the bits in a static-user-output register, use [`MdigControl`](../../Reference/dig/MdigControl.md)/[`MsysControl`](../../Reference/sys/MsysControl.md) or [`MdigInquire`](../../Reference/dig/MdigInquire.md)/[`MsysInquire`](../../Reference/sys/MsysInquire.md)(respectively), with [`M_USER_BIT_STATE_ALL`](../../Reference/sys/MsysControl.md); use this Aurora Imaging Library constant with a combination value to specify the static-user-output register to affect (for example, use [`M_USER_BIT_STATE_ALL`](../../Reference/dig/MdigControl.md) + [`M_USER_BIT_CC_IO`](../../Reference/dig/MdigControl.md) for the camera control static-user-output register). When setting [`M_USER_BIT_STATE_ALL`](../../Reference/sys/MsysControl.md), specify a control value in the form of a bit-encoded value that establishes the state of all the bits of the specified static-user-output register. For example, setting [`M_USER_BIT_STATE_ALL`](../../Reference/sys/MsysControl.md) to 0x44 (hexadecimal equivalent to 101100 in binary), sets [`M_USER_BIT2`](../../Reference/dig/MdigControl.md), [`M_USER_BIT3`](../../Reference/dig/MdigControl.md), and [`M_USER_BIT5`](../../Reference/dig/MdigControl.md) to a value of 1, and sets all other bits to 0. Note that, when inquiring the state of all the bits, the return value is also a bit-encoded value that specifies the current setting of each bit, where [`M_USER_BIT0`](../../Reference/dig/MdigControl.md) is represented by the least-significant bit of the bit-encoded value.

The following code snippet demonstrates how to route the state of two user-bits ([`M_USER_BIT5`](../../Reference/dig/MdigControl.md) and [`M_USER_BIT0`](../../Reference/dig/MdigControl.md)) to their corresponding auxiliary I/O signal ([`M_AUX_IO3`](../../Reference/dig/MdigControl.md) and [`M_AUX_IO12`](../../Reference/dig/MdigControl.md)). Recall that the number of a user-bit and its corresponding auxiliary I/O signal are not necessarily the same; it depends on the board (in this snippet, the numbers of Zebra Rapixo CL were used). Initially, the state of the two user-bits are set to 0, so a low signal is outputted. The snippet then shows how to change the two bits and their associated auxiliary I/O signals simultaneously, without affecting the other user-bits (and their associated signals).

> **Code example:** [userguide.io_signals.setting_up_an_output_signal02](userguide.io_signals.setting_up_an_output_signal02)
