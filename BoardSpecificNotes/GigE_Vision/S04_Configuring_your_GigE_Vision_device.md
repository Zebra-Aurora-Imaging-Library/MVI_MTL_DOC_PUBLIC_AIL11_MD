---
doctype: BoardSpecificNotes
part: ""
chapter: GigE_Vision
section: Configuring_your_GigE_Vision_device
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gige / Configuring your GigE Vision device"
---

# Configuring your GigE Vision camera and Gigabit Ethernet network adapter

There are several features that can improve the performance of your Zebra GigE Vision driver and associated GigE Vision-compliant camera.

> **Note:** Note that, some GigE Vision-compliant cameras include libraries that, when installed, might install network filter drivers or override your network adapter's native driver. While these third-party libraries have their own benefits, we cannot guarantee their compatibility with Aurora Imaging Library. For best results, the computer being used to communicate with your GigE Vision-compliant camera should always use Aurora Imaging Library for image capture, as well as your network adapter's native driver (for example, an Intel driver for an Intel network adapter).

## Automatic format switching

By default, the Zebra GigE Vision driver will tell the camera to transmit images in the same format as the grab buffer; this is called automatic format switching. If the grab buffer is allocated in a format that is not compliant with the camera's current pixel format, the Zebra GigE Vision driver changes the camera's pixel format to a format more compliant with the grab buffer before performing the copy operation. Automatic format switching prevents color space conversion, which can cause additional processing and delays in delivering the image from the camera to your computer. Unfortunately, there are cases where automatic format switching cannot occur (such as when the operation requires grabbing into internal buffers, grabbing to a container ([`M_CONTAINER`](../../Reference/buf/MbufControlContainer.md)) or dynamic (**M_DYNAMIC**) buffer, and when performing Bayer conversion in the camera or when the camera is in chunk mode).

When automatic format switching cannot be performed, the image is copied (in the camera's current pixel format) into a compatible automatically-allocated internal buffer. The Host will then copy the internal buffer to the specified grab buffer to force the conversion to the format of the specified grab buffer.

Ideally, you should try to avoid automatic format switching, by inquiring the camera's current pixel format using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_SOURCE_DATA_FORMAT`](../../Reference/dig/MdigInquire.md) before allocating your grab buffers (that is, using [`MbufAlloc...`](../../Reference/buf/MbufAllocColor.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) and the returned value from [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_SOURCE_DATA_FORMAT`](../../Reference/dig/MdigInquire.md). Note that, the camera's current format typically varies from camera to camera.

## Gigabit Ethernet network adapter settings

If your Ethernet controller settings are not optimal, Aurora Imaging Capture Works flags these issues and proposes adjustments to enhance acquisition performance. The following settings can be modified.

- **Receive buffers**. Gigabit Ethernet network adapters use receive buffers, also known as receive descriptors, to store received packets. If the number of packets sent by your camera exceeds the number of available receive buffers, the additional packets will be dropped. The number of receive buffers assigned to your network adapter therefore impacts the reliability of your camera's image transmission. Typical bandwidth usage of GigE Vision cameras requires that a large number of receive buffers be assigned to your network adapter. It is recommended to adjust this setting to its maximum value (typically 2048 for Intel Gigabit Ethernet network adapters).
- **Packet size**. Network traffic is transmitted in packets, and the default maximum packet size for most network adapters is set to 1500 bytes. In situations where larger packets make up the majority of network traffic, such as with GigE Vision image streams, enabling jumbo packets can improve overall network performance by reducing CPU utilization. Jumbo packets typically support packet sizes up to 9014 bytes.
  > **Note:** Note that, if you want to use jumbo packets and you use an Ethernet switch in your network, you should also enable jumbo packets on your switch, since jumbo packet support is typically disabled by default.
- **Ethernet flow control**. Ethernet flow control can pause and restart the flow of packets from your camera. This reduces the chance of on-board memory becoming too full to properly deal with the flow of packets, and reduces the chance of data loss (dropped packets). A number of network adapters support Ethernet flow control. Enabling your network adapter's Ethernet flow control settings is recommended.
- **Interrupt moderation**. Interrupt moderation controls how often interrupts from the network adapter are generated and handled by the system. Setting the interrupt moderation value impacts the processing of received Ethernet frames; lower values typically result in faster processing but can increase CPU load, while higher values might introduce latency but improve CPU utilization by reducing the rate of interruptions. It is important to customize these settings to suit your application. It is important to evaluate the tradeoff between these for your specific application requirements.

To manually configure your network adapter with this information under Microsoft Windows, refer to the Microsoft Windows documentation.

## Optimizing further

If your Gigabit Ethernet network adapter requires further optimization (that is, its performance is still not adequate after modifying the network adapter settings), try changing the inter-packet delay.

Situations can occur that cause delays in the reception of packets by your network adapter. Inter-packet delays help by spreading packet transmission over time, giving the receiving network adapter enough time to deal with incoming packets and reduces the chance of an overworked receiving network adapter dropping packets.

For an example of how to calculate inter-packet delays, refer to the [Zebra Aurora Imaging Library](https://github.com/Zebra-Aurora-Imaging-Library) page in GitHub.
