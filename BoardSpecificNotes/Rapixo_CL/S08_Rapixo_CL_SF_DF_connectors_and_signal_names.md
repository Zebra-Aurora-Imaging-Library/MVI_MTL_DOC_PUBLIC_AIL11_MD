---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Rapixo_CL_SF_DF_connectors_and_signal_names
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Rapixo CL SF DF connectors and signal names"
---

# Zebra Rapixo CL SF and CL Pro SF/DF connectors and signal names

This section serves as a reference to match Zebra Rapixo CL SF, Pro SF, and Pro DF's connectors and auxiliary signals with Aurora Imaging Library information, such as Aurora Imaging Library auxiliary/camera control signal numbers. To set/inquire all the settings for this board's auxiliary signals (for example, signal routing and timer settings), use [`MdigControl`](../../Reference/dig/MdigControl.md)/[`MdigInquire`](../../Reference/dig/MdigInquire.md), respectively.

Zebra Rapixo CL family members have a different number of acquisition paths and auxiliary/camera control signals. For each Zebra Rapixo CL family member, only the auxiliary/camera control signals associated with the following digitizer device numbers are supported:

| Zebra Rapixo CL type | Digitizer device # |
| --- | --- |
| Zebra Rapixo CL SF | [`M_DEV0`](../../Reference/dig/MdigAlloc.md). |
| Zebra Rapixo CL Pro SF | [`M_DEV0`](../../Reference/dig/MdigAlloc.md). |
| Zebra Rapixo CL Pro DF | [`M_DEV0`](../../Reference/dig/MdigAlloc.md), [`M_DEV1`](../../Reference/dig/MdigAlloc.md). |

The following table lists the connectors of the auxiliary/control signals that can be used for each digitizer device number:

| Digitizer device # | Zebra Rapixo CL SF | Zebra Rapixo CL Pro SF | Zebra Rapixo CL Pro DF |
| --- | --- | --- | --- |
| [`M_DEV0`](../../Reference/dig/MdigAlloc.md) | Auxiliary I/O connector A and C | Auxiliary I/O connector A and C | Auxiliary I/O connector A and C |
| [`M_DEV1`](../../Reference/dig/MdigAlloc.md) | Auxiliary I/O connector B and D |

Auxiliary I/O signals can have one or more functionalities (for example, trigger input, timer output, or user output, depending on the signal). Their possible functionalities are described in their description in the pinout table below. Note that, unlike some other Zebra frame grabbers, there is no limit to the number of events that can be triggered simultaneously using the auxiliary input signals, nor is there a restriction on which auxiliary signal can be used to trigger the event.

Only those auxiliary signals that have matching Aurora Imaging Library information are included in this section. For information on internal connectors and a comprehensive list of all available input and output signals, refer to the board's installation and hardware reference manual.

## Board connectors

The Zebra Rapixo CL SF, Pro SF, and Pro DF boards and cable adapter brackets provide several interface connectors. On the bracket of Zebra Rapixo CL and Pro SF, there are two Camera Link video input connectors and an auxiliary I/O connector. On the double bracket of Zebra Rapixo CL Pro DF, there are two pairs of Camera Link video input connectors and two auxiliary I/O connectors. On the cable adapter bracket, there are two external auxiliary I/O connectors.

*[Image: rapixo_CL_Pro__DB_SF_side_view_with_adapter.png]*

*[Image: rapixo_CL_Pro_DF_QB_side_view_with_adapter.png]*

Only the following connectors have auxiliary signals with matching Aurora Imaging Library information.

| Connector Name | Connector Abbreviation | Image | Description |
| --- | --- | --- | --- |
| Camera Link video input connectors | SDR (0, 1, 2, and 3) | *[Image: conn_Camera_Link_mini.png]* | The Camera Link video input connectors are 26-pin high-density female mini Camera Link connectors. They are used to receive video input, timing, synchronization signals and transmit/receive communication signals between the video source and the frame grabber. There are two Camera Link video input connectors (SDR 0 and 1) on Zebra Rapixo CL SF and Pro SF. There are four Camera Link video input connectors (SDR 0, 1, 2 and 3) on Zebra Rapixo CL Pro DF. |
| External auxiliary I/O connector | HD-15 [^HD-15_connector](A, B, C, and D) | *[Image: db15M_rev1.png]* | The external auxiliary I/O connectors are high-density D-subminiature 15-pin male connectors. They are used to transmit/receive auxiliary signals. External auxiliary I/O connectors A and (if present) B are located on Zebra Rapixo CL Pro, and external auxiliary I/O connectors C and D are located on the cable adapter bracket. |

