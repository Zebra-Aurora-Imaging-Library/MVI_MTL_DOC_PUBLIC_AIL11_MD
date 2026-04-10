---
doctype: Reference
module: sys
function: MsysInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysInquire"
---

# MsysInquire

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | Partial |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | Partial |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Inquire about a system setting.

## Syntax

```c
AIL_INT MsysInquire(
    AIL_ID    SysId,        //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires about the specified system setting.

Note that you can use [`MsysControl`](../../Reference/sys/MsysControl.md) to control specific system settings.

You can also interactively inquire most of the system settings in real-time, using Aurora Imaging Intellicam's Feature Browser.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system identifier. This parameter should be set to one of the following values:

*For the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, which you have allocated using the [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) function. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of system setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MsysInquire`](../../Reference/sys/MsysInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For general system settings

The following inquire types allow you to inquire general system settings.

---

### `M_ACCELERATOR_PRESENT`

**Board availability:** Concord PoE, GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires whether the processing accelerator is present.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the processing accelerator is not present. |
| `M_YES` | Specifies that the processing accelerator is present. |

---

### `M_ALLOCATION_OVERSCAN`

Inquires whether image buffers, allocated on the system, will be allocated with an overscan region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that image buffers allocated on the system will have no overscan region. |
| `M_ENABLE` *(default)* | Specifies that image buffers are allocated on the system with an overscan region. |

---

### `M_ALLOCATION_OVERSCAN_SIZE`

Inquires the size of the overscan region, added around all subsequently allocated image buffers ([`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default size of the overscan region. |
| `Value` | Specifies the size of the overscan region, in pixels. |

---

### `M_ASYNCHRONOUS_CALL_SUPPORT`

Inquires whether the system supports asynchronous function execution or not.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the system does not support asynchronous execution. **[Concord PoE, GenTL, GigE Vision, Host System, Indio, USB3 Vision, V4L2]** |
| `M_YES` | Specifies that the system supports asynchronous execution. **[Clarity UHD, GevIQ, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF]** |

---

### `M_BOARD_REVISION`

**Board availability:** Clarity UHD, GevIQ, Host System, Indio, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the board revision number.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the board revision number. |

---

### `M_BOARD_SUB_REVISION`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the board sub-revision number.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the board sub-revision number. |

---

### `M_BOARD_TYPE`

Inquires the type of system board.  To return only the main board type and not the sub-board types (for example, to get [`M_RAPIXO`](../../Reference/sys/MsysInquire.md) without [`M_CL`](../../Reference/sys/MsysInquire.md), [`M_PF`](../../Reference/sys/MsysInquire.md), or [`M_SFCL`](../../Reference/sys/MsysInquire.md)), mask the return value with **M_BOARD_TYPE_MASK**. For an example of how to use this mask, refer to the examples below.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This control type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_CLARITY_UHD` | Specifies a Zebra Clarity UHD board. **[Clarity UHD]** |
| `M_CLARITY_UHD + M_H264` | Specifies a Zebra Clarity UHD board with the optional on-board H.264 encoding module. **[Clarity UHD]** |
| `M_CONCORD_POE + M_DCH` | Specifies a Zebra Concord PoE base model Dual-Port board. **[Concord PoE]** |
| `M_CONCORD_POE + M_DCH + M_TOE` | Specifies a Zebra Concord PoE with ToE Dual-Port board. **[Concord PoE]** |
| `M_CONCORD_POE + M_QCH` | Specifies a Zebra Concord PoE base model Quad-Port board. **[Concord PoE]** |
| `M_CONCORD_POE + M_QCH + M_TOE` | Specifies a Zebra Concord PoE with ToE Quad-Port board. **[Concord PoE]** |
| `M_GENTL` | Specifies a camera that uses the Zebra driver for GenTL. **[GenTL]** |
| `M_GEVIQ + M_D25G` | Specifies a Zebra GevIQ Dual 25 Gbps Ethernet board. **[GevIQ]** |
| `M_GIGE_VISION` | Specifies a camera that uses the Zebra GigE Vision driver. **[GigE Vision]** |
| `M_HOST` | Specifies a Host system. **[Host System]** |
| `M_HOST+ M_4SIGHT + M_4SIGHT_EV6` | Specifies a Host system allocated on Zebra 4Sight EV6. **[Host System]** |
| `M_HOST+ M_4SIGHT + M_4SIGHT_EV7` | Specifies a Host system allocated on Zebra 4Sight EV7. **[Host System]** |
| `M_INDIO` | Specifies a Zebra Indio board. **[Indio]** |
| `M_IRIS_GTX` | Specifies a Zebra Iris GTX smart camera. **[Iris GTX]** |
| `M_RADIENT + M_CL + M_DBCL` | Specifies a Zebra Radient eV-CL dual-Base Camera Link board. **[Radient eV-CL]** |
| `M_RADIENT + M_CL + M_DFCL` | Specifies a Zebra Radient eV-CL dual-Full Camera Link board. **[Radient eV-CL]** |
| `M_RADIENT + M_CL + M_QBCL` | Specifies a Zebra Radient eV-CL quad-Base Camera Link board. **[Radient eV-CL]** |
| `M_RADIENT + M_CL + M_SBCL` | Specifies a Zebra Radient eV-CL single-Base Camera Link board. **[Radient eV-CL]** |
| `M_RADIENT + M_CL + M_SFCL` | Specifies a Zebra Radient eV-CL single-Full Camera Link board. **[Radient eV-CL]** |
| `M_RAPIXO + M_CL + M_DBCL` | Specifies a Zebra Rapixo DB-CL (dual-Base Camera Link) board. **[Rapixo CL]** |
| `M_RAPIXO + M_CL + M_DBCL + M_PF` | Specifies a Zebra Rapixo Pro DB-CL (dual-Base Camera Link) with a Processing FPGA. **[Rapixo CL]** |
| `M_RAPIXO + M_CL + M_DFCL + M_PF` | Specifies a Zebra Rapixo Pro DF-CL (dual-Full Camera Link) with a Processing FPGA. **[Rapixo CL]** |
| `M_RAPIXO + M_CL + M_QBCL + M_PF` | Specifies a Zebra Rapixo Pro QB-CL (squad-Base Camera Link) with a Processing FPGA. **[Rapixo CL]** |
| `M_RAPIXO + M_CL + M_SFCL` | Specifies a Zebra Rapixo SF-CL (single-Full Camera Link) board. **[Rapixo CL]** |
| `M_RAPIXO + M_CL + M_SFCL + M_PF` | Specifies a Zebra Rapixo Pro SF-CL (single-Full Camera Link) with a Processing FPGA. **[Rapixo CL]** |
| `M_RAPIXO + M_COF + M_Q10G` | Specifies a Zebra Rapixo CoF Quad 10 Gbps board. **[Rapixo CoF]** |
| `M_RAPIXO + M_COF + M_Q25G + M_PF` | Specifies a Zebra Rapixo CoF Quad 25 Gbps board with a Processing FPGA. **[Rapixo CoF]** |
| `M_RAPIXO + M_CXP + M_D12G` | Specifies a Zebra Rapixo CXP Dual 12G board. **[Rapixo CXP]** |
| `M_RAPIXO + M_CXP + M_Q6G` | Specifies a Zebra Rapixo CXP (Quad CXP-6) board. **[Rapixo CXP]** |
| `M_RAPIXO + M_CXP + M_Q6G + M_DFWD` | Specifies a Zebra Rapixo CXP DF (Quad CXP-6 with data forwarding) board. **[Rapixo CXP]** |
| `M_RAPIXO + M_CXP + M_Q12G` | Specifies a Zebra Rapixo CXP (Quad CXP-12) board. **[Rapixo CXP]** |
| `M_RAPIXO + M_CXP + M_Q12G + M_DFWD` | Specifies a Zebra Rapixo CXP DF (Quad CXP-12 with data forwarding) board. **[Rapixo CXP]** |
| `M_RAPIXO + M_CXP + M_Q12G + M_PF` | Specifies a Zebra Rapixo CXP Pro (Quad CXP-12 with a Processing FPGA) board. **[Rapixo CXP]** |
| `M_RAPIXO + M_CXP + M_S12G` | Specifies a Zebra Rapixo CXP Single 12G board. **[Rapixo CXP]** |
| `M_USB3_VISION` | Specifies a camera that uses the Zebra USB3 Vision driver. **[USB3 Vision]** |
| `M_V4L2` | Specifies a camera that uses the Video4Linux2 driver. **[V4L2]** |

---

### `M_COM_SUPPORTED`

Inquires whether the system can be used with the Aurora Imaging Library Industrial Communication module (Mcom).

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the system cannot be used. **[Clarity UHD, Concord PoE, GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `M_YES` | Specifies that the system can be used. **[Host System, Indio, Iris GTX]** |

---

### `M_CURRENT_THREAD_ID`

Inquires the identifier of the current system thread. This identifier can be used with the thread module.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `System thread identifier` | Specifies the identifier of the current system thread. |

---

### `M_DCF_SUPPORTED`

Inquires whether the system supports downloadable digitizer configuration format (_DCF_) files.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that DCF files are not supported. **[Concord PoE, Host System, Indio, Iris GTX]** |
| `M_TRUE` | Specifies that DCF files are supported. **[Clarity UHD, GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |

---

### `M_DEFAULT_PITCH_BYTE_MULTIPLE`

Inquires the pitch (or stride) multiple (in bytes) for the buffers allocated on the system.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value for the pitch multiple. |
| `Value` | Specifies the pitch multiple, in bytes. |

---

### `M_DEVICE_NAME`

**Board availability:** Concord PoE

Inquires the user-defined name for the board.  > **Note:** This inquire type is also available for the Zebra Concord PoE base model.

| Value | Description |
| --- | --- |
| `Value` | Specifies the device name that was set using[`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md). |

---

### `M_DEVICE_NAME_MAX_SIZE`

**Board availability:** Concord PoE

Inquires the maximum length that the device name can have of the string returned by [`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md).  > **Note:** This inquire type is also available for the Zebra Concord PoE base model.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum length of the string returned by [`M_DEVICE_NAME`](../../Reference/sys/MsysControl.md). |

---

### `M_DIGITIZER_NUM`

Inquires the total number of possible independent acquisition paths on the system. Note that this is not the same as the total number of allocated acquisition paths on the system.

#### System specific

| Board(s) | Note |
|---|---|
| GigE Vision, V4L2 | Inquires the total number of Video4Linux2 cameras discovered. |
| GigE Vision | Inquires the total number of GigE Vision cameras discovered. |
| USB3 Vision | Inquires the total number of USB cameras discovered. |
| GenTL | Inquires the total number of GenICam complaint cameras discovered. |
| Concord PoE | This inquire type is also available for the Zebra Concord PoE base model. |
| GevIQ | Inquires the total number of GigE Vision cameras connected directly to Zebra GevIQ or behind a switch. |

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of independent acquisition paths available. |

---

### `M_DIGITIZER_TYPE`

**Board availability:** Iris GTX

Inquires the type of frame grabber(s) available to allocate a digitizer on the system.

| Value | Description |
| --- | --- |
| `M_GTX2000` | Specifies a Zebra Iris GTX 2000. **[Iris GTX]** |
| `M_GTX2000C` | Specifies a Zebra Iris GTX 2000C. **[Iris GTX]** |
| `M_GTX3000` | Specifies a Zebra Iris GTX 3000. **[Iris GTX]** |
| `M_GTX3000C` | Specifies a Zebra Iris GTX 3000C. **[Iris GTX]** |
| `M_GTX5000` | Specifies a Zebra Iris GTX 5000. **[Iris GTX]** |
| `M_GTX5000C` | Specifies a Zebra Iris GTX 5000C. **[Iris GTX]** |
| `M_GTX8000` | Specifies a Zebra Iris GTX 8000. **[Iris GTX]** |
| `M_GTX8000C` | Specifies a Zebra Iris GTX 8000C. **[Iris GTX]** |
| `M_GTX12000` | Specifies a Zebra Iris GTX 12000. **[Iris GTX]** |
| `M_GTX12000C` | Specifies a Zebra Iris GTX 12000C. **[Iris GTX]** |
| `M_GTX16000` | Specifies a Zebra Iris GTX 16000. **[Iris GTX]** |
| `M_GTX16000C` | Specifies a Zebra Iris GTX 16000C. **[Iris GTX]** |

---

### `M_DISPLAY_OUTPUT_NUM`

Inquires the number of video output controllers available on your Zebra imaging board.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of video output controllers. |

---

### `M_DISTRIBUTED_AIL_PROTOCOL`

Inquires the protocol used for DAIL.

| Value | Description |
| --- | --- |
| `M_DAIL_NOT_USED` | Specifies that DAIL is not used. |
| `M_DAIL_SHM` | Specifies that DAIL is using the SHM protocol. |
| `M_DAIL_TCPIP` | Specifies that DAIL is using the TCP/IP protocol. |

---

### `M_DISTRIBUTED_AIL_REMOTE_COMPUTER_NAME`

Inquires the name of the remote computer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the computer name that was set in[`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_DISTRIBUTED_AIL_TYPE`

Inquires how the system is allocated via DAIL.

| Value | Description |
| --- | --- |
| `M_DAIL_LOCAL_HOST` | Specifies that the system is allocated on the local computer with DAIL. |
| `M_DAIL_NOT_USED` | Specifies that the system is not allocated with DAIL. |
| `M_DAIL_REMOTE` | Specifies that the system is allocated on a remote computer with DAIL. |

---

### `M_EXTENDED_INIT_FLAG`

Inquires the system initialization flag.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_CL` | Specifies to initialize Camera Link transport layer technology. |
| `M_COMPLETE` | Specifies to initialize the system completely; the system is initialized to its default state and any required resident software is downloaded. |
| `M_CXP` | Specifies to initialize CoaXPress transport layer technology. |
| `M_GEV` | Specifies to initialize Ethernet transport layer technology. |
| `M_MIXED` | Specifies to initialize the transport layer technology specified by the GenTL Producer (library). |
| `M_U3V` | Specifies to initialize USB transport layer technology. |
| `M_DEVICE_NAME` | Specifies that the[`SystemNum`](../../Reference/sys/MsysAlloc.md) parameter identifies the target board using its user-defined name. |

---

### `M_FIRMWARE_BUILDDATE`

**Board availability:** Clarity UHD, Concord PoE, GevIQ, Host System, Indio, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the date when the grab firmware was built.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the grab firmware build date. |

---

### `M_FIRMWARE_REVISION`

**Board availability:** Clarity UHD, Concord PoE, GevIQ, Host System, Indio, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the revision number of the grab firmware.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the grab firmware revision number. |

---

### `M_GC_FEATURE_EXECUTE_POLLING_MODE`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF

Inquires whether to automatically perform execution-complete polling on executable camera features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTOMATIC` | Specifies that the specific executable camera feature is executed synchronously. |
| `M_MANUAL` *(default)* | Specifies that the executable camera feature is executed asynchronously. |

---

### `M_GC_LOCAL_MAC_ADDRESS + n`

**Board availability:** Concord PoE

Inquires the MAC address at port _n_, where _n_ is a number between 0 and the number of Ethernet ports ([`M_GC_NIC_PORT_COUNT`](../../Reference/sys/MsysInquire.md)).  > **Note:** This inquire type is also available for the Zebra Concord PoE base model.

| Value | Description |
| --- | --- |
| `Value` | Specifies the MAC address at the specified port.  The MAC address is stored as a 48-bit integer in an _AIL_INT64_. Typically, MAC addresses are written as 6 separate 8-bit numbers (hexadecimal number pairs). To retrieve these numbers, map the 48-bit MAC address to 6 _AIL_UINT8_variables.  For example, the stored representation of the MAC address `84:20:fc:33:0e:e9`is `0x0000e90e33fc2084`. The least-significant byte (`84`) is the first number of the MAC address, the second least-significant byte (`0e`) is the second number of the MAC address and so on. The first 4 hexadecimal digits (2 most significant bytes) are not part of the MAC address. |

---

### `M_GC_NIC_PORT_COUNT`

**Board availability:** Concord PoE

Inquires the number of Ethernet ports.  > **Note:** This inquire type is also available for the Zebra Concord PoE base model.

| Value | Description |
| --- | --- |
| `2` | Specifies that the system has 2 Ethernet ports. |
| `4` | Specifies that the system has 4 Ethernet ports. |

---

### `M_GENICAM_AVAILABLE`

Inquires whether the system supports a GenICam-compliant device (such as a GigE Vision device).

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that GenICam is not available. |
| `M_TRUE` | Specifies that GenICam is available. |

---

### `M_GENTL_DEVICE_COUNT`

**Board availability:** GenTL

Inquires the number of GenTL devices associated with the specified interface.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of GenTL devices. |

---

### `M_GENTL_INTERFACE_COUNT`

**Board availability:** GenTL

Inquires the number of GenTL interfaces available to your Aurora Imaging Library system. This is useful when the GenTL Producer (library) is of a mixed type.

| Value | Description |
| --- | --- |
| `0 <= Value <= 33` | Specifies the number of GenTL interfaces. |

---

### `M_GRAB_FPGA_FAN_RPM`

**Board availability:** Clarity UHD, GevIQ, Rapixo CXP, Rapixo CoF

Inquires the revolutions per minute (RPM) of the grab firmware's fan.

| Value | Description |
| --- | --- |
| `Value` | Specifies the speed of the grab firmware's fan, in RPM. |

---

### `M_INSTALLED_SYSTEM_DEVICE_COUNT`

Inquires the number of installed devices of the specified system type.

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the number of devices is unknown. **[Clarity UHD, Concord PoE, GenTL, Indio, Iris GTX, USB3 Vision]** |
| `Value` | Specifies the number of installed devices of the specified system type. **[Clarity UHD, Concord PoE, GevIQ, GigE Vision, Host System, Indio, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |

---

### `M_LED_USER`

**Board availability:** Iris GTX

Inquires the color of the user LED on your Zebra Iris GTX.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GREEN` | Specifies to turn the user LED green. |
| `M_OFF` *(default)* | Specifies to turn the user LED off. |
| `M_ORANGE` | Specifies to turn the user LED orange. |
| `M_RED` | Specifies to turn the user LED red. |

---

### `M_LOCATION`

Inquires the location of the specified system.

| Value | Description |
| --- | --- |
| `M_LOCAL` | Specifies that the system is on the local computer. |
| `M_REMOTE` | Specifies that the system is on a remote computer or was allocated via Distributed Aurora Imaging Library. |

---

### `M_MEMORY_FREE`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the total amount of free on-board memory. Note that this memory might not be contiguous.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of free on-board memory, in bytes. |

---

### `M_MEMORY_FREE_BANK_0`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the total amount of free on-board memory the first SDRAM bank. Note that this memory might not be contiguous.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of free on-board memory in the first SDRAM bank, in bytes. |

---

### `M_MEMORY_FREE_BANK_1`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the total amount of free on-board memory second SDRAM bank. Note that this memory might not be contiguous.

| Value | Description |
| --- | --- |
| `Value` | Specifies the amount of free on-board memory second SDRAM bank, in bytes. |

---

### `M_MEMORY_SIZE`

**Board availability:** Clarity UHD, GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the amount of on-board memory.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total amount of on-board memory, in Mbytes. |

---

### `M_MEMORY_SIZE_BANK_0`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the total amount of on-board shared memory in the first SDRAM bank.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total amount of on-board shared memory in the first SDRAM bank, in Mbytes. |

---

### `M_MODIFIED_BUFFER_HOOK_MODE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires whether to run user-defined functions hooked to a buffer modification on separate threads, up to the number of CPU cores present in the computer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MULTI_THREAD` | Specifies to run user-defined functions hooked to a buffer modification on separate threads. |
| `M_SINGLE_THREAD` *(default)* | Specifies that only one thread should be created and that all user-defined functions hooked to buffer modifications are run on the same thread. |

---

### `M_NUM_CAMERA_PRESENT`

**Board availability:** GenTL, GigE Vision, USB3 Vision, V4L2

Inquires the number of GigE Vision-compliant cameras connected to the allocated system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of GigE Vision-compliant cameras. |

---

### `M_NUMBER`

Inquires the board number of the system.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default board. |
| `M_DEVn` | Specifies the device number (rank) of the board. |
| `"BoardIdentifierString"` | Specifies the user-defined name of the board. |
| `M_GENTL_PRODUCER` | Specifies the GenTL Producer (library) for which to allocate this Aurora Imaging Library GenTL system. |
| `0 <= Value <= 127` | Specifies the index. |

---

### `M_OWNER_APPLICATION`

Inquires the Aurora Imaging Library identifier of the application context in which the system has been allocated.

| Value | Description |
| --- | --- |
| `Application identifier` | Specifies the Aurora Imaging Library application context identifier. |

---

### `M_PCIE_NUMBER_OF_LANES`

**Board availability:** Clarity UHD, GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the number of PCIe lanes that are currently active.

| Value | Description |
| --- | --- |
| `1; 2; 4; 8; 16` | Specifies the number of active lanes. |

---

### `M_PCIE_NUMBER_OF_LANES_MAX`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the total number of PCIe lanes that your board can use.

| Value | Description |
| --- | --- |
| `1; 2; 4; 8; 16` | Specifies the total number of lanes. |

---

### `M_PCIE_SPEED`

**Board availability:** Clarity UHD, GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the negotiated generation of the PCIe standard currently being used by the active PCIe lanes. For example, if your board is a PCIe 3.0 device, but it is connected to a PCIe 2.0 device, this inquire type returns [`M_GEN2`](../../Reference/sys/MsysInquire.md).  To establish the current PCIe speed possible for your board, use the following equation:`[`M_PCIE_NUMBER_OF_LANES`](../../Reference/sys/MsysInquire.md) x 2 x _speed of PCIe generation_`

| Value | Description |
| --- | --- |
| `M_GENn` | Specifies the PCIe generation, where _n_ is a value from 1 to 3. |
| `M_INVALID` | Specifies that the PCIe generation cannot be returned. |

---

### `M_PCIE_SPEED_MAX`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the generation of the PCIe standard of your board. For example, if your board is a PCIe 3.0 device, this inquire type returns [`M_GEN3`](../../Reference/sys/MsysInquire.md).  To establish the maximum PCIe speed possible for your board, use the following equation: `[`M_PCIE_NUMBER_OF_LANES_MAX`](../../Reference/sys/MsysInquire.md) x 2 x _maximum speed of PCIe generation_`

| Value | Description |
| --- | --- |
| `M_GENn` | Specifies the PCIe generation, where _n_ is a value from 1 to 3. |
| `M_INVALID` | Specifies that the PCIe generation cannot be returned. |

---

### `M_POWER_OVER_CABLE`

**Board availability:** Concord PoE, Host System, Rapixo CXP

Inquires whether the board provides power to connected devices.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | For a Host system, this inquires whether PoE (Power over Ethernet) is enabled on the specified port.  This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |
| Concord PoE | For Zebra Concord PoE, this inquires whether PoE (Power over Ethernet) is enabled on all ports.  > **Note:** This control type is also available for the Zebra Concord PoE base model. |
| Rapixo CXP | For Zebra Rapixo CXP, this inquires whether PoCXP (Power over CXP) is automatically enabled when a PoCXP-compliant camera is connected to the specified connector.  [`M_ON`](../../Reference/sys/MsysInquire.md) and [`M_OFF`](../../Reference/sys/MsysInquire.md) are only returned when PoCXP has been manually enabled or disabled using [`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysControl.md). When automatic detection is enabled ([`M_AUTOMATIC`](../../Reference/sys/MsysInquire.md)), use [`M_POWER_OVER_CABLE_STATUS`](../../Reference/sys/MsysInquire.md)to learn whether PoCXP is currently enabled or disabled. |

| Value | Description |
| --- | --- |
| `M_AUTOMATIC` | Specifies to automatically enable or disable PoCXP on this CXP connector, depending on detected device support. |
| `M_OFF` | Specifies not to provide power to connected devices. |
| `M_ON` | Specifies to provide power to connected devices. |

---

### `M_POWER_OVER_CABLE_STATUS`

**Board availability:** Rapixo CXP

Inquires whether PoCXP (power over CXP) is enabled on a CXP connector, and whether an over- or under-current condition has been detected.  > **Note:** Once detected, an over- or under-current condition persists until the PoCXP status is reset for the connector (using [`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysControl.md) and [`M_RESET`](../../Reference/sys/MsysControl.md)). Typically, to prevent the over- or under-current condition from immediately recurring, you should first restart PoCXP for this connector using [`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysInquire.md) and [`M_OFF`](../../Reference/sys/MsysInquire.md) and then returning it to its previous setting ([`M_AUTOMATIC`](../../Reference/sys/MsysInquire.md) is recommended). If the condition persists, discontinue using PoCXP with your camera by leaving [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysInquire.md) set to [`M_OFF`](../../Reference/sys/MsysInquire.md).

| Value | Description |
| --- | --- |
| `M_OFF` | Specifies that PoCXP is disabled on the CXP connector, either because a non-PoCXP-compatible camera is connected, or because PoCXP was manually disabled (using[`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysControl.md) and [`M_OFF`](../../Reference/sys/MsysControl.md)). |
| `M_ON` | Specifies that PoCXP is enabled on the CXP connector, either because a PoCXP-compatible camera is connected, or because PoCXP was manually enabled (using[`MsysControl`](../../Reference/sys/MsysControl.md)with [`M_POWER_OVER_CABLE`](../../Reference/sys/MsysControl.md) and [`M_ON`](../../Reference/sys/MsysControl.md)). |
| `M_OVER_CURRENT` | Specifies that PoCXP is disabled on the connector because an over-current condition was detected and has not been reset. |
| `M_SENSE` | Specifies that PoCXP will be enabled or disabled automatically on the connector when a camera is connected (depending on detected device support). |
| `M_UNDER_CURRENT` | Specifies that PoCXP is disabled on the connector because an under-current condition was detected and has not been reset. |

---

### `M_PROCESSOR_NUM`

Inquires the number of processors (CPUs) available on the allocated Zebra imaging board.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of processors available. |

---

### `M_PROFINET_HARDWARE_SUPPORTED`

Inquires whether the system has access to a PROFINET Engine.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the system does not have access. **[Clarity UHD, Concord PoE, GenTL, GevIQ, GigE Vision, Host System, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |
| `M_YES` | Specifies that the system has access. **[Host System, Indio, Iris GTX]** |

---

### `M_SERIAL_NUMBER`

**Board availability:** Clarity UHD, Concord PoE, GenTL, GevIQ, GigE Vision, Host System, Indio, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, V4L2

Inquires the serial number of the Zebra Imaging board, as a string.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | [`M_SERIAL_NUMBER`](../../Reference/sys/MsysInquire.md) only applies to Zebra products. When a host system is allocated on a non-Zebra computer, no serial number is returned. |
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |
| V4L2 | When a Video4Linux2 system is allocated, no serial number is returned. |

---

### `M_SHARED_MEMORY_FREE`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the total amount of free on-board shared memory.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total amount of free on-board shared memory, in bytes. |

---

### `M_SHARED_MEMORY_SIZE`

**Board availability:** GevIQ, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the total amount of on-board shared memory.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total amount of on-board shared memory, in Mbytes. |

---

### `M_SYSTEM_DESCRIPTOR`

Inquires the system descriptor.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

---

### `M_SYSTEM_TYPE`

Inquires the type of system allocated.  If a DAIL remote system has been registered, you can use the following macros to further inquire about the system:  - To determine whether the system is a DAIL remote system, use the **M_IS_SYSTEM_DISTRIBUTED** macro. This macro returns a non-zero value if the system is a DAIL remote system and zero if the system is local. - To remove the bit flag for DAIL remote systems, use the**M_REAL_SYSTEM_TYPE** macro. This macro returns only the system type and not the system type and DAIL remote system bit flag.

| Value | Description |
| --- | --- |
| `M_SYSTEM_CLARITY_UHD_TYPE` | Specifies an Aurora Imaging Library Clarity UHD system. **[Clarity UHD]** |
| `M_SYSTEM_CONCORD_POE_TYPE` | Specifies an Aurora Imaging Library Concord POE system.  > **Note:** Note that to use Zebra Concord PoE for acquisition, you must allocate and use an Aurora Imaging Library GigE Vision system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_GIGE_VISION`](../../Reference/sys/MsysAlloc.md)) instead; refer to information denoted for a GigE Vision system. You only need to allocate and use an Aurora Imaging Library Concord PoE system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_CONCORD_POE`](../../Reference/sys/MsysAlloc.md)) to use the other functionality on the board and to inquire about the board itself. So information in this reference, for use with a Concord PoE system, is denoted Concord PoE with ToE since it is typically only applicable to this model of the board. If the information is applicable to the Zebra Concord PoE base model it will be explicitly specified. For more information, see Using an Aurora Imaging Library Concord PoE system with the Zebra Concord PoE base model.  > **Note:** This inquire type is also available for the Zebra Concord PoE base model. **[Concord PoE]** |
| `M_SYSTEM_GENTL_TYPE` | Specifies an Aurora Imaging Library GenTL system. **[GenTL]** |
| `M_SYSTEM_GEVIQ_TYPE` | Specifies an Aurora Imaging Library GevIQ system. **[GevIQ]** |
| `M_SYSTEM_GIGE_VISION_TYPE` | Specifies an Aurora Imaging Library GigE Vision system. **[GigE Vision]** |
| `M_SYSTEM_HOST_TYPE` | Specifies the Host. **[Concord PoE, Host System, Indio]** |
| `M_SYSTEM_INDIO_TYPE` | Specifies an Aurora Imaging Library Indio system. **[Indio]** |
| `M_SYSTEM_IRIS_GTX_TYPE` | Specifies an Aurora Imaging Library Iris GTX system. **[Iris GTX]** |
| `M_SYSTEM_RADIENTEVCL_TYPE` | Specifies an Aurora Imaging Library Radient eV-CL system. **[Radient eV-CL]** |
| `M_SYSTEM_RAPIXOCL_TYPE` | Specifies an Aurora Imaging Library Rapixo Pro CL system. **[Rapixo CL]** |
| `M_SYSTEM_RAPIXOCOF_TYPE` | Specifies an Aurora Imaging Library Rapixo CoF system. **[Rapixo CoF]** |
| `M_SYSTEM_RAPIXOCXP_TYPE` | Specifies an Aurora Imaging Library Rapixo CXP system. **[Rapixo CXP]** |
| `M_SYSTEM_USB3_VISION_TYPE` | Specifies an Aurora Imaging Library USB3 Vision system. **[USB3 Vision]** |
| `M_SYSTEM_V4L2_TYPE` | Specifies an Aurora Imaging Library Video4Linux2 system. **[V4L2]** |

---

### `M_TEMPERATURE`

**Board availability:** Clarity UHD, Iris GTX

Inquires the current temperature of the sensor.

#### System specific

| Board(s) | Note |
|---|---|
| Clarity UHD | On Zebra Clarity UHD, this value returns the temperature of the Host interface. |
| Iris GTX | On Zebra Iris GTX, this value returns the temperature of the daughter board (inbox). |

| Value | Description |
| --- | --- |
| `Value` | Specifies the current temperature, in degrees Celsius. |

---

### `M_TEMPERATURE_CPU`

**Board availability:** Iris GTX

Inquires the current temperature of the CPU.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current temperature, in degrees Celsius. |

---

### `M_TEMPERATURE_FPGA`

**Board availability:** Clarity UHD, GevIQ, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

Inquires the current temperature of the grab firmware.

#### System specific

| Board(s) | Note |
|---|---|
| Clarity UHD | Note that, on your Zebra Clarity UHD, this value inquires the current temperature of the Acquisition controller. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the current temperature, in degrees Celsius. |

---

### `M_TEMPERATURE_FPGA_MAX_MEASURED`

**Board availability:** Iris GTX

Inquires the maximum temperature of the grab firmware measured.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum measured temperature, in degrees Celsius. |

---

### `M_TEMPERATURE_IMAGE_SENSOR`

**Board availability:** Iris GTX

Inquires the current temperature of the image sensor.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current temperature, in degrees Celsius. |

---

### `M_THREAD_MODE`

Inquires whether threads allocated on the system can execute in asynchronous mode.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_ASYNCHRONOUS` | Specifies that threads allocated on the system can execute in asynchronous mode. |
| `M_SYNCHRONOUS` | Specifies that threads allocated on the system can only execute in synchronous mode. |

---

### `M_TIMEOUT`

**Board availability:** Clarity UHD, Concord PoE, GenTL, GevIQ, GigE Vision, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the maximum amount of time for the Host to wait for a synchronous function to return before generating a time-out error, in sec.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_INFINITE` | Waits indefinitely. |
| `Value` | Specifies the time to wait, in sec. |

---

### `M_UART_PRESENT`

Inquires the number of UARTs on the system.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |
| Concord PoE | > **Note:** This inquire type is also available for the Zebra Concord PoE base model. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of UARTs on the system. |

### Combination Constants — For inquiring if a system is remote

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine if the inquired system type is remote.

By default, an installed systems that is remote will have a distributed flag appended to the returned system type.

| Value | Description |
| --- | --- |
| `M_SYSTEM_DISTRIBUTED_FLAG` | Specifies that the system is a DAIL remote system. |

### For inquiring CXP connection errors and settings

The following inquire types and inquire values relate to (if available) the connection.

> **Board availability:** Rapixo CXP, Rapixo CoF

---

### `M_CONNECTION_STATE`

Inquires the state of the connection for a Zebra Rapixo CXP or Zebra Rapixo CoF input connector.  > **Note:** Note that, connection errors are only meaningful if the specified connection is locked.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the state of the connection is invalid.  > **Note:** For example, if you have a Zebra Rapixo CXP Dual board, you can only have connections at input connectors 0 and 1. Therefore, input connectors 2 and 3 would return [`M_INVALID`](../../Reference/sys/MsysInquire.md). |
| `M_LOCK` | Specifies that the state of the connection is locked. |
| `M_UNLOCK` | Specifies that the state of the connection is unlocked. |

---

### `M_TL_ERROR_CORRECTED_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of corrected duplicate character errors in the CXP control words.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of corrected duplicate character errors in the CXP control words. |

---

### `M_TL_ERROR_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of all errors encountered.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of all errors encountered. |

---

### `M_TL_ERROR_CTRL_CRC_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of CRC errors detected in a control packet.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of CRC errors detected in a control packet. |

---

### `M_TL_ERROR_DATA_CRC_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of CRC errors detected in a data packet.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of CRC errors detected in a data packet. |

---

### `M_TL_ERROR_ENCODING_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of protocol encoding errors detected.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of protocol encoding errors detected. |

---

### `M_TL_ERROR_EVENT_CRC_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of CRC errors detected in an event packet.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of CRC errors detected in an event packet. |

---

### `M_TL_ERROR_LOCK_LOSS_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of lock losses encountered.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of lock losses encountered. |

---

### `M_TL_ERROR_UNCORRECTED_COUNT`

**Board availability:** Rapixo CXP, Rapixo CoF

Inquires the count of uncorrected duplicate character errors in the CXP control words.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current count of uncorrected duplicate character errors in the CXP control words. |

### Combination Constants — For specifying the CXP input connector

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the CXP input connector.

> **Board availability:** Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_CONNECTIONn` | Specifies the connection made at CXP input connector _n_, where _n_ is the CXP input connector number from 0 to 3. |

### Combination Constants — For identifying the instance of the GenTL Producer library identifier to use

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the GenTL Producer (library) available to this Aurora Imaging Library GenTL system.

> **Board availability:** GenTL

| Value | Description |
| --- | --- |
| `M_GENTL_PRODUCER` | Specifies the GenTL Producer (library) available to this Aurora Imaging Library GenTL system. |

### Combination Constants — For specifying which configuration information file to access

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify about which interface type to inquire.

The GenTL interface refers to the type of physical connection (port) between the Host and the device (such as Camera Link or Ethernet connections). A mixed-type GenTL Producer (library) supports multiple interface types and[`M_GENTL_INTERFACEn`](../../Reference/sys/MsysInquire.md) can be used with any GenTL Producer provided it reports more than one type of interface. You can use the value below to specify about which interface type you would like to inquire.

> **Board availability:** GenTL

| Value | Description |
| --- | --- |
| `M_GENTL_INTERFACEn` | Specifies the GenTL interface to use. |

### Combination Constants — Returns the number of hook threads that were allocated

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the number of hook threads.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of hook threads that were allocated. |

### Combination Constants — For specifying the acquisition path to inquire

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the acquisition path to inquire.

> **Board availability:** Iris GTX

| Value | Description |
| --- | --- |
| `M_DEV0` | Specifies the number of the acquisition path to inquire. |

### Combination Constants — For specifying that the target board is identified using its user-defined name

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set that the board will have a user-defined board name..

You can use the following value on its own, or add it to the above-mentioned value, to specify that the target board is identified using its user-defined name.  > **Note:** This InitFlag is also available for the Zebra Concord PoE base model.

> **Board availability:** Concord PoE

| Value | Description |
| --- | --- |
| `M_DEVICE_NAME` | Specifies that the[`SystemNum`](../../Reference/sys/MsysAlloc.md) parameter identifies the target board using its user-defined name. |

### Combination Constants — For specifying which connector or port to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which connector or port to inquire.

| Value | Description |
| --- | --- |
| `M_CONNECTIONn` | Specifies to inquire connector_n_, where _n_ corresponds to a physical connector or port on your board or industrial computer. |

### For discovering connected devices

The following inquire types allow you to inquire information about discovered devices accessible by the specified system.  > **Note:** Before using these inquire types, you must refresh the list of discovered devices for the specified system using[`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_DISCOVER_DEVICE`](../../Reference/sys/MsysControl.md). This list is cached; if devices are connected or disconnected, you must refresh the list (until you do, using these inquire types will return outdated information).

> **Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

---

### `M_DISCOVER_DEVICE_ADDRESS + n`

**Board availability:** GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF

Inquires the IP address (IPv4) or the connector of device _n_ as a string, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

| Value | Description |
| --- | --- |
| `"BoardString"` | Specifies the string containing information about the board type, board device number (previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_DEVn`](../../Reference/sys/MsysAlloc.md)), and the acquisition path (previously allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) with [`M_DEVn`](../../Reference/dig/MdigAlloc.md)). **[Rapixo CXP, Rapixo CoF]** |
| `"nnn.nnn.nnn.nnn"` | Specifies the IP address of the GigE Vision-compliant grabberless interface camera, as a string. The address string is expressed in dotted decimal notation, where each dotted decimal (_nnn_) is a number between 000 and 255. **[GevIQ, GigE Vision]** |

---

### `M_DISCOVER_DEVICE_ALLOCATION_STATUS + n`

Inquires whether device _n_ has been allocated, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEVICE_ALLOCATED` | Specifies that the device is allocated (either by this application, or by another application). |
| `M_DEVICE_FREE` | Specifies that the device is not allocated. You can allocate the device using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). |
| `M_DEVICE_UNREACHABLE` | Specifies that the device is unreachable. **[GevIQ, GigE Vision, USB3 Vision]** |
| `M_DEVICE_UNREACHABLE_ON_DHCP_NETWORK` | Specifies that the device is unreachable on the DHCP network.  The device has DHCP disabled and is using a static IP or link-local address (LLA). Connect the device peer-to-peer to resolve the issue.  > **Note:** Note that to connect the device peer-to-peer, you must enable DHCP and disable static IP on the device. **[GevIQ, GigE Vision]** |

---

### `M_DISCOVER_DEVICE_COUNT`

Inquires the number of devices that are accessible by the specified system.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of devices that are accessible by the specified system. |

---

### `M_DISCOVER_DEVICE_DIGITIZER_NUMBER + n`

Inquires the digitizer number that you can pass to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) to allocate device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEVn` | Specifies the digitizer number. |

---

### `M_DISCOVER_DEVICE_INTERFACE_NAME + n`

Inquires the name of the host interface used by device _n_, where_n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

---

### `M_DISCOVER_DEVICE_INTERFACE_TYPE + n`

Inquires the type of interface used by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

| Value | Description |
| --- | --- |
| `M_CLHS` | Specifies that the device is connected using Camera Link HS. **[GenTL]** |
| `M_COF` | Specifies that the device is connected using CoaXPress-over-Fiber. **[GenTL, Rapixo CoF]** |
| `M_CUSTOM` | Specifies that the device is connected using a custom/non-standard interface type. **[GenTL]** |
| `M_CXP` | Specifies that the device is connected using CoaXPress. **[GenTL, Rapixo CXP]** |
| `M_GIGE_VISION` | Specifies that the device is connected using GigE Vision. **[GenTL, GevIQ, GigE Vision]** |
| `M_USB3_VISION` | Specifies that the device is connected using USB3 Vision. **[GenTL, USB3 Vision]** |
| `M_V4L2` | Specifies that the device is connected using Video4Linux2. **[V4L2]** |

---

### `M_DISCOVER_DEVICE_MANUFACTURER_INFO + n`

Inquires the manufacturer-specific information string reported by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).  If the camera does not provide this information, an empty string "\0" is written.

---

### `M_DISCOVER_DEVICE_MANUFACTURER_NAME + n`

Inquires the manufacturer name string reported by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).  If the camera does not provide this information, an empty string "\0" is written.

---

### `M_DISCOVER_DEVICE_MODEL_NAME + n`

Inquires the model name string reported by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).  If the camera does not provide this information, an empty string "\0" is written.

---

### `M_DISCOVER_DEVICE_SERIAL_NUMBER + n`

Inquires the serial number of device _n_, where_n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

---

### `M_DISCOVER_DEVICE_UNIQUE_IDENTIFIER + n`

Inquires the unique identifier reported by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).

