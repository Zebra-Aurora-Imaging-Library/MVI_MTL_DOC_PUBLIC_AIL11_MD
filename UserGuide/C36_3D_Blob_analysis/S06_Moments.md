---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Moments
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Moments"
---

# Moments

Using the 3D Blob Analysis module, you can calculate moment-based features, such as a blob's centroid (first-order moment), and its linearity or planarity (second-order moments). To enable the calculation of a specified order, use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md), and then set the order with [`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md) (the default is 2). Note that Aurora Imaging Library calculates all moments up to and including those of the specified order. Retrieve results using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md).

First-order moment calculations reflect the center of mass of a blob (its centroid), while second-order moment calculations reflect the distribution of points around an axis. Accordingly, second-order features include principal component analysis (PCA) results and related eigenvalues, as well as the standard deviation of points, the covariance matrix, and a blob's linearity and planarity.

*[Image: 3dim_PrincipalAxes.png]*

If you only require a specific type of moment-based feature result, you can use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) to enable that calculation directly. For example, you can enable PCA feature calculations ([`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_PCA`](../../Reference/3dblob/M3dblobControl.md)), and then retrieve only those results (using [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) with [`M_PRINCIPAL_AXIS_...`](../../Reference/3dblob/M3dblobGetResult.md)). Otherwise, enabling [`M_MOMENTS`](../../Reference/3dblob/M3dblobControl.md) with an order of at least 2 enables the calculation and eventual retrieval of all second-order moments, as well as first-order moments (the centroid).

You can also use [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md) to retrieve moment-based feature results with respect to the origin of the working coordinate system (ordinary moments), or with respect to the blob's centroid (central moments); use the [`M_MOMENT_XYZ()`](../../Reference/3dblob/M3dblobGetResult.md) and [`M_MOMENT_CENTRAL_XYZ()`](../../Reference/3dblob/M3dblobGetResult.md) result types, respectively. When setting the moment order ([`M_MOMENT_ORDER`](../../Reference/3dblob/M3dblobControl.md)) before calculating these results, note that the specified order must be a value that is greater than or equal to the sum of the powers in X, Y, and Z, according to the ordinary or central moment formula (see [`M3dblobGetResult`](../../Reference/3dblob/M3dblobGetResult.md)). For example, to retrieve only third, second, and first-order moment results, PowerX + PowerY + PowerZ must not exceed 3.
