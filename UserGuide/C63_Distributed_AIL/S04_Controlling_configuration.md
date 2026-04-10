---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Controlling_configuration
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Controlling configuration"
---

# Controlling configuration

In the controlling configuration, a local computer allocates an Aurora Imaging Library application context and then allocates one or more Distributed Aurora Imaging Library remote systems. It can allocate multiple board-type and Host-type Distributed Aurora Imaging Library remote systems.

The controlling application can only allocate remote systems on computers running the Distributed Aurora Imaging Library server. The local computer runs the Aurora Imaging Library controlling application, which manages the connection and communication with the remote systems.

All resources in a controlling configuration cluster can be shared transparently (automatic connection), except when network address translation (NAT), port forwarding, or other address translation tools are used. If these tools are used, a remote system can only share resources with the controlling application.

A simple controlling configuration cluster is illustrated below:

*[Image: DAIL_simple_cluster_described.png]*

## Steps to create a Distributed Aurora Imaging Library controlling configuration

The following steps provide a basic methodology for creating a Distributed Aurora Imaging Library controlling configuration:

1. Allocate an Aurora Imaging Library application context, using [`MappAlloc`](../../Reference/app/MappAlloc.md).
2. If required, allocate a system on the local computer, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). If you don't specify a target computer when allocating a system, Aurora Imaging Library allocates the system locally on the current computer (local computer).
3. Allocate a Distributed Aurora Imaging Library remote system on each of the remote computers, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with the target remote computer's identifier (for example, IP address).
   > **Note:** Note that to allocate a Distributed Aurora Imaging Library remote system on the local computer (for example, to test your application) specify "localhost" or 127.0.0.1 as the computer identifier.
4. Call the Aurora Imaging Library functions to perform the required operations, specifying objects allocated on these Distributed Aurora Imaging Library remote systems. Based on the objects passed, Aurora Imaging Library will perform the functions on the most appropriate computer; if some of the objects are not on the selected computer, they will be copied to it. For this reason, Aurora Imaging Library functions are most efficient when operating on objects whose systems are allocated on the same computer.
5. Free all allocated objects using [`MappFree`](../../Reference/app/MappFree.md), unless [`M_UNIQUE_ID`](../../Reference/app/MappAlloc.md) was specified during allocation.
   > **Note:** Note that all allocated objects must be freed before freeing the Aurora Imaging Library system context ([`MsysFree`](../../Reference/sys/MsysFree.md)), which must be freed before freeing the Aurora Imaging Library application context ([`MappFree`](../../Reference/app/MappFree.md)).

Before you can run a Distributed Aurora Imaging Library application, you must prepare the local computer and each of the remote computers with the appropriate software. See [Preparing computers for Distributed Aurora Imaging Library](S03_Preparing_computers_for_Distributed_AIL.md).

## Allocating Distributed Aurora Imaging Library remote systems

Distributed Aurora Imaging Library remote systems are allocated in a similar manner as all Aurora Imaging Library systems, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) and specifying the type of Aurora Imaging Library system to allocate. The additional information needed to allocate a Distributed Aurora Imaging Library remote system is the name or identifier of the computer on which to allocate the remote system.

To pass this information, the [`SystemDescriptor`](../../Reference/sys/MsysAlloc.md) parameter of [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) must specify the type of system to allocate, as well as identify the target remote computer on the network. To override the default Distributed Aurora Imaging Library server connection port, [`SystemDescriptor`](../../Reference/sys/MsysAlloc.md) should also specify the required port. The full syntax for the [`SystemDescriptor`](../../Reference/sys/MsysAlloc.md) parameter is typically as follows:

```
"dailtcp://RemoteComputerIdentifier:PortNumber/SystemType" 
```

where _RemoteComputerIdentifier_ should be replaced with the remote computer's name or IP address; Aurora Imaging Library supports both IPv4 and IPv6 addresses. _PortNumber_ should be replaced with the port that the local computer should access on the remote computer to initiate new connections, unless the default server connection port is appropriate; in which case, omit the ":" and the port number. _SystemType_ should be replaced with any valid Aurora Imaging Library system type, such as [`M_SYSTEM_RAPIXOCL`](../../Reference/sys/MsysAlloc.md). The "://" and "/" are required separators.

In the following case, you don't need to explicitly specify a server connection port; the default matches the port to which the Distributed Aurora Imaging Library server is listening on the remote computer.

*[Image: DAIL_server_connection_listening_port.png]*

> **Code example:** [userguide.distributed_ail.allocating_dail_remote_systems01](userguide.distributed_ail.allocating_dail_remote_systems01)

Whereas in the following case, you must explicitly specify a server connection port because by default, it does not correspond to the port to which the Distributed Aurora Imaging Library server is listening. In this case, the server connection port must be set to 58000 to establish a connection.

*[Image: DAIL_server_connection_listening_port_explicit.png]*

> **Code example:** [userguide.distributed_ail.allocating_dail_remote_systems02](userguide.distributed_ail.allocating_dail_remote_systems02)

