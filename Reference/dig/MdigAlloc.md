---
doctype: Reference
module: dig
function: MdigAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dig / MdigAlloc"
---

# MdigAlloc

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Allocate a digitizer.

## Syntax

```c
AIL_ID MdigAlloc(
    AIL_ID             SystemId,    //in
    AIL_INT            DigNum,      //in
    AIL_CONST_TEXT_PTR DataFormat,  //in
    AIL_INT64          InitFlag,    //in
    AIL_ID *           DigIdPtr     //out
)
```

## Description

This function allocates a digitizer on the specified system so that it can be used by subsequent Aurora Imaging Library digitizer functions. A digitizer on the target system must be allocated to acquire data from a camera. Its device number specifies the acquisition path to allocate for the digitizer. Use [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md) to grab.

Typically, each digitizer only uses a single acquisition path. However, in some cases a digitizer might use several acquisition paths depending on the input format of the camera. Its digitizer configuration format (DCF) establishes the number of acquisition paths used.

Multiple digitizers can be allocated on the same system. If the system supports the possibility of independent acquisition paths, the digitizers' device number together with their DCF establishes if they represent dependent or independent acquisition path(s) on the system. If independent, grabs from these digitizers occur simultaneously. If dependent, grabs from these digitizers occur consecutively.

If multiple cameras of the same type are connected to the same acquisition path(s), instead of allocating multiple digitizers, you can allocate a single digitizer in the required format and then use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_CHANNEL`](../../Reference/dig/MdigControl.md) to switch between the cameras. [`M_CHANNEL`](../../Reference/dig/MdigControl.md) selects the active input channel of the digitizer. The switch occurs upon the next call to [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md) with the digitizer. The default input channel is determined by the selected DCF (generally, [`M_CH0`](../../Reference/dig/MdigControl.md)).

It is faster to allocate all the required digitizers first, perform the required grabs, and then free all the digitizers rather than allocate, grab, and then free each digitizer in succession.

Upon execution of this function, Aurora Imaging Library ensures that the digitizer is present before allocating it and generates an error if it is not. Note that the corresponding hardware won't necessarily be programmed at the time of allocation.

After allocating the digitizer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the digitizer identifier returned is not [`M_NULL`](../../Reference/dig/MdigAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dig/MdigAlloc.md) was specified).

When the digitizer is no longer required, release it using[`MdigFree`](../../Reference/dig/MdigFree.md)unless [`M_UNIQUE_ID`](../../Reference/dig/MdigAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/dig/MdigAlloc.md) was specified, the smart identifier manages the digitizer's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the digitizer.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `DigNum` *(in, AIL_INT)*

Specifies the number or string to identify the device to allocate for the digitizer.

*For specifying the device number*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default device number. This value is set during Aurora Imaging Library installation and using the Aurora Imaging Configurator utility. Note that the end-user can always change this value. |
| `M_GC_CAMERA_ID` | Specifies the camera (or device) to allocate for the digitizer. |
| `M_DEVn` | Specifies which acquisition path on your frame grabber, or which camera by rank when dealing with a grabberless interface camera (for example, a GigE Vision or USB3 Vision camera), to allocate for the digitizer.

You can inquire the total number of possible independent acquisition paths on a system using [`MsysInquire`](../../Reference/sys/MsysInquire.md)with [`M_DIGITIZER_NUM`](../../Reference/sys/MsysInquire.md). |

### `DataFormat` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of the digitizer configuration format (DCF) appropriate for your camera. Depending on the target system, different DCFs are supported.

*For monochrome or color cameras, or 3D sensors*

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies the default digitizer format. This value is set during Aurora Imaging Library installation and using the Aurora Imaging Configurator utility. Note that the end-user can always change this value. |
| `M_NULL` | Specifies the data format is set by the digitizer allocated as the multicast controller. Note that this value should only be used when allocating a multicast worker (using [`M_GC_MULTICAST_WORKER`](../../Reference/dig/MdigAlloc.md)).

For more information about multicasting and configuring your Zebra GigE Vision digitizer, refer to Zebra GigE Vision driver. |
| `"DCF File name"` | Specifies the path and file name of the DCF (for example: _"C:\mydirectory\myfile"_).

See the installed Drivers directory (for example, _C:\Program Files\Aurora Imaging Library\11\Drivers_ for the current list of supported formats.

When the path of the directory from which to retrieve the file is on a remote computer (under Distributed Aurora Imaging Library), precede the path with `"remote:///"` (for example, `"remote:///C:\mydirectory"`). |

*For simulated digitizers*

| Value | Description |
| --- | --- |
| `"Image path[@n]"` | Specifies the path of the directory from which to retrieve image files (or an AVI file) when allocating a simulated digitizer (for example, _"C:\mydirectory\"_), where _n_ is the frame rate; the information in brackets is optional (for example, _"C:\mydirectory\@30"_). For a list of image file formats supported, refer to [`MbufImport`](../../Reference/buf/MbufImport.md).

Note that, the X- and Y-size of each image in the path should be the same; otherwise, the X- and Y-size is taken from the first image and all subsequent images are either cropped to these dimensions, or expanded without zooming or stretching the original image, accordingly. In the latter case, portions of the previous image(s) might still be visible at the borders if the new image is smaller than the previous image.

When the path of the directory from which to retrieve the image files (or AVI file) is on a remote computer (under Distributed Aurora Imaging Library), precede the path with `"remote:///"` (for example, `"remote:///C:\mydirectory"`). |
| `"M_2D_SIMULATOR"` | Specifies to use Aurora Imaging Library's internal 2D pattern generator with the simulated digitizer. This generates a series of images of an arrow, which can be used to simulate acquisition without the need to create sample images. The generated images are 640x480, in the 8-bit RGB color format. |
| `"M_3D_SIMULATOR"` | Specifies to use Aurora Imaging Library's internal 3D pattern generator with the simulated digitizer. This generates a meshed point cloud of an arrow, which can be used to simulate acquisition without the need to create sample point clouds. |
| `"SDCF File name"` | Specifies the path and file name of the simulated DCF for use with a simulated digitizer (for example: _"C:\mydirectory\myfile"_).

When the path of the directory from which to retrieve the SDCF file is on a remote computer (under Distributed Aurora Imaging Library), precede the path with `"remote:///"` (for example, `"remote:///C:\mydirectory"`). |

### `InitFlag` *(in, AIL_INT64)*

Specifies the type of initialization to perform on the digitizer.

*For specifying the type of initialization*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_DEV_NUMBER` | Specifies to allocate the digitizer for the camera with the specified device number. To specify the device number, use the DigNum paramater ([`M_DEVn`](../../Reference/dig/MdigAlloc.md)). |
| `M_EMULATED` | Specifies to allocate the digitizer as a simulated digitizer. |
| `M_GC_DEVICE_IP_ADDRESS` | Specifies to allocate the digitizer for the camera with the specified IP address. To specify the IP address, use **M_GC_CAMERA_ID()**. |
| `M_GC_DEVICE_NAME` | Specifies to allocate the digitizer for the camera with the specified device's user name. To specify the device's user name, use **M_GC_CAMERA_ID()**. |
| `M_MINIMAL` | Specifies that grabbing is not permitted with the allocated digitizer. When performing a grab, using either [`MdigGrab`](../../Reference/dig/MdigGrab.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md), an error is generated.

