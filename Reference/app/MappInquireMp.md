---
doctype: Reference
module: app
function: MappInquireMp
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappInquireMp"
---

# MappInquireMp

> Inquire about a multi-core processing application environment setting.

## Syntax

```c
AIL_INT MappInquireMp(
    AIL_ID    ContextAppId,  //in
    AIL_INT64 InquireType,   //in
    AIL_INT64 TypeFlag,      //in
    AIL_INT64 TypeValue,     //in
    void *    UserVarPtr     //out
)
```

## Description

This function inquires about a setting of an Aurora Imaging Library multi-core processing application environment. It can return whether multi-core processing can be used to execute certain parts of functions. It can also return how multi-core processing is performed.

In multi-thread environments, an [`MappInquireMp`](../../Reference/app/MappInquireMp.md) call returns information that applies to all application threads running Aurora Imaging Library, unless the specific setting was overridden for a specific thread using [`MthrControlMp`](../../Reference/thr/MthrControlMp.md); in which case, you should use [`MthrInquireMp`](../../Reference/thr/MthrInquireMp.md) to obtain information about that thread.

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappInquireMp.md). |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of multi-core processing application environment setting about which to inquire.

### `TypeFlag` *(in, AIL_INT64)*

Specifies additional information regarding the inquired setting. This parameter must be set to `M_DEFAULT` if not used.

### `TypeValue` *(in, AIL_INT64)*

Specifies additional information regarding the inquired setting. This parameter must be set to `M_DEFAULT` if not used.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MappInquireMp`](../../Reference/app/MappInquireMp.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about multi-core processing application environment setting

Note that [`TypeValue`](../../Reference/app/MappInquireMp.md) and [`TypeFlag`](../../Reference/app/MappInquireMp.md) should be set to [`M_DEFAULT`](../../Reference/app/MappInquireMp.md) if they are not used.

---

### `M_CORE_AFFINITY_MASK`

Inquires about the core affinity bit-mask, which indicates the CPU cores that can be used to run the multi-core processing part of functions.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_AFFINITY_MASK`](Reference/app/MappControlMp.md))* |  |
| `Zero-initialized bit-mask` | Specifies that all CPU cores that are available to the process running your application can be used ([`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md) is set to [`M_ALL`](../../Reference/app/MappControlMp.md)). |

---

### `M_CORE_AFFINITY_MASK_ARRAY_SIZE`

Inquires the size of the array required to store the core affinity bit-mask. See [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappControlMp.md) in [`MappControlMp`](../../Reference/app/MappControlMp.md) for more details.  > **Note:** The size of the array might represent a larger number of CPU cores than there are available in your computer. This is because the CPU cores might not be indexed by the operating system successively and/or are less than a multiple of 64.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the size of the core affinity mask array. |

---

### `M_CORE_AFFINITY_MASK_PROCESS`

Inquires about the core affinity bit-mask that the operating system assigned to the process running your application. This mask indicates all CPU cores available to the process at the time when the application was allocated using [`MappAlloc`](../../Reference/app/MappAlloc.md).

---

### `M_CORE_MAX`

Inquires the maximum number of CPU cores to use to process the multi-core processing part of each function.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_MAX`](Reference/app/MappControlMp.md))* |  |

---

### `M_CORE_MAX_FOR_COPY`

Inquires the maximum number of CPU cores to use to process the multi-core processing part of copy type functions.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_MAX_FOR_COPY`](Reference/app/MappControlMp.md))* |  |

---

### `M_CORE_MEMORY_BANK`

Inquires the index of a memory bank local to the specified CPU core.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the CPU core to inquire. |
| `-1` | Specifies that no memory bank is local to the specified CPU core or the CPU core is not available to the process running your application. |
| `Value >= 0` | Specifies the memory bank index. |

---

### `M_CORE_NUM_PROCESS`

Inquires the number of CPU cores available to the process running your application. Note that if the operating system restricts the available CPU cores, the number of CPU cores returned differs from the number of CPU cores present in your computer. Use [`M_CORE_AFFINITY_MASK_PROCESS`](../../Reference/app/MappInquireMp.md) to establish which CPU cores are available to the process running your application.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of CPU cores. |

---

### `M_CORE_SHARING`

Inquires whether multi-core processing can use multiple logical cores per physical CPU core, when hyper-threading is enabled and supported.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_SHARING`](Reference/app/MappControlMp.md))* |  |

