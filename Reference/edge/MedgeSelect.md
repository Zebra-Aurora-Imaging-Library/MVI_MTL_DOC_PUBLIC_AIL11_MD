---
doctype: Reference
module: edge
function: MedgeSelect
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeSelect"
---

# MedgeSelect

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Select edges for calculations and result retrieval.

## Syntax

```c
void MedgeSelect(
    AIL_ID     EdgeResultId,        //out
    AIL_INT64  Operation,           //in
    AIL_INT64  SelectionCriterion,  //in
    AIL_INT64  Condition,           //in
    AIL_DOUBLE Param1,              //in
    AIL_DOUBLE Param2               //in
)
```

## Description

This function selects edges that meet a specified criterion. These edges will be included in or excluded from future operations (calculations and result retrieval), or deleted entirely from the Edge Finder result buffer. To call this function, [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have been called at least once.

If this function is not called at least once, all edges are included by default. If there is more than one call to this function, the effect of the calls is cumulative, unless [`M_INCLUDE_ONLY`](../../Reference/edge/MedgeSelect.md) or [`M_EXCLUDE_ONLY`](../../Reference/edge/MedgeSelect.md) is specified as the operation to perform.

Once an edge has been excluded, it can typically be re-included by specifying [`M_INCLUDE`](../../Reference/edge/MedgeSelect.md) or [`M_INCLUDE_ONLY`](../../Reference/edge/MedgeSelect.md) in a future call to this function (with the correct criterion). However, if you use the result buffer with different images (in a call to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md)), all results in the result buffer are discarded and all new edges are re-included.

> **Note:** Unlike most other Aurora Imaging Library functions, if the edges were extracted from a calibrated source image, you must specify relevant values in world units, although you will have control over the units when retrieving results using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md).

You can use this function to select edges based on one of the following:

- A calculated edge feature (see [`MedgeControl`](../../Reference/edge/MedgeControl.md)), where the edge selection depends on whether the specified edge feature meets the specified condition.
- The inter-relationship of edges, where the edge selection depends on whether edges meet the specified box or chain condition of the specified edge or group of edges.
- The current status of edges, where the edge selection depends on the status of a specific edge, all edges, all included edges, or all excluded edges. These will be included, included only, excluded, excluded only, or deleted. For example, you can include only ([`M_INCLUDE_ONLY`](../../Reference/edge/MedgeSelect.md)) the excluded edges ([`M_EXCLUDED_EDGES`](../../Reference/edge/MedgeSelect.md)), which will essentially swap the previously included and excluded edges.
- The proximity of an edge or edges to a specified point, where the edge selection depends on the specified radius, and the nearest neighbor condition.

You can also use this function to crop and select a portion of a specified edge.

For more information on the different uses of this function, see[Calculating and retrieving results](../../UserGuide/C10_Edge_Finder/S07_Calculating_and_retrieving_results.md) and [Advanced edge extraction](../../UserGuide/C10_Edge_Finder/S08_Advanced_edge_extraction.md).

## Parameters

### `EdgeResultId` *(out, AIL_ID)*

Specifies the identifier of the Edge Finder result buffer to be used in the edge selection process.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform on the specified edges. Set this parameter to one of the following values.

*For specifying the operation*

| Value | Description |
| --- | --- |
| `M_DELETE` | Deletes edges that meet the specified condition. [`M_DELETE`](../../Reference/edge/MedgeSelect.md) affects only included edges, unless otherwise stated by the [`SelectionCriterion`](../../Reference/edge/MedgeSelect.md) parameter.

[`M_DELETE`](../../Reference/edge/MedgeSelect.md) removes edges permanently from the Edge Finder result buffer and, consequently, prevents these edges from being re-included. |
| `M_EXCLUDE` | Excludes all edges that meet the specified condition.

[`M_EXCLUDE`](../../Reference/edge/MedgeSelect.md) affects only the status of currently included edges. |
| `M_EXCLUDE_ONLY` | Excludes only those edges that meet the specified condition and includes all others.

The exclusion does not consider the present status of edges (whether they are excluded), except for edges that have been deleted ([`M_DELETE`](../../Reference/edge/MedgeSelect.md)), unless otherwise stated by the [`SelectionCriterion`](../../Reference/edge/MedgeSelect.md) parameter. |
| `M_INCLUDE` | Includes all edges that meet the specified condition.

[`M_INCLUDE`](../../Reference/edge/MedgeSelect.md) affects only the status of currently excluded edges. |
| `M_INCLUDE_ONLY` | Includes only those edges that meet the specified condition and excludes all others.

The inclusion does not consider the present status of edges (whether they are included), except for edges that have been deleted ([`M_DELETE`](../../Reference/edge/MedgeSelect.md)), unless otherwise stated by the [`SelectionCriterion`](../../Reference/edge/MedgeSelect.md) parameter. |

### `SelectionCriterion` *(in, AIL_INT64)*

