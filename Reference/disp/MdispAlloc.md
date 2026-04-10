---
doctype: Reference
module: disp
function: MdispAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispAlloc"
---

# MdispAlloc

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Allocate a display.

## Syntax

```c
AIL_ID MdispAlloc(
    AIL_ID             SystemId,     //in
    AIL_INT            DispNum,      //in
    AIL_CONST_TEXT_PTR DispFormat,   //in
    AIL_INT64          InitFlag,     //in
    AIL_ID *           DisplayIdPtr  //out
)
```

## Description

This function allocates a display on the specified system so that it can be used by subsequent Aurora Imaging Library display functions. Use [`MdispSelect`](../../Reference/disp/MdispSelect.md) to select the image buffer to display. Note that the buffer and the display should be allocated on the same system.

In general, the display is presented on the computer running the main Aurora Imaging Library application. However, if you are developing a Distributed Aurora Imaging Library application or an Aurora Imaging Web Library server application, you can allocate a display to be shown on a remote computer.

If you are developing a Distributed Aurora Imaging Library controlling application and you allocate a display on a Distributed Aurora Imaging Library remote system, you can select image buffers allocated on that remote system to the display. By default, these buffers are displayed on the local computer. However, to display the buffers on the remote computer, combine [`M_EXCLUSIVE`](../../Reference/disp/MdispAlloc.md) or [`M_WINDOWED`](../../Reference/disp/MdispAlloc.md) with [`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md) when allocating the display. This type of display is referred to as a remote display.

If you are developing an Aurora Imaging Library application that also acts as an Aurora Imaging Web Library server, you can allocate a display that can be presented in one or more instances of an Aurora Imaging Web Library client application (for example, running in a compatible web browser). To do so, use [`M_WEB`](../../Reference/disp/MdispAlloc.md) when allocating the display. This type of display is referred to as an Aurora Imaging Web Library display.

After allocating the display, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the display identifier returned is not [`M_NULL`](../../Reference/disp/MdispAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/disp/MdispAlloc.md) was specified).

When the display is no longer required, release it using[`MdispFree`](../../Reference/disp/MdispFree.md)unless [`M_UNIQUE_ID`](../../Reference/disp/MdispAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/disp/MdispAlloc.md) was specified, the smart identifier manages the display's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to allocate the display. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `DispNum` *(in, AIL_INT)*

Specifies the device to use for image display.

*For specifying the display device*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will use the most appropriate device for display purposes. If your imaging board has a display section, and it is available, Aurora Imaging Library will typically use it for display purposes. |

*For specifying the position of the device to use for an exclusive display*

| Value | Description |
| --- | --- |
| `M_BOTTOM` | Specifies to use the bottom-most device for an exclusive display. Note that this option cannot be combined with [`M_TOP`](../../Reference/disp/MdispAlloc.md). |
| `M_CENTER` | Specifies to use the center device for an exclusive display. |
| `M_LEFT` | Specifies to use the left-most device for an exclusive display. Note that this option cannot be combined with [`M_RIGHT`](../../Reference/disp/MdispAlloc.md). |
| `M_RIGHT` | Specifies to use the right-most device for an exclusive display. Note that this option cannot be combined with [`M_LEFT`](../../Reference/disp/MdispAlloc.md). |
| `M_TOP` | Specifies to use the top-most device for an exclusive display. Note that this option cannot be combined with [`M_BOTTOM`](../../Reference/disp/MdispAlloc.md). |

### `DispFormat` *(in, AIL_CONST_TEXT_PTR)*

Specifies the format of the display. For windowed displays, this parameter has no effect and should be set to [`"M_DEFAULT"`](../../Reference/disp/MdispAlloc.md). For exclusive displays, this parameter specifies the screen resolution.

*For specifying the default display format*

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies the default display format. For windowed displays, this is the only supported setting. For exclusive displays, this specifies the default video configuration format (VCF), which can be set during installation and later modified using the Aurora Imaging Configurator utility (Defaults tab). |

*For exclusive displays*

| Value | Description |
| --- | --- |
| `M_CURRENT_RESOLUTION` | Specifies to use the current resolution of the screen. This option is only available for an [`M_EXCLUSIVE`](../../Reference/disp/MdispAlloc.md) display. |
| `"vcf-file-name.vcf"` | Specifies the name of the video configuration format (VCF) that defines the resolution and refresh rate to use. The file typically has a VCF extension. The drive, directory, and name of the file must be specified, for example: _"C:\mydirectory\myfile"_.

For a file on the remote computer (under Distributed Aurora Imaging Library), the path and file name must be preceded by `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`).

The file name string can be specified in, for example, the "1024x768x32@60hz.vcf" format. For the current list of video configuration format (VCF) files, see the installed VCF folder, for example, _C:\Program Files\Aurora Imaging Library\11\Drivers\Host\vcf_. |

### `InitFlag` *(in, AIL_INT64)*

Specifies your display's type. This parameter can be set to one of the following:

*For specifying the display type*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default display type, which can be set using the Aurora Imaging Configurator utility. Typically, the default display type is [`M_WINDOWED`](../../Reference/disp/MdispAlloc.md). |
| `M_EXCLUSIVE` | Specifies to present the image buffer selected for display full-screen, without a windowed border, in one of Windows desktop screens. The image buffer is presented at the center of the screen.

To use an exclusive display, your Windows desktop should be using more than one screen. Allocating an exclusive display on the main screen might not be convenient; if you do so, the Aurora Imaging Library exclusive display will hide third-party applications designed to start on the main screen and hide the Windows taskbar.

> **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |
| `M_WEB` | Specifies to present the image buffer selected for display in one or more instances of an Aurora Imaging Web Library client application (for example, running in a compatible web browser).

To use an Aurora Imaging Web Library display, you must enable Aurora Imaging Web Library server functionality in your application using [`MappControl`](../../Reference/app/MappControl.md)with [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md). You must also publish the display using [`MobjControl`](../../Reference/obj/MobjControl.md)with[`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md).

