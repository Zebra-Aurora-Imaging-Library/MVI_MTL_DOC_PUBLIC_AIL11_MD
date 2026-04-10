---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: 3D_Registration_overview
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / 3D Registration overview"
---

# Aurora Imaging Library 3D Registration module

The Aurora Imaging Library 3D Registration module allows you to calculate the optimal pairwise registration of two or more point clouds, and defines the transformation matrices required to align the working coordinate systems of the point clouds. This is useful in many applications where you have multiple point clouds, with overlapping representations of the same 3D object, and you want the same points on the 3D object to be represented with the same coordinate values in every point cloud. This is important when you want to detect defects in a scanned object, determine an object's pose, or merge multiple point clouds into one single point cloud that represents a 3D object.

This module registers each point cloud to a specified other point cloud (reference point cloud) in the list to register. Once the module establishes the transformation that registers each point cloud with its reference point cloud, you can also retrieve the transformation that registers it to any other in the list.

The module uses the Iterative Closest Point (ICP) algorithm to measure the root-mean-square (RMS) error between points it automatically pairs with those in the reference point cloud (closest points are paired first); it iteratively tries to transform the point cloud so that this measurement decreases. In each iteration, the points are paired and transformed again.

The following animation shows how the algorithm pairs points and transforms the point cloud repeatedly, over several iterations:

*[Image: 3dreg_icp_iterations]*

The registration operation continually iterates until a stop condition is met. The registration operation is considered successful (or not) depending on which stop condition is met. The stop conditions are set to default values that should suit the majority of applications, but they rely on an effective preregistration. Preregistering a point cloud requires specifying either to register the centroids of the two point clouds, or to use a user-defined transformation matrix that moves a point cloud to a rough location that is close enough to the other point cloud for the algorithm to start.

You can copy results to perform diagnostic analysis. For example, you can copy a subsampled reference or target point cloud into a container for later analysis. Subsampling is a recommended way to speed up the pairwise registration process. Since subsampling can differ for each point cloud, copying lets you examine a particular point cloud that has undergone subsampling. You can also copy results to create a distance image, overlap mask, or pair index image, which allow you to examine the results of specific iterations (including the last iteration) of the registration process. The pixels in the resulting image show specific point pair relationships. A distance image gives numerical distances for paired points, while an overlap mask image depicts pairings using a binary representation. In a pair index image, each pixel's position and value reflect the respective indices of a paired reference and target point.
