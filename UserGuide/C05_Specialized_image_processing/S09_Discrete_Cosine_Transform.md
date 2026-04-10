---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Specialized_image_processing
section: Discrete_Cosine_Transform
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Specialized-image / Discrete Cosine Transform"
---

# Discrete Cosine Transform

Discrete Cosine Transform (DCT) is mainly used for image JPEG lossy compression. Aurora Imaging Library can perform DCT using [`MimTransform`](../../Reference/im/MimTransform.md). For a one dimensional signal, this separates the signal into a set of cosine waves of different frequency. For a two-dimensional signal (image), it can be interpreted as the decomposition of an image into a set of 2D cosine patterns. The composition of these waves make up the original waveform. The forward DCT is defined as (for an 8x8 matrix):

*[Image: ForwardDCT.png]*

where `_u_` and `_v_` are coordinates in the frequency domain.

*[Image: ForwardDCT2.png]*

The reverse DCT is defined as: *[Image: ReverseDCT.png]*

 where x and y are coordinates in the spatial domain.

Frequency 0, also called the DC component, is plotted in the top-left corner of the spectrum. All other components in the spectrum are called AC components. A DCT concentrates the low frequency components of the image in the first few coefficients (top-left corner) of the spectrum. Aurora Imaging Library divides the image into independent blocks of 8x8 pixels and performs the transform on each individual block. Centering of the spectrum is not supported in Aurora Imaging Library.
