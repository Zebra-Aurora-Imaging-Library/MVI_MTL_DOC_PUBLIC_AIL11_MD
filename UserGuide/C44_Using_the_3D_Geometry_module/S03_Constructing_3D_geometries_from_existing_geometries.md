---
doctype: UserGuide
part: "3D related information"
chapter: Using_the_3D_Geometry_module
section: Constructing_3D_geometries_from_existing_geometries
module_tag: 3dgeo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Using_the_3D_Geometry_module / Constructing 3D geometries from existing geometries"
---

# Constructing 3D geometries from existing geometries

You can use existing geometries as a starting point for building other 3D geometries. To do so, use [`M3dgeoConstruct`](../../Reference/3dgeo/M3dgeoConstruct.md). Whether you have defined a source 3D geometry using a dedicated function, or obtained the geometry from other processing and analysis operations (for example, fitting a 3D geometry to a point cloud), [`M3dgeoConstruct`](../../Reference/3dgeo/M3dgeoConstruct.md) offers multiple operations for constructing new 3D geometries. For instance, you can construct:

- A point at the center of another geometry.
- A plane that passes through the face of a box or cylinder.
- A cylinder in which its central axis coincides with an existing line.

Two points can form the basis for several new 3D geometries: a box from opposite corner points, a cylinder or line from start and end points, or a sphere from antipodal points.

For an example scenario of using [`M3dgeoConstruct`](../../Reference/3dgeo/M3dgeoConstruct.md), consider the following workflow, in which lines are constructed to join calculated points on an object:

1. Obtain a point cloud (for example, grabbed from a 3D sensor).
2. Fit 3D planes to the surfaces of an object in the point cloud ([`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md)).
3. Find intersection points of the planes ([`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md)).
4. Construct lines between the calculated points ([`M3dgeoConstruct`](../../Reference/3dgeo/M3dgeoConstruct.md)).

You can construct 3D geometries that are useful markers for further processing and analysis. For example, you can construct a unit line that points in the same direction as a plane's normal vector. Conversely, you can construct a plane whose normal vector is aligned with a source line.

You can use [`M3dgeoConstruct`](../../Reference/3dgeo/M3dgeoConstruct.md) with [`M_FLIP`](../../Reference/3dgeo/M3dgeoConstruct.md) to interchange the start and end points of a line or cylinder. An [`M_FLIP`](../../Reference/3dgeo/M3dgeoConstruct.md) operation can also invert a plane so that its normal vector points in the opposite direction.

## Constructing a box

The following table lists the operation and related parameter settings required to construct an [`M_BOX`](../../Reference/3dgeo/M3dgeoConstruct.md) geometry.