[^HD-15_connector]: Previously referred to as DBHD-15, but more accurately known as DE-15.

## Signal names and their matching Aurora Imaging Library constants

The table below lists the auxiliary signals with their associated Aurora Imaging Library information.

| Direction | Signal | Auxiliary | Description | Connector |
| --- | --- | --- | --- | --- |
| Out | `CC1 (0)` | `M_CC_IO1` | Camera control output 1 for acquisition path 0, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN9`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_6`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN11`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN1`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_4`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN2`). | HDR/SDR (0) (pin 18, 5) |
| Out | `CC1 (2)` | `M_CC_IO1` | Camera control output 1 for acquisition path 1, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN25`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_22`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN27`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN17`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_20`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN18`). | HDR/SDR (2) (pin 18, 5) |
| Out | `CC2 (0)` | `M_CC_IO2` | Camera control output 2 for acquisition path 0, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN9`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_6`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN11`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN1`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_4`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN2`). | HDR/SDR (0) (pin 4, 17) |
| Out | `CC2 (2)` | `M_CC_IO2` | Camera control output 2 for acquisition path 1, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN25`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_22`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN27`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN17`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_20`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN18`). | HDR/SDR (2) (pin 4, 17) |
| Out | `CC3 (0)` | `M_CC_IO3` | Camera control output 3 for acquisition path 0, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN9`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_6`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN11`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN1`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_4`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN2`). | HDR/SDR (0) (pin 16, 3) |
| Out | `CC3 (2)` | `M_CC_IO3` | Camera control output 3 for acquisition path 1, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN25`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_22`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN27`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN17`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_20`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN18`). | HDR/SDR (2) (pin 16, 3) |
| Out | `CC4 (0)` | `M_CC_IO4` | Camera control output 4 for acquisition path 0, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN9`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_6`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN11`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN1`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_4`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV0`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN2`). | HDR/SDR (0) (pin 2, 15) |
| Out | `CC4 (2)` | `M_CC_IO4` | Camera control output 4 for acquisition path 1, which supports: timer output, user output, VSYNC output, HSYNC output, clock output, or rerouting of specific auxiliary input signals. Only the following auxiliary input signals can be rerouted to this output signal:

 - [`M_AUX_IO1`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN25`).
