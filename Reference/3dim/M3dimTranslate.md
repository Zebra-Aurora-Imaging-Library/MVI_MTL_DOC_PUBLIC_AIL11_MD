---
doctype: Reference
module: 3dim
function: M3dimTranslate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimTranslate"
---

# M3dimTranslate

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

> Translate a specified point cloud or 3D geometry.

## Syntax

```c
void M3dimTranslate(
    AIL_ID     SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID     DstContainerBufOrGeometry3dgeoId,  //out
    AIL_DOUBLE TranslationX,                      //in
    AIL_DOUBLE TranslationY,                      //in
    AIL_DOUBLE TranslationZ,                      //in
    AIL_INT64  ControlFlag                        //in
)
```

## Description

This function applies the specified translation to the points in the source point cloud container or to the geometry in the source 3D geometry object. You can specify a translation (displacement) along the X, Y, and/or Z-axis of the working coordinate system, in world units.

If the source is a point cloud container, [`M3dimTranslate`](../../Reference/3dim/M3dimTranslate.md) applies the specified translation to the coordinates in the container's [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component, and the modified component is added to the destination container. The source container's [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md) component is copied to the destination container. If [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md), [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md), and [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) components exist in the source container, they are also copied to the destination container. Any previously existing reflectance, normals, and mesh components are removed from the destination container.

> **Note:** Note that, when translating a point cloud, this function affects the coordinates of the points and not their storage location in the container.

## Parameters

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies a source point cloud container or 3D geometry object.

*For specifying the source container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `M_XY_PLANE` | Specifies the XY (Z=0) plane. |
| `Source 3D geometry object identifier` | Specifies the identifier of the source 3D geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. |
| `Source container identifier` | Specifies the identifier of the source container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `DstContainerBufOrGeometry3dgeoId` *(out, AIL_ID)*

Specifies the destination container or 3D geometry object.

*For specifying the destination container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Destination 3D geometry object identifier` | Specifies the identifier of the destination 3D geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `Destination container identifier` | Specifies the identifier of the destination container, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). The destination container must not be a child container. |

### `TranslationX` *(in, AIL_DOUBLE)*

Specifies the displacement in the X-direction.

### `TranslationY` *(in, AIL_DOUBLE)*

Specifies the displacement in the Y-direction.

### `TranslationZ` *(in, AIL_DOUBLE)*

Specifies the displacement in the Z-direction.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
