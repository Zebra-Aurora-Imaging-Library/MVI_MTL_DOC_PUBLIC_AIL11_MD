---
doctype: Reference
module: im
function: MimRemap
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimRemap"
---

# MimRemap

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

> Remap source pixels to destination pixels according to the specified mode.

## Syntax

```c
void MimRemap(
    AIL_ID    RemapContextImId,  //in
    AIL_ID    SrcImageBufId,     //in
    AIL_ID    DstImageBufId,     //out
    AIL_INT64 ControlFlag        //in
)
```

## Description

This function performs window leveling by linearly remapping source pixels to destination pixels according to the specified mode. By default, Aurora Imaging Library performs remapping calculations using the possible minimum and maximum values of the full value range of the specified destination image buffer. You can also use a custom remap image processing context and specify different values.

When using a custom remap image processing context, you can choose the range of source pixel values that are remapped, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SRC_START_VALUE`](../../Reference/im/MimControl.md) and [`M_SRC_END_VALUE`](../../Reference/im/MimControl.md), as well as the range of destination pixel values to which they are remapped using [`M_DST_START_VALUE`](../../Reference/im/MimControl.md) and [`M_DST_END_VALUE`](../../Reference/im/MimControl.md).

By default, Aurora Imaging Library determines the remapping for the destination image buffer. However, when not using a remap image processing context, you can specify to remap the values such that the value zero in the source buffer maps to the value zero in the destination buffer with [`M_CENTERED`](../../Reference/im/MimRemap.md). Note that saturation can occur when [`M_CENTERED`](../../Reference/im/MimRemap.md) is specified.

## Parameters

### `RemapContextImId` *(in, AIL_ID)*

Specifies the identifier of a remap image processing context. If a custom remap image processing context is not used, this parameter must be set to [`M_DEFAULT`](../../Reference/im/MimRemap.md).

*For specifying the remap context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a linear remapping from source pixel to corresponding destination pixel.

> **Note:** Note, Aurora Imaging Library performs remapping calculations using the possible minimum and maximum values of the specified destination image buffer. By default, this is the full value range according to the buffer type. To control these values, use [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md). For the source image buffer, the minimum and maximum values used for calculations depend on the specified remapping mode ([`ControlFlag`](../../Reference/im/MimRemap.md)). |
| `Remap image processing context ID` | Specifies the identifier of a remap image processing context, previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_IM_REMAP_CONTEXT`](../../Reference/im/MimAlloc.md). |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the remapping mode. When using a custom remap image processing context, this parameter must be set to [`M_DEFAULT`](../../Reference/im/MimRemap.md).

*For specifying the remapping method to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

When using the default remap image processing context, the default remapping mode is [`M_FIT_SRC_RANGE`](../../Reference/im/MimRemap.md).

When using a custom remap image processing context, this is the only possible setting. Use [`MimControl`](../../Reference/im/MimControl.md) with [`M_SRC_RANGE`](../../Reference/im/MimControl.md) to specify how the source range is established. |
| `M_FIT_SRC_DATA` | Specifies to remap according to the source data. The true minimum and maximum pixel values will be calculated from the source image buffer using [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md). |
| `M_FIT_SRC_RANGE` | Specifies to remap according to the minimum and maximum pixel values specified for the source image buffer ([`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md)). By default, this is the full value range according to the buffer type.

If the source buffer has data below the specified [`M_MIN`](../../Reference/buf/MbufControl.md) or above the specified [`M_MAX`](../../Reference/buf/MbufControl.md), this data will be mapped to unpredictable values. If the goal is to clip this data to the [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) values of the destination buffer, use [`MimClip`](../../Reference/im/MimClip.md) with [`M_OUT_RANGE`](../../Reference/im/MimClip.md) on the source buffer first to set the data outside the range to the specified source [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) values. |

*For specifying the remapping centered to zero*

| Value | Description |
| --- | --- |
| `M_CENTERED` | Specifies to center the range to zero. |