- [`M_AUX_IO2`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_22`).
- [`M_AUX_IO5`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN27`).
- [`M_AUX_IO7`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`OPTO_AUX_IN17`).
- [`M_AUX_IO8`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`TTL_AUX_IO_20`).
- [`M_AUX_IO10`](../../Reference/dig/MdigControl.md) on [`M_DEV1`](../../Reference/dig/MdigAlloc.md) (`LVDS_AUX_IN18`). | HDR/SDR (2) (pin 2, 15) |
| In | `LVDS_AUX_IN2` | `M_AUX_IO10` | LVDS auxiliary signal (input) for acquisition path 0, which supports: trigger input, user input, or quadrature input bit 0. | HD-15 (A) (pin 5, 4) |
| In | `LVDS_AUX_IN3` | `M_AUX_IO11` | LVDS auxiliary signal (input) for acquisition path 0, which supports: user input, trigger input, timer-clock input, or quadrature input bit 1. | HD-15 (A) (pin 8, 6) |
| In | `LVDS_AUX_IN10` | `M_AUX_IO4` | LVDS auxiliary signal (input) for acquisition path 0, which supports: trigger input or user input. | HD-15 (C) (pin 5, 4) |
| In | `LVDS_AUX_IN11` | `M_AUX_IO5` | LVDS auxiliary signal (input) for acquisition path 0, which supports: trigger input or user input. | HD-15 (C) (pin 8, 6) |
| In | `LVDS_AUX_IN18` | `M_AUX_IO10` | LVDS auxiliary signal (input) for acquisition path 1, which supports: trigger input, user input, or quadrature input bit 0. | HD-15 (B) (pin 5, 4) |
| In | `LVDS_AUX_IN19` | `M_AUX_IO11` | LVDS auxiliary signal (input) for acquisition path 1, which supports: user input, trigger input, timer-clock input, or quadrature input bit 1. | HD-15 (B) (pin 8, 6) |
| In | `LVDS_AUX_IN26` | `M_AUX_IO4` | LVDS auxiliary signal (input) for acquisition path 1, which supports: trigger input or user input. | HD-15 (D) (pin 5, 4) |
| In | `LVDS_AUX_IN27` | `M_AUX_IO5` | LVDS auxiliary signal (input) for acquisition path 1, which supports: trigger input or user input. | HD-15 (D) (pin 8, 6) |
| Out | `LVDS_AUX_OUT7` | `M_AUX_IO12` | LVDS auxiliary signal (output) for acquisition path 0, which supports: timer output or user output. | HD-15 (A) (pin 14, 13) |
| Out | `LVDS_AUX_OUT23` | `M_AUX_IO13` | LVDS auxiliary signal (output) for acquisition path 1, which supports: timer output or user output. | HD-15 (B) (pin 14, 13) |
| In | `OPTO_AUX_IN0` | `M_AUX_IO6` | Opto-isolated auxiliary signal (input) for acquisition path 0, which supports: user input or trigger input. | HD-15 (A) (pin 9, 15) |
| In | `OPTO_AUX_IN1` | `M_AUX_IO7` | Opto-isolated auxiliary signal (input) for acquisition path 0, which supports: user input or trigger input. | HD-15 (A) (pin 11, 12) |
| In | `OPTO_AUX_IN8` | `M_AUX_IO0` | Opto-isolated auxiliary signal (input) for acquisition path 0, which supports: trigger input or user input. | HD-15 (C) (pin 9, 15) |
| In | `OPTO_AUX_IN9` | `M_AUX_IO1` | Opto-isolated auxiliary signal (input) for acquisition path 0, which supports: trigger input or user input. | HD-15 (C) (pin 11, 12) |
| In | `OPTO_AUX_IN16` | `M_AUX_IO6` | Opto-isolated auxiliary signal (input) for acquisition path 1, which supports: user input or trigger input. | HD-15 (B) (pin 9, 15) |
| In | `OPTO_AUX_IN17` | `M_AUX_IO7` | Opto-isolated auxiliary signal (input) for acquisition path 1, which supports: user input or trigger input. | HD-15 (B) (pin 11, 12) |
| In | `OPTO_AUX_IN24` | `M_AUX_IO0` | Opto-isolated auxiliary signal (input) for acquisition path 1, which supports: trigger input or user input. | HD-15 (D) (pin 9, 15) |
| In | `OPTO_AUX_IN25` | `M_AUX_IO1` | Opto-isolated auxiliary signal (input) for acquisition path 1, which supports: trigger input or user input. | HD-15 (D) (pin 11, 12) |
| In-Out | `TTL_AUX_IO_4` | `M_AUX_IO8` | TTL auxiliary signal (input/output) for acquisition path 0, which supports: user input/output, trigger input. | HD-15 (A) (pin 1) |
| In-Out | `TTL_AUX_IO_5` | `M_AUX_IO9` | TTL auxiliary signal (input/output) for acquisition path 0, which supports: timer output, trigger input, user input, or user output. | HD-15 (A) (pin 2) |
| In-Out | `TTL_AUX_IO_6` | `M_AUX_IO2` | TTL auxiliary signal (input/output) for acquisition path 0, which supports: trigger input, user input, user output, or timer output. | HD-15 (A) (pin 3) |
| In-Out | `TTL_AUX_IO_14` | `M_AUX_IO3` | TTL auxiliary signal (input/output) for acquisition path 0, which supports: trigger input, user input, or user output. | HD-15 (C) (pin 3) |
| In-Out | `TTL_AUX_IO_20` | `M_AUX_IO8` | TTL auxiliary signal (input/output) for acquisition path 1, which supports: trigger input, user input, or user output. | HD-15 (B) (pin 1) |
| In-Out | `TTL_AUX_IO_21` | `M_AUX_IO9` | TTL auxiliary signal (input/output) for acquisition path 1, which supports: timer output, trigger input, user input, or user output. | HD-15 (B) (pin 2) |
| In-Out | `TTL_AUX_IO_22` | `M_AUX_IO2` | TTL auxiliary signal (input/output) for acquisition path 1, which supports: timer output, trigger input, user input, or user output. | HD-15 (B) (pin 3) |
| In-Out | `TTL_AUX_IO_30` | `M_AUX_IO3` | TTL auxiliary signal (input/output) for acquisition path 1, which supports: trigger input, user input, or user output. | HD-15 (D) (pin 3) |
