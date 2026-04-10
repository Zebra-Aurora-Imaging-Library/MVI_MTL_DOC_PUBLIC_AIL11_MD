---
doctype: Reference
module: thr
function: MthrInquireMp
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / thr / MthrInquireMp"
---

# MthrInquireMp

> Inquires about an Aurora Imaging Library multi-core processing setting of a thread context.

## Syntax

```c
AIL_INT MthrInquireMp(
    AIL_ID    ThrId,        //in
    AIL_INT64 InquireType,  //in
    AIL_INT64 TypeFlag,     //in
    AIL_INT64 TypeValue,    //in
    void *    ResultPtr     //out
)
```

## Description

This function inquires about an Aurora Imaging Library multi-core processing setting of a thread context. This function can return whether multi-core processing is used to execute the Aurora Imaging Library functions on the specified thread. It can also return how multi-core processing is performed for the specified thread.

[`MthrInquireMp`](../../Reference/thr/MthrInquireMp.md) always takes precedence over [`MappInquireMp`](../../Reference/app/MappInquireMp.md).

## Parameters

### `ThrId` *(in, AIL_ID)*

Specifies the identifier of a user-allocated Aurora Imaging Library thread context allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md).

*For specifying the identifier of a user-allocated Aurora Imaging Library thread context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default Aurora Imaging Library thread context identifier associated with the current Host thread. |
| `Thread context identifier` | Specifies the identifier of a user-allocated Aurora Imaging Library thread context. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of multi-core processing setting about which to inquire.

### `TypeFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `TypeValue` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `ResultPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MthrInquireMp`](../../Reference/thr/MthrInquireMp.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about general types of thread settings

---

### `M_CORE_AFFINITY_MASK`

Inquires the core affinity bit-mask, which indicates the CPU cores that can be used to run the multi-core processing part of Aurora Imaging Library functions.  The first element of the array represents the first 64 CPU cores. The least-significant bit of the first element represents CPU core 0. The most-significant bit of the first element represents CPU core 63. The least-significant bit of the second element represents CPU core 64 and so on. A CPU core is available if its corresponding bit is enabled (1). If its corresponding bit is disabled (0), the CPU core is not available. CPU cores always have the same indices, as long as the hardware in your computer and the operating system does not change.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_AFFINITY_MASK`](Reference/thr/MthrControlMp.md))* |  |
| `Zero-initialized bit-mask` | Specifies that all CPU cores that are available to the process running your Aurora Imaging Library application can be used ([`M_CORE_AFFINITY_MASK`](../../Reference/thr/MthrControlMp.md) is set to [`M_ALL`](../../Reference/thr/MthrControlMp.md)). |

---

### `M_CORE_AFFINITY_MASK_ARRAY_SIZE`

Inquires the size of the array required to store the core affinity bit-mask. See [`M_CORE_AFFINITY_MASK`](../../Reference/thr/MthrControlMp.md) in [`MthrControlMp`](../../Reference/thr/MthrControlMp.md) for more details.  > **Note:** The size of the array might represent a larger number of CPU cores than there are available in your computer. This is because the CPU cores might not be indexed by the operating system successively and/or are less than a multiple of 64.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the size of the core affinity mask array. |

---

### `M_CORE_MAX`

Inquires the maximum number of CPU cores to use to process the multi-core processing part of each Aurora Imaging Library function on the specified thread.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_MAX`](Reference/thr/MthrControlMp.md))* |  |

---

### `M_CORE_MAX_FOR_COPY`

Inquires the maximum number of CPU cores to use to process the multi-core processing part of copy type functions on the specified thread.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_MAX_FOR_COPY`](Reference/thr/MthrControlMp.md))* |  |

---

### `M_CORE_NUM_EFFECTIVE`

Inquires the number of CPU cores available for executing the multi-core processing part of Aurora Imaging Library functions on the specified thread.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of CPU cores available. |

---

### `M_CORE_SHARING`

Inquires whether Aurora Imaging Library multi-core processing can use multiple logical cores per physical CPU core, when hyper-threading is enabled and supported.

| Value | Description |
| --- | --- |
| *(see [`M_CORE_SHARING`](Reference/thr/MthrControlMp.md))* |  |

---

### `M_MP_PRIORITY`

Inquires the processing priority of the multi-core processing part of Aurora Imaging Library functions.

| Value | Description |
| --- | --- |
| *(see [`M_MP_PRIORITY`](Reference/thr/MthrControlMp.md))* |  |

---

### `M_MP_USE`

Inquires whether multi-core processing can be used to execute certain parts of Aurora Imaging Library functions.  [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MP_FORCED_DISABLE`](../../Reference/app/MappInquireMp.md) takes precedence over this setting.

| Value | Description |
| --- | --- |
| *(see [`M_MP_USE`](Reference/thr/MthrControlMp.md))* |  |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information cast to an _AIL_INT_, except for [`M_CORE_AFFINITY_MASK`](../../Reference/thr/MthrInquireMp.md). In this case, the returned value is undefined. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