---

### `M_DISCOVER_DEVICE_USER_NAME + n`

Inquires the user-defined name string reported by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).  Refer to your camera manual to learn how to set this name string.  If the camera does not provide this information, an empty string "\0" is written.

---

### `M_DISCOVER_DEVICE_VERSION + n`

Inquires the version number string reported by device _n_, where _n_ is a number between 0 and the number of discovered devices ([`M_DISCOVER_DEVICE_COUNT`](../../Reference/sys/MsysInquire.md)).  If the camera does not provide this information, an empty string "\0" is written.

### For inquiring I/O signals and their mode

The following inquire types and inquire values allow you to inquire the number, mode, and the purpose of I/O signals (such as, auxiliary signal). Once the format, routing, and mode are determined for an I/O signal, you can further inquire the I/O signal using the inquire types described in the [`GroupUserBits`](../../Reference/GroupUserBits.md) table.  > **Note:** Note that for other Zebra imaging boards that have auxiliary I/O signals, but are not supported with the constants below, see [`MdigInquire`](../../Reference/dig/MdigInquire.md).

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_AUX_IO_COUNT`

Inquires the total number of auxiliary signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of auxiliary signals. |

---

### `M_AUX_IO_COUNT_IN`

Inquires the total number of auxiliary input and I/O signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of auxiliary input and I/O signals. |

---

### `M_AUX_IO_COUNT_OUT`

Inquires the total number of auxiliary output and I/O signals.

| Value | Description |
| --- | --- |
| `Value` | Specifies the total number of auxiliary output and I/O signals. |

---

### `M_IO_DEBOUNCE_TIME`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the amount of time that the specified auxiliary input signal is debounced.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the minimum amount of time to ignore any additional signal transitions after accepting a signal transition, in nsec. |

---

### `M_IO_FORMAT`

**Board availability:** Concord PoE, Host System, Indio

Inquires the format for an I/O signal. For more information, refer to the Technical information appendix in the _Installation and Hardware Reference_ manual for your Zebra imaging board.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_CURRENT_SOURCE` | Specifies that the specified I/O is an current-controlled output. |
| `M_OPEN_DRAIN` | Specifies that the specified I/O is an open collector (open drain) I/O signal. |
| `M_OPTO` | Specifies that the specified I/O is an opto-coupled I/O signal. |

