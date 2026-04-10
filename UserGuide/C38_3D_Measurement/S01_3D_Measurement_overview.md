---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: 3D_Measurement_overview
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / 3D Measurement overview"
---

# Aurora Imaging Library 3D Measurement module

The Aurora Imaging Library 3D Measurement module allows you to find markers in a depth map and to fit a geometry to found markers. Markers denote the location of transitions along a profile. An edge transition is a sudden, sustained increase or decrease in Z-values at an object's border. A ridge transition is a sudden, temporary increase or decrease in Z-values that quickly returns to the starting depth, identifying the center of a thin, crest-like object. Note that invalid transitions can also be identified, either in the middle of an invalid region, or where the data goes from valid to invalid (or vice versa).

The 3D Measurement module allows you to search for markers in each row (profile) of a depth map (useful for analyzing the entire depth map), in a profile extracted from a depth map along a path (useful for finding an object when you don't know its exact location), or in profiles extracted from a depth map at regular intervals perpendicular to a template (useful for verifying the location or shape of an object).

*[Image: 3dmeas_context_comparison.png]*

Note that the 3D Measurement module can only fit a geometry to markers found in a template's perpendicular profiles.

To help locate the marker in a depth map, you can define its essential characteristics, such as the required polarity and location. The more precisely you define the characteristics of a marker, the more likely Aurora Imaging Library can distinguish it from other similar (though unwanted) aspects of the depth map.

The module also allows you to restore a 3D measurement context from a file or memory stream, or to save a 3D measurement context to a file or memory stream.
