---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Code_context_initialization_modes
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Code context initialization modes"
---

# Code context initialization modes

A code context is an Aurora Imaging Library object that stores code models and operation settings. Some of these settings can be initialized to predetermined values that change based on the selected code context initialization mode. When allocating your code context (using [`McodeAlloc`](../../Reference/code/McodeAlloc.md)), you can chose between 2 types of initialization modes:

- [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md). This configures the context for a faster but less robust [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation by making assumptions about the code to read and/or grade (such as, that the foreground color is black). This is the default value.
- [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md). This configures the context for a more robust [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation by not making many assumptions about the code to read and grade (such as, that the foreground color could be black or white). This initialization mode is recommended if your target images lack similarity (for example, some but not all include complex or noisy backgrounds), contain distorted codes, or contain codes that are at an angle or even flipped. It is also recommended when searching for 2D matrix codes that are composed of dots. Note that this initialization mode is not recommended with [`M_PHARMACODE`](../../Reference/code/McodeModel.md).
  > **Note:** Note that when selecting this mode, the default of the affected code context and code model settings might change between Aurora Imaging Library releases to improve robustness, given new features or standard recommendations.

The initialization mode can be changed after allocation, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md). When you change the initialization mode, control types that have already been set to some value (other than default) are not changed. Instead, the initialization mode changes those control types that are set to their default value ([`M_DEFAULT`](../../Reference/code/McodeControl.md)).
