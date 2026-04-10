---
doctype: Reference
module: func
function: MfuncParam
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / func / MfuncParam"
---

# MfuncParam

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

> Register a parameter.

## Syntax

```c
void MfuncParam(
    AIL_ID       ContextFuncId,  //in
    AIL_INT      ParamIndex,     //in
    const void * ParamValuePtr,  //in
    AIL_UINT     ParamType,      //in
    AIL_INT      NumOfItems,     //in
    AIL_INT64    AilObjectType,  //in
    AIL_INT      Attribute       //in
)
```

## Description

This function allows you to register a parameter of any data type to the current user-defined Aurora Imaging Library function. The [`MfuncParam`](../../Reference/func/MfuncParam.md) function should be called in the caller function after a call to [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) and before a call to [`MfuncCall`](../../Reference/func/MfuncCall.md).

[`MfuncParam`](../../Reference/func/MfuncParam.md) calls an explicitly named type-specific variation of this function. For example, to register an _AIL_INT_, it calls [`MfuncParamAilInt`](../../Reference/MfuncParamAilInt.md). Note that, for execution efficiency, you can call the appropriate, explicitly named, type-specific variation of [`MfuncParam`](../../Reference/func/MfuncParam.md) directly. When registering parameters using the explicitly named type-specific variation of [`MfuncParam`](../../Reference/func/MfuncParam.md), you must use that function's specific signature, found below.

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies the identifier of the user-defined Aurora Imaging Library function.

### `ParamIndex` *(in, AIL_INT)*

Specifies the index of the parameter within the user-defined Aurora Imaging Library function's parameter list. The index of the first parameter is considered to be one.

### `ParamValuePtr` *(in, *void)*

Specifies the address of the array or variable to register.

### `ParamType` *(in, AIL_UINT)*

Specifies the parameter type of the parameter in the user-defined Aurora Imaging Library function. Parameter types include data types, arrays of data types, void pointers, strings, and file names.

### `NumOfItems` *(in, AIL_INT)*

Specifies the size of the data pointed to by the [`ParamValuePtr`](../../Reference/func/MfuncParam.md) parameter.

### `AilObjectType` *(in, AIL_INT64)*

Specifies the type of the Aurora Imaging Library object when the data type of the parameter of the user-defined function is _AIL_ID_. The available object types are divided into two groups. When the specified object is of a type listed in the first group, you can specify a single or multiple object type(s) from this group. The object types of the second group cannot be used in combination with each other.

*For specifying the type of Aurora Imaging Library object accepted by the Aurora Imaging Library identifier type parameter (one or multiple)*

