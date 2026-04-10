---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Specialized_image_processing
section: Co_occurrence_matrix_statistics
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Specialized-image / Co occurrence matrix statistics"
---

# Co-occurrence matrix statistics

To establish whether two grayscale images contain a similar texture, you can compare their co-occurrence matrix statistics. You use these statistics to characterize the texture; you use them to provide a signature for a given texture. Traditionally, these second-order statistics are used to discern differences in the patterns (textures) in small images (such as tears in fabric). Taking the same statistics from a large series of images allows you to see trends that characterize the pattern/texture. With Aurora Imaging Library, you can compute the co-occurrence matrix of a grayscale image and compute statistics from this matrix, using [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) with an [`M_STATISTICS_CONTEXT`](../../Reference/im/MimAlloc.md) context that has some [`M_STAT_GLCM...`](../../Reference/im/MimControl.md)enabled ([`MimControl`](../../Reference/im/MimControl.md)). The co-occurrence matrix is a normalized version of the frequency distribution of two grayscale values occurring at a specified pixel offset from each other. A co-occurrence matrix (and its corresponding statistics) can be calculated for the entire image or for the neighborhood of every n pixels.

## Building the grayscale co-occurrence matrix

Aurora Imaging Library builds the grayscale co-occurrence matrix (GLCM) for the specified pixel-pair offset, set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_GLCM_PAIR_OFFSET_X`](../../Reference/im/MimControl.md) and [`M_GLCM_PAIR_OFFSET_Y`](../../Reference/im/MimControl.md). It establishes how often each possible combination of two grayscale values occur at this offset from each other, regardless of the order of the values. For example, to evaluate the frequency that two grayscale values occur next to each other in X, set [`M_GLCM_PAIR_OFFSET_X`](../../Reference/im/MimControl.md) to 1 and [`M_GLCM_PAIR_OFFSET_Y`](../../Reference/im/MimControl.md) to 0. If one pixel pair is (3,0), and another pixel pair is (0,3), both are recorded in the co-occurrence matrix as (0,3). To establish the pair of a pixel on the border, mirror overscan is used.

The following table shows an example of how a source image is used to build a co-occurrence matrix.

| Original source image | Building the co-occurrence matrix for a pixel pair offset of (X + 1,Y + 0) | The co-occurrence matrix showing the frequency of each pair of values with an offset of (X + 1,Y + 0) |
| --- | --- | --- |
| *[Image: IM-stat-coor-grayscale-source-image.png]* | \| Position (X,Y) \| Position of paired pixel \| Values of paired pixels \|<br/>\| --- \| --- \| --- \|<br/>\| (0,0) \| (1,0) \| (3,0)[^LowValue] \|<br/>\| (1,0) \| (2,0) \| (0,1) \|<br/>\| (2,0) \| (3,0) \| (1,1) \|<br/>\| (3,0) \| (3,0)[^MirrorOverscan] \| (1,1) \|<br/>\| (0,1) \| (1,1) \| (0,4) \|<br/>\| (1,1) \| (2,1) \| (4,2)[^LowValue] \|<br/>\| (2,1) \| (3,1) \| (2,0)[^LowValue] \|<br/>\| (3,1) \| (3,1) \| (0,0) \|<br/>\| (0,2) \| (1,2) \| (2,3) \|<br/>\| (1,2) \| (2,2) \| (3,4) \|<br/>\| (2,2) \| (3,2) \| (4,1)[^LowValue] \|<br/>\| (3,2) \| (3,2)[^MirrorOverscan] \| (1,1) \|<br/>\| (0,3) \| (1,3) \| (0,3) \|<br/>\| (1,3) \| (2,3) \| (3,1)[^LowValue] \|<br/>\| (2,3) \| (3,3) \| (1,2) \|<br/>\| (3,3) \| (2,3)[^MirrorOverscan] \| (2,2) \| | *[Image: IM-stat-coor-co-occurrence-matrix.png]* |

[^LowValue]: In the case where the first digit is higher than the second in a pixel pair (for example, (3,0)), the frequency of this pixel pair is counted such that the first digit is always lower than the second (so, a pixel pair of (3,0) is recorded as (0,3) in the co-occurrence matrix).

[^MirrorOverscan]: In this case, mirror overscan is used for pixel pairs that fall outside of the source image, if using the entire image. The last pixel of each row and column is mirrored.

## Tiling your image

Using co-occurrence statistics to discern discrepancies works best on small areas. As such, Aurora Imaging Library allows you to calculate the co-occurrence matrix (and corresponding statistics) for the neighborhood of every _n_th pixel. Each of these neighborhoods would then have their own co-occurrence statistic results available for comparison.

Each neighborhood is considered a tile. Set the tile size using [`MimControl`](../../Reference/im/MimControl.md) with [`M_TILE_SIZE_X`](../../Reference/im/MimControl.md) and [`M_TILE_SIZE_Y`](../../Reference/im/MimControl.md). Set the distance between the tiles (step) using [`M_STEP_SIZE_X`](../../Reference/im/MimControl.md) and [`M_STEP_SIZE_Y`](../../Reference/im/MimControl.md). If the step size and tile size force tiles to overlap, they will. If some part of these tiles falls outside the image buffer, buffer overscan is used to make up the difference. It uses transparent overscan if the buffer is a child buffer and there are underlying parent buffer pixels; otherwise it uses mirror overscan. Aurora Imaging Library tries to equalize the number of columns or rows of overscan. For example, if 2 columns of overscan would be required, one will be placed before the first column, and one after the last column. This can result in the first pixel being an overscan pixel. If the image buffer has an overscan region, it is used. If the overscan region is too small to fit the overflow, Aurora Imaging Library will internally extend the image buffer's overscan region.

The 8 x 7 image below is divided into six 4x4 tiles that overlap in X by 1 column. To configure this example, the tile size X and Y are set to 4 ([`M_TILE_SIZE_X`](../../Reference/im/MimControl.md), [`M_TILE_SIZE_Y`](../../Reference/im/MimControl.md)). The step size is set to (3,4) ( [`M_STEP_SIZE_X`](../../Reference/im/MimControl.md), [`M_STEP_SIZE_Y`](../../Reference/im/MimControl.md)).

*[Image: IM-stats-cooccurence-tiling.png]*

## Supported co-occurrence statistics

Aurora Imaging Library supports the following six different co-occurrence matrix statistics. Note that, in the following calculations, _i_ represents the grayscale value of the first pixel, while _j_ represents the grayscale value of the paired pixel. _P<sub>ij</sub>_ represents the value of the normalized co-occurrence matrix of the given pixel pair. Mu_<sub>i</sub>_ (_µ<sub>i</sub>_) represents the weighted average index of the row (_i_) and Mu_<sub>j</sub>_ (_µ<sub>j</sub>_) represents the weighted average index of the column (_j_). Sigma_<sub>i</sub>_ (_ơ<sub>i</sub>_) represents the standard deviation of the row and Sigma_<sub>j</sub>_ (_ơ<sub>j</sub>_) represents the standard deviation of the column.

To enable a co-occurrence matrix statistic, use [`MimControl`](../../Reference/im/MimControl.md) with [`M_STAT_GLCM...`](../../Reference/im/MimControl.md) set to [`M_ENABLE`](../../Reference/im/MimControl.md). To perform the operation, call [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md).

- **Contrast**([`M_STAT_GLCM_CONTRAST`](../../Reference/im/MimControl.md)). A measure of the difference between paired pixel values, whereby the difference is squared so that the calculated value grows more rapidly the further paired pixel values are from the diagonal of the co-occurrence matrix (the more the values of paired pixels are different from each other).
  *[Image: MimStat_cooc_contrast_eq.png]*
- **Dissimilarity** ([`M_STAT_GLCM_DISSIMILARITY`](../../Reference/im/MimControl.md)). A measure very similar to contrast except that the distances are not squared so that the calculated value grows linearly the further pairs of pixel values are from the diagonal of the co-occurrence matrix.
  *[Image: MimStat_cooc_dissimilarity_eq.png]*
- **Pearson's coefficient correlation**([`M_STAT_GLCM_CORRELATION`](../../Reference/im/MimControl.md)). A measures of how close your data comes to forming a straight line in the co-occurrence matrix. The line's orientation (diagonal or not) is not specified but the result is dependent on the slope of the line.
  *[Image: MimStat_cooc_correlation_eq.png]*
- **Energy**([`M_STAT_GLCM_ENERGY`](../../Reference/im/MimControl.md)). A measure of the concentration of the same pixel pairs occurring in the target area. This value is high when the co-occurrence matrix has few entries of large magnitude.
  *[Image: MimStat_cooc_energy_eq.png]*
- **Entropy** ([`M_STAT_GLCM_ENTROPY`](../../Reference/im/MimControl.md)). A measure of the irregularity of the same pixel pairs occurring in the target area. The calculated entropy value is highest when the values of the co-occurrence matrix are quite uniformly distributed through the matrix. This occurs when the area has no pairs of pixels that occur more often than others.
  *[Image: MimStat_cooc_entropy_eq.png]*
- **Homogeneity** ([`M_STAT_GLCM_HOMOGENEITY`](../../Reference/im/MimControl.md)). A measure of how similar are the values of paired pixels (how close to the diagonal are the values of paired pixels to the diagonal of the co-occurrence matrix). Homogeneity is the inverse of contrast. Homogeneity is a value between 0 and 1.
  *[Image: MimStat_cooc_homogeneity_eq.png]*

## A co-occurrence example

The following example calculates several co-occurrence statistics for two images of similar pieces of textile. The results are returned and drawn for comparison.

The two different images produce slightly different co-occurrence results that are visible, when drawn:

| Description | Fabric without defects | Fabric with defects |
| --- | --- | --- |
| Source images | *[Image: imstatcooc-finenodefectcropped.png]* | *[Image: imstatcooctissuefinedefectcropped.png]* |
| Drawn version of the co-occurrence matrix | *[Image: imstatcooc-matrixfinenodefectcropped.png]* | *[Image: imstatcooc-matrixfinewdefectcropped.png]* |
| Statistical results | Energy: 0.025895 Contrast: 251.493018 Entropy: 7.865064 Dissimilarity: 11.860745 Homogeneity: 0.092092 Correlation: 0.938123 | Energy: 0.024421 Contrast: 305.347826 Entropy: 7.996176 Dissimilarity: 13.070599 Homogeneity: 0.084361 Correlation: 0.944868 |

## Limiting the bit depth of your image

The co-occurrence statistic operations require that your source image is at most 10 bits deep. When performing a co-occurrence statistics operation using a 32-bit buffer, Aurora Imaging Library rescales the pixel values of the source image to fit within an unsigned integer buffer. That resulting buffer is then bit-shifted to fit within the bit-depth limitation, as set by [`M_GLCM_QUANTIFICATION`](../../Reference/im/MimControl.md).

In the case where the source image was grabbed into a buffer with a greater bit-depth (such as, grabbing a 10-bit image and storing it in a 16-bit buffer), use [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MAX`](../../Reference/buf/MbufControl.md) and [`M_MIN`](../../Reference/buf/MbufControl.md)to limit the maximum and minimum pixel values of the buffer. For example, a 10-bit image, stored in a 16-bit buffer, will be a 10-bit image with the additional bits being zero or sign-extended. Due to the positioning of the zero or sign-extended bits within the 16-bit image (specifically the 6 MSB are set to zero, for an unsigned buffer, or -1, for a signed buffer), your co-occurrence matrix would end up using these zero or the sign-extended bits in its calculations. To avoid this situation, set [`M_MAX`](../../Reference/buf/MbufControl.md) to 1023 (which is 2<sup>10</sup>-1) and set [`M_MIN`](../../Reference/buf/MbufControl.md) to 0. This results in the zero or sign-extended bits being ignored, and the preceding MSB being used to build the co-occurrence matrix.
