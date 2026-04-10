---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Specifying_the_dimensions_of_a_data_buffer
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Specifying the dimensions of a data buffer"
---

# Specifying the dimensions of a data buffer

Data buffers can have up to three dimensions: an X, Y, and band dimension. Most data buffers have an X dimension (for example, LUT buffers) or an X and Y dimension (for example, monochrome image buffers). The third dimension has been provided to allow you to store data for multiple bands (for example, for each color band used to represent an image). When allocating multi-band buffers, each band will be of the same data depth, type, and size.

Once you finish using a data buffer, you should release its memory space, using [`MbufFree`](../../Reference/buf/MbufFree.md).

*[Image: buffer5d.png]*

Certain Aurora Imaging Library functions support manipulating multi-band image buffers. For more information about multi-band buffers, see [Multi-band buffers](S08_Multi_band_buffers.md). For details on handling color image buffers, see [Dealing with color](../C02_Building_an_application/S11_Dealing_with_color.md).

## Specifying the dimensions of a multi-frame image buffer

A multi-frame image buffer is one in which you plan on storing multiple sequential frames in vertically adjacent locations in the buffer. Multi-frame image buffers are used when grabbing images with a digitizer that supports frame burst technology. To work with a multi-frame image buffer, allocate the image buffer with a height that is the product of the Y-size of each individual frame and the number of frames that will be grabbed into the buffer on each grab command.

Before calling the grab command ([`MdigGrab`](../../Reference/dig/MdigGrab.md) or one grab of [`MdigProcess`](../../Reference/dig/MdigProcess.md)) you must also set the number of sequential frames to grab into your multi-frame image buffer using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_FRAME_BURST_SIZE`](../../Reference/dig/MdigControl.md). The set value defines the number of frames that will be stored continguously in the same buffer. Note that the end-of-grab event only occurs once the entire group of frames has been grabbed, reducing the number of events that need to be handled.
