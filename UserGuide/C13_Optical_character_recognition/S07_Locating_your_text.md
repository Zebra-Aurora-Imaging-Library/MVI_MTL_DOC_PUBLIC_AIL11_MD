---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Locating_your_text
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Locating your text"
---

# Locating your text

Aurora Imaging Library OCR tries to locate the strings to read/verify automatically. Both the angle of the string and the string length information are used to perform the location. Under the appropriate conditions, the location step can be skipped to improve the speed of a read/verify operation, using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SKIP_STRING_LOCATION`](../../Reference/ocr/MocrControl.md).

When using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context, you can only skip the location step if you create a child buffer that contains only the target string and a minimum amount of space around it. Use other modules in Aurora Imaging Library to determine the required buffer size. For more information, refer to [Using child buffers, ROIs, or a copy to manipulate specific data areas](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md).

When using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, string location can be skipped if you are using a clean target image.

If results are unsatisfactory, the image might need further processing, such as using [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_TOP_HAT`](../../Reference/im/MimMorphic.md) or [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md). For more information, refer to [Top and bottom hat](../C04_Advanced_image_processing/S03_Custom_morphological_operations.md).
