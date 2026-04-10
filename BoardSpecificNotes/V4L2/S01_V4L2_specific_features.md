---
doctype: BoardSpecificNotes
part: ""
chapter: V4L2
section: V4L2_specific_features
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / v4l2 / V4L2 specific features"
---

# Zebra Video4Linux2 Consumer (library) overview

The Zebra Video4Linux2 Consumer (library) allows you to use a Video4Linux2 (V4L2) driver with Aurora Imaging Library. The Zebra Video4Linux2 Consumer (based on the Linux Video4Linux2 API) provides a way to communicate with any device capable of video capture (provided it has a V4L2 driver working within the running Linux kernel) and can control the capture process and move images from the driver into the user space. The Zebra Video4Linux2 Consumer allows for access and configuration of devices including video input, video output, and encoding for capture devices.

*[Image: bsn_v4l2.png]*

Many drivers come with the Linux kernel, but a hardware device/camera manufacturer could distribute one separately. Make sure that the required/separately distributed V4L2 driver is either installed by the software provided with the device, or by installing a compatible V4L2 driver.

Note that not all V4L2 image formats are supported by Aurora Imaging Library, and some cameras might not have any Aurora Imaging Library-supported image formats. When using cameras with auto-focus, auto-lighting, or similar features, you might need to grab a few frames before obtaining a clear picture.