| Value | Description |
| --- | --- |
| `M_APPLICATION` | Specifies an Aurora Imaging Library application context allocated using [`MappAlloc`](../../Reference/app/MappAlloc.md). |
| `M_ARRAY` | Specifies an Aurora Imaging Library array buffer allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md). |
| `M_AUGMENTATION_RESULT` | Specifies an Aurora Imaging Library augmentation result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md). |
| `M_CONTAINER` | Specifies an Aurora Imaging Library container buffer allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md). |
| `M_COUNT_LIST` | Specifies an Aurora Imaging Library count list result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_COUNT_LIST`](../../Reference/im/MimAllocResult.md). |
| `M_DIGITIZER` | Specifies an Aurora Imaging Library digitizer allocated using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). |
| `M_DISPLAY` | Specifies an Aurora Imaging Library display allocated using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md). |
| `M_EVENT` | Specifies an Aurora Imaging Library event allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) with[`M_EVENT`](../../Reference/thr/MthrAlloc.md). |
| `M_EVENT_LIST` | Specifies an Aurora Imaging Library event list result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md). |
| `M_EXTREME_LIST` | Specifies an Aurora Imaging Library extreme list result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md). |
| `M_FIND_ORIENTATION_LIST` | Specifies an Aurora Imaging Library find orientation list result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md). |
| `M_GRAPHIC_CONTEXT` | Specifies an Aurora Imaging Library 2D graphics context allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |
| `M_GRAPHIC_LIST` | Specifies an Aurora Imaging Library 2D graphics list allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). |
| `M_HIST_LIST` | Specifies an Aurora Imaging Library histogram list result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_HIST_LIST`](../../Reference/im/MimAllocResult.md). |
| `M_HTTP_SERVER` | Specifies an HTTP server allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md) with [`M_HTTP_SERVER`](../../Reference/obj/MobjAlloc.md). |
| `M_IM_CONTEXT` | Specifies an Aurora Imaging Library image processing context allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md). |
| `M_IMAGE` | Specifies an Aurora Imaging Library image buffer allocated using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md). |
| `M_KERNEL` | Specifies an Aurora Imaging Library kernel buffer allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md). |
| `M_LOCATE_DIFFERENCE_RESULT` | Specifies an Aurora Imaging Library locate difference result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md). |
| `M_LOCATE_PEAK_1D_RESULT` | Specifies an Aurora Imaging Library locate 1D peak result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md). |
| `M_LUT` | Specifies an Aurora Imaging Library LUT buffer allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_LUT`](../../Reference/buf/MbufAlloc2d.md). |
| `M_MESSAGE_MAILBOX` | Specifies an Aurora Imaging Library message mailbox allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md). |
| `M_PROJ_LIST` | Specifies an Aurora Imaging Library project list result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md). |
| `M_SEQUENCE_CONTEXT` | Specifies an Aurora Imaging Library sequence context allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md). |
| `M_STATISTICS_RESULT` | Specifies an Aurora Imaging Library [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md). |
| `M_STRUCT_ELEMENT` | Specifies an Aurora Imaging Library structuring element buffer allocated using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) with [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md). |
| `M_SYS_IO_CONTEXT` | Specifies an Aurora Imaging Library system I/O context allocated using [`MsysIoAlloc`](../../Reference/sysIo/MsysIoAlloc.md). |
| `M_SYSTEM` | Specifies an Aurora Imaging Library system context allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |
| `M_WAVELET_TRANSFORM_RESULT` | Specifies an Aurora Imaging Library wavelet transform result buffer allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md). |

*For specifying the type of Aurora Imaging Library object accepted by the Aurora Imaging Library identifier type parameter (only one)*

| Value | Description |
| --- | --- |
| `M_3D_DISPLAY` | Specifies an Aurora Imaging Library 3D display allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md). |
| `M_3D_DISPLAY_PICKING_AREA_CONTEXT` | Specifies an Aurora Imaging Library picking area context allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) with [`M_PICKING_AREA_CONTEXT`](../../Reference/3ddisp/M3ddispAlloc.md). |
| `M_3D_DISPLAY_PICKING_AREA_RESULT` | Specifies an Aurora Imaging Library picking area result buffer allocated using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md) with [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md). |
| `M_3D_DISPLAY_PICKING_CONTEXT` | Specifies an Aurora Imaging Library picking context allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) with [`M_PICKING_CONTEXT`](../../Reference/3ddisp/M3ddispAlloc.md). |
| `M_3D_DISPLAY_PICKING_RESULT` | Specifies an Aurora Imaging Library picking result buffer allocated using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md) with [`M_PICKING_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md). |
| `M_3D_GRAPHIC_LIST` | Specifies an Aurora Imaging Library 3D graphics list allocated using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md). |
| `M_3DBLOB_CALCULATE_CONTEXT` | Specifies an Aurora Imaging Library 3D blob context that can be used with [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md), allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_CALCULATE_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). |
| `M_3DBLOB_DRAW_3D_CONTEXT` | Specifies an Aurora Imaging Library 3D blob context that can be used with [`M3dblobDraw3d`](../../Reference/3dblob/M3dblobDraw3d.md), allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). |
| `M_3DBLOB_SEGMENTATION_CONTEXT` | Specifies an Aurora Imaging Library 3D blob context that can be used with [`M3dblobSegment`](../../Reference/3dblob/M3dblobSegment.md), allocated using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_SEGMENTATION_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). |
| `M_3DBLOB_SEGMENTATION_RESULT` | Specifies an Aurora Imaging Library 3D blob result buffer for segmentation results, allocated using [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md). |
| `M_3DGEO_GEOMETRY` | Specifies an Aurora Imaging Library 3D geometry object allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `M_3DGEO_TRANSFORMATION_MATRIX` | Specifies an Aurora Imaging Library 3D transformation matrix object allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md). |
| `M_3DIM_CALCULATE_MAP_SIZE_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_CALCULATE_MAP_SIZE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_FILL_GAPS_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILL_GAPS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_FILTER_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimFilter`](../../Reference/3dim/M3dimFilter.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_FILTER_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_FIND_TRANSFORMATION_RESULT` | Specifies an Aurora Imaging Library 3D image processing result buffer for find transformation results, allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_FIND_TRANSFORMATION_RESULT`](../../Reference/3dim/M3dimAllocResult.md). |
| `M_3DIM_LATTICE_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_LATTICE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_LATTICE_RESULT` | Specifies an Aurora Imaging Library 3D image processing result buffer for lattice results, allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_LATTICE_RESULT`](../../Reference/3dim/M3dimAllocResult.md). |
| `M_3DIM_MESH_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimMesh`](../../Reference/3dim/M3dimMesh.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_MESH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_NORMALS_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimNormals`](../../Reference/3dim/M3dimNormals.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_NORMALS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_OUTLIERS_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimOutliers`](../../Reference/3dim/M3dimOutliers.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_OUTLIERS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_PROFILE_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROFILE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_PROFILE_RESULT` | Specifies an Aurora Imaging Library 3D image processing result buffer for profile results, allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md). |
| `M_3DIM_PROJECT_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROJECT_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_REMAP_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimRemapDepthMap`](../../Reference/3dim/M3dimRemapDepthMap.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_REMAP_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_STATISTICS_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimStat`](../../Reference/3dim/M3dimStat.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_STATISTICS_RESULT` | Specifies an Aurora Imaging Library 3D image processing result buffer used to store [`M3dimStat`](../../Reference/3dim/M3dimStat.md) results, and is allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/3dim/M3dimAllocResult.md). |
| `M_3DIM_STITCH_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STITCH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_SUBSAMPLE_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimSample`](../../Reference/3dim/M3dimSample.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DIM_SURFACE_SAMPLE_CONTEXT` | Specifies an Aurora Imaging Library 3D image processing context that can be used with [`M3dimSample`](../../Reference/3dim/M3dimSample.md), allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SURFACE_SAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md). |
| `M_3DMAP_ALIGN_CONTEXT` | Specifies an Aurora Imaging Library 3D align context allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_ALIGN_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md). |
| `M_3DMAP_ALIGN_RESULT` | Specifies an Aurora Imaging Library 3D align result buffer allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_ALIGN_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). |
| `M_3DMAP_DEPTH_CORRECTED_DATA` | Specifies an Aurora Imaging Library 3D reconstruction result buffer allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_DEPTH_CORRECTED_DATA`](../../Reference/3dmap/M3dmapAllocResult.md). |
| `M_3DMAP_DRAW_3D_CONTEXT` | Specifies an Aurora Imaging Library 3D draw context allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmap/M3dmapAlloc.md). |
| `M_3DMAP_LASER_CALIBRATION_DATA` | Specifies an Aurora Imaging Library 3D reconstruction result buffer allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_LASER_CALIBRATION_DATA`](../../Reference/3dmap/M3dmapAllocResult.md). |
| `M_3DMAP_LASER_CONTEXT` | Specifies an Aurora Imaging Library 3D reconstruction context allocated using [`M3dmapAlloc`](../../Reference/3dmap/M3dmapAlloc.md) with [`M_LASER`](../../Reference/3dmap/M3dmapAlloc.md). |
| `M_3DMAP_POINT_CLOUD_RESULT` | Specifies an Aurora Imaging Library 3D reconstruction result buffer allocated using [`M3dmapAllocResult`](../../Reference/3dmap/M3dmapAllocResult.md) with [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). |
| `M_3DMEAS_DRAW_3D_PATH_CONTEXT` | Specifies an Aurora Imaging Library draw path 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMEAS_DRAW_3D_PROFILE_CONTEXT` | Specifies an Aurora Imaging Library draw profile 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMEAS_DRAW_3D_TEMPLATE_CONTEXT` | Specifies an Aurora Imaging Library draw template 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_DRAW_3D_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMEAS_FIND_MARKER_PATH_CONTEXT` | Specifies an Aurora Imaging Library path 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIND_MARKER_PATH_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMEAS_FIND_MARKER_PATH_RESULT` | Specifies an Aurora Imaging Library path 3D measurement result allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_PATH_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md). |
| `M_3DMEAS_FIND_MARKER_PROFILE_CONTEXT` | Specifies an Aurora Imaging Library profile 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIND_MARKER_PROFILE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMEAS_FIND_MARKER_PROFILE_RESULT` | Specifies an Aurora Imaging Library profile 3D measurement result allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_PROFILE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md). |
| `M_3DMEAS_FIND_MARKER_TEMPLATE_CONTEXT` | Specifies an Aurora Imaging Library template 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIND_MARKER_TEMPLATE_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMEAS_FIND_MARKER_TEMPLATE_RESULT` | Specifies an Aurora Imaging Library template 3D measurement result allocated using [`M3dmeasAllocResult`](../../Reference/3dmeas/M3dmeasAllocResult.md) with [`M_FIND_MARKER_TEMPLATE_RESULT`](../../Reference/3dmeas/M3dmeasAllocResult.md). |
| `M_3DMEAS_FIT_CONTEXT` | Specifies an Aurora Imaging Library fit 3D measurement context allocated using [`M3dmeasAlloc`](../../Reference/3dmeas/M3dmeasAlloc.md) with [`M_FIT_CONTEXT`](../../Reference/3dmeas/M3dmeasAlloc.md). |
| `M_3DMET_CALCULATE_RESULT` | Specifies an Aurora Imaging Library calculate 3D metrology result buffer allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_CALCULATE_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md). |
| `M_3DMET_DISTANCE_CONTEXT` | Specifies an Aurora Imaging Library distance 3D metrology context allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DISTANCE_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |
| `M_3DMET_DRAW_3D_CONTEXT` | Specifies an Aurora Imaging Library draw 3D metrology context allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |
| `M_3DMET_FIT_CONTEXT` | Specifies an Aurora Imaging Library fit 3D metrology context allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_FIT_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |
| `M_3DMET_FIT_RESULT` | Specifies an Aurora Imaging Library fit 3D metrology result buffer allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_FIT_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md). |
| `M_3DMET_STATISTICS_CONTEXT` | Specifies an Aurora Imaging Library statistics 3D metrology context allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_STATISTICS_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |
| `M_3DMET_STATISTICS_RESULT` | Specifies an Aurora Imaging Library statistics 3D metrology result buffer allocated using [`M3dmetAllocResult`](../../Reference/3dmet/M3dmetAllocResult.md) with [`M_STATISTICS_RESULT`](../../Reference/3dmet/M3dmetAllocResult.md). |
| `M_3DMET_VOLUME_CONTEXT` | Specifies an Aurora Imaging Library calculate volume 3D metrology context allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_VOLUME_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). |
| `M_3DMOD_DRAW_3D_CONTEXT` | Specifies an Aurora Imaging Library draw 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_DRAW_3D_GEOMETRIC_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw occurrences of geometric models). |
| `M_3DMOD_DRAW_3D_SURFACE_CONTEXT` | Specifies an Aurora Imaging Library draw 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_DRAW_3D_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md) (to draw occurrences of surface models). |
| `M_3DMOD_FIND_BOX_CONTEXT` | Specifies an Aurora Imaging Library box 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_BOX_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md). |
| `M_3DMOD_FIND_BOX_RESULT` | Specifies an Aurora Imaging Library box 3D model finder result buffer allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_BOX_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md). |
| `M_3DMOD_FIND_CYLINDER_CONTEXT` | Specifies an Aurora Imaging Library cylinder 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_CYLINDER_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md). |
| `M_3DMOD_FIND_CYLINDER_RESULT` | Specifies an Aurora Imaging Library cylinder 3D model finder result buffer allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_CYLINDER_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md). |
| `M_3DMOD_FIND_PLANAR_SURFACE_CONTEXT` | Specifies an Aurora Imaging Library planar surface 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_PLANAR_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md). |
| `M_3DMOD_FIND_PLANAR_SURFACE_RESULT` | Specifies an Aurora Imaging Library planar surface 3D model finder result buffer allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_PLANAR_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md). |
| `M_3DMOD_FIND_RECTANGULAR_PLANE_CONTEXT` | Specifies an Aurora Imaging Library rectangular plane 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_RECTANGULAR_PLANE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md). |
| `M_3DMOD_FIND_RECTANGULAR_PLANE_RESULT` | Specifies an Aurora Imaging Library rectangular plane 3D model finder result buffer allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_RECTANGULAR_PLANE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md). |
| `M_3DMOD_FIND_SPHERE_CONTEXT` | Specifies an Aurora Imaging Library sphere 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SPHERE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md). |
| `M_3DMOD_FIND_SPHERE_RESULT` | Specifies an Aurora Imaging Library sphere 3D model finder result buffer allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SPHERE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md). |
| `M_3DMOD_FIND_SURFACE_CONTEXT` | Specifies an Aurora Imaging Library surface 3D model finder context that can be allocated using [`M3dmodAlloc`](../../Reference/3dmod/M3dmodAlloc.md) with [`M_FIND_SURFACE_CONTEXT`](../../Reference/3dmod/M3dmodAlloc.md). |
| `M_3DMOD_FIND_SURFACE_RESULT` | Specifies an Aurora Imaging Library surface 3D model finder result buffer allocated using [`M3dmodAllocResult`](../../Reference/3dmod/M3dmodAllocResult.md) with [`M_FIND_SURFACE_RESULT`](../../Reference/3dmod/M3dmodAllocResult.md). |
| `M_3DREG_DRAW_3D_CONTEXT` | Specifies an Aurora Imaging Library draw 3D registration context that can be used with [`M3dregDraw3d`](../../Reference/3dreg/M3dregDraw3d.md), allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md). |
| `M_3DREG_PAIRWISE_REGISTRATION_CONTEXT` | Specifies an Aurora Imaging Library pairwise 3D registration context allocated using [`M3dregAlloc`](../../Reference/3dreg/M3dregAlloc.md) with [`M_PAIRWISE_REGISTRATION_CONTEXT`](../../Reference/3dreg/M3dregAlloc.md). |
| `M_3DREG_PAIRWISE_REGISTRATION_RESULT` | Specifies an Aurora Imaging Library pairwise 3D registration result buffer allocated using [`M3dregAllocResult`](../../Reference/3dreg/M3dregAllocResult.md) with [`M_PAIRWISE_REGISTRATION_RESULT`](../../Reference/3dreg/M3dregAllocResult.md). |
| `M_AGM_EDGE_BASED_FIND_CONTEXT` | Specifies an Aurora Imaging Library Advanced Geometric Matching context allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md). |
| `M_AGM_EDGE_BASED_FIND_RESULT` | Specifies an Aurora Imaging Library Advanced Geometric Matching result buffer allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md). |
| `M_AGM_EDGE_BASED_TRAIN_CONTEXT` | Specifies an Aurora Imaging Library Advanced Geometric Matching context allocated using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md). |
| `M_AGM_EDGE_BASED_TRAIN_RESULT` | Specifies an Aurora Imaging Library Advanced Geometric Matching result buffer allocated using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md). |
| `M_BEAD_CONTEXT` | Specifies an Aurora Imaging Library bead context allocated using [`MbeadAlloc`](../../Reference/bead/MbeadAlloc.md). |
| `M_BEAD_RESULT` | Specifies an Aurora Imaging Library bead result buffer allocated using [`MbeadAllocResult`](../../Reference/bead/MbeadAllocResult.md). |
| `M_BLOB_CONTEXT` | Specifies an Aurora Imaging Library blob analysis context allocated using [`MblobAlloc`](../../Reference/blob/MblobAlloc.md). |
| `M_BLOB_RESULT` | Specifies an Aurora Imaging Library blob analysis result buffer allocated using [`MblobAllocResult`](../../Reference/blob/MblobAllocResult.md). |
| `M_CAL_CALCULATE_HAND_EYE_CONTEXT` | Specifies an Aurora Imaging Library calculate hand-eye context (hand-eye calibration) allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_CALCULATE_HAND_EYE_CONTEXT`](../../Reference/cal/McalAlloc.md). |
| `M_CAL_CALCULATE_HAND_EYE_RESULT` | Specifies an Aurora Imaging Library calculate hand-eye result buffer allocated using [`McalAllocResult`](../../Reference/cal/McalAllocResult.md) with [`M_CALCULATE_HAND_EYE_RESULT`](../../Reference/cal/McalAllocResult.md). |
| `M_CAL_CONTEXT` | Specifies an Aurora Imaging Library camera calibration context allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md). |
| `M_CAL_DRAW_3D_CONTEXT` | Specifies an Aurora Imaging Library 3D draw calibration context allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md)with [`M_DRAW_3D_CONTEXT`](../../Reference/cal/McalAlloc.md). |
| `M_CAL_FIXTURING_OFFSET` | Specifies an Aurora Imaging Library fixturing offset object allocated using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_FIXTURING_OFFSET`](../../Reference/cal/McalAlloc.md). |
| `M_CLASS_CLASSIFIER_ANO_PREDEFINED_CONTEXT` | Specifies an Aurora Imaging Library predefined anomaly detection classifier context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_CLASSIFIER_CNN_PREDEFINED_CONTEXT` | Specifies an Aurora Imaging Library predefined CNN classifier context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_CLASSIFIER_DET_PREDEFINED_CONTEXT` | Specifies an Aurora Imaging Library predefined object detection classifier context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_CLASSIFIER_ONNX_CONTEXT` | Specifies an Aurora Imaging Library ONNX classifier context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_CLASSIFIER_SEG_PREDEFINED_CONTEXT` | Specifies an Aurora Imaging Library predefined segmentation classifier context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_CLASSIFIER_TREE_ENSEMBLE_CONTEXT` | Specifies an Aurora Imaging Library tree ensemble classifier context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_DATASET_FEATURES` | Specifies an Aurora Imaging Library features dataset context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_DATASET_IMAGES` | Specifies an Aurora Imaging Library images dataset context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_PREDICT_ANO_RESULT` | Specifies an Aurora Imaging Library anomaly detection prediction result buffer, allocated using[`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_PREDICT_CNN_RESULT` | Specifies an Aurora Imaging Library CNN prediction result buffer, allocated using[`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_PREDICT_DET_RESULT` | Specifies an Aurora Imaging Library object detection prediction result buffer, allocated using[`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_PREDICT_ONNX_RESULT` | Specifies an ONNX prediction result buffer, allocated using[`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_ONNX_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_PREDICT_SEG_RESULT` | Specifies an Aurora Imaging Library segmentation prediction result buffer, allocated using[`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_PREDICT_TREE_ENSEMBLE_RESULT` | Specifies an Aurora Imaging Library tree ensemble prediction result buffer, allocated using[`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_PREPARE_IMAGES_CNN` | Specifies an Aurora Imaging Library CNN data preparation context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_PREPARE_IMAGES_DET` | Specifies an Aurora Imaging Library object detection data preparation context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_PREPARE_IMAGES_SEG` | Specifies an Aurora Imaging Library segmentation data preparation context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_STAT_ANO_CONTEXT` | Specifies an Aurora Imaging Library statistics classification context for anomaly detection, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_ANO`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_STAT_ANO_RESULT` | Specifies an Aurora Imaging Library statistics classification result buffer for anomaly detection, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_STAT_CNN_CONTEXT` | Specifies an Aurora Imaging Library statistics classification context for image classification, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_STAT_CNN_RESULT` | Specifies an Aurora Imaging Library statistics classification result buffer for image classification, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_STAT_DET_CONTEXT` | Specifies an Aurora Imaging Library statistics classification context for object detection, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_DET`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_STAT_DET_RESULT` | Specifies an Aurora Imaging Library statistics classification result buffer for object detection, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_STAT_SEG_CONTEXT` | Specifies an Aurora Imaging Library statistics classification context for segmentation, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_STAT_SEG_RESULT` | Specifies an Aurora Imaging Library statistics classification result buffer for segmentation, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_STAT_TREE_ENSEMBLE_CONTEXT` | Specifies an Aurora Imaging Library statistics classification context for feature classification, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_STAT_TREE_ENSEMBLE_RESULT` | Specifies an Aurora Imaging Library statistics classification result buffer for feature classification, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_TRAIN_ANO_CONTEXT` | Specifies an Aurora Imaging Library anomaly detection training context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_TRAIN_ANO_RESULT` | Specifies an Aurora Imaging Library anomaly detection training result buffer, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_TRAIN_CNN_CONTEXT` | Specifies an Aurora Imaging Library CNN training context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_TRAIN_CNN_RESULT` | Specifies an Aurora Imaging Library CNN training result buffer, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_TRAIN_DET_CONTEXT` | Specifies an Aurora Imaging Library object detection training context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_TRAIN_DET_RESULT` | Specifies an Aurora Imaging Library object detection training result buffer, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_TRAIN_SEG_CONTEXT` | Specifies an Aurora Imaging Library segmentation training context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_TRAIN_SEG_RESULT` | Specifies an Aurora Imaging Library segmentation training result buffer, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CLASS_TRAIN_TREE_ENSEMBLE_CONTEXT` | Specifies an Aurora Imaging Library tree ensemble training context, allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `M_CLASS_TRAIN_TREE_ENSEMBLE_RESULT` | Specifies an Aurora Imaging Library tree ensemble training result buffer, allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `M_CODE_CONTEXT` | Specifies an Aurora Imaging Library code context allocated using [`McolAlloc`](../../Reference/col/McolAlloc.md). |
| `M_CODE_DETECT_RESULT` | Specifies an Aurora Imaging Library code detect result buffer allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md). |
| `M_CODE_GRADE_RESULT` | Specifies an Aurora Imaging Library code grade result buffer allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md). |
| `M_CODE_MODEL` | Specifies an Aurora Imaging Library code model allocated using [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_ADD`](../../Reference/code/McodeModel.md). |
| `M_CODE_READ_RESULT` | Specifies an Aurora Imaging Library code read result buffer allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md). |
| `M_CODE_TRAIN_RESULT` | Specifies an Aurora Imaging Library code train result buffer allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md). |
| `M_CODE_WRITE_RESULT` | Specifies an Aurora Imaging Library code write result buffer allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md). |
| `M_COL_MATCH_CONTEXT` | Specifies an Aurora Imaging Library color analysis context (for matching) allocated using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md). |
| `M_COL_MATCH_RESULT` | Specifies an Aurora Imaging Library color analysis result buffer (for matching) allocated using [`McolAllocResult`](../../Reference/col/McolAllocResult.md) with [`M_COLOR_MATCHING_RESULT`](../../Reference/col/McolAllocResult.md). |
| `M_COL_RELATIVE_CALIBRATION_CONTEXT` | Specifies an Aurora Imaging Library relative color calibration context allocated using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md). |
| `M_COM_CONTEXT` | Specifies an Aurora Imaging Library industrial communication context allocated using [`McomAlloc`](../../Reference/com/McomAlloc.md). |
| `M_DLOCR_DATASET` | Specifies an Aurora Imaging Library Deep Learning OCR dataset allocated using [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md). |
| `M_DLOCR_FINETUNE_CONTEXT` | Specifies an Aurora Imaging Library Deep Learning OCR fine-tuning context allocated using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md). |
| `M_DLOCR_FINETUNE_RESULT` | Specifies an Aurora Imaging Library Deep Learning OCR fine-tuning result buffer allocated using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md). |
| `M_DLOCR_READ_CONTEXT` | Specifies an Aurora Imaging Library Deep Learning OCR read context allocated using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md). |
| `M_DLOCR_READ_RESULT` | Specifies an Aurora Imaging Library Deep Learning OCR read result buffer allocated using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md). |
| `M_DMR_CONTEXT` | Specifies an Aurora Imaging Library SureDotOCR context allocated using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md). |
| `M_DMR_RESULT` | Specifies an Aurora Imaging Library SureDotOCR result buffer allocated using [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md). |
| `M_EDGE_CONTOUR` | Specifies an Aurora Imaging Library edge contour context allocated using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md) with [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md). |
| `M_EDGE_CREST` | Specifies an Aurora Imaging Library edge crest context allocated using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md) with [`M_CREST`](../../Reference/edge/MedgeAlloc.md). |
| `M_EDGE_RESULT` | Specifies an Aurora Imaging Library edge result buffer allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md). |
| `M_MEAS_CONTEXT` | Specifies an Aurora Imaging Library measurement context allocated using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). |
| `M_MEAS_MARKER` | Specifies an Aurora Imaging Library measurement marker allocated using [`MmeasAllocMarker`](../../Reference/meas/MmeasAllocMarker.md). |
| `M_MEAS_RESULT` | Specifies an Aurora Imaging Library measurement result buffer allocated using [`MmeasAllocResult`](../../Reference/meas/MmeasAllocResult.md). |
| `M_MET_CONTEXT` | Specifies an Aurora Imaging Library metrology context allocated using [`MmetAlloc`](../../Reference/met/MmetAlloc.md) with [`M_CONTEXT`](../../Reference/met/MmetAlloc.md). |
| `M_MET_DERIVED_GEOMETRY_REGION` | Specifies an Aurora Imaging Library metrology derived geometry region object allocated using [`MmetAlloc`](../../Reference/met/MmetAlloc.md) with [`M_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetAlloc.md). |
| `M_MET_RESULT` | Specifies an Aurora Imaging Library metrology result buffer allocated using [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md). |
| `M_MOD_GEOMETRIC` | Specifies an Aurora Imaging Library Model Finder context allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md). |
| `M_MOD_GEOMETRIC_CONTROLLED` | Specifies an Aurora Imaging Library Model Finder context allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md). |
| `M_MOD_RESULT` | Specifies an Aurora Imaging Library Model Finder result buffer allocated using [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md) with [`M_DEFAULT`](../../Reference/mod/MmodAllocResult.md). |
| `M_MOD_SHAPE_CIRCLE_CONTEXT` | Specifies an Aurora Imaging Library Model Finder context allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md). |
| `M_MOD_SHAPE_ELLIPSE_CONTEXT` | Specifies an Aurora Imaging Library Model Finder context allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md). |
| `M_MOD_SHAPE_RECTANGLE_CONTEXT` | Specifies an Aurora Imaging Library Model Finder context allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_SHAPE_RECTANGLE`](../../Reference/mod/MmodAlloc.md). |
| `M_MOD_SHAPE_RESULT` | Specifies an Aurora Imaging Library Model Finder result buffer allocated using [`MmodAllocResult`](../../Reference/mod/MmodAllocResult.md) with [`M_SHAPE_...`](../../Reference/mod/MmodAllocResult.md). |
| `M_MOD_SHAPE_SEGMENT_CONTEXT` | Specifies an Aurora Imaging Library Model Finder context allocated using [`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md). |
| `M_MUTEX` | Specifies an Aurora Imaging Library mutex allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) with [`M_MUTEX`](../../Reference/thr/MthrAlloc.md). |
| `M_OCR_FONT` | Specifies an Aurora Imaging Library character recognition font context allocated using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md). |
| `M_OCR_RESULT` | Specifies an Aurora Imaging Library character recognition result buffer allocated using [`MocrAllocResult`](../../Reference/ocr/MocrAllocResult.md). |
| `M_PAT_CONTEXT` | Specifies an Aurora Imaging Library pattern matching context allocated using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md). |
| `M_PAT_RESULT` | Specifies an Aurora Imaging Library pattern matching result buffer allocated using [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md). |
| `M_REG_DFF_CONTEXT` | Specifies an Aurora Imaging Library registration context allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md). |
| `M_REG_DFF_RESULT` | Specifies an Aurora Imaging Library registration result buffer allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `M_REG_EDOF_CONTEXT` | Specifies an Aurora Imaging Library registration context allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md). |
| `M_REG_EDOF_RESULT` | Specifies an Aurora Imaging Library registration result buffer allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `M_REG_HDR_CONTEXT` | Specifies an Aurora Imaging Library registration context allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md). |
| `M_REG_HDR_RESULT` | Specifies an Aurora Imaging Library registration result buffer allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `M_REG_PHOTOMETRIC_STEREO_CONTEXT` | Specifies an Aurora Imaging Library registration context allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_PHOTOMETRIC_STEREO`](../../Reference/reg/MregAlloc.md). |
| `M_REG_PHOTOMETRIC_STEREO_RESULT` | Specifies an Aurora Imaging Library registration result buffer allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `M_REG_STITCHING_CONTEXT` | Specifies an Aurora Imaging Library registration context allocated using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_STITCHING`](../../Reference/reg/MregAlloc.md). |
| `M_REG_STITCHING_RESULT` | Specifies an Aurora Imaging Library registration result buffer allocated using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md). |
| `M_SELECTABLE_THREAD` | Specifies an Aurora Imaging Library selectable thread allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) with [`M_SELECTABLE_THREAD`](../../Reference/thr/MthrAlloc.md). |
| `M_STR_CONTEXT` | Specifies an Aurora Imaging Library string context allocated using [`MstrAlloc`](../../Reference/str/MstrAlloc.md). |
| `M_STR_RESULT` | Specifies an Aurora Imaging Library string result buffer allocated using [`MstrAllocResult`](../../Reference/str/MstrAllocResult.md). |
| `M_SYSTEM_THREAD` | Specifies an Aurora Imaging Library thread allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) with [`M_THREAD`](../../Reference/thr/MthrAlloc.md). |
| `M_THREAD` | Specifies an Aurora Imaging Library thread context object allocated using [`MthrAlloc`](../../Reference/thr/MthrAlloc.md) with [`M_THREAD`](../../Reference/thr/MthrAlloc.md). |
| `M_USER_OBJECT_1` | Specifies an Aurora Imaging Library object from group one of the user-defined object types allocated using [`MfuncAllocId`](../../Reference/func/MfuncAllocId.md) with [`M_USER_OBJECT_1`](../../Reference/func/MfuncAllocId.md). |
| `M_USER_OBJECT_2` | Specifies an Aurora Imaging Library object from group two of the user-defined object types allocated using [`MfuncAllocId`](../../Reference/func/MfuncAllocId.md) with [`M_USER_OBJECT_2`](../../Reference/func/MfuncAllocId.md). |

