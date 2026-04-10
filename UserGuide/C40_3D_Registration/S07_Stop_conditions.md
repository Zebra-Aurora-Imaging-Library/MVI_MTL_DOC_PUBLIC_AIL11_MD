---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Registration
section: Stop_conditions
module_tag: 3dreg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Registration / Stop conditions"
---

# Stop conditions

There are three stop conditions that can be specified using [`M3dregControl`](../../Reference/3dreg/M3dregControl.md), and a fourth which is internally set. When one of the stop conditions is met, the registration operation stops. The stop conditions are:

- Root-mean-square (RMS) error threshold, set with [`M_RMS_ERROR_THRESHOLD`](../../Reference/3dreg/M3dregControl.md). The registration operation stops when the last iteration's RMS error is lower than the specified threshold. By default, this value is set to 0.0.
- Relative RMS error threshold, set with [`M_RMS_ERROR_RELATIVE_THRESHOLD`](../../Reference/3dreg/M3dregControl.md). The registration operation stops when the change in RMS error between the last two iterations, expressed as a percentage, is lower than the specified threshold. The change in RMS error is typically larger in the first iterations, and smaller in the final iterations. More specifically, where _E<sub>i</sub>_ is the RMS error of iteration _i_, the relative RMS error is calculated as:
  [(_E<sub>i-1</sub>_ - _E<sub>i</sub>_)/_E<sub>i-1</sub>_]*100.
  By default, this value is set to 0.1 (that is, 0.1%).
- Maximum number of iterations, set with [`M_MAX_ITERATIONS`](../../Reference/3dreg/M3dregControl.md). The registration operation stops when the maximum number of iterations is reached. By default, this value is set to 20.
- Minimum number of paired points, set internally. The registration operation stops when there are too few paired points between the two point clouds. Typically, this occurs before the registration operation starts because the point clouds are too far away. This can also occur when the point clouds diverge rather than converge over several iterations.

For example, you could keep the default values for [`M_RMS_ERROR_THRESHOLD`](../../Reference/3dreg/M3dregControl.md) and [`M_MAX_ITERATIONS`](../../Reference/3dreg/M3dregControl.md), and set [`M_RMS_ERROR_RELATIVE_THRESHOLD`](../../Reference/3dreg/M3dregControl.md) to 0.15 (that is, 0.15%). In this scenario, during the registration process, the RMS error could be 1.0 after the first iteration, 0.998 after the second, and 0.997 after the third. This means that the relative RMS error from the first to the second iteration is 0.2% ([(1.0-0.998)/1.0]*100) and from the second to the third iteration is about 0.1% ([(0.998-0.997)/0.998]*100). After the third iteration, the registration operation stops because the third iteration's relative RMS error is lower than the specified threshold of 0.15%. This means that the iterative refinements of the transformation were sufficiently small. However, if the relative RMS error never decreased below the specified threshold of 0.15%, the registration operation would stop after the default maximum number of iterations, which is 20. If the point clouds started to diverge, and there were insufficient point pairs to calculate an RMS error, the registration operation would also stop.

Each of these four stop conditions has an associated result which can be retrieved using [`M3dregGetResult`](../../Reference/3dreg/M3dregGetResult.md) with [`M_STATUS_REGISTRATION_ELEMENT`](../../Reference/3dreg/M3dregGetResult.md). Typically, the pairwise 3D registration operation is considered a success if its status is [`M_RMS_ERROR_RELATIVE_THRESHOLD_REACHED`](../../Reference/3dreg/M3dregGetResult.md) or [`M_RMS_ERROR_THRESHOLD_REACHED`](../../Reference/3dreg/M3dregGetResult.md). Depending on your application, [`M_MAX_ITERATIONS_REACHED`](../../Reference/3dreg/M3dregGetResult.md) can be considered a success or a timeout.