Specifies on what the selection criterion will be based. Note that for an edge selection based on the proximity of the edges to a point, this parameter must be set to `M_NULL`.

### `Condition` *(in, AIL_INT64)*

Specifies the condition for the edge selection. Note that for an edge selection based on the current status of edges, this parameter must be set to `M_NULL`.

### `Param1` *(in, AIL_DOUBLE)*

Specifies a value that is dependent on the feature and condition chosen. If the edges have been calculated using a calibrated source image, you must specify the relevant values in the real world. If this parameter is unused, set it to `M_NULL`.

### `Param2` *(in, AIL_DOUBLE)*

Specifies a value that is dependent on the feature and condition chosen. If the edges have been calculated using a calibrated source image, you must specify the relevant values in the real world. If this parameter is unused, set it to `M_NULL`.

## Parameter Associations

### For specifying the edge selection method

---

### `Edge Finder result buffer identifier for a chain cropping based edge selection`

Specifies an Edge Finder result buffer on which to perform a chain cropping edge selection. This type of selection essentially splits off a portion of a single specified edge, assigns the portion a new edge label, and selects the portion for inclusion, exclusion, or deletion from future operations on the result buffer. The other portion keeps its current edge label value and status. The two portions are considered two touching edges after the split. Therefore, all their calculated edge features are updated after the split.  The specified edgel index value is the dividing point.

| Value | Description |
| --- | --- |
| `M_CROP_CHAIN` | Specifies a chain cropping edge selection. |
| `M_GREATER` | Specifies that the edgels with an index value greater than the specified edgel index value are considered part of the new edge. |
| `M_GREATER_OR_EQUAL` | Specifies that the edgels with an index value greater than or equal to the specified edgel index value are considered part of the new edge. |
| `M_LESS` | Specifies that the edgels with an index value less than the specified edgel index value are considered part of the new edge. |
| `M_LESS_OR_EQUAL` | Specifies that the edgels with an index value less than or equal to the specified edgel index value are considered part of the new edge. |

---

### `Edge Finder result buffer identifier for a feature based edge selection`

Specifies an Edge Finder result buffer on which to perform an edge selection based on a calculated edge feature. Edges whose specified feature satisfies the condition are included, excluded, or deleted from future operations on the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average strength will not be calculated. |
| `M_ENABLE` | Specifies that the average strength will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme right edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme right edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme left edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme left edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme bottom edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme bottom edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme top edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme top edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the X-position of the center of gravity will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position of the center of gravity will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the circle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the circle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the radius will not be calculated. |
| `M_ENABLE` | Specifies that the radius will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge's closure state will not be calculated. |
| `M_ENABLE` | Specifies that the edge's closure state will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the convex elongation will not be calculated. |
| `M_ENABLE` | Specifies that the convex elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle will not be calculated. |
| `M_ENABLE` | Specifies that the angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the ellipse will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the ellipse will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the major axis will not be calculated. |
| `M_ENABLE` | Specifies that the major axis will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minor axis will not be calculated. |
| `M_ENABLE` | Specifies that the minor axis will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that a fast length will not be calculated. |
| `M_ENABLE` | Specifies that a fast length will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret elongation will not be calculated. |
| `M_ENABLE` | Specifies that the Feret elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the general Feret will not be calculated. |
| `M_ENABLE` | Specifies that the general Feret will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the average Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the X-Feret value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the Y-Feret value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the label value will not be calculated. |
| `M_ENABLE` *(default)* | Specifies that the label value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the length of each edge will not be calculated. |
| `M_ENABLE` | Specifies that the length of each edge will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _A_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _A_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _B_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _B_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _C_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _C_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error will not be calculated. |
| `M_ENABLE` | Specifies that the fit error will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the moment elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle of the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the angle of the moment elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position will not be calculated. |
| `M_ENABLE` | Specifies that the X-position will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the number of edgels will not be calculated. |
| `M_ENABLE` | Specifies that the number of edgels will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the strength will not be calculated. |
| `M_ENABLE` | Specifies that the strength will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the tortuosity measure will not be calculated. |
| `M_ENABLE` | Specifies that the tortuosity measure will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-maximum at Y-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the X-maximum at Y-maximum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-minimum at Y-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the X-minimum at Y-minimum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-maximum at X-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-maximum at X-minimum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-minimum at X-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-minimum at X-maximum will be calculated. |
| `M_EQUAL` | Specifies that edges with values for the specified feature equal to [`Param1`](../../Reference/edge/MedgeSelect.md) are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_GREATER` | Specifies that edges with values for the specified feature greater than [`Param1`](../../Reference/edge/MedgeSelect.md) are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_GREATER_OR_EQUAL` | Specifies that edges with values for the specified feature greater than or equal to [`Param1`](../../Reference/edge/MedgeSelect.md) are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_IN_RANGE` | Specifies that edges with values for the specified feature in the range of [`Param1`](../../Reference/edge/MedgeSelect.md) to [`Param2`](../../Reference/edge/MedgeSelect.md), inclusive, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_LESS` | Specifies that edges with values for the specified feature less than[`Param1`](../../Reference/edge/MedgeSelect.md) are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_LESS_OR_EQUAL` | Specifies that edges with values for the specified feature less than or equal to [`Param1`](../../Reference/edge/MedgeSelect.md)*are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_NOT_EQUAL` | Specifies that edges with values for the specified feature not equal to [`Param1`](../../Reference/edge/MedgeSelect.md) are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_OUT_RANGE` | Specifies that edges with values for the specified feature less than [`Param1`](../../Reference/edge/MedgeSelect.md), or greater than [`Param2`](../../Reference/edge/MedgeSelect.md), are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_NULL` | Specifies that this parameter is unused. |
| `Value` | Specifies the high condition limit. |

