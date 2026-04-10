---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Averaging_an_input_sequence
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Averaging an input sequence"
---

# Averaging an input sequence

An effective technique to remove random noise is to average a grabbed sequence of the same target image. For instance, Gaussian noise affects the value of any given pixel for each grabbed frame, adding to or subtracting from the actual pixel value. Therefore, over several image acquisitions, this noise averages out to zero. As a rule of thumb, the standard deviation of the noise is reduced by a factor roughly equal to the square root of the number of averaged frames. In other words, the signal-to-noise ratio increases as the square root of the number of frames increases.

With Aurora Imaging Library, you can average an input sequence, using either one of the following operations:

- Adding all input frames and then dividing the result by a specified weight factor. To specify this operation, use [`MimArith`](../../Reference/im/MimArith.md).
- Adding weighted input frames to a weighted accumulator buffer `((I<sub>acc</sub> = _a_I<sub>in</sub> + (1 - _a_)I<sub>acc</sub>) or (I<sub>acc</sub> = _a_(I<sub>in</sub> - I<sub>acc</sub>) + I<sub>acc</sub>))`. To specify this operation, use [`MimArithMultiple`](../../Reference/im/MimArithMultiple.md) with [`M_WEIGHTED_AVERAGE`](../../Reference/im/MimArithMultiple.md).

The latter approach also acts as a temporal filter if the input is changing. This allows you to filter out moving objects from a constant background.

To not lose any frames in your sequence, you can use a technique called multiple buffering, where you can grab data into one buffer while other buffers are processed. For more information, see [Multiple buffering](../C27_Grabbing_with_your_digitizer/S08_Grabbing_and_processing.md), and for an example on how to implement multiple buffering, see the _MdigProcess.cpp_ file in Aurora Imaging Library's example directory.
