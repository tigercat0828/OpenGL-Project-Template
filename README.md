## Introduction
A project template that integrates useful libraries for developing OpenGL applications.

I understand dealing with these libraries and linking them can be painful, so you don't have to.

---
- Window & Input : SDL2
- OpenGL Loader : GLAD
- Math : GLM
- GUI : imgui
- Log : spdlog


## How to struct the code
- Edit the content of `CMakeLists.txt`
```cmake
project(YourProjectName)        # name your project name
set(CMAKE_CXX_STANDARD 17)      # set the C++ standard
set(PROGRAM_NAME main)          # set the executable name
```
- Place your source files into `src` and header files into `include`
- Place your shaders, textures, and any other resource files into the `Assets` directory.

```bat
├─Assets
├─include
├─src
│  ├─glad
│  └─imgui
└─ThirdParty
    ├─include
    └─lib
```

## How to build the project

1. Clone the project or download it as a ZIP file.
2. Navigate to the project directory.
3. Configure and build the project in Release/Debug mode:
```bat
mkdir build
cd build
cmake ..
cmake --build . --config Release  
```


