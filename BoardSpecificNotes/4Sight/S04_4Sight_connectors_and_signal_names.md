---
doctype: BoardSpecificNotes
part: ""
chapter: 4Sight
section: 4Sight_connectors_and_signal_names
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / 4sight / 4Sight connectors and signal names"
---

# Zebra 4Sight EV7 connectors and signal names

This section serves as a reference to match Zebra 4Sight EV7's connectors and auxiliary signals with Aurora Imaging Library information, such as Aurora Imaging Library auxiliary signal numbers.

Only those auxiliary signals that have matching Aurora Imaging Library information are included in this section. To set/inquire all the settings for this computer's auxiliary signals, use [`MsysControl`](../../Reference/sys/MsysControl.md)/[`MsysInquire`](../../Reference/sys/MsysInquire.md).

For information on other connectors and a comprehensive list of all available input and output signals, refer to the computer's installation and hardware reference manual.

## Board connectors

Only the following connector has auxiliary signals with matching Aurora Imaging Library information.

| Connector Name | Image | Description |
| --- | --- | --- |
| Auxiliary I/O terminal block connector | *[Image: 4Sight_EV_Aux_IO_Connector.png]* | The auxiliary I/O interface connector is a terminal block connector. It is composed of 8 isolated auxiliary output signals and 8 isolated auxiliary input signals. These auxiliary signals can be used to send and receive digital control signals to and from external devices, respectively. |

## Signal names and their matching Aurora Imaging Library constants

The table below lists the auxiliary signals with their associated Aurora Imaging Library information.

| Direction | Signal | Auxiliary | Description | Connector |
| --- | --- | --- | --- | --- |
| In | `AUX_ISOIND_IN1` | `M_AUX_IO8` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 1) |
| In | `AUX_ISOIND_IN2` | `M_AUX_IO9` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 2) |
| In | `AUX_ISOIND_IN3` | `M_AUX_IO10` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 3) |
| In | `AUX_ISOIND_IN4` | `M_AUX_IO11` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 4) |
| In | `AUX_ISOIND_IN5` | `M_AUX_IO12` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 5) |
| In | `AUX_ISOIND_IN6` | `M_AUX_IO13` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 6) |
| In | `AUX_ISOIND_IN7` | `M_AUX_IO14` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 7) |
| In | `AUX_ISOIND_IN8` | `M_AUX_IO15` | Industrial auxiliary signal (input), which supports: user input or quadrature input. | Terminal block (Input) (pin 8) |
| Out | `AUX_ISOIND_OUT1` | `M_AUX_IO0` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 1) |
| Out | `AUX_ISOIND_OUT2` | `M_AUX_IO1` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 2) |
| Out | `AUX_ISOIND_OUT3` | `M_AUX_IO2` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 3) |
| Out | `AUX_ISOIND_OUT4` | `M_AUX_IO3` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 4) |
| Out | `AUX_ISOIND_OUT5` | `M_AUX_IO4` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 5) |
| Out | `AUX_ISOIND_OUT6` | `M_AUX_IO5` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 6) |
| Out | `AUX_ISOIND_OUT7` | `M_AUX_IO6` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 7) |
| Out | `AUX_ISOIND_OUT8` | `M_AUX_IO7` | Industrial auxiliary signal (output), which supports: user output, timer output, and outputting the state of an I/O command register bit. | Terminal block (Output) (pin 8) |
