---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Mandatory_allocations_allocating_an_application_and_its_systems
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Mandatory allocations allocating an application and its systems"
---

# Mandatory allocations: allocating an application context and its systems

At the beginning of each application, you must:

1. Allocate an Aurora Imaging Library application context using [`MappAlloc`](../../Reference/app/MappAlloc.md). This initializes Aurora Imaging Library.
2. Allocate an Aurora Imaging Library system using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). An Aurora Imaging Library system typically represents a group of hardware resources capable of grabbing, storing, processing, and/or displaying images.
   *[Image: AppFlow.png]*

At the end of each application, free each system using [`MsysFree`](../../Reference/sys/MsysFree.md), and then free the application context using [`MappFree`](../../Reference/app/MappFree.md).

## Aurora Imaging Library application context

An Aurora Imaging Library application context is a data structure (specifically an Aurora Imaging Library object) populated with the information needed to run an Aurora Imaging Library application. You must allocate an application context before calling any other Aurora Imaging Library function (using [`MappAlloc`](../../Reference/app/MappAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md)). Upon allocating an application context, it is initialized with the information required by Aurora Imaging Library. This initialization determines the type of CPU and CPU cores available, the licenses available which depend on the version of Aurora Imaging Library installed, and the files/functions available based on the licenses. Furthermore, it sets up an Aurora Imaging Library identification table that will hold the identifiers of all objects. It initializes how errors are displayed ([`MappAlloc`](../../Reference/app/MappAlloc.md) with [`M_QUIET`](../../Reference/app/MappAlloc.md) to suppress the display of error messages), and initializes and controls the creation of trace logs ([`MappAlloc`](../../Reference/app/MappAlloc.md) with [`M_TRACE_LOG_DISABLE`](../../Reference/app/MappAlloc.md) to disable the creation of trace logs).

## Aurora Imaging Library system

An Aurora Imaging Library system is an object that represents a group of hardware resources that Aurora Imaging Library can access and use. You must allocate at least one system, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md). Upon allocating an Aurora Imaging Library system, Aurora Imaging Library gathers information about the specified hardware and opens communication between your application and the appropriate hardware drivers. Once communication has been established, you can allocate and then use the system's hardware resources. Hardware resources might include memory ([`MbufAlloc...`](../../Reference/buf/MbufAlloc2d.md)), display ([`MdispAlloc`](../../Reference/disp/MdispAlloc.md)), and digitizers ([`MdigAlloc`](../../Reference/dig/MdigAlloc.md)).

> **Note:** See the [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) function reference and the _Aurora Imaging Library Hardware-specific Notes_ for the list of supported Aurora Imaging Library systems and the hardware resources that they require.

### Categories of Aurora Imaging Library systems

Aurora Imaging Library systems can be divided into three general categories with different capabilities: the Host system (which does not require specific hardware), acquisition-type systems (which provide access to image grabbing hardware), and non-acquisition-type systems (which provide access to Zebra hardware that only has non-image-grabbing functionality, such as I/O signaling).

The following table summarizes which Aurora Imaging Library systems fall into each category, what capabilities they have, and what specific hardware they require. Aurora Imaging Library systems that support processing usually use the Host CPU and memory.

| Category of Aurora Imaging Library Systems | Processing/display | Grabbing | Specific hardware requirements |
| --- | --- | --- | --- |
| Host system | Yes | Simulated using images from files | No |
| Acquisition-type system | Yes | Yes | Yes |
| Zebra frame grabber-specifc system (for example, Aurora Imaging Library Rapixo CXP system) | Specific Zebra frame grabber (for example, Zebra Rapixo CXP) |
| Aurora Imaging Library GigE Vision system | Gigabit Ethernet port (or faster) |
| Aurora Imaging Library USB3 Vision system | USB 3.0 port |
| Aurora Imaging Library GenTL Consumer system | Depends on GenTL Producer | Device that is a GenTL Producer |
| Non-acquisition-type system | No | No | Yes |
| Aurora Imaging Library Indio system | Zebra Indio |
| Aurora Imaging Library Concord PoE system | Zebra Concord PoE |

Typically, a single system represents a single board in your computer (and some Host hardware resources). However, a few types of Aurora Imaging Library systems represent all boards (hardware components) of that specific type in a computer. For example, a single Aurora Imaging Library GigE Vision system represents all GigE Vision adapters in your computer. This means that once you allocate a GigE Vision system, you can allocate and grab from any connected GigE Vision camera (or other device). Additionally, different functionality on one board might require the allocation of multiple different types of Aurora Imaging Library systems. For example, to use the functionality of a Zebra Indio board, you allocate a GigE Vision system to access and grab images from a GigE Vision camera (or other device), and you allocate an Aurora Imaging Library Indio system to use all other functionality on the board (for example, the auxiliary I/O signals). For information, refer to the _Aurora Imaging Library Hardware-specific Notes_ for your particular hardware.

### Multiple systems

You can allocate more than one system per application context and then use their identifiers to access their hardware resources. All currently supported Aurora Imaging Library systems also support multi-processing. This means that the same board can be allocated by more than one process simultaneously; CPU and memory resources will be split between each process as needed. However, many resources (such as individual digitizers on some boards) cannot be allocated by more than one process simultaneously. Refer to the _Aurora Imaging Library Hardware-specific Notes_ for your particular hardware to determine which resources cannot be allocated by more than one process simultaneously.

Processing can be done using the resources of the different systems within a single process. Any operation involving more than one system will be performed by the most appropriate one. By default, if none of these systems is more appropriate than the Host, the Host is used to perform the operation.

### Remote systems

During Aurora Imaging Library system allocation, you can specify that the hardware resources are on a remote computer. This allocates a remote system, which makes the resources of the specified computer available: its specified Zebra imaging board (or third-party hardware), its CPU, and its memory. Both the local and remote computers must have Aurora Imaging Library/Aurora Imaging Library Lite installed with the Distributed Aurora Imaging Library option.

An Aurora Imaging Library application that uses remote systems is called a Distributed Aurora Imaging Library controlling application, and the remote systems are called Distributed Aurora Imaging Library remote systems. In a Distributed Aurora Imaging Library controlling application, multiple Distributed Aurora Imaging Library remote systems can collaborate with each other and with local Aurora Imaging Library systems across a network (for example, a local area network, or a wide area network such as the internet) controlled by a single application. A group of computers interacting using Distributed Aurora Imaging Library is called a cluster.

When developing a Distributed Aurora Imaging Library controlling application that uses functionality only available in the full version of Aurora Imaging Library, the local computer requires an Aurora Imaging Library development license; the remote computer(s) only require appropriate runtime licenses for Distributed Aurora Imaging Library and the modules that they actually use. When only running such an application, all computers in the cluster require appropriate runtime licenses for Distributed Aurora Imaging Library and the Aurora Imaging Library modules that they actually use; sending a command to another computer is not using a module so no specific license is needed in this case.

When developing or running a Distributed Aurora Imaging Library application that uses only Aurora Imaging Library Lite functionality, the local and remote computers require a Distributed Aurora Imaging Library supplemental license.

For more information on Distributed Aurora Imaging Library, see [Distributed Aurora Imaging Library](../C63_Distributed_AIL/ChapterInformation.md).
