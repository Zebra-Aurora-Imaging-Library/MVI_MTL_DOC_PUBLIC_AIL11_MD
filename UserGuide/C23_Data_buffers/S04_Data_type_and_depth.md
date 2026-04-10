---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Data_type_and_depth
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Data type and depth"
---

# Data type and depth

The data depth of a buffer indicates the number of bits per band in the buffer (1, 8, 16, 32). The data type of a buffer indicates how its data is internally represented (that is, whether the data is considered signed, unsigned, or floating-point). Supported combinations are: 1-bit packed binary; 8-, 16-, and 32-bit integer (signed and unsigned); and 32-bit floating-point. If a function can only operate on data buffers of certain depths, this is explicitly stated in the function's description, otherwise the function can be used with any combination of data buffers.

The packed binary data format represents each pixel by a single bit, in a state of 0 or 1. Therefore, 8 pixels can be packed in a single byte (known as an 8-bit data unit); that is, in a format eight times smaller than an 8-bit image.

Processing done directly on a packed binary buffer is very fast and efficient. Many Aurora Imaging Library functions support accelerated processing using packed binary buffers. General processing functions which do not support packed binary buffers directly, automatically convert the data into a suitable data type buffer, perform the operation, and re-convert the resulting buffer to packed binary.

For efficiency, when possible, you should store binary data in packed binary buffers (rather than, for example, 8-bit integer buffers with only the values 0 and 0xFF). General processing functions that are optimized for packed binary buffers are noted as such in the _Aurora Imaging Library Reference_.

In general, the fewer bits per pixel in a buffer, the faster an operation can be performed on the buffer. Packed binary buffers are the fastest to process. When you need to use integer buffers, use 8 bits per pixel when possible, 16 bits if necessary, and 32 bits as a last resort. When you need non-integer values, extra precision, or a greater dynamic range, you can use floating-point data buffers.
