---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Using_Iris_GTX
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Using Iris GTX"
---

# Using Zebra Iris GTX with Aurora Imaging Library

To use Zebra Iris GTX, you must allocate it as an Aurora Imaging Library Iris GTX system ([`M_SYSTEM_IRIS_GTX`](../../Reference/sys/MsysAlloc.md)), using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). This allocation opens communication with Zebra Iris GTX and allows Aurora Imaging Library to use its resources. To grab images from your Zebra Iris GTX, you must allocate a single digitizer, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEV0`](../../Reference/dig/MdigAlloc.md). You can allocate an Aurora Imaging Library Iris GTX system for your smart camera in multiple processes (executables). However, only one digitizer can be allocated at a time for your Zebra Iris GTX's image sensor (across all processes).

To set up grabbing with triggers, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TRIGGER...`](../../Reference/dig/MdigControl.md).

## Using the Advanced I/O Engine with Zebra Iris GTX

Zebra Iris GTX has an Advanced I/O Engine that controls the auxiliary I/O interface. The Advanced I/O Engine includes 7 auxiliary signals (4 inputs and 3 outputs), 1 analog intensity control signal, 8 general timers, 1 exposure timer, 1 strobe timer, 1 quadrature decoder, and 1 I/O command list (with 2 reference latches). For more information on some of these features, refer to [I/O signals and communicating with external devices](../../UserGuide/C56_IO_signals/ChapterInformation.md).

> **Note:** Note, to program the exposure timer, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_EXPOSURE...`](../../Reference/dig/MdigControl.md); whereas, to control the strobe timer, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_TIMER...`](../../Reference/dig/MdigControl.md) + [`M_TIMER_STROBE`](../../Reference/dig/MdigControl.md).

## Using EtherNet/IP, Modbus, CC-Link, or PROFINET

You can use the Gigabit Ethernet port for communication with external devices using the EtherNet/IP, Modbus (over TCP/IP), CC-Link, or PROFINET industrial protocol. For PROFINET communication, the port has access to the PROFINET Engine; the interface of this engine is recognized as a second network device with its own IP and MAC settings (when the PROFINET service is enabled) even though it shares the same LAN connection. By default, PROFINET is disabled. To enable it, open the Aurora Imaging Configurator utility, expand the **Communication** tab, select the **PROFINET** page, and add a PROFINET protocol instance; note that instance names are case sensitive. Once the protocol instance is added, the status of the protocol instance should be **Ready**. If the status is **Waiting NIC**, there is no cable connected to the Ethernet port.

For more information, see [Steps to perform industrial communication](../../UserGuide/C57_Industrial_communication/S02_Steps_to_perform_industrial_communication.md).

## Using the analog intensity control signal with Zebra Iris GTX

Zebra Iris GTX has an analog intensity control signal that provides a slow changing analog intensity (dimming) control signal from 0-10 VDC. This control signal can be used to set the intensity for a light controller (such as, an Advanced Illumination inline control system, a Smart Vision Lights brick light, or similar device). This signal should not be used to draw any current or drive power; it should only be used as a reference voltage for dimming control. To set the intensity of the light controller, connect the lighting controller's intensity pin directly to the analog intensity control pin and use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_LIGHTING_BRIGHT_FIELD`](../../Reference/dig/MdigControl.md). To see an example of how to connect your light controller to the analog intensity control signal, refer to the Zebra Iris GTX Hardware and Installation manual.

## Performing Bayer color conversion in hardware

Zebra Iris GTX color smart cameras use a sensor with a Bayer color filter (as specified by the DCF); when grabbing, the cameras perform Bayer color conversion in hardware before saving the images. If the images require white balancing, Zebra Iris GTX can perform this automatically in hardware if white balancing is enabled using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). If performing white balancing, you can use the default white balance coefficients, automatically have them calculated (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md)), or set explicit coefficients ([`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md)). For information on Bayer color conversion, refer to [Using images acquired with a Bayer color filter](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

If you don't want to perform Bayer color conversion in hardware, disable it using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md).

The [`M_BAYER...`](../../Reference/dig/MdigControl.md) control types can only be used when grabbing from a color version of Zebra Iris GTX; otherwise, an error will be generated.

## Zebra Iris GTX exposure mode

On Zebra Iris GTX, you can specify that the exposure duration is set with a timer or the width of the trigger signal, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_EXPOSURE_MODE`](../../Reference/dig/MdigControl.md) set to [`M_TIMED`](../../Reference/dig/MdigControl.md) or [`M_TRIGGER_WIDTH`](../../Reference/dig/MdigControl.md), respectively. When set to[`M_TIMED`](../../Reference/dig/MdigControl.md), the exposure duration will last as long as the time specified with [`M_EXPOSURE_TIME`](../../Reference/dig/MdigControl.md).

[`M_TRIGGER_WIDTH`](../../Reference/dig/MdigControl.md) can only be used when [`M_GRAB_TRIGGER_ACTIVATION`](../../Reference/dig/MdigControl.md) is set to[`M_LEVEL_HIGH`](../../Reference/dig/MdigControl.md) or [`M_LEVEL_LOW`](../../Reference/dig/MdigControl.md). When set to [`M_LEVEL_HIGH`](../../Reference/dig/MdigControl.md), the exposure will be triggered upon the rising edge of the trigger signal, and will remain active while the trigger signal is high. When set to [`M_LEVEL_LOW`](../../Reference/dig/MdigControl.md), the exposure will be triggered upon the falling edge of the trigger signal, and will remain active while the trigger signal is low.

*[Image: Trigger_exposure_modes.png]*
