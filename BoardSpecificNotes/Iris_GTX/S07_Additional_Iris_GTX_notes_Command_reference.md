---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Additional_Iris_GTX_notes_Command_reference
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Additional Iris GTX notes Command reference"
---

# Miscellaneous reference information for Zebra Iris GTX

This section includes additional command reference information for Zebra Iris GTX. The information found in this section might be a reiteration of content previously documented.

The following modules are mentioned in this section:

- [Mdig](S07_Additional_Iris_GTX_notes_Command_reference.md).

## Mdig

This section presents information about Zebra Iris GTX and the digitizer module.

### MdigControl/Inquire

The following lists additional Iris GTX support when using [`MdigControl`](../../Reference/dig/MdigControl.md)/[`MdigInquire`](../../Reference/dig/MdigInquire.md).

- The minimum supported exposure time that can be set using [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_EXPOSURE_TIME** is 50 microseconds for monochrome sensors. The inquire of **M_EXPOSURE_TIME + M_MIN_VALUE** returns 50 microseconds.
- The Zebra Iris GTX supports **M_GRAB_SCALE_X** for all values from 1 to 1/16 and**M_GRAB_SCALE_Y** for 1 and 1/2.
  The value for **M_SOURCE_SIZE_Y** or** M_SOURCE_OFFSET_Y** must be a multiple of 4. Also, only **M_SOURCE_SIZE_Y** has an effect on the frame rate or grab period. The **M_SOURCE_SIZE_X** does not have an effect on the grab period.
- The value for **M_SOURCE_SIZE_Y** or **M_SOURCE_OFFSET_Y** must be a multiple of 4.
  **M_SOURCE_SIZE_Y** has an effect on the frame rate or grab period, whereas the **M_SOURCE_SIZE_X** does not have an effect on the grab period.
- **M_SOURCE_SIZE_Y** can be set to 4 only if **M_GRAB_SCALE_Y** is set to 1.

> **Note:** You should avoid grabbing using the Zebra Iris GTX with a 10-bit grab resolution or allocating a grab buffer with the **M_MONO16** attribute.