---

### `M_IO_GLITCH_FILTER_STATE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires whether the glitch filter is enabled.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to use a glitch filter. |
| `M_ENABLE` | Specifies to use a glitch filter. |

---

### `M_IO_INTERRUPT_ACTIVATION`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the signal transition upon which to generate an interrupt, if interrupt generation has been enabled for the specified I/O signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies to generate an interrupt upon both a low-to-high and a high-to-low signal transition. |
| `M_EDGE_FALLING` | Specifies that an interrupt will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that an interrupt will be generated upon a low-to-high signal transition. |

---

### `M_IO_INTERRUPT_STATE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires whether to generate an interrupt upon the specified transition of the I/O signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to generate an interrupt. |
| `M_ENABLE` | Specifies to generate an interrupt. |

---

### `M_IO_INVERTER`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires whether the output of the specified I/O signal should be inverted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to invert the specified I/O signal. |
| `M_ENABLE` | Specifies to invert the specified I/O signal. |

---

### `M_IO_MODE`

**Board availability:** Host System, Indio, Iris GTX

Inquires mode of the specified I/O signal.

| Value | Description |
| --- | --- |
| `M_INPUT` | Specifies that the signal is for input. |
| `M_OUTPUT` | Specifies that the signal is for output. |

---

