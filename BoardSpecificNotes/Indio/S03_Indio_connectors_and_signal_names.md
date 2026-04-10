---
doctype: BoardSpecificNotes
part: ""
chapter: Indio
section: Indio_connectors_and_signal_names
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / indio-u53 / Indio connectors and signal names"
---

# Zebra Indio connectors and signal names

This section serves as a reference to match Zebra Indio's connectors and auxiliary signals with Aurora Imaging Library information, such as Aurora Imaging Library auxiliary signal numbers. To set/inquire all the settings for this board's auxiliary signals (for example, signal routing and timer settings), use [`MsysControl`](../../Reference/sys/MsysControl.md)/[`MsysInquire`](../../Reference/sys/MsysInquire.md), respectively.

Auxiliary I/O signals can have one or more functionalities (for example, outputting the state of an I/O command register bit, timer output, or user output, depending on the signal). Their possible functionalities are described in their description in the pinout table below. Note that, unlike some other Zebra frame grabbers, there is no limit to the number of events that can be triggered simultaneously using the auxiliary input signals, nor is there a restriction on which auxiliary signal can be used to trigger the event.

Only those auxiliary signals that have matching Aurora Imaging Library information are included in this section. For information on internal connectors and a comprehensive list of all available input and output signals, refer to the board's installation and hardware reference manual.

## Board connectors

The Zebra Indio board provides an auxiliary I/O connector and a Gigabit Ethernet connector.

*[Image: Indio_bracket.png]*

Only the following connectors have auxiliary signals with matching Aurora Imaging Library information.

| Connector Name | Connector Abbreviation | Image | Description |
| --- | --- | --- | --- |
| Auxiliary I/O connector | DB-37 | *[Image: DSUB37_F_connector_on_bracket.PNG]* | The external auxiliary I/O connector is a D-subminiature 37-pin female connector. It is used to transmit/receive auxiliary signals. |

## Signal names and their matching Aurora Imaging Library constants

The table below lists the auxiliary signals with their associated Aurora Imaging Library information.

| Direction | Signal | Auxiliary | Description | Connector |
| --- | --- | --- | --- | --- |
| Out | `AUX_OPTOIND_OUT0` | `M_AUX_IO0` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 2, 1) |
| Out | `AUX_OPTOIND_OUT1` | `M_AUX_IO1` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 4, 3) |
| Out | `AUX_OPTOIND_OUT2` | `M_AUX_IO2` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 7, 6) |
| Out | `AUX_OPTOIND_OUT3` | `M_AUX_IO3` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 9, 8) |
| Out | `AUX_OPTOIND_OUT4` | `M_AUX_IO4` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 12, 11) |
| Out | `AUX_OPTOIND_OUT5` | `M_AUX_IO5` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 14, 13) |
| Out | `AUX_OPTOIND_OUT6` | `M_AUX_IO6` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 17, 16) |
| Out | `AUX_OPTOIND_OUT7` | `M_AUX_IO7` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | DB-37 (pin 19, 18) |
| In | `AUX_OPTOIND_IN8` | `M_AUX_IO8` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 21, 20) |
| In | `AUX_OPTOIND_IN9` | `M_AUX_IO9` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 23, 22) |
| In | `AUX_OPTOIND_IN10` | `M_AUX_IO10` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 26, 25) |
| In | `AUX_OPTOIND_IN11` | `M_AUX_IO11` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 28, 27) |
| In | `AUX_OPTOIND_IN12` | `M_AUX_IO12` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 30, 29) |
| In | `AUX_OPTOIND_IN13` | `M_AUX_IO13` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 32, 31) |
| In | `AUX_OPTOIND_IN14` | `M_AUX_IO14` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 35, 34) |
| In | `AUX_OPTOIND_IN15` | `M_AUX_IO15` | Industrial auxiliary signal (input), which supports: user input, I/O command list counter source, timer clock, timer arm, I/O command list reference latch trigger, quadrature input bit 0 or 1, or quadrature decoder counter reset source. | DB-37 (pin 37, 36) |
