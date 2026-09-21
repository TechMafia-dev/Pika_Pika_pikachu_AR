# Pika Pika Pikachu AR

An Android augmented-reality and computer-vision sandbox built while exploring **ARCore / Sceneform, OpenCV, camera processing, and native C++ integration**.

The repository evolved as a collection of small experiments rather than a single production application. The most recognizable experiment is the **EasyLearnAR** module, which sets up an ARCore camera experience and includes 3D sample assets such as a Pikachu model.

## What this project explores

- Android development with **Java**
- **ARCore**-based augmented reality
- **Google Sceneform 1.15.0** for AR scene setup
- **OpenCV** camera processing
- Feature / corner detection from live camera frames
- Passing OpenCV `Mat` objects between Java and **native C++ through JNI**
- Working with **Wavefront OBJ / MTL** 3D assets
- Multi-module Android project structure

## Project structure

```text
Pika_Pika_pikachu_AR/
├── app/                  # Java + OpenCV camera-processing experiment
├── appcpp/               # OpenCV + native C++ / JNI experiment
├── easylearnar/          # ARCore + Sceneform experiment
├── OpenCV/               # OpenCV Android library module
└── settings.gradle
```

### `easylearnar` — EasyLearnAR

The AR module uses:

- ARCore
- Sceneform UX 1.15.0
- `ArFragment`
- Android camera permission and AR hardware requirement
- Java 8 compatibility

The module includes sample 3D assets:

- `pika.obj` + `pika.mtl` + `pika.png`
- `chair.obj` + `chair.mtl`

The current checked-in implementation establishes the AR scene using Sceneform's `ArFragment`. The repository also contains the 3D assets for experimentation; model placement / interaction logic is not implemented in the current `MainActivity_EasyLearn_AR` source.

### `app` — OpenCV camera experiment

This module uses the device camera through OpenCV and performs real-time feature detection.

The Java camera pipeline:

1. Receives an RGBA and grayscale camera frame.
2. Runs `goodFeaturesToTrack()` on the grayscale image.
3. Finds up to 20 candidate corners.
4. Draws the detected points back onto the live RGBA frame.
5. Displays the processed camera feed.

This is a compact example of building a real-time computer-vision pipeline directly inside an Android application.

### `appcpp` — Native OpenCV / JNI experiment

The C++ module extends the same camera-processing idea into native code.

The Java layer passes the native addresses of the grayscale and RGBA OpenCV matrices to:

```text
FindFeatures(long addrGray, long addrRGB)
```

The JNI implementation reconstructs the OpenCV matrices in C++, runs `goodFeaturesToTrack()`, and draws the detected corners using OpenCV.

This creates a simple pipeline:

```text
Android Camera
      │
      ▼
OpenCV Mat
      │
      ▼
Java / JNI boundary
      │
      ▼
Native C++ + OpenCV
      │
      ▼
Processed camera frame
```

## Technology stack

| Area | Technology |
|---|---|
| Platform | Android |
| Primary language | Java |
| Native code | C++ |
| Build system | Gradle |
| Android Gradle Plugin | 7.2.1 |
| Compile SDK | 32 |
| Minimum SDK | 23 |
| AR | ARCore |
| AR framework | Google Sceneform 1.15.0 |
| Computer vision | OpenCV |
| Native bridge | JNI |
| 3D assets | OBJ / MTL |

## Why this repository exists

This project captures an earlier stage of hands-on exploration into the intersection of:

**mobile development → computer vision → 3D assets → augmented reality → native performance**

Rather than relying only on high-level SDKs, the repository experiments with the underlying camera-processing path as well as the Java-to-native boundary.

## Running the project

The repository is an older Android/Sceneform-era project, so current Android Studio / SDK environments may require compatibility adjustments.

### Prerequisites

- Android Studio
- Android SDK with API 32 available
- Java 8-compatible build environment
- For `easylearnar`: an **ARCore-supported Android device**
- For the OpenCV modules: an Android device with camera access

### Build

Clone the repository and open it in Android Studio.

The project contains multiple application modules:

- `app`
- `appcpp`
- `easylearnar`

Select the module you want to run and build the corresponding application.

## Notes

- `OpenCV/` is included as an Android library module rather than being treated purely as an external runtime dependency.
- `appcpp` requires the Android NDK / CMake toolchain because it builds a native shared library.
- `easylearnar` declares ARCore as a required device capability.
- The repository contains generated/build artifacts from the original development environment; these are not required to understand the core source architecture.

## Project status

**Archived / experimental project.**

The repository is preserved primarily as a record of experimentation with Android AR, OpenCV, JNI, and native computer vision rather than as a currently maintained production application.

## Takeaways

This project demonstrates experience with:

- Android application structure and Gradle modules
- Real-time camera pipelines
- Classical computer-vision algorithms
- OpenCV on Android
- JNI and native C++ integration
- ARCore / Sceneform setup
- 3D asset handling
- Bridging high-level application code with lower-level vision processing

---

**Repository:** `TechMafia-dev/Pika_Pika_pikachu_AR`
