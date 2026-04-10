---
doctype: BoardSpecificNotes
part: ""
chapter: Indio
section: Using_Indio
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / indio-u53 / Using Indio"
---

# Using Zebra Indio with Aurora Imaging Library

Zebra Indio allows you to use a GigE Vision-compliant camera for acquisition. To use a GigE Vision-compliant camera for acquisition with Zebra Indio and Aurora Imaging Library, you must allocate an Aurora Imaging Library GigE Vision system ([`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens communication with GigE Vision compliant cameras. You can then access and grab from multiple connected GigE Vision cameras by allocating a digitizer for each connected camera (using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEVn`](../../Reference/dig/MdigAlloc.md)). Each process (executable) can allocate only one Aurora Imaging Library GigE Vision system. Each camera can be used by one process (executable) at a time. For additional information, refer to [Zebra GigE Vision driver](../GigE_Vision/ChapterInformation.md).

To use other functionality available on Zebra Indio (for example, I/O signals and industrial communication) or to inquire about the status (for example, board type) of the board, you must allocate an Aurora Imaging Library Indio system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)with [`M_SYSTEM_INDIO`](../../Reference/sys/MsysAlloc.md).

Zebra Indio is preinstalled with a runtime license for Aurora Imaging Library, which includes the Industrial and Robot Communication module (Mcom) and interface module. Be aware that to access the Aurora Imaging Library license that is included on the Zebra Indio board, you must install the Zebra Indio driver.

## Using the Advanced I/O Engine of Zebra Indio

Zebra Indio has an Advanced I/O Engine that controls the auxiliary I/O interface. The engine includes 16 timers, 2 rotary decoders, 2 I/O command lists, a user-output register, 8 auxiliary output signals, and 8 auxiliary input signals. To route auxiliary I/O signals and configure interrupts, use [`MsysControl`](../../Reference/sys/MsysControl.md). To inquire this information, use [`MsysInquire`](../../Reference/sys/MsysInquire.md). For more information on some of these features, refer to [I/O signals and communicating with external devices](../../UserGuide/C56_IO_signals/ChapterInformation.md).

## Using EtherNet/IP, Modbus, or PROFINET

You can use the Gigabit Ethernet port for communication with external devices using the EtherNet/IP, Modbus (over TCP/IP), or PROFINET industrial protocol. In this case, you should not use the port for acquisition. Unless using PROFINET, you should not use the port for general network traffic. Both EtherNet/IP and Modbus over TCP/IP communication require installation of the Intel i210 Network Adapter Driver available from [downloadcenter.intel.com](https://downloadcenter.intel.com/). When PROFINET is used, communication is assisted using the Zebra PROFINET Engine; its interface is recognized as a second network device with its own IP and MAC seetings (when the PROFINET service is enabled) even though it shares the same LAN connection. For more information, see [Steps to perform industrial communication](../../UserGuide/C57_Industrial_communication/S02_Steps_to_perform_industrial_communication.md).
