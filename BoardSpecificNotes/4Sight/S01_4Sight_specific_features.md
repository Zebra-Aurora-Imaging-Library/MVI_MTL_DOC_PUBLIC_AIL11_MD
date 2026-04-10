---
doctype: BoardSpecificNotes
part: ""
chapter: 4Sight
section: 4Sight_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / 4sight / 4Sight specific features"
---

# Zebra 4Sight overview

Zebra 4Sight is a series of vision controllers (imaging computers). A vision controller is a system that connects hardware components to image processing software, providing robust vision processing capabilities. There are two families:

- **Zebra 4Sight EV7**. Zebra 4Sight EV7 is a fanless industrial vision controller that comes with hardware-assisted PROFINET support. Zebra 4Sight EV7 integrates processing and display capabilities, along with image capture from multiple GigE Vision and/or USB3 Vision cameras. Zebra 4Sight EV7 has 2.5 Gigabit Ethernet networking capabilities with support for Power-over-Ethernet (PoE), as well as industrial communication using the PROFINET, EtherNet/IP, CC-Link, or Modbus/TCP industrial protocol.
  Zebra 4Sight EV7 also has an Advanced I/O Engine; the engine provides use of 16 optically-isolated digital auxiliary I/O signals (8 inputs and 8 outputs), and includes I/O command lists, timers, quadrature decoders, and user outputs.
- **Zebra 4Sight XV7**. Zebra 4Sight XV7 is a non-industrial vision controller that offers expandability. Zebra 4Sight XV7 integrates processing and display capability, along with image capture from USB3 Vision and/or GigE Vision cameras. Zebra 4Sight XV7 supports networking and industrial communication (over GigE Ethernet ports or a RS-232/RS-422/RS-485 port).
  Zebra 4Sight XV7 also supports add-on boards compliant with the full-height, half-length PCIe form factor. Zebra 4Sight XV7 has four PCIe connectors for connecting full-height, half-length add-on boards. For example, you can connect Zebra frame grabbers and/or Zebra digital I/O boards.
  > **Note:** Zebra 4Sight XV7 does not have hardware-assisted PROFINET; however, you can connect a Zebra I/O add-on board that supports hardware-assisted PROFINET communication, such as the Zebra Indio.

## Summary of Zebra 4Sight EV7 features

The following table outlines the features currently available for Zebra 4Sight EV7.

| Features | Zebra 4Sight EV7 |
| --- | --- |
| CPU | Yes (Intel Core i3-1220PE or i5-1250PE) |
| Host memory | Up to 64 Gbytes |
| Networking | Four (4) 2.5 Gigabit Ethernet ports with PoE Two (2) standard Gigabit Ethernet ports (1 of which provides hardware-assisted PROFINET) |
| USB | Four (4) USB 3.2 SuperSpeed ports Two (2) USB 2.0 ports |
| Number of supported UARTS | Two (2) serial ports (one RS-232 and one RS-232/RS-485) |
| Number of auxiliary signals | Eight (8) inputs Eight (8) outputs |
| PROFINET Engine | Yes |
| Number of timers | Sixteen (16) |
| Number of quadrature decoders | Two (2) |
| Number of I/O command lists | Two (2) |
| Display connectors | Two (2) DisplayPort connectors |

> **Note:** You can access the latest version of the Zebra 4Sight EV7 Installation and Hardware Reference at [zebra.com/4sight-ev7-info](https://zebra.com/4sight-ev7-info). Click on the **Documentation** tab to access it.

## Summary of Zebra 4Sight XV7 features

The following table outlines the features currently available for Zebra 4Sight XV7.

| Features | Zebra 4Sight XV7 |
| --- | --- |
| CPU | Yes (Intel i5-14501E or Intel i7-14700 or i9-14900) |
| Host memory | Up to 192 Gbytes |
| Interface type | PCIe |
| Networking[^xv7_profinet] | Four (4) GigE Ethernet interfaces (100/1000/2500 BaseT Ethernet) |
| Number of supported UARTS | One (1) serial port connector (RS-232, RS-422, or RS-485) |
| Display connectors | Two (2) DisplayPort connectors One (1) HDMI connector One (1) VGA connector |

[^xv7_profinet]: Zebra 4Sight XV7 does not support hardware-assisted PROFINET. To add PROFINET support to Zebra 4sight XV7, connect an add-on board with PROFINET support (for example, Zebra Indio).

> **Note:** All of the information found in this chapter also applies to the predecessors of Zebra 4Sight XV7 (for example, Zebra 4Sight XV6). You can access the latest version of the Zebra 4Sight XV6 Installation and Hardware Reference at [zebra.com/4sight-xv6-info](https://zebra.com/4sight-xv6-info). Click on the **Documentation** tab to access it.
