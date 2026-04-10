---
doctype: Reference
module: app
function: MappControlMp
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappControlMp"
---

# MappControlMp

> Control a multi-core processing application environment setting.

## Syntax

```c
void MappControlMp(
    AIL_ID    ContextAppId,  //out
    AIL_INT64 ControlType,   //in
    AIL_INT64 TypeFlag,      //in
    AIL_INT64 TypeValue,     //in
    void *    ValuePtr       //in
)
```

## Description

This function controls the settings of an Aurora Imaging Library multi-core processing application environment. It establishes whether multi-core processing can be used to execute certain parts of functions. It also sets how multi-core processing is performed.

In multi-thread environments, an [`MappControlMp`](../../Reference/app/MappControlMp.md) call applies to all application threads running Aurora Imaging Library, unless the specific setting was overridden for a specific thread using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md).

## Parameters

### `ContextAppId` *(out, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappControlMp.md). |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of multi-core processing application environment setting to control.

### `TypeFlag` *(in, AIL_INT64)*

Specifies additional information regarding the control setting. This parameter must be set to `M_DEFAULT` if not used.

### `TypeValue` *(in, AIL_INT64)*

Specifies the setting's new value.

### `ValuePtr` *(in, *void)*

Specifies the address which contains more information about the setting's new value. Set this parameter to `M_NULL` if not used.

## Parameter Associations

### For specifying multi-core processing application environment settings

Set unused parameters to [`M_NULL`](../../Reference/app/MappControlMp.md).  Note that [`TypeValue`](../../Reference/app/MappControlMp.md) and [`TypeFlag`](../../Reference/app/MappControlMp.md) should be set to [`M_DEFAULT`](../../Reference/app/MappControlMp.md) if they are not used.

---

### `M_CORE_AFFINITY_MASK`

