---
doctype: BoardSpecificNotes
part: ""
chapter: USB3_Vision
section: Additional_USB3_Vision_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / usb3vision / Additional USB3 Vision notes User guide"
---

# Miscellaneous user guide information for Zebra USB3 Vision

This section includes additional user guide information for Zebra USB3 Vision. The information found in this section might be a reiteration of content previously documented.

## Supported standards

The Zebra USB3 Vision driver supports the following standards:

- GenICam GenApi version 3.4.2.
- GenICam Standard Feature Naming Convention (SFNC) version 2.7.
- GenCP 1.1.
- USB3 Vision 1.1.

## Additional features

The following are additional features supported for Zebra USB3 Vision:

- Grab containers.
- **M_DYNAMIC** buffers.
- Device enumeration API.
- Streaming Monochrome and Bayer data of 10- and 12 bit packed pixel format.

The Zebra USB3 Vision driver allocates 10 auxiliary buffers during digitizer allocation.

Aurora Imaging Library USB3 Vision system does not require Aurora Imaging Library's non-paged memory for image acquisition.

## Particularities

An error message is generated if [`MdigControl`](../../Reference/dig/MdigControl.md) is executed with **M_WHITE_BALANCE** and **M_CALCULATE** while a grab is in progress.

In the case where a synchronization lost occurs, the driver automatically attempts to resynchronize the image stream with the camera. This resets the BlockID to 0.

If two cameras are used and **M_DEV0** camera is removed from the PC, the**M_DEV1** camera will be remapped to **M_DEV0**, as if **M_DEV1** and **M_DEV0** were not allocated on their associated cameras.

BGR or RGB, packed (10 or 12 bits) pixel formats are not supported.

FLIR (Point Grey) cameras with old firmware on Windows 10 need to be unplugged before uninstalling Aurora Imaging Library. Failure to do so can cause a variety of post uninstall USB problems.

Your camera must be USB3 Vision compliant for it to work with the Zebra USB3 Vision driver. For more information, refer to [automate.org/vision/vision-standards/vision-standards](https://www.automate.org/vision/vision-standards/vision-standards).