*For distinguishing between the different object types*

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset within the selected object type group. The value must have only one of its 16 least significant bits set. |

### `Attribute` *(in, AIL_INT)*

Specifies whether the parameter will be used for data input or to store new data.

## Parameter Associations

### For registering a primitive data-type parameter in an Aurora Imaging Library user-defined function

The following values specify to call type-specific variations of [`MfuncParam`](../../Reference/func/MfuncParam.md) that register parameters that handle primitive Aurora Imaging Library data types, such as, _AIL_DOUBLE_, _AIL_INT_, and _AIL_UINT64_, as well as strings, void pointers and file names.  Unless otherwise specified, when specifying values from this table, you must set the [`NumOfItems`](../../Reference/func/MfuncParam.md) parameter to [`M_DEFAULT`](../../Reference/func/MfuncParam.md), the [`AilObjectType`](../../Reference/func/MfuncParam.md) parameter to [`M_NULL`](../../Reference/func/MfuncParam.md), and the [`Attribute`](../../Reference/func/MfuncParam.md) parameter to [`M_NULL`](../../Reference/func/MfuncParam.md).

---

### `M_PARAM_TYPE_AIL_DOUBLE`

Specifies to call [`MfuncParamAilDouble`](../../Reference/MfuncParamAilDouble.md), which then registers a type _AIL_DOUBLE_ parameter for the current user-defined Aurora Imaging Library function.

