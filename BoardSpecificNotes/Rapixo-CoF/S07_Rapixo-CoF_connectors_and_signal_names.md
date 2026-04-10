---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CoF
section: Rapixo-CoF_connectors_and_signal_names
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cof / Rapixo-CoF connectors and signal names"
---

# Zebra Rapixo CoF connectors and signal names

This section serves as a reference to match Zebra Rapixo CoF's connectors and auxiliary signals with Aurora Imaging Library information, such as Aurora Imaging Library auxiliary signal numbers. To set/inquire all the settings for this board's auxiliary signals (for example, signal routing and timer settings), use [`MdigControl`](../../Reference/dig/MdigControl.md)/[`MdigInquire`](../../Reference/dig/MdigInquire.md), respectively.

On Zebra Rapixo CoF, the auxiliary signals are acquisition path independent. Any digitizer allocated on the board ([`M_DEV0`](../../Reference/dig/MdigAlloc.md) through [`M_DEV3`](../../Reference/dig/MdigAlloc.md)) can access any of the auxiliary signals.

Auxiliary I/O signals can have one or more functionalities (for example, trigger input, timer output, or user output, depending on the signal). Their possible functionalities are described in their description in the pinout table below.

Only those auxiliary signals that have matching Aurora Imaging Library information are included in this section. For information on internal connectors and a comprehensive list of all available input and output signals, refer to the board's installation and hardware reference manual.

## Board connectors

The Zebra Rapixo CoF board provide several interface connectors: the QSFP transceiver cage and an HD-15 connector on the main bracket. In addition, close to the top edge of the main board, there is an internal auxiliary I/O connector that can be used with an adapter bracket to access additional I/O. On the cable adapter bracket, there are two external auxiliary I/O connectors which can connect to internal auxiliary connector 1/2 or 2/3.

*[Image: Rapixo_CoF_adapter_brackets_side.png]*

Only the following connectors have auxiliary signals with matching Aurora Imaging Library information.

| Connector Name | Connector Abbreviation | Image | Description |
| --- | --- | --- | --- |
| External auxiliary I/O connectors | HD-15 (0, 1, 2 and 3) | *[Image: db15M_rev1.png]* | The external auxiliary I/O connectors are high-density D-subminiature 15-pin male connectors. They are used to transmit/receive auxiliary signals. External auxiliary I/O connector 0 is located on the main bracket, and external auxiliary I/O connectors 1/2 or 2/3 are located on the cable adapter bracket. |

## Signal names and their matching Aurora Imaging Library constants

The table below lists the auxiliary signals with their associated Aurora Imaging Library information. The pinout for auxiliary I/O connector 0 is as follows. Auxiliary I/O connectors 1, 2, and 3 have the same pinout as auxiliary I/O connector 0, except you must add 8, 16, or 24, respectively, to the number at the end of their hardware signal name and Aurora Imaging Library constant. For example, AUX(TRIG)_TTL_IO_4 (` `M_AUX_IO4` `) on connector 0 would be AUX(TRIG)_TTL_IO_12 (` `M_AUX_IO12` `) on connector 1.

| Direction | Signal | Auxiliary | Description | Connector |
| --- | --- | --- | --- | --- |
| In-Out | `AUX(TRIG)_TTL_IO_4` | `M_AUX_IO4` | TTL auxiliary signal 4 (input/output), which supports: timer output, trigger input, user input, or user output. | HD-15 (0) (pin 1) |
| In-Out | `AUX(TRIG)_TTL_IO_5` | `M_AUX_IO5` | TTL auxiliary signal 5 (input/output), which supports: timer output, trigger input, user input, or user output. | HD-15 (0) (pin 2) |
| In-Out | `AUX(TRIG)_TTL_IO_6` | `M_AUX_IO6` | TTL auxiliary signal 6 (input/output), which supports: timer output, trigger input, user input, or user output. | HD-15 (0) (pin 3) |
| In | `AUX(TRIG)_LVDS_IN2` | `M_AUX_IO2` | LVDS auxiliary signal 2 (input), which supports: trigger input, user input, or quadrature input bit 0. | HD-15 (0) (pin 4, 5) |
| In | `AUX(TRIG)_LVDS_IN3` | `M_AUX_IO3` | LVDS auxiliary signal 3 (input), which supports: trigger input, user input, or quadrature input bit 1. | HD-15 (0) (pin 6, 8) |
| In | `AUX(TRIG)_OPTO_IN1` | `M_AUX_IO1` | Opto-isolated auxiliary signal 1 (input), which supports: trigger input or user input. | HD-15 (0) (pin 12, 11) |
| Out | `AUX(EXP)_LVDS_OUT7` | `M_AUX_IO7` | LVDS auxiliary signal 7 (output), which supports: timer output or user output. | HD-15 (0) (pin 13, 14) |
| In | `AUX(TRIG)_OPTO_IN0` | `M_AUX_IO0` | Opto-isolated auxiliary signal 0 (input), which supports: trigger input or user input. | HD-15 (0) (pin 15, 9) |
