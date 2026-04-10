---
doctype: Reference
module: bead
function: MbeadGetNeighbors
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / bead / MbeadGetNeighbors"
---

# MbeadGetNeighbors

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

> Get the template and/or vertex closest to a source point.

## Syntax

```c
void MbeadGetNeighbors(
    AIL_ID     ContextBeadId,     //in
    AIL_INT    LabelOrIndex,      //in
    AIL_DOUBLE PositionX,         //in
    AIL_DOUBLE PositionY,         //in
    AIL_INT *  TemplateLabelPtr,  //out
    AIL_INT *  PointIndexPtr,     //out
    AIL_INT64  ControlFlag        //in
)
```

## Description

This function retrieves the label of the template, and the vertex within that template, that is closest to the specified position. This function can also retrieve the vertex within the specified template that is closest to the specified position. You can only use this function with templates whose path follows a polyline ([`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_PATH`](../../Reference/bead/MbeadControl.md) set to either [`M_POLYLINE_SEED`](../../Reference/bead/MbeadControl.md) or [`M_POLYLINE`](../../Reference/bead/MbeadControl.md)).

To set a maximum distance for which Aurora Imaging Library considers a template or vertex to be the closest, use [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_CLOSEST_POINT_MAX_DISTANCE`](../../Reference/bead/MbeadControl.md). By default, there is no maximum distance.

## Parameters

### `ContextBeadId` *(in, AIL_ID)*

Specifies the identifier of the bead context. The bead context must have been previously allocated on the required system using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the bead template in which the closest vertex is expected to be located.

*For specifying a bead template*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies no label or index value.

When using [`M_CLOSEST_TEMPLATE`](../../Reference/bead/MbeadGetNeighbors.md), [`LabelOrIndex`](../../Reference/bead/MbeadGetNeighbors.md) must be set to [`M_NULL`](../../Reference/bead/MbeadGetNeighbors.md). |
| `M_TEMPLATE_INDEX` | Specifies the index of the template. |
| `M_TEMPLATE_LABEL` | Specifies the label of the template. |

### `PositionX` *(in, AIL_DOUBLE)*

Specifies the source point's X-position.

### `PositionY` *(in, AIL_DOUBLE)*

Specifies the source point's Y-position.

### `TemplateLabelPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the label of the closest template.

### `PointIndexPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the index of the closest vertex.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to retrieve the closest template and/or the closest vertex.

*For specifying whether to receive the closest template or the closest vertex*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CLOSEST_TEMPLATE`](../../Reference/bead/MbeadGetNeighbors.md). |
| `M_CLOSEST_POINT` | Specifies to return the vertex, within the specified template, that is closest to the specified source point. |
| `M_CLOSEST_TEMPLATE` | Specifies to return the template, and the vertex within that template, that are closest to the specified source point. |
