---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: Basic_concepts
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library 3D Measurement module

The basic concepts and vocabulary conventions for the Aurora Imaging Library 3D Measurement module are:

- **Depth map**. An image where the gray value of a pixel represents its depth in the world, or a container where the range component stores depth values with X and Y-coordinates identified by column and row index, respectively.
- **Edge transition**. A transition from a lower Z-value to a higher Z-value (or vice versa) that is sustained over a distance and occurs at an object's border.
- **Gap**. An invalid region in the profile resulting from missing data in the source depth map.
- **Invalid transition**. A transition from valid data to invalid data (or vice versa).
- **Marker**. Identifies the location of a single transition or pair of transitions along a profile. The Aurora Imaging Library 3D Measurement module allows you to search for edge transitions, invalid transitions, and ridge transitions.
- **Path**. The route along which to find transitions.
- **Profile**. A representation of the depth information in a cross-section of data.
- **Ridge transition**. A transition from a lower Z-value to a higher Z-value (or vice versa) that quickly returns to the starting depth, forming a local maximum or minimum at the center of a thin object.
- **Template**. Represents the location of an expected edge in a depth map.