---

### `Edge Finder result buffer identifier for an inter-relationship based edge selection`

Specifies an Edge Finder result buffer on which to perform an edge selection based on the inter-relationship of edges. Edges that fall inside or outside of a boundary defined by other edges are included, excluded, or deleted from future operations on the Edge Finder result buffer. The specified condition establishes whether edges inside or outside are selected. You specify the delimiting edges; to define the boundary, you can specify to use the edges as is or to use their bounding box as a delimiter.

| Value | Description |
| --- | --- |
| `M_ALL_EDGES` | Specifies all edges regardless of their present status, except for edges that have been deleted. |
| `M_EXCLUDED_EDGES` | Specifies all currently excluded edges, except for edges that have been deleted. |
| `M_INCLUDED_EDGES` | Specifies all currently included edges. |
| `M_SPECIFIC_EDGE` | Specifies a specific edge. Use [`Param1`](../../Reference/edge/MedgeSelect.md) to set the label value of the edge. |
| `M_INSIDE_BOX` | Specifies that edges inside the bounding box of the specified edge, or inside any individual bounding box of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_INSIDE_CHAIN` | Specifies that edges inside the chain of the specified edge, or inside any individual chain of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_INSIDE_OR_EQUAL_BOX` | Specifies that edges inside or equal to the bounding box of the specified edge, or inside or equal to any individual bounding box of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_INSIDE_OR_EQUAL_CHAIN` | Specifies that edges inside or equal to the chain of the specified edge, or inside or equal to any individual chain of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_OUTSIDE_BOX` | Specifies that edges outside the bounding box of the specified edge, or outside any individual bounding box of the specified group of egdes, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_OUTSIDE_CHAIN` | Specifies that edges outside the chain of the specified edge, or outside any individual chain of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_OUTSIDE_OR_EQUAL_BOX` | Specifies that edges outside or equal to the bounding box of the specified edge, or outside or equal to any individual bounding box of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_OUTSIDE_OR_EQUAL_CHAIN` | Specifies that edges outside or equal to the chain of the specified edge, or outside or equal to any individual chain of the specified group of edges, are included, excluded, or deleted from future operations on the specified Edge Finder result buffer. |
| `M_NULL` | Specifies that this parameter is unused (when using a group of edges to delimit the boundary). |
| `Value` | Specifies the label of the edge. |

---

### `Edge Finder result buffer identifier for a proximity based edge selection`

Specifies an Edge Finder result buffer on which to perform an edge selection based on the proximity of an edge, or edges, to a specified point.

| Value | Description |
| --- | --- |
| `M_ALL_NEAREST_NEIGHBORS` | Specifies that all edges within the specified radius from the stated point are included, excluded, or deleted from future operations on the specified Edge Finder result buffer.  Define the search radius using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_NEAREST_NEIGHBOR_RADIUS`](../../Reference/edge/MedgeControl.md). |
| `M_NEAREST_NEIGHBOR` | Specifies that the closest edge from the specified point is included, excluded, or deleted from future operations on the specified Edge Finder result buffer.  You can use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_NEAREST_NEIGHBOR_RADIUS`](../../Reference/edge/MedgeControl.md)to specify a circular area (around the specified point) in which to find the closest edge. If an edge is totally outside the search radius, it will not be considered as the closest edge. If some part of the edge chain lies within the search radius, it will be considered as a neighbor. |

---

### `Edge Finder result buffer identifier for a status based edge selection`

Specifies an Edge Finder result buffer on which to perform an edge selection based on the current status (included or excluded) of edges.

| Value | Description |
| --- | --- |
| `M_ALL_EDGES` | Specifies to select all edges regardless of their present status, except for edges that have been deleted. |
| `M_EXCLUDED_EDGES` | Specifies to select all currently excluded edges, except for edges that have been deleted. |
| `M_INCLUDED_EDGES` | Specifies to select all currently included edges. |

Note that no edges are selected from a delimiting edge that is not closed. For more information, see [`M_CLOSURE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). Also, if an edge is not completely enclosed by a closed edge, it is considered as an outside or equal edge.
