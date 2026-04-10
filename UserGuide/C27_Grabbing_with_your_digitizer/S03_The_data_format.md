---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: The_data_format
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / The data format"
---

# Data format

[`MdigAlloc`](../../Reference/dig/MdigAlloc.md) needs the digitizer configuration format (DCF) that corresponds to your camera to perform the digitizer allocation. The DCF defines such settings as the input frequency and resolution, and will determine limits when grabbing an image. Once a digitizer has been allocated, many of the settings are established based on the selected DCF. You can change these settings to values other than the default using [`MdigControl`](../../Reference/dig/MdigControl.md) and other [`Mdig...`](../../Reference/dig/MdigAlloc.md) functions. You can use [`MdigInquire`](../../Reference/dig/MdigInquire.md) to inquire about a digitizer's settings.

Aurora Imaging Library provides a number of predefined DCFs for the basic cameras supported by your frame grabber. Aurora Imaging Library provides a few alternate DCF files that you can load if the predefined DCFs do not suit your needs.

If you find a DCF file that is appropriate for your camera, but you need to adjust some of the more common settings, you can do so directly, without adjusting the file, using the [`Mdig...`](../../Reference/dig/MdigControl.md) functions. For more specialized adjustments, you can adjust the DCF file itself, using Aurora Imaging Intellicam.

If you cannot find an appropriate DCF file because, perhaps, you have a non-standard camera or other video source, you can create your own DCF file, using Intellicam. For more information on Intellicam, refer to the _Aurora Imaging Intellicam User Guide_ manual.

If you cannot develop the required DCF using Intellicam, you should provide the camera specifications to your Zebra Technical Support Engineer. A suitable customized DCF file can then be developed, if your frame grabber supports the camera.
