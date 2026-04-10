---
doctype: BoardSpecificNotes
part: ""
chapter: 4Sight
section: Using_4Sight
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / 4sight / Using 4Sight"
---

# Using Zebra 4Sight with Aurora Imaging Library

Aurora Imaging Library treats Zebra 4Sight units as a normal computer.

To use the processing power of Zebra 4Sight, allocate a Host system using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_HOST`](../../Reference/sys/MsysAlloc.md).

Zebra 4Sight EV7 has auxiliary signals capabilities. For more information, see the [Using the Advanced I/O Engine of Zebra 4Sight EV7](S02_Using_4Sight.md).

Zebra 4Sight EV7 allows you to use either GigE Vision-compliant cameras or USB3 Vision-compliant cameras directly connected to the unit.

| Camera type | System name | Aurora Imaging Library system-specific information (Aurora Imaging Library Reference) |
| --- | --- | --- |
| GigE Vision | [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md) | GigE Vision |
| USB3 Vision | [`M_SYSTEM_USB3_VISION`](../../Reference/sys/MsysAlloc.md) | USB3 Vision |

Depending on the model of your Zebra 4Sight unit, it can come preinstalled with either the Windows 10 IoT or Windows 11 IoT Enterprise operating system and a runtime license for both Aurora Imaging Library and Aurora Design Assistant, or just Aurora Imaging Library. The runtime license includes the industrial and robot communication module and allows for Distributed Aurora Imaging Library configurations; this might change over time, contact your Zebra representative for more information.

If you are using a Zebra 4Sight XV7, you can install additional boards in the unit, such as a Zebra frame grabber or I/O board. To use the features of an installed board, you must first allocate the board as an Aurora Imaging Library system. For more information, refer to the _Hardware-specific Notes_ for that particular board (for example, see [Using Indio](../Indio/S02_Using_Indio.md) for Zebra Indio).

> **Note:** Note that when installing a redistribution (runtime) version of Aurora Imaging Library on Zebra 4Sight units, the license number will be based on the hardware fingerprint of your unit. This means that you will not be able to copy the licensed version of Aurora Imaging Library to another computer.

## Using the Advanced I/O Engine of Zebra 4Sight EV7

Zebra 4Sight EV7 has an Advanced I/O Engine that controls the auxiliary I/O interface. The engine includes 16 timers, 2 quadrature decoders, 2 I/O command lists, 8 auxiliary output signals, and 8 auxiliary input signals. To route auxiliary I/O signals and configure interrupts, use [`MsysControl`](../../Reference/sys/MsysControl.md). To inquire this information, use [`MsysInquire`](../../Reference/sys/MsysInquire.md). For more information on some of these features, refer to [I/O signals and communicating with external devices](../../UserGuide/C56_IO_signals/ChapterInformation.md).

## Using EtherNet/IP, Modbus, CC-Link, or PROFINET on Zebra 4Sight EV7

Zebra 4Sight EV7 supports industrial communication with external devices using protocols such as EtherNet/IP, Modbus (over TCP/IP), CC-Link, or PROFINET. To use hardware-assisted PROFINET, you must use the port that has access to the PROFINET engine (LAN2). The interface of the PROFINET engine is recognized as a second network device with its own IP and MAC settings (when the PROFINET service is enabled) even though it shares the same LAN connection. To enable PROFINET, open the Aurora Imaging Configurator utility, expand the **Communication** tab, select the **PROFINET** page, and add a PROFINET protocol instance; note that instance names are case sensitive. Once the protocol instance is added, the status of the protocol instance should be **Ready**. If the status is **Waiting NIC**, there is no cable connected to the Ethernet port. By default, PROFINET is disabled.

For more information, see [Steps to perform industrial communication](../../UserGuide/C57_Industrial_communication/S02_Steps_to_perform_industrial_communication.md).
