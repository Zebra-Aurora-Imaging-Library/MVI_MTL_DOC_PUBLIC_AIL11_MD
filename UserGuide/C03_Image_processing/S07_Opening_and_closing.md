---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Opening_and_closing
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Opening and closing"
---

# Opening and closing

Another way of improving the image might be to remove, for example, small particles that have been introduced by dust, or holes in objects. These tasks can generally be accomplished with an opening or closing operation, respectively.

Opening and closing operations determine each pixel's value according to its geometric relationship with neighborhood pixels, and as such are part of a larger group of operations known as morphological operations.

Besides removing small particles, opening operations also break isthmuses or connections between touching objects. Aurora Imaging Library provides the [`MimOpen`](../../Reference/im/MimOpen.md) function to perform a basic opening operation on 3 by 3 neighborhoods taking all neighborhood pixels into account.

Closing operations are very useful in filling holes in objects; however in doing so, they also connect close objects, as shown below. [`MimClose`](../../Reference/im/MimClose.md) performs a standard 3 by 3 closing operation taking all neighborhood pixels into account.

*[Image: cellchdbin.png]*

Note, opening and closing operations work best on binary images.

Since opening is the result of eroding and then dilating an image, and closing is the result of dilating and then eroding an image, you can also customize an opening or closing operation, using [`MimErode`](../../Reference/im/MimErode.md) and [`MimDilate`](../../Reference/im/MimDilate.md). You can perform a binary opening or closing operation using a neighborhood of a predefined shape, using [`MimMorphicShape`](../../Reference/im/MimMorphicShape.md). For more information on erosion and dilation, see [Custom morphological operations](../C04_Advanced_image_processing/S03_Custom_morphological_operations.md).