---

### `M_PARAM_TYPE_AIL_INT`

Specifies to call [`MfuncParamAilInt`](../../Reference/MfuncParamAilInt.md), which then registers a type _AIL_INT_ parameter for the current user-defined Aurora Imaging Library function.

---

### `M_PARAM_TYPE_AIL_INT32`

Specifies to call [`MfuncParamAilInt32`](../../Reference/MfuncParamAilInt32.md), which then registers a type _AIL_INT32_ parameter for the current user-defined Aurora Imaging Library function.

---

### `M_PARAM_TYPE_AIL_INT64`

Specifies to call [`MfuncParamAilInt64`](../../Reference/MfuncParamAilInt64.md), which then registers a type _AIL_INT64_ parameter for the current user-defined Aurora Imaging Library function.

---

### `M_PARAM_TYPE_AIL_UINT`

Specifies to call [`MfuncParamAilUint`](../../Reference/MfuncParamAilUint.md), which then registers a type _AIL_UINT_ parameter for the current user-defined Aurora Imaging Library function.

---

### `M_PARAM_TYPE_AIL_UINT32`

Specifies to call [`MfuncParamAilUint32`](../../Reference/MfuncParamAilUint32.md), which then registers a type _AIL_UINT32_ parameter for the current user-defined Aurora Imaging Library function.