| Operation | Src1 | Src2 | Param | Result | Example diagram |
| --- | --- | --- | --- | --- | --- |
| [`M_BOTH_CORNERS`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Point | - Point | — | 1 box | *[Image: 3dgeo_ConstructBox_2points.png]* |

## Constructing a cylinder

The following table lists the possible operations and related parameter settings required to construct an [`M_CYLINDER`](../../Reference/3dgeo/M3dgeoConstruct.md) geometry.

| Operation | Src1 | Src2 | Param | Result | Example diagram |
| --- | --- | --- | --- | --- | --- |
| [`M_AXIS`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Line | — | Radius >= 0 | 1 cylinder | *[Image: 3dgeo_ConstructCylinder_Axis.png]* |
| [`M_FLIP`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Cylinder | — | — | 1 cylinder | *[Image: 3dgeo_ConstructCylinder_Flip.png]* |
| [`M_TWO_POINTS`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Point | - Point | Radius >= 0 | 1 cylinder | *[Image: 3dgeo_ConstructCylinder_TwoPoints.png]* |

## Constructing a line

The following table lists the possible operations and related parameter settings required to construct an [`M_LINE`](../../Reference/3dgeo/M3dgeoConstruct.md) geometry.

| Operation | Src1 | Src2 | Param | Result | Example diagram |
| --- | --- | --- | --- | --- | --- |
| [`M_AXIS`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Cylinder | — | Radius >= 0 | 1 line | *[Image: 3dgeo_ConstructLine_Cylinder.png]* |
| [`M_EDGE`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Box | — | 0 &lt;= Value &lt;= 11 (to specify the edge) *[Image: TableWidener.png]* | 1 line | *[Image: 3dgeo_ConstructLine_Edge.png]* |
| [`M_FLIP`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Line | — | — | 1 line | *[Image: 3dgeo_ConstructLine_Flip.png]* |
| [`M_NORMAL`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Plane | — | — | 1 line (1 unit in length) | *[Image: 3dgeo_ConstructLine_Normal_NoSrc2.png]* |
| - Point | — | 1 line (1 unit in length) | *[Image: 3dgeo_ConstructLine_Normal_WithSrc2.png]* |
| [`M_TWO_POINTS`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Point | - Point | — | 1 line | *[Image: 3dgeo_ConstructLine_2points.png]* |

## Constructing a plane

The following table lists the possible operations and related parameter settings required to construct an [`M_PLANE`](../../Reference/3dgeo/M3dgeoConstruct.md) geometry.

| Operation | Src1 | Src2 | Param | Result | Example diagram |
| --- | --- | --- | --- | --- | --- |
| [`M_FACE`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Box<br/>- Cylinder | — | Box: 0 &lt;= Value &lt;= 5 (to specify the face) Finite cylinder: 0 or 1 (to specify the first (0) or second (1) circular base) Infinite cylinder: 0 (specifies the cylinder's start point) *[Image: TableWidener.png]* | 1 plane | *[Image: 3dgeo_ConstructPlane_Face.png]* |
| [`M_FLIP`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Line | — | — | 1 plane | *[Image: 3dgeo_ConstructPlane_Flip.png]* |
| [`M_LINE_AND_POINT`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Line | - Point | — | 1 plane | *[Image: 3dgeo_ConstructPlane_LinePoint.png]* |
| [`M_NORMAL`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Line | — | — | 1 plane | *[Image: 3dgeo_ConstructPlane_Normal_LineOnly.png]* |
| - Point | — | 1 plane | *[Image: 3dgeo_ConstructPlane_Normal_Line&Point.png]* |

## Constructing a point

The following table lists the possible operations and related parameter settings required to construct an [`M_POINT`](../../Reference/3dgeo/M3dgeoConstruct.md) geometry.

| Operation | Src1 | Src2 | Param | Result | Example diagram |
| --- | --- | --- | --- | --- | --- |
| [`M_CENTER`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Box<br/>- Finite cylinder<br/>- Finite line<br/>- Point<br/>- Sphere | — | — | 1 point | *[Image: 3dgeo_ConstructPoint_Center.png]* |
| [`M_CLOSEST_TO_ORIGIN`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Plane | — | — | 1 point | *[Image: 3dgeo_ConstructPoint_ClosestToOrigin.png]* |
| [`M_CORNER`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Box | — | 0 &lt;= Value &lt;= 7 (to specify the corner) *[Image: TableWidener.png]* | 1 point | *[Image: 3dgeo_ConstructPoint_Corner.png]* |
| [`M_END_POINT`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Finite line<br/>- Finite cylinder | — | — | 1 point | *[Image: 3dgeo_ConstructPoint_End.png]* |
| [`M_START_POINT`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Line<br/>- Cylinder | — | — | 1 point | *[Image: 3dgeo_ConstructPoint_Start.png]* |

## Constructing a sphere

The following table lists the possible operations and related parameter settings required to construct an [`M_SPHERE`](../../Reference/3dgeo/M3dgeoConstruct.md) geometry.

| Operation | Src1 | Src2 | Param | Result | Example diagram |
| --- | --- | --- | --- | --- | --- |
| [`M_CENTER_AND_RADIUS`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Point | — | Radius >= 0 | 1 sphere | *[Image: 3dgeo_ConstructSphere_Center.png]* |
| [`M_DIAMETER`](../../Reference/3dgeo/M3dgeoConstruct.md) | - Point | - Point | — | 1 sphere | *[Image: 3dgeo_ConstructSphere_Diameter.png]* |

## Constructing 3D geometries example

The _3dmetrologyconstructions.cpp_ example uses [`M3dgeoConstruct`](../../Reference/3dgeo/M3dgeoConstruct.md) to draw lines between points, as well as to flip planes to a required orientation. To run/view this and other examples, use Aurora Imaging Example Launcher.
