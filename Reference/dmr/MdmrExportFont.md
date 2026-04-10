---
doctype: Reference
module: dmr
function: MdmrExportFont
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrExportFont"
---

# MdmrExportFont

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

> Export a font from a SureDotOCR context.

## Syntax

```c
void MdmrExportFont(
    AIL_CONST_TEXT_PTR FileName,          //in
    AIL_INT64          FileFormat,        //in
    AIL_ID             ContextDmrId,      //in
    AIL_INT64          FontLabelOrIndex,  //in
    AIL_INT64          ControlFlag        //in
)
```

## Description

This function exports a font from a SureDotOCR context to a file. The exported font includes all its characters. String models use fonts to read dot-matrix text. Once you export a font, you can import it to any SureDotOCR context using [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the SureDotOCR font file in which to export the font. The function handles (internally) the opening and closing of the file. For easier use with other Aurora Imaging Library software products, the specified file name must use the MDMRF file extension, which refers to a SureDotOCR font file. If you specify a file that already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which to interactively specify the drive, directory, and name of a SureDotOCR font file. |
| `"FileName"` | Specifies the drive, directory, and name of a SureDotOCR font file (for example, _"C:\mydirectory\MySweetSweetFont.mdmrf"_).

To specify a SureDotOCR font file on a remote computer (under Distributed Aurora Imaging Library), prefix the file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MySweetSweetFont.mdmrf"`). |

### `FileFormat` *(in, AIL_INT64)*

Specifies the format of the file in which to export the font. Set this parameter to the value below.

*For specifying the file format*

| Value | Description |
| --- | --- |
| `M_DMR_FONT_FILE` | Specifies a SureDotOCR font file format (MDMRF). See [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) for information on how SureDotOCR font files represent their data. |

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context from which to export the font. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `FontLabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of the font to export from the SureDotOCR context. Set this parameter to one of the values below.

*For identifying the font*

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies to export the font by indicating its index. |
| `M_FONT_LABEL` | Specifies to export the font by indicating its label. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
