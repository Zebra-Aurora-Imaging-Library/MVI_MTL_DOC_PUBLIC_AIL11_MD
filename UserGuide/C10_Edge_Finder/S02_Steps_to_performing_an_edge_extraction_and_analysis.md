---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Steps_to_performing_an_edge_extraction_and_analysis
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Steps to performing an edge extraction and analysis"
---

# Steps to extract and analyze edges

The following steps provide a basic methodology for using the Aurora Imaging Library Edge Finder module:

1. Allocate an Edge Finder context, using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md).
2. If necessary, adjust general processing controls to fit your application, using successive calls to [`MedgeControl`](../../Reference/edge/MedgeControl.md).
3. If necessary, select the required features to calculate, using successive calls to [`MedgeControl`](../../Reference/edge/MedgeControl.md).
4. Allocate an Edge Finder result buffer to hold the results of the edge extraction, using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).
5. Calculate edges and edge features, using [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).
6. If necessary, exclude or delete edges that do not meet a required criterion, using [`MedgeSelect`](../../Reference/edge/MedgeSelect.md). Excluded edges will be ignored in future calculations, while deleted edges will be removed from the Edge Finder result buffer.
7. If necessary, select new features and perform a post-calculation, using [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).
8. Get the number of edges currently included and retrieve the required results from the Edge Finder result buffer, using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md). Results for the excluded or deleted edges will not be returned.
9. If necessary, get the coordinates of edgels from an Edge Finder result buffer that correspond to the closest neighbors from a list of user-specified source point coordinates, using [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md).
10. If necessary, copy data from user-supplied arrays to a specified Edge Finder result buffer, using [`MedgePut`](../../Reference/edge/MedgePut.md).
11. If necessary, draw edges and edge features, using [`MedgeDraw`](../../Reference/edge/MedgeDraw.md).
12. If necessary, save your Edge Finder context, using [`MedgeSave`](../../Reference/edge/MedgeSave.md) or [`MedgeStream`](../../Reference/edge/MedgeStream.md).
13. Free all your allocated objects, using [`MedgeFree`](../../Reference/edge/MedgeFree.md), unless [`M_UNIQUE_ID`](../../Reference/edge/MedgeAlloc.md) was specified during allocation.

You can repeat steps 6, 7, and 8 until all the required results are obtained. Note that successively excluding or deleting unwanted edges and then calculating more features is the recommended procedure to achieve the required results, especially if you have a large number of unwanted edges.
