---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Basic_concepts
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library 3D Blob Analysis module

The basic concepts and vocabulary conventions for the Aurora Imaging Library 3D Blob Analysis module are:

- **Bounding box.**The smallest axis-aligned box that contains all the points in a blob.
- **Centroid.**The center of gravity of a blob.
- **Eigenvalue**The amount of variance as measured along a principal component. The largest eigenvalue corresponds to the variance of the first principal component.
- **Feret.**The diameter of a blob at a given angle or position.
- **Moment.**The distribution of matter (in this case, 3D points) about a point or axis.
- **PCA.**Principal component analysis. The principal components of a dataset (in this case, 3D points distributed in a blob) are the directions along which there is the most variance, or spread, in the data. The first principal component (or principal axis) represents the maximum variance; the second principal component represents the second-most variance, perpendicular to the first; and the third principal component is perpendicular to the first two components.
- **PCA-aligned bounding box.**The smallest box containing all the points in a blob that is also aligned with the blob's three principal components (principal axes).
- **Point cloud.**A set of 3D points representing objects in a scene.
- **Semi-oriented bounding box.**The smallest box containing all the points in a blob that is axis-aligned in Z but not in X and Y.
- **Working coordinate system.**The implicit real world coordinate system used to express the coordinates of 3D points in a point cloud. Each point cloud container has its own working coordinate system.
