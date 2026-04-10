---
doctype: BoardSpecificNotes
part: ""
chapter: Iris_GTX
section: Using_the_Varioptic_lens
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / iris-gtx / Using the Varioptic lens"
---

# Auto-focusing the Varioptic lens

Zebra Iris GTX supports using the Varioptic Caspian C-39NO-160-I2C or C-39N0-250-I2C lens. This lens can be auto-focused using [`MdigFocus`](../../Reference/dig/MdigFocus.md). Note that this function is only available in the full version of Aurora Imaging Library.

The Varioptic Caspian C-39NO-160-I2C lens can also be controlled using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_FOCUS`](../../Reference/dig/MdigControl.md) + [`M_WAIT`](../../Reference/dig/MdigControl.md) to wait a predetermined amount of time for the camera to focus before calling the user-defined function associated with [`MdigFocus`](../../Reference/dig/MdigFocus.md).

The Varioptic Caspian C-39NO-160-I2C lens has a range of 1024 lens motor positions. When using this or any other type of auto-focusing lens, to determine the minimum and maximum number of lens motor positions, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_FOCUS`](../../Reference/dig/MdigInquire.md) + [`M_MIN_VALUE`](../../Reference/dig/MdigInquire.md), or [`M_FOCUS`](../../Reference/dig/MdigInquire.md) + [`M_MAX_VALUE`](../../Reference/dig/MdigInquire.md), respectively.
