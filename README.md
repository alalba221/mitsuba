Mitsuba Fork for CMake
======================

This fork is modified for CMake/Scons compilation, dependencies are updated to lastest(?) version.(Only Windows platform supported)

## Modifications

- Download NBRDF code from [https://github.com/asztr/Neural-BRDF](https://github.com/asztr/Neural-BRDF),
  - copy all files from their `mitsuba/bsdfs` into ours `src/bsdfs`.
  - create a sub folder `nbrdfscene` under the `data` floder and copy their's sample files: `mitsuba/envmap.exr` and `mitsuba/sample_scene.xml`  into it.
- Download the  `merl_nbrdf.zip` file from [https://github.com/asztr/asztr.github.io/tree/main/publications/nbrdf2021/supplemental/data](https://github.com/asztr/asztr.github.io/tree/main/publications/nbrdf2021/supplemental/data), unzip it then copy it's `merl_nbrdf` to `data` floder
- Update the CMakefile file by adding

```
  add_library(nbrdf_npy SHARED nbrdf_npy.cpp)
  target_link_libraries(nbrdf_npy mitsuba-python)
  set_target_properties(nbrdf_npy PROPERTIES FOLDER "plugins")
```

  at the end of  `src\bsdfs\CMakeLists.txt`

- fix compiling errors in their c++ code
## Usage

### Environment

Following environment/dependencies are required:

- Visual Studio
- [CMake](https://cmake.org/download/)
- [Qt 5.15](https://www.qt.io/download-qt-installer)
- [Python 3.9](https://www.python.org/)
- 7zip

### Dependencies Compilation

Before you start, please notice that:

- **All commands below are required to execute under VS Command Prompt(x64).**
- **Please make sure you can download files in cmake.** If you can't, or you don't want to, please refer to the "Dependencies List" section, and download them manually.

1. Modify `dependencies/CMakeLists.txt`
   - Check Line 9. Choose a number <= your CPU cores.
   - Check Line 12 and Line 13 for proxies. If you don't know what it is, or you don't need it, just comment these 2 lines.
2. Dependencies compilation: Go to `dependencies/`, and execute `cmake ./`. (**Too many files, it will take a long time to compile.**)

### Mitsuba Compilation

You can choose compile with CMake or SCons. (CMake is recommended)

#### CMake Compilation

You can compile with CMake GUI or command line.

1. Specify `QT_DIR` and `Python_ROOT_DIR`.
2. Create folder `cbuild`. (Compiling in folder `build` is not recommended, because some scripts are in that folder and you may not want to mess them up.)
3. Config and generate project in GUI. (or go to `cbuild` folder and execute `cmake ..` in command line)
4. Open Visual Studio Solution `Mitsuba.sln`, compile entire solution.
5. You can run/debug mitsuba as normal VS project now.


## Dependencies list

- If you can't download dependencies in cmake, you can download them manually to `dependencies/Downloads`. After downloading, you can start from Step 1 in "Compilation" section.
- **All downloaded content are stored in `dependencies/Downloads`**
- In this table, if "Filename" ends with '/', means this is a folder; otherwise this is a file.

| Library       | Filename                 | Link                                                                                                     |
| ------------- | ------------------------ | -------------------------------------------------------------------------------------------------------- |
| zlib          | `zlib/`                | `git clone --recurse-submodules https://github.com/madler/zlib.git`                                    |
| OpenEXR       | `openexr/`             | `git clone --recurse-submodules https://github.com/AcademySoftwareFoundation/openexr.git`              |
| libjpeg-turbo | `libjpeg/`             | `git clone --recurse-submodules https://github.com/libjpeg-turbo/libjpeg-turbo.git`                    |
| libpng        | `libpng/`              | `git clone --recurse-submodules https://github.com/glennrp/libpng.git`                                 |
| boost         | `boost/`               | `git clone --recurse-submodules --depth=1 --branch=boost-1.77.0 https://github.com/boostorg/boost.git` |
| xerces-c      | `xerces-c/`            | `git clone --recurse-submodules https://github.com/apache/xerces-c.git`                                |
| glew          | `glew-2.2.0-win32.zip` | https://github.com/nigels-com/glew/releases/download/glew-2.2.0/glew-2.2.0-win32.zip                     |
| half          | `half-2.2.0.zip`       | https://iweb.dl.sourceforge.net/project/half/half/2.2.0/half-2.2.0.zip                                   |
| glext         | `glext.h`              | https://www.khronos.org/registry/OpenGL/api/GL/glext.h                                                   |
| khr           | `khrplatform.h`        | https://www.khronos.org/registry/EGL/api/KHR/khrplatform.h                                               |
| FFTW          | `fftw-3.3.5-dll64.zip` | https://fftw.org/pub/fftw/fftw-3.3.5-dll64.zip                                                           |

## Konwn Issues

- Fail to export hdr images under debug mode. (Works fine in release mode)