### `M_IO_SOURCE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the type of signal to route to an output signal, or an I/O signal set to output mode.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EXPOSURE` | Specifies to route the exposure signal of the camera. |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal. |
| `M_IO_COMMAND_LISTn` | Specifies to route a bit of the I/O command register of I/O command list _n_, where _n_ is the number of the I/O command list. |
| `M_ROTARY_ENCODERn` | Specifies to route the output of rotary encoder _n_, where _n_ is the number of rotary encoders available. |
| `M_TIMER_STROBE` | Specifies to route the internal timer strobe signal. |
| `M_TIMERn` | Specifies to route the output of timer _n_, where _n_ is the number of timers available. |
| `M_USER_BITn` *(default)* | Specifies to route the state of bit n of the main static-user-output register, where _n_ is the bit number. |

---

### `M_IO_STATUS`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the status of the specified I/O signal.

#### System specific

| Board(s) | Note |
|---|---|
| Iris GTX | Inquiring the status of signals dedicated for trigger inputs will generate an error. |
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the I/O signal is disabled. **[Host System]** |
| `M_OFF` | Specifies that the I/O signal is off. |
| `M_ON` | Specifies that the I/O signal is on. |
| `M_UNKNOWN` | Specifies that the I/O signal cannot be inquired with its current configuration. |

