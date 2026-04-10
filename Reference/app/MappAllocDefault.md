---
doctype: Reference
module: app
function: MappAllocDefault
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappAllocDefault"
---

# MappAllocDefault

> Allocate default Aurora Imaging Library objects.

## Syntax

```c
void MappAllocDefault(
    AIL_INT  InitFlag,         //in
    AIL_ID * ContextAppIdPtr,  //out
    AIL_ID * SysIdPtr,         //out
    AIL_ID * DispIdPtr,        //out
    AIL_ID * DigIdPtr,         //out
    AIL_ID * ImageBufIdPtr     //out
)
```

## Description

[`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) is implemented as a macro. This macro can allocate and set up the requested objects using the defaults specified during allocation and using the Aurora Imaging Configurator utility. This macro can allocate and initialize an application context, and allocate the default system. On this system, [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) can allocate the default digitizer and display, and allocate and clear a default displayable image buffer.

The installation customizes the default setup. After installation, if you want to review or change the default settings of [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md), use the Aurora Imaging Configurator utility.

Note, if allocating the default digitizer and the default DCF, selected using the Aurora Imaging Configurator utility, is for a 3-band color (RGB) camera, then the default image buffer will be 3-band if allocated; otherwise, it will be 1-band if allocated. The size of a default image buffer is 640x480, in pixels.

Note, upon execution of this function, a default 2D graphics context is automatically allocated. This default 2D graphics context can be used in function calls by specifying [`M_DEFAULT`](../../Reference/3dmap/M3dmapDraw.md) wherever a 2D graphics context identifier is required.

When you have finished using the allocated objects, you must free them using [`MappFreeDefault`](../../Reference/app/MappFreeDefault.md). With multiple threads, you must free the allocated application context (using [`MappFreeDefault`](../../Reference/app/MappFreeDefault.md)) in the thread in which it was allocated.

## Parameters

### `InitFlag` *(in, AIL_INT)*

Specifies the type of initialization setup to perform and is used principally to initialize the default system. This parameter can be set to one of the following:

*For specifying the type of initialization setup*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPLETE` *(default)* | Specifies to initialize the system completely; the system is initialized to its default state and any required resident software is downloaded. At least one complete initialization is necessary after you power-up your system. |

*For specifying a cluster node value*

| Value | Description |
| --- | --- |
| `M_CLUSTER_NODE` | Specifies a cluster node value when none is specified with a cluster manager. A Distributed Aurora Imaging Library publishing application requires a cluster node value that identifies the publishing application. If you have not specified a cluster manager for this application, ensure that each publishing application in the cluster has a unique, non-zero value.

> **Note:** Note that a cluster manager automatically assigns a cluster node value to an application. It is the recommended method of managing cluster node values. |

### `ContextAppIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the application context identifier. Upon execution of this function, the application context is allocated and its identifier returned. Instead of using [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md), you can use [`MappAlloc`](../../Reference/app/MappAlloc.md) to allocate an application context. Note, an application context must be allocated to allocate any other object.

### `SysIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the system identifier. Upon execution of this function, the default system is allocated and its identifier returned. Instead of using [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md), you can use [`MsysAlloc`](../../Reference/sys/MsysAlloc.md)to allocate a system. [`MappAlloc`](../../Reference/app/MappAlloc.md) will also allocate a default Host system. Note, a system must be allocated to allocate any other objects on it (display, digitizer or data buffers).

### `DispIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the identifier of the default display. If this parameter is set to `M_NULL`, a display is not allocated; otherwise, the default display is allocated and its identifier returned.

### `DigIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the identifier of the default digitizer. If this parameter is set to `M_NULL`, a digitizer is not allocated; otherwise, the default digitizer is allocated and its identifier returned.

### `ImageBufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the identifier of the default image buffer. If this parameter is set to `M_NULL`, an image buffer is not allocated; otherwise, the default image buffer is allocated and its identifier returned. It is then cleared and displayed on the system's display screen.

For example, a typical setup for a Zebra board in its power-up state with one input device and one default image buffer (full-screen size) selected on the default display is:

> **Code example:** [reference.MappAllocDefault.example01](reference.MappAllocDefault.example01)

If, for example, you don't need to acquire data from the camera but want to perform the rest of the above setup, you would make the following call:

> **Code example:** [reference.MappAllocDefault.example02](reference.MappAllocDefault.example02)
