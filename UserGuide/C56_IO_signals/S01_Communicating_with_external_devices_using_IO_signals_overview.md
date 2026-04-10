---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Communicating_with_external_devices_using_IO_signals_overview
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Communicating with external devices using IO signals overview"
---

# Communicating with external devices using I/O signals overview

Many non-video input and output signals on your camera, Zebra frame grabber (or interface board), or Zebra computer can be controlled using Aurora Imaging Library to synchronize with, react to, and/or cause external events. For example, you can route input or output (I/O) signals for synchronizing with or controlling other devices, such as an external trigger device or a strobe light, respectively. These I/O signals are categorized into three groups: auxiliary signals, camera control signals, and transport layer signals. Depending on the signal, an I/O signal can support one or more functionalities, such as:

- Clock input or output.
- CSYNC, HSYNC, or VSYNC.
- Data valid.
- Exposure output.
- Field polarity input.
- I/O command register bit output.
- Pulse-train strobe output.
- Quadrature input.
- Timer output.
- Trigger input.
- Trigger signal bypass.
- User input.
- User output.

For more information on which functionalities are supported by the signals of your hardware, refer to the Connectors and signal names section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra product. In addition, some products have an advanced I/O engine (for example, Iris GTX, or Indio), which refers to a series of hardware elements that allow you to communicate and coordinate events with I/O pins. These elements, like timers, I/O command lists, and rotary decoders are further described in this chapter. Only those I/O signals with matching Aurora Imaging Library information are documented in Aurora Imaging Library Help.

Note that if your hardware supports I/O command lists, you can schedule commands to affect a bit of an I/O command register and route this bit's state to an output signal that supports this type of the output. You can schedule commands based on time or positional (rotary decoder) information.

When working with I/O signals, you will primarily be using functions in the Digitizer module ([`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md)) or the System module ([`MsysControl`](../../Reference/sys/MsysControl.md) and [`MsysInquire`](../../Reference/sys/MsysInquire.md)), depending on your hardware. You can use Aurora Imaging Intellicam's Feature Browser to interactively test the digitizer and system settings in real-time; depending on your Zebra product, you can also save the settings selected in the Feature Browser to the DCF for your camera.