---

### `M_IO_STATUS_ALL`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the status of all available I/O signals. Note that if there are I/O signals that cannot be inquired, the bits representing those signals, in the bit-encoded value returned, are not necessarily valid; these bits should be ignored.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the bit-encoded value representing the status of all available and inquirable I/O signals. |

### Combination Constants — For inquiring the type and number of the I/O signal

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the type and number of the I/O signal.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to affect auxiliary signal n, where _n_ is the signal number. |

### Combination Constants — For specifying the type of I/O signal to inquire

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the type of I/O signal to inquire.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_AUX_IO` | Specifies to inquire all auxiliary signals. |

### For inquiring the state of specified user-bits in a static-user-output register

The following inquire types and inquire values specify the settings for specified bits in a static-user-output register. The user-bits are the bits associated with output signals or I/O signals set to output. To establish which user-bits can be routed to a specific signal, see the _connectors and signal names_ section of the _Aurora Imaging Library Hardware-specific Notes_ chapter for your Zebra imaging board.  > **Note:** Note that for other Zebra imaging boards that have user-bits, but are not supported with the constants below, see [`MdigInquire`](../../Reference/dig/MdigInquire.md).

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_USER_BIT_COUNT`

Inquires the total number of bits of the main static-user-output register.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of bits in the main static-user-output register. |

