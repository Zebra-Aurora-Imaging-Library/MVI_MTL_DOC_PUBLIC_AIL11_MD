---
doctype: BoardSpecificNotes
part: ""
chapter: GenTL
section: GenTL_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / gentl / GenTL specific features"
---

# Zebra GenTL Consumer (library) overview

The Zebra GenTL Consumer (library) allows you to use a third-party GenTL Producer (library) with Aurora Imaging Library. The Zebra GenTL Consumer (based on the GenICam GenTL standard) provides a software-generic way to communicate with one or more third-party devices through multiple interface technologies (such as, using GigE Vision, Camera Link, USB3 Vision, or a custom/non-standard interface to connect the camera and the Host).

*[Image: bsn_gentl_consumer_producer.png]*

The Zebra GenTL Consumer requires you to first install a third-party, vendor-supplied, standard compliant, GenTL Producer. Once installed, the Aurora Imaging Configurator utility lists the available GenTL Producer libraries. It is possible that multiple Producer libraries can access the same device (camera). For example, if you install multiple GenTL Producer libraries for GigE Vision-compliant devices (from different vendors), it is possible that any one of them can be used to grab from a GigE Vision-compliant device.

A GenTL Producer is a software library implementing the GenTL interface to enable an application or a software library to access and configure hardware in a generic way (for example, to stream image data from a camera). The Aurora Imaging Library GenTL Consumer is a software library that communicates with multiple GenTL Vision-compliant devices via the specified GenTL Producer.

You can install more than one GenTL Producer on your computer. These libraries are indexed and sorted by Aurora Imaging Library. You can inquire the indices of the installed GenICam GenTL Producers in the Aurora Imaging Configurator utility, or inquire the count of different GenTL Producers installed using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_GENTL_PRODUCER_COUNT`](../../Reference/app/MappInquire.md).

If you are using a GigE Vision or USB3 Vision-compliant device, or a camera connected to a Zebra frame grabber, you should use the native library for maximum performance (rather than the Zebra GenTL Consumer). The Zebra GenTL Consumer should be used with devices (cameras) that use a third-party, proprietary communication protocol, and for which the vendor has supplied a GenTL Producer.

## Support features and standards

The Zebra GenTL driver supports GenTL version 1.6. It also supports **M_DYNAMIC** and **M_CONTAINER** grab buffers.

The Zebra GenTL driver supports the following standards:

- GenICam GenTL Standard version 1.6.
- GenICam GenApi version 3.4.2.
- GenICam GenDC Standard version 1.1.
- GenICam Standard Feature Naming Convention (SFNC) version 2.7.
