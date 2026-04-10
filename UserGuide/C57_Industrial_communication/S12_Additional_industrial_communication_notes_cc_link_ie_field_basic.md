---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Additional_industrial_communication_notes_cc_link_ie_field_basic
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Additional industrial communication notes cc link ie field basic"
---

# Miscellaneous user guide information for Industrial Communication: CC-Link IE Field Basic

This section includes additional user guide and command reference information for the CC-Link IE Field Basic Industrial Communication protocol. The information found in this section might be a reiteration of content previously documented.

The CC-Link IE Field Basic protocol implemented in the Industrial Communication module allows an Aurora Imaging Library application to exchange data with a CC-Link IE Field Basic controller device. The Aurora Imaging Library application and executing system are understood to be a CC-Link IE Field Basic worker device. The data exchange supported between the application and controller device using CC-Link IE Field Basic is illustrated below.

*[Image: comm_cc-link.png]*

The following are additional CC-Link IE Field Basic protocol command reference information in the Industrial Communication module.

- The [`McomAlloc`](../../Reference/com/McomAlloc.md) function allocates a CC-Link IE Field Basic communication context. Note that the connection to the controller device is attempted as soon as the Industrial Communication context is allocated. As such, the protocol service must be enabled and configured in Aurora Imaging Configurator prior to allocating the context.
- The [`McomInquire`](../../Reference/com/McomInquire.md) function inquires the number of occupied stations in the CC-Link protocol instance as specified on the Aurora Imaging Configurator **Communication CC-Link** page.
  **M_COM_CCLINK_TOTAL_OCCUPIED_STATION** is the inquire type.
  The return value is an AIL_INT from 1 to 16.
- The [`McomWrite`](../../Reference/com/McomWrite.md) function writes data to a controller device. The function writes the data that is sent to the controller device during its next read cycle to local computer memory. The following is additional information for the parameter values of **DataObjectEntryName**:
  - **M_COM_CCLINK_INPUT_REGISTER**: Specifies to write CC-Link 16-bit input register data in local memory. The function's offset parameter is an offset in 16-bit registers. The offset must be between 0 and `32 * [Number of occupied stations (max=16)]`. The function's size parameter must be set to the number of 16-bit registers.
  - **M_COM_CCLINK_INPUT_FLAG**: Specifies to write CC-Link 1-bit input flag data in local memory. The function's offset parameter is an offset in bits. The offset must be between 0 and `64 * [Number of occupied stations (max=16)]`. The function's size parameter must be set to the number of bits.
- The [`McomRead`](../../Reference/com/McomRead.md) function reads data from a controller device. The function reads the data received from the controller device during its last write cycle from local computer memory. The following is additional information for the parameter values of **DataObjectEntryName**::
  - **M_COM_CCLINK_INPUT_REGISTER**: Specifies to write CC-Link 16-bit input register data in local memory. The function's offset parameter is an offset in 16-bit registers. The offset must be between 0 and `32 * [Number of occupied stations (max=16)]`. The function's size parameter must be set to the number of 16-bit registers.
  - **M_COM_CCLINK_INPUT_FLAG**: Specifies to write CC-Link 1-bit input flag data in local memory. The function's offset parameter is an offset in bits. The offset must be between 0 and `64 * [Number of occupied stations (max=16)]`. The function's size parameter must be set to the number of bits.
  - **M_COM_CCLINK_OUTPUT_REGISTER**: Specifies to read CC-Link 16-bit output register data in local memory. The function's offset parameter is an offset in 16-bit registers. The offset must be between 0 and `32 * [Number of occupied stations (max=16)]`. The function's size parameter must be set to the number of 16-bit registers.
  - **M_COM_CCLINK_OUTPUT_FLAG**: Specifies to read CC-Link 1-bit output flag data in local memory. The function's offset parameter is an offset in bits. The offset must be between 0 and `64 * [Number of occupied stations (max=16)]`. The function's size parameter must be set to the number of bits.