---

### `M_USER_BIT_STATE`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the state of the specified bit in a static-user-output register.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_OFF` | Specifies that the specified bit is set to off. |
| `M_ON` | Specifies that the specified bit is set to on. |

---

### `M_USER_BIT_STATE_ALL`

**Board availability:** Concord PoE, Host System, Indio, Iris GTX

Inquires the state of all the bits in the main static-user-output register.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value` | Specifies a bit-encoded value that establishes the value of all the bits of the specified static-user-output register. |

### Combination Constants — For inquiring the bit in the static-user-output register

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the bit in the static-user-output register.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_USER_BITn` | Specifies to affect bit n of the main static-user-output register. |

### For inquiring the settings of a timer

The following inquire types and inquire values allow you to inquire about timers and the signals generated from a timer (timer output signals). For more information, see [Timers and coordinating events](../../UserGuide/C56_IO_signals/S06_Timers_and_coordinating_events.md).

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_TIMER_ARM`

Inquires whether to enable the timer arming mechanism.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that timer arming is disabled. |
| `M_ENABLE` | Specifies that timer arming is enabled. |

---

### `M_TIMER_ARM_ACTIVATION`

Inquires the signal transition upon which to arm the timer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the timer will be armed by both a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the timer will be armed upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the timer will be armed upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer is continuously armed during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer is continuously armed during a low signal polarity. |

---

### `M_TIMER_ARM_SOURCE`

Inquires which input signal will arm the timer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the source signal used to arm the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_EXPOSURE` | Specifies to use the exposure signal as the trigger source. |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal to the specified signal. |
| `M_SOFTWARE` | Specifies to use software to arm the specified timer. |
| `M_TIMER_STROBE` | Specifies to route the strobe's timer signal to the specified signal. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the source signal to arm the timer, where _n_ is the timer number. |

---

### `M_TIMER_CLOCK_ACTIVATION`

Inquires the edge of the signal that will increment the clock used to control the active portion of the timer's output signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the clock will be incremented by both a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the clock will be incremented by a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the clock will be incremented by a low-to-high signal transition. |

---

### `M_TIMER_CLOCK_FREQUENCY`

Inquires the frequency of the clock source signal for the active portion of the timer's output signal ([`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysInquire.md)).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the signal is not periodic or the frequency is unknown. |
| `0 < Value <= Frequency of M_SYSCLK` | Specifies the frequency, in Hz. |

---

### `M_TIMER_CLOCK_SOURCE`

Inquires the source of the clock that drives the active portion of the specified timer's output signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the clock source, where _n_ is the number of the auxiliary signal. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the clock source, where _n_ is the number of the rotary decoder. |
| `M_SYSCLK` *(default)* | Specifies to use the allocated system's clock source. |

---

### `M_TIMER_DELAY`

Inquires the delay between the timer trigger and the active portion of the timer's output signal.  Note, an error is generated if the specified delay cannot be respected.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= Max. value` | Specifies the delay. |

---

### `M_TIMER_DELAY_CLOCK_ACTIVATION`

Inquires the signal transition that will increment the clock used to control the delay between the timer's trigger and the active portion of the timer's output signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the clock will be incremented by both a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the clock will be incremented by a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the clock will be incremented by a low-to-high signal transition. |

---

### `M_TIMER_DELAY_CLOCK_FREQUENCY`

