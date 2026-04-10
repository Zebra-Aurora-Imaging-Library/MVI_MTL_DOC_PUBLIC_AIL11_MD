---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Grabbing_to_the_display
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Grabbing to the display"
---

# Grabbing to the display

To grab to the display, you should grab into a displayable grab image buffer ([`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_DISP`](../../Reference/buf/MbufAllocColor.md)+[`M_GRAB`](../../Reference/buf/MbufAllocColor.md)) that is selected for display ([`MdispSelect`](../../Reference/disp/MdispSelect.md)). The display of the selected buffer is typically maintained with separate internal buffers in display memory or Host non-paged memory. The selected buffer and the display are maintained and synchronized internally.

The synchronization frequency is dependent upon whether performing a monoshot or continuous grab. A monoshot grab updates the selected buffer first and then its display. A continuous grab updates the specified buffer's display immediately after each frame is grabbed; only the last frame is stored in the selected buffer for processing. If you need to save/process each grabbed frame, you should perform the grab using [`MdigGrab`](../../Reference/dig/MdigGrab.md) (that is, perform a monoshot grab); alternatively, you can use [`MdigProcess`](../../Reference/dig/MdigProcess.md) for its multiple buffering capabilities.
