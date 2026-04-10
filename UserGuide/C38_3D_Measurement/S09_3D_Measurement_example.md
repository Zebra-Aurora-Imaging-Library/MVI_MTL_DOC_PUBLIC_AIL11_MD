---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Measurement
section: 3D_Measurement_example
module_tag: 3dmeas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Measurement / 3D Measurement example"
---

# 3D measurement example

The example _3dMeasOverview.cpp_ demonstrates how to find markers in a depth map and fit a geometry to found markers. To run this example, use Aurora Imaging Example Launcher.

Specifically, this example does the following:

- Finds markers in each row (profile) of a depth map.
  *[Image: 3dmeas_profile_context_example.png]*
- Extracts a profile along a path and then finds markers in the extracted profile.
  *[Image: 3dmeas_path_context_example.png]*
- Extracts profiles at regular intervals perpendicular to a template, finds a marker in each extracted profile, and then fits a geometry to the found markers.
  *[Image: 3dmeas_template_context_example.png]*
- Extracts a profile along a path and then finds pair markers in the extracted profile.
  *[Image: 3dmeas_pair_marker_example.png]*
- Extracts a profile along a path and then finds ridge transitions in the extracted profile.
  *[Image: 3dmeas_ridge_transition_example.png]*
