---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Reference_pointcloud
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Reference pointcloud"
---

# Specifying the reference point cloud

The pairwise 3D registration context should contain one registration element for each point cloud that you want to register, and each registration element should contain information on how to preregister it, and which other point cloud is its reference. Once all the preregistration and registration settings are specified after multiple calls to [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md) and [`M3dregControl`](../../Reference/3dreg/M3dregControl.md), only one call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md) is required.

There are some limits on how you can define the topology of, or the relationship between, the point clouds in the registration context. One point cloud must use the global coordinate system as its reference (set using [`M3dregSetLocation`](../../Reference/3dreg/M3dregSetLocation.md) with [`M_REGISTRATION_GLOBAL`](../../Reference/3dreg/M3dregSetLocation.md)). Additionally, the references for points clouds cannot be circularly defined. If you take a specific point cloud and look at its reference, and then at its reference's reference, and continue through the series of point clouds linked in this manner, the first point cloud must never appear as another's reference. In the following image, the topology has been incorrectly specified because it forms a loop:

*[Image: 3dreg_PointCloudTopology_Incorrect.png]*

After a successful call to [`M3dregCalculate`](../../Reference/3dreg/M3dregCalculate.md), you can retrieve the transformation matrix that specifies the alignment for any point cloud in the topology to any other point cloud in the topology.

Note, however, only the point clouds originally paired for registration in the context will have a fully optimized transformation. Small errors in the transformation can be propagated down long chains in the topology, affecting other registrations. Furthermore, if these point clouds were merged, the merged point cloud will contain all of the propagated errors as well. In the following image, the topology has been correctly specified, but errors in the registration will be propagated down the long chain:

*[Image: 3dreg_PointCloudTopology_Inefficient.png]*

If the chain is kept short, the propagation error will be minimized, increasing the overall accuracy of any retrieved transformations or merged point clouds. In the following image, the topology has been correctly specified, and propagation errors are minimized because there are no long chains:

*[Image: 3dreg_PointCloudTopology_Efficient.png]*