> **Note:** An Aurora Imaging Web Library display cannot be presented by your Aurora Imaging Library application. It can only be presented by a connected Aurora Imaging Web Library client application (running on the local computer or a remote computer).

> **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |
| `M_WINDOWED` | Specifies to present the image buffer selected for display in its own window on the Windows desktop screen(s). The display window is tracked and updated with the image buffer selected for display; that is, if the window moves or is occluded, the window is updated with the image buffer accordingly.

> **Note:** This display type can be used with [`MdispSelect`](../../Reference/disp/MdispSelect.md) and [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) to display images, unless combined with [`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md). A remote display requires[`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |
| `M_WPF` | Specifies to present the image buffer selected for display using an AILWPFDisplay control. See [Using WPF with Aurora Imaging Library](../../UserGuide/C60_Using_AIL_with_NET/S09_Using_WPF_with_AIL.md) in the User Guide for details of how to use.

> **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |

*For controlling where and how the display is allocated*

| Value | Description |
| --- | --- |
| `M_REMOTE_DISPLAY` | Specifies that the display is displayed on the remote computer in a Distributed Aurora Imaging Library application. This setting is only available when the display is allocated on a remote system. If the display is allocated on a remote system but this setting is not specified, the display is presented on the local computer.

To present the display on the remote computer, the Distributed Aurora Imaging Library server cannot be running as a service; you must start it manually. To do so, assuming that Distributed Aurora Imaging Library has already been installed during the setup, you must logon to the remote computer and, from the Aurora Imaging Configurator utility, push the **Start Server** button, found in the **Server Settings** pane under the **Distributed AIL** item. For more information, see [Setting up the Distributed Aurora Imaging Library server on remote computers](../../UserGuide/C63_Distributed_AIL/S03_Preparing_computers_for_Distributed_AIL.md).

> **Note:** This display type requires [`MdispSelect`](../../Reference/disp/MdispSelect.md) to display images. |

### `DisplayIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the display identifier or specifies the data type that the function should use to return the display identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated display; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated display; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_DISP_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the display (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address for the display identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated display.

If allocation fails, [`M_NULL`](../../Reference/disp/MdispAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the display identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_DISP_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/disp/MdispAlloc.md) was specified).
