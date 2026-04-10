---
doctype: Reference
module: app
function: MappAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappAlloc"
---

# MappAlloc

> Allocate an application context.

## Syntax

```c
AIL_ID MappAlloc(
    AIL_CONST_TEXT_PTR ServerDescription,  //in
    AIL_INT64          InitFlag,           //in
    AIL_ID *           ContextAppIdPtr     //out
)
```

## Description

This function allocates an Aurora Imaging Library application context. An application context must be allocated prior to using any other function.

In multi-thread environments, an application context is allocated in the main thread and is shared by all threads. [`Mapp...`](../../Reference/app/MappAlloc.md) function calls from any thread apply to all threads, unless specifically localized to that thread (typically by specifying [`M_THREAD_CURRENT`](../../Reference/app/MappControl.md) when calling the function).

In addition, a default 2D graphics context is also allocated upon allocation of an application context. You can use this default 2D graphics context in graphic function calls by specifying [`M_DEFAULT`](../../Reference/3dmap/M3dmapDraw.md) wherever a 2D graphics context identifier is required. In multi-thread applications, a default 2D graphics context is allocated for each thread to avoid inter-thread interference.

When allocating a new application context that will be used in a Distributed Aurora Imaging Library monitoring configuration (either the monitoring application or publishing application), you should specify the cluster manager. The cluster manager is a server that prevents duplicate application context identifiers by supplying each new application context with a range of allowable identifiers to choose from.

After allocating the application context, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the application context identifier returned is not [`M_NULL`](../../Reference/app/MappAlloc.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/app/MappAlloc.md) was specified).

When the application context is no longer required, release it using[`MappFree`](../../Reference/app/MappFree.md)unless [`M_UNIQUE_ID`](../../Reference/app/MappAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/app/MappAlloc.md) was specified, the smart identifier manages the application context's lifetime and you must not manually free it.

## Parameters

### `ServerDescription` *(in, AIL_CONST_TEXT_PTR)*

Specifies the computer name or IP address of the cluster manager. When not in a Distributed Aurora Imaging Library environment, set this parameter to [`M_NULL`](../../Reference/app/MappAlloc.md).

*For specifying the cluster manager*

| Value | Description |
| --- | --- |
| `"M_DEFAULT"` | Specifies to use the cluster manager specified in the Aurora Imaging Configurator utility (**Cluster Manager** pane, accessible from the **Distributed Aurora Imaging Library** item).

> **Note:** This only applies when allocating an application within a Distributed Aurora Imaging Library monitoring configuration. |
| `M_NULL` | Specifies that no cluster manager will be used. This applies to applications in the controlling configuration and any application not part of a Distributed Aurora Imaging Library configuration. |
| `"ClusterManagerName"` | Specifies the computer name or IP address of the cluster manager.

Replace  with a string specifying the computer name or IP address of the server used as the cluster manager; both IPv4 and IPv6 addresses are supported.

> **Note:** This only applies when allocating an application within a Distributed Aurora Imaging Library monitoring configuration. |

### `InitFlag` *(in, AIL_INT64)*

Specifies the type of initialization to perform on the application context. This parameter should be set to one of the following values:

*For specifying the type of initialization*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default initialization. During the allocation of the application context, reported error messages are displayed. |
| `M_QUIET` | Specifies to suppress the displaying of error messages during the allocation of the application context. Note that this value can be combined with [`M_TRACE_LOG_DISABLE`](../../Reference/app/MappAlloc.md). |
| `M_TRACE_LOG_DISABLE` | Specifies to disable the creation of trace logs by any external program, such as Aurora Imaging Profiler or the Aurora Imaging Configurator utility (Interactive Troubleshooting). Calling [`MappControl`](../../Reference/app/MappControl.md) with [`M_TRACE`](../../Reference/app/MappControl.md) set to [`M_LOG_ENABLE`](../../Reference/app/MappControl.md) will override [`M_TRACE_LOG_DISABLE`](../../Reference/app/MappAlloc.md) from the point in the code that the function appears.

Disabling trace logs with [`M_TRACE_LOG_DISABLE`](../../Reference/app/MappAlloc.md) helps to protect intellectual property against reverse engineering because no external program will be able to generate a trace for this application. Note that this value can be combined with [`M_QUIET`](../../Reference/app/MappAlloc.md). |

*For specifying the hardware-acceleration display mode*

| Value | Description |
| --- | --- |
| `M_X11_ACCELERATION` | Specifies whether Aurora Imaging Library can use X11 acceleration for display purposes. This determines the hardware acceleration display mode. |

*For specifying a cluster node value*

| Value | Description |
| --- | --- |
| `M_CLUSTER_NODE` | Specifies a cluster node value when none is specified with a cluster manager. A Distributed Aurora Imaging Library publishing application requires a cluster node value that identifies the publishing application. If you have not specified a cluster manager for this application, ensure that each publishing application in the cluster has a unique, non-zero value.

> **Note:** Note that a cluster manager automatically assigns a cluster node value to an application. It is the preferred method of managing cluster node values. |

### `ContextAppIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the application context identifier or specifies the data type that the function should use to return the application context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated application context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated application context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_APP_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the application context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated application context.

If allocation fails, [`M_NULL`](../../Reference/app/MappAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the application context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_APP_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/app/MappAlloc.md) was specified).

## Remarks

> If you are creating a DLL that includes a call to [`MappAlloc`](../../Reference/app/MappAlloc.md), ensure that the call is not made from the `DllMain()` function, because [`MappAlloc`](../../Reference/app/MappAlloc.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MappAlloc`](../../Reference/app/MappAlloc.md) from an initialization function in your DLL instead.
