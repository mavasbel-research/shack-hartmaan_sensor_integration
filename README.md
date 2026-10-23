<div align="center">

# Shack–Hartmann Sensor Integration

**MATLAB scripts (2022) that drive a USB Shack–Hartmann wavefront sensor through its vendor WFS driver: live spot-field images, spot centroids, deviations and the wavefront.**

[![MATLAB](https://img.shields.io/badge/MATLAB-0076A8?style=flat-square)](https://www.mathworks.com/products/matlab.html)
[![Platform](https://img.shields.io/badge/platform-Windows%2064--bit-0078D6?style=flat-square)](#-requirements)
[![Years](https://img.shields.io/badge/year-2022-555555?style=flat-square)](https://github.com/mavasbel-research/shack-hartmaan_sensor_integration/commits/main)
[![Status](https://img.shields.io/badge/status-research%20archive-lightgrey?style=flat-square)](#-known-issues)

</div>

> [!NOTE]
> Lab integration scripts written in February–March 2022. They need the sensor hardware and its vendor driver, and are kept as a reference.

## 🔭 What `basic_reading.m` does

```mermaid
flowchart TD
    A["loadlibrary WFS_64.dll<br/>(vendor driver)"] --> B["List instruments<br/>ID, name, serial number"]
    B --> C["WFS_init on USB::0x1313::0x0000::&lt;id&gt;"]
    C --> D["Select microlens array 0<br/>camera 1024 × 1024<br/>pupil 5.0 × 5.0"]
    D --> E["Spot-field image<br/>with auto exposure"]
    E --> F["Spot centroids, diameters<br/>and intensities"]
    F --> G["Beam centroid · deviations<br/>to the reference · wavefront"]
    G -- "SPACE in the figure stops" --> E
    G --> H["WFS_close · unloadlibrary"]
```

The live figure shows the spot-field image. The centroids, deviations and wavefront are computed each iteration but are not plotted or saved.

## 🗂️ Files

| File | Purpose |
|:---|:---|
| `basic_reading.m` | Sensor read-out loop shown above |
| `daq_simple_test.m` | Voltage test of a multiplexer on `COM3`: enables channels, ramps to 100 and back to 0, and notes the expected meter readings (+200 V, +100 V, −100 V) |

## 🧰 Requirements

| Requirement | Detail |
|:---|:---|
| Windows 64-bit | the driver paths are hard-coded under `C:\Program Files\IVI Foundation\VISA\Win64\` |
| MATLAB with `loadlibrary` and a supported C compiler | to load `WFS_64.dll` with `WFS.h`, `visa.h` and `vpptype.h` |
| Wavefront sensor driver | `WFS_64.dll` and `WFS.h`; the USB vendor ID `0x1313` belongs to Thorlabs |
| Multiplexer helpers `initMux`, `enMux`, `setMux`, `vRampDaq` | only for `daq_simple_test.m`; they are not part of this repository |

## ▶️ Running

1. Install the sensor driver so the files above exist at those paths, and connect the sensor.
2. In MATLAB, from the repository folder:
   ```matlab
   basic_reading
   ```
3. Press <kbd>Space</kbd> with the figure focused to stop and release the device.

## ⚠️ Known issues

| Issue | Where |
|:---|:---|
| The device is chosen automatically (the last one listed); the interactive choice is commented out | `basic_reading.m:48-50` |
| Re-running closes a previous session only if `hdl` is still in the workspace | `basic_reading.m:9-12` |
| `daq_simple_test.m` cannot run without the four external helper functions | `daq_simple_test.m` |
| The repository name misspells Hartmann as "hartmaan" | repository name |

This repository does not include a license file.

---

<div align="center">
<sub>Part of <a href="https://github.com/mavasbel-research">mavasbel-research</a> · <a href="https://github.com/mavasbel">Marco Vásquez Beltrán</a></sub>
</div>
