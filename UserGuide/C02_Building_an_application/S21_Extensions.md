---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Extensions
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Extensions"
---

# Aurora Imaging Library file extension list

The following is a list of all the various Aurora Imaging Library file extension types that the various Aurora Imaging Library module's functions can read from and/or write to; this includes but is not limited to saving or loading contexts, images, and result buffers. The modules that do not support saving or loading are indicated as such in the table below.

Note that Aurora Imaging Library will generally determine the compatibility of a file based on the data within the file, and not on the file extension itself.

| Abbreviation | Module name | Proprietary file extension | Other file extensions |
| --- | --- | --- | --- |
| Magm | Advanced Geometric Matcher | .magm | .dxf (to define a single-definition model from a CAD DXF file) |
| Mapp | Application | Not applicable | Not applicable |
| Mbead | Bead | .mbead,  (.bcf deprecated) | Not applicable |
| Mblob | Blob Analysis | .mblob | Not applicable |
| Mbuf | Buffer | .mbuf, .mbufc, .mim | .avi, .bmp,  .gendc,  .png,  .ply, .raw, .stl,  .tiff,  .jp2, .jpeg/jpg |
| Mcal | Camera Calibration | .mca,  .mim (if saved with an image) | Not applicable |
| Mclass | Classification | .mclass,  .mclassd | .csv,  .dot, .onnx, .txt |
| Mcode | Code | .mco (for an Aurora Imaging Library Code reader context) | .txt (for a code report) |
| Mcol | Color Analysis (Color Matching context) | .mcol (for a color matching context) | Not applicable |
| Color Analysis (Relative Color Calibration context) | .mccc (for a relative color calibration context) | Not applicable |
| Mcom | Industrial Communication | Not applicable | Not applicable |
| Mdig | Digitizer | .dcf (digitizer configuration format) | Not applicable |
| Mdisp | Display | .vcf | Not applicable |
| Mdlocr | Deep Learning OCR | .mdlocr | Not applicable |
| Mdmr | SureDotOCR | .mdmr (for a SureDotOCR context),  .mdmrf (for a SureDotOCR font) | Not applicable |
| Medge | Edge Finder | .mef | .dxf (for an Edge Finder CAD result buffer) |
| Mfpga | FPGA | .mbf | Not applicable |
| Mfunc | Function Development | Not applicable | .dll |
| Mgen | Data Generation | Not applicable | Not applicable |
| Mgra | Graphics | Not applicable | Not applicable |
| Mim | Image Processing | .mic | Not applicable |
| Mmeas | Measurement | .mrk | Not applicable |
| Mmet | Metrology | .met | Not applicable |
| Mmod | Geometric Model Finder | .mmf | .dxf (to define a synthetic model) |
| Mobj | Aurora Imaging Library objects | Not applicable | Not applicable |
| Mocr | Optical Character Recognition | .mfo | Not applicable |
| Mpat | Pattern Matching | .mpat,  (.mod deprecated) | Not applicable |
| Mreg | Registration | .mreg, (.mrf deprecated) | Not applicable |
| Mseq | Sequence | Not applicable | .avi,  .mpeg4/mp4 (with H .264 encoding) |
| Mstr | String Reader | .msr | any system font file supported by your operating system |
| Msys | System | Not applicable | Not applicable |
| Mthr | Thread | Not applicable | Not applicable |
| Profiler | Aurora Imaging Profiler | .mtrace | Not applicable |
| M3dblob | 3D Blob | .m3dblob | Not applicable |
| M3ddisp | 3D Display | Not applicable | Not applicable |
| M3dgeo | 3D Geometry | .m3dgeo | Not applicable |
| M3dgra | 3D Graphics | Not applicable | Not applicable |
| M3dim | 3D Image Processing | .m3dim | Not applicable |
| M3dmap | 3D Reconstruction | .m3d | .ply,  .stl |
| M3dmeas | 3D Measurement | .m3dmeas | Not applicable |
| M3dmet | 3D Metrology | .m3dmet | Not applicable |
| M3dmod | 3D Model Finder | .m3dmod | Not applicable |
| M3dreg | 3D Registration | .m3dreg | Not applicable |