Inquires the frequency of the clock source signal for the delay between the timer's trigger and the active portion of the timer's output signal ([`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysInquire.md)).

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_UNKNOWN` | Specifies that the signal is not periodic or the frequency is unknown. |
| `0 < Value <= Frequency of M_SYSCLK` | Specifies the frequency, in Hz. |

---

### `M_TIMER_DELAY_CLOCK_SOURCE`

Inquires the source of the clock that drives the delay between the timer's trigger and the active portion of the specified timer's output signal.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the clock source, where _n_ is the number of the auxiliary signal. |
| `M_FOLLOW_TIMER_CLOCK` *(default)* | Specifies to use the clock source specified by [`M_TIMER_CLOCK_SOURCE`](../../Reference/sys/MsysInquire.md). |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the clock source, where _n_ is the number of the rotary decoder. |
| `M_SYSCLK` | Specifies to use the allocated system's clock source. |

---

### `M_TIMER_DURATION`

Inquires the duration for the active portion of the timer's output signal.  Note, an error is generated if the specified duration cannot be respected.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= Max. value` | Specifies the duration of the active portion of the timer output signal. |

---

### `M_TIMER_OUTPUT_INVERTER`

Inquires whether the output of the timer should be inverted. This causes the low portion of the signal (the delay period) to be high and the high portion of the signal (the active portion) to be low.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to invert the output of the timer. |
| `M_ENABLE` | Specifies to invert the output of the timer. |

---

### `M_TIMER_STATE`

Inquires the state of the specified timer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the timer is disabled. |
| `M_ENABLE` | Specifies that the timer is enabled. |

---

### `M_TIMER_TRIGGER_ACTIVATION`

Inquires the signal variation upon which to generate a timer trigger, if the specified timer is enabled.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that a timer trigger will be generated both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that a timer trigger will be generated upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that a timer trigger will be generated upon a low-to-high signal transition. |
| `M_LEVEL_HIGH` | Specifies that a timer trigger is continuously issued during a high signal polarity. |
| `M_LEVEL_LOW` | Specifies that a timer trigger is continuously issued during a low signal polarity. |

---

### `M_TIMER_TRIGGER_OVERLAP`

Inquires how to deal with a new trigger that occurs while the previously triggered timer (both its delay and duration) has not expired.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LATCH` | Specifies that a trigger received, while the associated timer has not expired, will be latched (stored). |
| `M_OFF` *(default)* | Specifies that a new trigger is ignored. |
| `M_RESET` | Specifies that a new trigger automatically resets the timer (regardless of whether it is in its delay or active period) and then restarts the timer. |

---

### `M_TIMER_TRIGGER_SOURCE`

Inquires the trigger source for the specified timer.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_CONTINUOUS` | Specifies to run the specified timer in periodic mode; no actual trigger signal is used. |
| `M_EXPOSURE` | Specifies to use the exposure signal as the trigger source. |
| `M_GRAB_TRIGGER_READY` | Specifies to route the internal grab trigger ready signal to the specified signal. |
| `M_IO_COMMAND_LISTn` | Specifies to use a bit of the I/O command register of I/O command list _n_, where _n_ is the number of the I/O command list. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder _n_ as the trigger source, where _n_ is the number of the rotary decoder. |
| `M_SOFTWARE` | Specifies to use a software trigger as the trigger source. |
| `M_TIMER_STROBE` | Specifies to route the strobe's timer signal to the specified signal. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the timer number. |

---

### `M_TIMER_USAGE`

Inquires how the timer should be used.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PULSE_GENERATION` *(default)* | Specifies the normal use of the timer, as described in [Timers and coordinating events](../../UserGuide/C56_IO_signals/S06_Timers_and_coordinating_events.md). |
| `M_PULSE_MEASUREMENT` | Specifies to use the timer to measure the duration of the pulse that occurs on the timer's trigger source. |

---

### `M_TIMER_VALUE`

Inquires the current value of the timer's duration.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies current value of the timer's duration. If [`M_TIMER_DELAY_CLOCK_SOURCE`](../../Reference/sys/MsysInquire.md) is set to [`M_SYSCLK`](../../Reference/sys/MsysInquire.md) or a frequency is specified using [`M_TIMER_DELAY_CLOCK_FREQUENCY`](../../Reference/sys/MsysInquire.md), this value is specified in nsec. Otherwise, it is specified in number of signal transitions on the source signal. |

### Combination Constants — For specifying which on-board timer to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which on-board timer to inquire.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_TIMERn` | Specifies on-board timer _n_, where _n_ is the number of the timer. |

### Combination Constants — For inquiring the maximum or minimum value for the setting

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the maximum or minimum value for the setting.

> **Board availability:** Concord PoE, Host System, Indio

| Value | Description |
| --- | --- |
| `M_MAX_VALUE` | Specifies the maximum value for this setting. |
| `M_MIN_VALUE` | Specifies the minimum value for this setting. |

### For inquiring the settings of a rotary decoder

The following inquire types and inquire values allow you to inquire about the settings of a rotary decoder.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

---

### `M_ROTARY_ENCODER_BIT0_SOURCE`

Inquires the auxiliary input signal on which to receive bit 0 of the 2-bit Gray code.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the signal on which to receive bit 0 of the 2-bit Gray code, where _n_ is the number of the auxiliary signal. |

---

### `M_ROTARY_ENCODER_BIT1_SOURCE`

Inquires the auxiliary input signal on which to receive bit 1 of the 2-bit Gray code.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the signal on which to receive bit 1 of the 2-bit Gray code, where _n_ is the number of the auxiliary signal. |

---

### `M_ROTARY_ENCODER_OUTPUT_MODE`

Inquires the rotary decoder's counter value and/or the direction of movement upon which the rotary decoder should output a pulse.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_POSITION_TRIGGER` | Specifies to output a pulse upon the trigger generated by [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md). |
| `M_STEP_ANY` | Specifies to output a pulse upon any change in the rotary decoder's counter value (position change in any direction). |
| `M_STEP_BACKWARD` | Specifies to output a pulse upon a rotary decoder counter decrement only. |
| `M_STEP_FORWARD` | Specifies to output a pulse upon a rotary decoder counter increment only. |
| `M_STEP_FORWARD_NEW_POSITIVE` | Specifies to output a pulse upon a rotary decoder counter increment of a new value that has not been reached before. |

---

### `M_ROTARY_ENCODER_POSITION`

Inquires the current value of the rotary decoder's counter.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= 429497295` | Specifies the current value of the counter. |

---

### `M_ROTARY_ENCODER_POSITION_TRIGGER`

Inquires the value of the rotary decoder's counter upon which a trigger is generated.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `0 <= Value <= 0xFFFFFFFF` | Specifies the value of the counter upon which a trigger is generated. |

---

### `M_ROTARY_ENCODER_RESET_ACTIVATION`

Inquires the signal transition upon which to reset the rotary decoder's counter to 0.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EDGE_FALLING` | Specifies to reset the rotary decoder upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies to reset the rotary decoder upon a low-to-high signal transition. |

---

### `M_ROTARY_ENCODER_RESET_SOURCE`

Inquires the signal source to use to reset the rotary decoder's counter to 0.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies not to reset using a hardware signal source. |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source for the specified timer, where _n_ is the number of the auxiliary signal. |
| `M_POSITION_TRIGGER` | Specifies to use the trigger signal generated by the rotary decoder when the counter reaches the value specified with [`M_ROTARY_ENCODER_POSITION_TRIGGER`](../../Reference/sys/MsysControl.md). |

---

### `M_ROTARY_ENCODER_STATE`

Inquires whether the specified rotary decoder is enabled.

#### System specific

| Board(s) | Note |
|---|---|
| Host System | This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the rotary decoder is disabled. |
| `M_ENABLE` | Specifies that the rotary decoder is enabled. |

### Combination Constants — For specifying which rotary decoder to inquire about

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which rotary decoder to inquire.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies rotary decoder _n_, where _n_ is the number of the rotary decoder. |

### For controlling the settings of a data latch associated with rotary encoders

The following control types allow you to control the settings of a data latch associated with the rotary decoders of your system.

> **Board availability:** Host System

---

### `M_SYS_DATA_LATCH_STATE`

Inquires the state of the specified data latch.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the data latch is disabled. |
| `M_ENABLE` | Specifies that the data latch is enabled. |

---

### `M_SYS_DATA_LATCH_TRIGGER_ACTIVATION`

Inquires the trigger signal transition upon which to store the specified information to the specified data latch.  To set the signal with which to trigger the data latch, use [`M_SYS_DATA_LATCH_TRIGGER_SOURCE`](../../Reference/sys/MsysInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY_EDGE` | Specifies that the specified information is stored in the data latch both upon a high-to-low and a low-to-high signal transition. |
| `M_EDGE_FALLING` | Specifies that the specified information is stored in the data latch upon a high-to-low signal transition. |
| `M_EDGE_RISING` *(default)* | Specifies that the specified information is stored in the data latch upon a low-to-high signal transition. |

---

### `M_SYS_DATA_LATCH_TRIGGER_SOURCE`

Inquires what triggers storing the specified information to the specified data latch.

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. |
| `M_TIMERn` | Specifies to use the output signal of timer _n_ as the trigger source, where _n_ is the number of the timer. |

---

### `M_SYS_DATA_LATCH_TYPE`

Inquires which rotary decoder the specified data latch will store the position counter value of when the data latch is triggered.

| Value | Description |
| --- | --- |
| `M_ROTARY_ENCODERn` | Specifies to store the value of the counter of rotary decoder _n_, where _n_ is a number between 1 and 2. |

### Combination Constants — For specifying which data latch to set

> *Essential, cannot be used alone.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which data latch to affect.

> **Board availability:** Host System

| Value | Description |
| --- | --- |
| `M_LATCHn` | Specifies which data latch to inquire, where_n_ is 1 or 2. |

### For UART settings

The following inquire types allow you to inquire UART settings.

---

### `M_COM_PORT_NUMBER`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the Microsoft Windows COM port number associated with the specified UART. Note that the number of UARTs available differs, depending on the version of your board.  You must specify which COM port to inquire. See below for combination values.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Microsoft Windows COM port number associated with the specified UART. |

---

### `M_UART_BYTES_READ`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of bytes read from the UART, after waiting for the UART read operation (using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md)) to complete.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of bytes read from the UART. |

---

### `M_UART_BYTES_WRITTEN`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of bytes written to the UART, after waiting for the UART write operation (using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_UART_WRITE_STRING`](../../Reference/sys/MsysControl.md)) to complete.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of bytes written to the UART, in bytes. |

---

### `M_UART_DATA_PENDING`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of pending data bytes that are currently available to read from the UART input buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of pending data bytes. |

---

### `M_UART_DATA_SIZE`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of data bits per character that is sent or received by the UART.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `7` | Specifies that the data length is 7 bits. |
| `8` *(default)* | Specifies that the data length is 8 bits. |

---

### `M_UART_INTERFACE_TYPE`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the type of interface used by the UART.

| Value | Description |
| --- | --- |
| `M_RS232` | Specifies that an RS-232 interface is used. |
| `M_RS485` | Specifies that an RS-485 interface is used. |

---

### `M_UART_PARITY`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires whether character data is sent or received with a parity bit and how the parity bit is set.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that no extra bit is added (no parity). |
| `M_EVEN` | Specifies that the number of 1's will be even. |
| `M_ODD` | Specifies that the number of 1's will be odd. |

---

### `M_UART_READ_STRING_MAXIMUM_SIZE`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the maximum length of the string to be read using [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum length of the string, in bytes. |

---

### `M_UART_READ_STRING_SIZE`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of bytes to read when calling [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md). If this length is specified by [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysControl.md), the returned result will be [`M_DEFAULT`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the use of [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysInquire.md) to delineate the end of the string to read. |
| `Value` | Specifies the string length, in bytes. |

---

### `M_UART_SPEED`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the baud rate of the UART.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `300;600;1200;1800;2400;4800;7200;9600;14400;19200;38400;57600;115200` *(default)* | Specifies the baud rate of the UART. |

---

### `M_UART_STOP_BITS`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the number of extra data bit(s) that are added to each character to indicate the end of a character.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `1` *(default)* | Specifies that there is 1 stop bit. |
| `2` | Specifies that there are 2 stop bits. |

---

### `M_UART_STRING_DELIMITER`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the character used to terminate strings of incoming or outgoing data. The delimiter is used but not sent when writing data with [`M_UART_WRITE_STRING`](../../Reference/sys/MsysControl.md); it is read for incoming data with [`M_UART_READ_STRING`](../../Reference/sys/MsysControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the character used to terminate strings. |

---

### `M_UART_TIMEOUT`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the maximum time to wait between each byte when reading incoming data.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Waits indefinitely. |
| `Value` | Specifies the time to wait, in msec. |

---

### `M_UART_WRITE_STRING_SIZE`

**Board availability:** Radient eV-CL, Rapixo CL

Inquires the length of the string to be sent to the UART for transmission.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the use of [`M_UART_STRING_DELIMITER`](../../Reference/sys/MsysInquire.md) to end the string. |
| `Value` | Specifies the string length, in bytes. |

### Combination Constants — For COM Ports and UARTs

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set which UART to inquire.

> **Board availability:** Radient eV-CL, Rapixo CL

| Value | Description |
| --- | --- |
| `M_UART_NB` | Specifies which UART should be inquired. |

### For inquiring about an action command

The following inquire types and inquire values allow you to inquire the details of an action command. Action commands require both an Aurora Imaging Library-side and a camera-side configuration. The following inquire types and inquire values inquire the Aurora Imaging Library-side of the action command. To inquire the camera-side, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) with the appropriate feature values. For more information, refer to Triggering simultaneous actions in multiple GigE Vision cameras.  > **Note:** To inquire about features of the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.