If you specify a server connection port, ensure that the specified port corresponds to the listening port on the target remote computer. The listening port can be configured using the Aurora Imaging Configurator utility on the target remote computer.

A typical IPv4 string has the format `n.n.n.n`, where _n_ is a number between 0 and 255. A typical IPv6 string has the format `x:x:x:x:x:x:x:x`, where _x_ is a hexadecimal number between 0000 and FFFF. If you are supplying an IPv6 address, you must use square brackets to separate the address from the port. For example:

```
"dailtcp://[x:x:x:x:x:x:x:x]:PortNumber/SystemType" 
```

### Allocating a remote system by default

You can set your local computer's default system to a Distributed Aurora Imaging Library remote system. This means that when you allocate a default system on your local computer, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`SystemDescriptor`](../../Reference/sys/MsysAlloc.md) set to [`M_SYSTEM_DEFAULT`](../../Reference/sys/MsysAlloc.md), you will automatically set up a Distributed Aurora Imaging Library controlling configuration with a connection to this default remote system.

Your local computer's default system is set using the Aurora Imaging Configurator utility. To reset your default system to a Distributed Aurora Imaging Library remote system, you must first register the remote system on your local computer. To register a remote system, open the Aurora Imaging Configurator utility and enter the network computer's name or IP address, then select a system on that remote computer. This is done in the **Registration** pane, found in **Distributed Aurora Imaging Library - Controlling**. Any single Aurora Imaging Library system registered on the local computer, including the registered remote systems, can be chosen as the default system using the Aurora Imaging Configurator utility on the **Default Values** pane, found under the **General** item.

> **Note:** Note that a remote computer has a default system of its own. You can allocate the remote computer's default system by setting the [`SystemDescriptor`](../../Reference/sys/MsysAlloc.md) parameter to `"dailtcp://RemoteComputerIdentifier/M_SYSTEM_DEFAULT"`.

## Execution of Aurora Imaging Library functions on remote systems

In the controlling configuration, Aurora Imaging Library decides where to execute an Aurora Imaging Library function based on the Aurora Imaging Library objects passed as parameters to the function. Therefore, to perform the required operations on a remote computer, allocate a Distributed Aurora Imaging Library remote system on this computer and then pass objects allocated on this remote system to the corresponding Aurora Imaging Library functions.

When all of the Aurora Imaging Library objects passed are allocated on the same Distributed Aurora Imaging Library remote system, Aurora Imaging Library executes the function on the computer (processor) associated with this system.

When the Aurora Imaging Library objects passed are allocated on different systems, Aurora Imaging Library establishes if there is a computer (processor) associated with one of the systems most suitable to execute the operation. To be suitable, Aurora Imaging Library must be able to temporarily copy all relevant Aurora Imaging Library objects to this computer. Only objects allocated with the Aurora Imaging Library Buffer module ([`Mbuf...`](../../Reference/buf/MbufAlloc1d.md)) can be copied to another computer (**portable**); other objects are not portable. If an Aurora Imaging Library object is not portable, the computer associated with this object is typically selected as the most suitable. If two of the Aurora Imaging Library objects are not portable and are on different computers, an error is generated.

If Aurora Imaging Library establishes that a remote computer is the most suitable to execute an Aurora Imaging Library function, the controlling application sends a command to this computer to execute the function. Then, using inter-system calls, the remote computer typically communicates directly with the other remote computers, to fetch a copy of any required Aurora Imaging Library objects located on these computers and to send back any changes. Inter-system calls improve performance when multiple remote computers are involved. If an object is located on the local computer, inter-system calls are not used; the local computer will perform the required copy operations.

Since additional copy operations are avoided, Aurora Imaging Library functions are most efficient when operating on objects whose systems are all allocated on the same computer.

## Remote displays

When you allocate a display on a Distributed Aurora Imaging Library remote system, you can select image buffers allocated on that system, to the display. By default, these buffers are displayed on the local computer.

*[Image: DAIL_local_remote_display01.png]*

> **Code example:** [userguide.distributed_ail.displays_and_dail_remote_systems01](userguide.distributed_ail.displays_and_dail_remote_systems01)

If you want to display the buffers on the remote computer, specify [`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md) when allocating the display. This is not supported for displays which display into a user-defined window.

*[Image: DAIL_local_remote_display02.png]*

> **Code example:** [userguide.distributed_ail.displays_and_dail_remote_systems02](userguide.distributed_ail.displays_and_dail_remote_systems02)

When displaying on the local computer and the overlay mechanism is enabled, the display's overlay buffer is superimposed on the displayed buffer before the transfer.

By default, the display is updated after each modification to the displayed buffer (or the overlay buffer), and the application waits for the data to be transferred to the display before continuing. When a buffer, located on a remote system, is displayed on the local computer, the update time can significantly slow down your application. To reduce delays, you can display in asynchronous mode using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_ASYNC_UPDATE`](../../Reference/disp/MdispControl.md). This mode uses multiple internal buffers to queue the updates and allows the application to continue once the update has been queued. This maximizes your bandwidth usage while minimizing processing delays.

