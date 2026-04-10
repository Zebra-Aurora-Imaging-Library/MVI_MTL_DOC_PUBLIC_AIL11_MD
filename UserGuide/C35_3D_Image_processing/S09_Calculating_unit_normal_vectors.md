---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: Calculating_unit_normal_vectors
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / Calculating unit normal vectors"
---

# Calculating unit normal vectors

You can calculate each point's unit normal vector, which is a vector perpendicular to a surface estimated from the positions of surrounding points. Normal vectors are useful, for example, when reconstructing a surface for the point cloud using [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md) (for certain surface reconstruction modes only), or for registration operations using the 3D Registration module. To calculate the normal vectors, use [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md). This function stores the unit normal vector of each point in the normals component of the operation's destination container, creating or reallocating the component if necessary. Note that, to increase calculation speed, [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md) can use either existing organizational information in the point cloud or an existing mesh component.

Once you have calculated the normals of a point cloud, you can draw the normal vectors as lines in a 3D display. To do so, use [`M3dimGet`](../../Reference/3dim/M3dimGet.md) to retrieve the coordinates of points from the point cloud's range component; then, call [`M3dimGet`](../../Reference/3dim/M3dimGet.md) again to retrieve the X-, Y-, and Z-components of the normal vectors from the normals component. Finally, pass the coordinates and vector components to [`M3dgraLines`](../../Reference/3dgra/M3dgraLines.md) while also specifying [`M_POINT_AND_VECTOR`](../../Reference/3dgra/M3dgraLines.md).

*[Image: 3dim_normal_vectors.png]*
