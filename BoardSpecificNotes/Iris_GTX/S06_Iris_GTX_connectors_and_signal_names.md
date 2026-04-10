---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Iris_GTX_connectors_and_signal_names
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Iris GTX connectors and signal names"
---

# Zebra Iris GTX connectors and signal names

This section serves as a reference to match Zebra Iris GTX's connectors and auxiliary signals with Aurora Imaging Library information, such as Aurora Imaging Library auxiliary signal numbers.

Auxiliary I/O signals can have one or more functionalities (for example, trigger input, user input or user output, depending on the signal). Their possible functionalities are listed in their description in the pinout table below.

Only those auxiliary signals that have matching Aurora Imaging Library information are included in this section. To set/inquire the routing, or state of an auxiliary signal, use [`MsysControl`](../../Reference/sys/MsysControl.md)/[`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_IO_...`](../../Reference/sys/MsysControl.md) control/inquire types, respectively. Whereas, the Aurora Imaging Library function that you should use to act upon an input signal or setup the source of an output signal depends on the functionality. For example, you can use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_USER_BIT_...`](../../Reference/sys/MsysControl.md) control types, [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER_...`](../../Reference/dig/MdigControl.md) control types, and you can use their corresponding [`MsysInquire`](../../Reference/sys/MsysInquire.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md) inquire types to inquire them.

To control a timer associated with the strobe signal, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER_...`](../../Reference/dig/MdigControl.md) + [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md) control types. To control the exposure timer, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_EXPOSURE...`](../../Reference/dig/MdigControl.md). To control the 8 general system timers available, use [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_TIMER_...`](../../Reference/sys/MsysControl.md) + [`M_TIMERn`](../../Reference/dig/MdigControl.md), where _n_ is a number from 1 to 8.

For a comprehensive list of all available input and output signals, refer to the board's installation and hardware reference manual.

## Board connectors

Only the following connector has auxiliary I/O signals with matching Aurora Imaging Library information.

| Connector Name | Connector Abbreviation | Image | Description |
| --- | --- | --- | --- |
| Digital I/O and power connector | M12-12P | *[Image: Iris_GTR_M12_PWR_IO_Connector_PinOut.png]* | This connector is an M12 12-pin female connector. It is used to transmit and receive digital auxiliary I/O signals and provide power to your Zebra Iris GTX. |

## Signal names and their matching Aurora Imaging Library constants

The table below lists the auxiliary I/O signals and the dedicated trigger signal with their associated Aurora Imaging Library information.

| Direction | Signal | Auxiliary | Description | Connector |
| --- | --- | --- | --- | --- |
| Out | `AUX_OPTOIND_OUT0` | `M_AUX_IO0` | Opto-isloated auxiliary signal 0 (output), which supports: user output, timer output, strobe timer output, and outputting the state of an I/O command register bit. | M12-12P (pin 10) |
| Out | `AUX_OPTOIND_OUT1` | `M_AUX_IO1` | Opto-isloated auxiliary signal 1 (output), which supports: user output, timer output, strobe timer output, and outputting the state of an I/O command register bit. | M12-12P (pin 12) |
| Out | `AUX_OPTOIND_OUT2` | `M_AUX_IO2` | Opto-isloated auxiliary signal 2 (output), which supports: user output, timer output, strobe timer output, and outputting the state of an I/O command register bit. | M12-12P (pin 3) |
| In | `AUX(TRIG)_OPTOIND_IN3` | `M_AUX_IO3` | Opto-isolated auxiliary signal 3 (input), which supports: User-defined signal 0 (input 1 of 4), acquisition trigger, timer trigger source, and rotary encoder bit source. | M12-12P (pin 5) |
| In | `AUX_OPTOIND_IN4` | `M_AUX_IO4` | Opto-isolated auxiliary signal 4 (input), which supports: User-defined signal 1 (input 2 of 4), acquisition trigger, timer trigger source, and rotary encoder bit source. | M12-12P (pin 9) |
| In | `AUX_OPTOIND_IN5` | `M_AUX_IO5` | Opto-isolated auxiliary signal 5 (input), which supports: User-defined signal 2 (input 3 of 4), acquisition trigger, timer trigger source, and rotary encoder bit source. | M12-12P (pin 7) |
| In | `AUX_OPTOIND_IN6` | `M_AUX_IO6` | Opto-isolated auxiliary signal 6 (input), which supports: User-defined signal 3 (input 4 of 4), acquisition trigger, timer trigger source, and rotary encoder bit source. | M12-12P (pin 8) |