> **Board availability:** Concord PoE, GevIQ, GigE Vision

---

### `M_GC_ACTION_ACKNOWLEDGE_NUMBER`

**Board availability:** GevIQ, GigE Vision

Inquires the number of acknowledgments that Aurora Imaging Library should expect to receive when the action command is issued.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the number of cameras added to the action command, using [`M_GC_ACTION_ADD_DEVICE`](../../Reference/sys/MsysInquire.md). |
| `Value` | Specifies the number of acknowledgments expected. |

---

### `M_GC_ACTION_DEVICE_KEY`

Inquires the action device key for the action command. The action device key identifies the cameras on which the action should be performed.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** To inquire the features of the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.  > **Note:** If using an Aurora Imaging Library Concord PoE system, this inquire type is only available for Zebra Concord PoE with ToE. To inquire about an action command when using the Zebra Concord PoE base model, use an Aurora Imaging Library GigE system with this inquire type. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the action device key. |

---

### `M_GC_ACTION_GROUP_KEY`

Inquires the action group key for the action command. The action group key identifies which action you want to perform on the camera.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** To inquire the features of the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.  > **Note:** If using an Aurora Imaging Library Concord PoE system, this inquire type is only available for Zebra Concord PoE with ToE. To inquire about an action command when using the Zebra Concord PoE base model, use an Aurora Imaging Library GigE system with this inquire type. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the action group key. |

---

### `M_GC_ACTION_GROUP_MASK`

Inquires the action group mask for the action command. In the case where you need one (or more) cameras to temporarily ignore an action command, you can mask out the action command by changing the action group mask.

#### System specific

| Board(s) | Note |
|---|---|
| Concord PoE | > **Note:** To inquire the features of the camera, use [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)with a digitizer allocated for the camera on an Aurora Imaging Library GigE system.  > **Note:** If using an Aurora Imaging Library Concord PoE system, this inquire type is only available for Zebra Concord PoE with ToE. To inquire about an action command when using the Zebra Concord PoE base model, use an Aurora Imaging Library GigE system with this inquire type. |

| Value | Description |
| --- | --- |
| `Value` | Specifies the action group mask. |

---

### `M_GC_ACTION_TIME`

**Board availability:** GevIQ, GigE Vision

Inquires the time at which the action command should execute.

| Value | Description |
| --- | --- |
| `Value` | Specifies the time at which to execute the action command on your camera, relative to the time at which the action command was sent, in sec. |

### For inquiring a Trigger-over-Ethernet packet (action command or GigE Vision software trigger) for transmission using a ToE module

The following inquire types and inquire values allow you to inquire the details of a Trigger-over-Ethernet packet for transmission using a ToE module. The Trigger-over-Ethernet packet can be sent as an action command or a GigE Vision software trigger. Action commands and GigE Vision software triggers require both an Aurora Imaging Library-side and a camera-side configuration. The following inquire types and inquire values inquire about the Aurora Imaging Library-side of the action command or GigE Vision software trigger. To configure the camera-side, you need to use a digitizer allocated for the camera on an Aurora Imaging Library GigE Vision system and use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate feature values. For more information, refer to Triggering simultaneous actions in multiple GigE Vision cameras.

> **Board availability:** Concord PoE

---

### `M_GC_TRIGGER_SELECTOR`

Inquires the type of GigE Vision trigger that should take place on the camera upon receiving the ToE packet (for example, FrameStart). For the camera to know what should be triggered upon receiving the packet, this control type must match the trigger selector on your camera. To configure the trigger selector on your camera, use [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the appropriate GenICam SFNC feature (for example, TriggerSelector).

| Value | Description |
| --- | --- |
| `"FeatureName"` | Specifies the type of trigger; see your GigE Vision camera's documentation for a list of available types. |

---

### `M_TRIGGER_SOURCE`

Inquires the event that will cause the ToE module to send the specified action command or GigE Vision software trigger as a ToE packet.

| Value | Description |
| --- | --- |
| `M_AUX_IOn` | Specifies to use auxiliary input signal _n_ as the trigger source, where _n_ is the number of the auxiliary signal. |
| `M_IO_COMMAND_LISTn` | Specifies to use the I/O command list _n_, where _n_ is the number of the I/O command list. |
| `M_ROTARY_ENCODERn` | Specifies to use rotary decoder n as the trigger source, where _n_ is the number of the rotary decoder. |
| `M_SOFTWAREn` | Specifies to use software as a trigger source to trigger the ToE module, where _n_ is the number of the software trigger; _n_ can be a value between 1 and 4. |
| `M_TIMERn` | Specifies to use the output signal of the specified timer as the trigger source, where _n_ is the number of the timer. |

---

### `M_TRIGGER_STATE`

Inquires the state of the specified action command or GigE Vision software trigger in the ToE module.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies the ToE packet is disabled. |
| `M_ENABLE` | Specifies the ToE packet is enabled, and will be transmitted when its associated event (specified using [`M_TRIGGER_SOURCE`](../../Reference/sys/MsysInquire.md)) occurs. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — For specifying which action command to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set which action command to inquire.

> **Board availability:** Concord PoE, GevIQ, GigE Vision

| Value | Description |
| --- | --- |
| `M_GC_ACTIONn` | Specifies to inquire about action command _n_, where _n_ is a value from 0 to 31. |

### Combination Constants — For specifying which GigE Vision software trigger to inquire

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which GigE Vision software trigger to inquire.

> **Board availability:** Concord PoE

| Value | Description |
| --- | --- |
| `M_GC_TRIGGER_SOFTWAREn` | Specified to inquire about the GigE Vision software trigger n, where n is a value from 0 to 31. This combination value cannot be used with[`M_GC_ACTION_DEVICE_KEY`](../../Reference/sys/MsysInquire.md), [`M_GC_ACTION_GROUP_KEY`](../../Reference/sys/MsysInquire.md), or [`M_GC_ACTION_GROUP_MASK`](../../Reference/sys/MsysInquire.md).  > **Note:** If using an Aurora Imaging Library Concord PoE system, this inquire type is only available for Zebra Concord PoE with ToE. |

### Combination Constants — For specifying which I/O command register bit was used

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify which I/O command register bit was used.

> **Board availability:** Concord PoE, Host System, Indio, Iris GTX

| Value | Description |
| --- | --- |
| `M_IO_COMMAND_BITn` | Specifies I/O command register bit _n_, where _n_ represents the bit number. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

The following example uses [`M_BOARD_TYPE`](../../Reference/sys/MsysInquire.md). The returned value is then masked so that only the board type is returned.

> **Code example:** [reference.MsysInquire.M_BOARD_TYPE](reference.MsysInquire.M_BOARD_TYPE)

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 8 to 15.

For Zebra 4Sight EV6/EV7, _n_ can be either 1 or 2.

For Zebra 4Sight EV6/EV7, _n_ is a number from 1 to 4.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 1 to 16.

For Zebra 4Sight EV6/EV7, _n_ can be a value from 0 to 7.
