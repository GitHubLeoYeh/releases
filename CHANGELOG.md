# Changelog

## 2022-07-25

### Board Client `0.17.0`

- We added device listing support for Windows, Linux, and MacOS. Now the available cameras will be correctly populated in the Board Manager for all OSes.
- We removed the synchronization mechanism that aligned FPS in the main loop with the lowest FPS coming from the cams. Overall this should give more stable performance.
- We fixed a bug where cameras were not correctly freed when one of the cams had failed, resulting in segmentation fault crashes.
- We added two flags to the `autodarts` binary:
  - If you run `autodarts --version`, it will return the Board Client version.
  - If you run `autodarts --opencv-info`, it will return build information on the included OpenCV build.
- We optimized the included OpenCV version to use a pre-compiled version of the `libjpeg-turbo` library with optimizers for different kernel architectures.
- We reduced the footprint of the included OpenCV version to only bundle the necessary libraries, which overall reduced the size of the binaries.
- We added support for Windows, Linux, and MacOS to retrieve host information from all OSes.
- **Most importantly, we added support for Windows and provide an executable for Windows for the first time. Windows, however, is quite picky when it comes to connecting multiple cameras to the same USB port. It is adviced to connect every camera individually to different USB ports. I have tested on multiple machines, and this generally seems to work. It is not guaranteed to work, though. Windows support is still young, and things might break. So, be patient.**

## 2022-01-24

### Board Client `0.16.1`

- We fixed a bug with the undistortion where different resolutions to the `distortion.json` resolution caused problems.

## 2022-01-17

### Board Manager `0.16.0`

- We updated the Config page. It now lists all of the available devices in a dropdown and only shows resolutions that are common across all devices.
- The Config page now also shows the Board State Bar and Controls at the top.
- The Config page now allows to revert the Motion Detection and Dart Detection settings to their defaults. The default settings are based on the chosen resolution.

### Board Client `0.16.0`

- Support for the new Board Manager `0.16.0`.
- Dart Detection: We updated the fallback algorithm when there is only one camera that can see the dart. It should be more accurate now, albeit still bad. If only one camera sees the dart, detections may vary.
- Dart Detection: We added a check for the intersection angle between every pair of two lines. If the intersection angle is less than 5 degrees, the intersection is ignored. This is to counteract problems with inaccuracies where two lines are almost parallel. This only applies in cases where there are three lines.
- Cam: We simplified the startup of the cams so that cam startup does not block the initialization of other modules.
- V4L2: The Board Client now uses the `v4l2-ctl` command internally to list the available devices and sets reasonable defauls when generating the default config.
- The Board Client now sends some user statistics to the Board Server such as Client Version, OS Version information, Config, Calibration, and Distortion parameters.
- Config, Calibration, and Distortion settings are now loaded directly at startup and are not depending on the Cam initialization any longer.
- Config: We updated the default settings to more reasonable defaults. Settings now scale with the `height` as apposed to the product of `height` and `width` (total number of pixels).