Sets the core affinity bit-mask, which indicates on which CPU core(s) (processor(s)) to run the multi-core processing part of Aurora Imaging Library functions. Essentially, the core affinity mask defines the preferred processing CPU core(s) to use for multi-core processing.  You can override this setting for a specific thread using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_AFFINITY_MASK`](../../Reference/thr/MthrControlMp.md).  To establish which CPU cores are assigned to the process running your application, call [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_CORE_AFFINITY_MASK_PROCESS`](../../Reference/app/MappInquireMp.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to automatically select a value using the current computer setup. The selected value is typically [`M_ALL`](../../Reference/app/MappControlMp.md). |
| `M_ALL` | Specifies to use all CPU cores available to the process running the application, as per the operating system. |
| `M_USER_DEFINED` | Specifies to use a user-defined core affinity bit-mask. |
| `M_NULL` | Specifies that a user-defined mask is not used. |
| `Non-zero initialized bit-mask array` | Specifies a non-zero initialized bit-mask array.  The first element of the array represents the first 64 CPU cores. The least-significant bit of the first element represents CPU core 0. The most-significant bit of the first element represents CPU core 63. The least-significant bit of the second element represents CPU core 64 and so on. CPU cores can be used for multi-core processing if their corresponding bit is enabled (1). If their corresponding bit is disabled (0), multi-core processing is not allowed to occur on those CPU cores. CPU cores always have the same indices, as long as the hardware in your computer and the operating system does not change.  A normal core affinity bit-mask should have at least one bit enabled so that at least one CPU core is enabled for processing. A core affinity bit-mask whose bits are all set to zero is therefore a special case and represents the default setting of all CPU cores being enabled for processing. |

---

### `M_CORE_MAX`

Sets the maximum number of CPU cores to use to process the multi-core processing part of each function, when multi-core processing is enabled. You can enable multi-core processing using the Aurora Imaging Configurator utility, [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_MP_USE`](../../Reference/app/MappControlMp.md), or [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_USE`](../../Reference/thr/MthrControlMp.md).  > **Note:** Note that this control type overrides the value set with the Aurora Imaging Configurator utility.  To set a different number of CPU cores for the multi-core processing part of each function on a specific thread, use [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_MAX`](../../Reference/thr/MthrControlMp.md).  The effective number of CPU cores available to Aurora Imaging Library is limited by the number of CPU cores installed in your computer and any limits imposed by the operating system. To establish the number of CPU cores assigned to the process running your application, call [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_CORE_NUM_PROCESS`](../../Reference/app/MappInquireMp.md).  > **Note:** Note that the first call to [`MappAlloc`](../../Reference/app/MappAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) determines the number of CPU cores available from the operating system. This information is stored in Aurora Imaging Library and not updated dynamically. Changing the number of processors available at the operating-system level, after your application is allocated, can result in erratic and unpredictable behavior.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the default value. This value is set using the Aurora Imaging Configurator utility. |
| `1 <= Value <= 65535` | Specifies the maximum number of CPU cores to use. To use only one CPU core and disable multi-core processing, set the value to 1 or use the [`M_MP_USE`](../../Reference/app/MappControlMp.md) control type.  > **Note:** Note that specifying the number of cores to use as 1 is effectively the equivalent of setting [`M_MP_USE`](../../Reference/app/MappControlMp.md) to [`M_DISABLE`](../../Reference/app/MappControlMp.md). |

---

### `M_CORE_MAX_FOR_COPY`

Sets the maximum number of CPU cores to use to process the multi-core processing part of copy type functions, such as [`MbufCopy`](../../Reference/buf/MbufCopy.md), when multi-core processing is enabled. Multi-core processing can be enabled using the Aurora Imaging Configurator utility, [`MappControlMp`](../../Reference/app/MappControlMp.md) with [`M_MP_USE`](../../Reference/app/MappControlMp.md), or [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_USE`](../../Reference/thr/MthrControlMp.md).  > **Note:** Note that this control type overrides the value set with the Aurora Imaging Configurator utility.  To set a different number of CPU cores for the multi-core processing part of copy type functions on a specific thread, use [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_MAX_FOR_COPY`](../../Reference/thr/MthrControlMp.md).  The number of cores set with [`M_CORE_MAX_FOR_COPY`](../../Reference/app/MappControlMp.md) only affects copy type functions, and supercedes the number of cores set with [`M_CORE_MAX`](../../Reference/app/MappControlMp.md). For instance, if [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) specifies to use 4 cores, then all functions will use 4 cores. If afterwards [`M_CORE_MAX_FOR_COPY`](../../Reference/app/MappControlMp.md) specifies 2 cores, all copy type functions will use 2 cores, while the remaining functions will still use 4.  The effective number of CPU cores available to Aurora Imaging Library is limited by the number of CPU cores installed in your computer and any limits imposed by the operating system. To establish the number of CPU cores assigned to the process running your application, call [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_CORE_NUM_PROCESS`](../../Reference/app/MappInquireMp.md).  > **Note:** Note that the first call to [`MappAlloc`](../../Reference/app/MappAlloc.md) or [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) determines the number of CPU cores available from the operating system. This information is stored in Aurora Imaging Library and not updated dynamically. Changing the number of processors available at the operating-system level, after your application is allocated, can result in erratic and unpredictable behavior.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the default value. This value is set using the Aurora Imaging Configurator utility. |
| `M_FOLLOW_CORE_MAX` | Specifies to use the current value of [`M_CORE_MAX`](../../Reference/app/MappControlMp.md). |
| `1 <= Value <= 65535` | Specifies the maximum number of CPU cores to use. To use only one CPU core and disable multi-core processing for copy type functions, set the value to 1. |

---

### `M_CORE_SHARING`

Sets whether multi-core processing can use multiple logical cores per physical CPU core, when hyper-threading is enabled and supported.  An Intel processor's hyper-threading technology allows each of its physical CPU cores to be represented by multiple logical CPU cores, typically improving parallelization of computations. However, depending on the processing operation, this might reduce the processing speed. If you disable [`M_CORE_SHARING`](../../Reference/app/MappControlMp.md), Aurora Imaging Library multi-core processing executes as if hyper-threading was disabled and restricts multi-core processing to one logical core per physical CPU core, minimizing logical core interactions.  Note that a thread might not have exclusive access to a CPU core; other processes or threads might still use the other logical cores of a physical CPU core and might impede multi-core processing restricted this way.  This control type only affects how the multi-core processing part of functions is performed.  You can override this setting for a specific thread using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_CORE_SHARING`](../../Reference/thr/MthrControlMp.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the default value. This default value is initially [`M_DISABLE`](../../Reference/app/MappControlMp.md), but can be changed using the Aurora Imaging Configurator utility. To change the default value, select the **Core Sharing** option in the **Basic Settings** pane, accessible from the **Multi-Processing** item in the Aurora Imaging Configurator utility. |
| `M_DISABLE` | Specifies to use only one logical core per physical CPU core, if hyper-threading is enabled. |
| `M_ENABLE` | Specifies to use all the logical cores of a physical CPU core, if hyper-threading is enabled. |

---

### `M_MP_PRIORITY`

Sets the processing priority of the multi-core processing part of functions.  You can override this setting for a specific thread using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_PRIORITY`](../../Reference/thr/MthrControlMp.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABOVE_NORMAL` | Specifies that the multi-core processing part of functions will be executed with above normal priority. |
| `M_BELOW_NORMAL` | Specifies that the multi-core processing part of functions will be executed with below normal priority. |
| `M_HIGHEST` | Specifies that the multi-core processing part of functions will be executed with high priority. Only [`M_TIME_CRITICAL`](../../Reference/app/MappControlMp.md) assigns a higher priority. |
| `M_IDLE` | Specifies that the multi-core processing part of functions will be executed with idle priority. All idle priority processing objects are only executed when a CPU core is idle. |
| `M_LOWEST` | Specifies that the multi-core processing part of functions will be executed with low priority. |
| `M_NORMAL` *(default)* | Specifies that the multi-core processing part of functions will be executed with normal priority. |
| `M_TIME_CRITICAL` | Specifies that the multi-core processing part of functions will be executed with time critical priority. Time critical processing objects have highest priority. |

---

### `M_MP_USE`

Sets whether multi-core processing can be used to execute certain parts of functions.  To enable or disable multi-core processing for a specific thread, use [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) with [`M_MP_USE`](../../Reference/thr/MthrControlMp.md).  > **Note:** Note that the Aurora Imaging Configurator utility can override and disable all multi-core processing. To inquire whether the Aurora Imaging Configurator utility has disabled multi-core processing, use [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MP_FORCED_DISABLE`](../../Reference/app/MappInquireMp.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the default value. This value is set using the Aurora Imaging Configurator utility. |
| `M_DISABLE` | Specifies that multi-core processing cannot be used. |
| `M_ENABLE` | Specifies that multi-core processing can be used. |

---

### `M_MP_USE_PERFORMANCE_LEVEL`

Sets which cores a multi-core processing application can use on a heterogeneous architecture, according to their performance level.  Use [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MP_NB_PERFORMANCE_LEVEL`](../../Reference/app/MappInquireMp.md) to get the number of core performance levels.

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to use all CPU cores, regardless of performance level. |
| `1 <= Value <= 5` | Specifies the performance level.  > **Note:** Note that Level 1 corresponds to the fastest cores. |
| `M_DEFAULT` | Specifies to use the default value. This value is set using the Aurora Imaging Configurator utility. |
| `M_DISABLE` | Specifies that multi-core processing cannot be used on cores with the specified performance level. |
| `M_ENABLE` | Specifies that multi-core processing can be used on cores with the specified performance level. |

The number of CPU cores actually used for multi-core processing will be affected by [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) (or [`M_CORE_MAX_FOR_COPY`](../../Reference/app/MappControlMp.md) for copy type functions such as [`MbufCopy`](../../Reference/buf/MbufCopy.md)) and [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md). Typically, the number of cores used for multi-processing will be the lesser of the two values.

- If [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) (or [`M_CORE_MAX_FOR_COPY`](../../Reference/app/MappControlMp.md)) specifies a larger number than the number of CPU cores enabled with [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md), only the CPU cores enabled with the core affinity mask will be used for multi-core processing.
- If [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) (or [`M_CORE_MAX_FOR_COPY`](../../Reference/app/MappControlMp.md)) specifies a smaller number than the number of CPU cores enabled with [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md), Aurora Imaging Library will restrict multi-core processing to the number of CPU cores specified with [`M_CORE_MAX`](../../Reference/app/MappControlMp.md) (or [`M_CORE_MAX_FOR_COPY`](../../Reference/app/MappControlMp.md)), but only from those enabled with the core affinity mask.
