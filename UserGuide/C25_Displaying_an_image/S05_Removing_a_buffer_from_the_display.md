---
doctype: UserGuide
part: "2D related information"
chapter: Displaying_an_image
section: Removing_a_buffer_from_the_display
module_tag: disp
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / display / Removing a buffer from the display"
---

# Removing a buffer from the display

To remove an image buffer from the display, you can use [`MdispSelect`](../../Reference/disp/MdispSelect.md) or [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) with [`M_NULL`](../../Reference/disp/MdispSelect.md), depending on which function was used to select the buffer to the display. For Aurora Imaging Library windowed displays, this closes the associated window; whereas for user-defined windowed displays, this leaves the associated window open but leaves it blank. For exclusive displays, this causes the display to disappear, allowing the desktop to reappear on the screen.

To display a different image buffer, you are not required to remove the current buffer from the display; selecting another buffer for display automatically updates the display with the new buffer.

You can only remove the entire image buffer from the display. Therefore, when displaying a parent buffer, you cannot remove one of its child buffers from the display.

Once you have finished using a display, you must free it using [`MdispFree`](../../Reference/disp/MdispFree.md).

Freeing the display, or freeing the buffer currently selected to the display, produces the same visual effect as when using [`MdispSelect`](../../Reference/disp/MdispSelect.md) or [`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) with [`M_NULL`](../../Reference/disp/MdispSelect.md).
