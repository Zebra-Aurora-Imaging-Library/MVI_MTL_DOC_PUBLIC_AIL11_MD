---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: The_digitizer_number
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / The digitizer number"
---

# Device number

In addition to the data format, [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) requires that you specify the number or string to identify the acquisition path(s) to allocate for a digitizer. Typically, only one acquisition path is required to grab from a camera. Note that a connection to a grabberless interface camera is considered an acquisition path.

Typically, each acquisition path can only be associated with one digitizer at a time. In these cases, if the specified acquisition path is not available (for example, because no camera is connected, or because the acquisition path is already allocated), the allocation fails and an error is generated. Note that some older frame grabbers did not have this restriction and would perform grabs from these digitizers sequentially.

## Using a frame grabber

When using a camera connected to a frame grabber, use [`M_DEVn`](../../Reference/dig/MdigAlloc.md) (where _n_ is the device number) to specify which acquisition path(s) on the frame grabber to allocate for the digitizer. Typically, this corresponds to the index of the connector on the frame grabber.

> **Note:** For a CXP camera that uses multiple connections, you set the device number of the digitizer to the index of the connector used for the main connection. The other connections (acquisition paths) used for the camera are associated with the digitizer automatically.

### Simultaneous acquisition

With most Zebra frame grabbers, you can connect multiple cameras and grab from all connected cameras simultaneously. To grab from several sources simultaneously, you must allocate an independent digitizer for each camera using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) and then make multiple calls to [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md) with the identifiers of the digitizers.

Refer to the _Aurora Imaging Library Hardware-specific Notes_ for more information regarding the number of cameras from which you can acquire data simultaneously using your Zebra frame grabber.

> **Note:** For some older frame grabbers, multiple connections could be associated with the same acquisition path. In these cases, each camera must be connected to a different acquisition path to allow simultaneous grabbing.

## Using a grabberless interface camera

A grabberless interface camera is an all encompassing term for any camera whose physical interface does not require a frame grabber. For example, Aurora Imaging Library supports GigE Vision compliant (network) cameras and USB3 Vision compliant cameras. Aurora Imaging Library can support other types of grabberless interface cameras if they are distributed with a standard-compliant GenTL Producer (library).

When using a grabberless interface camera, you can use the camera name or device number (the camera's rank, determined when Aurora Imaging Library enumerates the cameras that are visible from the operating system of your computer) to identify the camera to allocate for the digitizer. For GigE Vision cameras, you can also use the IP address to identify the camera to allocate for the digitizer.

All GigE Vision cameras can transmit data to the Host independently of each other. However, completion of a grab from a GigE Vision camera might be delayed due to the bandwidth limitations of your network, packet size limitations, and/or network adapter interrupt moderation settings. While GigE Vision cameras can be used over any Ethernet network, you can guarantee that a camera has the maximum bandwidth by connecting it directly to a network adapter.

> **Note:** Since the camera's classification depends on the standards of communication, additional information on grabberless interface cameras can be found in the Aurora Imaging Library Hardware-specific Notes section of the Aurora Imaging Library Help.

## Using a Zebra Iris smart camera

For a Zebra Iris smart camera, the device number is always [`M_DEV0`](../../Reference/dig/MdigAlloc.md).