To reduce transmission delays when the displayed buffer is modified, only the modified areas of the buffer are transmitted.

By default, Aurora Imaging Library sends updates from the remote system to the local computer as fast as possible, limited by the available bandwidth and transmission delay. However, when updating the display asynchronously, it is possible to set an upper limit to the number of updates to perform per second, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_UPDATE_RATE_MAX`](../../Reference/disp/MdispControl.md). When a limit is specified, you can assume that there will be a minimum delay of 1/[`M_UPDATE_RATE_MAX`](../../Reference/disp/MdispControl.md) between two consecutive updates. Between transmissions, updates are accumulated into one update. Alternatively, whether updating the display synchronously or asynchronously, you can use [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_UPDATE_RATE_DIVIDER`](../../Reference/disp/MdispControl.md) to skip updates. In this case, you specify after how many buffer modifications to update the display with the last modification, regardless of the time lapsed. This is especially useful when performing continuous grabs. When using [`M_UPDATE_RATE_DIVIDER`](../../Reference/disp/MdispControl.md), if fewer than the specified number of buffer modifications occur, the display will not be updated; whereas with [`M_UPDATE_RATE_MAX`](../../Reference/disp/MdispControl.md), the display will eventually be updated.

To reduce bandwidth usage, it is also possible to use compression so that the amount of data transmitted is minimal. You can activate compression using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_COMPRESSION_TYPE`](../../Reference/disp/MdispControl.md). When enabled, data is compressed before being transmitted. The best compression rate is obtained with [`M_JPEG_LOSSY`](../../Reference/disp/MdispControl.md). The [`M_JPEG2000_LOSSY`](../../Reference/disp/MdispControl.md) and [`M_JPEG2000_LOSSLESS`](../../Reference/disp/MdispControl.md) compression types support much higher ratios of compression without compromising image quality. However, you should avoid the JPEG2000 compression types unless absolutely necessary since compression/decompression time is high; you should only use them when the benefits of compression outweigh the overhead associated with the compression/decompression. For example, you might need to use the JPEG2000 compression types when there are fine details in your image or overlay buffer. For both JPEG and JPEG2000 lossy compression types, you can manually adjust the compression factor by adjusting the quantization factor, using [`MdispControl`](../../Reference/disp/MdispControl.md) with [`M_Q_FACTOR`](../../Reference/disp/MdispControl.md). By default, the quantization factor for the compression is set to 80, but you can specify an integer value in the range of 1 to 99. The higher the factor, the more the compression, but the image quality suffers.

To change the default settings, you can use the Aurora Imaging Configurator utility on the remote computer. If you select the **High quality** display option in the Aurora Imaging Configurator utility, the display is updated synchronously and compression is disabled by default; if you select the **Optimized for bandwidth usage** display option, the display is updated asynchronously and [`M_JPEG_LOSSY`](../../Reference/disp/MdispControl.md) compression is used by default.

## Default values on remote systems

When a function is executed on a remote computer and you select **M_DEFAULT** (or any local defaults such as [`M_SYSTEM_DEFAULT`](../../Reference/sys/MsysAlloc.md)), the default value set up on that remote computer is used. The default value could have been setup when installing Aurora Imaging Library on that computer or using the Aurora Imaging Configurator utility on that computer.

As mentioned above, the only exception is when allocating a display. When allocating a display on a remote system, the display appears, by default, on the local computer. The local computer's default video configuration format (VCF) is the format used. If you specify to have it appear on a remote computer ([`M_REMOTE_DISPLAY`](../../Reference/disp/MdispAlloc.md)), the default VCF of the remote computer is used.

## Files on remote systems

When a function takes a file name and is executed on the remote computer, Aurora Imaging Library assumes that the file is on the local computer. To specify a file on the remote computer, prefix the specified file name with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`).

## Asynchronous calls

Most functions executed on a remote computer are executed asynchronously. The function execution request is sent to the remote computer and the local call returns immediately, without waiting for the request to complete. Except for functions that return values (for example, allocation and inquire functions), almost all other functions are asynchronous. Asynchronous calls are queued on the remote computer and processed in the order that they were issued on a thread-by-thread basis.

## Multi-threading

The controlling configuration supports multi-threading. Each thread in the controlling application has a corresponding thread on the remote system. All requests made on a specific thread in the controlling application are executed on the same corresponding thread on the remote system; the sequence of calls in a thread is respected. Threads on the remote system are independent and parallelism in the controlling application is respected.

## Executing a user-defined function on the remote system

When time is of essence and you have a series of functions that need to be executed on a remote computer and some are synchronous, you can create an Aurora Imaging Library user-defined function that is executed on the remote computer and that calls the required series of functions.

*[Image: DAIL_user_defined_function.png]*

This avoids unnecessary communication with the local computer. To develop such an Aurora Imaging Library user-defined function, see [The Aurora Imaging Library function development module](../C68_The_AIL_function_development_module/ChapterInformation.md).