---

### `M_PARAM_TYPE_AIL_UINT64`

Specifies to call [`MfuncParamAilUint64`](../../Reference/MfuncParamAilUint64.md), which then registers a type _AIL_INT64_ parameter for the current user-defined Aurora Imaging Library function.

### For registering a primitive data-type array parameter in an Aurora Imaging Library user-defined function

The following values specify to call type-specific variations of [`MfuncParam`](../../Reference/func/MfuncParam.md) that register parameters that handle arrays of primitive Aurora Imaging Library data types, such as, _AIL_DOUBLE_, _AIL_INT_, and _AIL_UINT64_.  When specifying values from this table, you must set the [`NumOfItems`](../../Reference/func/MfuncParam.md) parameter to the number of items in the array, and you must set the [`AilObjectType`](../../Reference/func/MfuncParam.md) parameter to [`M_NULL`](../../Reference/func/MfuncParam.md).

---

### `M_PARAM_TYPE_ARRAY_AIL_DOUBLE`

Specifies to call [`MfuncParamArrayAilDouble`](../../Reference/MfuncParamArrayAilDouble.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_DOUBLE_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_ARRAY_AIL_INT`

Specifies to call [`MfuncParamArrayAilInt`](../../Reference/MfuncParamArrayAilInt.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_INT_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_ARRAY_AIL_INT32`

Specifies to call [`MfuncParamArrayAilInt32`](../../Reference/MfuncParamArrayAilInt32.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_INT32_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_ARRAY_AIL_INT64`

Specifies to call [`MfuncParamArrayAilInt64`](../../Reference/MfuncParamArrayAilInt64.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_INT64_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_ARRAY_AIL_UINT`

Specifies to call [`MfuncParamArrayAilUint`](../../Reference/MfuncParamArrayAilUint.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_UINT_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_ARRAY_AIL_UINT32`

Specifies to call [`MfuncParamArrayAilUint32`](../../Reference/MfuncParamArrayAilUint32.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_UINT32_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_ARRAY_AIL_UINT64`

Specifies to call [`MfuncParamArrayAilUint64`](../../Reference/MfuncParamArrayAilUint64.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_UINT64_.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` |  |

### For registering a const array of primitive data-types parameter in an Aurora Imaging Library user-defined function

The following values specify to call type-specific variations of [`MfuncParam`](../../Reference/func/MfuncParam.md) that register parameters that handle const arrays of primitive Aurora Imaging Library data types, such as, _AIL_DOUBLE_, _AIL_INT_, and _AIL_UINT64_, as well as const pointers and const strings.  Unless otherwise specified, when specifying values from this table, you must set the [`NumOfItems`](../../Reference/func/MfuncParam.md) parameter to the number of items in the array, and you must set the [`AilObjectType`](../../Reference/func/MfuncParam.md) parameter to [`M_NULL`](../../Reference/func/MfuncParam.md).

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_DOUBLE`

Specifies to call [`MfuncParamConstArrayAilDouble`](../../Reference/MfuncParamConstArrayAilDouble.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_DOUBLE_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_INT`

Specifies to call [`MfuncParamConstArrayAilInt`](../../Reference/MfuncParamConstArrayAilInt.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_INT_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_INT32`

Specifies to call [`MfuncParamConstArrayAilInt32`](../../Reference/MfuncParamConstArrayAilInt32.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_INT32_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_INT64`

Specifies to call [`MfuncParamConstArrayAilInt64`](../../Reference/MfuncParamConstArrayAilInt64.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_INT64_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT`

Specifies to call [`MfuncParamConstArrayAilUint`](../../Reference/MfuncParamConstArrayAilUint.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_UINT_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT32`

Specifies to call [`MfuncParamConstArrayAilUint32`](../../Reference/MfuncParamConstArrayAilUint32.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_UINT32_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_UINT64`

Specifies to call [`MfuncParamConstArrayAilUint64`](../../Reference/MfuncParamConstArrayAilUint64.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_UINT64_.

| Value | Description |
| --- | --- |
| `M_IN` |  |

### For registering a string or file name type parameter in an Aurora Imaging Library user-defined function

The following values specify to call type-specific variations of [`MfuncParam`](../../Reference/func/MfuncParam.md) that register a string or file name parameter.  Unless otherwise specified, when specifying values from this table, you must set the [`AilObjectType`](../../Reference/func/MfuncParam.md) parameter to [`M_NULL`](../../Reference/func/MfuncParam.md).

---

### `M_PARAM_TYPE_AIL_TEXT`

Specifies to call [`MfuncParamAilText`](../../Reference/MfuncParamAilText.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a string.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies for Aurora Imaging Library to auto-calculate the number of characters in the string. |
| `Value` | Specifies the number of characters in the string. |
| `M_IN` |  |
| `M_OUT` |  |

---

### `M_PARAM_TYPE_CONST_AIL_TEXT`

Specifies to call [`MfuncParamConstAilText`](../../Reference/MfuncParamConstAilText.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const string.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies for Aurora Imaging Library to auto-calculate the number of characters in the string. |
| `Value` | Specifies the number of characters in the string. |
| `M_IN` |  |

---

### `M_PARAM_TYPE_FILENAME`

Specifies to call [`MfuncParamFilename`](../../Reference/MfuncParamFilename.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a file name.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies for Aurora Imaging Library to auto-calculate the number of characters in the string. |
| `Value` | Specifies the number of characters in the string. |
| `M_IN` | Specifies that the parameter of the current user-defined Aurora Imaging Library function will be registered for file reading only. |
| `M_OUT` | Specifies that the parameter of the current user-defined Aurora Imaging Library function will be registered for file writing only. |

### For registering a pointer type parameter in an Aurora Imaging Library user-defined function

The following values specify to call type-specific variations of [`MfuncParam`](../../Reference/func/MfuncParam.md) that register pointer parameters, including arrays and const arrays of pointers.  Unless otherwise specified, when specifying values from this table, you must set the [`AilObjectType`](../../Reference/func/MfuncParam.md) parameter to [`M_NULL`](../../Reference/func/MfuncParam.md).

---

### `M_PARAM_TYPE_CONST_DATA_PTR`

Specifies to call [`MfuncParamConstDataPointer`](../../Reference/MfuncParamConstDataPointer.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const data pointer.

| Value | Description |
| --- | --- |
| `M_IN` |  |

---

### `M_PARAM_TYPE_DATA_PTR`

Specifies to call [`MfuncParamDataPointer`](../../Reference/MfuncParamDataPointer.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a data pointer.

| Value | Description |
| --- | --- |
| `M_IN` |  |
| `M_OUT` | Specifies that the object passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) is used to store new data from the user-defined function. |

### For registering an Aurora Imaging Library identifier type parameter in an Aurora Imaging Library user-defined function

The following values specify to call type-specific variations of [`MfuncParam`](../../Reference/func/MfuncParam.md) that register parameters that handle an _AIL_ID_, including arrays and const arrays of _AIL_ID_s.  When registering _AIL_ID_ parameters, you must specify a value for [`AilObjectType`](../../Reference/func/MfuncParam.md).

---

### `M_PARAM_TYPE_AIL_ID`

Specifies to call [`MfuncParamAilId`](../../Reference/MfuncParamAilId.md), which then registers a type _AIL_ID_ parameter of the current user-defined Aurora Imaging Library function.

| Value | Description |
| --- | --- |
| `M_IN` | Specifies that the object referenced by the _AIL_ID_ passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) is used for data input. |
| `M_OUT` | Specifies that the object referenced by the _AIL_ID_ passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) is used to store new data from the user-defined function. |

---

### `M_PARAM_TYPE_ARRAY_AIL_ID`

Specifies to call [`MfuncParamArrayAilId`](../../Reference/MfuncParamArrayAilId.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles an array of type _AIL_ID_.

| Value | Description |
| --- | --- |
| `M_IN` | Specifies that the objects referenced in the array passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) are used for data input. |
| `M_OUT` | Specifies that the objects referenced by the _AIL_ID_ passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) are used to store new data from the user-defined function. |

---

### `M_PARAM_TYPE_CONST_ARRAY_AIL_ID`

Specifies to call [`MfuncParamConstArrayAilId`](../../Reference/MfuncParamConstArrayAilId.md), which then registers a parameter of the current user-defined Aurora Imaging Library function that handles a const array of type _AIL_ID_.

| Value | Description |
| --- | --- |
| `M_IN` | Specifies that the objects referenced in the array passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) are used for data input. |

### Combination Constants — For specifying optional settings for an Aurora Imaging Library identifier type parameter

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify the optional settings for an Aurora Imaging Library identifier type parameter.

When you register an Aurora Imaging Library identifier type of parameter, the user-defined function will throw an error if a value other than an _AIL_ID_ is passed to it. There are, however, non-standard identifier values that can be accepted. The values specified below specify which non-standard identifier values are valid in addition to an _AIL_ID_.  There is also the optional of specifying that an Aurora Imaging Library identifier will not be internally modified or compensated during the execution of the user-defined function.

| Value | Description |
| --- | --- |
| `M_ALLOW_DEFAULT_ID` | Specifies that this parameter will accept **M_DEFAULT** as a valid _AIL_ID_. |
| `M_ALLOW_NULL_ID` | Specifies that this parameter will accept **M_NULL** as a valid _AIL_ID_. |
| `M_AS_VALUE` | Specifies that this parameter will accept any valid _AIL_ID_, but will not internally modify it. This means different things depending on if this value is added to [`M_IN`](../../Reference/func/MfuncParam.md) or [`M_OUT`](../../Reference/func/MfuncParam.md).  For an Aurora Imaging Library identifier type parameter set to [`M_IN`](../../Reference/func/MfuncParam.md), the buffer specified by the user is used as the source of information for the user-defined function. When that source buffer is on a different system, the Aurora Imaging Library identifier for that buffer is internally modified if the content of the buffer needs to be copied to another system for processing. This modification is called compensation.  If your application uses the Aurora Imaging Library identifiers exactly as specified by the user, your application could crash by inadvertently pointing to a buffer whose content has been copied to another buffer with a compensated Aurora Imaging Library identifier. You can avoid this by adding [`M_AS_VALUE`](../../Reference/func/MfuncParam.md) to [`M_IN`](../../Reference/func/MfuncParam.md) when registering your parameter. This has the effect of keeping the specific value of the Aurora Imaging Library identifier specified by the user, without compensating it.  For an Aurora Imaging Library identifier type parameter set to [`M_OUT`](../../Reference/func/MfuncParam.md), the buffer specified by the user is used as the destination of information for the user-defined function. When you register this parameter without [`M_AS_VALUE`](../../Reference/func/MfuncParam.md), the user must specify the _AIL_ID_ of a previously allocated buffer. When the user-defined function is called, the content of that destination buffer will be filled with new data.  When you register this parameter as [`M_OUT`](../../Reference/func/MfuncParam.md) + [`M_AS_VALUE`](../../Reference/func/MfuncParam.md), the user must specify a pointer. When the user-defined function is called, the pointer will be filled with an _AIL_ID_. The _AIL_ID_ could be, for example, a newly-allocated buffer or the result of an inquire. |

### Combination Constants — For specifying the type of image buffer when the

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify the image buffer type, if [`AilObjectType`](../../Reference/func/MfuncParam.md) is set to [`M_IMAGE`](../../Reference/func/MfuncParam.md).

If you specify an [`M_IMAGE`](../../Reference/func/MfuncParam.md) object (from the [`ParamValuePtr`](../../Reference/func/MfuncParam.md) parameter) that does not correspond to any of the type(s) selected from the values listed below, an error is returned. If none of the values specified below are added, any [`M_IMAGE`](../../Reference/func/MfuncParam.md) object will be accepted. Note that you cannot specify any of the following if the [`AilObjectType`](../../Reference/func/MfuncParam.md) parameter is set to a combination of values.

| Value | Description |
| --- | --- |
| `M_DISP` | Specifies that the buffer can be displayed. |
| `M_GRAB` | Specifies that the buffer can have data grabbed into it. |
| `M_PROC` | Specifies that the buffer can be processed. |

This parameter should be set to one or both ([`M_IN`](../../Reference/func/MfuncParam.md) + [`M_OUT`](../../Reference/func/MfuncParam.md)) of the following values.

Specifies that the object passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) is used for data input.

Specifies that the array passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) is used for data input.

Specifies that the array passed to [`ParamValuePtr`](../../Reference/func/MfuncParam.md) is used to store new data from the user-defined function.
