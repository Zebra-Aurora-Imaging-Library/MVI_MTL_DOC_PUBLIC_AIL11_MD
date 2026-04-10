---
doctype: BoardSpecificNotes
part: ""
chapter: Concord_PoE
section: Concord_POE_connectors_and_signal_names
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / Concord_PoE / Concord POE connectors and signal names"
---

# Zebra Concord PoE with ToE connectors and signal names

This section serves as a reference to match Zebra Concord PoE with ToE's connectors and auxiliary signals with Aurora Imaging Library information, such as Aurora Imaging Library auxiliary signal numbers. To set/inquire all the settings for this board's auxiliary signals (for example, signal routing and timer settings), use [`MsysControl`](../../Reference/sys/MsysControl.md)/[`MsysInquire`](../../Reference/sys/MsysInquire.md), respectively.

Auxiliary I/O signals can have one or more functionalities (for example, outputting the state of an I/O command register bit, timer output, or user output, depending on the signal). Their possible functionalities are described in their description in the pinout table below. Note that, there is no limit to the number of events that can be triggered simultaneously using the auxiliary input signals, nor is there a restriction on which auxiliary signal can be used to trigger the event.

Only those auxiliary signals that have matching Aurora Imaging Library information are included in this section. For information on internal connectors and a comprehensive list of all available input and output signals, refer to the board's installation and hardware reference manual.

## Board connectors

The Zebra Concord PoE with ToE board provides an external auxiliary I/O connector and Gigabit Ethernet connectors.

*[Image: Concord_POE_with_TOE_connectors.png]*

Only the following connectors have auxiliary signals with matching Aurora Imaging Library information.

| Connector Name | Connector Abbreviation | Image | Description |
| --- | --- | --- | --- |
| External auxiliary I/O connector | HD-15 | *[Image: db15M_rev1.PNG]* | The external auxiliary I/O connectors is used to transmit/receive auxiliary signals from the Advanced I/O engine. |

## Signal names and their matching Aurora Imaging Library constants

The table below lists the auxiliary signals with their associated Aurora Imaging Library information.

| Direction | Signal | Auxiliary | Description | Connector |
| --- | --- | --- | --- | --- |
| Out | `AUX_OPTOIND_OUT0` | `M_AUX_IO0` | Opto-isolated auxiliary signal 0 (output), which supports: user output, timer output, and receiving the state of an I/O command register bit. | HD-15 (pin 12, 11) |
| Out | `AUX_OPTOIND_OUT1` | `M_AUX_IO1` | Opto-isolated auxiliary signal 1 (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | HD-15 (pin 6, 1) |
| In | `AUX_OPTOIND_IN2` | `M_AUX_IO2` | Opto-isolated auxiliary signal 2 (input), which supports: user input, I/O command list counter source, timer clock, timer arm, reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | HD-15 (pin 5, 4) |
| In | `AUX_OPTOIND_IN3` | `M_AUX_IO3` | Opto-isolated auxiliary signal 3 (input), which supports: user input, I/O command list counter source, timer clock, timer arm, reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | HD-15 (pin 3, 4) |
| In | `AUX_OPTOIND_IN4` | `M_AUX_IO4` | Opto-isolated auxiliary signal 4 (input), which supports: user input, I/O command list counter source, timer clock, timer arm, reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | HD-15 (pin 9, 8) |
| In | `AUX_OPTOIND_IN5` | `M_AUX_IO5` | Opto-isolated auxiliary signal 5 (input), which supports: user input, I/O command list counter source, timer clock, timer arm, reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | HD-15 (pin 2, 8) |
| In | `AUX_OPTOIND_IN6` | `M_AUX_IO6` | Opto-isolated auxiliary signal 6 (input), which supports: user input, I/O command list counter source, timer clock, timer arm, reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | HD-15 (pin 15, 14) |
| In | `AUX_OPTOIND_IN7` | `M_AUX_IO7` | Opto-isolated auxiliary signal 7 (input), which supports: user input, I/O command list counter source, timer clock, timer arm, reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | HD-15 (pin 13, 14) |