To receive a callback when a camera is connected or disconnected, use [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_CAMERA_PRESENT`](../../Reference/dig/MdigHookFunction.md).

Up to 8 digitizers from [`M_DEV0`](../../Reference/dig/MdigAlloc.md) to [`M_DEV7`](../../Reference/dig/MdigAlloc.md) can be allocated simultaneously with this type of initialization. |

*For specifying further details*

| Value | Description |
| --- | --- |
| `M_GC_MANIFEST_ENTRY` | Specifies which camera's device description file (XML) to download from the camera's manifest table. Note that, by default the Aurora Imaging Library driver will always try to download a GenICam schema 1.1 compliant XML file. If no such file is present, then a GenICam schema 1.0 file is used. If manifest tables are not supported in the camera, then the single camera's device description file is downloaded. To skip this download, use [`M_GC_XML_DOWNLOAD_SKIP`](../../Reference/dig/MdigAlloc.md). |
| `M_GC_PACKET_SIZE_NEGOTIATION_SKIP` | Specifies that the packet size negotiation task is skipped. Instead, Aurora Imaging Library uses the GigE Vision digitizer's packet size register. If the packet size is too small or if the packet size is not supported by your digitizer, acquisition reliability will suffer. To force the packet size to a certain value, either store it in the DCF or use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_PACKET_SIZE`](../../Reference/dig/MdigControl.md). |
| `M_GC_XML_DOWNLOAD_SKIP` | Specifies that the camera's device description file (XML) should not be downloaded from the camera associated with the digitizer. The cached version, present in the Aurora Imaging Library XML repository is used instead. If the camera's device description file is not present in the Aurora Imaging Library XML repository or if the camera's device description file's name and/or size differs from the file in the Aurora Imaging Library XML repository, the camera's device description file will be downloaded and this flag is ignored.