---

### `M_MEMORY_BANK_AFFINITY_MASK`

Inquires about a memory bank affinity bit-mask. This mask identifies the memory banks local to the CPU cores available to the process running your application. The operating system establishes the CPU cores available to the process.  The first element of the array represents the first 64 memory banks. The least-significant bit of the first element represents memory bank 0. The most-significant bit of the first element represents memory bank 63. The least-significant bit of the second element represents memory bank 64 and so on. A memory bank is local to the CPU cores available to the process if the memory bank's corresponding bit is enabled (1). If its corresponding bit is disabled (0), the memory bank is not local to the CPU cores available to the process or is not present in your computer.

| Value | Description |
| --- | --- |
| `M_LOCAL` | Specifies that [`M_MEMORY_BANK_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md) returns an affinity bit-mask that identifies local memory banks. |

---

### `M_MEMORY_BANK_AFFINITY_MASK_ARRAY_SIZE`

Inquires the size of the array required to store the memory bank affinity bit-mask.  > **Note:** The size of the array might represent a larger number of memory banks than available in your computer. This is because the memory banks might not be indexed by the operating system successively and/or are less than a multiple of 64.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the size of the memory bank affinity array. |

---

### `M_MEMORY_BANK_CORE_AFFINITY_MASK`

Inquires about the core affinity bit-mask representing the CPU cores local to the specified memory bank.

---

### `M_MEMORY_BANK_NUM`

Inquires the number of memory banks local to the CPU cores available to the process running your application. The operating system establishes the CPU cores available to the process.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of memory banks. |

---

### `M_MP_FORCED_DISABLE`

Inquires whether Aurora Imaging Library applications can use multi-core processing to execute certain parts of functions. This is set using the Aurora Imaging Configurator utility.  This setting takes precedence over [`M_MP_USE`](../../Reference/app/MappInquireMp.md).

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that multi-core processing works normally and all control types are in effect. |
| `M_YES` | Specifies that multi-core processing is disabled regardless of any other control type. |

---

### `M_MP_NB_PERFORMANCE_LEVEL`

Inquires the number of core performance levels.  For example, if your CPU has a heterogeneous architecture consisting two core types, this inquire will return 2.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of core performance levels. |

---

### `M_MP_PRIORITY`

Inquires the processing priority of the multi-core processing part of functions.

| Value | Description |
| --- | --- |
| *(see [`M_MP_PRIORITY`](Reference/app/MappControlMp.md))* |  |

---

### `M_MP_USE`

Inquires whether multi-core processing can be used to execute certain parts of functions.  [`M_MP_FORCED_DISABLE`](../../Reference/app/MappInquireMp.md) takes precedence over this setting.

| Value | Description |
| --- | --- |
| *(see [`M_MP_USE`](Reference/app/MappControlMp.md))* |  |

---

### `M_MP_USE_PERFORMANCE_LEVEL`

Inquires which cores a multi-core processing application can use on a heterogeneous architecture, according to their performance level.

| Value | Description |
| --- | --- |
| `1 <= Value <= 5` | Specifies the performance level.  > **Note:** Note that Level 1 corresponds to the fastest cores. |
| *(see [`M_MP_USE_PERFORMANCE_LEVEL`](Reference/app/MappControlMp.md))* |  |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information. The return value is undefined for [`M_CORE_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md), [`M_CORE_AFFINITY_MASK_PROCESS`](../../Reference/app/MappInquireMp.md), [`M_MEMORY_BANK_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md), and [`M_MEMORY_BANK_CORE_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md).

The first element of the array represents the first 64 CPU cores. The least-significant bit of the first element represents CPU core 0. The most-significant bit of the first element represents CPU core 63. The least-significant bit of the second element represents CPU core 64 and so on. A CPU core is available if its corresponding bit is enabled (1). If its corresponding bit is disabled (0), the CPU core is not available. CPU cores always have the same indices, as long as the hardware in your computer and the operating system does not change.
