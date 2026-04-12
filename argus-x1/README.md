# ARGUS X-1

This repository contains only the state machine and header files of ARGUS X-1. The full implementation will not be shared until next year due to potential academic publication.

## Baseline Performance Test

Test Conditions:

- Resolution: 640x400
- Rate: Limited to 60 FPS
- Exposure: Fixed (150)
- Target: High-contrast red LED under green ambient light
- Processing: No Kalman Filter or Estimation (Raw Data)

### Metrics

| Metric                         | Value                          |
|--------------------------------|--------------------------------|
| Closed-loop update rate        | 60.0 Hz                        |
| Jitter (1σ)                    | 1.93 ms                        |
| Tracking uptime                | 96.5% over 90.85 s             |
| RMS error (X/Y)                | 12.76 px / 11.32 px            |
| Mean bias (X/Y)                | -0.011 px / +0.390 px          |
| Occlusion recoveries           | 0.314 s, 1.482 s, 1.253 s      |
| 95th percentile absolute error | 27.46 px / 22.63 px            |
| Median absolute error          | 4.93 px / 5.28 px              |
| Time to first acquisition      | 0.313 s                        |
| Settle time                    | 0.117 s                        |

At the time of this test, the system still isn't perfect. The Y-axis servo has a manufacturing defect, some critical components are held together with double-sided tape, and the camera's unnecessarily long platform introduces extra inertia. On the software side, there are flaws in the control logic causing derivative and integral corruption near the deadzone boundary, which produce an unwanted deadband near the setpoint and interfere with the overall control behavior. Additionally, since I placed the camera upside down I had to use `cv::flip(data.gray, data.gray, -1)` to compensate but I am not certain whether this interferes with anything in the control loop yet.

Even under these constraints, X-1 successfully handled three deliberate reacquisitions without interruption, operated continuously, and exceeded my expectations despite its current limitations.