> **Note:** Note that [`M_GC_XML_DOWNLOAD_SKIP`](../../Reference/dig/MdigAlloc.md) and [`M_GC_XML_FORCE_DOWNLOAD`](../../Reference/dig/MdigAlloc.md) are mutually exclusive, and cannot be used in combination with each other. |
| `M_GC_XML_FORCE_DOWNLOAD` | Specifies that the camera's device description file (XML) should be downloaded from the camera associated with the digitizer. The cached version, present in the Aurora Imaging Library XML repository is overwritten.

> **Note:** Note that [`M_GC_XML_DOWNLOAD_SKIP`](../../Reference/dig/MdigAlloc.md) and [`M_GC_XML_FORCE_DOWNLOAD`](../../Reference/dig/MdigAlloc.md) are mutually exclusive, and cannot be used in combination with each other. |

*For specifying the multicasting controller-worker relationship*

| Value | Description |
| --- | --- |
| `M_GC_MULTICAST_CONTROLLER` | Specifies that the allocated digitizer will be the controller digitizer in a multicasting controller-worker or monitor relationship. Assigning the allocated digitizer as a controller digitizer does not limit the digitizer in any way. In addition, as a multicast controller digitizer, it is responsible to program the camera's destination stream and message channels (if supported) with an administratively scoped IP multicast address. |
| `M_GC_MULTICAST_MONITOR` | Specifies that the allocated digitizer will be a special type of worker digitizer (a monitor digitizer) in a multicast controller-worker relationship. Assigning the allocated digitizer as a monitor digitizer limits its capabilities, since a monitor digitizer can only receive images and events (GenICam messages) from the camera and request packet resends, as necessary. In the controller-worker relationship, only the multicast controller digitizer has control. Therefore, the monitor digitizer's DCF that you pass [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) should only contain X-size, Y-size, and pixel format information.

Image acquisition is only performed when the controller digitizer (on another computer) initiates a grab and the GigE Vision camera streams the data. If the controller has not initiated the image acquisition, then when using [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md) with the monitor digitizer, the monitor digitizer will wait until the controller digitizer initiates the image acquisition.

Note that a monitor digitizer cannot be allocated if the GigE Vision camera is in **chunk mode**. |
| `M_GC_MULTICAST_WORKER` | Specifies that the allocated digitizer will be a worker digitizer in a multicast controller-worker relationship. Assigning the allocated digitizer as a worker digitizer limits its capabilities, since a worker digitizer can only read from the camera (using [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)), receive images and events (GenICam messages) from the camera, and request packet resends. In the controller-worker relationship, only the multicast controller digitizer has control. Therefore, the worker digitizer's DCF that you pass to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) is not used. For best effect, set the [`DataFormat`](../../Reference/dig/MdigAlloc.md) parameter to [`M_NULL`](../../Reference/dig/MdigAlloc.md).

Image acquisition is only performed when the controller digitizer (on another computer) initiates a grab and the GigE Vision camera streams the data. If the controller has not initiated the image acquisition, then when using [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), or [`MdigProcess`](../../Reference/dig/MdigProcess.md) with the worker digitizer, the worker digitizer will wait until the controller digitizer initiates the image acquisition. |

### `DigIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the digitizer identifier or specifies the data type that the function should use to return the digitizer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated digitizer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated digitizer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_DIG_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the digitizer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated digitizer.

If allocation fails, [`M_NULL`](../../Reference/dig/MdigAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the digitizer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DIG_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/dig/MdigAlloc.md) was specified).

## Remarks

> If you are creating a DLL that includes a call to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md), ensure that the call is not made from the `DllMain()` function, because [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) from an initialization function in your DLL instead.
